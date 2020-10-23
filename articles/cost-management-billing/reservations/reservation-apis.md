---
title: Azure rezervasyon otomasyonu API’leri | Microsoft Docs
description: Program aracılığıyla rezervasyon bilgilerini almak için kullanabileceğiniz Azure API’leri hakkında bilgi edinin.
author: yashesvi
ms.reviewer: yashesvi
tags: billing
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: f12c1f20e01cb67adff2c83c7262f92daf8da969
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151489"
---
# <a name="apis-for-azure-reservation-automation"></a>Azure rezervasyon otomasyonu API’leri

Kuruluş için Azure hizmeti veya yazılım rezervasyonları ile ilgili bilgileri program aracılığıyla almak için Azure API’lerini kullanın.

## <a name="find-reservation-plans-to-buy"></a>Satın alınacak rezervasyon planlarını bulun

Kuruluşunuzun kullanımına göre hangi rezervasyon planının satın alınacağına yönelik öneriler almak için Rezervasyon önerisi API’sini kullanın. Daha fazla bilgi için bkz. [Rezervasyon önerileri alma](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).

Ayrıca Tüketim API Kullanım Ayrıntısını kullanarak da kaynak kullanımınızı analiz edebilirsiniz. Daha fazla bilgi için bkz [Kullanım Ayrıntıları - Ödeme Hesabına Göre Faturalama Dönemi Listesi](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslistforbillingperiod-legacy). Genellikle rezervasyon için en uygun aday, sürekli olarak kullandığınız Azure kaynaklarıdır.

## <a name="buy-a-reservation"></a>Ayırma satın alma

REST API’leri kullanarak program aracılığıyla Azure rezervasyonları ve yazılım planları satın alabilirsiniz. Daha fazla bilgi için bkz [Rezervasyon Siparişi - Satın Alma API’si](/rest/api/reserved-vm-instances/reservationorder/purchase).

REST API kullanılarak satın alma isteğinde bulunma örneği aşağıda verilmiştir:

```
PUT https://management.azure.com/providers/Microsoft.Capacity/reservationOrders/<GUID>?api-version=2019-04-01
```

İstek gövdesi:

```
{
 "sku": {
    "name": "standard_D1"
  },
 "location": "westus",
 "properties": {
    "reservedResourceType": "VirtualMachines",
    "billingScopeId": "/subscriptions/ed3a1871-612d-abcd-a849-c2542a68be83",
    "term": "P1Y",
    "quantity": "1",
    "displayName": "TestReservationOrder",
    "appliedScopes": null,
    "appliedScopeType": "Shared",
    "reservedResourceProperties": {
      "instanceFlexibility": "On"
    }
  }
}
```

Azure portalında bir rezervasyon da satın alabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

Hizmet planları:
- [Sanal makine](../../virtual-machines/windows/prepay-reserved-vm-instances.md?toc=/azure/billing/TOC.json)
-  [Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md?toc=/azure/billing/TOC.json)
- [SQL Database](../../azure-sql/database/reserved-capacity-overview.md?toc=/azure/billing/TOC.json)

Yazılım planları:
- [SUSE Linux yazılımı](../../virtual-machines/linux/prepay-suse-software-charges.md?toc=/azure/billing/TOC.json)

## <a name="get-reservations"></a>Rezervasyon alma

Kurumsal Anlaşma (EA müşterisi) olan bir Azure müşterisiyseniz, [Ayrılmış Örnek işlemi ücretlerini alma API’si](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) kullanarak kuruluşunuzun satın aldığı rezervasyonları alabilirsiniz. Diğer abonelikler için, [Rezervasyon Siparişi - Listeleme](/rest/api/reserved-vm-instances/reservationorder/list) API’sini kullanarak satın aldığınız rezervasyon listesini alın ve görüntüleme izinlerine sahip olun. Varsayılan olarak, rezervasyonu satın alan kişi veya hesap sahibi, rezervasyonu görüntüleme izinlerine sahiptir.

## <a name="see-reservation-usage"></a>Rezervasyon kullanım bilgilerini görüntüleme

Bir EA müşterisiyseniz, kuruluşunuzda rezervasyonların nasıl kullanıldığını program aracılığıyla görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [Kurumsal müşteriler için Ayrılmış Örnek kullanım bilgilerini alma](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage). Diğer abonelikler için [Rezervasyon Özetleri - Rezervasyon Siparişine ve Rezervasyona Göre Listeleme](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation) API’sini kullanın.

Kuruluşunuzun rezervasyonlarının gerektiği şekilde kullanılmadığını belirlerseniz:

- Kuruluşunuzun oluşturduğu sanal makinelerin, rezervasyondaki sanal makine boyutuyla aynı olduğundan emin olun.
- Örnek boyutu esnekliğinin açık olduğundan emin olun. Daha fazla bilgi için bkz [Rezervasyonları yönetme - Ayrılmış Sanal Makine Örnekleri için iyileştirme ayarını değiştirme](manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).
- Rezervasyon kapsamını, daha geniş çapta geçerli olması için paylaşılan olarak değiştirin. Daha fazla bilgi için bkz. [Rezervasyonları yönetme - Rezervasyon kapsamını değiştirme](manage-reserved-vm-instance.md#change-the-reservation-scope).
- Kullanılmayan miktarı değiştirin. Daha fazla bilgi için bkz. [Rezervasyonları yönetme](manage-reserved-vm-instance.md).

## <a name="give-access-to-reservations"></a>Rezervasyonlara erişme izni verme

[Rezervasyon - İşlem - Listeleme API’si](/rest/api/reserved-vm-instances/reservationorder/list) kullanarak bir kullanıcının erişim yetkisine sahip olduğu tüm rezervasyonların listesini alın. Program aracılığıyla bir rezervasyona erişme izni vermek için aşağıdaki makalelerden birine bakın:

- [REST API kullanarak Azure rol atamalarını ekleme veya kaldırma](../../role-based-access-control/role-assignments-rest.md)
- [Azure PowerShell kullanarak Azure rol atamalarını ekleme veya kaldırma](../../role-based-access-control/role-assignments-powershell.md)
- [Azure CLI kullanarak Azure rol atamalarını ekleme veya kaldırma](../../role-based-access-control/role-assignments-cli.md)

## <a name="split-or-merge-reservation"></a>Rezervasyonu bölme veya birleştirme

Bir rezervasyon içinde birden fazla kaynak örneği satın aldıktan sonra o rezervasyon içindeki örnekleri farklı aboneliklere atamak isteyebilirsiniz. Rezervasyon kapsamını, aynı faturalama bağlamındaki tüm abonelikler için geçerli olacak şekilde değiştirebilirsiniz. Ancak maliyet yönetimi veya bütçeleme amacıyla bu kapsamı “tek abonelik” olarak tutmak ve belirli bir aboneliğe rezervasyon örnekleri atamak isteyebilirsiniz.

Bir rezervasyonu bölmek için [Rezervasyon - Bölme](/rest/api/reserved-vm-instances/reservation/split) API’sini kullanın. Ayrıca PowerShell kullanarak bir rezervasyonu bölebilirsiniz. Daha fazla bilgi için bkz. [Rezervasyonları yönetme - Rezervasyonu iki rezervasyona bölme](manage-reserved-vm-instance.md#split-a-single-reservation-into-two-reservations).

İki rezervasyonu tek bir rezervasyonda birleştirmek için [Rezervasyon - Birleştirme](/rest/api/reserved-vm-instances/reservation/merge) API’sini kullanın.

## <a name="change-scope-for-a-reservation"></a>Rezervasyon kapsamını değiştirme

Rezervasyon kapsamı, tek abonelik, tek kaynak grubu veya faturalama bağlamınızdaki tüm abonelikler olabilir. Kapsamı tek abonelik veya tek kaynak grubu olarak ayarlarsanız rezervasyon, seçilen abonelikte çalıştırılan kaynaklarla eşleştirilir. Aboneliği veya kaynak grubunu siler ya da taşırsanız rezervasyon kullanılmaz.  Kapsamı paylaşılan olarak ayarlarsanız Azure, faturalama bağlamındaki tüm aboneliklerde çalıştırılan kaynaklarla rezervasyonu eşleştirir. Faturalama bağlamı, rezervasyonu satın almak için kullandığınız aboneliğe bağlıdır. Satın alma sırasında kapsamı seçebilir veya satın aldıktan sonra istediğiniz zaman değiştirebilirsiniz. Daha fazla bilgi için bkz. [Rezervasyonları Yönetme - Kapsamı değiştirme](manage-reserved-vm-instance.md#change-the-reservation-scope).

Kapsamı program aracılığıyla değiştirmek için [Rezervasyon - Güncelleştirme](/rest/api/reserved-vm-instances/reservation/update) API’sini kullanın.

## <a name="learn-more"></a>Daha fazlasını öğrenin

- [Azure rezervasyonları nedir?](save-compute-costs-reservations.md)
- [Sanal makine rezervasyon indiriminin nasıl uygulandığını anlama](../manage/understand-vm-reservation-charges.md)
- [SUSE Linux Enterprise yazılım planı indiriminin nasıl uygulandığını anlama](understand-suse-reservation-charges.md)
- [Diğer rezervasyon indirimlerinin nasıl uygulandığını anlama](understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](reserved-instance-windows-software-costs.md)
- [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](/partner-center/azure-reservations)