---
title: Microsoft Azure Data Box Gateway'e genel bakış | Microsoft Docs
description: Azure’a veri aktarabilmenizi sağlayan bir sanal gereç depolama çözümü olan Azure Data Box Gateway'i açıklar
services: databox
author: alkohli
ms.service: databox
ms.topic: overview
ms.date: 01/18/2019
ms.author: alkohli
ms.openlocfilehash: 9670d67fa1eb79e9e5e8c81726c10cc78767fb74
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435473"
---
# <a name="what-is-azure-data-box-gateway-preview"></a>Azure Data Box Gateway (Önizleme) nedir? 

Azure Data Box Gateway, Azure rahatça veri göndermenize olanak tanıyan bir depolama çözümüdür. Bu makalede Azure Data Box Gateway çözümüne, avantajlarına, önemli özelliklerine ve bu cihazı dağıtabileceğiniz senaryolara genel bir bakış sağlanır. 

Data Box Gateway, sanallaştırılmış ortamınızda veya hiper yöneticinizde sağlanan sanal makineye dayalı bir sanal cihazdır. Sanal cihaz sizde durur ve NFS ile SMB protokollerini kullanarak buna verileri yazarsınız. Ardından cihaz verilerinizi Azure blok blobuna veya Azure Dosyaları'na aktarır. 

> [!IMPORTANT]
> Data Box Gateway, Önizleme aşamasındadır. Bu çözümü dağıtmadan önce lütfen [Önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="use-cases"></a>Uygulama alanları

Bulutta arşivleme, olağanüstü durum kurtarma gibi durumlarda veya verilerinizin bulut ölçeğinde işlenmesi gerektiğinde, verileri buluta aktarmak için Data Box Gateway'den yararlanılabilir. Burada Data Box Gateway'in veri aktarımı için kullanılabileceği çeşitli senaryoları bulabilirsiniz.

- **Bulutta arşivleme** - Data Box Gateway'i kullanarak güvenli ve verimli bir yöntemle yüzlerce terabaytlık veriyi Azure depolamaya kopyalayın. Arşivleme senaryoları için veriler tek seferlik veya sürekli olarak alınabilir.

- **Veri toplama** - Veri işleme ve veri analizi için birden çok kaynaktan gelen verileri Azure Depolama'da tek bir konumda toplayın.  

- **Şirket içi iş yükleriyle tümleştirme** - Bulut depolamasını kullanan ve sık kullanılan dosyalara yerel erişim gerektiren yedekleme ve geri yükleme gibi şirket içi iş yükleriyle tümleştirin. 

## <a name="benefits"></a>Avantajlar

Data Box Gateway'in şöyle avantajları vardır:

- **Kolay veri aktarımı**- Verileri Azure depolamasında içeri ve dışarı taşımayı, yerel ağ paylaşımıyla çalışma kadar kolay hale getirir.  
- **Yüksek performanslı** - Azure'a ve Azure'dan yüksek performanslı aktarımlarla ağda veri taşıma yükünü ortadan kaldırır. 
- **Hızlı erişim ve yüksek veri alımı ücretlerine iş saatlerinde** -veri kutusu ağ geçidi sanal cihaz sağlandığında, yerel kapasite boyutu tanımlayan bir yerel önbellek vardır. Veri disk boyutu olarak başına belirtilmelidir [sanal cihaz en düşük gereksinimler](data-box-gateway-system-requirements.md#specifications-for-the-virtual-device). Yerel önbellek aşağıdaki avantajları sağlar:
    - Yerel önbellek, yüksek hızda veri alımı sağlar. Yüksek veri miktarı en yüksek çalışma saatleri sırasında alınır, önbellek verileri tutmak ve buluta yükleyin.
    - Yerel önbellek, belirli bir eşiği kadar hızlı okuma erişimi verir. Cihaz 50-%60 olana kadar tam, CİHAZDAN tüm okuma Hızlı hale getirecek önbellekten da erişilir. Cihaz üzerindeki kullanılan alanın bu eşiğin üzerinde giden sonra cihaz, yerel dosyaları kaldırmak başlar. 
 
- **Sınırlı bant genişliği kullanımı** - Yoğun iş saatlerinde kullanımı sınırlandırmak amacıyla ağ kısıtlandığında bile veriler Azure'a yazılabilir.  

## <a name="key-capabilities"></a>Temel işlevler

Data Box Gateway'in şöyle özellikleri vardır:

|Özellik |Açıklama  |
|---------|---------|
|Hız     | Tümüyle otomatik ve son derece iyileştirilmiş veri aktarımı ve bant genişliği.|
|Desteklenen protokoller     | Veri alımında standart SMB ve NFS protokolleri için destek. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Data Box Gateway sistem gereksinimleri](data-box-gateway-system-requirements.md).|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi.|
|Hızlı erişim     | En son kullanılan dosyalara hızlı erişim için cihazda yerel önbellek.|
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımının güvenliğini sağlamak için BitLocker desteği       |
|Dayanıklılık     | Yerleşik ağ dayanıklılığı        |


## <a name="specifications"></a>Belirtimler

Data Box Gateway sanal cihazının belirtimleri şöyledir:

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Sanal işlemciler (çekirdekler)   | En az 4 |            
| Bellek  | En az 8 GB|
| Kullanılabilirlik|Tek düğüm|
| Diskler| İşletim sistemi diski: 250 GB <br> Veri diski: 2 TB en, ölçülü kaynak sağlanan ve SSD'ler ile desteklenir|
| Ağ arabirimleri|1 veya daha çok sanal ağ arabirimi|
| Yerel dosya paylaşımı protokolleri|SMB ve NFS  |
| Güvenlik| Cihaza ve verilere erişimin kilidini açmak için kimlik doğrulaması <br> Kullanım halindeki veriler AES-256 bit şifrelemesi kullanılarak şifrelenir|
| Yönetim| Yerel web kullanıcı arabirimi - Cihazın ilk kurulumu, tanılaması ve güç yönetimi <br> Azure portalı - Data Box Gateway cihazlarının gündelik yönetimi       |


## <a name="components"></a>Bileşenler

Data Box Gateway çözümü Data Box Gateway kaynağından, Data Box Gateway sanal cihazından ve yerel bir web kullanıcı arabiriminden oluşur.

* **Data Box Gateway sanal cihazı** - Sanallaştırılmış ortamınızda veya hiper yöneticinizde sağlanan sanal makineye dayalı olan ve Azure'a veri göndermenizi sağlayan cihaz. 
    
* **Data Box Gateway kaynağı** – Azure portalında, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Gateway cihazını yönetmenize olanak tanıyan bir kaynak. Data Box Gateway kaynağını kullanarak kaynakları oluşturun ve yönetin, cihazlarla uyarıları görüntüleyin ve yönetin, paylaşımları yönetin.  

    <!--![The Data Box Gateway service in Azure portal](media/data-box-overview/data-box-Gateway-service1.png)-->

    <!--For more information, go to [Use the Data Box Gateway service to administer your Data Box Gateway device](data-box-gateway-portal-ui-admin.md).-->

* **Data Box yerel web kullanıcı arabirimi** - Tanılama çalıştırmak, Data Box Gateway cihazını kapatmak ve yeniden başlatmak, kopya günlüklerini görüntülemek ve hizmet isteğinde bulunmak üzere Microsoft Desteği'ne başvurmak için yerel web kullanıcı arabirimini kullanın.

    <!--![The Data Box Gateway local web UI](media/data-box-gateway-overview/data-box-gateway-local-web-ui.png)-->

    <!-- For information about using the web-based UI, go to [Use the web-based UI to administer your Data Box](data-box-gateway-portal-ui-admin.md).-->


## <a name="region-availability"></a>Bölge kullanılabilirliği

Data Box Edge fiziksel cihazı, Azure kaynağı ve verileri aktardığınız hedef depolama hesabının üçünün de aynı bölgede bulunması gerekmez.

- **Kaynak kullanılabilirliği** - Bu sürümde, Data Box Edge kaynağı şu bölgelerde kullanılabilir:
    - **Birleşik Devletler** -Batı ABD2 ve Doğu ABD
    - **Avrupa Birliği** - Batı Avrupa
    - **Asya Pasifik** - Güneydoğu Asya

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. 

    En iyi performansı elde etmek için, depolama hesaplarının Data Box verilerini depoladığı bölgeler cihazın bulunduğu yere yakın konumlandırılmalıdır. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar. 


## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Gateway sistem gereksinimlerini](data-box-gateway-system-requirements.md) gözden geçirin.
- [Data Box Gateway sınırlarını](data-box-gateway-limits.md) anlayın.
- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.




