---
title: Microsoft Azure 'de veri şifreleme modelleri
description: Bu makalede Microsoft Azure Içindeki veri şifreleme modellerine genel bir bakış sunulmaktadır.
services: security
documentationcenter: na
author: msmbaldwin
manager: rkarlin
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mbaldwin
ms.openlocfilehash: 836e01d3cd8fb25dda1616803d8b6f3e9ff4e06f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89645743"
---
# <a name="data-encryption-models"></a>Veri şifreleme modelleri

Çeşitli şifreleme modellerinin ve bunların uzmanlarının ve dezavantajlarını anlamak, Azure 'daki çeşitli kaynak sağlayıcılarının bekleyen şifreleme uygulamasını anlamak için gereklidir. Bu tanımlar, ortak dil ve Taksonomi sağlamak için Azure 'daki tüm kaynak sağlayıcıları genelinde paylaşılır.

Sunucu tarafı şifreleme için üç senaryo vardır:

- Service-Managed anahtarları kullanarak sunucu tarafı şifreleme
  - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirir
  - Microsoft, anahtarları yönetir
  - Tam bulut işlevselliği

- Azure Key Vault 'de müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme
  - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirir
  - Müşteri denetimleri Azure Key Vault aracılığıyla anahtarları
  - Tam bulut işlevselliği

- Müşteri tarafından denetlenen donanımda müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme
  - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirir
  - Müşteri tarafından denetlenen donanımda müşteriler denetim anahtarları
  - Tam bulut işlevselliği

Sunucu tarafı şifreleme modelleri, Azure hizmeti tarafından gerçekleştirilen şifrelemeye başvurur. Bu modelde, kaynak sağlayıcısı şifreleme ve şifre çözme işlemlerini gerçekleştirir. Örneğin, Azure Storage verileri düz metin işlemlerinde alabilir ve şifreleme ve şifre çözme işlemlerini dahili olarak gerçekleştirir. Kaynak sağlayıcı, belirtilen yapılandırmaya bağlı olarak Microsoft tarafından veya müşteri tarafından yönetilen şifreleme anahtarlarını kullanabilir.

![Sunucu](./media/encryption-models/azure-security-encryption-atrest-fig3.png)

Rest modellerindeki sunucu tarafı şifrelemenin her biri, anahtar yönetiminin farklı özelliklerini gösterir. Bu, şifreleme anahtarlarının nerede ve nasıl oluşturulduğunu, ayrıca erişim modellerini ve anahtar döndürme yordamlarını da içerir.  

İstemci tarafı şifreleme için aşağıdakileri göz önünde bulundurun:

- Azure hizmetleri şifresi çözülen verileri göremez
- Müşteriler, anahtarları şirket içinde (veya diğer güvenli depolarda) yönetebilir ve depolar. Anahtarlar Azure hizmetleri için kullanılamaz
- Azaltılmış bulut işlevselliği

Azure 'daki desteklenen şifreleme modelleri, daha önce bahsedilen "Istemci şifrelemesi" ve "sunucu tarafı şifreleme" olarak iki ana gruba ayrılır. Kullanılan Rest modelindeki şifrelemeden bağımsız olarak Azure Hizmetleri, TLS veya HTTPS gibi güvenli bir taşımanın kullanımını her zaman önerir. Bu nedenle, Aktarımdaki şifrelemeye, Aktarım Protokolü tarafından değinilmesi gerekir ve bekleyen modeldeki şifrelemeyi belirlemede büyük bir faktör olmamalıdır.

## <a name="client-encryption-model"></a>İstemci şifreleme modeli

İstemci şifreleme modeli, kaynak sağlayıcısı veya hizmet tarafından Azure veya çağıran uygulama dışında gerçekleştirilen şifrelemeyi ifade eder. Şifreleme, Azure 'daki hizmet uygulaması veya müşteri veri merkezinde çalışan bir uygulama tarafından gerçekleştirilebilir. Her iki durumda da, bu şifreleme modeli kullanılırken Azure Kaynak sağlayıcısı, verilerin şifresini herhangi bir şekilde çözebilme veya şifreleme anahtarlarına erişim olmadan şifrelenmiş bir veri blobu alır. Bu modelde, anahtar yönetimi çağıran hizmet/uygulama tarafından yapılır ve Azure hizmetine opaktır.

![İstemci](./media/encryption-models/azure-security-encryption-atrest-fig2.png)

## <a name="server-side-encryption-using-service-managed-keys"></a>Hizmet tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme

Birçok müşteri için, en önemli gereksinim, verilerin bekleyen her durumda şifrelendiğinden emin olunması sağlamaktır. Hizmet tarafından yönetilen anahtarları kullanan sunucu tarafı şifreleme, müşterilerin belirli bir kaynağı (depolama hesabı, SQL DB vb.) şifreleme için işaretlemesini ve anahtar verme, döndürme ve yedekleme gibi tüm anahtar yönetim yönlerini Microsoft 'a bırakarak bu modeli sağlar. Rest 'de şifrelemeyi destekleyen çoğu Azure hizmeti, şifreleme anahtarlarının yönetimini Azure 'a boşaltma için genellikle bu modeli destekler. Azure Kaynak sağlayıcısı anahtarları oluşturur, güvenli depolamaya koyar ve gerektiğinde bunları alır. Bu, hizmetin anahtarlara tam erişimi olduğu ve hizmetin kimlik bilgisi yaşam döngüsü yönetimi üzerinde tam denetime sahip olduğu anlamına gelir.

![lebilmesi](./media/encryption-models/azure-security-encryption-atrest-fig4.png)

Hizmet tarafından yönetilen anahtarlar kullanılarak yapılan sunucu tarafı şifreleme, bu nedenle, müşteri için düşük ek yük ile bekleyen şifreleme gereksinimini hızla ortadan kaldırır. Bir müşteri kullanılabilir olduğunda, genellikle hedef abonelik ve kaynak sağlayıcısı için Azure portal açar ve bunu gösteren bir kutuyu kontrol eder ve verilerin şifrelenmesini ister. Bazı kaynak yöneticileri, hizmet tarafından yönetilen anahtarlarla sunucu tarafında şifreleme varsayılan olarak açık olur.

Microsoft tarafından yönetilen anahtarlarla sunucu tarafı şifreleme, hizmetin anahtarları depolamak ve yönetmek için tam erişimi olduğunu göstermez. Bazı müşteriler daha fazla güvenlik elde ettikleri için anahtarları yönetmek isteyebilir, bu modelin değerlendirmesi sırasında özel bir anahtar depolama çözümüyle ilişkili maliyet ve risk göz önünde bulundurulmalıdır. Çoğu durumda, bir kuruluş, şirket içi bir çözümün kaynak kısıtlamalarının veya risklerin, Rest anahtarlarındaki şifrelemenin bulut yönetimi riskinden daha büyük olabileceğini tespit edebilir.  Ancak, bu model, şifreleme anahtarlarının oluşturulmasını veya yaşam döngüsünü denetlemek veya farklı personele hizmetin şifreleme anahtarlarını (hizmet için genel yönetim modelinden anahtar yönetimi) yönetmekten farklı bir şekilde yönetmesi için yeterli olmayabilir.

### <a name="key-access"></a>Anahtar erişimi

Hizmet tarafından yönetilen anahtarlarla sunucu tarafı şifreleme kullanıldığında, anahtar oluşturma, depolama ve hizmet erişiminin hepsi hizmet tarafından yönetilir. Genellikle, temel Azure kaynak sağlayıcıları veri şifreleme anahtarlarını verilere yakın bir depoda depolar ve anahtar şifreleme anahtarları güvenli bir dahili depoda depolanırken hızlı bir şekilde kullanılabilir ve erişilebilir olur.

**Avantajlar**

- Basit kurulum
- Microsoft, anahtar döndürme, yedekleme ve artıklığı yönetir
- Müşterinin, uygulamayla ilişkili maliyeti veya özel bir anahtar yönetim şeması riski yoktur.

**Dezavantajlar**

- Şifreleme anahtarları üzerinde müşteri denetimi yok (anahtar belirtimi, yaşam döngüsü, iptal, vb.)
- Hizmet için genel yönetim modelinden anahtar yönetimini ayırma yeteneği yoktur

## <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Azure Key Vault 'de müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme

Gereksinimin, bekleyen verileri şifrelemek ve şifreleme anahtarlarını denetlemek için gereken senaryolarda, müşteriler Key Vault içindeki müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifrelemeyi kullanabilir. Bazı hizmetler Azure Key Vault yalnızca kök anahtar şifreleme anahtarını saklayabilir ve şifreli veri şifreleme anahtarını verilere daha yakın bir iç konuma saklayabilir. Bu senaryoda müşteriler, kendi anahtarlarını Key Vault (BYOK – Kendi Anahtarını Getir) öğesine getirebilir ya da yenilerini oluşturabilir ve bunları istediğiniz kaynakları şifrelemek için kullanabilir. Kaynak sağlayıcısı şifreleme ve şifre çözme işlemlerini gerçekleştirdiğinde, yapılandırılan anahtar şifreleme anahtarını tüm şifreleme işlemleri için kök anahtar olarak kullanır.

Anahtar şifreleme anahtarlarının kaybolması, veri kaybı anlamına gelir. Bu nedenle anahtarlar silinmemelidir. Anahtarlar her oluşturulduğunda veya döndürüldüğünde yedeklenmelidir. Anahtar şifreleme anahtarlarını depolayan kasalardaki [geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) etkinleştirilmelidir. Anahtar silmek yerine, etkin ayarını false olarak ayarlayın veya bitiş tarihini ayarlayın.

### <a name="key-access"></a>Anahtar erişimi

Azure Key Vault içindeki müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme modeli, gerektiğinde şifrelemek ve şifresini çözmek için anahtarlara erişen hizmeti içerir. Rest anahtarlarındaki şifrelemeye, bir erişim denetim ilkesi aracılığıyla bir hizmet tarafından erişilebilir hale getirilir. Bu ilke, anahtarı almak için hizmet kimliği erişimine izin verir. İlişkili bir abonelik adına çalışan bir Azure hizmeti, Bu abonelikteki bir kimlikle yapılandırılabilir. Hizmet Azure Active Directory kimlik doğrulaması gerçekleştirebilir ve bu hizmet, abonelik adına işlem gören bir kimlik doğrulama belirteci alabilir. Bu belirteç daha sonra, erişimi verilen bir anahtarı elde etmek için Key Vault sunulabilir.

Şifreleme anahtarlarını kullanan işlemler için, bir hizmet kimliğine şu işlemlerden herhangi birine erişim verilebilir: şifre çözme, şifreleme, unwrapKey, wrapKey, doğrulama, imzalama, alma, listeleme, güncelleştirme, oluşturma, içeri aktarma, silme, yedekleme ve geri yükleme.

Bekleyen verileri şifrelemek veya şifrelerini çözmek için bir anahtar almak üzere Kaynak Yöneticisi hizmet örneğinin çalışacağı hizmet kimliğinin UnwrapKey olması gerekir (şifre çözme anahtarını almak için) ve WrapKey (yeni anahtar oluştururken anahtar kasasına bir anahtar eklemek için).

>[!NOTE]
>Key Vault yetkilendirmesi hakkında daha fazla ayrıntı için, [Azure Key Vault belgelerindeki](../../key-vault/general/secure-your-key-vault.md)anahtar kasasının güvenliğini sağlama sayfasına bakın.

**Avantajlar**

- Kullanılan anahtarlar üzerinde tam denetim – şifreleme anahtarları, müşterinin denetiminde müşterinin Key Vault yönetilir.
- Birden çok hizmeti tek bir ana öğe ile şifreleme yeteneği
- Hizmet için genel yönetim modelinden anahtar yönetimini ayırt edebilir
- , Bölgeler arasında hizmet ve anahtar konumunu tanımlayabilir

**Dezavantajlar**

- Müşteri, anahtar erişim yönetimi için tam sorumluluğa sahiptir
- Müşteri, anahtar yaşam döngüsü yönetimi için tam sorumluluğa sahiptir
- Ek kurulum & yapılandırma ek yükü

## <a name="server-side-encryption-using-customer-managed-keys-in-customer-controlled-hardware"></a>Müşteri tarafından denetlenen donanımda müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme

Bazı Azure Hizmetleri, ana bilgisayarı kendi anahtarınızı (HYOK) anahtar yönetim modelinize etkinleştirir. Bu yönetim modu, bekleyen verileri şifrelemek ve Microsoft 'un denetimi dışında özel bir depodaki anahtarları yönetmek için gereken senaryolarda faydalıdır. Bu modelde, hizmetin anahtarı bir dış siteden alması gerekir. Performans ve kullanılabilirlik garantisi etkilendi ve yapılandırma daha karmaşıktır. Ayrıca, şifreleme ve şifre çözme işlemleri sırasında hizmetin DEK erişimi olduğundan, bu modelin genel güvenlik garantisi, anahtarların Azure Key Vault ' de müşteri tarafından yönetilme biçimine benzer.  Sonuç olarak, belirli anahtar yönetim gereksinimlerine sahip olmadıkları sürece bu model çoğu kuruluşa uygun değildir. Bu sınırlamalar nedeniyle, çoğu Azure hizmeti, müşteri denetimli donanımda sunucu tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemeyi desteklemez.

### <a name="key-access"></a>Anahtar erişimi

Müşteri denetimli donanımda hizmet tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifreleme kullanıldığında, anahtarlar müşteri tarafından yapılandırılan bir sistemde tutulur. Bu modeli destekleyen Azure Hizmetleri, müşterinin sağladığı anahtar deposuyla güvenli bir bağlantı kurmak için bir yol sağlar.

**Avantajlar**

- Kullanılan kök anahtar üzerinde tam denetim – şifreleme anahtarları müşterinin sunduğu bir mağaza tarafından yönetilir
- Birden çok hizmeti tek bir ana öğe ile şifreleme yeteneği
- Hizmet için genel yönetim modelinden anahtar yönetimini ayırt edebilir
- , Bölgeler arasında hizmet ve anahtar konumunu tanımlayabilir

**Dezavantajlar**

- Anahtar depolama, güvenlik, performans ve kullanılabilirlik için tam sorumluluk
- Anahtar erişim yönetimi için tam sorumluluk
- Anahtar yaşam döngüsü yönetimi için tam sorumluluk
- Önemli kurulum, yapılandırma ve devam eden bakım maliyetleri
- Müşteri veri merkezi ve Azure veri merkezleri arasında ağ kullanılabilirliğine yönelik daha fazla bağımlılık.

## <a name="supporting-services"></a>Destekleyici hizmetler
Her şifreleme modelini destekleyen Azure hizmetleri:

| Ürün, özellik veya hizmet | Service-Managed anahtarı kullanarak Server-Side   | Customer-Managed anahtarı kullanarak Server-Side | Client-Managed anahtarı kullanarak Client-Side  |
|----------------------------------|--------------------|-----------------------------------------|--------------------|
| **Yapay Zeka ve Makine Öğrenmesi**      |                    |                    |                    |
| Azure Bilişsel Arama           | Evet                | Evet                | -                  |
| Azure Bilişsel Hizmetler         | Evet                | Evet                | -                  |
| Azure Machine Learning           | Evet                | Evet                | -                  |
| Azure Machine Learning Studio    | Evet                | Önizleme, RSA 2048 bit | -               |
| Content Moderator                | Evet                | Evet                | -                  |
| Yüz                             | Evet                | Evet                | -                  |
| Language Understanding           | Evet                | Evet                | -                  |
| Kişiselleştirme                     | Evet                | Evet                | -                  |
| Soru-Cevap Oluşturucu                        | Evet                | Evet                | -                  |
| Konuşma Hizmetleri                  | Evet                | Evet                | -                  |
| Translator Metin Çevirisi                  | Evet                | Evet                | -                  |
| Power BI                         | Evet                | Evet, RSA 4096 bit  | -                  |
| **Analiz**                    |                    |                    |                    |
| Azure Stream Analytics           | Evet                | yok\*              | -                  |
| Event Hubs                       | Evet                | Evet                | -                  |
| İşlevler                        | Evet                | Evet                | -                  |
| Azure Analysis Services          | Evet                | -                  | -                  |
| Azure Veri Kataloğu               | Evet                | -                  | -                  |
| Azure HDInsight                  | Evet                | Tümü                | -                  |
| Azure Izleyici Application Insights | Evet                | Evet                | -                  |
| Azure Izleyici Log Analytics      | Evet                | Evet                | -                  |
| Azure Veri Gezgini              | Evet                | Evet                | -                  |
| Azure Data Factory               | Evet                | Evet                | -                  |
| Azure Data Lake Store            | Evet                | Evet, RSA 2048 bit  | -                  |
| **Kapsayıcılar**                   |                    |                    |                    |
| Azure Kubernetes Service         | Evet                | Evet                | -                  |
| Container Instances              | Evet                | Evet                | -                  |
| Container Kayıt Defteri               | Evet                | Evet                | -                  |
| **İşlem**                      |                    |                    |                    |
| Sanal Makineler                 | Evet                | Evet                | -                  |
| Sanal makine ölçek kümesi        | Evet                | Evet                | -                  |
| SAP HANA                         | Evet                | Evet                | -                  |
| App Service                      | Evet                | Yes\*\*            | -                  |
| Otomasyon                       | Evet                | Yes\*\*            | -                  |
| Azure İşlevleri                  | Evet                | Yes\*\*            | -                  |
| Azure portal                     | Evet                | Yes\*\*            | -                  |
| Logic Apps                       | Evet                | Evet                | -                  |
| Azure tarafından yönetilen uygulamalar       | Evet                | Yes\*\*            | -                  |
| Service Bus                      | Evet                | Evet                | -                  |
| Site Recovery                    | Evet                | Evet                | -                  |
| **Veritabanları**                    |                    |                    |                    |
| Sanal Makinelerde SQL Server   | Evet                | Evet                | Evet                |
| Azure SQL Veritabanı               | Evet                | Evet, RSA 3072 bit  | Evet                |
| MariaDB için Azure SQL veritabanı   | Evet                | -                  | -                  |
| MySQL için Azure SQL veritabanı     | Evet                | Evet                | -                  |
| PostgreSQL için Azure SQL veritabanı | Evet               | Evet                | -                  |
| Azure Synapse Analytics          | Evet                | Evet, RSA 3072 bit  | -                  |
| SQL Server Stretch Database      | Evet                | Evet, RSA 3072 bit  | Evet                |
| Tablo Depolama                    | Evet                | Evet                | Evet                |
| Azure Cosmos DB                  | Evet                | Evet                | -                  |
| Azure Databricks                 | Evet                | Evet                | -                  |
| Azure Veritabanı Geçiş Hizmeti | Evet                | yok\*              | -                  |
| **DevOps**                       |                    |                    |                    |
| Azure DevOps Services            | Evet                | -                  | Evet                |
| Azure Repos                      | Evet                | -                  | Evet                |
| **Kimlik**                     |                    |                    |                    |
| Azure Active Directory           | Evet                | -                  | -                  |
| Azure Active Directory Domain Services | Evet          | Evet                | -                  |
| **Tümleştirme**                  |                    |                    |                    |
| Service Bus                      | Evet                | Evet                | Evet                |
| Event Grid                       | Evet                | -                  | -                  |
| API Management                   | Evet                | -                  | -                  |
| **IoT Hizmetleri**                 |                    |                    |                    |
| IoT Hub                          | Evet                | Evet                | Evet                |
| Cihaz sağlamayı IoT Hub      | Evet                | Evet                | -                  |
| **Yönetim ve İdare**    |                    |                    |                    |
| Azure Site Recovery              | Evet                | -                  | -                  |
| Azure Geçişi                    | Evet                | Evet                | -                  |
| **Medya**                        |                    |                    |                    |
| Media Services                   | Evet                | -                  | Evet                |
| **Güvenlik**                     |                    |                    |                    |
| IoT için Azure Güvenlik Merkezi    | Evet                | Evet                | -                  |
| Azure Sentinel                   | Evet                | Evet                | -                  |
| **Depolama**                      |                    |                    |                    |
| Blob Depolama                     | Yes                | Evet                | Evet                |
| Premium blob depolaması             | Evet                | Evet                | Evet                |
| Disk Depolama                     | Evet                | Evet                | -                  |
| Ultra Disk Depolama               | Evet                | Evet                | -                  |
| Yönetilen Disk Depolama             | Evet                | Evet                | -                  |
| Dosya Depolama                     | Yes                | Evet                | -                  |
| Dosya Premium depolaması             | Evet                | Evet                | -                  |
| Dosya Eşitleme                        | Evet                | Evet                | -                  |
| Kuyruk Depolama                    | Evet                | Evet                | Evet                |
| Avere vFXT                       | Evet                | -                  | -                  |
| Redis için Azure Önbelleği            | Evet                | yok\*              | -                  |
| Azure NetApp Files               | Evet                | Evet                | -                  |
| Arşiv Depolama                  | Evet                | Evet                | -                  |
| StorSimple                       | Evet                | Evet                | Evet                |
| Azure Backup                     | Evet                | Evet                | Evet                |
| Data Box                         | Evet                | -                  | Evet                |
| Data Box Edge                    | Evet                | Evet                | -                  |

\* Bu hizmet, verileri kalıcı olarak tutmaz. Varsa geçici önbellekler bir Microsoft anahtarıyla şifrelenir.

\*\* Bu hizmet, Customer-Managed anahtarla Server-Side şifrelemeyi zaten destekleyen, kendi Key Vault, depolama hesabınızda veya diğer veri kalıcı hizmette veri depolamayı destekler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da şifrelemenin nasıl kullanıldığını](encryption-overview.md)öğrenin.
- Azure 'un, verileri şifreleyen tehditleri azaltmak için [çift şifrelemeyi](double-encryption.md) nasıl kullandığını öğrenin.
