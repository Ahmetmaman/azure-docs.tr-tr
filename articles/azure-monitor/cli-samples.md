---
title: Azure Izleyici CLı örnekleri
description: Azure Izleyici özellikleri için örnek CLı komutları. Azure Izleyici, uyarı bildirimleri göndermenizi, yapılandırılmış telemetri verileri değerlerine göre Web URL 'Lerini çağırmayı, otomatik ölçeklendirme Cloud Services, sanal makineler ve Web Apps sağlayan bir Microsoft Azure hizmetidir.
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/16/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8eaf8c2e140f0b323db0c20a2e9946884c51df04
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039181"
---
# <a name="azure-monitor-cli-samples"></a>Azure Izleyici CLı örnekleri
Bu makalede, Azure Izleyici özelliklerine erişmenize yardımcı olacak örnek komut satırı arabirimi (CLı) komutları gösterilmektedir. Azure Izleyici, Cloud Services, sanal makineleri ve Web Apps otomatik olarak ve yapılandırılmış telemetri verileri değerlerine göre uyarı bildirimleri göndermenizi veya Web URL 'Lerini çağırmayı sağlar.

## <a name="prerequisites"></a>Önkoşullar

Azure CLı 'yı henüz yüklemediyseniz [Azure CLI yükleme](/cli/azure/install-azure-cli)yönergelerini izleyin. Ayrıca, CLı 'yı tarayıcınızda etkileşimli bir deneyim olarak çalıştırmak için [Azure Cloud Shell](/azure/cloud-shell) de kullanabilirsiniz. [Azure IZLEYICI CLI başvurusu](/cli/azure/monitor)'ndaki tüm kullanılabilir komutların tam başvurusuna bakın. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
İlk adım, Azure hesabınızda oturum açmak için kullanılır.

```azurecli
az login
```

Bu komutu çalıştırdıktan sonra ekrandaki yönergeler aracılığıyla oturum açmanız gerekir. Tüm komutlar varsayılan aboneliğiniz bağlamında çalışır.

Geçerli aboneliğinizin ayrıntılarını listeleyin.

```azurecli
az account show
```

Çalışma bağlamını farklı bir aboneliğe değiştirin.

```azurecli
az account set -s <Subscription ID or name>
```

Desteklenen tüm Azure Izleyici komutlarının listesini görüntüleyin.

```azurecli
az monitor -h
```

## <a name="view-activity-log"></a>Etkinlik günlüğünü görüntüle

Etkinlik günlüğü olaylarının listesini görüntüleyin.

```azurecli
az monitor activity-log list
```

Tüm kullanılabilir seçenekleri görüntüleyin.

```azurecli
az monitor activity-log list -h
```

Günlükleri bir resourceGroup öğesine göre listeleyin.

```azurecli
az monitor activity-log list --resource-group <group name>
```

Günlükleri çağırana göre listele.

```azurecli
az monitor activity-log list --caller myname@company.com
```

Bir tarih aralığı içinde bir kaynak türü üzerinde çağırana göre günlükleri listeleyin.

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Uyarılarla çalışma 
> [!NOTE]
> Şu anda CLı 'de yalnızca uyarılar (klasik) desteklenir. 

### <a name="get-alert-classic-rules-in-a-resource-group"></a>Bir kaynak grubunda uyarı (klasik) kuralları alın

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-classic-rule"></a>Ölçüm Uyarısı (klasik) kuralı oluşturma

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-classic-rule"></a>Uyarı (klasik) kuralını silme

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Günlük profilleri

Günlük profilleriyle çalışmak için bu bölümdeki bilgileri kullanın.

### <a name="get-a-log-profile"></a>Günlük profili al

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Saklama ile bir günlük profili ekleme

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Bekletme ve EventHub ile bir günlük profili ekleme

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>Günlük profilini kaldırma

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>Tanılama

Tanılama ayarlarıyla çalışmak için bu bölümdeki bilgileri kullanın.

### <a name="get-a-diagnostic-setting"></a>Tanılama ayarı al

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluştur 

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <storage account ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

### <a name="delete-a-diagnostic-setting"></a>Tanılama ayarını Sil

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Otomatik Ölçeklendirme

Otomatik ölçeklendirme ayarlarıyla çalışmak için bu bölümdeki bilgileri kullanın. Bu örnekleri değiştirmeniz gerekir.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Bir kaynak grubu için otomatik ölçeklendirme ayarlarını al

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Bir kaynak grubundaki otomatik ölçeklendirme ayarlarını ada göre al

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını ayarla

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```
