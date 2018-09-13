---
title: SQL veritabanı uyarılar oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Belirttiğiniz koşullar karşılandığında bildirimleri veya otomasyonu tetikleyebilir SQL veritabanı uyarılar oluşturmak için Azure portalını kullanın.
author: aamalvea
manager: craigg
services: sql-database
ms.service: sql-database
ms.custom: monitor and tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: aamalvea
ms.openlocfilehash: aec4ce2e4fb22872673c977816ffbe8616567885
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44718245"
---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Azure SQL veritabanı ve veri ambarı için Uyarılar oluşturmak için Azure portalını kullanma

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalını kullanarak Azure SQL veritabanı ve veri ambarı uyarıları ayarlama işlemini göstermektedir. Bu makalede ayrıca uyarı nokta ayarlamaya yönelik en iyi yöntemler sağlar.    

İzleme ölçümlerini veya olayları, Azure hizmetleriniz hakkındaki bağlı olarak bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -belirtilen bir ölçüm değeri bir eşiği herhangi bir yönde atadığınız geçtiğinde bir uyarı tetikler. Diğer bir deyişle, her ikisi de tetikleyen zaman koşul önce ve sonra daha sonra ne zaman koşulu artık karşılanmıyor.    
* **Etkinlik günlüğü olayları** -üzerinde bir uyarıyı tetikleyebilir *her* olay veya yalnızca belirli sayıda olayları oluşur.

Bir uyarı tetiklendiğinde aşağıdakileri yapılandırabilirsiniz:

* Hizmet Yöneticisi ve ortak yöneticilerine e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* Web kancası çağırma

Yapılandırma ve uyarı kuralları kullanma hakkında bilgi edinin

* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Komut satırı arabirimi (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portalı ile bir ölçüm üzerinde uyarı kuralı oluşturma
1. İçinde [portalı](https://portal.azure.com/)izlemek olan kaynağı bulun ve seçin.
2. Bu adım, SQL veritabanı ve SQL DW elastik havuzları için farklıdır: 

   - **SQL veritabanı ve elastik havuzlar yalnızca**: seçin **uyarılar** veya **uyarı kuralları** izleme bölümü altında. Simge ve metin, farklı kaynaklar için biraz değişebilir.  
   
     ![İzleme](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **SQL DW yalnızca**: seçin **izleme** ortak görevleri bölümünün altında. Tıklayın **DWU kullanımı** grafiği.

     ![ORTAK GÖREVLER](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Seçin **uyarısı Ekle** komut ve alanları doldurun.
   
    ![Uyarı Ekle](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Adı** Uyarınız kuralı ve seçin bir **açıklama**, bildirim e-postalarda de gösterilmektedir.
5. Seçin **ölçüm** izleyin ve ardından istediğiniz bir **koşul** ve **eşiği** ölçüm için değer. Ayrıca **süresi** uyarı tetiklenmeden önce ölçüm kuralının karşılanması gereken süre. Örneğin, "PT5M" süresi kullanıyorsanız ve uyarıyı % 80'in CPU görünüyor, uyarının ne zaman tetikler **ortalama** 5 dakika boyunca % 80'in CPU olmuştur. İlk tetikleyici gerçekleşir sonra ortalama CPU üzerinde 5 dakika % 80 aşağısına olduğunda tekrar tetikler. CPU ölçüm dakikada bir gerçekleşir. Desteklenen zaman pencereleri için aşağıdaki tabloya başvurun ve toplama yazın ve her uyarı kullandığı değil tüm uyarılar, ortalama değer kullanın.   
6. Denetleme **e-posta sahipleri...**  yöneticileri ve ortak yöneticilerin uyarı tetiklendiğinde almayacağınızı istiyorsanız.
7. Uyarı tetiklendiğinde bildirim alacak başka e-postalar istiyorsanız, bunları eklemek **ek yönetici email(s)** alan. Noktalı virgülle - birden çok e-postaları ayırın  *email@contoso.com;email2@contoso.com*
8. Geçerli bir URI koymak **Web kancası** adlı bir uyarı tetiklendiğinde istiyorsanız alan.
9. Seçin **Tamam** uyarı oluşturma işlemi tamamlandığında.   

Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="managing-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturulduktan sonra bunu seçebilirsiniz ve:

* Ölçüm eşiği ve gerçek değerler önceki gün gösteren bir grafiği görüntüleyin.
* Düzenleyin veya silin.
* **Devre dışı** veya **etkinleştirme** , geçici olarak durdurmak veya bu uyarı için bildirimleri almaya devam etmek istiyorsanız.


## <a name="sql-database-alert-values"></a>SQL veritabanı uyarı değerleri

| Kaynak Türü | Ölçüm Adı | Kolay Ad | Toplama Türü | En düşük uyarı zaman penceresi|
| --- | --- | --- | --- | --- |
| SQL veritabanı | cpu_percent | CPU yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | physical_data_read_percent | Veri G/Ç yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | log_write_percent | Günlük g/ç yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | dtu_consumption_percent | DTU yüzdesi | Ortalama | 5 dakika |
| SQL veritabanı | depolama | Toplam veritabanı boyutu | Maksimum | 30 dakika |
| SQL veritabanı | connection_successful | Başarılı bağlantılar | Toplam | 10 dakika |
| SQL veritabanı | connection_failed | Başarısız Bağlantılar | Toplam | 10 dakika |
| SQL veritabanı | blocked_by_firewall | Güvenlik Duvarı tarafından engellendi | Toplam | 10 dakika |
| SQL veritabanı | Kilitlenme | Kilitlenmeler | Toplam | 10 dakika |
| SQL veritabanı | storage_percent | Veri boyutu yüzdesi | Maksimum | 30 dakika |
| SQL veritabanı | xtp_storage_percent | Bellek içi OLTP depolama percent(Preview) | Ortalama | 5 dakika |
| SQL veritabanı | workers_percent | Çalışan yüzde | Ortalama | 5 dakika |
| SQL veritabanı | sessions_percent | Oturumlarının yüzde | Ortalama | 5 dakika |
| SQL veritabanı | dtu_limit | DTU sınırı | Ortalama | 5 dakika |
| SQL veritabanı | dtu_used | Kullanılan DTU | Ortalama | 5 dakika |
||||||
| Elastik havuz | cpu_percent | CPU yüzdesi | Ortalama | 10 dakika |
| Elastik havuz | physical_data_read_percent | Veri G/Ç yüzdesi | Ortalama | 10 dakika |
| Elastik havuz | log_write_percent | Günlük g/ç yüzdesi | Ortalama | 10 dakika |
| Elastik havuz | dtu_consumption_percent | DTU yüzdesi | Ortalama | 10 dakika |
| Elastik havuz | storage_percent | Depolama yüzdesi | Ortalama | 10 dakika |
| Elastik havuz | workers_percent | Çalışan yüzde | Ortalama | 10 dakika |
| Elastik havuz | eDTU_limit | eDTU sınırı | Ortalama | 10 dakika |
| Elastik havuz | storage_limit | Depolama sınırı | Ortalama | 10 dakika |
| Elastik havuz | eDTU_used | kullanılan eDTU | Ortalama | 10 dakika |
| Elastik havuz | storage_used | Kullanılan depolama | Ortalama | 10 dakika |
||||||               
| SQL Veri Ambarı | cpu_percent | CPU yüzdesi | Ortalama | 10 dakika |
| SQL Veri Ambarı | physical_data_read_percent | Veri G/Ç yüzdesi | Ortalama | 10 dakika |
| SQL Veri Ambarı | depolama | Toplam veritabanı boyutu | Maksimum | 10 dakika |
| SQL Veri Ambarı | connection_successful | Başarılı bağlantılar | Toplam | 10 dakika |
| SQL Veri Ambarı | connection_failed | Başarısız Bağlantılar | Toplam | 10 dakika |
| SQL Veri Ambarı | blocked_by_firewall | Güvenlik Duvarı tarafından engellendi | Toplam | 10 dakika |
| SQL Veri Ambarı | service_level_objective | Veritabanının hizmet düzeyi hedefi | Toplam | 10 dakika |
| SQL Veri Ambarı | dwu_limit | dwu limiti | Maksimum | 10 dakika |
| SQL Veri Ambarı | dwu_consumption_percent | DWU yüzdesi | Ortalama | 10 dakika |
| SQL Veri Ambarı | dwu_used | Kullanılan DWU | Ortalama | 10 dakika |
||||||


## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bakışın](../monitoring-and-diagnostics/monitoring-overview.md) toplamak ve izlemek bilgi türleri dahil olmak üzere.
* Daha fazla bilgi edinin [uyarıları Web kancalarını yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Alma bir [tanılama günlüklerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) ve hizmet ayrıntılı yüksek sıklık düzeyindeki ölçümleri toplayın.
* Alma bir [ölçümleri koleksiyonun genel bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) hizmetinizin kullanılabilir ve yanıt verdiğinden emin olmak için.
