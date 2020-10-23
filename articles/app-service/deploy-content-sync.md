---
title: İçeriği bir bulut klasöründen Eşitle
description: OneDrive veya Dropbox dahil olmak üzere bir bulut klasöründen içerik eşitlemesi aracılığıyla Azure App Service uygulamanızı nasıl dağıtacağınızı öğrenin.
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.topic: article
ms.date: 12/03/2018
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 880edff95bb548ec5328c543a542ea5dfcfc362f
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92150296"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Bir bulut klasöründen içerik eşitleme Azure App Service
Bu makalede, içeriğinizi Dropbox ve OneDrive 'dan [Azure App Service](./overview.md) eşitlenecek. 

İsteğe bağlı içerik eşitleme dağıtımı App Service [kudu dağıtım altyapısı](https://github.com/projectkudu/kudu/wiki)tarafından desteklenir. Belirlenen bir bulut klasöründe uygulama kodunuzla ve içeriklerle çalışabilir ve ardından bir düğmeye tıklayarak App Service eşitleyebilirsiniz. İçerik eşitleme, kudu derleme sunucusunu kullanır. 

## <a name="enable-content-sync-deployment"></a>İçerik eşitleme dağıtımını etkinleştir

İçerik eşitlemesini etkinleştirmek için [Azure portal](https://portal.azure.com)App Service uygulama sayfanıza gidin.

Sol taraftaki menüden **Deployment Center**  >  **OneDrive** veya **Dropbox**  >  **Yetkilendir**' e tıklayın. Yetkilendirme istemlerini izleyin. 

![Azure portal dağıtım merkezinde OneDrive veya Dropbox 'ın nasıl yetkilendirilemez olduğunu gösterir.](media/app-service-deploy-content-sync/choose-source.png)

Yalnızca OneDrive veya Dropbox ile bir kez yetkilendirme yapmanız gerekir. Zaten yetkiniz varsa **devam**' a tıklamanız yeterlidir. **Hesabı Değiştir**' i tıklatarak yetkili OneDrive veya Dropbox hesabını değiştirebilirsiniz.

![Azure portal dağıtım merkezinde yetkili OneDrive veya Dropbox hesabının nasıl değiştirileceğini gösterir.](media/app-service-deploy-content-sync/continue.png)

**Yapılandır** sayfasında, eşitlenmesini istediğiniz klasörü seçin. Bu klasör, OneDrive veya Dropbox 'ta aşağıdaki belirtilen içerik yolu altında oluşturulur. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

İşiniz bittiğinde **devam**' a tıklayın.

**Özet** sayfasında, seçeneklerinizi doğrulayıp **son**' a tıklayın.

## <a name="synchronize-content"></a>İçeriği eşitler

Bulut klasörünüzdeki içerikleri App Service eşitlemek istediğinizde, **Dağıtım Merkezi** sayfasına dönün ve **Eşitle**' ye tıklayın.

![Bulut klasörünüzün App Service ile nasıl eşitleneceğini gösterir.](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > API 'lerde temeldeki farklılıklar nedeniyle, **OneDrive iş** Şu anda desteklenmiyor. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>İçerik eşitleme dağıtımını devre dışı bırak

İçerik eşitlemesini devre dışı bırakmak için [Azure portal](https://portal.azure.com)App Service uygulama sayfanıza gidin.

Sol menüde **Dağıtım Merkezi**  >  **bağlantısı kes**' e tıklayın.

![Bulut klasörünüzün Azure portal App Service uygulamanızın bağlantısının nasıl yapılacağını gösterir.](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel git deposundan dağıtma](deploy-local-git.md)