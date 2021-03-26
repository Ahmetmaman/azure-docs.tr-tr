---
title: Yazılım planı indirimi - Azure | Microsoft Docs
description: Yazılım planı indirimlerinin sanal makinelerde yazılıma nasıl uygulandığını öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 03/25/2021
ms.author: banders
ms.openlocfilehash: 8bf53715b7f19c44d9114150e617f903cd05a51e
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105566324"
---
# <a name="azure-software-plan-discount"></a>Azure yazılım planı indirimi

SUSE ve RedHat için Azure yazılım planları, dağıtılan sanal makinelere uygulanan rezervasyonlardır. Yazılım planı indirimi, rezervasyonla eşleşen dağıtılan sanal makinelerin yazılım kullanımına uygulanır.

Bir sanal makineyi kapattığınızda, varsa eşleşen başka bir sanal makineye otomatik olarak indirim uygulanır. Yazılım planı, bir sanal makine üzerinde yazılımı çalıştırma maliyetini kapsar. İşlem, depolama ve ağ iletişimi gibi diğer ücretler ayrı olarak ücretlendirilir.

Doğru planı satın almak için, sanal makine kullanımınızı ve bu sanal makinelerdeki vCPU sayısını anlamanız gerekir. Kullanım verilerinize göre, hangi planın satın alınacağını belirlemenize yardımcı olması için aşağıdaki bölümleri kullanın.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi “*kullanılmadığı takdirde hakkı kaybedilen*” bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

## <a name="review-redhat-vm-usage-before-you-buy"></a>Satın almadan önce RedHat sanal makine kullanımını gözden geçirme

Kullanım verilerinizden ürün adını bulun ve aynı tür ve boyutta RedHat planını satın alın.

Örneğin, kullanımınız **Red Hat Enterprise Linux - 1-4 vCPU VM Lisansı** ürününü içeriyorsa, **1-4 vCPU VM** için **Red Hat Enterprise Linux** ürününü satın almanız gerekir.

<!--ADD RHEL SCREENSHOT -->

## <a name="review-suse-vm-usage-before-you-buy"></a>Satın almadan önce SUSE sanal makine kullanımını gözden geçirme

Kullanım verilerinizden ürün adını bulun ve aynı tür ve boyutta SUSE planını satın alın.

Örneğin, kullanımınız **SUSE Linux Enterprise Server Priority - 2-4 vCPU VM Desteği** ürünü içinse, **2-4 vCPU** için **SUSE Linux Enterprise Server Priority** ürününü satın almanız gerekir.

![Satın alınacak ürünü seçme örneği](./media/understand-suse-reservation-charges/select-suse-linux-enterprise-server-priority-2-4-vcpu.png)

## <a name="discount-applies-to-different-vm-sizes-for-suse-plans"></a>SUSE planları için farklı sanal makine boyutlarına indirim uygulanır

Ayrılmış Sanal Makine Örnekleri gibi SUSE planı satın alımları da örnek boyutu esnekliği sunar. Başka bir deyişle, farklı bir vCPU sayısıyla bir sanal makine dağıttığınızda bile indiriminiz geçerli olur. Yazılım planı içindeki farklı sanal makine boyutları için indirim geçerlidir.

İndirim tutarı, aşağıdaki tablolarda listelenen orana bağlıdır. Bu oran, söz konusu gruptaki her bir ölçüm için ilgili ayak izini karşılaştırır. Bu oran, sanal makine vCPU’larına bağlıdır. Kaç tane sanal makine örneğinin SUSE Linux planı indirimi aldığını hesaplamak için oran değerini kullanın.

Örneğin, 3 veya 4 vCPU içeren bir sanal makine için SUSE Linux Enterprise Server for HPC Priority planı satın alırsanız, bu rezervasyon için oran 2 olur. İndirim, şunlar için SUSE yazılımı maliyetini kapsar:

- 1 veya 2 vCPU ile dağıtılan 2 sanal makine,
- 3 veya 4 vCPU ile dağıtılan 1 sanal makine,
- veya 5 ya da daha fazla vCPU ile dağıtılan bir sanal makinenin yaklaşık %77’si veya 0,77.

5 veya daha fazla vCPU için oran 2,6’dır. Bu nedenle 5 veya daha fazla vCPU içeren bir sanal makineli SUSE rezervasyonu, yazılım maliyetinin yalnızca bir kısmını kapsar; bu da yaklaşık %77’ye denk gelir.

Aşağıdaki tablolarda, rezervasyon satın alabileceğiniz yazılım planları, bunların ilişkili kullanım ölçümleri ve her biri için oranlar gösterilmektedir.

### <a name="suse-linux-enterprise-server-for-hpc-priority"></a>SUSE Linux Enterprise Server for HPC Priority

|SUSE VM | MeterId| Oran| Örnek sanal makine boyutu|
| -------| ------------------------| --- |--- |
|SUSE Linux Enterprise Server for HPC Priority 1-2 vCPU|e275a668-ce79-44e2-a659-f43443265e98|1|D2s_v3|
|SUSE Linux Enterprise Server for HPC Priority 3-4 vCPU|e531e1c0-09c9-4d83-b7d0-a2c6741faa22|2|D4s_v3|
|SUSE Linux Enterprise Server for HPC Priority 5 + vCPU|4edcd5a5-8510-49a8-a9fc-c9721f501913|2,6|D8s_v3|

### <a name="suse-linux-enterprise-server-for-hpc-standard"></a>SUSE Linux Enterprise Server for HPC Standard

|SUSE VM | MeterId | Oran|Örnek sanal makine boyutu|
| ------- | --- | ------------------------| --- |
|SUSE Linux Enterprise Server for HPC Standard 1-2 vCPU |8c94ad45-b93b-4772-aab1-ff92fcec6610|1|D2s_v3|
|SUSE Linux Enterprise Server for HPC Standard 3-4 vCPU|4ed70d2d-e2bb-4dcd-b6fa-42da71861a1c|1,92308|D4s_v3|
|SUSE Linux Enterprise Server for HPC Standard 5 + vCPU |907a85de-024f-4dd6-969c-347d47a1bdff|2,92308|D8s_v3|

### <a name="suse-linux-enterprise-server-for-sap-standard"></a>SAP Standard için SUSE Linux Enterprise Server

Daha önce, SAP Standard için SUSE Linux Enterprise Server SUSE Linux Enterprise Server for SAP Priority olarak adlandırılmıştır.

|SUSE VM | MeterId | Oran|Örnek sanal makine boyutu|
| ------- |------------------------| --- | --- |
|SAP Standard 1-2 vCPU için SUSE Linux Enterprise Server|497fe0b6-fa3c-4e3d-a66b-836097244142|1|D2s_v3|
|SAP Standard 3-4 vCPU için SUSE Linux Enterprise Server |847887de-68ce-4adc-8a33-7a3f4133312f|2|D4s_v3|
|SAP Standard 5 + vCPU için SUSE Linux Enterprise Server |18ae79cd-dfce-48c9-897b-ebd3053c6058|2,41176|D8s_v3|

### <a name="suse-linux-enterprise-server-standard"></a>SUSE Linux Enterprise Server Standard

|SUSE VM | MeterId | Oran|Örnek sanal makine boyutu|
| ------- |------------------------| --- |--- |
|SUSE Linux Enterprise Server Standard 1-2 çekirdek vCPU sayısı |4b2fecfc-b110-4312-8f9d-807db1cb79ae|1|D2s_v3|
|SUSE Linux Enterprise Server Standard 3-4 çekirdek vCPU sayısı |0c3ebb4c-db7d-4125-b45a-0534764d4bda|1,92308|D4s_v3|
|SUSE Linux Enterprise Server Standard 5 + vCPU |7b349b65-d906-42e5-833f-b2af38513468|2,30769| D8s_v3|

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyonlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
- [Azure Ayrılmış Sanal Makine Örnekleri ile SUSE yazılım planları için ön ödeme yapma](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../../virtual-machines/prepay-reserved-vm-instances.md)
- [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)