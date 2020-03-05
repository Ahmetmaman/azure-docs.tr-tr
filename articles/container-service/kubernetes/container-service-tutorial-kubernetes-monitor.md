---
title: Kullanım DıŞı Azure Container Service öğreticisi-Kubernetes 'i Izleme
description: Azure Container Service öğreticisi - Log Analytics ile Kubernetes’i İzleme
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 04/05/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 84c2438a8c25b1b64f46e12923212812beac687d
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273331"
---
# <a name="deprecated-monitor-a-kubernetes-cluster-with-log-analytics"></a>Kullanım DıŞı Log Analytics ile bir Kubernetes kümesini izleme

> [!TIP]
> Azure Kubernetes hizmetini kullanan Bu öğreticinin güncelleştirilmiş sürümü için bkz. [kapsayıcılar Için Azure izleyici (Önizleme) genel bakış](../../azure-monitor/insights/container-insights-overview.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Kubernetes kümenizin ve kapsayıcılarınızın izlenmesi, özellikle de büyük ölçekli olarak birden fazla uygulamayı ve bir üretim kümesini yönetiyorsanız kritik önem taşır.

Microsoft ya da diğer sağlayıcılar tarafından sunulan çeşitli Kubernetes izleme çözümlerinden yararlanabilirsiniz. Bu öğreticide, Microsoft’un bulut tabanlı BT yönetim çözümü olan [Log Analytics](../../operations-management-suite/operations-management-suite-overview.md)’teki Kapsayıcılar çözümünü kullanarak Kubernetes kümenizi izlersiniz. (Kapsayıcılar çözümü önizlemededir.)

Yedi bölümün sonuncusu olan bu öğreticide aşağıdaki görevler ele alınır:

> [!div class="checklist"]
> * Log Analytics Çalışma Alanı ayarlarını alma
> * Kubernetes düğümlerinde Log Analytics aracılarını ayarlama
> * Log Analytics portalında veya Azure portalında izleme bilgilerine erişme

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama kapsayıcı görüntülerine paketlendi, bu görüntüler Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma](./container-service-tutorial-kubernetes-prepare-app.md) konusuna dönün.

## <a name="get-workspace-settings"></a>Çalışma Alanı ayarlarını alma

[Log Analytics portalına](https://mms.microsoft.com) erişebildiğinizde **Ayarlar** > **Bağlı Kaynaklar** > **Linux Sunucuları** seçeneğine gidin. Burada, *Workspace ID* (Çalışma Alanı Kimliği) ile birincil veya ikincil *Workspace Key* (Çalışma Alanı Anahtarı) değerini bulabilirsiniz. Kümede Log Analytics aracılarını ayarlamak için gerekli olacak bu değerleri not alın.

## <a name="create-kubernetes-secret"></a>Kubernetes gizli dizisi oluşturma

[kubectl create secret][kubectl-create-secret] komutunu kullanarak Log Analytics çalışma alanı ayarlarını `omsagent-secret` adlı Kubernetes gizli dizisinde depolayın. `WORKSPACE_ID` değerini Log Analytics çalışma alanı kimliğiyle, `WORKSPACE_KEY` değerini ise çalışma alanı anahtarı ile güncelleştirin.

```console
kubectl create secret generic omsagent-secret --from-literal=WSID=WORKSPACE_ID --from-literal=KEY=WORKSPACE_KEY
```

## <a name="set-up-log-analytics-agents"></a>Log Analytics aracılarını ayarlama

Aşağıdaki Kubernetes bildirim dosyası, bir Kubernetes kümesindeki kapsayıcı izleme aracılarını yapılandırmak için kullanılabilir. Bu dosya, her küme düğümünde aynı pod’u çalıştıran bir Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) oluşturur.

Aşağıdaki metni `oms-daemonset.yaml` adlı bir dosyaya kaydedin.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: 1.4.3-174
    dockerProviderVersion: 1.0.0-30
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log
          name: host-log
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        - mountPath: /var/lib/docker/containers
          name: containerlog-path
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   nodeSelector:
    beta.kubernetes.io/os: linux
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
   volumes:
    - name: docker-sock
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
    - name: omsagent-secret
      secret:
       secretName: omsagent-secret
    - name: containerlog-path
      hostPath:
       path: /var/lib/docker/containers
```

Aşağıdaki komut ile DaemonSet oluşturun:

```console
kubectl create -f oms-daemonset.yaml
```

DaemonSet’in oluşturulduğunu görmek için şunu çalıştırın:

```console
kubectl get daemonset
```

Çıktı aşağıdakine benzer:

```output
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Aracılar çalıştırıldıktan sonra Log Analytics’in verileri alıp işlemesi birkaç dakika sürer.

## <a name="access-monitoring-data"></a>İzleme verilerine erişme

[Kapsayıcı çözümü](../../azure-monitor/insights/containers.md) ile Log Analytics portalında veya Azure portalında kapsayıcı izleme verilerini görüntüleyin ve analiz edin.

[Log Analytics portalını](https://mms.microsoft.com) kullanarak Kapsayıcı çözümünü yüklemek için **Çözüm Galerisi**’ne gidin. Sonra **Kapsayıcı Çözümü**’nü ekleyin. Alternatif olarak, Kapsayıcı çözümünü [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview)’ten ekleyebilirsiniz.

Log Analytics portalında, panodaki **Kapsayıcılar** özet kutucuğunu bulun. Kapsayıcı olayları, hatalar, durum, görüntü envanterinin yanı sıra CPU ve bellek kullanımı gibi ayrıntılar için kutucuğa tıklayın. Daha ayrıntılı bilgi için herhangi bir kutucuğun istediğiniz satırına tıklayın veya bir [günlük araması](../../log-analytics/log-analytics-log-searches.md) gerçekleştirin.

![Azure portalda Kapsayıcılar panosu](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Benzer şekilde, Azure portalında **Log Analytics**’e gidip çalışma alanınızın adını seçin. **Kapsayıcılar** özet kutucuğunu görmek için **Çözümler** > **Kapsayıcılar**’a tıklayın. Ayrıntıları görmek için kutucuğa tıklayın.

Verileri sorgulamaya ve analiz etmeye ilişkin ayrıntılı yönergeler için [Azure Log Analytics belgelerine](../../azure-monitor/log-query/log-query-overview.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Log Analytics ile Kubernetes kümenizi izlediniz. Dahil edilen görevler:

> [!div class="checklist"]
> * Log Analytics Çalışma Alanı ayarlarını alma
> * Kubernetes düğümlerinde Log Analytics aracılarını ayarlama
> * Log Analytics portalında veya Azure portalında izleme bilgilerine erişme


Container Service için önceden oluşturulmuş betik örneklerini görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure Container Service betik örnekleri](cli-samples.md)
