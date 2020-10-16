---
title: CAF Foundation şema örneğine genel bakış
description: Azure Foundation şema örneği için Bulut Benimseme Çerçevesi’ne (CAF) genel bakış ve mimari.
ms.date: 09/14/2020
ms.topic: sample
ms.openlocfilehash: 77e8b79ec7cf217161099808cee4364e31c6d6dd
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91950287"
---
# <a name="overview-of-the-microsoft-cloud-adoption-framework-for-azure-foundation-blueprint-sample"></a>Azure Foundation şema örneği için Microsoft Bulutu Benimseme çerçevesine genel bakış

Azure Foundation şeması için Microsoft Bulutu Benimseme Çerçevesi (CAF), üretim düzeyindeki ilk Azure uygulamanız için gerekli çekirdek altyapı kaynakları ve ilke denetimleri kümesini dağıtır. Bu temel şema, CAF’de bulunan önerilen desene göre belirlenir.

## <a name="architecture"></a>Mimari

CAF Foundation şema örneği, kuruluşların bulut varlıklarını yönetmek için gerekli olan temel denetimleri belirlemek üzere kullanılabileceği, Azure’da önerilen altyapı kaynaklarını dağıtır. Bu örnek, bir kuruluşun Azure’ı güvenle kullanmaya başlamasını sağlayacak kaynakları, ilkeleri ve şablonları dağıtıp uygular.

:::image type="complex" source="../../media/caf-blueprints/caf-foundation-architecture.png" alt-text="CAF Foundation görüntüsü, Azure’ı kullanmaya başlamak amacıyla temel oluşturmaya yönelik CAF rehberliğinin bir parçası olarak yüklenenleri tanımlar." border="false":::
   CAF Foundation şemasını dağıtarak elde edilen bir Azure mimarisini tanımlar.  Günlükleri kaydetmek için bir depolama hesabı ve depolama hesabında depolanmak üzere yapılandırılmış bir Log Analytics’in yer aldığı, kaynak gruplarına sahip bir abonelik için geçerlidir. Burada, Azure Güvenlik Merkezi standart kurulumuyla yapılandırılmış bir Azure Key Vault da gösterilmektedir. Tüm bu temel altyapılara Azure Active Directory kullanılarak erişilebilir ve bunlar Azure İlkesi kullanılarak zorunlu kılınabilir.     
:::image-end:::

Bu uygulama, güvenli, tam olarak izlenen, kurumsal kullanıma hazır bir temel sağlamak amacıyla kullanılan çeşitli Azure hizmetlerinden oluşur. Bu ortam şunlardan oluşur:

- Paylaşılan hizmetler ortamına dağıtılmış VM'lerin gizli dizilerini barındırmak için kullanılan [Azure Key Vault](../../../../key-vault/general/overview.md) örneği
- [Depolama Hesapları](../../../../storage/common/storage-introduction.md)’na güvenli dağıtımınıza başladığınız andan itibaren tüm eylemlerin ve hizmetlerin tanılama günlüğüne kaydedilmesi için merkezi bir konumda günlüğe kaydedildiğinden emin olmak amacıyla dağıtılan [Log Analytics](../../../../azure-monitor/overview.md) dağıtımı
- Geçirilen iş yüklerinize yönelik tehdit koruması sağlayan [Azure Güvenlik Merkezi](../../../../security-center/security-center-introduction.md) (standart sürüm) dağıtımı
- Şema ayrıca şunlar için [Azure İlkesi](../../../policy/overview.md) tanımlarını belirler ve dağıtır:
  - İlke tanımları:
    - Kaynak gruplarına uygulanan etiketleme (CostCenter)
    - Kaynak grubuna kaynakları CostCenter Etiketiyle ekleme
    - Kaynaklar ve Kaynak Grupları için izin verilen Azure Bölgesi
    - İzin verilen Depolama Hesabı SKU’ları (dağıtırken seçin)
    - İzin verilen Azure VM SKU’ları (dağıtırken seçin)
    - Ağ İzleyicisi’nin dağıtılmasını gerektirme 
    - Azure Depolama Hesabı Güvenli aktarım Şifrelemesi gerektirme
    - Kaynak türlerini reddetme (dağıtırken seçin)  
  - İlke girişimleri:
    - Azure Güvenlik Merkezi’nde İzleme’yi etkinleştirme (100’den fazla ilke tanımı)

Tam bu öğeler [Azure Mimari Merkezi - Referans Mimarileri](/azure/architecture/reference-architectures/)'nde yayımlanan kanıtlanmış uygulamalara dayanır.

> [!NOTE]
> CAF Foundation, iş yükleri için temel mimariyi hazırlar.
> Bu işlevsel mimarinin arkasında yine iş yüklerini dağıtmanız gerekir.

Daha fazla bilgi için bkz. [Azure için Microsoft Bulut Benimseme Çerçevesi - Hazır](/azure/cloud-adoption-framework/ready/).

## <a name="next-steps"></a>Sonraki adımlar

CAF Foundation şema örneğinin genel bakış bilgilerini ve mimarisini gözden geçirdiniz.

> [!div class="nextstepaction"]
> [CAF Foundation şeması - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.