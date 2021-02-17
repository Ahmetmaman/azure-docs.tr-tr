---
title: Windows Güvenliği olay verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Windows sistemlerinizdeki tüm güvenlik olaylarını Azure Sentinel çalışma alanınıza akışa almak için güvenlik olayları bağlayıcısını kullanmayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2020
ms.author: yelevin
ms.openlocfilehash: bc75ee64174957ad6486146b4da6f8a66a2120e5
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100570551"
---
# <a name="connect-windows-security-events"></a>Windows güvenlik olaylarını bağlama 

Güvenlik olayları Bağlayıcısı, Windows sistemlerinizden (sunucular ve iş istasyonları, fiziksel ve sanal) tüm güvenlik olaylarını Azure Sentinel çalışma alanınıza aktarmanıza olanak tanır. Bu, panolarınızda Windows Güvenlik olaylarını görüntülemenize, bunları özel uyarılar oluşturmakta kullanmanıza ve araştırmalarınızı geliştirmek için bunları kullanarak kuruluşunuzun ağına daha fazla bilgi vererek ve güvenlik işlemleri yeteneklerini genişletmenize olanak sağlar. Hangi olayların akışının aşağıdaki kümeler arasından seçim yapabilirsiniz: <a name="event-sets"></a>

- **Tüm olaylar** -tüm Windows güvenliği ve AppLocker olayları.
- **Ortak** -denetim amaçlarıyla standart bir olay kümesi. Bu küme, tam Kullanıcı denetim izi içerir. Örneğin, Kullanıcı oturum açma ve Kullanıcı oturumu kapatma olaylarını (olay kimlikleri 4624, 4634) içerir. Ayrıca, güvenlik grubu değişiklikleri, anahtar etki alanı denetleyicisi Kerberos işlemleri ve diğer olay türleri gibi Denetim eylemleri, kabul edilen en iyi yöntemlere sahiptir.

    **Ortak** olay kümesi, bu yüzden yaygın olmayan bazı olay türlerini içerebilir.  Bunun nedeni, **yaygın** olarak ayarlanan ana noktanın, olayların hacminin daha yönetilebilir bir düzeye azaltılmasından kaynaklanır, ancak tam denetim izi yeteneklerini sürdürmektedir.

- **Minimal** -olası tehditleri gösterebilen küçük bir olay kümesi. Bu küme tam denetim izi içermiyor. Yalnızca başarılı bir ihlal olduğunu gösterebilen olayları ve çok düşük bir oluşma oranına sahip diğer önemli olayları ele alır. Örneğin, başarılı ve başarısız Kullanıcı oturum açmaları (olay kimlikleri 4624, 4625) içerir, ancak denetim için önemli olan, ihlal algılama için anlamlı olmayan ve görece yüksek hacimde bir oturum açma bilgisi (4634) içermez. Bu küme veri hacminin çoğu, oturum açma olayları ve işlem oluşturma olaylarından oluşur (olay KIMLIĞI 4688).

- **Hiçbiri** -güvenlik veya AppLocker olayı yok. (Bu ayar, bağlayıcıyı devre dışı bırakmak için kullanılır.)

    Aşağıdaki listede her bir küme için güvenlik ve uygulama dolabı olay kimliklerinin tamamen bir dökümü verilmiştir:

    | Olay kümesi | Toplanan olay kimlikleri |
    | --- | --- |
    | **En az** | 1102, 4624, 4625, 4657, 4663, 4688, 4700, 4702, 4719, 4720, 4722, 4723, 4724, 4727, 4728, 4732, 4735, 4737, 4739, 4740, 4754, 4755, 4756, 4767, 4799, 4825, 4946, 4948, 4956, 5024, 5033, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222 |
    | **Common** | 1, 299, 300, 324, 340, 403, 404, 410, 411, 412, 413, 431, 500, 501, 1100, 1102, 1107, 1108, 4608, 4610, 4611, 4614, 4622, 4624, 4625, 4634, 4647, 4648, 4649, 4657, 4661, 4662, 4663, 4665, 4666, 4667, 4688, 4670, 4672, 4673, 4674, 4675, 4689, 4697, 4700, 4702, 4704, 4705, 4716, 4717, 4718, 4719, 4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729 , 4733, 4732, 4735, 4737, 4738, 4739, 4740, 4742, 4744, 4745, 4746, 4750, 4751, 4752, 4754, 4755, 4756, 4757, 4760, 4761, 4762, 4764, 4767, 4768, 4771, 4774, 4778, 4779, 4781, 4793, 4797, 4798, 4799, 4800, 4801, 4802, 4803, 4825, 4826, 4870, 4886, 4887, 4888, 4893, 4898, 4902, 4904, 4905, 4907, 4931, 4932, 4933, 4946, 4948, 4956, 4985, 5024, 5033, 5059, 5136, 5137 , 5140, 5145, 5632, 6144, 6145, 6272, 6273, 6278, 6416, 6423, 6424, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222, 26401, 30004 |

> [!NOTE]
> Tek bir çalışma alanı bağlamı içindeki güvenlik olayları koleksiyonu, Azure Güvenlik Merkezi 'nden veya Azure Sentinel 'ten yapılandırılabilir, ancak her ikisiyle de ayarlanamaz. Azure Güvenlik Merkezi 'nden Azure Defender uyarılarını zaten alan bir çalışma alanında Azure Sentinel 'i oluşturuyorsanız ve güvenlik olaylarını toplamak üzere ayarlandıysa, iki seçeneğiniz vardır:
> - Azure Güvenlik Merkezi 'nde güvenlik olayları koleksiyonunu olduğu gibi bırakın. Bu olayları Azure Sentinel 'de ve Azure Defender 'da sorgulayabilir ve analiz edebilirsiniz. Bununla birlikte, bağlayıcının bağlantı durumunu izleyebilmeyecek veya Azure Sentinel 'de yapılandırmasını değiştiremeyeceksiniz. Bu sizin için önemliyse ikinci seçeneği göz önünde bulundurun.
>
> - Azure Güvenlik Merkezi 'nde [güvenlik olayları koleksiyonunu devre dışı bırakın](../security-center/security-center-enable-data-collection.md) ve yalnızca Azure Sentinel 'de güvenlik olayları bağlayıcısını ekleyin. İlk seçenekte olduğu gibi, hem Azure Sentinel hem de Azure Defender/ASC içindeki olayları sorgulayabilir ve çözümleyebilirsiniz, ancak artık bağlayıcının bağlantı durumunu izleyebilir ya da yalnızca Azure Sentinel 'de ve yapılandırmasını değiştirebilirsiniz.

## <a name="set-up-the-windows-security-events-connector"></a>Windows güvenlik olayları bağlayıcısını ayarlama

Azure Sentinel 'de Windows Güvenlik olaylarınızı toplamak için:

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin. Bağlayıcılar listesinden **güvenlik olayları**' na tıklayın ve ardından sağ alt köşedeki **bağlayıcı sayfası aç** düğmesine tıklayın. Daha sonra, bu bölümün geri kalanında açıklandığı şekilde **yönergeler** sekmesi altındaki ekrandaki yönergeleri uygulayın.

1. Bağlayıcı sayfasındaki **Önkoşullar** bölümünde açıklandığı gibi uygun izinlere sahip olduğunuzu doğrulayın.

1. Azure Sentinel 'de güvenlik olaylarını akışa almak istediğiniz makinelere [Log Analytics aracısını](../azure-monitor/agents/log-analytics-agent.md) (Microsoft Monitoring Agent veya MMA olarak da bilinir) indirip yükleyin.

    Azure sanal makineleri için:
    
    1. **Aracıyı Azure Windows sanal makinesine** ve ardından aşağıda görüntülenen bağlantıya tıklayın.
    1. Bağlamak istediğiniz her bir sanal makine için, sağ tarafta görüntülenen listede adına tıklayın ve ardından **Bağlan**' a tıklayın.

    Azure olmayan Windows makineleri (fiziksel, sanal şirket içi veya başka bir buluttaki sanal) için:

    1. **Azure olmayan Windows makinesine aracıyı** ve ardından aşağıda görüntülenen bağlantıyı seçin.
    1. Sağ tarafta, **Windows bilgisayarları** altında görüntülenen uygun indirme bağlantılarına tıklayın.
    1. İndirilen yürütülebilir dosyayı kullanarak, istediğiniz Windows sistemlerine aracıyı yükleyin ve yukarıda bahsedilen indirme bağlantılarının altında görüntülenen **çalışma alanı kimliğini ve anahtarları** kullanarak yapılandırın.

    > [!NOTE]
    >
    > Gerekli internet bağlantısı olmadan Windows sistemlerine, olayları Azure Sentinel 'e akışa almaya devam etmek için, alt sağ taraftaki bağlantıyı kullanarak bir proxy görevi görecek şekilde **OMS ağ geçidini** ayrı bir makineye indirip yükleyin.  Olaylarını toplamak istediğiniz her Windows sistemine de Log Analytics aracısını yüklemeniz gerekir.
    >
    > Bu senaryo hakkında daha fazla bilgi için [ **Log Analytics ağ geçidi** belgelerine](../azure-monitor/agents/gateway.md)bakın.

    Ek yükleme seçenekleri ve daha fazla ayrıntı için [ **Log Analytics aracı** belgelerine](../azure-monitor/agents/agent-windows.md)bakın.

1. Akışa almak istediğiniz olay kümesini ([Tümü, ortak veya en az](#event-sets)) seçin.

1. **Güncelleştir**’e tıklayın.

1. Windows güvenlik olayları için Log Analytics ilgili şemayı kullanmak için, `SecurityEvent` sorgu penceresine yazın.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakika kadar sürebilir. 

### <a name="configure-the-security-events-connector-for-anomalous-rdp-login-detection"></a>Anormal RDP oturum açma algılaması için güvenlik olayları bağlayıcısını yapılandırma

> [!IMPORTANT]
> Anomalous RDP oturum açma algılaması Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel, anormal Uzak Masaüstü Protokolü (RDP) oturum açma etkinliğini belirlemek için, güvenlik olayları verilerine makine öğrenimi (ML) uygulayabilir. Senaryolar şunlardır:

- **Olağan DıŞı IP** -IP adresi nadiren veya son 30 gün içinde hiç gözlemlenmedi

- **Olağan dışı coğrafi konum** -IP adresi, şehir, ülke ve ASN nadiren veya son 30 gün içinde hiç gözlemlenmedi

- **Yeni Kullanıcı** -yeni bir Kullanıcı, bir IP adresi ve coğrafi konumdan oturum açtığında, her ikisinin de 30 günden önceki verilere göre görülmemelidir.

**Yapılandırma yönergeleri**

1. **Güvenlik olayları** veri BAĞLAYıCıSı aracılığıyla RDP oturum açma verilerini (olay kimliği 4624) toplamalısınız. Azure Sentinel 'de akışı yapmak için "hiçbiri" nin yanı sıra bir [olay kümesini](#event-sets) seçtiğinizden emin olun.

1. Azure Sentinel portalından **analiz**' e ve ardından **kural şablonları** sekmesine tıklayın. **(Önizleme) anormal RDP oturum açma algılaması** kuralını seçin ve **durum** kaydırıcısını **etkin** olarak taşıyın.

    > [!NOTE]
    > Makine öğrenimi algoritması, Kullanıcı davranışının temel profilini oluşturmak için 30 günlük veri gerektirdiğinden, herhangi bir olay algılanabilmesi için 30 günlük güvenlik olayları verilerinin toplanmasına izin vermelisiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Windows Güvenlik olaylarını Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Yerleşik](tutorial-detect-threats-built-in.md) veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri algılamaya başlayın.

