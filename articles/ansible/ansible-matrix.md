---
title: Azure için anerişilebilir modül ve Sürüm matrisi
description: Azure için anerişilebilir modül ve Sürüm matrisi
keywords: anerişilebilir, roller, matris, sürüm, Azure, DevOps
ms.topic: reference
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 18654500a78178c46e72f9f6cd01e8507fa179f0
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72241535"
---
# <a name="ansible-module-and-version-matrix"></a>Anerişilebilir modül ve Sürüm matrisi

Anormal, Azure kaynaklarını sağlama ve yapılandırma konusunda kullanılacak bir modül paketini içerir. Bu kaynaklar sanal makineler, ölçek kümeleri, ağ hizmetleri ve kapsayıcı hizmetleri içerir. Bu makalede, Azure için çeşitli anormal modüller ve teslim ettikleri anormal sürümler listelenmiştir.

## <a name="ansible-modules-for-azure"></a>Azure için anerişilebilir modüller

Aşağıdaki modüller doğrudan uzak konaklarda veya playbooks aracılığıyla yürütülebilir.

Bu modüller, anormal bir resmi yayından ve aşağıdaki Microsoft PlayBook rollerinden edinilebilir.

| Azure için anerişilebilir modül                   |  Anerişilebilir 2,4 |  Anerişilebilir 2,5 |  Anerişilebilir 2,6 | Anerişilebilir 2,7 | Anerişilebilir 2,8 | Anormal rol | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|--------------|--------------| 
| **Hesaplamasını**                    |           |                          |                          |                            |           |           |
| azure_rm_availabilityset                    | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_availabilityset_facts              | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_deployment                         | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_deployment_facts                   | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_functionapp                        | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_functionapp_facts                  | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_image                              | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_image_facts                        | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_resource                           | -            | -                           | Evet          | Evet          | Evet          | Evet          |
| azure_rm_resource_facts                     | -            | -                           | Evet          | Evet          | Evet          | Evet          |
| azure_rm_resourcegroup                      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_resourcegroup_facts                | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachine                     | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualmachineextension           | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachineextension_facts      | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_virtualmachineimage_facts          | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescaleset            | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescaleset_facts      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescalesetextension    | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_virtualmachinescalesetextension_facts | -            | -                        | -            | -            | Evet          | Evet          |
| azure_rm_virtualmachinescalesetinstance     | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_virtualmachinescalesetinstance_facts | -            | -                         | -            | -            | Evet          | Evet          |
| **İşlemleri**                              |              |                             |              |              |              |              |
| azure_rm_appgateway                         | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_appgwroute                         | -            | -                           | -            | -            | -          | Evet          |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | -          | Evet          |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | -          | Evet          |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | -          | Evet          |
| azure_rm_applicationsecuritygroup           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_applicationsecuritygroup_facts     | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_cdnendpoint                        | -            | -                         | -          | -            | Evet          | Evet          |
| azure_rm_cdnendpoint_facts                  | -            | -                         | -          | -            | Evet          | Evet          |
| azure_rm_cdnprofile                         | -            | -                         | -          | -            | Evet          | Evet          |
| azure_rm_cdnprofile_facts                   | -            | -                         | -          | -            | Evet          | Evet          |
| azure_rm_dnsrecordset                       | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_dnsrecordset_facts                 | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_dnszone                            | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_dnszone_facts                      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_loadbalancer                       | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_loadbalancer_facts                 | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_networkinterface                   | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_networkinterface_facts             | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_publicipaddress                    | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_publicipaddress_facts              | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_route                              | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_routetable                         | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_routetable_facts                   | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_securitygroup                      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_subnet                             | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_subnet_facts                       | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_trafficmanagerendpoint             | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerendpoint_facts       | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerprofile              | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerprofile_facts        | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_virtualnetwork                     | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualnetwork_facts               | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualnetworkpeering              | -            | -                         | -          | -            | Evet          | Evet          |
| **Depo**                    |           |                          |                          |                            |           |           |
| azure_rm_manageddisk                        | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_manageddisk_facts                  | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_storageaccount                     | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_storageaccount_facts               | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_storageblob                        | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| **Web**                    |           |                          |                          |                             |           |           |
| azure_rm_appserviceplan                     | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_appserviceplan_facts               | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_webapp                             | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_webapp_facts                       | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_webappslot                         | -            | -                         | -          | -            | Evet          | Evet          |
| **Kapsayıcılar**                    |           |                          |                          |                            |           |           |
| azure_rm_acs                                | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_aks                                | -            | -                           | Evet          | Evet          | Evet          | Evet          |
| azure_rm_aks_facts                          | -            | -                           | Evet          | Evet          | Evet          | Evet          |
| azure_rm_aksversion_facts                   | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_containerinstance                  | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_containerinstance_facts            | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_containerregistry                  | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_containerregistry_facts            | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Evet          | Evet          |
| **Veritabanları**                    |           |                          |                          |                             |           |           |
| azure_rm_cosmosdbaccount                    | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_cosmosdbaccount_facts              | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbconfiguration               | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbconfiguration_facts         | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbdatabase                    | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbdatabase_facts              | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbfirewallrule                | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbfirewallrule_facts          | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbserver                      | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mariadbserver_facts                | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mysqldatabase                      | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_mysqlserver                        | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_postgresqldatabase                 | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_postgresqlserver                   | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_rediscache                         | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_rediscache_facts                   | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_rediscachefirewallrule             | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_sqldatabase                        | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Evet          | Evet          | Evet          |
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_sqlserver                          | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_sqlserver_facts                    | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| **Analiz**                    |           |                          |                          |                             |           |           |
| azure_rm_hdinsightcluster                   | -            | -                           | -            | -            | Evet          | Evet          |
| **Tümleştirme**                    |           |                          |                          |                             |           |           |
| azure_rm_servicebus                         | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_servicebus_facts                   | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_servicebusqueue                    | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_servicebussaspolicy                | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_servicebustopic                    | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_servicebustopicsubscription        | -            | -                           | -            | -            | Evet          | Evet          |
| **Security**                    |           |                          |                          |                             |           |           |
| azure_rm_keyvault                           | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -          | Evet          | Evet          |
| azure_rm_keyvaultkey                        | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_keyvaultsecret                     | -            | Evet                         | Evet          | Evet          | Evet          | Evet          |
| azure_rm_roleassignment                     | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_roleassignment_facts               | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_roledefinition                     | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_roledefinition_facts               | -            | -                           | -            | -            | Evet          | Evet          |
| **DevOps**               |           |                          |                          |                             |           |           |
| azure_rm_devtestlab                         | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlab_facts                   | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabarmtemplate_facts        | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabartifact_facts           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabartifactsource           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabartifactsource_facts     | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabcustomimage              | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabenvironment              | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabpolicy                   | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabschedule                 | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabvirtualmachine           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabvirtualmachine_facts | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabvirtualnetwork           | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabvirtualnetwork_facts     | -            | -                           | -            | -            | Evet          | Evet          |
| **Azure Izleyici**          |           |                          |                          |                             |           |           |
| azure_rm_autoscale                  | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_autoscale_facts            | -            | -                         | -          | Evet          | Evet          | Evet          |
| azure_rm_loganalyticsworkspace              | -            | -                           | -            | -            | Evet          | Evet          |
| azure_rm_loganalyticsworkspace_facts        | -            | -                           | -            | -            | Evet          | Evet          |

## <a name="introduction-to-playbook-role-for-azure"></a>Azure için PlayBook rolüne giriş

[Azure_preview_module PlayBook rolü](https://galaxy.ansible.com/Azure/azure_preview_modules/) , en son Azure modüllerini içerir. Güncelleştirmeler ve hata düzeltmeleri, resmi olarak erişilebilir sürümden daha zamanında yapılır. Azure kaynak sağlama amaçları için kullanılması durumunda, `azure_preview_module` PlayBook rolünü yüklemek için önerilir.

@No__t-0 PlayBook rolü her üç haftada bir yayımlanır.

## <a name="next-steps"></a>Sonraki adımlar

PlayBook rolleri hakkında daha fazla bilgi için bkz. yeniden [kullanılabilir PlayBook 'ları oluşturma](https://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
