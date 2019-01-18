---
title: Uzaktan izleme çözümünü yerel olarak (Visual Studio Code) - Azure'ı dağıtma | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda test ve geliştirme için Visual Studio Code kullanarak yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtmayı gösterir.
author: avneet723
manager: hegate
ms.author: avneet723
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 6e2fafa398b09d0822c4582e196345b812e6fc52
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54392248"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio-code"></a>Uzaktan izleme çözüm Hızlandırıcısını yerel olarak - Visual Studio kod dağıtma

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, test ve geliştirme için yerel makinenize Uzaktan izleme çözüm Hızlandırıcısını dağıtma işlemini göstermektedir. Mikro hizmetler, Visual Studio Code'da çalıştırmayı öğrenin. Yerel mikro hizmetlerin dağıtımı aşağıdaki bulut hizmetlerini kullanır: IOT Hub, Cosmos DB, Azure akış analizi ve Azure Time Series Insights.

## <a name="prerequisites"></a>Önkoşullar

Uzaktan izleme çözüm Hızlandırıcısını tarafından kullanılan Azure Hizmetleri dağıtmak için bir etkin Azure aboneliği gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine Kurulumu

Yerel dağıtımını tamamlamak için aşağıdaki araçları, yerel geliştirme makinenizde yüklü gerekir:

* [Git](https://git-scm.com/)
* [.Net Core](https://dotnet.microsoft.com/download)
* [Docker](https://www.docker.com)
* [Nginx](http://nginx.org/en/download.html)
* [Visual Studio Code](https://code.visualstudio.com/)
* [VS Code'nın C# uzantısı](https://code.visualstudio.com/docs/languages/csharp)
* [Node.js v8](https://nodejs.org/) -bu yazılımları Azure kaynaklarını oluşturmayı betiklerini kullanan bilgisayarları CLI önkoşuldur. Node.js v10 kullanmayın

> [!NOTE]
> Visual Studio Code, Windows, Mac ve Ubuntu için kullanılabilir.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Mikro Hizmetleri çalıştırın

Bu bölümde, Uzaktan izleme mikro Hizmetleri çalıştırın. Web kullanıcı Arabirimi yerel olarak çalıştırdığınızda Docker cihaz benzetimi hizmetinde ve Visual Studio code'da mikro hizmetler.

### <a name="build-the-code"></a>Kodu oluşturma

Komut isteminde azure-iot-pcs-remote-monitoring-dotnet\services gidin ve Kodu derlemek için aşağıdaki komutları çalıştırın.

```cmd
dotnet restore
dotnet build -c Release
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Yerel makinede diğer mikro hizmetlerin dağıtımı

Aşağıdaki adımlar Visual Studio 2017'de Uzaktan izleme mikro hizmetleri çalıştırma işlemini gösterir:

1. Visual Studio Code'u başlatın.
1. Açık **azure-iot-pcs-remote-monitoring-dotnet** yerel kopyanızı VS code'da modülünden.
1. Dosyaları kopyalama **launch.json** ve **tasks.json** scripts\local\launch\idesettings\vscode gelen\. Yeni bir klasör oluşturun **azure-iot-pcs-remote-monitoring-dotnet\.vscode** dosyaların yapıştırın.
1. VS Code'da hata ayıklama panelini açın ve çalıştırın **tüm mikro hizmetleri çalıştırmak** yapılandırma. Bu yapılandırma, cihaz benzetimi mikro hizmet Docker'da çalıştırılır ve diğer mikro hizmetler hata ayıklayıcıda çalıştırır.

Çıkış için örnek olarak **Auth** hizmeti **hata ayıklama konsolunu** aşağıdaki gibi görünür:

[![Yerel-kimlik doğrulama-hizmeti Dağıt](./media/deploy-locally-vscode/auth-debug-results-inline.png)](./media/deploy-locally-vscode/auth-debug-results-expanded.png#lightbox)

### <a name="run-the-web-ui"></a>Web kullanıcı arabirimini çalıştırma

Bu adımda, web kullanıcı Arabirimi başlatın. Gidin **azure-iot-pcs-remote-monitoring-dotnet\webui** yerel klasöre kopyalayın ve aşağıdaki komutları çalıştırın:

```cmd
npm install
npm start
```

Başlangıç tamamlandıktan sonra tarayıcınızı sayfası görüntüler **http://localhost:3000/dashboard**. Bu sayfadaki hataları beklenmektedir. Uygulama hatasız görüntülemek için aşağıdaki adımı tamamlayın.

### <a name="configure-and-run-nginx"></a>Yapılandırma ve NGINX çalıştırma

Yerel makinenizde çalışan mikro hizmetler ve web uygulaması bağlamak için bir ters proxy sunucuyu ayarlayın:

* Kopyalama **nginx.conf** dosya **webui\scripts\localhost** klasörüne **nginx\conf** yükleme dizini.
* Çalıştırma **ngınx**.

Çalıştırma hakkında daha fazla bilgi için **ngınx**, bkz: [nginx Windows için](http://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan izleme çözümü panosuna erişmek için gidin [ http://localhost:9000 ](http://localhost:9000) tarayıcınızda.

## <a name="clean-up"></a>Temizleme

Gereksiz önlemek için bulut Hizmetleri ücretleri sınamanızı tamamladığınızda, Azure aboneliğinizden kaldırın. Hizmetlerini kaldırmak için gidin [Azure portalında](https://ms.portal.azure.com) ve delete kaynak grubunda **start.cmd** oluşturulan komut dosyası.

Ayrıca, kaynak kodunu github'dan kopyaladığınız oluşturulan uzaktan izleme depo yerel kopyasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan izleme çözüm dağıttığınıza göre sonraki adım olarak [çözüm panosunun özelliklerini keşfedin](quickstart-remote-monitoring-deploy.md).