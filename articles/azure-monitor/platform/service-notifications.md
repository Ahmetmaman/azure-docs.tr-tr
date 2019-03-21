---
title: Azure hizmet durumu bildirimlerine nelerdir?
description: Hizmet durumu bildirimlerine, Microsoft Azure tarafından yayınlanan hizmet sistem durumu iletileri görüntülemenize olanak sağlar.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2018
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: ab4aa2f37d5a883c83b8ee09b5de6551ebf13d2e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57995144"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme

Hizmet durumu bildirimlerine, Azure tarafından yayımlanır ve aboneliğiniz kapsamındaki kaynaklar hakkında bilgiler içerir. Bu bildirimleri bir alt sınıfı etkinlik günlüğü olaylardır ve etkinlik günlüğü'nde bulunabilir. Hizmet durumu bildirimlerine bilgilendirici veya sınıf bağlı olarak eyleme dönüştürülebilir olabilir.

Hizmet durumu bildirimlerine çeşitli sınıfı vardır:  

- **Eylem gerekiyor:** Azure, olağan dışı bir şey hesabınızdaki gerçekleşir ve bu sorunu gidermek için sizinle çalışma görebilirsiniz. Azure, gerçekleştirmeniz gereken eylemler veya nasıl Azure mühendislik veya desteğe başvurun ya da gerçekleşen bir bildirim gönderir.  
- **Yardımlı kurtarma:** Bir olayın oluştuğunu ve mühendislerin etkisi hala yaşayan onayladıktan. Azure mühendislik doğrudan tam sağlık hizmetlerinizi geri yüklemek için birlikte çalışması gerekir.  
- **Olay:** Hizmeti etkileyen bir olay şu anda bir veya daha fazla, aboneliğinizdeki kaynaklar etkiliyor.  
- **Bakım:** Bir veya daha fazla aboneliğiniz kapsamındaki kaynaklar etkileyebilecek planlanan bakım etkinliği.  
- **Bilgi:** Olası en iyi duruma getirme kaynağınızı geliştirebilecek kullanın. 
- **Güvenlik:** Acil güvenlik ilgili bilgiler çözümlerinizin Azure üzerinde çalıştırın.

Her hizmet durumu bildirimi kapsamını ve etkisini kaynaklarınıza hakkında ayrıntılı bilgi içerir. Ayrıntıları içerir:

Özellik adı | Açıklama
-------- | -----------
kanallar | Aşağıdaki değerlerden biri: **Yönetici** veya **işlemi**.
correlationId | Genellikle bir GUID dize biçiminde. Genellikle aynı eyleme ait olayları aynı Correlationıd paylaşın.
eventDataId | Olayın benzersiz tanımlayıcısı.
EventName | Olay başlığı.
düzey | Bir olay düzeyi
resourceProviderName | Etkilenen kaynak bir kaynak sağlayıcısının adı.
Kaynak türü| Etkilenen kaynak bir kaynağın türü.
alt durumu | Genellikle karşılık gelen REST, HTTP durum kodu diyebilirsiniz, ama alt açıklayan diğer dizeleri de içerebilir. Örneğin: Tamam (HTTP durum kodu: 200) oluşturuldu (HTTP durum kodu: 201) kabul edildi (HTTP durum kodu: 202), içerik yok (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400) bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503) ve ağ geçidi zaman aşımı (HTTP durum kodu: 504).
eventTimestamp | Olaya karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda, zaman damgası.
submissionTimestamp | Olay sorgulamak için kullanılabilen kalktığında zaman damgası.
subscriptionId | Bu olay günlüğe Azure aboneliği.
durum | İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır: **Başlatılan**, **sürüyor**, **başarılı**, **başarısız**, **etkin**, ve **Çözümlendi**.
operationName | İşlemin adı.
category | Bu özellik her zaman, **ServiceHealth**.
resourceId | Etkilenen kaynak kaynak kimliği.
Properties.Title | Bu iletişim için yerelleştirilmiş başlığı. İngilizce varsayılandır.
Properties.communication | HTML biçimlendirmesi ile iletişimi yerelleştirilmiş ayrıntıları. İngilizce varsayılandır.
Properties.incidentType | Aşağıdaki değerlerden biri: **Eylem gereklidir**, **bilgilendirici**, **olay**, **Bakım**, veya **güvenlik**.
Properties.trackingId | Bu olay ile ilişkili olduğu olay. Bir olayla ilgili olayları ilişkilendirmek için bunu kullanın.
Properties.impactedServices | Hizmetleri ve bölgeleri olaydan etkilenen açıklar JSON kaçan bir blob. Her biri, hizmetlerin bir listesini özelliği içeren bir **ServiceName**ve her biri etkilenen bölgelerin bir listesini bir **RegionName**.
Properties.defaultLanguageTitle | İngilizce dilinde iletişim.
Properties.defaultLanguageContent | HTML biçimlendirmesi veya düz metin olarak İngilizce dilinde iletişim.
Properties.Stage | Olası değerler için **olay**, ve **güvenlik** olan **etkin** **çözümlenmiş** veya **RCA**. İçin **eylem gereklidir** veya **bilgilendirici** tek değerdir **etkin.** İçin **Bakım** bunlar: **Etkin**, **planlı**, **Inprogress**, **iptal**, **filtrelerden**, **çözümlenen**, veya **tam**.
Properties.communicationId | Bu olay ile ilişkili olduğu iletişimi.

### <a name="details-on-service-health-level-information"></a>Hizmet durumu düzeyi bilgileri hakkında ayrıntılı bilgi

**Eylem gerekiyor** (properties.incidentType == eylem gereklidir)
- Bilgilendirici - mevcut hizmetlere etkiyi önlemek için gereken yönetici eylemi
    
**Bakım** (properties.incidentType bakım ==)
- Uyarı - Acil bakım
- Bilgilendirici - standart planlı bakım

**Information** (properties.incidentType == Information)
- Bilgi - yönetici, mevcut hizmetlere etkiyi önlemek için gerekebilir

**Güvenlik** (properties.incidentType güvenlik ==)
- Hata - yaygın sorunları birden çok bölgede birden fazla hizmetlere erişen müşteriler çok geniş bir yelpazedeki etkileyen.
- Uyarı - sorunları erişimi belirli hizmetlere ve/veya müşteri alt kümesini etkileyen özel bölgeler.
- Hizmet kullanılabilirliği etkileyen değil bilgilendirici - yönetim işlemleri ve/veya gecikme süresi, etkileyen sorunları.

**Hizmet sorunları** (properties.incidentType olay ==)
- Hata - yaygın sorunları birden çok bölgede birden fazla hizmetlere erişen müşteriler çok geniş bir yelpazedeki etkileyen.
- Uyarı - sorunları erişimi belirli hizmetlere ve/veya müşteri alt kümesini etkileyen özel bölgeler.
- Hizmet kullanılabilirliği etkileyen değil bilgilendirici - yönetim işlemleri ve/veya gecikme süresi, etkileyen sorunları.


## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Azure portalında, hizmet durumu bildirimlerini görüntüleme
1.  İçinde [Azure portalında](https://portal.azure.com)seçin **İzleyici**.

    ![Ekran görüntüsü, Azure portal menüsünde, seçilen İzleyici](./media/service-notifications/home-monitor.png)

    Azure İzleyici, izleme ayarlarınızı ve verilerinizi tek bir birleştirilmiş görünümde getirmektedir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

3.  Seçin **uyarılar**.

    ![Seçili uyarıları ile izleme ekran etkinlik günlüğü](./media/service-notifications/service-health-summary.png)
4. Seçin **+ etkinlik günlüğü uyarısı Ekle**, gelecek hizmet bildirimleri için bildirim emin olmak için bir alarm ayarlayın. Daha fazla bilgi için [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Sonraki adımlar
Alma [uyarı bildirimleri hizmet durumu bildirimi her](../../azure-monitor/platform/alerts-activity-log-service-notifications.md) gönderilir.  
Daha fazla bilgi edinin [etkinlik günlüğü uyarıları](../../azure-monitor/platform/activity-log-alerts.md).

