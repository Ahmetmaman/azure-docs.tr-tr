---
title: Windows sanal masaüstü-Azure 'da yerleşik uygulamaları yayımlama
description: Windows sanal masaüstü 'nde yerleşik uygulamaları yayımlama.
author: Heidilohr
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b088f1f58e26dca854c17b0765607ebe76017dfe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88009503"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Windows sanal masaüstünde yerleşik uygulamaları yayımlama

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/publish-apps-2019.md)bakın.

Bu makalede, Windows sanal masaüstü ortamınızda uygulamaların nasıl yayımlanacağı açıklanır.

## <a name="publish-built-in-apps"></a>Yerleşik uygulamaları yayımlama

Yerleşik bir uygulama yayımlamak için:

1. Konak havuzunuzdaki sanal makinelerden birine bağlanın.
2. [Bu makaledeki](/powershell/module/appx/get-appxpackage?view=win10-ps/)yönergeleri izleyerek yayımlamak Istediğiniz uygulamanın **PackageFamilyName** alın.
3. Son olarak, `<PackageFamilyName>` önceki adımda bulduğunuz **PackageFamilyName** ile değiştirilmiş aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   New-AzWvdApplication -Name <applicationname> -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -FilePath "shell:appsFolder\<PackageFamilyName>!App" -CommandLineSetting <Allow|Require|DoNotAllow> -IconIndex 0 -IconPath <iconpath> -ShowInPortal:$true
   ```

>[!NOTE]
> Windows sanal masaüstü, uygulamaları yalnızca ile başlayan bir konum yüklerken yayımlamayı destekler `C:\Program Files\WindowsApps` .

## <a name="update-app-icons"></a>Uygulama simgelerini Güncelleştir

Bir uygulamayı yayımladıktan sonra, normal simge resmi yerine varsayılan Windows uygulaması simgesine sahip olur. Simgeyi normal simgesine değiştirmek için, istediğiniz simgenin görüntüsünü bir ağ paylaşımında koyun. Desteklenen görüntü biçimleri PNG, BMP, GIF, JPG, JPEG ve ICO.

## <a name="publish-microsoft-edge"></a>Microsoft Edge 'i Yayımla

Microsoft Edge 'i yayımlamak için kullandığınız işlem, diğer uygulamalar için yayımlama işleminden biraz farklıdır. Microsoft Edge 'i varsayılan giriş sayfası ile yayımlamak için şu cmdlet 'i çalıştırın:

```powershell
New-AzWvdApplication -Name -ResourceGroupName -ApplicationGroupName -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" -CommandLineSetting <Allow|Require|DoNotAllow> -iconPath "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\microsoftedge.exe" -iconIndex 0 -ShowInPortal:$true
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcıların [Windows sanal masaüstü kullanıcıları için özet akışını özelleştirmek](customize-feed-for-virtual-desktop-users.md)üzere uygulamaların nasıl görüntülendiğini düzenlemek için akışların nasıl yapılandırılacağı hakkında bilgi edinin.
- Msix uygulama iliştirme özelliği hakkında daha fazla bilgi [edinin.](app-attach.md)

