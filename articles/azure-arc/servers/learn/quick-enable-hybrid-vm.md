---
title: Azure Arc etkin sunucularıyla karma makineyi bağlama
description: Azure Arc etkin sunucularıyla karma makinenizi bağlamayı ve kaydetmeyi öğrenin.
ms.topic: quickstart
ms.date: 09/23/2020
ms.openlocfilehash: b57f30821a105a99041d8187716b75096116ea8e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91327893"
---
# <a name="quickstart-connect-hybrid-machine-with-azure-arc-enabled-servers"></a>Hızlı başlangıç: Azure Arc etkin sunucularıyla karma makineyi bağlama

[Azure yay özellikli sunucular](../overview.md) , şirket içi, kenar ve çok yüksek ortamlarda barındırılan Windows ve Linux makinelerinizi yönetmenizi ve yönetmenizi sağlar. Bu hızlı başlangıçta, bağlı makine aracısını, Arc etkin sunucularıyla yönetim için Azure dışında barındırılan Windows veya Linux makinenizde dağıtıp yapılandıracaksınız.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Yay özellikli sunucular dağıtımı karma bağlı makine Aracısı, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerine sahip olmanızı gerektirir. Linux 'ta, kök hesabı ve Windows 'ta yerel Yöneticiler grubunun üyesi olan bir hesapla.

* Başlamadan önce, aracı [önkoşullarını](../agent-overview.md#prerequisites) gözden geçirdiğinizden emin olun ve aşağıdakileri doğrulayın:

    * Hedef makineniz desteklenen bir [işletim sistemi](../agent-overview.md#supported-operating-systems)çalıştırıyor.

    * Hesabınıza [gerekli Azure rollerine](../agent-overview.md#required-permissions)atama verildi.

    * Makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya ara sunucu üzerinden bağlanıyorsa, [listelenen](../agent-overview.md#networking-configuration) URL 'lerin engellenmediğinden emin olun.

    * Azure Arc etkin sunucuları yalnızca [burada](../overview.md#supported-regions)belirtilen bölgeleri destekler.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

## <a name="register-azure-resource-providers"></a>Azure Kaynak sağlayıcılarını kaydetme

Azure Arc etkin sunucuları, bu hizmeti kullanabilmeniz için aboneliğinizde aşağıdaki Azure Kaynak sağlayıcılarına bağımlıdır:

* Microsoft. HybridCompute
* Microsoft. GuestConfiguration

Aşağıdaki komutları kullanarak bunları kaydedin:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

## <a name="generate-installation-script"></a>Yükleme betiği oluştur

Azure Arc ile indirme, yükleme ve bağlantı kurma işlemlerini otomatik hale getirmeye yönelik betik, Azure portal kullanılabilir. İşlemi gerçekleştirmek için aşağıdakileri yapın:

1. **Tüm hizmetler**' e tıklayarak ve ardından **makineler-Azure yay**' i arayıp seçerek Azure yay hizmetini Azure Portal başlatın.

    :::image type="content" source="./media/quick-enable-hybrid-vm/search-machines.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

1. **Makineler-Azure yay** sayfasında sol üst kısımdaki **Ekle**' yi veya orta bölmenin altındaki **makine-Azure yayı oluştur** seçeneğini belirleyin.

1. **Yöntem seçin** sayfasında, **etkileşimli betiği kullanarak makine Ekle** kutucuğunu seçin ve ardından **betik oluştur**' u seçin.

1. **Betik oluştur** sayfasında, makinenin Azure 'da yönetilmesini istediğiniz aboneliği ve kaynak grubunu seçin. Makine meta verilerinin depolanacağı Azure konumunu seçin.

1. **Betik oluştur** sayfasında, **işletim sistemi** açılan listesinde, betiğin üzerinde çalıştığı işletim sistemini seçin.

1. Makine Internet 'e bağlanmak için bir proxy sunucusu üzerinden iletişim kurduğundan, Ileri ' yi seçin **: ara sunucu**.

1. **Proxy sunucusu** sekmesinde, proxy sunucusu IP adresini veya makinenin proxy sunucusuyla iletişim kurmak için kullanacağı adı ve bağlantı noktası numarasını belirtin. Değeri biçiminde girin `http://<proxyURL>:<proxyport>` .

1. **Gözden geçir + oluştur**' u seçin.

1. **Gözden geçir + oluştur** sekmesinde Özet bilgilerini gözden geçirin ve ardından **İndir**' i seçin. Hala değişiklik yapmanız gerekiyorsa, **önceki**' yi seçin.

## <a name="install-the-agent-using-the-script"></a>Betiği kullanarak aracıyı yükler

### <a name="windows-agent"></a>Windows aracısı

1. Sunucusunda oturum açın.

1. Yükseltilmiş bir 64 bit PowerShell komut istemi açın.

1. Betiği kopyaladığınız klasöre veya paylaşıma değiştirin ve betiği çalıştırarak sunucuda yürütün `./OnboardingScript.ps1` .

### <a name="linux-agent"></a>Linux Aracısı

1. Linux aracısını doğrudan Azure ile iletişim kurabilen hedef makineye yüklemek için şu komutu çalıştırın:

    ```bash
    bash ~/Install_linux_azcmagent.sh
    ```

    * Hedef makine bir ara sunucu üzerinden iletişim kuruyorsa, aşağıdaki komutu çalıştırın:

        ```bash
        bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
        ```

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. Makinenizde [Azure Portal](https://aka.ms/hybridmachineportal)görüntüleyin.

:::image type="content" source="./media/quick-enable-hybrid-vm/enabled-machine.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

## <a name="next-steps"></a>Sonraki adımlar

Linux veya Windows hibrit makinenizi etkinleştirmiş ve hizmete başarıyla bağlandığınıza göre, Azure Ilkesini Azure 'da uyumluluğu anlamak üzere etkinleştirmeye hazırsınız.

Log Analytics aracısının yüklü olmadığı Azure Arc etkin sunucuların etkin makinesini nasıl tanımlayacağınızı öğrenmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [Uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma](tutorial-assign-policy-portal.md)
