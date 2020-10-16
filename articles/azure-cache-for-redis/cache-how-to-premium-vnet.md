---
title: Bir sanal ağ ve Redsıs için Premium Azure önbelleği yapılandırma
description: Redsıs örnekleri için Premium katman Azure önbelleğiniz için sanal ağ desteği oluşturma ve yönetme hakkında bilgi edinin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: devx-track-csharp
ms.topic: conceptual
ms.date: 10/09/2020
ms.openlocfilehash: 34e4781d1437b34607a6d9e4f99ec5bd2ef9b46d
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91999986"
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-cache-for-redis"></a>Redsıs için Premium Azure önbelleği için sanal ağ desteğini yapılandırma
Redin için Azure önbelleğinde, kümeleme, kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri de dahil olmak üzere, önbellek boyutu ve özellikleri seçimine esneklik sağlayan farklı önbellek teklifleri vardır. VNet, buluttaki özel bir ağ. Redsıs örneği için bir Azure önbelleği bir sanal ağ ile yapılandırıldığında, bu, genel olarak adreslenebilir değildir ve yalnızca VNet içindeki sanal makineler ve uygulamalardan erişilebilir. Bu makalede, Redsıs örneği için Premium bir Azure önbelleği için sanal ağ desteğinin nasıl yapılandırılacağı açıklanır.

> [!NOTE]
> Redsıs için Azure önbelleği hem klasik hem de Kaynak Yöneticisi sanal ağları destekler.
> 
> 

## <a name="why-vnet"></a>Neden VNet?
[Azure sanal ağ (VNet)](https://azure.microsoft.com/services/virtual-network/) dağıtımı, Azure önbelleğiniz için gelişmiş güvenlik ve yalıtımın yanı sıra alt ağlar, erişim denetim ilkeleri ve diğer özellikler için erişimi daha da kısıtlamak sağlar.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Sanal ağ (VNet) desteği, önbellek oluşturma sırasında **redsıs dikey penceresinde yeni Azure önbelleğinde** yapılandırılır. 

1. Premium önbellek oluşturmak için [Azure Portal](https://portal.azure.com) oturum açın ve **kaynak oluştur**' u seçin. Bkz. Azure portal önbellekler oluşturmaya ek olarak, Kaynak Yöneticisi şablonları, PowerShell veya Azure CLı kullanarak da oluşturabilirsiniz. Redu için Azure önbelleği oluşturma hakkında daha fazla bilgi için bkz. [önbellek oluşturma](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Kaynak oluştur.":::
   
2. **Yeni** sayfada **veritabanları** ' nı seçin ve ardından **redsıs için Azure önbelleği**' ni seçin.

    :::image type="content" source="media/cache-private-link/2-select-cache.png" alt-text="Kaynak oluştur.":::

3. **Yeni Redis Cache** sayfasında, yeni Premium önbelleğiniz için ayarları yapılandırın.
   
   | Ayar      | Önerilen değer  | Açıklama |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca rakam, harf veya kısa çizgi içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlamalı ve bitmeli ve ardışık kısa çizgi içeremez. Önbellek örneğinizin *ana bilgisayar adı* * \<DNS name> . Redis.cache.Windows.net*olacaktır. | 
   | **Abonelik** | Açılır ve aboneliğinizi seçin. | Redsıs örneği için bu yeni Azure önbelleğinin oluşturulacağı abonelik. | 
   | **Kaynak grubu** | Açılır ve bir kaynak grubu seçin veya **Yeni oluştur** ' u seçin ve yeni bir kaynak grubu adı girin. | Önbelleğinizin ve diğer kaynaklarınızın oluşturulacağı kaynak grubunun adı. Tüm uygulama kaynaklarınızı tek bir kaynak grubuna yerleştirerek, bunları birlikte kolayca yönetebilir veya silebilirsiniz. | 
   | **Konum** | Açılır ve bir konum seçin. | Önbelleğinizi kullanacak diğer hizmetlerin yakınında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
   | **Önbellek türü** | Açılır ve Premium özellikleri yapılandırmak için Premium önbellek seçin. Ayrıntılar için bkz. [redsıs fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/). |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için bkz. [redsıs Için Azure önbelleği 'Ne genel bakış](cache-overview.md). |

4. **Ağ** sekmesini seçin veya sayfanın altındaki **ağ** düğmesine tıklayın.

5. **Ağ** sekmesinde, bağlantı yönteminiz olarak **sanal ağlar** ' ı seçin. Yeni bir sanal ağ kullanmak için, [Azure Portal kullanarak sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network) veya [Azure Portal kullanarak bir sanal ağ oluşturma (klasik)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) adımlarını izleyerek ve ardından Premium önbelleğinizi oluşturmak ve yapılandırmak üzere **Redsıs dikey penceresinde yeni Azure önbelleğine** geri dönüp oluşturun.

> [!IMPORTANT]
> Redsıs için Azure önbelleğinin bir Kaynak Yöneticisi VNet 'e dağıtılmasında, önbelleğin Redsıs örnekleri için Azure önbelleği dışında başka hiçbir kaynak içermeyen bir ayrılmış alt ağda olması gerekir. Redsıs için Azure önbelleğini diğer kaynakları içeren bir alt ağa bir Kaynak Yöneticisi VNet 'e dağıtmak için bir girişimde bulunuldu, dağıtım başarısız olur.
> 
> 

   | Ayar      | Önerilen değer  | Açıklama |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Sanal ağ** | Açılır ve Sanal ağınızı seçin. | Önbelleğiniz ile aynı abonelikte ve konumda bulunan bir sanal ağ seçin. | 
   | **Alt ağ** | Açılır ve alt ağlarınızı seçin. | Alt ağın adres aralığı CıDR gösteriminde (ör. 192.168.1.0/24) olmalıdır. Sanal ağın adres alanı tarafından içerilmelidir. | 
   | **Statik IP adresi** | Seçim Statik bir IP adresi girin. | Statik IP belirtmezseniz, otomatik olarak bir IP adresi seçilir. | 

> [!IMPORTANT]
> Azure, bazı IP adreslerini her alt ağ içinde ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumu için ayrılmıştır ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için bkz. [Bu alt AĞLARDAKI IP adreslerini kullanma konusunda herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Azure VNET altyapısı tarafından kullanılan IP adreslerine ek olarak, alt ağdaki her reddo örneği, yük dengeleyici için bir parça başına iki IP adresi ve bir ek IP adresi kullanır. Kümelenmemiş bir önbelleğin bir parça olduğu kabul edilir.
> 
> 

6. **İleri: Gelişmiş** sekmesini seçin veya sayfanın altındaki **İleri: Gelişmiş** düğmesine tıklayın.

7. Premium önbellek örneğinin **Gelişmiş** SEKMESINDE, TLS olmayan bağlantı noktası, kümeleme ve veri kalıcılığı için ayarları yapılandırın. 

8. **Sonraki: Etiketler** sekmesini seçin veya sayfanın altındaki **Sonraki: Etiketler** düğmesine tıklayın.

9. İsteğe bağlı olarak, **Etiketler** sekmesinde, kaynağı sınıflandırmak istiyorsanız ad ve değeri girin. 

10.  **Gözden geçir + oluştur**' u seçin. Azure 'un yapılandırmanızı doğruladığı, gözden geçir + Oluştur sekmesine götürülürsünüz.

11. Yeşil doğrulama başarılı iletisi göründüğünde **Oluştur**' u seçin.

Önbelleğin oluşturulması biraz zaman alır. Redsıs **genel bakış**   sayfasında ilerlemeyi izleyebilirsiniz.  **Durum**    **çalışıyor**olarak görüntülendiğinde, önbellek kullanıma hazırdır. Önbellek oluşturulduktan sonra, **Kaynak menüsünden** **sanal ağ ' a** tıklayarak VNET 'in yapılandırmasını görüntüleyebilirsiniz.

![Sanal ağ][redis-cache-vnet-info]

VNet kullanırken Redsıs örneği için Azure önbelleğinize bağlanmak için aşağıdaki örnekte gösterildiği gibi bağlantı dizesinde önbelleğinizin ana bilgisayar adını belirtin:

```csharp
private static Lazy<ConnectionMultiplexer>
    lazyConnection = new Lazy<ConnectionMultiplexer> (() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

## <a name="azure-cache-for-redis-vnet-faq"></a>Redsıs VNet için Azure önbelleği hakkında SSS
Aşağıdaki liste, Redsıs ölçeklendirmesi için Azure önbelleği hakkında sık sorulan soruların yanıtlarını içerir.

* Redsıs ve VNET 'ler için Azure önbelleğindeki bazı yaygın yanlış yapılandırma sorunları nelerdir?
* [Önbelleğim VNET 'te çalıştığını nasıl doğrulayabilirim?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* Bir VNET 'te Redsıs için Azure Önbelleğim 'e bağlanmaya çalışırken, neden uzak sertifika geçersiz olduğunu belirten bir hata alıyorum?
* [Standart veya temel önbellek ile sanal ağları kullanabilir miyim?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* Bazı alt ağlardaki Reda için Azure önbelleği neden başarısız oluyor, ancak başkaları tarafından kullanılamaz mi?
* [Alt ağ adres alanı gereksinimleri nelerdir?](#what-are-the-subnet-address-space-requirements)
* [VNET 'te önbellek barındırırken tüm önbellek özellikleri çalışıyor mu?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-vnets"></a>Redsıs ve VNET 'ler için Azure önbelleğindeki bazı yaygın yanlış yapılandırma sorunları nelerdir?
Redin için Azure önbelleği bir sanal ağda barındırılıyorsa, aşağıdaki tablolardaki bağlantı noktaları kullanılır. 

>[!IMPORTANT]
>Aşağıdaki tablolardaki bağlantı noktaları engellenirse, önbellek düzgün çalışmayabilir. Bu bağlantı noktalarından biri veya daha fazlası engelleniyorsa, bir sanal ağda redin için Azure önbelleği kullanılırken en sık karşılaşılan yanlış yapılandırma sorunu vardır.
> 
> 

- [Giden bağlantı noktası gereksinimleri](#outbound-port-requirements)
- [Gelen bağlantı noktası gereksinimleri](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>Giden bağlantı noktası gereksinimleri

Dokuz giden bağlantı noktası gereksinimi vardır. Bu aralıklardaki giden istekler, önbelleğe alma için gereken diğer hizmetlere giden bağlantı veya düğümler arası iletişim için redin alt ağına iç bağlantı sağlar. Coğrafi çoğaltma için, birincil ve çoğaltma önbelleğinin alt ağları arasındaki iletişim için ek giden gereksinimler vardır.

| Bağlantı noktaları | Yön | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Outbound |TCP |Azure depolama/PKI (Internet) üzerinde redsıs bağımlılıkları | (Redsıs alt ağı) |* |
| 443 | Outbound | TCP | Azure Key Vault redsıs bağımlılığı | (Redsıs alt ağı) | AzureKeyVault <sup>1</sup> |
| 53 |Outbound |TCP/UDP |DNS 'de redsıs bağımlılıkları (Internet/VNet) | (Redsıs alt ağı) | 168.63.129.16 ve 169.254.169.254 <sup>2</sup> ve alt ağ <sup>3</sup> için özel DNS sunucusu |
| 8443 |Outbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) | (Redsıs alt ağı) |
| 10221-10231 |Outbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) | (Redsıs alt ağı) |
| 20226 |Outbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı) |
| 13000-13999 |Outbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı) |
| 15000-15999 |Outbound |TCP |Redsıs ve Geo-Replication iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı) (Coğrafi çoğaltma eş alt ağı) |
| 6379-6380 |Outbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı) |

<sup>1</sup> ' AzureKeyVault ' hizmet etiketini Kaynak Yöneticisi ağ güvenlik grupları ile birlikte kullanabilirsiniz.

<sup>2</sup> Microsoft 'un sahip olduğu bu IP adresleri, Azure DNS hizmet veren ana bilgisayar VM 'sini ele almak için kullanılır.

özel DNS sunucusu olmayan alt ağlar için <sup>3</sup> veya özel DNS 'yi yoksayarak daha yeni redsıs önbellekler gerekmez.

#### <a name="geo-replication-peer-port-requirements"></a>Coğrafi çoğaltma eş bağlantı noktası gereksinimleri

Azure sanal ağlarında bulunan önbellekler arasında coğrafi çoğaltma kullanıyorsanız, lütfen alt ağdaki tüm çoğaltma bileşenlerinin, gelecekteki bir coğrafi Yük devretme durumunda bile birbirleriyle doğrudan iletişim kurabilmesi için, önerilen yapılandırmanın, her iki önbellekle gelen ve giden yönlerdeki 15000-15999 bağlantı noktalarının engellemesini kaldırmanız gerektiğini unutmayın.

#### <a name="inbound-port-requirements"></a>Gelen bağlantı noktası gereksinimleri

Sekiz gelen bağlantı noktası aralığı gereksinimi vardır. Bu aralıklardaki gelen istekler, aynı VNET 'te barındırılan diğer hizmetlerden veya Redsıs alt ağı iletişimlerinde iç yollardır.

| Bağlantı noktaları | Yön | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Inbound |TCP |Redsıs ile istemci iletişimi, Azure Yük Dengeleme | (Redsıs alt ağı) | (Redsıs alt ağı), sanal ağ, Azure Load Balancer <sup>1</sup> |
| 8443 |Inbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı) |
| 8500 |Inbound |TCP/UDP |Azure yük dengeleme | (Redsıs alt ağı) |Azure Load Balancer |
| 10221-10231 |Inbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı), Azure Load Balancer |
| 13000-13999 |Inbound |TCP |Redsıs kümelerine istemci iletişimi, Azure Yük Dengelemesi | (Redsıs alt ağı) |Sanal ağ, Azure Load Balancer |
| 15000-15999 |Inbound |TCP |Redsıs kümelerine istemci iletişimi, Azure Yük Dengelemesi ve Geo-Replication | (Redsıs alt ağı) |Sanal ağ, Azure Load Balancer, (coğrafi çoğaltma eş alt ağı) |
| 16001 |Inbound |TCP/UDP |Azure yük dengeleme | (Redsıs alt ağı) |Azure Load Balancer |
| 20226 |Inbound |TCP |Redsıs iç iletişimleri | (Redsıs alt ağı) |(Redsıs alt ağı) |

<sup>1</sup> NSG kurallarını yazmak Için ' AzureLoadBalancer ' (Kaynak Yöneticisi) hizmet etiketini (veya klasik için ' AZURE_LOADBALANCER ') kullanabilirsiniz.

#### <a name="additional-vnet-network-connectivity-requirements"></a>Ek VNET ağ bağlantısı gereksinimleri

Redsıs için Azure önbelleği için bir sanal ağda karşılanmamış olabilecek ağ bağlantısı gereksinimleri vardır. Redo için Azure Cache, bir sanal ağ içinde kullanıldığında aşağıdaki öğelerin tümünün düzgün çalışmasını gerektirir.

* Dünya çapındaki Azure depolama uç noktalarına giden ağ bağlantısı. Bu, Redsıs örneği için Azure önbelleği ile aynı bölgede bulunan uç noktaları ve **diğer** Azure bölgelerinde bulunan depolama uç noktalarını içerir. Azure depolama uç noktaları şu DNS etki alanları altında çözümlenir: *Table.Core.Windows.net*, *BLOB.Core.Windows.net*, *Queue.Core.Windows.net*ve *File.Core.Windows.net*. 
* *OCSP.msocsp.com*, *mscrl.Microsoft.com*ve *CRL.Microsoft.com*giden ağ bağlantısı. Bu bağlantı, TLS/SSL işlevselliğini desteklemek için gereklidir.
* Sanal ağın DNS yapılandırması, önceki noktalarda bahsedilen tüm uç noktaları ve etki alanlarını çözebilme yeteneğine sahip olmalıdır. Bu DNS gereksinimleri, sanal ağ için yapılandırılmış ve korunan geçerli bir DNS altyapısının sağlanması sağlanarak karşılanacaktır.
* Aşağıdaki Azure Izleme uç noktalarına giden ağ bağlantısı: shoebox2-black.shoebox2.metrics.nsatc.net, north-prod2.prod2.metrics.nsatc.net, azglobal-black.azglobal.metrics.nsatc.net, shoebox2-red.shoebox2.metrics.nsatc.net, east-prod2.prod2.metrics.nsatc.net, azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Önbelleğim VNET 'te çalıştığını nasıl doğrulayabilirim?

>[!IMPORTANT]
>VNET 'te barındırılan Redsıs örneği için bir Azure önbelleğine bağlanırken, önbellek istemcileriniz aynı VNET 'te veya aynı Azure bölgesinde sanal VNET eşlemesi etkinleştirilmiş bir sanal ağda olmalıdır. Genel Sanal Ağ Eşleme Şu anda desteklenmiyor. Buna tüm test uygulamaları veya tanılama ile ping araçları dahildir. İstemci uygulamasının nerede barındırıldığından bağımsız olarak, ağ güvenlik gruplarının, istemcinin ağ trafiğinin Redsıs örneğine erişmesine izin verilecek şekilde yapılandırılması gerekir.
>
>

Bağlantı noktası gereksinimleri önceki bölümde açıklandığı gibi yapılandırıldıktan sonra, aşağıdaki adımları gerçekleştirerek önbelleğinizin çalıştığını doğrulayabilirsiniz.

- Tüm önbellek düğümlerini [yeniden başlatın](cache-administration.md#reboot) . Tüm gerekli önbellek bağımlılıklarına ulaşılamadığından ( [gelen bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#inbound-port-requirements) ve [giden bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#outbound-port-requirements)bölümünde belirtildiği gibi), önbellek başarıyla yeniden başlatılabilir.
- Önbellek düğümleri yeniden başlatıldıktan sonra (Azure portal önbellek durumu tarafından raporlanarak), aşağıdaki testleri gerçekleştirebilirsiniz:
  - [tcpıng](https://www.elifulkerson.com/projects/tcping.php)kullanarak önbellek uç noktasına (6380 numaralı bağlantı noktasını kullanarak) önbellek ile aynı VNET içinde olan bir makineden ping gönderin. Örneğin:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Araç, `tcping` bağlantı noktasının açık olduğunu bildirirse, önbellek, VNET 'teki istemcilerden bağlantı için kullanılabilir.

  - Test etmenin başka bir yolu da, önbelleğe bağlanan ve önbellekten bazı öğeleri ekleyen ve alan bir test önbelleği istemcisi (StackExchange. Redsıs kullanan basit bir konsol uygulaması olabilir) oluşturmaktır. Örnek istemci uygulamasını, önbellek ile aynı VNET 'teki bir sanal makineye yükler ve önbelleğe bağlantıyı doğrulamak için çalıştırın.


### <a name="when-trying-to-connect-to-my-azure-cache-for-redis-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid"></a>Bir VNET 'te Redsıs için Azure Önbelleğim 'e bağlanmaya çalışırken, neden uzak sertifika geçersiz olduğunu belirten bir hata alıyorum?

Bir sanal ağ içinde redin için bir Azure önbelleğine bağlanmaya çalışırken şöyle bir sertifika doğrulama hatası görürsünüz:

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

Nedeni, ana bilgisayara IP adresiyle bağlanıyorsunuz olabilir. Ana bilgisayar adı kullanmanızı öneririz. Diğer bir deyişle, aşağıdakileri kullanın:     

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Aşağıdaki bağlantı dizesine benzer IP adresini kullanmaktan kaçının:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

DNS adını çözemezseniz, bazı istemci kitaplıkları `sslHost` StackExchange. redsıs istemcisi tarafından sağlanacak şekilde yapılandırma seçeneklerini içerir. Bu, sertifika doğrulama için kullanılan ana bilgisayar adını geçersiz kılmanızı sağlar. Örneğin:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Standart veya temel önbellek ile sanal ağları kullanabilir miyim?
VNET 'ler yalnızca Premium önbellekler ile kullanılabilir.

### <a name="why-does-creating-an-azure-cache-for-redis-fail-in-some-subnets-but-not-others"></a>Bazı alt ağlardaki Reda için Azure önbelleği neden başarısız oluyor, ancak başkaları tarafından kullanılamaz mi?
Redsıs for Kaynak Yöneticisi VNet 'e yönelik bir Azure önbelleği dağıtıyorsanız, önbelleğin başka kaynak türü içermeyen bir ayrılmış alt ağda olması gerekir. Redsıs için Azure önbelleğini diğer kaynakları içeren bir Kaynak Yöneticisi VNet alt ağına dağıtmaya yönelik bir girişim yapılırsa, dağıtım başarısız olur. Redsıs için yeni bir Azure önbelleği oluşturabilmeniz için, alt ağ içindeki mevcut kaynakları silmeniz gerekir.

Birden çok kaynak türünü klasik VNet 'e, yeterli sayıda IP adresiniz varsa dağıtabilirsiniz.

### <a name="what-are-the-subnet-address-space-requirements"></a>Alt ağ adres alanı gereksinimleri nelerdir?
Azure, bazı IP adreslerini her alt ağ içinde ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumu için ayrılmıştır ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için bkz. [Bu alt AĞLARDAKI IP adreslerini kullanma konusunda herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure VNET altyapısı tarafından kullanılan IP adreslerine ek olarak, alt ağdaki her reddo örneği, yük dengeleyici için bir parça başına iki IP adresi ve bir ek IP adresi kullanır. Kümelenmemiş bir önbelleğin bir parça olduğu kabul edilir.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>VNET 'te önbellek barındırırken tüm önbellek özellikleri çalışıyor mu?
Önbelleğiniz bir sanal ağın parçası olduğunda, yalnızca VNET 'teki istemciler önbelleğe erişebilir. Sonuç olarak, aşağıdaki önbellek yönetimi özellikleri şu anda çalışmaz.

* Redsıs konsolu-Redsıs konsolu, sanal ağ dışında bir yerel tarayıcınızda çalıştığından, önbelleğinize bağlanamaz.


## <a name="use-expressroute-with-azure-cache-for-redis"></a>Redsıs için Azure Cache ile ExpressRoute kullanma

Müşteriler, [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı hattını sanal ağ altyapısına bağlanarak şirket Içi ağınızı Azure 'a genişletmelerini sağlayabilir. 

Varsayılan olarak, yeni oluşturulan bir ExpressRoute devresi, sanal ağ üzerinde Zorlamalı tünel (varsayılan bir yol 0.0.0.0/0) gerçekleştirmez. Sonuç olarak, doğrudan VNET 'ten giden Internet bağlantısına izin verilir ve istemci uygulamaları Redsıs için Azure önbelleği dahil diğer Azure uç noktalarına bağlanabilir.

Bununla birlikte, genel bir müşteri yapılandırması, giden Internet trafiğini şirket içi akışa yönelden zorlayan Zorlamalı tünel oluşturma (varsayılan rota tanıtma) kullanmaktır. Bu trafik akışı Reda için Azure Cache ile bağlantıyı keser ve ardından, redin örneği için Azure önbelleğinin bağımlılıklarıyla iletişim kuramaması gibi şirket içi trafik engellenir.

Çözüm, redin için Azure önbelleğini içeren alt ağda bir (veya daha fazla) Kullanıcı tanımlı yol (UDRs) tanımlamak içindir. UDR, varsayılan yol yerine kabul edilecek alt ağa özgü yolları tanımlar.

Mümkünse, aşağıdaki yapılandırmanın kullanılması önerilir:

* ExpressRoute yapılandırması 0.0.0.0/0 duyurur ve varsayılan olarak, Şirket içindeki tüm giden trafiği tünellere zorlar.
* Reds için Azure önbelleğini içeren alt ağa uygulanan UDR, genel İnternet 'e TCP/IP trafiği için çalışan bir rota ile 0.0.0.0/0 tanımlar; Örneğin, bir sonraki atlama türünü ' Internet ' olarak ayarlayarak.

Bu adımların Birleşik etkisi, alt ağ düzeyi UDR 'nin ExpressRoute zorlamalı tünelden öncelikli olmasını sağlar ve bu sayede redin için Azure önbelleğinden giden Internet erişimi sağlar.

ExpressRoute kullanarak bir şirket içi uygulamadan Redsıs örneği için Azure önbelleğine bağlanmak, performans nedenleriyle tipik bir kullanım senaryosu değildir (Redsıs istemcileri için en iyi performans için Azure önbelleğinin Redsıs için Azure önbelleğiyle aynı bölgede olması gerekir).

>[!IMPORTANT] 
>Bir UDR 'de tanımlanan yolların, ExpressRoute yapılandırması tarafından tanıtılan tüm yollarla öncelikli **olması gerekir** . Aşağıdaki örnek, büyük 0.0.0.0/0 adres aralığını kullanır ve bu nedenle daha belirli adres aralıkları kullanılarak yol tanıtımlarının yanlışlıkla geçersiz kılınması olabilir.

>[!WARNING]  
>Redsıs için Azure önbelleği, **genel eşleme yolundan özel eşleme yoluna yönelik yolların yanlışlıkla çapraz bir şekilde tanıtıldığı**ExpressRoute yapılandırmalarında desteklenmez. Ortak eşleme yapılandırılmış ExpressRoute yapılandırmalarında, büyük bir Microsoft Azure IP adresi aralığı kümesi için Microsoft 'tan yol tanıtımları alın. Bu adres aralıkları özel eşleme yolunda yanlış bir şekilde bildiriliyorsa, sonuç Redsıs örneği için Azure önbelleğindeki tüm giden ağ paketlerinin bir müşterinin Şirket içi ağ altyapısına yanlışlıkla zorla tünellemesini sağlar. Bu ağ akışı Redsıs için Azure önbelleğini keser. Bu sorunun çözümü, genel eşleme yolundan özel eşleme yoluna yönelik çapraz reklam yollarını durdurmaktır.


Bu [genel bakışta](../virtual-network/virtual-networks-udr-overview.md)Kullanıcı tanımlı yollarla ilgili arka plan bilgileri bulabilirsiniz.

ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Sonraki adımlar
Redsıs özellikleri için Azure önbelleği hakkında daha fazla bilgi edinin.

* [Redsıs Premium hizmet katmanları için Azure önbelleği](cache-overview.md#service-tiers)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png
