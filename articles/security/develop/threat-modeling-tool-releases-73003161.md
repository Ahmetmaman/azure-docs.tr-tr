---
title: Microsoft Threat Modeling Tool Release 03/22/2020-Azure
description: Tehdit modelleme aracı sürümü 7.3.00316.1 için sürüm notlarını belgeleme.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 03/22/2020
ms.openlocfilehash: e182d40d20529b5743fa9105c68108a8ae55d943
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94516909"
---
# <a name="threat-modeling-tool-update-release-73003161---03222020"></a>Threat Modeling Tool güncelleştirme sürümü 7.3.00316.1-03/22/2020

Microsoft Threat Modeling Tool (TMT) sürümü 7.3.00316.1 Mart 22 2020 ' de yayımlanmıştır ve aşağıdaki değişiklikleri içerir:

- Erişilebilirlik geliştirmeleri
- Hata düzeltmeleri
- Yeni DiagramReader özelliği

## <a name="notable-bug-fixes"></a>Notable hata düzeltmeleri

### <a name="exporting-the-threat-list-to-csv"></a>Tehdit listesini CSV 'ye aktarma

CSV 'ye dışarı aktar işlevi tehdit listesindeki hangi alanların dışarı aktarılacağını sürekli olarak seçti. Artık tehdit listesindeki tüm alanlar CSV dosyasına aktaralınacaktır. 

### <a name="ux-bugs"></a>UX hataları

- Birincil iş akışındaki (Oluştur/Aç/çözümle) ve Şablon Düzenleyicisi deneyiminde yardım menüleri artık tutarlı menü seçeneklerine sahiptir.
- Kalıplar bölmesindeki arama çubuğu artık standart bir imlece sahiptir ve uygun Etiketler eklenmiştir.

## <a name="new-features"></a>Yeni özellikler

### <a name="diagramreader-feature-has-been-added"></a>DiagramReader özelliği eklendi

Bir model açıkken ana menüye yeni bir DiagramReader özelliği eklenmiştir. Bu özellik, modelin grafik temsilini metin tabanlı bir anlatıcı olarak dönüştürür. 

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen işletim sistemleri:
  - [Microsoft Windows 10 yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- Gerekli .NET sürümü:
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek gereksinimler:
  - Araç ve şablonların güncelleştirmelerini almak için internet bağlantısı

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Threat Modeling Tool belgeleri [docs.Microsoft.com](./threat-modeling-tool.md)konumunda bulunur ve [Aracı kullanma hakkında](./threat-modeling-tool-getting-started.md)bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)en son sürümünü indirin.