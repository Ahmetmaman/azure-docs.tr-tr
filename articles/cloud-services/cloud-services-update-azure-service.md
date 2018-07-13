---
title: Bir bulut hizmeti güncelleştirme | Microsoft Docs
description: Azure'da bulut Hizmetleri güncelleştirmesi hakkında bilgi edinin. Bir bulut hizmeti bir güncelleştirme kullanılabilir olmasını sağlamak için nasıl devam eder öğrenin.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: 2f5a82fac18ab34bfa9d6b46f553227ed44a994a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008102"
---
# <a name="how-to-update-a-cloud-service"></a>Bir bulut hizmeti güncelleştirme

Kendi rolleri ve konuk işletim sistemi, gibi bir bulut hizmeti güncelleştirme üç adımlı bir işlemdir. İlk olarak ikili dosyaları ve işletim sistemi sürümü ve yeni bulut hizmeti için yapılandırma dosyalarını karşıya yüklenmelidir. Ardından, Azure yeni bulut hizmeti sürümü gereksinimlerine göre bulut hizmeti için işlem ve ağ kaynakları ayırır. Son olarak, Azure, artımlı olarak Kiracı yeni sürümü veya konuk işletim sistemi, uygulamanızın kullanılabilirliği korurken güncelleştirmek için sıralı yükseltme yapar. Bu son adım – sıralı yükseltme ayrıntıları bu makalede açıklanır.

## <a name="update-an-azure-service"></a>Bir Azure hizmeti güncelleştirmesi
Azure rol örneklerinizin yükseltme etki alanları (UD) adı verilen mantıksal gruplamalarda düzenler. Yükseltme etki alanları (UD), grup olarak güncelleştirilen rol örneği mantıksal kümeleridir.  Bir bulut Azure güncelleştirmeleri bir UD trafik devam etmek için diğer UD örnekleri sağlayan bir anda hizmeti.

Yükseltme etki alanlarının varsayılan sayısı 5'tir. Hizmet tanım dosyası (.csdef) upgradeDomainCount özniteliği dahil olmak üzere farklı bir yükseltme etki alanlarının sayısını belirtebilirsiniz. UpgradeDomainCount özniteliği hakkında daha fazla bilgi için bkz. [WebRole şeması](https://msdn.microsoft.com/library/azure/gg557553.aspx) veya [WorkerRole şeması](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Azure, hizmetinizde bir veya daha fazla rolün bir yerinde güncelleştirme gerçekleştirdiğinizde, ait oldukları yükseltme etki alanına göre rol örneklerinin kümesini güncelleştirir. Azure güncelleştirmeleri, bunları getirilmesi güncelleştirme durdurma belirli bir yükseltme etki alanındaki – tüm çevrimiçi – geri ardından sonraki etki alanına oturum taşır. Yalnızca geçerli yükseltme etki alanında çalışan örnekleri durdurarak Azure bir güncelleştirme çalışan hizmet olabildiğince az etkileyerek olduğunda emin olur. Daha fazla bilgi için [nasıl güncelleştirme geçer](#howanupgradeproceeds) bu makalenin ilerleyen bölümlerinde.

> [!NOTE]
> While koşulları **güncelleştirme** ve **yükseltme** Azure bağlamda biraz farklı bir anlamı yoktur, bu belgedeki özelliklerin açıklamaları ve işlemler için birbirlerinin yerine kullanılabilir.
>
>

Hizmetinizi güncelleştirilmesi bu rol için bir rolün en az iki örneğini tanımlamalıdır yerinde kapalı kalma süresi olmadan. Hizmet, tek bir rol örneği oluşuyorsa, yerinde güncelleştirme tamamlanana kadar hizmet kullanılamaz.

Bu konu Azure güncelleştirmeleri hakkında aşağıdaki bilgileri içerir:

* [Güncelleştirme sırasında hizmeti değişikliklere izin](#AllowedChanges)
* [Nasıl bir yükseltme devam eder](#howanupgradeproceeds)
* [Bir güncelleştirme geri alma](#RollbackofanUpdate)
* [Devam eden bir dağıtımda birden çok mutating işlemleri başlatma](#multiplemutatingoperations)
* [Yükseltme etki alanlarında rollerin](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Güncelleştirme sırasında hizmeti değişikliklere izin
Aşağıdaki tabloda, güncelleştirme sırasında bir hizmet için izin verilen değişiklikleri gösterir:

| Barındırma, hizmetler ve roller için değişikliklere izin | Yerinde güncelleştirme | Aşamalı (VIP swap) | Silin ve yeniden Dağıt |
| --- | --- | --- | --- |
| İşletim sistemi sürümü |Evet |Evet |Evet |
| .NET güven düzeyi |Evet |Evet |Evet |
| Sanal makine boyutu<sup>1</sup> |Evet<sup>2</sup> |Evet |Evet |
| Yerel depolama ayarları |Yalnızca artırmak<sup>2</sup> |Evet |Evet |
| Roller bir hizmet olarak ekleyip |Evet |Evet |Evet |
| Belirli bir rol örneği sayısı |Evet |Evet |Evet |
| Sayı veya bir hizmet için uç noktası türü |Evet<sup>2</sup> |Hayır |Evet |
| Adları ve yapılandırma ayarlarının değerleri |Evet |Evet |Evet |
| Değerleri (ancak değil adları) yapılandırma ayarları |Evet |Evet |Evet |
| Yeni sertifika Ekle |Evet |Evet |Evet |
| Mevcut sertifikaları değiştirme |Evet |Evet |Evet |
| Yeni kod dağıtın |Evet |Evet |Evet |

<sup>1</sup> boyut değişiklik bulut hizmeti için kullanılabilir boyutlar kümesini sınırlıdır.

<sup>2</sup> gerektiren Azure SDK'yı 1.5 veya sonraki sürümler.

> [!WARNING]
> Sanal makine boyutu değiştirme, yerel veri yok.
>
>

Aşağıdaki öğeler, güncelleştirme sırasında desteklenmez:

* Rol adının değiştirilmesi. Kaldırın ve ardından yeni adla rolünü ekleyin.
* Yükseltme etki alanı sayısı değiştiriliyor.
* Yerel kaynakların boyutu kısaltır.

Diğer güncelleştirmeler yerel kaynak boyutu azaltma gibi hizmet tanımı yapıyorsanız VIP takas güncelleştirme yerine gerçekleştirmeniz gerekir. Daha fazla bilgi için [takas dağıtım](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Nasıl bir yükseltme devam eder
Tüm rolleri hizmetinizdeki veya tek bir rol hizmetinde güncelleştirmek isteyip istemediğinizi karar verebilirsiniz. Her iki durumda da yükseltiliyor ve ilk yükseltme etki alanına ait her bir rolün tüm örneklerini durduruldu, yükseltme ve tekrar çevrimiçi. Bunlar yeniden çevrimiçi olduktan sonra ikinci bir yükseltme etki alanındaki örnekleri durduruldu, yükseltme ve tekrar çevrimiçi. Bir bulut hizmeti, en fazla bir yükseltme aynı anda etkin olabilir. Yükseltme, bulut hizmetinin en son sürümü her zaman gerçekleştirilir.

Aşağıdaki diyagramda, nasıl tüm rollerin hizmetinde yükseltiyorsanız yükseltmeye devam eder gösterilmektedir:

![Hizmet yükseltme](media/cloud-services-update-azure-service/IC345879.png "hizmet yükseltme")

Bu bir sonraki diyagramda tek bir rol yükseltiyorsanız güncelleştirme nasıl devam eder gösterilmektedir:

![Yükseltme rol](media/cloud-services-update-azure-service/IC345880.png "yükseltme rolü")  

Azure yapı denetleyicisi, otomatik güncelleştirme sırasında sonraki UD yürütmek güvenli olduğunda belirlemek için bulut hizmeti düzenli aralıklarla değerlendirir. Bu sistem durumu değerlendirmesi rol başına temelinde gerçekleştirilir ve yalnızca en son sürümünü (yani zaten öğrendiniz UD örneklerden) durumlarda göz önünde bulundurur. Olduğunu doğrular, her rol için rol örnekleri en az sayıda tatmin edici bir terminal durumuna elde ettiği başarılar.

### <a name="role-instance-start-timeout"></a>Rol örneği başlangıç zaman aşımı
Yapı denetleyicisi, 30 dakika boyunca her bir rol örneği Başlatıldı durumuna ulaşmasını bekler. Zaman aşımı süresi aşılırsa, yapı denetleyicisi walking sonraki rol örneği için devam eder.

### <a name="impact-to-drive-data-during-cloud-service-upgrades"></a>Bulut hizmeti sırasında sürücü veri etkisi yükseltme

Yükseltme yolu Azure yükseltmeleri Hizmetleri nedeniyle gerçekleştirildiği sırada hizmet tek bir örnekten birden fazla örneğe yükseltirken hizmetinizi kapsama dahil edilecektir. Hizmet düzeyi sözleşmesi guaranteeing hizmet kullanılabilirliği, yalnızca birden fazla örneğiyle dağıtılan hizmetler için geçerlidir. Aşağıdaki listede, her sürücüdeki verilerin her bir Azure hizmet yükseltme senaryosu tarafından nasıl etkilendiğini açıklar:

|Senaryo|C sürücüsü|D sürücüsünü|E sürücü|
|--------|-------|-------|-------|
|VM yeniden başlatma|Korunur|Korunur|Korunur|
|Portal yeniden başlatma|Korunur|Korunur|Yok|
|Portal yeniden görüntü oluşturma|Korunur|Yok|Yok|
|Yerinde yükseltme|Korunur|Korunur|Yok|
|Düğüm geçişi|Yok|Yok|Yok|

Yukarıdaki listede, E: sürücü rolün kök sürücüsünde temsil eder ve sabit kodlu olmamalıdır, unutmayın. Bunun yerine, **RoleRoot %** sürücü temsil etmek için ortam değişkeni.

Tek Örnekli bir hizmeti yükseltilirken kapalı kalma süresini en aza indirmek için hazırlık sunucusu için yeni bir çok örnekli hizmet dağıtma ve bir VIP takası gerçekleştirin.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Bir güncelleştirme geri alma
Azure hizmetlerini güncelleştirme sırasında bir hizmet üzerinde ek işlemler tarafından Azure yapı denetleyicisi ilk güncelleştirme isteğini kabul edildikten sonra başlatmak vererek yönetme esnekliği sağlar. Bir geri alma yalnızca bir güncelleştirme olduğunda (yapılandırma değişikliği) gerçekleştirilebilir veya yükseltme zamanı **sürüyor** dağıtımda durum. Bir güncelleştirme veya yükseltme henüz yeni sürüme güncelleştirilmemiş hizmetini en az bir örnek var olduğu sürece devam ediyor olarak kabul edilir. Bir geri alma izin verilip verilmediğini test etmek için tarafından döndürülen RollbackAllowed bayrak değerini kontrol edin [alma dağıtım](https://msdn.microsoft.com/library/azure/ee460804.aspx) ve [bulut hizmeti özelliklerini almak](https://msdn.microsoft.com/library/azure/ee460806.aspx) işlemleri ayarlanır true.

> [!NOTE]
> Yalnızca geri alma çağırmak için mantıklı bir **yerinde** güncelleştirmek ya da hizmetiniz bir tüm çalışan örneği diğeriyle VIP takas yükseltmeleri hatalarıyla ilgili olduğundan yükseltme.
>
>

Geri alma devam eden güncelleştirme dağıtımda aşağıdaki etkileri gösterir:

* Hangi henüz güncelleştirildi veya yeni sürüme yükseltilmesi herhangi bir rol örneği güncelleştirilmez veya hedef sürümü, hizmet bu örneği zaten çalıştığından, yükseltme.
* Hangi zaten güncelleştirilmiş veya yeni bir hizmet paketi sürümüne yükseltilmeyen rol örnekleri (\*.cspkg) dosya veya hizmet yapılandırması (\*.cscfg) dosya (veya her iki dosya) bu dosyaları yükseltme öncesi sürümüne geri.

Bu, işlevsel olarak aşağıdaki özellikler tarafından sağlanır:

* [Geri alma güncelleştirmesi veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx) yapılandırma güncelleştirmesinde çağrılabilen bir işlemi (çağırarak tetiklenen [dağıtım yapılandırmasını değiştirme](https://msdn.microsoft.com/library/azure/ee460809.aspx)) veya yükseltme (çağırarak tetiklenen [ Yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx)) var olduğu sürece en az bir örnek hizmetinde henüz yeni sürüme güncelleştirilmemiş.
* Yanıt gövdesi bir parçası olarak döndürülen kilitli öğeyi ve RollbackAllowed öğesi [alma dağıtım](https://msdn.microsoft.com/library/azure/ee460804.aspx) ve [bulut hizmeti özelliklerini almak](https://msdn.microsoft.com/library/azure/ee460806.aspx) işlemleri:

  1. Kilitli öğeyi mutating bir işlemi belirli bir dağıtım üzerinde çağrılabilir Algıla sağlar.
  2. RollbackAllowed öğesi algılama sağlar [geri alma güncelleştirmesi veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx) üzerinde belirli bir dağıtım işlemi çağrılabilir.

  Bir geri alma işlemi gerçekleştirmek için kilitli hem RollbackAllowed öğeleri denetleme gerekmez. RollbackAllowed ayarlandığını doğrulamak için eklerini true. Bu yöntemler kümesine istek üst bilgisini kullanarak çağrılır, bu öğeleri yalnızca döndürülür "x-ms-version: 2011-10-01" veya sonraki bir sürümü. Sürüm üstbilgileri hakkında daha fazla bilgi için bkz. [Hizmet Yönetimi sürümü oluşturma](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Bazı durumlarda bir geri alma bir güncelleştirme olduğunda veya yükseltme desteklenmiyor, bunlar şu şekildedir:

* Yerel kaynaklar - güncelleştirmeyi bir rolü Azure platformu için yerel kaynak artarsa azalma geri izin vermez.
* Kota sınırlamalarını - güncelleştirme ölçeği azaltma işlemi artık çalışmayabilir olduysa geri alma işlemi tamamlamak için yeterli işlem kotası vardır. Her Azure aboneliği, bu aboneliğe ait tüm barındırılan hizmetler tarafından kullanılabilecek çekirdek sayısını belirten, kendisiyle ilişkili bir kota vardır. Belirli bir güncelleştirme işlemin geri aboneliğiniz kota sonra bu yana koyabilirsiniz, bir geri alma etkin değil.
* Yarış durumu - ilk güncelleştirme Tamamlandı durumunda geri almanın mümkün değildir.

Kullanıyorsanız, bir güncelleştirme geri alma ne zaman yararlı olabilir, bir örnek verilmiştir [yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx) Azure ana yerinde yükseltme sırasında hizmet barındırılan hızı denetlemek için el ile modu işleminde piyasaya sunuluyor.

Yükseltme dağıtımı sırasında çağırmanızı [yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx) el ile modunda ve yükseltme etki alanları gitmeye başlayabilirsiniz. Yükseltme, izleme gibi belirli bir noktada, Not incelemeniz ilk yükseltme etki alanlarında bazı rol örneklerini yanıt veremez duruma, çağırabilirsiniz [geri alma güncelleştirmesi veya yükseltme](https://msdn.microsoft.com/library/azure/hh403977.aspx) bırakır, dağıtım işlemi henüz yükseltme örneklere ve geri alma örnekleri, bir önceki hizmet paketi ve yapılandırması için yükseltilmiş olduğu.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Devam eden bir dağıtımda birden çok mutating işlemleri başlatma
Bazı durumlarda, devam eden bir dağıtım birden çok eşzamanlı mutating işlemleri başlatmak isteyebilirsiniz. Örneğin, bir hizmet güncelleştirmesi gerçekleştirebilir ve bu güncelleştirmeyi hizmetinizi kullanıma sunulacaktır, ancak bazı değişiklikler örn yapmak istediğiniz güncelleştirme geri dönmek için farklı bir güncelleştirme uygulamak veya hatta dağıtımı silin. Hizmet yükseltme yükseltilen rol örneği sürekli olarak çökmesine neden olan hatalı kodu içeriyorsa, bu gerekli olabilir durumu gösterir. Bu durumda, Azure yapı denetleyicisi yükseltilmiş etki alanındaki örneklerinin sayısı yetersiz iyi durumda olduğundan, yükseltme uygulama devam eden yapmak mümkün olmayacaktır. Bu durum şeklinde adlandırılan bir *takılı dağıtım*. Güncelleştirme geri alma ya da yeni bir güncelleştirme başarısız üst uygulama dağıtımını Ayır biri.

Azure yapı denetleyicisi tarafından güncelleştirmek veya hizmeti yükseltmek için ilk istek alındı sonra sonraki diziyi işlemleri başlayabilirsiniz. Diğer bir deyişle, mutating başka bir işlem başlamadan önce ilk işlemin tamamlanmasını beklemek zorunda değildir.

İlk güncelleştirme devam ederken, ikinci bir güncelleştirme işlemi başlatılıyor, benzer şekilde geri alma işlemi gerçekleştirir. İkinci güncelleştirmeyi otomatik modundaysa, ilk yükseltme etki alanı büyük olasılıkla zaman içinde aynı noktada çevrimdışı olan birden fazla yükseltme etki alanlarının örneklerine baştaki hemen yükseltilir.

Mutating işlem aşağıdaki gibidir: [dağıtım yapılandırmasını değiştirme](https://msdn.microsoft.com/library/azure/ee460809.aspx), [yükseltme dağıtımı](https://msdn.microsoft.com/library/azure/ee460793.aspx), [güncelleştirme dağıtım durumu](https://msdn.microsoft.com/library/azure/ee460808.aspx), [dağıtımı Sil ](https://msdn.microsoft.com/library/azure/ee460815.aspx), ve [geri alma güncelleştirmesi veya yükseltmesi](https://msdn.microsoft.com/library/azure/hh403977.aspx).

İki işlem [alma dağıtım](https://msdn.microsoft.com/library/azure/ee460804.aspx) ve [bulut hizmeti özelliklerini almak](https://msdn.microsoft.com/library/azure/ee460806.aspx), mutating bir işlemi belirli bir dağıtım üzerinde çağrılabilir olup olmadığını belirlemek için denetlenen kilitli bayrağı döndürür.

Kilitli bayrağı döndüren sürümü bu yöntemleri çağırmak için istek üst bilgisini ayarlayın gerekir "x-ms-version: 2011-10-01" veya bir sonraki. Sürüm üstbilgileri hakkında daha fazla bilgi için bkz. [Hizmet Yönetimi sürümü oluşturma](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Yükseltme etki alanlarında rollerin
Azure rol örneklerini sayıda hizmet açıklaması (.csdef) dosyasının bir parçası olarak yapılandırılabilir yükseltme etki alanları arasında eşit olarak dağıtır. Yükseltme etki alanlarının en yüksek sayısı 20'dir ve varsayılan değer 5'tir. Hizmet tanım dosyası değiştirme hakkında daha fazla bilgi için bkz. [Azure Hizmet tanım düzenini (.csdef dosyası)](cloud-services-model-and-package.md#csdef).

Örneğin, rolünüzün on örneği varsa, varsayılan olarak her bir yükseltme etki alanı iki örneği içerir. Rolünüz 14 örneği varsa, ardından dört yükseltme etki alanlarında üç örneklerini içerir ve iki beşinci bir etki alanı içerir.

Yükseltme etki alanları, sıfır tabanlı diziniyle tanımlanır: 0 kimliği ilk yükseltme etki alanı varsa ve ikinci yükseltme etki alanı kimliği 1 ve benzeri.

Aşağıdaki diyagramda, nasıl iki rol içeren bir hizmet dağıtılmış iki yükseltme etki alanı hizmeti tanımlar, gösterilmektedir. Hizmet, sekiz örnekleri web rolü ve çalışan rolünün dokuz örneği çalışıyor.

![Yükseltme etki alanlarının dağılımı](media/cloud-services-update-azure-service/IC345533.png "yükseltme etki alanlarının dağılımı")

> [!NOTE]
> Azure örnekleri yükseltme etki alanları arasında nasıl ayrıldığını denetleyen olduğunu unutmayın. Hangi örneklerinin hangi etki alanı için ayrılmış belirtmek mümkün değildir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Cloud Services nasıl yönetilir?](cloud-services-how-to-manage-portal.md)  
[Bulut hizmetleri nasıl izlenir?](cloud-services-how-to-monitor.md)  
[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)  
