---
title: Azure Terraform VS Code Modül Oluşturucu
description: Yeoman'ı kullanarak Terraform temel şablonu oluşturma hakkında bilgi edinin.
services: terraform
ms.service: terraform
keywords: terraform, devops, sanal makine, azure, yeoman
author: v-mavick
manager: jeconnoc
ms.author: v-mavick
ms.topic: tutorial
ms.date: 09/12/2018
ms.openlocfilehash: 513b123c44cf2cd37cf81a91e0d2da9599eb1fcd
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396423"
---
# <a name="create-a-terraform-base-template-using-yeoman"></a>Yeoman kullanarak Terraform temelli şablon oluşturma

[Terraform](https://docs.microsoft.com/azure/terraform/
), Azure'da kolayca altyapı oluşturmanın yolunu sağlar. [Yeoman](http://yeoman.io/), modül geliştiricisinin Terraform modülleri oluşturma işini büyük ölçüde kolaylaştırırken mükemmel bir *en iyi deneyim* çerçevesi sağlar.

Bu makalede, Yeoman modül oluşturucusunu kullanarak temel Terraform şablonu oluşturma hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Ön koşullar

- Windows 10, Linux veya macOS 10.10+ çalıştıran bir bilgisayar.
- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- **Visual Studio Code**: Yeoman oluşturucusu tarafından oluşturulan dosyaları incelemek için [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) kullanacağız. Ancak, tercih ettiğiniz herhangi bir kod düzenleyiciyi kullanabilirsiniz.
- **Terraform**: Yeoman tarafından oluşturulan modülü çalıştırmak için [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ) yüklü olmalıdır.
- **Docker**: Yeoman oluşturucusu tarafından oluşturulan modülü çalıştırmak için [Docker](https://www.docker.com/get-started) kullanacağız. (Tercih ederseniz örnek modülü çalıştırmak için Docker yerine Ruby kullanabilirsiniz.)
- **Go programlama dili**: Yeoman tarafından oluşturulan test çalışmaları Go dilinde yazıldığı için [Go](https://golang.org/) yüklü olmalıdır.

>[!NOTE]
>Bu öğreticideki yordamların birçoğu, komut satırı girişleri içerir. Burada açıklanan adımlar tüm işletim sistemleri ve komut satırı araçları için geçerlidir. Örneklerimizde PowerShell kullanmayı seçtik. Ancak Git Bash, Windows komut istemleri veya Linux ya da macOS komut satırı komutları gibi çeşitli alternatiflerden herhangi birini kullanabilirsiniz.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

### <a name="install-nodejs"></a>Node.js yükleme

Terraform'u Cloud Shell'de kullanabilmek için [Node.js](https://nodejs.org/en/download/) 6.0+ sürümünü yüklemeniz gerekir.

>[!NOTE]
>Node.js uygulamasının yüklü olup olmadığını belirlemek için bir terminal penceresi açıp `node --version` yazın.

### <a name="install-yeoman"></a>Yeoman’ı yükleme

Bir komut isteminden `npm install -g yo` komutunu girin

![Yeoman’ı yükleme](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Terraform modülü için Yeoman şablonunu yükleyin

Bir komut isteminden `npm install -g generator-az-terra-module` komutunu girin.

![generator-az-terra-module yükleyin](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Yeoman’ın yüklü olduğunu doğrulamak için bir terminal penceresinden `yo --version` komutunu girin.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Yeoman tarafından oluşturulan modülü tutmak için boş bir klasör oluşturun

Yeoman şablonu **geçerli dizinde** dosyaları oluşturur. Bu nedenle, bir dizin oluşturmanız gerekir.

>[!Note]
>Bu boş dizinin $GOPATH/src altına yerleştirilmesi gerekir. Bunu yapmak için yönergeleri [burada](https://github.com/golang/go/wiki/SettingGOPATH) bulabilirsiniz.

Bir komut isteminden:

1. Oluşturmak üzere olduğumuz yeni, boş dizini içermesini istediğiniz üst dizine gidin.
1. `mkdir <new-directory-name>` yazın.

    >[!NOTE]
    ><new-directory-name> değerini yeni dizininizin adıyla değiştirin. Biz bu örnekte, yeni dizini `GeneratorDocSample` olarak adlandırdık.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. `cd <new directory's name>` yazıp **enter** tuşuna basarak yeni dizine gidin.

    ![Yeni dizininize gidin](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Bu dizinin boş olduğundan emin olmak için `ls` yazın. Bu komutun elde edilen çıktısında listelenmiş hiçbir dosya olmamalıdır.

## <a name="create-a-base-module-template"></a>Temel modül şablonu oluşturma

Bir komut isteminden:

1. `yo az-terra-module` yazın.

1. Ekrandaki yönergeleri izleyerek aşağıdaki bilgileri sağlayın:

    - *Terraform modülü proje adı*

        ![Proje adı](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >Bu örnekte `doc-sample-module` girdik.

    - *Docker görüntü dosyasını eklemek ister misiniz?*

        ![Docker görüntü dosyası eklensin mi?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >`y` yazın. **n** değerini seçerseniz, oluşturulan modül kodu yalnızca yerel modda çalışmayı destekler.

3. Oluşturulan dosyaları görüntülemek için `ls` girin.

    ![Oluşturulan dosyaları listeleme](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Oluşturulan modülü kodunu gözden geçirme

1. Visual Studio Code'u başlatma

1. Menü çubuğundan **Dosya > Klasör Aç**'ı ve oluşturduğunuz klasörü seçin.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Yeoman modül oluşturucusu tarafından oluşturulan dosyalardan bazılarına göz atalım.

>[!Note]
>Bu makalede, Yeoman modül oluşturucusu tarafından oluşturulan main.tf, variables.tf ve outputs.tf dosyalarını kullanacağız. Ancak, kendi modüllerinizi oluştururken bu dosyaları Terraform modülünüzün işlevselliğine uyacak şekilde düzenleyebilirsiniz. Bu dosyaların ve kullanımlarının daha ayrıntılı bir tartışması için bkz. [Terraform Modüllerinde Terratest.](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster)

### <a name="maintf"></a>main.tf

*random-shuffle* adlı bir modülü tanımlar. Girdi, *string_list* şeklindedir. Çıktı, permütasyon sayısıdır.

### <a name="variablestf"></a>variables.tf

Modül tarafından kullanılan girdi ve çıktı değişkenlerini tanımlar.

### <a name="outputstf"></a>outputs.tf

Modülün çıktısını tanımlar. Burada, yerleşik bir Terraform modülü olan **random_shuffle** tarafından döndürülen değerdir.

### <a name="rakefile"></a>Rakefile

Derleme adımlarını tanımlar. Bu adımlar şunlardır:

- **build**: main.tf dosyasının biçimlendirmesini doğrular.
- **unit**: Oluşturulan modül iskeleti, bir birim testi için kod içermez. Bir birim testi senaryosu belirtmek isterseniz, o kodu burada eklersiniz.
- **e2e**: Modülün uçtan uca testini çalıştırır.

### <a name="test"></a>test

- Test çalışmaları Go dilinde yazılır.
- Testteki tüm kodlar uçtan uca testlerdir.
- Uçtan uca testler **fixture** altında tanımlanan tüm öğeleri sağlamak üzere Terraform kullanır ve sonra **template_output.go** kodundaki çıktıyı önceden tanımlı beklenen değerler ile karşılaştırır.
- **Gopkg.lock** ve **Gopkg.toml**: Bağımlılıklarınızı tanımlar. 

## <a name="test-the-module-using-docker"></a>Docker kullanarak modülü test etme

>[!NOTE]
>Örneğimizde, modülü yerel bir modül olarak çalıştırıyoruz ve Azure’a gerçek anlamda temas etmiyoruz.

### <a name="confirm-docker-is-installed-and-running"></a>Docker’ın yüklü ve çalışır durumda olduğunu onaylayın

Bir komut isteminden `docker version` komutunu girin.

![Docker sürümü](media/terraform-vscode-module-generator/ymg-docker-version.png)

Elde edilen çıktı, Docker'ın yüklü olduğunu onaylar.

Docker’ın gerçekten çalışır durumda olduğunu onaylamak için `docker info` girin.

![Docker bilgisi](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Docker kapsayıcısı ayarlama

1. Bir komut isteminden şu komutu girin:

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    **Başarıyla derlendi** iletisi gösterilir.

    ![Başarıyla derlendi](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Komut isteminden `docker image ls` komutunu girin.

    Yeni oluşturduğunuz *terra-mod-example* modülünü listede görürsünüz.

    ![Depo sonuçları](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >Modül adı *terra-mod-example*, yukarıdaki 1. adımda girdiğiniz komutta belirtilmiştir.

1. `docker run -it terra-mod-example /bin/sh` yazın.

    Docker şu anda çalışmaktadır ve `ls` girilerek dosya listelenebilir.

    ![Docker dosyasını listeleme](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

1. `bundle install` yazın.

    **Paket tamamlandı** iletisini bekleyin, ardından sonraki adıma geçin.

1. `rake build` yazın.

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="perform-the-end-to-end-test"></a>Uçtan uca test gerçekleştirme

1. `rake e2e` yazın.

1. Birkaç dakika sonra **BAŞARILI** iletisi görünür.

    ![BAŞARILI](media/terraform-vscode-module-generator/ymg-pass.png)

1. Uçtan uca testi tamamlamak için `exit` girin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Terraform Visual Studio Code uzantısını yükleme ve kullanma.](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
