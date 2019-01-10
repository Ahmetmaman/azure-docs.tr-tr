---
title: Kapsayıcılar için Azure İzleyici görünümü günlükleri gerçek zamanlı olarak | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure İzleyici ile kubectl kullanmadan kapsayıcı günlüklerini (stdout/stderr) gerçek zamanlı görünümünü açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2018
ms.author: magoedte
ms.openlocfilehash: 27368ec1f41553950ab1689f8b37c15d14d29808
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156672"
---
# <a name="how-to-view-container-logs-real-time-with-azure-monitor-for-containers-preview"></a>Itanium tabanlı sistemler için kapsayıcı günlükleri gerçek zamanlı Azure İzleyici ile kapsayıcılar (Önizleme) için görüntüleme
Şu anda önizlemede olan bu özellik, kubectl komutlarını çalıştırmak zorunda kalmadan Azure Kubernetes Service (AKS), kapsayıcı günlüklerini (stdout/stderr) gerçek zamanlı bir görünüm sağlar. Bu seçeneği belirlediğinizde, yeni bölmesi altında kapsayıcıları performans veri tablosu üzerinde görünür. **kapsayıcıları** görünümü ve canlı günlük başka sorunlara gerçek zamanlı giderilmesine yardımcı olmak için kapsayıcı altyapısı tarafından oluşturulan gösterir.  

Günlükleri erişimi denetlemek için günlükleri desteklediği üç farklı yöntem Canlı:

1. AKS etkin Kubernetes RBAC yetkilendirme olmadan 
2. AKS ile Kubernetes RBAC yetkilendirme etkin
3. Çoklu oturum dayalı AKS Azure Active Directory (AD ile) SAML etkin 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes kümesi olmadan etkin RBAC
 
Kubernetes RBAC yetkilendirme ile yapılandırılmamışsa veya Azure AD çoklu oturum açma ile tümleşik bir Kubernetes kümesi varsa, bu adımları izlemeniz gerekmez. Kubernetes yetkilendirme kube-API kullandığından, salt okunur izinler gereklidir.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC yetkilendirme
Kubernetes RBAC yetkilendirme etkinleştirilirse, küme rolü bağlama uygulamak gerekir. Aşağıdaki örnek adımlarda bu yaml yapılandırma şablondan küme rolünü yapılandırmak nasıl ekleyebileceğiniz gösterilmektedir.   

1. Kopyalayın ve yapıştırın yaml dosyası ve LogReaderRBAC.yaml kaydedin.  

   ```
   kind: ClusterRole 
   apiVersion: rbac.authorization.k8s.io/v1 
   metadata:   
      name: containerHealth-log-reader 
   rules: 
      - apiGroups: [""]   
        resources: ["pods/log"]   
        verbs: ["get"] 
   --- 
   kind: ClusterRoleBinding 
   apiVersion: rbac.authorization.k8s.io/v1 
   metadata:   
      name: containerHealth-read-logs-global 
   subjects:   
      - kind: User     
        name: clusterUser
        apiGroup: rbac.authorization.k8s.io 
    roleRef:   
       kind: ClusterRole
       name: containerHealth-log-reader
       apiGroup: rbac.authorization.k8s.io 
   ```

2. Aşağıdaki komutu çalıştırarak küme kural bağlama oluşturur: `kubectl create -f LogReaderRBAC.yaml`. 

## <a name="configure-aks-with-azure-active-directory"></a>AKS ile Azure Active Directory'yi yapılandırma
AKS, Azure Active Directory (AD) kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. Bu ilk kez yapılandırıyorsanız, bkz. [Azure Active Directory Tümleştirme ile Azure Kubernetes hizmeti](../../aks/aad-integration.md). Oluşturma adımları sırasında [istemci uygulaması](../../aks/aad-integration.md#create-client-application) belirtin **yeniden yönlendirme URI'si**, başka bir URI listeye eklemeniz `https://ininprodeusuxbase.microsoft.com/*`.  

>[!NOTE]
>Kimlik doğrulama işlemini yapılandırmayı Azure Active Directory ile çoklu oturum açma için yalnızca yeni bir AKS kümesi ilk dağıtım sırasında gerçekleştirilebilir. Çoklu oturum zaten dağıtılmış bir AKS kümesi için üzerinde yapılandıramazsınız.  
> 

## <a name="view-live-logs"></a>Canlı günlüklerini görüntüle
Görüntülediğinizde **kapsayıcıları**, yapabilecekleriniz **görünümü kapsayıcı günlüklerini** veya **Canlı kapsayıcı günlüklerini görüntüleme**.  Seçtiğinizde, **görünüm kapsayıcısını Canlı günlükler**, daha fazla gerçek zamanlı sorunları gidermeye yardımcı olması için kapsayıcı altyapısı tarafından oluşturulan gösteren Canlı günlüğe kaydetme ve kapsayıcıları performans veri tablosu altında yeni bir bölme görünür.  
1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Gelen **Microsoft Azure** menüsünde **İzleyici** seçip **kapsayıcıları**.  
3. Bir kapsayıcı altındaki listeden seçin **izlenen kapsayıcıları** görünümü.  
4. Seçin **kapsayıcıları** görünümü ve seçilen kapsayıcı bağlantı için özellikler panelini **Canlı kapsayıcı günlüklerini görüntüleme** listelenir.  
5. AKS kümesi ile SSO AAD kullanılarak yapılandırıldıysa, ilk kez kullanıldığında, tarayıcı oturumu sırasında kimlik doğrulaması istenir. Hesabınızı ve Azure ile kimlik doğrulamasını tamamlamak seçin.  

Başarıyla kimlik doğrulandıktan sonra dinamik günlük bölmesini Orta bölmenin alt kısmında görünür. Getirme durum göstergesi sağda bölmesinin olan yeşil onay işareti, gösteriliyorsa veri alabildiği anlamına gelir.
    
  ![Canlı günlükler alınan veriler](./media/container-insights-live-logs/live-logs-pane-01.png)  

Arama çubuğunda, bu metin günlüğündeki vurgulamak için bir anahtar sözcük göre filtre uygulayabilirsiniz.   

  ![Canlı günlükler bölmesi filtresi örneği](./media/container-insights-live-logs/live-logs-pane-filter-01.png)

Otomatik kaydırma askıya alma ve okuma yeni günlük verilerine el ile kaydırma izin bölmesinde davranışını denetlemek için tıklayın **kaydırma** seçeneği.  Otomatik kaydırma yeniden etkinleştirmek için tıklamanız yeterlidir **kaydırma** yeniden seçeneği.  Tıklayarak günlük verisi alımı duraklatabilirsiniz **duraklatmak** seçenek ve devam etmeye hazır olduğunuzda tıklamanız yeterlidir **Play**.  

![Canlı günlükler bölmesi duraklatma Canlı görünümü](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).