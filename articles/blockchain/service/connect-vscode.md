---
title: Azure blok zinciri hizmetine bağlanmak için Visual Studio Code kullanma
description: Visual Studio Code 'de Ethereum uzantısı için Azure blok zinciri geliştirme setini kullanarak bir Azure blok zinciri hizmeti Konsorsiyumu ağı 'na bağlanma
ms.date: 04/22/2020
ms.topic: quickstart
ms.reviewer: caleteet
ms.openlocfilehash: 8b502966317c5d07e89de4ae70ff72b899e963e6
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "82084847"
---
# <a name="quickstart-use-visual-studio-code-to-connect-to-an-azure-blockchain-service-consortium-network"></a>Hızlı başlangıç: Azure blok zinciri hizmeti Consortium ağına bağlanmak için Visual Studio Code kullanma

Bu hızlı başlangıçta, Azure blok zinciri hizmetinde bir konsorsiyunuza eklemek üzere Ethereum Visual Studio Code (VS Code) uzantısı için Azure blok zinciri geliştirme setini yüklersiniz ve kullanıyorsunuz. Azure blok zinciri geliştirme seti, Ethereum blok zinciri defterlerindeki akıllı sözleşmeleri oluşturma, bağlama, derleme ve dağıtma işlemlerini basitleştirir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Azure Portal veya hızlı başlangıç kullanarak bir blok zinciri üyesi oluşturma](create-member.md) [: Azure CLI kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi](create-member-cli.md) oluşturma
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Ethereum uzantısı için Azure blok zinciri geliştirme seti](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node.js 10.15. x veya üzeri](https://nodejs.org)
* [Git 2.10. x veya üzeri](https://git-scm.com)
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/) Yolunuza python.exe ekleyin. Azure blok zinciri geliştirme seti için yolunuzda Python sürümü 2.7.15 olması gerekir.
* [Truffle 5.0.0](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLı 6.0.0](https://github.com/trufflesuite/ganache-cli)

Windows 'da, Node-JP modülü için yüklü bir C++ derleyicisi gereklidir. MSBuild araçlarını kullanabilirsiniz:

* Visual Studio 2017 yüklüyse, NPM 'yi komutuyla MSBuild araçlarını kullanacak şekilde yapılandırın `npm config set msvs_version 2017 -g`
* Visual Studio 2019 yüklüyse, NPM için MS derleme araçları yolunu ayarlayın. Örneğin, `npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"`
* Aksi takdirde, tek başına VS Build araçlarını kullanarak `npm install --global windows-build-tools` yükseltilmiş bir *yönetici olarak çalıştır* komut kabuğu ' nu kullanın.

Node-cayp hakkında daha fazla bilgi için [GitHub 'daki node-cayp deposuna](https://github.com/nodejs/node-gyp)bakın.

### <a name="verify-azure-blockchain-development-kit-environment"></a>Azure blok zinciri geliştirme seti ortamını doğrulama

Azure blok zinciri geliştirme seti, geliştirme ortamı önkoşullarının karşılandığını doğrular. Geliştirme ortamınızı doğrulamak için:

VS Code komut paletinde **Azure blok Zinciri: karşılama sayfasını göster**' i seçin.

Azure blok zinciri geliştirme seti, tamamlanmasının yaklaşık bir dakika süren bir doğrulama betiği çalıştırır. Bu çıktıyı, **terminal > yeni Terminal**' i seçerek görüntüleyebilirsiniz. Terminal menü çubuğunda, açılan menüde **Çıkış** sekmesini ve **Azure blok zincirini** seçin. Başarılı doğrulama aşağıdaki görüntüde olduğu gibi görünür:

![Geçerli geliştirme ortamı](./media/connect-vscode/valid-environment.png)

 Gerekli bir aracı eksik ise, **Azure blok zinciri geliştirme seti-önizleme** adlı yeni bir sekme, indirme bağlantılarıyla gerekli araçları listeler.

![Geliştirme Seti gerekli uygulamalar](./media/connect-vscode/required-apps.png)

Hızlı başlangıç ile devam etmeden önce eksik önkoşulları yükler.

## <a name="connect-to-consortium-member"></a>Consortium üyesine Bağlan

Azure blok zinciri geliştirme seti VS Code uzantısını kullanarak, konsorsiyum üyelerine bağlanabilirsiniz. Bir konsorsiyumun bağlantısı kurulduktan sonra, akıllı sözleşmeleri bir Azure blok zinciri hizmeti Consortium üyesine derleyebilir, oluşturabilir ve dağıtabilirsiniz.

Azure blok zinciri hizmeti Consortium üyesine erişiminiz yoksa, önkoşul hızlı başlangıcını doldurun: Azure portal veya hızlı başlangıç: Azure CLI kullanarak [bir blok zinciri üyesi oluşturma](create-member.md) [: Azure CLI kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi](create-member-cli.md)oluşturma.

1. VS Code gezgin bölmesinde, **Azure blok zinciri** uzantısını genişletin.
1. **Ağa bağlan**' ı seçin.

   ![Ağa bağlan](./media/connect-vscode/connect-consortium.png)

    Azure kimlik doğrulaması istenirse, bir tarayıcı kullanarak kimlik doğrulaması yapmak için istemleri izleyin.
1. Komut paleti açılan menüsünde **Azure blok zinciri hizmeti** ' ni seçin.
1. Azure blok zinciri hizmeti Consortium üyele ilişkili aboneliği ve kaynak grubunu seçin.
1. Listeden Consortium ' ı seçin.

Konsorsiyum ve blok zinciri üyeleri VS Code gezgin kenar çubuğunda listelenir.

![Gezgin 'de görünen konsorsiyum](./media/connect-vscode/consortium-node.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure blok zinciri hizmeti 'nde bir konsorsiyunuza eklemek üzere Ethereum VS Code uzantısı için Azure blok zinciri geliştirme setini kullandınız. Bir işlem aracılığıyla akıllı sözleşme işlevi oluşturmak, derlemek, dağıtmak ve yürütmek için Ethereum için Azure blok zinciri geliştirme setini kullanmak üzere bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetinde akıllı sözleşmeler oluşturma, derleme ve dağıtma](send-transaction.md)