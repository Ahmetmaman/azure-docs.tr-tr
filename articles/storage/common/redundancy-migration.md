---
title: Depolama hesabının nasıl çoğaltıldığını değiştirme
titleSuffix: Azure Storage
description: Mevcut bir depolama hesabındaki verilerin nasıl çoğaltılacağı nasıl değiştirileceğini öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/24/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: c305292e915e02a1b53eb140ccd052990efbd315
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91827306"
---
# <a name="change-how-a-storage-account-is-replicated"></a>Depolama hesabının nasıl çoğaltıldığını değiştirme

Azure depolama, geçici donanım arızaları, ağ veya güç kesintileri ve çok büyük doğal felaketler de dahil olmak üzere planlı ve planlanmamış olaylardan korunabilmesi için verilerinizin birden çok kopyasını her zaman depolar. Yedeklilik, depolama hesabınızın, başarısızlık durumunda bile [Azure depolama Için hizmet düzeyi sözleşmesini (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) karşıladığından emin olmanızı sağlar.

Azure Storage aşağıdaki çoğaltma türlerini sunar:

- Yerel olarak yedekli depolama (LRS)
- Alanlar arası yedekli depolama (ZRS)
- Coğrafi olarak yedekli depolama (GRS) veya Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)
- Coğrafi bölge yedekli depolama (GZRS) veya Okuma Erişimli Coğrafi bölge yedekli depolama (RA-GZRS)

Bu seçeneklerin her birine genel bir bakış için bkz. [Azure Storage yedekliği](storage-redundancy.md).

## <a name="switch-between-types-of-replication"></a>Çoğaltma türleri arasında geçiş yap

Bir depolama hesabını bir çoğaltma türünden başka bir türe geçirebilirsiniz, ancak bazı senaryolar diğerlerinden daha basittir. Coğrafi çoğaltma eklemek veya kaldırmak istiyorsanız, çoğaltma ayarını güncelleştirmek için Azure portal, PowerShell veya Azure CLı ' yi kullanabilirsiniz. Ancak, birincil bölgede verilerin nasıl çoğaltıldığını değiştirmek istiyorsanız, LRS 'den ZRS 'ye geçerek veya bunun tersini yaparak el ile geçiş gerçekleştirmeniz gerekir.

Aşağıdaki tabloda, her bir çoğaltma türünden diğerine nasıl geçkullanılacağına ilişkin bir genel bakış sunulmaktadır:

| Geçiş | ... LRS 'ye | ... GRS/RA-GRS 'ye | ... ZRS 'ye | ... GZRS/RA-GZRS |
|--------------------|----------------------------------------------------|---------------------------------------------------------------------|----------------------------------------------------|---------------------------------------------------------------------|
| <b>... LRS 'den</b> | Yok | Azure portal, PowerShell veya CLı kullanarak çoğaltma ayarını değiştirme<sup>1</sup> | El ile geçiş gerçekleştirme <br /><br /> VEYA <br /><br /> Dinamik geçiş isteme | El ile geçiş gerçekleştirme <br /><br /> VEYA <br /><br /> Önce GRS/RA-GRS ' y e geçin ve ardından dinamik geçiş isteyin<sup>1</sup> |
| <b>... GRS/RA-GRS 'den</b> | Çoğaltma ayarını değiştirmek için Azure portal, PowerShell veya CLı kullanın | Yok | El ile geçiş gerçekleştirme <br /><br /> VEYA <br /><br /> Önce LRS 'ye geçin ve ardından dinamik geçiş isteyin | El ile geçiş gerçekleştirme <br /><br /> VEYA <br /><br /> Dinamik geçiş isteme |
| <b>... ZRS 'den</b> | El ile geçiş gerçekleştirme | El ile geçiş gerçekleştirme | Yok | Azure portal, PowerShell veya CLı kullanarak çoğaltma ayarlarını değiştirme<sup>1, 2</sup> |
| <b>... GZRS/RA-GZRS öğesinden</b> | El ile geçiş gerçekleştirme | El ile geçiş gerçekleştirme | Çoğaltma ayarını değiştirmek için Azure portal, PowerShell veya CLı kullanın | Yok |

<sup>1</sup> bir kerelik çıkış ücreti doğurur.<br />
<sup>2</sup> ZRS 'den GZRS/ra-GZRS veya tam tersi olarak dönüştürme şu bölgelerde desteklenmez: ABD Doğu 2, ABD Doğu, Avrupa Batı.

> [!CAUTION]
> (RA-) GRS veya (RA-) GZRS hesabınız için bir [Hesap yük devretmesi](storage-disaster-recovery-guidance.md) gerçekleştirdiyseniz, hesap yük devretmeden sonra yeni birincil bölgede yerel olarak yedekli olur. Yük devretme işleminden kaynaklanan bir LRS hesabı için ZRS veya GZRS 'e dinamik geçiş desteklenmez. Bu, yeniden çalışma işlemleri olarak adlandırılan bu durum da geçerlidir. Örneğin, RA-GZRS ' d e bir hesap yük devretmesini ikincil bölgedeki LRS 'ye devreder ve sonra yeniden RA-GRS olarak yapılandırıp özgün birincil bölgeye başka bir hesap yük devretme işlemi gerçekleştirirseniz, birincil bölgede RA-GZRS için özgün dinamik geçişe yönelik desteğe başvuramıyoruz. Bunun yerine, ZRS veya GZRS için el ile geçiş gerçekleştirmeniz gerekir.

## <a name="change-the-replication-setting"></a>Çoğaltma ayarını değiştir

Bir depolama hesabının çoğaltma ayarını değiştirmek için Azure portal, PowerShell veya Azure CLı ' yi kullanarak, verilerin birincil bölgede nasıl çoğaltılabileceğini değiştirirsiniz. Birincil bölgedeki LRS 'den birincil bölgedeki ZRS 'ye geçiş yapıyorsanız veya bunun tersini yaparsanız, el ile geçiş ya da dinamik geçiş gerçekleştirmeniz gerekir.

Depolama hesabınızın nasıl çoğaltılacağı, uygulamalarınız için zaman kaybına neden olmaz.

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal depolama hesabınızın artıklık seçeneğini değiştirmek için şu adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Yapılandırma** ayarını seçin.
1. **Çoğaltma** ayarını güncelleştirin.

![Portalda çoğaltma seçeneğinin nasıl değiştirileceğini gösteren ekran görüntüsü](media/redundancy-migration/change-replication-option.png)

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile depolama hesabınızın artıklık seçeneğini değiştirmek için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu çağırın ve `-SkuName` parametresini belirtin:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage_account> `
    -SkuName <sku>
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile depolama hesabınızın artıklık seçeneğini değiştirmek için [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update) komutunu çağırın ve `--sku` parametresini belirtin:

```azurecli-interactive
az storage account update \
    --name <storage-account>
    --resource-group <resource_group> \
    --sku <sku>
```

---

## <a name="perform-a-manual-migration-to-zrs-gzrs-or-ra-gzrs"></a>ZRS, GZRS veya RA-GZRS için el ile geçiş gerçekleştirin

LRS 'den ZRS 'ye geçerek veya bunun tersini yaparak Depolama hesabınızdaki verilerin birincil bölgede çoğaltılmasını değiştirmek istiyorsanız el ile geçiş yapmayı tercih edebilirsiniz. El ile geçiş, dinamik geçişten daha fazla esneklik sağlar. El ile geçişin zamanlamasını kontrol edersiniz, bu nedenle geçişin belirli bir tarihten sonra tamamlanmasını istiyorsanız bu seçeneği kullanın.

LRS 'den birincil bölgedeki ZRS 'ye el ile geçiş gerçekleştirdiğinizde veya bunun tersini yaptığınızda, hedef depolama hesabı coğrafi olarak yedekli olabilir ve ayrıca ikincil bölgeye okuma erişimi için de yapılandırılabilir. Örneğin, bir LRS hesabını bir adımda GZRS veya RA-GZRS hesabına geçirebilirsiniz.

El ile geçiş, uygulama kapalı kalma süresine yol açabilir. Uygulamanız yüksek kullanılabilirlik gerektiriyorsa, Microsoft dinamik geçiş seçeneği de sağlar. Dinamik geçiş, kapalı kalma süresi olmayan bir yerinde geçiş işlemidir.

El ile geçişle, mevcut depolama hesabınızdan verileri birincil bölgede ZRS kullanan yeni bir depolama hesabına kopyalayabilirsiniz. El ile geçiş yapmak için aşağıdaki seçeneklerden birini kullanabilirsiniz:

- AzCopy gibi mevcut bir aracı kullanarak, Azure Storage istemci kitaplıklarından biri veya güvenilir bir üçüncü taraf aracından verileri kopyalayın.
- Hadoop veya HDInsight hakkında bilginiz varsa, hem kaynak depolama hesabı hem de hedef depolama hesabı hesabını kümenize ekleyebilirsiniz. Daha sonra, paralel hale getirmek gibi bir araçla veri kopyalama işlemini yapın.

## <a name="request-a-live-migration-to-zrs-gzrs-or-ra-gzrs"></a>ZRS, GZRS veya RA-GZRS için dinamik geçiş isteyin

Depolama hesabınızı, birincil bölgede uygulama kesinti olmadan LRS 'den ZRS 'ye geçirmeniz gerekiyorsa, Microsoft 'tan Canlı geçiş isteğinde bulunabilir. LRS 'den GZRS veya RA-GZRS 'e geçiş yapmak için önce GRS veya RA-GRS ' ye geçin ve ardından dinamik geçiş isteyin. Benzer şekilde, GRS veya RA-GRS ' den GZRS veya RA-GZRS ' d e dinamik geçiş isteğinde bulabilirsiniz. GRS veya RA-GRS 'den ZRS 'ye geçiş yapmak için önce LRS 'ye geçin ve ardından dinamik geçiş isteyin.

Dinamik geçiş sırasında, Depolama hesabınızdaki verilere dayanıklılık veya kullanılabilirlik kaybı olmaksızın erişebilirsiniz. Azure Storage SLA, geçiş işlemi sırasında sürdürülür. Dinamik geçişle ilişkili bir veri kaybı yok. Hizmet uç noktaları, erişim anahtarları, paylaşılan erişim imzaları ve diğer hesap seçenekleri geçişten sonra değişmeden kalır.

ZRS yalnızca genel amaçlı v2 hesaplarını destekler, bu nedenle ZRS 'ye dinamik geçiş isteği göndermeden önce depolama hesabınızı yükseltdiğinizden emin olun. Daha fazla bilgi için bkz. [genel amaçlı v2 depolama hesabına yükseltme](storage-account-upgrade.md). Depolama hesabı, dinamik geçiş yoluyla geçirilecek verileri içermelidir.

Dinamik geçiş yalnızca LRS veya GRS çoğaltmasını kullanan depolama hesapları için desteklenir. Hesabınız RA-GRS kullanıyorsa, önce devam etmeden önce hesabınızın çoğaltma türünü LRS veya GRS olarak değiştirmeniz gerekir. Bu ara adım, geçişten önce RA-GRS tarafından sunulan ikincil salt okuma uç noktasını kaldırır.

Microsoft canlı geçiş isteğinizi anında işler ama canlı bir geçişin ne zaman tamamlanacağı garanti edilemez. Verilerinizin belirli bir tarihe kadar ZRS'ye geçirilmesi gerekiyorsa, Microsoft bunun yerine el ile geçiş yapmanızı önerir. Genel olarak hesabınızda ne kadar çok veri varsa bu verilerin geçişi de o kadar uzun sürer.

Şu durumlarda el ile geçiş gerçekleştirmeniz gerekir:

- Verilerinizi, kaynak hesaptan farklı bir bölgede bulunan bir ZRS depolama hesabına geçirmek istiyorsunuz.
- Depolama hesabınız bir Premium Sayfa Blobu veya Blok Blobu hesabıdır.
- ZRS 'den LRS, GRS veya RA-GRS 'ye veri geçirmek istiyorsunuz.
- Depolama Hesabınız arşiv katmanındaki verileri içerir.

[Azure Destek Portalı](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)aracılığıyla dinamik geçiş isteğinde bulunabilir. Portaldan ZRS 'ye dönüştürmek istediğiniz depolama hesabını seçin.

1. **Yeni destek isteği ' ni**seçin.
2. Hesap bilgilerinizi temel alan **temel bilgileri** doldurun: 
    - **Sorun türü**: **Teknik**' i seçin.
    - **Hizmet**: **hizmetlerimi** ve **depolama hesabı yönetimi**seçin.
    - **Kaynak**: ZRS 'ye dönüştürmek istediğiniz kaynağı seçin.
3. **İleri**’yi seçin.
4. **Sorun** bölümünün aşağıdaki değerlerini belirtin:
    - **Önem derecesi**: varsayılan değeri olduğu gibi bırakın.
    - **Sorun türü**: **veri geçişini**seçin.
    - **Kategori**: **ZRS 'ye geçir**' i seçin.
    - **Başlık**: Örneğin, **ZRS hesabı geçişi**gibi açıklayıcı bir başlık yazın.
    - **Ayrıntılar**: **Ayrıntılar** kutusuna ek ayrıntılar yazın, örneğin, bölgedeki [LRS, GRS] öğesinden ZRS 'ye geçiş yapmak istiyorum \_ \_ .
5. **İleri**’yi seçin.
6. İletişim bilgilerinin **iletişim bilgileri** dikey penceresinde doğru olduğunu doğrulayın.
7. **Oluştur**’u seçin.

Bir destek kişisi sizinle iletişim kuracaktır ve ihtiyacınız olan herhangi bir yardımı sağlar.

> [!NOTE]
> Dinamik geçiş, Premium dosya paylaşımları için şu anda desteklenmiyor. Şu anda yalnızca el ile kopyalama veya taşıma işlemi destekleniyor.
>
> GZRS depolama hesapları Şu anda arşiv katmanını desteklemez. Daha fazla ayrıntı için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) .
>
> Yönetilen diskler yalnızca LRS için kullanılabilir ve ZRS 'ye geçirilemez. Standart HDD 'de standart SSD ile yönetilen diskler için anlık görüntüler ve görüntüler saklayabilir ve [LRS ve ZRS seçenekleri arasından seçim](https://azure.microsoft.com/pricing/details/managed-disks/)yapabilirsiniz. Kullanılabilirlik kümeleriyle tümleştirme hakkında daha fazla bilgi için bkz. [Azure yönetilen disklere giriş](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets).

## <a name="switch-from-zrs-classic"></a>ZRS klasik 'den geçiş

> [!IMPORTANT]
> Microsoft, ZRS klasik hesaplarını 31 Mart 2021 ' de kullanımdan kaldırır ve geçirebilir. Kullanım dışı bırakmadan önce ZRS klasik müşterilerine daha fazla ayrıntı sunulacaktır.
>
> ZRS 'nin belirli bir bölgede genel kullanıma açık hale gelmesi durumunda müşteriler artık bu bölgedeki Azure portal ZRS klasik hesapları oluşturamayacak. ZRS klasik hesapları oluşturmak için Microsoft PowerShell ve Azure CLı kullanmak, ZRS klasik kullanım dışı olana kadar bir seçenektir. ZRS 'nin kullanılabildiği durumlar hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](storage-redundancy.md).

ZRS klasik, verileri bir veya iki bölge içindeki veri merkezlerinde zaman uyumsuz olarak çoğaltır. Microsoft ikinciye yük devretmeyi başlatmadığı müddetçe çoğaltılan veriler kullanılamayabilir. ZRS klasik hesabı LRS, GRS veya RA-GRS ' y e veya bu bilgisayarlardan dönüştürülemez. ZRS klasik hesapları, ölçümleri veya günlüğe kaydetmeyi de desteklemez.

ZRS klasik, yalnızca genel amaçlı v1 (GPv1) depolama hesaplarında **blok Bloblar** için kullanılabilir. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

ZRS hesabı verilerini LRS, GRS, RA-GRS veya ZRS klasik hesabına el ile geçirmek için aşağıdaki araçlardan birini kullanın: AzCopy, Azure Depolama Gezgini, PowerShell veya Azure CLı. Ayrıca, Azure depolama istemci kitaplıklarından biriyle kendi geçiş çözümünüzü oluşturabilirsiniz.

ZRS klasik depolama hesabınızı, ZRS 'nin kullanılabildiği bölgelerde Azure portal, PowerShell veya Azure CLı kullanarak da ZRS 'ye yükseltebilirsiniz.

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal ZRS 'ye yükseltmek için hesabın **yapılandırma** ayarlarına gidin ve **Yükselt**' i seçin:

![Portalda ZRS Classic 'i ZRS 'ye yükseltme](media/redundancy-migration/portal-zrs-classic-upgrade.png)

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell kullanarak ZRS 'ye yükseltmek için aşağıdaki komutu çağırın:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> -AccountName <storage_account> -UpgradeToStorageV2
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak ZRS 'ye yükseltme yapmak için aşağıdaki komutu çağırın:

```cli
az storage account update -g <resource_group> -n <storage_account> --set kind=StorageV2
```

---

## <a name="costs-associated-with-changing-how-data-is-replicated"></a>Verilerin nasıl çoğaltıldığını değiştirmekle ilişkili maliyetler

Verilerin nasıl çoğaltıldığına ilişkin maliyet, dönüştürme yolunuza göre değişir. En az pahalı olan Azure depolama yedekliliği teklifleri, LRS, ZRS, GRS, RA-GRS, GZRS ve RA-GZRS ' yi içerir.

Örneğin, LRS *'den* başka bir çoğaltma türüne geçmek, daha ayrıntılı bir artıklık düzeyine taşıdığınız için ek ücretler doğurur. GRS veya RA-GRS ' *e* geçiş, verileriniz (birincil bölgenizde) uzak ikincil bölgenize çoğaltıldığı için çıkış bant genişliği ücretine neden olur. Bu ücret, ilk kurulumda tek seferlik bir maliyettir. Veriler kopyalandıktan sonra başka geçiş ücretleri yoktur. Bant genişliği ücretleri hakkında daha fazla bilgi için bkz. [Azure Depolama fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

GRS 'den LRS 'ye depolama hesabınızı geçirirseniz, ek bir maliyet yoktur, ancak çoğaltılan verileriniz ikincil konumdan silinir.

> [!IMPORTANT]
> Depolama hesabınızı RA-GRS ' d e veya LRS 'ye geçirirseniz, bu hesap, dönüştürülen tarihin ötesinde 30 gün boyunca RA-GRS olarak faturalandırılır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure depolama artıklığı](storage-redundancy.md)
- [Bir depolama hesabı için son eşitleme zamanı özelliğini denetleyin](last-sync-time-get.md)
- [Yüksek oranda kullanılabilir uygulamalar tasarlamak için coğrafi artıklığı kullanın](geo-redundant-design.md)
