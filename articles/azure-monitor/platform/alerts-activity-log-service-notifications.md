---
title: Etkinlik günlüğü uyarıları, Azure hizmet bildirimleri alma
description: SMS, e-posta veya Web kancası Azure hizmet meydana geldiğinde bildirim alın.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.openlocfilehash: b5c11517209b9dfea8a28ce09084abd76d789d78
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270952"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalını kullanarak hizmet durumu bildirimlerine için etkinlik günlüğü uyarıları ayarlama işlemini göstermektedir.  

Azure, Azure aboneliğinizin hizmet durumu bildirimlerini gönderirken bir uyarı alabilirsiniz. Şuna bağlı olarak uyarıyı yapılandırabilirsiniz:

- Hizmet durumu bildirimi (hizmet sorunları, planlı bakımı, sistem durumu danışmanları) sınıfı.
- Etkilenen abonelik.
- Etkilenen hizmetler.
- Etkilenen olduğu bölgelerin.

> [!NOTE]
> Hizmet durumu bildirimlerine, sistem durumu olaylarını kaynakla ilgili bir uyarı göndermez.

Kimin uyarı göndermesi gerektiğini de yapılandırabilirsiniz:

- Var olan bir eylem grubu seçin.
- (Bu, gelecekteki uyarılar için kullanılabilir) yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md).

Azure Resource Manager şablonları kullanarak hizmet durumu bildirimi uyarılarını yapılandırma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları](alerts-activity-log.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak yeni bir eylem grubu için bir hizmet durumu bildirimi üzerinde uyarı oluşturma
1. İçinde [portalı](https://portal.azure.com)seçin **hizmet durumu**.

    !["Hizmet durumu" hizmeti](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. İçinde **uyarılar** bölümünden **sistem durumu uyarılarını**.

    !["Sistem Durumu Uyarıları" sekmesi](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. Seçin **Oluştur hizmet durumu Uyarısı** ve alanları doldurun.

    !["Hizmet durumu uyarısı oluştur" komutu](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Seçin **abonelik**, **Hizmetleri**, ve **bölgeleri** için uyarı almak istediğiniz.

    !["Etkinlik günlüğü uyarısı Ekle" iletişim kutusu](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Bu abonelik, etkinlik günlüğü uyarısı kaydetmek için kullanılır. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğünde olayları izler.

1. Seçin **olay türleri** için uyarı almak istediğiniz: *Hizmet sorunu*, *planlı Bakım*, ve *sistem durumu danışmanları* 

1. Girerek, uyarı ayrıntılarını tanımlama bir **uyarı kuralı adı** ve **açıklama**.

1. Seçin **kaynak grubu** kaydedilecek uyarı almak istediğiniz.

1. Yeni bir eylem grubu seçerek oluşturma **yeni eylem grubu**. Bir ad girin **eylem grubu adı** kutu ve bir ad girin **kısa ad** kutusu. Kısa ad bu uyarı tetiklendiğinde gönderilen bildirimleri başvuruluyor.

    ![Yeni bir eylem grubu oluştur](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. Alıcıları listesi, alıcı sağlayarak tanımlayın:

    a. **Ad**: Alıcının adı, diğer adı veya tanımlayıcısı girin.

    b. **Eylem türü**: SMS, e-posta, Web kancası, Azure uygulama ve daha fazlasını seçin.

    c. **Ayrıntılar**: Seçilen eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI, vb. girin.

1. Seçin **Tamam** eylem grubu oluşturmaya ve ardından **uyarı kuralı oluştur** Uyarınız tamamlanması.

Birkaç dakika içinde uyarı etkin ve tetiklemek oluşturma sırasında belirttiğiniz koşullara göre başlar.

Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](../../service-health/service-health-alert-webhook-guide.md). Etkinlik günlüğü uyarıları için Web kancası şeması hakkında daha fazla bilgi için bkz: [günlük uyarıları Azure etkinlik için Web kancaları](../../azure-monitor/platform/activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu gelecekteki tüm uyarı tanımları için var olan bir eylem grubu olarak yeniden kullanılabilir.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir uyarı mevcut eylem grubu için bir hizmet durumu bildirimi oluşturun

1. 1 ile 7, hizmet durumu bildirimi oluşturmak için önceki bölümdeki adımları izleyin. 

1. Altında **tanımla eylem grubu**, tıklayın **seçme eylemini grubu** düğmesi. Uygun bir eylem grubu seçin.

1. Seçin **Ekle** eylem grubu eklemek ve ardından **uyarı kuralı oluştur** Uyarınız tamamlanması.

Birkaç dakika içinde uyarı etkin ve tetiklemek oluşturma sırasında belirttiğiniz koşullara göre başlar.

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak yeni bir eylem grubu için bir hizmet durumu bildirimi üzerinde uyarı oluşturma

Bir e-posta hedef ile bir eylem grubu oluşturur ve hedef aboneliği için tüm hizmet durumu bildirimlerini sağlayan bir örnek verilmiştir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
    },
    "resources": [
        {
            "comments": "Action Group",
            "type": "microsoft.insights/actionGroups",
            "name": "[parameters('actionGroups_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "groupShortName": "[parameters('actionGroups_name')]",
                "enabled": true,
                "emailReceivers": [
                    {
                        "name": "[parameters('actionGroups_name')]",
                        "emailAddress": "[parameters('emailAddress')]"
                    }
                ],
                "smsReceivers": [],
                "webhookReceivers": []
            },
            "dependsOn": []
        },
        {
            "comments": "Service Health Activity Log Alert",
            "type": "microsoft.insights/activityLogAlerts",
            "name": "[parameters('activityLogAlerts_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "scopes": [
                    "[variables('alertScope')]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
                            "webhookProperties": {}
                        }
                    ]
                },
                "enabled": true,
                "description": ""
            },
            "dependsOn": [
                "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
            ]
        }
    ]
}
```

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme

Bir uyarı oluşturduktan sonra görünür **uyarılar** bölümünü **İzleyici**. Yönetmek istediğiniz uyarıyı seçin:

* Düzenleyin.
* Silin.
* Geçici olarak durdurmak veya uyarı bildirimleri almaya devam etmek istiyorsanız, etkinleştirmek veya devre dışı.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](../../service-health/service-health-alert-webhook-guide.md).
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Hakkında bilgi edinin [bildirim hız sınırlaması](../../monitoring-and-diagnostics/monitoring-alerts-rate-limiting.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](../../azure-monitor/platform/activity-log-alerts-webhook.md).
- Alma bir [etkinlik günlüğü uyarılarına genel bakış](../../monitoring-and-diagnostics/monitoring-overview-alerts.md)ve uyarıları alma hakkında bilgi edinin. 
- Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md).
