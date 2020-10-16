---
title: Azure Application Gateway 'de birden çok site barındırma
description: Bu makalede Azure Application Gateway çoklu site desteğine genel bir bakış sunulmaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 07/20/2020
ms.author: surmb
ms.topic: conceptual
ms.openlocfilehash: 53f6f37454de886934a483b40daad24204958baf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87474334"
---
# <a name="application-gateway-multiple-site-hosting"></a>Application Gateway birden çok site barındırma

Birden çok site barındırma, bir uygulama ağ geçidinin aynı bağlantı noktasında birden fazla Web uygulaması yapılandırmanızı sağlar. Bir uygulama ağ geçidine 100'den fazla web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Örneğin contoso.com, fabrikam.com ve adatum.com olmak üzere üç etki alanı, uygulama ağ geçidinin IP adresine işaret eder. Üç çoklu site dinleyicisi oluşturup her dinleyiciyi ilgili bağlantı noktası ve protokol ayarına göre yapılandırabilirsiniz. 

Ayrıca çoklu site dinleyicisinde joker karakter ana bilgisayar adı tanımlayabilir ve bunu dinleyici başına en fazla 5 ana bilgisayar adı için yapabilirsiniz. Daha fazla bilgi için bkz. [dinleyicide joker ana bilgisayar adları](#wildcard-host-names-in-listener-preview).

:::image type="content" source="./media/multiple-site-overview/multisite.png" alt-text="Çok siteli Application Gateway":::

> [!IMPORTANT]
> Kurallar, v1 SKU 'SU için portalda listelendikleri sırada işlenir. V2 SKU 'SU için, tam eşleşmeler daha yüksek önceliğe sahiptir. Temel dinleyiciyi yapılandırmadan önce çok siteli dinleyicileri yapılandırmanız önerilir.  Bu işlem, trafiğin doğru arka uca yönlendirilmesini güvence altına alır. Temel dinleyici listede ilk sıradaysa ve gelen bir istekle eşleşiyorsa, o dinleyici tarafından işlenir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı uygulama ağ geçidi dağıtımında aynı üst etki alanının birden çok alt etki alanını barındırabilirsiniz. Örneğin, `http://blog.contoso.com` `http://app.contoso.com` tek bir uygulama ağ geçidi dağıtımını barındırabilirsiniz.

## <a name="wildcard-host-names-in-listener-preview"></a>Dinleyicide joker karakter ana bilgisayar adları (Önizleme)

Application Gateway, çok siteli HTTP (ler) dinleyicisi kullanılarak ana bilgisayar tabanlı yönlendirmeye izin verir. Şimdi, ana bilgisayar adında yıldız işareti (*) ve soru işareti (?) gibi joker karakterler ve çok siteli HTTP (S) dinleyicisi başına en fazla 5 ana bilgisayar adı kullanabilirsiniz. Örneğin, `*.contoso.com`.

Ana bilgisayar adında bir joker karakter kullanarak, tek bir dinleyicide birden çok konak adını eşleştirebilirsiniz. Örneğin, `*.contoso.com` ve ile eşleştirebilir `ecom.contoso.com` `b2b.contoso.com` `customer1.b2b.contoso.com` . Bir ana bilgisayar adları dizisi kullanarak, istekleri bir arka uç havuzuna yönlendirmek için bir dinleyici için birden çok konak adı yapılandırabilirsiniz. Örneğin, bir dinleyici, `contoso.com, fabrikam.com` her iki konak adı için istekleri kabul edecek şekilde içerebilir.

:::image type="content" source="./media/multiple-site-overview/wildcard-listener-diag.png" alt-text="Çok siteli Application Gateway":::

>[!NOTE]
> Bu özellik önizleme aşamasındadır ve yalnızca Application Gateway Standard_v2 ve WAF_v2 SKU 'SU için kullanılabilir. Önizlemeler hakkında daha fazla bilgi edinmek için [buradaki kullanım koşullarına](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

>[!NOTE]
>Bu özellik şu anda yalnızca [Azure PowerShell](tutorial-multiple-sites-powershell.md) ve [Azure CLI](tutorial-multiple-sites-cli.md)üzerinden kullanılabilir. Portal desteği yakında geliyor.
> Lütfen portal desteği tam olarak kullanılamadığından, yalnızca konak adları parametresini kullanıyorsanız, dinleyici portalda temel bir dinleyici olarak görünür ve dinleyici listesi görünümünün ana bilgisayar adı sütununda yapılandırılan ana bilgisayar adları gösterilmez. Joker bir dinleyicide yapılan herhangi bir değişiklik için, portalda desteklenene kadar Azure PowerShell veya CLı kullandığınızdan emin olun.

[Azure PowerShell](tutorial-multiple-sites-powershell.md), yerine kullanmanız gerekir `-HostNames` `-HostName` . Ana bilgisayar adları ile, virgülle ayrılmış değerler olarak 5 adede kadar konak adı belirtebilir ve joker karakterler kullanabilirsiniz. Örneğin, `-HostNames "*.contoso.com,*.fabrikam.com"`

[Azure CLI](tutorial-multiple-sites-cli.md)'de yerine kullanmanız gerekir `--host-names` `--host-name` . Ana bilgisayar adları ile, en fazla 5 ana bilgisayar adını virgülle ayrılmış değerler olarak belirtebilir ve joker karakterler kullanabilirsiniz. Örneğin, `--host-names "*.contoso.com,*.fabrikam.com"`

### <a name="allowed-characters-in-the-host-names-field"></a>Ana bilgisayar adları alanında izin verilen karakterler:

* `(A-Z,a-z,0-9)` -alfasayısal karakterler
* `-` -kısa çizgi veya eksi
* `.` -ayırıcı olarak dönem
*   `*` -izin verilen aralıkta birden çok karakterle eşleştirebilir
*   `?` -izin verilen aralıktaki tek bir karakterle eşleştirebilir

### <a name="conditions-for-using-wildcard-characters-and-multiple-host-names-in-a-listener"></a>Bir dinleyicide joker karakter ve birden çok konak adı kullanma koşulları:

*   Tek bir dinleyicide yalnızca 5 ana bilgisayar adı belirtebilirsiniz
*   Yıldız işareti `*` , bir etki alanı stili adının veya ana bilgisayar adının bir bileşeninde yalnızca bir kez belirtilebilir. Örneğin, Component1 *. component2*. component3. `(*.contoso-*.com)` geçerli.
*   Ana bilgisayar adında yalnızca en fazla iki yıldız olabilir `*` . Örneğin, `*.contoso.*` geçerlidir ve `*.contoso.*.*.com` geçersizdir.
*   Ana bilgisayar adında yalnızca en fazla 4 joker karakter olabilir. Örneğin, `????.contoso.com` geçerlidir, `w??.contoso*.edu.*` ancak `????.contoso.*` geçersizdir.
*   `*` `?` Ana bilgisayar adının (veya veya) bir bileşeninde birlikte yıldız işareti ve soru işareti kullanılması `*?` `?*` `**` geçersizdir. Örneğin, `*?.contoso.com` ve `**.contoso.com` geçersizdir.

### <a name="considerations-and-limitations-of-using-wildcard-or-multiple-host-names-in-a-listener"></a>Bir dinleyicide joker veya birden çok konak adı kullanmanın konuları ve sınırlamaları:

*   [SSL sonlandırma ve uçtan uca SSL](ssl-overview.md) , protokolü HTTPS olarak yapılandırmanızı ve dinleyici yapılandırmasında kullanılacak bir sertifikayı yüklemenizi gerektirir. Çok siteli bir dinleyicide, ana bilgisayar adını da girebilirsiniz, genellikle bu, SSL sertifikasının CN 'si olur. Dinleyicide birden çok konak adı belirtirken veya joker karakter kullandığınızda, aşağıdakileri göz önünde bulundurmanız gerekir:
    *   Bu, *. contoso.com gibi bir joker karakter ana bilgisayar adı ise, CN gibi bir joker karakter sertifikası yüklemeniz gerekir. contoso.com
    *   Aynı dinleyicide birden çok konak adı bahsedildiğinde, belirtilen ana bilgisayar adlarıyla eşleşen CNs ile bir SAN sertifikası (konu alternatif adları) yüklemeniz gerekir.
*   Ana bilgisayar adından bahsetmek için normal bir ifade kullanamazsınız. Ana bilgisayar adı modelini oluşturmak için yalnızca yıldız işareti (*) ve soru işareti (?) gibi joker karakterler kullanabilirsiniz.
*   Arka uç sistem durumu denetimi için, HTTP ayarları başına birden çok [özel yoklamaları](application-gateway-probe-overview.md) ilişkilendiremezsiniz. Bunun yerine, arka uçta Web sitelerinden birini araştırın veya arka uç sunucusunun localhost 'u araştırmanız için "127.0.0.1" kullanabilirsiniz. Ancak, bir dinleyicide joker karakter veya birden çok konak adı kullanırken, belirtilen tüm etki alanı desenlerinin istekleri, kural türüne (temel veya yol tabanlı) bağlı olarak arka uç havuzuna yönlendirilir.
*   "Hostname" özellikleri bir dizeyi giriş olarak alır; burada yalnızca bir joker karakter olmayan etki alanı adından bahsetmeniz ve "ana bilgisayar adları" girdi olarak bir dize dizisi alır, burada 5 joker karakter etki alanı adlarından bahsetmeniz yeterlidir. Ancak her iki özellik de aynı anda kullanılamaz.
*   Joker karakter veya birden çok konak adı kullanan bir hedef dinleyicisi ile [yeniden yönlendirme](redirect-overview.md) kuralı oluşturamazsınız.

Çok siteli bir dinleyicide joker ana bilgisayar adlarını yapılandırma hakkında adım adım kılavuz için bkz. [Azure PowerShell kullanarak birden çok site oluşturma](tutorial-multiple-sites-powershell.md) veya [Azure CLI 'yi kullanma](tutorial-multiple-sites-cli.md) .

## <a name="host-headers-and-server-name-indication-sni"></a>Barındırma üstbilgileri ve Sunucu Adı Belirtme (SNI)

Aynı altyapıda birden çok site barındırmayı etkinleştirmek için üç yaygın mekanizma bulunur.

1. Birden çok web uygulamasının her birini benzersiz bir IP adresinde barındırın.
2. Birden çok web uygulamasını aynı IP adresinde barındırmak için ana bilgisayar adını kullanın.
3. Birden çok web uygulamasını aynı IP adresinde barındırmak için farklı bağlantı noktalarını kullanın.

Şu anda Application Gateway trafiği dinlediği tek bir genel IP adresini destekler. Bu nedenle, her biri kendi IP adresine sahip olan birden çok uygulama desteklenmemektedir. 

Application Gateway, her biri farklı bağlantı noktalarında dinleyen birden çok uygulamayı destekler, ancak bu senaryo uygulamaların standart olmayan bağlantı noktalarında trafiği kabul etmesini gerektirir. Bu, genellikle istediğiniz bir yapılandırma değildir.

Application Gateway, aynı genel IP adresinde ve bağlantı noktasında birden çok web sitesini barındırmak için HTTP 1.1 barındırma bilgilerini kullanır. Uygulama ağ geçidinde barındırılan siteler Sunucu Adı Belirtme (SNı) TLS uzantılı TLS yük boşaltma 'yı da destekleyebilir. Bu senaryo, istemci tarayıcısının ve arka uç web grubunun RFC 6066’da belirtildiği gibi HTTP/1.1 ve TLS uzantısını desteklemesi gerektiği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway 'de birden çok site barındırmayı yapılandırma hakkında bilgi edinin
* [Azure portalını kullanma](create-multiple-sites-portal.md)
* [Azure PowerShell’i kullanma](tutorial-multiple-sites-powershell.md) 
* [Azure CLI’yı kullanma](tutorial-multiple-sites-cli.md)

Uçtan uca şablon tabanlı dağıtım için [birden çok site barındırma kullanan Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) ziyaret edebilirsiniz.
