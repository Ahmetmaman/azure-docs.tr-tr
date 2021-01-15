---
title: FTP/S kullanarak içerik dağıtma
description: Uygulamanızı FTP veya FTPS kullanarak Azure App Service dağıtma hakkında bilgi edinin. Şifrelenmemiş FTP 'yi devre dışı bırakarak Web sitesi güvenliğini geliştirebilirsiniz.
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.topic: article
ms.date: 09/18/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: cfec5ec5f14afc8c4eba5c21c5904687c9b187cc
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209262"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uygulamanızı FTP/S kullanarak Azure App Service dağıtma

Bu makalede, [Azure App Service](./overview.md)için Web uygulamanızı, mobil uygulama arka ucunu veya API uygulamanızı DAĞıTMAK üzere FTP veya FTPS 'nin nasıl kullanılacağı gösterilmektedir.

Uygulamanızın FTP/S uç noktası zaten etkin. FTP/S dağıtımını etkinleştirmek için yapılandırma gerekmez.

## <a name="open-ftp-dashboard"></a>FTP panosunu aç

1. [Azure Portal](https://portal.azure.com), **uygulama hizmetleri**' ni arayıp seçin.

    ![Uygulama Hizmetleri için arama yapın.](media/app-service-continuous-deployment/search-for-app-services.png)

2. Dağıtmak istediğiniz Web uygulamasını seçin.

    ![Uygulamanızı seçin.](media/app-service-continuous-deployment/select-your-app.png)

3. **Dağıtım Merkezi**  >  **FTP**  >  **panosu**' nu seçin.

    ![FTP panosunu aç](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>FTP bağlantı bilgilerini al

FTP panosunda, FTPS uç noktası ve uygulama kimlik bilgilerini kopyalamak için **Kopyala** ' yı seçin.

![FTP bilgilerini Kopyala](./media/app-service-deploy-ftp/ftp-dashboard.png)

Uygulama **kimlik bilgilerini** , her bir uygulama için benzersiz olduğundan uygulamanıza dağıtmak için kullanmanız önerilir. Ancak, **Kullanıcı kimlik bilgileri**' ne tıklarsanız, aboneliğinizdeki tüm App SERVICE uygulamalarda FTP/S oturumu için kullanabileceğiniz Kullanıcı düzeyi kimlik bilgilerini ayarlayabilirsiniz.

> [!NOTE]
> Kullanıcı düzeyi kimlik bilgilerini kullanan bir FTP/FTPS uç noktasında kimlik doğrulaması yapmak için aşağıdaki biçimde bir Kullanıcı adı gerekir: 
>
>`<app-name>\<user-name>`
>
> Kullanıcı düzeyi kimlik bilgileri, belirli bir kaynak değil kullanıcıya bağlı olduğundan, oturum açma işlemini doğru uygulama uç noktasına yönlendirmek için Kullanıcı adının bu biçimde olması gerekir.
>

## <a name="deploy-files-to-azure"></a>Azure 'a dosya dağıtma

1. FTP istemcinizden (örneğin, [Visual Studio](https://www.visualstudio.com/vs/community/), [siduck](https://cyberduck.io/)veya [WinSCP](https://winscp.net/index.php)), uygulamanıza bağlanmak için topladığınız bağlantı bilgilerini kullanın.
2. Dosyalarınızı ve ilgili dizin yapılarını Azure 'daki [ **/site/Wwwroot** dizinine](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) (veya WebJobs için **/site/Wwwroot/App_Data/Jobs/** dizininden) kopyalayın.
3. Uygulamanın düzgün çalıştığını doğrulamak için uygulamanızın URL 'sine gidin. 

> [!NOTE] 
> [Git tabanlı dağıtımlardan](deploy-local-git.md)farklı olarak, FTP dağıtımı aşağıdaki dağıtım önerilerini desteklemez: 
>
> - bağımlılık geri yüklemeleri (NuGet, NPM, PI ve besteci tahminleri gibi)
> - .NET ikili dosyalarının derlenmesi
> - web.config oluşturma ( [Node.js bir örnek](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Bu gerekli dosyaları yerel makinenizde el ile oluşturun ve ardından uygulamanızla birlikte dağıtın.
>

## <a name="enforce-ftps"></a>FTPS 'yi zorla

Gelişmiş güvenlik için yalnızca TLS/SSL üzerinden FTP 'ye izin vermeniz gerekir. FTP dağıtımını kullanmıyorsanız hem FTP hem de FTPS 'yi devre dışı bırakabilirsiniz.

Uygulamanızın kaynak sayfasında, [Azure Portal](https://portal.azure.com)' de,   >  sol gezinmede yapılandırma **genel ayarları** ' nı seçin.

Şifrelenmemiş FTP 'yi devre dışı bırakmak için, **FTPS** ' yi yalnızca **FTP durumunda** seçin. Hem FTP hem de FTPS 'yi tamamen devre dışı bırakmak için **devre dışı** seçeneğini belirleyin İşlemi tamamladıktan sonra **Kaydet**’e tıklayın. **Yalnızca FTPS** kullanıyorsanız, Web uygulamanızın **TLS/SSL ayarları** dikey penceresine giderek TLS 1,2 veya üstünü zorlayabilmeniz gerekir. TLS 1,0 ve 1,1 **yalnızca FTPS** ile desteklenmez.

![FTP/S 'yi devre dışı bırak](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure)kullanarak FTP dağıtımı için bkz. [Web UYGULAMASı oluşturma ve FTP ile dosya DAĞıTMA (Azure CLI)](./scripts/cli-deploy-ftp.md).

[Azure PowerShell](/cli/azure)kullanarak FTP dağıtımı için bkz. [FTP (PowerShell) kullanarak bir Web uygulamasına dosya yükleme](./scripts/powershell-deploy-ftp.md).

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>FTP dağıtımı sorunlarını giderme

- [Uygulamanızı FTP/S kullanarak Azure App Service dağıtma](#deploy-your-app-to-azure-app-service-using-ftps)
  - [FTP panosunu aç](#open-ftp-dashboard)
  - [FTP bağlantı bilgilerini al](#get-ftp-connection-information)
  - [Azure 'a dosya dağıtma](#deploy-files-to-azure)
  - [FTPS 'yi zorla](#enforce-ftps)
  - [Betiklerle otomatikleştirme](#automate-with-scripts)
  - [FTP dağıtımı sorunlarını giderme](#troubleshoot-ftp-deployment)
    - [FTP dağıtımı ile ilgili sorunları nasıl giderebilirim?](#how-can-i-troubleshoot-ftp-deployment)
    - [Kodumu FTP ve yayınlayamıyorum. Sorunu nasıl çözebilirim?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
    - [Pasif mod aracılığıyla Azure App Service FTP 'ye nasıl bağlanabilirim?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)
  - [Sonraki adımlar](#next-steps)
  - [Diğer kaynaklar](#more-resources)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>FTP dağıtımı ile ilgili sorunları nasıl giderebilirim?

FTP dağıtımında sorun gidermeye yönelik ilk adım, bir çalışma zamanı uygulama sorunundan bir dağıtım sorununu yalıtmaya yöneliktir.

Dağıtım sorunu genellikle uygulamanıza hiçbir dosya veya yanlış dosya dağıtılmasına neden olur. FTP dağıtımınızı inceleyerek veya alternatif bir dağıtım yolu (örneğin, kaynak denetimi) seçerek sorun giderebilirsiniz.

Çalışma zamanı uygulama sorunu genellikle uygulamanıza dağıtılan doğru dosya kümesine, ancak yanlış uygulama davranışına neden olur. Çalışma zamanında kod davranışına odaklanarak ve belirli hata yollarını inceleyerek sorun gidermeye devam edebilirsiniz.

Bir dağıtımı veya çalışma zamanı sorununu anlamak için bkz. [dağıtım ve çalışma zamanı sorunları](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>FTP kullanamıyorum ve kodumu yayımlayamıyorum. Sorunu nasıl çözebilirim?
Doğru ana bilgisayar adını ve [kimlik bilgilerini](#open-ftp-dashboard)girdiğinizden emin olun. Ayrıca, makinenizde bulunan aşağıdaki FTP bağlantı noktalarının bir güvenlik duvarı tarafından engellenmediğinden emin olun:

- FTP denetim bağlantı noktası: 21, 990
- FTP veri bağlantısı bağlantı noktası: 989, 10001-10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Pasif mod aracılığıyla Azure App Service FTP 'ye nasıl bağlanabilirim?
Azure App Service hem etkin hem de Pasif mod aracılığıyla bağlanmayı destekler. Dağıtım makineleriniz genellikle bir güvenlik duvarının arkasında (işletim sisteminde veya bir ev veya iş ağının parçası olarak) olduğundan Pasif mod tercih edilir. [WinSCP belgelerinden bir örnek](https://winscp.net/docs/ui_login_connection)görüntüleyin. 

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için [Git Ile Azure 'a](deploy-local-git.md)dağıtım yapmayı deneyin. Azure 'a git tabanlı dağıtım, sürüm denetimi, paket geri yükleme, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Diğer kaynaklar

* [Azure App Service dağıtım kimlik bilgileri](deploy-configure-credentials.md)