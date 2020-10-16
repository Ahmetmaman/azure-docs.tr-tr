---
title: Azure Otomasyonu durum yapılandırması için mevcut sunuculardan yapılandırmalar oluşturma
description: Bu makalede, Azure Otomasyonu durum yapılandırması için mevcut sunuculardan yapılandırmaların nasıl oluşturulacağı açıklanır.
keywords: DSC, PowerShell, yapılandırma, kurulum
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8f376fb383e50a39f0f12d45cf9b5ae47ad6fcbb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86186512"
---
# <a name="create-configurations-from-existing-servers"></a>Mevcut sunuculardan yapılandırma oluşturma

> Uygulama hedefi: Windows PowerShell 5,1

Mevcut sunuculardan yapılandırmaların oluşturulması zorlu bir görev olabilir.
Yalnızca ilgilendiğiniz kişiler için *Tüm* ayarları istemezsiniz.
Hatta, yapılandırmanın başarıyla uygulanabilmesi için ayarların ne sırada uygulanması gerektiğini bilmeniz gerekir.

> [!NOTE]
> Bu makale, açık kaynak topluluğu tarafından tutulan bir çözüme başvurur.
> Destek, Microsoft 'tan değil yalnızca GitHub işbirliği biçiminde kullanılabilir.

## <a name="community-project-reversedsc"></a>Topluluk projesi: Smardsc

[Smardsc](https://github.com/microsoft/reversedsc) adlı bir topluluk tarafından korunan çözüm, SharePoint 'i başlatan bu alanda çalışacak şekilde oluşturulmuştur.

Çözüm, [Sharepointdsc kaynağında](https://github.com/powershell/sharepointdsc) oluşturulur ve mevcut SharePoint sunucularından [bilgi toplamak](https://github.com/Microsoft/sharepointDSC.reverse#how-to-use) için onu genişletir.
En son sürümde hangi bilgi düzeyinin ekleneceğini belirlemek için birden çok [ayıklama modu](https://github.com/Microsoft/SharePointDSC.Reverse/wiki/Extraction-Modes) vardır.

Çözümü kullanmanın sonucu, SharePointDSC yapılandırma betiklerine göre kullanılacak [yapılandırma verilerini](https://github.com/Microsoft/sharepointDSC.reverse#configuration-data) oluşturuyor.

Veri dosyaları oluşturulduktan sonra, MOF dosyaları oluşturmak ve [MOF dosyalarını Azure Otomasyonu 'na yüklemek](./tutorial-configure-servers-desired-state.md#create-and-upload-a-configuration-to-azure-automation)Için bunları [DSC yapılandırma betikleri](/powershell/scripting/dsc/overview/overview) ile birlikte kullanabilirsiniz.
Daha sonra, yapılandırma çekmek için [Şirket](./automation-dsc-onboarding.md#enable-physicalvirtual-linux-machines) Içinden veya [Azure 'da](./automation-dsc-onboarding.md#enable-azure-vms) sunucularınızı kaydettirin.

Smardsc 'yi denemek için [PowerShell Galerisi](https://www.powershellgallery.com/packages/ReverseDSC/) ziyaret edin ve çözümü indirin veya "proje sitesi" ' ne tıklayarak [belgeleri](https://github.com/Microsoft/sharepointDSC.reverse)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell DSC 'yi anlamak için bkz. [Windows PowerShell Istenen durum yapılandırmasına genel bakış](/powershell/scripting/dsc/overview/overview).
- [DSC kaynaklarında](/powershell/scripting/dsc/resources/resources)PowerShell DSC kaynakları hakkında bilgi edinin.
- Yerel Configuration Manager yapılandırması hakkında daha fazla bilgi için bkz. [yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaconfig).
