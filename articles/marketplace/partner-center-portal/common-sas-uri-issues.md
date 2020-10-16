---
title: Ortak SAS URI sorunları ve düzeltmeleri-Azure Marketi
description: Paylaşılan erişim imzaları ile çalışırken karşılaşılan yaygın sorunlar ve önerilen çözünürlükler.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 04/09/2020
ms.openlocfilehash: 4eb3c3e893a276aed10807a13a0f2d6d3bc4e71d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87316847"
---
# <a name="common-sas-uri-issues-and-fixes"></a>Ortak SAS URI sorunları ve düzeltmeleri

Aşağıda, paylaşılan erişim imzaları ile çalışırken (çözümünüz için karşıya yüklenen VHD 'leri tanımlamak ve paylaşmak için kullanılan) ve önerilen çözümlerin yanı sıra karşılaşılan yaygın sorunlar verilmiştir.

| **Konuda** | **Hata Iletisi** | **Düzeltme** |
| --------- | ------------------- | ------- |
| *Görüntüler kopyalanırken hata oluştu* |  |  |
| SAS URI 'sinde "?" bulunamadı | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Önerilen araçları kullanarak SAS URI 'sini güncelleştirin. |
| SAS URI 'SI içinde "St" ve "sa" parametreleri yok | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URI 'sini uygun **Başlangıç tarihi** ve **bitiş tarihi** değerleriyle güncelleştirin. |
| SAS URI 'SI içinde "SP = rl" değil | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URI 'sini, ve olarak ayarlanan izinlerle `Read` güncelleştirin `List` . |
| SAS URI 'SI, VHD adında boşluk içeriyor | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Boşluk kaldırmak için SAS URI 'sini güncelleştirin. |
| SAS URI yetkilendirme hatası | `Failure: Copying Images. Not able to download blob due to authorization error.` | SAS URI biçimini gözden geçirin ve düzeltin. Gerekirse yeniden oluşturun. |
| SAS URI 'SI "St" ve "o" parametrelerinin tam tarih-saat belirtimi yoktur | `Failure: Copying Images. Not able to download blob due to incorrect SAS Uri.` | SAS URI 'SI **Başlangıç tarihi** ve **bitiş tarihi** parametreleri ( `st` ve alt `se` dizeleri), gibi tam bir tarih-saat biçimine sahip olmalıdır `11-02-2017T00:00:00Z` . Kısaltılmış sürümler geçersiz (Azure CLı 'deki bazı komutlar varsayılan olarak kısaltılmış değerler oluşturabilir). |
|  |  |  |

Ayrıntılar için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
