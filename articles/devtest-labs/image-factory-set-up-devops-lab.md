---
title: Azure DevTest Labs 'de Azure DevOps 'dan bir görüntü fabrikası çalıştırın
description: Bu makalede, Azure DevOps 'tan (eski adıyla Visual Studio Team Services) görüntü fabrikası çalıştırmak için gereken tüm hazırlıklar ele alınmaktadır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: fa7050bae1ff8681e04b6ab38220be9eaf38a64a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85476147"
---
# <a name="run-an-image-factory-from-azure-devops"></a>Azure DevOps’tan bir görüntü fabrikası çalıştırma
Bu makalede, Azure DevOps 'tan (eski adıyla Visual Studio Team Services) görüntü fabrikası çalıştırmak için gereken tüm hazırlıklar ele alınmaktadır.

> [!NOTE]
> Herhangi bir düzenleme altyapısı çalışacaktır! Azure DevOps zorunlu değildir. Görüntü fabrikası Azure PowerShell betikleri kullanılarak çalıştırılır. bu nedenle, Windows Görev Zamanlayıcı, diğer CI/CD sistemleri ve benzeri bir kullanarak el ile çalıştırılabilir.

## <a name="create-a-lab-for-the-image-factory"></a>Görüntü fabrikası için Laboratuvar oluşturma
Görüntü fabrikası ayarlamanın ilk adımı Azure DevTest Labs bir laboratuvar oluşturmaktır. Bu laboratuvar, sanal makineleri oluşturduğumuz ve özel görüntüleri kaydettiğiniz Image Factory laboratuvarıdır. Bu laboratuvar, genel görüntü fabrikası sürecinin bir parçası olarak kabul edilir. Bir laboratuvar oluşturduktan sonra, daha sonra ihtiyaç duyacağınız için, bu adı kaydettiğinizden emin olun.

## <a name="scripts-and-templates"></a>Betikler ve şablonlar
Takımınızın görüntü fabrikasını benimsedeki bir sonraki adım, nelerin kullanılabildiğini öğrenmektir. Görüntü fabrikası betikleri ve şablonları, [DevTest Labs GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory)deposunda herkese açık bir şekilde bulunabilir. Parçaların ana hattı aşağıda verilmiştir:

- Görüntü fabrikası. Kök klasörüdür.
    - Yapılandırmada. Görüntü fabrikası için girişler
        - Altın Dengörüntüler. Bu klasör, özel görüntülerin tanımlarını temsil eden JSON dosyalarını içerir.
        - Üzerinde Labs.js. Takım, belirli özel görüntüleri almak için kaydolan dosya.
- Betikler. Görüntü fabrikası için altyapı.

Bu bölümdeki makaleler, bu betikler ve şablonlar hakkında daha fazla ayrıntı sağlar.

## <a name="create-an-azure-devops-team-project"></a>Azure DevOps takım projesi oluşturma
Azure DevOps, kaynak kodu depolamanızı sağlar, Azure PowerShell tek bir yerde çalıştırabilir. Görüntüleri güncel tutmak için yinelenen çalıştırmalar zamanlayabilirsiniz. Herhangi bir sorunu tanılamak için sonuçları günlüğe kaydetmeye yönelik iyi olanaklar vardır.  Azure DevOps 'ın kullanılması bir gereksinim değildir ancak Azure 'a bağlanabilecek ve Azure PowerShell çalıştırabileceğiniz herhangi bir bandı/altyapıyı kullanabilirsiniz.

Yerine kullanmak istediğiniz mevcut bir DevOps hesabınız veya projeniz varsa, bu adımı atlayın.

Başlamak için Azure DevOps 'da ücretsiz bir hesap oluşturun. https://www.visualstudio.com/ **Azure DevOps** (eski adıyla VSTS) altında **ücretsiz olarak kullanmaya** başlayın ' ı ziyaret edin ve seçin. Benzersiz bir hesap adı seçmeniz gerekir ve git kullanarak kodu yönetmeyi seçtiğinizden emin olun. Bu oluşturulduktan sonra, URL 'YI takım projenize kaydedin. Örnek bir URL aşağıda verilmiştir: `https://<accountname>.visualstudio.com/MyFirstProject` .

## <a name="check-in-the-image-factory-to-git"></a>Git 'e görüntü fabrikası iade etme
Görüntü fabrikası için tüm PowerShell, şablonlar ve yapılandırmalar [ortak DevTest Labs GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory)deposunda bulunur. Yeni takım projenize kodu almanın en hızlı yolu bir depoyu içeri aktarmanız. Bu, tüm DevTest Labs deposunu çeker (böylece ek belgeler ve örnekler alacaksınız).

1. Önceki adımda oluşturduğunuz Azure DevOps projesini ziyaret edin (URL, **https: \/ / \<accountname> . VisualStudio.com/MyFirstProject**gibi görünür).
2. **Depoyu Içeri aktar**' ı seçin.
3. DevTest Labs deposunun **kopya URL** 'sini girin: `https://github.com/Azure/azure-devtestlab` .
4. **İçeri aktar**'ı seçin.

    ![Git deposunu içeri aktar](./media/set-up-devops-lab/import-git-repo.png)

Yalnızca tam olarak gerekenleri (görüntü fabrikası dosyaları) iade etme kararı verirseniz, git deposunu kopyalamak ve yalnızca **betikler/ımagefactory** dizininde bulunan dosyaları göndermek için [buradaki](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs) adımları izleyin.

## <a name="create-a-build-and-connect-to-azure"></a>Derleme oluşturma ve Azure 'a bağlanma
Bu noktada, Azure DevOps 'daki bir git deposunda depolanan kaynak dosyaları vardır. Şimdi, Azure PowerShell çalıştırmak için bir işlem hattı ayarlamanız gerekir. Bu adımları uygulamak için çok sayıda seçenek vardır. Bu makalede, basitlik için derleme tanımını kullanırsınız, ancak DevOps Build, DevOps sürümü (tek veya birden çok ortam), Windows Görev Zamanlayıcı gibi diğer yürütme motorları veya Azure PowerShell yürütebilirler.

> [!NOTE]
> Oluşturulacak çok önemli bir nokta (10 +) özel görüntü olduğunda, bazı PowerShell dosyalarının çalıştırılması uzun zaman aldığına dikkat edin. Ücretsiz barındırılan DevOps derleme/yayın aracılarında 30 dakikalık bir zaman aşımı vardır. bu nedenle, çok sayıda görüntü oluşturmaya başladığınızda ücretsiz barındırılan aracıyı kullanamazsınız. Bu zaman aşımı testi, kullanmaya karar vereceğiniz her türlü bir uygulama için geçerlidir, uzun süre çalışan Azure PowerShell betikleri için tipik zaman aşımlarını genişletebileceğinizi doğrulamak iyi bir yoldur. Azure DevOps söz konusu olduğunda, ücretli barındırılan aracıları kullanabilir veya kendi Yapı aracınızı kullanabilirsiniz.

1. Başlamak için DevOps projenizin giriş sayfasında **derlemeyi ayarla** ' yı seçin:

    ![Kurulum oluştur düğmesi](./media/set-up-devops-lab/setup-build-button.png)
2. Yapı için bir **ad** belirtin (örneğin: yapı ve test laboratuvarlarına görüntü sunma).
3. **Boş** bir derleme tanımı seçin ve yapınızı oluşturmak için **Uygula** ' yı seçin.
4. Bu aşamada, derleme aracısı için **barındırılan** seçeneğini belirleyebilirsiniz.
5. Derleme tanımını **kaydedin** .

    ![Derleme tanımı](./media/set-up-devops-lab/build-definition.png)

## <a name="configure-the-build-variables"></a>Yapı değişkenlerini yapılandırma
Komut satırı parametrelerini basitleştirmek için, görüntü fabrikasını oluşturan anahtar değerlerini bir dizi derleme değişkenine kapsülleyebilirsiniz. **Değişkenler** sekmesini seçin ve birkaç varsayılan değişkenin listesini görürsünüz. Azure DevOps 'a girilecek değişkenlerin listesi aşağıdadır:


| Değişken Adı | Değer | Notlar |
| ------------- | ----- | ----- |
| ConfigurationLocation | /Scripts/ımagefactory/Configuration | Bu, depodaki **yapılandırma** klasörünün tam yoludur. Yukarıdaki tüm depoyu içeri aktardıysanız, soldaki değer doğrudur. Aksi takdirde yapılandırma konumunu işaret etmek için güncelleştirin. |
| DevTestLabName | Myımagefactory | Görüntü oluşturmak için fabrika olarak kullanılan Azure DevTest Labs laboratuvarın adı. Yoksa, bir tane oluşturun. Laboratuvarın, hizmet uç noktasının erişimi olan aynı abonelikte olduğundan emin olun. |
| Imageretention | 1 | Her türden kaydetmek istediğiniz görüntü sayısı. Varsayılan değeri 1 olarak ayarlayın. |
| MachinePassword | ******* | Sanal makineler için yerleşik yönetici hesabı parolası. Bu geçici bir hesaptır, bu yüzden güvenli olduğundan emin olun. Güvenli bir dize olduğundan emin olmak için sağdaki küçük kilit simgesini seçin. |
| MachineUserName | Imagefactoryuser | Sanal makineler için yerleşik yönetici hesabı Kullanıcı adı. Bu geçici bir hesaptır. |
| StandardTimeoutMinutes | 30 | Normal Azure işlemlerini beklememiz gereken zaman aşımı. |
| kaynak grubundaki |  0000000000-0000-0000-0000-0000000000000 | Laboratuvarın bulunduğu ve hizmet uç noktasının erişimi olan aboneliğin KIMLIĞI. |
| VMSize | Standard_A3 | **Oluşturma** adımı için kullanılacak sanal makinenin boyutu. Oluşturulan VM 'Ler geçicidir. Boyut, [Laboratuvar için etkinleştirilmiş](devtest-lab-set-lab-policy.md)olan bir olmalıdır. Yeterli sayıda [abonelik çekirdeği kotası](../azure-resource-manager/management/azure-subscription-service-limits.md)olduğunu doğrulayın.

![Derleme değişkenleri](./media/set-up-devops-lab/configure-build-variables.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Sonraki adım, hizmet sorumlusunu ayarlamaya yönelik olur. Bu Azure Active Directory, DevOps derleme aracısının Kullanıcı adına Azure 'da çalışmasını sağlayan bir kimliktir. Bunu ayarlamak için, ilk Azure PowerShell oluşturma adımını ekleyerek başlayın.

1. **Görev Ekle**' yi seçin.
2. **Azure PowerShell**arayın.
3. Bunu bulduktan sonra, görevi yapıya eklemek için **Ekle** ' yi seçin. Bunu yaptığınızda, görevin sol tarafta eklenmiş olarak göründüğünü görürsünüz.

![PowerShell adımını ayarlama](./media/set-up-devops-lab/set-up-powershell-step.png)

Hizmet sorumlusu oluşturmanın en hızlı yolu, Azure DevOps 'ın bunu bizim için gerçekleştirmesini sağlamasıdır.

1. Yeni eklediğiniz **görevi** seçin.
2. **Azure bağlantı türü**için **Azure Resource Manager**seçin.
3. Hizmet sorumlusunu ayarlamak için **Yönet** bağlantısını seçin.

Daha fazla bilgi için [bu blog gönderisine](https://devblogs.microsoft.com/devops/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) bakın. **Yönet** bağlantısını seçtiğinizde, Azure bağlantısı kurmak Için DevOps (blog gönderisine ikinci ekran görüntüsü) içinde doğru yere gideceksiniz. Bunu ayarlarken **Azure Resource Manager hizmet uç noktası** ' nı seçtiğinizden emin olun.

## <a name="complete-the-build-task"></a>Derleme görevini doldurun
Yapı görevini seçerseniz, sağ bölmedeki, doldurulması gereken tüm ayrıntıları görürsünüz.

1. İlk olarak, derleme görevini adlandırın: **sanal makine oluşturma**.
2. **Azure Resource Manager** seçerek oluşturduğunuz **hizmet sorumlusunu** seçin
3. **Hizmet uç noktasını**seçin.
4. **Betik yolu**için... seçeneğini belirleyin **. (üç nokta)** sağ tarafta.
5. **MakeGoldenImageVMs.ps1** betiğe gidin.
6. Betik parametreleri şöyle görünmelidir: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -DevTestLabName $(DevTestLabName) -vmSize $(VMSize) -machineUserName $(MachineUserName) -machinePassword (ConvertTo-SecureString -string '$(MachinePassword)' -AsPlainText -Force) -StandardTimeoutMinutes $(StandardTimeoutMinutes)`

    ![Derleme tanımını doldurun](./media/set-up-devops-lab/complete-build-definition.png)


## <a name="queue-the-build"></a>Derlemeyi kuyruğa al
Yeni bir derlemeyi kuyruğa alarak her şeyin doğru şekilde ayarlandığını doğrulayalım. Derleme çalışırken, her şeyin doğru şekilde çalıştığını onaylamak için [Azure Portal](https://portal.azure.com) geçin ve görüntü fabrikası laboratuvarınızda **tüm sanal makineler** ' i seçin. Laboratuvarda üç sanal makinenin oluşturulduğunu görmeniz gerekir.

![Laboratuvardaki VM 'Ler](./media/set-up-devops-lab/vms-in-lab.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure DevTest Labs temel alarak görüntü fabrikası ayarlamanın ilk adımı tamamlanmıştır. Serinin bir sonraki makalesinde, bu VM 'Leri genelleştirirsiniz ve özel görüntülere kaydedilir. Daha sonra, diğer laboratuvarlarınıza dağıtılır. Serinin sonraki makalesine bakın: [özel görüntüleri kaydetme ve birden çok laboratuvara dağıtma](image-factory-save-distribute-custom-images.md).
