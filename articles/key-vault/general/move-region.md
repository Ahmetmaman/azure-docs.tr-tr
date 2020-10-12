---
title: Anahtar kasasını farklı bir bölgeye taşıma-Azure Key Vault | Microsoft Docs
description: Bu makalede, anahtar kasasının farklı bir bölgeye taşınması hakkında rehberlik sunulmaktadır.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 04/24/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another region.
ms.openlocfilehash: 7d58cd26b6e4ca77da98f8c2f82dbdb481ccbb50
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88585761"
---
# <a name="move-an-azure-key-vault-across-regions"></a>Bir Azure anahtar kasasını bölgeler arasında taşıma

Azure Key Vault, anahtar kasasını bir bölgeden diğerine taşımaya izin veren bir kaynak taşıma işlemini desteklemez. Bu makalede bir iş ihtiyacı olan kuruluşların bir anahtar kasasını başka bir bölgeye taşıması için geçici çözümler ele alınmaktadır. Her geçici çözüm seçeneğinde sınırlamalar vardır. Bir üretim ortamında uygulamayı denemeden önce bu geçici çözümlerin etkilerini anlamak önemlidir.

Bir anahtar kasasını başka bir bölgeye taşımak için, başka bir bölgede bir Anahtar Kasası oluşturun ve ardından her bir parolayı mevcut anahtar kasasından yeni anahtar kasasına el ile kopyalayın. Bunu, aşağıdaki iki seçenekten birini kullanarak yapabilirsiniz.

## <a name="design-considerations"></a>Tasarım konusunda dikkat edilmesi gerekenler

Başlamadan önce aşağıdaki kavramları göz önünde bulundurun:

* Anahtar Kasası adları genel olarak benzersizdir. Kasa adını yeniden kullanamazsınız.
* Yeni anahtar kasasındaki erişim ilkelerinizi ve ağ yapılandırma ayarlarınızı yeniden yapılandırmanız gerekir.
* Yeni anahtar kasasında geçici silme ve Temizleme korumasını yeniden yapılandırmanız gerekir.
* Yedekleme ve geri yükleme işlemi, otomatik döndürme ayarlarınızı korumaz. Ayarları yeniden yapılandırmanız gerekebilir.

## <a name="option-1-use-the-key-vault-backup-and-restore-commands"></a>Seçenek 1: Anahtar Kasası yedekleme ve geri yükleme komutlarını kullanma

Backup komutunu kullanarak kasaızdaki her bir gizli dizi, anahtar ve sertifikayı yedekleyebilirsiniz. Gizli dizileri şifreli bir blob olarak indirilir. Daha sonra blobu yeni anahtar kasanıza geri yükleyebilirsiniz. Komutların listesi için bkz. [Azure Key Vault komutları](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-6.13.0#key_vault).

Backup ve restore komutlarının kullanılması iki sınırlamalara sahiptir:

* Bir coğrafya içindeki anahtar kasasını yedekleyebilir ve başka bir Coğrafya 'ya geri yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure coğrafi lıkları](https://azure.microsoft.com/global-infrastructure/geographies/).

* Yedekleme komutu her gizli dizinin tüm sürümlerini yedekler. Çok sayıda eski sürümleri olan bir gizli diziniz varsa (10’dan fazla), istek boyutu izin verilen en büyük boyutu aşabilir ve işlem başarısız olabilir.

## <a name="option-2-manually-download-and-upload-the-key-vault-secrets"></a>2. seçenek: Anahtar Kasası gizli dizilerini El Ile indirme ve karşıya yükleme

Belirli gizli türleri el ile indirebilirsiniz. Örneğin, sertifikaları PFX dosyası olarak indirebilirsiniz. Bu seçenek, sertifikalar gibi bazı gizli türlerin coğrafi kısıtlamalarını ortadan kaldırır. PFX dosyalarını herhangi bir bölgedeki herhangi bir anahtar kasasında karşıya yükleyebilirsiniz. Gizli dizileri, parola korumalı olmayan bir biçimde indirilir. Taşıma sırasında gizli dizilerinizi güvenli hale getirmekten siz sorumlusunuz.
