---
title: ISO 27001 ATıCı/SQL iş yükü şeması örneği dağıtma
description: ISO 27001 App Service ortamı/SQL veritabanı iş yükü şeması 'nın, şema yapıt parametresi ayrıntıları dahil olmak üzere adımlarını dağıtın.
ms.date: 07/13/2020
ms.topic: sample
ms.openlocfilehash: 74f2670b79d1968755e376d1f5a75bbb76e6e6c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87072888"
---
# <a name="deploy-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması örneğini dağıtma

Azure şemaları ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması ' nı dağıtmak için aşağıdaki adımlar gerçekleştirilmelidir:

> [!div class="checklist"]
> - [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneğini dağıtma
> - Örnekten yeni bir şema oluşturma
> - Örneğinizin kopyasını **Yayımlandı** olarak işaretleyin
> - Şemanızın kopyasını mevcut bir aboneliğe atayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 paylaşılan hizmetleri şema örneğini dağıtma

Bu şema örneği dağıtılmadan önce [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneğinin hedef aboneliğe dağıtılması gerekir. ISO 27001 paylaşılan hizmetleri şema örneğinin başarılı bir şekilde dağıtılması olmadan, bu şema örneğinde altyapı bağımlılıkları eksik olur ve dağıtım sırasında başarısız olur.

> [!IMPORTANT]
> Bu şema örneği [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneği ile aynı abonelikte atanmalıdır.

## <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, başlangıç noktası olarak örneği kullanarak ortamınızda yeni bir şema oluşturup şema örneğini uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemalar**’ı arayıp seçin.

1. Soldaki **Başlangıç** sayfasında, _Şema oluştur_ bölümündeki **Oluştur** düğmesini seçin.

1. _Diğer örnekler_ altında **ISO 27001: Ao/SQL iş yükü** şeması örneğini bulun ve **Bu örneği kullan**' ı seçin.

1. Şema örneğinin _Temel bilgiler_ bölümüne girin:

   - **Şema adı**: ISO 27001 Ao/SQL iş yükü şeması örneğinin kopyasına bir ad verin.
   - **Tanım konumu**: üç noktayı kullanın ve örneğin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Yapıtlar_ sekmesini seçin veya sayfanın en altındaki **Sonraki: Yapıtlar** seçeneğini belirleyin.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğu parametreler içerir; bu parametreleri daha sonra tanımlayacağız. Şema örneğini gözden geçirmeyi tamamladığınızda **Taslağı Kaydet**’i seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımlama

Şimdi şema örneği kopyanız, ortamınızda oluşturulmuştur. Bu, **Taslak** modunda oluşturulur ve atanabilmesi ve dağıtılabilmesi için önce **Yayımlandı** durumunda olmalıdır. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik bu değişikliği ISO 27001 standbundan uzağa taşıyabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemalar**’ı arayıp seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Filtreleri kullanarak şema örneği kopyanızı bulup seçin.

1. Sayfanın üst kısmında **Şemayı yayımla**’yı seçin. Sağdaki yeni sayfada, şema örneği kopyanız için bir **Sürüm** sağlayın. Bu özellik daha sonra bir değişiklik yaptığınızda faydalı olacaktır. "ISO 27001 şema örneğinden yayımlanan ilk sürüm" gibi **değişiklik notları** sağlayın. Sayfanın alt kısmında **Yayımla** düğmesini seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adımda, şema örneği kopyasının her bir dağıtımını benzersiz hale getirmek için parametreler sağlanır.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemalar**’ı arayıp seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Filtreleri kullanarak şema örneği kopyanızı bulup seçin.

1. Şema tanımı sayfasının en üstünde **Şema ata**’yı seçin.

1. Şema ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: şema örneğinin kopyasını kaydettiğiniz yönetim grubundaki bir veya daha fazla abonelik seçin. Birden fazla abonelik seçerseniz, girilen parametreler kullanılarak her biri için bir atama oluşturulur.
     - **Atama adı**: ad, BLUEPRINT adına göre önceden doldurulur.
       Gerektiğinde değiştirin veya olduğu gibi bırakın.
     - **Konum**: yönetilen kimliğin oluşturulacağı bölgeyi seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - Şema **tanımı sürümü**: şema örneğinin kopyasının **yayınlanmış** bir sürümünü seçin.

   - Kilit ataması

     Ortamınız için şema kilitleme ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistem tarafından _atanmış_ yönetilen kimlik seçeneğini bırakın.

   - Şema parametreleri

     Bu bölümde tanımlanan parametreler, tutarlılık sağlamak için şemadaki yapıtların birçoğu tarafından kullanılır.

     - **Kuruluş adı**: kuruluşunuz için kısa bir ad girin. Bu özellik birincil olarak adlandırma kaynakları için kullanılır.
     - **Paylaşılan hizmet aboneliği kimliği**: [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneğinin atandığı abonelik kimliği.
     - **Varsayılan alt ağ adresi ön eki**: sanal ağ varsayılan alt ağının CIDR Gösterimi.
       Varsayılan değer _10.1.0.0/16_' dır.
     - **Iş yükü konumu**: yapıtların hangi konuma dağıtıldığını belirler. Tüm hizmetler tüm konumlarda kullanılabilir değildir. Bu tür hizmetleri dağıtan yapıtlar, bu yapıtın dağıtılacağı konum için bir parametre seçeneği sağlar.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametreler, tanımlandığı yapıt için geçerlidir. Bu parametreler, Blueprint atama sırasında tanımlandıklarından [dinamik parametrelerdir](../../concepts/parameters.md#dynamic-parameters) . Tam liste veya yapıt parametreleri ve açıklamaları için bkz. [yapıt parametreleri tablosu](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra, sayfanın alt kısmında **Ata** seçeneğini belirleyin. Şema ataması oluşturulur ve yapıt dağıtımı başlar. Dağıtım kabaca bir saat sürer. Dağıtımın durumunu denetlemek için şema atamasını açın.

> [!WARNING]
> Azure Blueprints hizmeti ve yerleşik şema örnekleri **ücretsizdir**. Azure kaynakları, [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Bu şema örneği tarafından dağıtılan kaynakları çalıştırma maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri tablosu

Aşağıdaki tabloda, şema yapıtı parametrelerinin bir listesi verilmiştir:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|Log Analytics kaynak grubu|Kaynak grubu|Adı|**Kilitli** - **Organization name** `-workload-log-rg` kaynak grubunu benzersiz hale getirmek için kuruluş adını ile birleştirir.|
|Log Analytics kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Log Analytics şablonu|Resource Manager şablonu|Hizmet katmanı|Log Analytics çalışma alanının katmanını ayarlar. Varsayılan değer _PerNode_değeridir.|
|Log Analytics şablonu|Resource Manager şablonu|Günlük tutma gün sayısı|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Log Analytics şablonu|Resource Manager şablonu|Konum|Log Analytics çalışma alanını oluşturmak için kullanılan bölge. Varsayılan değer _Batı ABD 2_.|
|Ağ kaynak grubu|Kaynak grubu|Adı|**Kilitli** - **Organization name** `-workload-net-rg` kaynak grubunu benzersiz hale getirmek için kuruluş adını ile birleştirir.|
|Ağ kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Ağ güvenlik grubu şablonu|Resource Manager şablonu|Günlük tutma gün sayısı|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Azure Güvenlik Duvarı özel IP 'si|[Azure Güvenlik duvarının](../../../../firewall/overview.md)özel IP 'sini yapılandırır. _Iso 27001: paylaşılan hizmetler_ yapıt parametresi **Azure Güvenlik Duvarı alt ağ adresi ön eki**' nde tanımlanan CIDR gösteriminin bir parçası olmalıdır. Varsayılan değer _10.0.4.4_' dir.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Paylaşılan hizmetler abonelik KIMLIĞI|Bir Iş yükü ve paylaşılan hizmetler arasında VNET eşlemesini etkinleştirmek için kullanılan değer.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Sanal ağ adresi ön eki|Sanal ağın CıDR gösterimi. Varsayılan değer _10.1.0.0/16_' dır.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Varsayılan alt ağ adresi ön eki|Sanal ağ varsayılan alt ağının CıDR gösterimi. Varsayılan değer _10.1.0.0/16_' dır.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|IP adresi ekler|İlk VM 'yi ekleyen IP adresi. Bu değer özel VNET DNS olarak kullanılır.|
|Key Vault kaynak grubu|Kaynak grubu|Adı|**Kilitli** - **Organization name** `-workload-kv-rg` kaynak grubunu benzersiz hale getirmek için kuruluş adını ile birleştirir.|
|Key Vault kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Key Vault şablonu|Resource Manager şablonu|AAD nesne KIMLIĞI|Key Vault örneğine erişim gerektiren hesabın AAD nesne tanımlayıcısı. Varsayılan değer olmadığından boş bırakılamaz. Azure portal bu değeri bulmak için, _Hizmetler_altında "kullanıcılar" ı arayın ve seçin. Hesap adını filtrelemek için _ad_ kutusunu kullanın ve bu hesabı seçin. _Kullanıcı profili_ sayfasında, _nesne kimliğinin_yanındaki "kopyalamak için tıklayın" simgesini seçin.|
|Key Vault şablonu|Resource Manager şablonu|Günlük tutma gün sayısı|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Key Vault şablonu|Resource Manager şablonu|Key Vault SKU 'SU|Oluşturulan Key Vault SKU 'sunu belirtir. Varsayılan değer _Premium_' dur.|
|Key Vault şablonu|Resource Manager şablonu|Azure SQL Server Yönetici Kullanıcı adı|Azure SQL Server erişmek için kullanılan Kullanıcı adı. **Azure SQL veritabanı şablonundaki**aynı özellik değeriyle eşleşmelidir. Varsayılan değer _SQL-admin-User_değeridir.|
|Azure SQL veritabanı kaynak grubu|Kaynak grubu|Adı|**Kilitli** - **Organization name** `-workload-azsql-rg` kaynak grubunu benzersiz hale getirmek için kuruluş adını ile birleştirir.|
|Azure SQL veritabanı kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Azure SQL veritabanı şablonu|Resource Manager şablonu|Azure SQL Server Yönetici Kullanıcı adı|Azure SQL Server için Kullanıcı adı. **Key Vault şablonunda**aynı özellik değeriyle eşleşmelidir. Varsayılan değer _SQL-admin-User_değeridir.|
|Azure SQL veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici parolası (Key Vault kaynak KIMLIĞI)|Key Vault kaynak KIMLIĞI. "/Subscription/{SubscriptionID}/resourceGroups/{orgName}-Workload-kV/Providers/Microsoft.KeyVault/Vaults/{orgName}-Workload-kV" kullanın ve `{subscriptionId}` abonelik Kimliğiniz ile ve `{orgName}` **kuruluşunuzun adı** şema parametresiyle değiştirin.|
|Azure SQL veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici parolası (Key Vault gizli dizi adı)|SQL Server yöneticisinin Kullanıcı adı. **Azure SQL Server Yönetici Kullanıcı adı** **Key Vault şablon** özelliğindeki değerle eşleşmesi gerekir.|
|Azure SQL veritabanı şablonu|Resource Manager şablonu|Günlük tutma gün sayısı|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Azure SQL veritabanı şablonu|Resource Manager şablonu|AAD yönetim nesnesi KIMLIĞI|Active Directory yöneticisi olarak atanacak kullanıcının AAD nesne KIMLIĞI. Varsayılan değer olmadığından boş bırakılamaz. Azure portal bu değeri bulmak için, _Hizmetler_altında "kullanıcılar" ı arayın ve seçin. Hesap adını filtrelemek için _ad_ kutusunu kullanın ve bu hesabı seçin. _Kullanıcı profili_ sayfasında, _nesne kimliğinin_yanındaki "kopyalamak için tıklayın" simgesini seçin.|
|Azure SQL veritabanı şablonu|Resource Manager şablonu|AAD yönetici oturumu açma|Şu anda Microsoft hesapları (örneğin live.com veya outlook.com) yönetici olarak ayarlanamaz. Yalnızca kuruluşunuzdaki kullanıcılar ve güvenlik grupları yönetici olarak ayarlanabilir. Varsayılan değer olmadığından boş bırakılamaz. Azure portal bu değeri bulmak için, _Hizmetler_altında "kullanıcılar" ı arayın ve seçin. Hesap adını filtrelemek için _ad_ kutusunu kullanın ve bu hesabı seçin. _Kullanıcı profili_ sayfasında, _Kullanıcı adını_kopyalayın.|
|App Service Ortamı kaynak grubu|Kaynak grubu|Adı|**Kilitli** - **Organization name** `-workload-ase-rg` kaynak grubunu benzersiz hale getirmek için kuruluş adını ile birleştirir.|
|App Service Ortamı kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|App Service Ortamı şablonu|Resource Manager şablonu|Etki alanı adı|Örnek tarafından oluşturulan Active Directory adı. Varsayılan değer _contoso.com_' dir.|
|App Service Ortamı şablonu|Resource Manager şablonu|Ao konumu|App Service Ortamı konumu. Varsayılan değer _Batı ABD 2_.|
|App Service Ortamı şablonu|Resource Manager şablonu|Günlük tutma Application Gateway gün cinsinden|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|

## <a name="next-steps"></a>Sonraki adımlar

Artık ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması örneğini dağıtma adımlarını gözden geçirdiğinize göre, mimari ve denetim eşlemesi hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [Iso 27001 App Service ortamı/SQL veritabanı iş yükü şeması-genel bakış](./index.md) 
>  [Iso 27001 App Service ortamı/SQL veritabanı iş yükü şeması \ yazdırma denetimi eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
