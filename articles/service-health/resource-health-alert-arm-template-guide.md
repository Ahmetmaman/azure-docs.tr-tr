---
title: Resource Manager şablonlarını kullanarak bir Azure kaynak sistem durumu uyarılarını yapılandırma | Microsoft Docs
description: Azure kaynaklarınızı kullanılamaz duruma geldiğinde programlı olarak bunu size bildirecek uyarılar oluşturun.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/4/2018
ms.author: shtabriz
ms.openlocfilehash: dcc978c8e169cca2e2cd91a63ef1814a8c3bd87c
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184109"
---
# <a name="configure-resource-health-alerts-using-resource-manager-templates"></a>Resource Manager şablonlarını kullanarak kaynak sistem durumu uyarılarını yapılandırma

Bu makale, Azure Resource Manager şablonları ve Azure PowerShell kullanarak program aracılığıyla kaynak sistem durumu etkinlik günlüğü uyarıları oluşturma işlemini gösterir.

Azure kaynaklarınızın geçerli ve geçmiş sistem durumu hakkında bilgilendirmeye azure kaynak durumu korur. Azure kaynak durumu uyarıları gerçek zamanlı olduğunda bu kaynakları bir değişiklik sistem durumlarına sahip bildirebilir. Kaynak durumu oluşturma uyarılar program aracılığıyla oluşturma ve Uyarıları toplu özelleştirme kullanıcıların sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu sayfadaki yönergeleri izleyerek için birkaç önceden ayarlamalar yapmanız gerekir:

1. Yüklemeniz gereken [Azure PowerShell Modülü](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (`AzureRm`)
2. Şunları yapmanız [oluşturmak veya bir eylem grubu yeniden](../azure-monitor/platform/action-groups.md) size bildirecek şekilde yapılandırılmış

## <a name="instructions"></a>Yönergeler
1. PowerShell kullanarak Azure'da hesabınızı kullanarak oturum açın ve etkileşim kurmak istediğiniz aboneliği seçin

        Login-AzureRmAccount
        Select-AzureRmSubscription -Subscription <subscriptionId>

    > Kullanabileceğiniz `Get-AzureRmSubscription` abonelikleri listelemek için erişiminiz olması.

2. Bulun ve tam Azure Resource Manager Kimliğini kaydetmek için eylem grubu

        (Get-AzureRmActionGroup -ResourceGroupName <resourceGroup> -Name <actionGroup>).Id

3. Oluşturmak ve kaydetmek için kaynak durumu uyarı olarak Resource Manager şablonu `resourcehealthalert.json` ([aşağıdaki ayrıntılara göz atın](#resource-manager-template-for-resource-health-alerts))

4. Bu şablonu kullanarak yeni bir Azure Resource Manager dağıtım oluştur

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <resourceGroup> -TemplateFile <path\to\resourcehealthalert.json>

5. Uyarı adı ve eylem grubu kaynağı daha önce kopyaladığınız kimliği yazın istenir:

        Supply values for the following parameters:
        (Type !? for Help.)
        activityLogAlertName: <Alert Name>
        actionGroupResourceId: /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.insights/actionGroups/<actionGroup>

6. Her şey başarıyla yaradıysa, PowerShell'de bir onay alırsınız.

        DeploymentName          : ExampleDeployment
        ResourceGroupName       : <resourceGroup>
        ProvisioningState       : Succeeded
        Timestamp               : 11/8/2017 2:32:00 AM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name                     Type       Value
                                ===============          =========  ==========
                                activityLogAlertName     String     <Alert Name>
                                activityLogAlertEnabled  Bool       True
                                actionGroupResourceId    String     /...
        
        Outputs                 :
        DeploymentDebugLogLevel :

Bu işlemi tamamen otomatik bir hale getirme planlıyorsanız, yalnızca 5. adım değerlerinin istemez için Resource Manager şablonu Düzen gerektiğini unutmayın.

## <a name="resource-manager-template-for-resource-health-alerts"></a>Kaynak sistem durumu uyarıları için Resource Manager şablonu

Kaynak durumu uyarı oluşturmak için başlangıç noktası olarak bu temel şablonu kullanabilirsiniz. Bu şablon, yazıldığı gibi çalışır ve bir Abonelikteki tüm kaynaklar genelinde tüm yeni etkinleştirilen kaynak sistem durumu olayları uyarıları almak için kaydolduktan.

> Bu makalenin alt kısmında da sinyal/gürültü oranına için bu şablona göre kaynak sistem durumu uyarılarını artırmalısınız daha karmaşık bir uyarı şablon ekledik.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": true,
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "ResourceHealth"
            },
            {
              "field": "status",
              "equals": "Active"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

Ancak, bunun gibi geniş bir uyarı genellikle önerilmez. Nasıl Biz bu uyarı, biz aşağıda verdiğiniz olayları odaklanmak için aşağı kapsamını belirleyebilirsiniz öğrenin.

### <a name="adjusting-the-alert-scope"></a>Uyarı kapsamı ayarlama

Kaynak sistem durumu uyarıları, üç farklı kapsamlardaki olayları izlemek için yapılandırılabilir:

 * Abonelik Düzeyi
 * Kaynak grubu düzeyi
 * Kaynak düzeyi

Uyarı şablonu abonelik düzeyinde yapılandırılır, ancak yalnızca belirli kaynakları veya belirli bir kaynak grubu içindeki kaynakları hakkında bilgilendirmek üzere uyarınızı yapılandırmak istiyorsanız, yalnızca değiştirmeniz gerekir `scopes` yukarıdaki bölümde şablonu.

Kaynak grubu düzeyi için bir kapsam, kapsamları bölümü gibi görünmelidir:
```json
"scopes": [
    "/subscriptions/<subscription id>/resourcegroups/<resource group>"
],
```

Ve kaynak düzeyi kapsam için kapsam bölümü şöyle görünmelidir:

```json
"scopes": [
    "/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/<resource>"
],
```

Örneğin, `"/subscriptions/d37urb3e-ed41-4670-9c19-02a1d2808ff9/resourcegroups/myRG/providers/microsoft.compute/virtualmachines/myVm"`

> Azure Portalı'na gidin ve URL'de bu dizesini almak için Azure kaynağınıza görüntülerken bakın.

### <a name="adjusting-the-resource-types-which-alert-you"></a>Kaynağı ayarlama, uyarı türleri

Uyarılar abonelik veya kaynak grubu düzeyinde kaynakları farklı türde olabilir. Yalnızca kaynak türleri bir belirli bir alt kümesinden gelen uyarılar sınırlandırmak istiyorsanız, tanımlayabilirsiniz `condition` şablon bölümünü şu şekilde:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "resourceType",
                    "equals": "Microsoft.Compute/virtualMachines",
                    "containsAny": null
                },
                {
                    "field": "resourceType",
                    "equals": "Microsoft.Storage/storageAccounts",
                    "containsAny": null
                },
                ...
            ]
        }
    ]
},
```

Burada kullandığımız `anyOf` belirlediğimizden, belirli kaynak türlerine hedeflemek için uyarılar verme koşullardan herhangi biri ile eşleştirilecek kaynak sistem durumu uyarısı izin vermek için sarmalayıcı.

### <a name="adjusting-the-resource-health-events-that-alert-you"></a>Kaynak durumu olayları size uyarı ayarlama
Kaynak sistem durumu olayı geçmeleri, sistem durumu olayı durumunu temsil eden bir dizi aşamadan gidebilirsiniz: `Active`, `InProgress`, `Updated`, ve `Resolved`.

Uyarınız yalnızca bildirmek için yapılandırmak istediğiniz bu durumda, yalnızca bir kaynak sistem durumu kötü olduğunda bildirim almak isteyebilirsiniz `status` olduğu `Active`. Ayrıca diğer aşamalar hakkında bildirim almak istiyorsanız, ancak bu ayrıntıları ekleyebilirsiniz şu şekilde:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "status",
                    "equals": "Active"
                },
                {
                    "field": "status",
                    "equals": "InProgress"
                },
                        {
                    "field": "status",
                    "equals": "Resolved"
                }
            ]
        }
    ]
}
```

Sistem durumu olaylarını dört tüm aşamalarında için bildirim almak isteyen, bu durum tümünü bir araya kaldırabilirsiniz ve uyarı bakılmadan, size bildirecektir `status` özelliği.

### <a name="adjusting-the-resource-health-alerts-to-avoid-unknown-events"></a>"Bilinmeyen" olayları önlemek için kaynak durumu uyarıları ayarlama

Azure kaynak durumu raporlama yapabilir, en son kaynaklarınızın sistem durumunu sürekli olarak bunları izleyerek test çalıştırıcılar kullanma. İlgili bildirilen durum durumlar şunlardır: "Kullanılabilir olma", "Kullanılamıyor" ve "Düşürülmüş". Ancak, Çalıştırıcısı ve Azure kaynak iletişim kurmada başarısız olduğu durumlarda, kaynak için bir "Bilinmeyen" durumu bildirilir ve bir "Etkin" sistem durumu olayı kabul edilir.

Bir kaynak, "Bilinmeyen" bildirdiğinde, ancak sistem durumunun son doğru rapor bu yana değişmemiştir olasıdır. "Bilinmeyen" olayları ile ilgili uyarılar ortadan kaldırmak istiyorsanız, bu mantığı şablonda belirtebilirsiniz:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Available",
                    "containsAny": null
                },
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Unavailable",
                    "containsAny": null
                },
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Degraded",
                    "containsAny": null
                }
            ]
        },
        {
            "anyOf": [
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Available",
                    "containsAny": null
                },
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Unavailable",
                    "containsAny": null
                },
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Degraded",
                    "containsAny": null
                }
            ]
        },
    ]
},
```

Bu örnekte biz yalnızca burada geçerli ve önceki sistem durumunu "Bilinmeyen" yok olayları bildirme. Bu değişikliği doğrudan, cep telefonu veya e-posta uyarıları gönderilen yararlı bir toplama olabilir.

### <a name="adjusting-the-alert-to-avoid-user-initiated-events"></a>Kullanıcı tarafından başlatılan olayları önlemek için uyarı ayarlama

Kaynak sistem durumu olayları tetikleyicisi tarafından başlatılan bir platform olabilir ve kullanıcı tarafından başlatılan olayları. Bu sistem durumu olayı, Azure platformu tarafından neden yalnızca bir bildirim göndermesini mantıklı olabilir.

Yalnızca bu tür olaylar için filtre uygulamak için uyarınızı yapılandırmak kolaydır:

```json
"condition": {
    "allOf": [
        ...,
        {
            "field": "properties.cause",
            "equals": "PlatformInitiated",
            "containsAny": null
        }
    ]
}
```

## <a name="recommended-resource-health-alert-template"></a>Önerilen kaynak durumu uyarı şablonu

Önceki bölümde açıklanan farklı ayarlamaları kullanarak sinyal/gürültü oranına en üst düzeye çıkarmak üzere yapılandırıldığı kapsamlı bir uyarı şablonu oluşturabilirsiniz.

İşte ne kullanmanızı öneririz:
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "activityLogAlertName": {
            "type": "string",
            "metadata": {
                "description": "Unique name (within the Resource Group) for the Activity log alert."
            }
        },
        "actionGroupResourceId": {
            "type": "string",
            "metadata": {
                "description": "Resource Id for the Action group."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Insights/activityLogAlerts",
            "apiVersion": "2017-04-01",
            "name": "[parameters('activityLogAlertName')]",
            "location": "Global",
            "properties": {
                "enabled": true,
                "scopes": [
                    "[subscription().id]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ResourceHealth",
                            "containsAny": null
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Available",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Unavailable",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Degraded",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Available",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Unavailable",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Degraded",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.cause",
                                    "equals": "PlatformInitiated",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "status",
                                    "equals": "Active",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "Resolved",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "InProgress",
                                    "containsAny": null
                                }
                            ]
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[parameters('actionGroupResourceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

Ancak, en iyi hangi yapılandırmalar sizin için geçerli olduğunu, bu nedenle kendi özelleştirme yapmak için bu belgede verilen araçları kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi edinin:
-  [Azure kaynak durumu genel bakış](Resource-health-overview.md)
-  [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)

Hizmet durumu uyarıları oluşturun:
-  [Hizmet durumu için uyarıları yapılandırma](../azure-monitor/platform/alerts-activity-log-service-notifications.md) 
