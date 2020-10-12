---
title: Azure Güvenlik denetimi-günlüğe kaydetme ve Izleme
description: Azure Güvenlik denetimini günlüğe kaydetme ve Izleme
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 82114164d70eae71678e70ff2bdb7ea44a54d4cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87076306"
---
# <a name="security-control-logging-and-monitoring"></a>Güvenlik denetimi: günlüğe kaydetme ve Izleme

Güvenlik günlüğü ve izleme, Azure hizmetleri için Denetim günlüklerini etkinleştirme, alma ve depolama ile ilgili etkinliklere odaklanır.

## <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.1 | 6.1 | Microsoft |

Microsoft, Azure kaynakları için zaman kaynaklarını korur, ancak işlem kaynaklarınızın zaman eşitleme ayarlarını yönetme seçeneğiniz vardır.

- [Azure Windows işlem kaynakları için Saat eşitlemesini yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/time-sync)

- [Azure Linux işlem kaynakları için Saat eşitlemesini yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/time-sync)

## <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.2 | 6,5, 6,6 | Müşteri |

Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Azure Izleyici ile Azure sanal makine iç konak günlüklerini toplama](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

## <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.3 | 6,2, 6,3 | Müşteri |

Denetim, güvenlik ve tanılama günlüklerine erişmek için Azure kaynaklarında tanılama ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

## <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.4 | 6,2, 6,3 | Müşteri |

İşlem kaynağının Microsoft 'a ait olması durumunda, Microsoft bunu izlemekten sorumludur. İşlem kaynağı kuruluşunuza aitse, bunu izlemek sizin sorumluluğunuzdadır. İşletim sistemini izlemek için Azure Güvenlik Merkezi 'ni kullanabilirsiniz. İşletim sisteminden Güvenlik Merkezi tarafından toplanan veriler işletim sistemi türü ve sürümü, işletim sistemi (Windows olay günlükleri), çalışan süreçler, makine adı, IP adresleri ve oturum açan kullanıcı içerir. Log Analytics Aracısı Ayrıca kilitlenme bilgi döküm dosyalarını da toplar.

- [Azure Izleyici ile Azure sanal makine iç konak günlüklerini toplama](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Azure Güvenlik Merkezi veri toplamayı anlama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)

## <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.5 | 6.4 | Müşteri |

Azure Izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Log Analytics çalışma alanı saklama süresini ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

- [Log Analytics veri saklama süresini değiştirme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

## <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2,6 | 6.7 | Müşteri |

Anormal davranış için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın.

Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir. 

- [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Log Analytics çalışma alanını anlayın](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

- [Azure Izleyici 'de özel sorgular gerçekleştirme](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

## <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.7 | 6.8 | Müşteri |

Güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikleri izlemek ve uyarmak için Log Analytics çalışma alanıyla Azure Güvenlik Merkezi 'ni kullanın.

Alternatif olarak, Azure Sentinel 'de ve yerleşik verileri etkinleştirebilir.

- [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

- [Log Analytics günlük verilerinde uyarı alma](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

## <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.8 | 8.6 | Müşteri |

Azure sanal makineleri ve Cloud Services için kötü amaçlı yazılımdan koruma olayı toplamayı etkinleştirin.

- [Sanal makineler için Microsoft Antimalware 'i yapılandırma](/powershell/module/servicemanagement/azure.service/set-azurevmmicrosoftantimalwareextension?view=azuresmps-4.0.0)

- [Cloud Services için Microsoft Antimalware yapılandırma](/powershell/module/servicemanagement/azure.service/set-azureserviceantimalwareextension?view=azuresmps-4.0.0)

- [Microsoft kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

## <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2.9 | 8.7 | Müşteri |

Kuruluşunuzun ihtiyaç duyduğu şekilde, DNS günlüğü çözümü için Azure Marketi 'nden bir üçüncü taraf çözümü uygulayın.  

## <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 2,10 | 8.8 | Müşteri |

İşlem oluşturma olayını ve CommandLine alanını günlüğe kaydetmek için desteklenen tüm Azure Windows sanal makinelerinde Microsoft Monitoring Agent kullanın.   Desteklenen Azure Linux sanal makineleri için, her düğüm temelinde konsol günlüğünü el ile yapılandırabilir ve verileri depolamak için Syslog kullanabilirsiniz.  Ayrıca, Azure Izleyici Log Analytics çalışma alanını kullanarak günlükleri gözden geçirin ve Azure sanal makinelerindeki günlüğe kaydedilen verilerde sorgular gerçekleştirin. 

- [Azure Güvenlik Merkezinde veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

- [Azure Izleyici 'de özel sorgular gerçekleştirme](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

- [Azure İzleyici'de Syslog veri kaynakları](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [kimlik ve Access Control](security-control-identity-access-control.md)