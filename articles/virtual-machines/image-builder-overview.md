---
title: Azure görüntü Oluşturucu (Önizleme) hakkında bilgi edinin
description: Azure 'da sanal makineler için Azure Image Builder hakkında daha fazla bilgi edinin.
author: danielsollondon
ms.author: danis
ms.date: 05/02/2019
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: image-builder
ms.reviewer: cynthn
ms.openlocfilehash: 1c70edfc3bad2be70d26c71736ca06fcc4a8dcdb
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101672505"
---
# <a name="preview-azure-image-builder-overview"></a>Önizleme: Azure Image Builder 'a genel bakış

Standartlaştırılmış sanal makine (VM) görüntüleri, kuruluşların buluta geçiş yapmasına ve dağıtımlarda tutarlılık sağlamanıza olanak tanır. Görüntüler genellikle önceden tanımlanmış güvenlik ve yapılandırma ayarlarını ve gerekli yazılımları içerir. Kendi görüntüleme işlem hattınızı ayarlamak için zaman, altyapı ve kurulum gerekir, ancak Azure VM Image Builder sayesinde görüntünüzü açıklayan basit bir yapılandırma sağlamanız, hizmete göndermeniz ve görüntünün oluşturulup dağıtılması sağlanır.
 
Azure VM görüntü Oluşturucu (Azure görüntü Oluşturucu), bir Windows veya Linux tabanlı Azure Marketi görüntüsü, mevcut özel görüntüler veya Red Hat Enterprise Linux (RHEL) ISO ile başlamanıza ve kendi özelleştirmelerinizi eklemeye başlamanızı sağlar. Görüntü Oluşturucu [HashiCorp Packer](https://packer.io/)üzerinde oluşturulduğundan, mevcut Packer kabuğu sağlayıcısı oluştur betiklerinizi de içeri aktarabilirsiniz. Ayrıca, [Azure Paylaşılan görüntü galerisinde](shared-image-galleries.md)bir yönetilen görüntü veya VHD olarak, görüntülerinizi nerede barındırıyorsanız istediğinizi belirtebilirsiniz.

> [!IMPORTANT]
> Azure görüntü Oluşturucu Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="preview-features"></a>Önizleme özellikleri

Önizleme için, bu özellikler desteklenir:

- En düşük güvenlik ve kurumsal yapılandırmalarınızı içeren altın taban çizgisi görüntülerinin oluşturulması ve departmanların ihtiyaçları için daha fazla özelleştirme yapmasına izin vermek.
- Var olan görüntülerin düzeltme eki uygulama, görüntü Oluşturucu var olan özel görüntüleri sürekli olarak yamanız için izin verir.
- Mevcut yapılandırma sunucularına (DSC, Chef, Pupevcil hayvan vb.), dosya paylaşımlarına veya diğer yönlendirilebilir sunuculara/hizmetlere bağlanabilmeniz için görüntü oluşturucuyu var olan sanal ağlarınıza bağlayın.
- Azure Paylaşılan görüntü Galerisi ile tümleştirme, görüntüleri küresel olarak dağıtmanıza, sürümüne ve ölçeklendirmenize olanak tanır ve size bir görüntü yönetim sistemi sağlar.
- Mevcut görüntü oluşturma işlem hatlarıyla tümleştirme, işlem hattınızdan görüntü Oluşturucu 'Yu çağırın veya basit önizleme resmi Oluşturucu Azure DevOps görevini kullanın.
- Mevcut bir görüntü özelleştirme işlem hattını Azure 'a geçirin. Görüntüleri özelleştirmek için mevcut betikleri, komutlarınızı ve işlemlerinizi kullanın.
- Azure Stack desteklemek için VHD biçimindeki görüntülerin oluşturulması.
 

## <a name="regions"></a>Bölgeler
Azure görüntü Oluşturucu hizmeti bu bölgelerde önizleme için kullanılabilir olacak. Görüntüler, bu bölgelerin dışına dağıtılabilir.
- Doğu ABD
- Doğu ABD 2
- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Kuzey Avrupa
- West Europe

## <a name="os-support"></a>İşletim sistemi desteği
AıB, Azure Marketi temel işletim sistemi görüntülerini destekleyecektir:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7,6, 7,7
- CentOS 7,6, 7,7
- SLES 12 SP4
- SLES 15, SLES 15 SP1
- Windows 10 RS5 kurumsal/kurumsal çoklu oturum/profesyonel
- Windows 2016
- Windows 2019

RHEL ISOs desteği artık desteklenmiyor.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure görüntü Oluşturucu, bir Azure Kaynak sağlayıcısı tarafından erişilebilen, tam olarak yönetilen bir Azure hizmetidir. Azure görüntü Oluşturucu işleminin üç ana bölümü vardır: kaynak, özelleştirme ve dağıtma, bunlar bir şablonda temsil edilir. Aşağıdaki diyagramda, bazı özellikleriyle birlikte bileşenler gösterilmektedir. 
 

**Image Builder işlemi** 

![Azure Image Builder işleminin kavramsal çizimi](./media/image-builder-overview/image-builder-process.png)

1. Görüntü şablonunu bir. JSON dosyası olarak oluşturun. Bu. JSON dosyası, görüntü kaynağı, özelleştirmeler ve dağıtım hakkındaki bilgileri içerir. [Azure Image Builder GitHub deposunda](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts)birden çok örnek vardır.
1. Bu hizmeti hizmetine gönderirseniz, belirttiğiniz kaynak grubunda bir görüntü şablonu yapıtı oluşturulur. Arka planda, görüntü Oluşturucu kaynak görüntüyü veya ISO ve komut dosyalarını gerektiği şekilde indirir. Bunlar, aboneliğinizde otomatik olarak oluşturulan ayrı bir kaynak grubunda depolanır: IT_ \<DestinationResourceGroup> _ \<TemplateName> . 
1. Görüntü şablonu oluşturulduktan sonra görüntüyü oluşturabilirsiniz. Arka plan resmi tasarımcısında, IT_ \<DestinationResourceGroup> _ kaynak grubunda BIR VM (varsayılan boyut: Standard_D1_v2), ağ, genel IP, NSG ve depolama oluşturmak için şablon ve kaynak dosyalarını kullanır \<TemplateName> .
1. Görüntü oluşturma işleminin bir parçası olarak, görüntü Oluşturucu görüntüyü şablona göre dağıtır, sonra \<DestinationResourceGroup> \<TemplateName> işlem için oluşturulan IT_ _ kaynak grubundaki ek kaynakları siler.


## <a name="permissions"></a>İzinler
(AıB) için kaydolmanız durumunda bu, AıB hizmetine bir hazırlama kaynak grubu (IT_ *) oluşturma, yönetme ve silme izni verir ve bu, görüntü derlemesi için gerekli olan kaynak ekleme haklarına sahip olur. Bu işlem, başarılı bir kayıt sırasında aboneliğinizde kullanılabilir hale getirilen bir AıB hizmet sorumlusu adı (SPN) tarafından yapılır.

Azure VM Image Builder 'ın görüntüleri yönetilen görüntülere veya paylaşılan bir görüntü galerisine dağıtmasını sağlamak için, görüntüleri okuma ve yazma izinlerine sahip Azure Kullanıcı tarafından atanan bir kimlik oluşturmanız gerekir. Azure depolama 'ya erişiyorsanız, bu durumda özel kapsayıcıları okumak için izinler gerekir.

Başlangıçta kimlik oluşturma hakkında [Azure Kullanıcı tarafından atanan yönetilen kimlik](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) belgelerini izlemeniz gerekir.

İzin vermek için gereken kimliğe sahip olduktan sonra, bunu yapmak için bir Azure özel rol tanımı kullanabilir ve ardından Kullanıcı tarafından atanan yönetilen kimliği özel rol tanımını kullanacak şekilde atayabilirsiniz.

İzinler [burada](https://github.com/danielsollondon/azvmimagebuilder/blob/master/aibPermissions.md#azure-vm-image-builder-permissions-explained-and-requirements)ayrıntılı olarak açıklanırlar ve örnekler bunun nasıl uygulandığını gösterir.

> [!Note]
> Daha önce AıB ile, AıB SPN 'yi kullanacaksınız ve görüntü kaynak gruplarına SPN izinleri vermelisiniz. Bu modelden daha sonraki yetenekler için izin vermek üzere taşınıyoruz. 26, 2020 Mayıs 'dan, görüntü Oluşturucu kullanıcı tarafından atanan bir kimliğe sahip olmayan şablonları kabul etmez, var olan şablonların bir [Kullanıcı kimliği](./linux/image-builder-json.md)ile hizmete yeniden gönderilmesi gerekir. Burada örnek olarak, Kullanıcı tarafından atanan bir kimlik oluşturma ve bunları bir şablona ekleme işlemlerinin nasıl yapılacağı gösterilmektedir. Daha fazla bilgi için lütfen bu [belgede bu belgeleri](https://github.com/danielsollondon/azvmimagebuilder#service-updates-and-latest-release-information) gözden geçirin ve güncelleştirmeleri yayınlar.

## <a name="costs"></a>Maliyetler
Azure Image Builder ile görüntü oluştururken, derlerken ve depolarken bazı işlem, ağ ve depolama maliyetlerine tabi olursunuz. Bu maliyetler, el ile özel görüntüler oluşturma bölümünde tahakkuk eden maliyetlere benzer. Kaynaklar için Azure ücretlerinizi ücretlendirilecektir. 

Görüntü oluşturma işlemi sırasında dosyalar indirilir ve kaynak grubunda depolanır ve bu, `IT_<DestinationResourceGroup>_<TemplateName>` küçük bir depolama maliyetlerine neden olur. Bunları tutmak istemiyorsanız, görüntü oluşturulduktan sonra **görüntü şablonunu** silin.
 
Image Builder, VM için gereken bir D1v2 VM boyutu, depolama alanı ve ağ kullanarak bir VM oluşturur. Bu kaynaklar, derleme işleminin süresi boyunca son verilecek ve görüntü Oluşturucu görüntüyü oluşturmayı tamamladığında silinir. 
 
Azure Image Builder, görüntüyü seçtiğiniz bölgelere dağıtır ve bu da ağ çıkış ücretleri doğuracaktır.

## <a name="hyper-v-generation"></a>Hyper-V oluşturma
Image Builder Şu anda yalnızca yerel olarak Hyper-V oluşturma (Gen1) 1 görüntülerini Azure Paylaşılan görüntü Galerisi 'ne (SıG) veya yönetilen görüntüye oluşturmayı destekler. Gen2 görüntüleri oluşturmak istiyorsanız, bir kaynak Gen2 görüntüsü kullanmanız ve VHD 'ye dağıtmanız gerekir. Bundan sonra, VHD 'den yönetilen bir görüntü oluşturmanız ve bunu bir Gen2 görüntüsü olarak SıG 'a eklemeniz gerekecektir.
 
## <a name="next-steps"></a>Sonraki adımlar 
 
Azure görüntü Oluşturucu 'Yu denemek için [Linux](./linux/image-builder.md) veya [Windows](./windows/image-builder.md) görüntülerini oluşturma makalelerine bakın.

