---
title: FHıR için Azure API 'de dışarı aktarma ayarlarını yapılandırma
description: Bu makalede, FHıR için Azure API 'de dışarı aktarma ayarlarının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/5/2020
ms.author: matjazl
ms.openlocfilehash: e4adceea5c2cd2a36d7a867ca9b9d2ad7c33c155
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91529992"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Dışarı aktarma ayarını yapılandırın ve depolama hesabını ayarlayın

FHıR için Azure API, FHıR hesabı için Azure API 'sinden bir depolama hesabına veri vermenize olanak tanıyan $export komutunu destekler.

FHIR için Azure API 'de dışarı aktarma yapılandırması ile ilgili üç adım vardır:

1. FHıR hizmeti için Azure API 'de yönetilen kimliği etkinleştirme
2. Azure Storage hesabı oluşturma (daha önce yapmadıysanız) ve FHıR için Azure API 'sine depolama hesabına izin atama
3. FHıR için Azure API 'sinde depolama hesabı verme depolama hesabı olarak seçiliyor

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Azure API 'sinde FHıR için yönetilen kimliği etkinleştirme

Azure API 'YI dışa aktarma için yapılandırmanın ilk adımı, hizmette sistem genelinde yönetilen kimliği etkinleştirmektir. Azure 'da Yönetilen kimlikler hakkında [buradan](../active-directory/managed-identities-azure-resources/overview.md)bilgi edinebilirsiniz.

Bunu yapmak için FHıR hizmeti için Azure API 'ye gidin ve kimlik dikey penceresini seçin. Durumu on olarak değiştirmek, FHıR hizmeti için Azure API 'de yönetilen kimliği etkinleştirir.

![Yönetilen kimliği etkinleştir](media/export-data/fhir-mi-enabled.png)

Şimdi bir sonraki adıma geçebilir ve bir depolama hesabı oluşturabilir ve hizmetimizin atayabilirsiniz.

## <a name="adding-permission-to-storage-account"></a>Depolama hesabına izin ekleniyor

Dışarı aktarma içindeki bir sonraki adım, FHıR hizmetinin depolama hesabına yazabilmesi için Azure API 'SI için izin atamaya yöneliktir.

Depolama hesabı oluşturduktan sonra depolama hesabı 'nda Access Control (ıAM) dikey penceresine gidin ve rol atamaları Ekle ' yi seçin.

![Rol atamasını dışarı aktar](media/export-data/fhir-export-role-assignment.png)

Burada, hizmet adınızla rol Depolama Blobu verileri katkıda bulunanı ekleyeceğiz.

![Rol Ekle](media/export-data/fhir-export-role-add.png)

Artık, $export için varsayılan depolama hesabı olarak FHıR için Azure API 'sinde depolama hesabını seçebileceğiniz bir sonraki adıma hazırlıyoruz.

## <a name="selecting-the-storage-account-for-export"></a>$export için depolama hesabı seçiliyor

Son adım, FHıR için Azure API 'nin verileri uygulamasına aktarmak için kullanacağı Azure Depolama hesabını atacaktır. Bunu yapmak için, Azure portal 'deki FHıR hizmeti için Azure API 'sindeki tümleştirme dikey penceresine gidin ve depolama hesabını seçin

![FHıR dışarı aktarma depolaması](media/export-data/fhir-export-storage.png)

Bundan sonra $export komutunu kullanarak verileri dışarı aktarmaya hazırız.

>[!div class="nextstepaction"]
>[Ek ayarlar](azure-api-for-fhir-additional-settings.md)
