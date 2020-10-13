---
title: Örnek ayarlama ve kimlik doğrulaması (betik ile)
titleSuffix: Azure Digital Twins
description: Otomatikleştirilmiş bir dağıtım betiği çalıştırarak Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 83741f5bc55eb222b379a274ef403f766553b21f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91328657"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-scripted"></a>Azure dijital TWINS örneği ve kimlik doğrulaması (komut dosyası) ayarlama

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama**adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, işlemi kolaylaştıran [ **otomatikleştirilmiş bir dağıtım betiği** örneği](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/) çalıştırarak bu adımları tamamlar. 
* Betiğin arka planda çalıştığı el ile CLı adımlarını görüntülemek için, bu makalenin CLı sürümüne bakın: [*nasıl yapılır: bir örnek ve kimlik doğrulaması (CLI) ayarlama*](how-to-set-up-instance-cli.md).
* Azure portal göre el ile adımları görüntülemek için, bu makalenin Portal sürümüne bakın: [*nasıl yapılır: bir örnek ve kimlik doğrulaması (portal) ayarlama*](how-to-set-up-instance-portal.md).

[!INCLUDE [digital-twins-setup-steps-prereq.md](../../includes/digital-twins-setup-steps-prereq.md)]

## <a name="prerequisites-download-the-script"></a>Önkoşullar: betiği Indir

Örnek betik, PowerShell 'de yazılmıştır. Bu örnek bağlantıyı inceleyerek makinenize indirebileceğiniz [**Azure dijital TWINS örneklerinin**](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/)bir parçasıdır. Bu, söz konusu örnek bağlantısına giderek ve başlık altındaki *posta indirme* düğmesini seçmiş olursunuz.

Bu işlem, örnek projeyi _**Azure_Digital_Twins_samples.zip**_ olarak makinenize indirir. Makinenizde bulunan klasöre gidin ve dosyaları ayıklamak için dosyayı ayıklayın.

Sıkıştırılmış klasörde, dağıtım betiği _Azure_Digital_Twins_samples > betiklerinin > **deploy.ps1** _bulunur.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="run-the-deployment-script"></a>Dağıtım betiğini çalıştırma

Bu makalede bir Azure dijital TWINS örneği ve gerekli kimlik doğrulama yarı otomatik olarak dağıtmak için bir Azure dijital TWINS kod örneği kullanılmaktadır. Ayrıca, kendi komut dosyası etkileşimlerinizi yazmak için bir başlangıç noktası olarak da kullanılabilir.

Dağıtım betiğini Cloud Shell ' de çalıştırma adımları aşağıda verilmiştir.
1. Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com/) penceresine gidin. Şu komutu kullanarak oturum açın:
    ```azurecli
    az login
    ```
    CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler. Aksi takdirde, konumunda bir tarayıcı sayfası açın *https://aka.ms/devicelogin* ve terminalinizde görünen yetkilendirme kodunu girin.
 
2. Cloud Shell simge çubuğunda, Cloud Shell PowerShell sürümünü çalıştıracak şekilde ayarlandığından emin olun.

    :::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-powershell.png" alt-text="PowerShell sürümünün seçimini gösteren pencere Cloud Shell&quot;:::

1. &quot;Dosyaları karşıya yükle/Indir&quot; simgesini seçin ve &quot;karşıya yükle" yi seçin.

    :::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-upload.png" alt-text="PowerShell sürümünün seçimini gösteren pencere Cloud Shell&quot;:::

1. &quot;Dosyaları karşıya yükle/Indir&quot; simgesini seçin ve &quot;karşıya yükle" düğmesine basın. Bu işlem, dosyayı Cloud Shell penceresinde çalıştırabilmeniz için Cloud Shell dosyasına yükler.

4. Komutu Cloud Shell penceresine göndererek betiği çalıştırın `./deploy.ps1` . (Cloud Shell yapıştırılacağını geri çek, Windows ve Linux 'ta **CTRL + SHIFT + v** veya MacOS 'ta **cmd + SHIFT + v** kullanabilirsiniz. Sağ tıklama menüsünü de kullanabilirsiniz.)

    ```azurecli
    ./deploy.ps1
    ```

    Komut dosyası otomatik kurulum adımları üzerinden çalışırken, aşağıdaki değerleri geçirmeniz istenir:
    * Örnek için: kullanılacak Azure aboneliğinizin *ABONELIK kimliği*
    * Örnek için: örneği dağıtmak istediğiniz *konum* . Azure dijital TWINS 'i destekleyen bölgeleri görmek için [*bölgeye göre kullanılabilen Azure ürünlerini*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins)ziyaret edin.
    * Örnek için: bir *kaynak grubu* adı. Var olan bir kaynak grubunu kullanabilir veya oluşturmak için yeni bir ad girebilirsiniz.
    * Örnek için: Azure dijital TWINS örneğiniz için bir *ad* . Yeni örneğin adı, aboneliğinizin bölgesi içinde benzersiz olmalıdır (yani, aboneliğiniz seçtiğiniz adı kullanan bölgede başka bir Azure dijital TWINS örneğine sahipse, farklı bir ad seçmeniz istenir).
    * Uygulama kaydı için: kayıt ile ilişkilendirilecek bir *Azure AD uygulama görünen adı* . Bu uygulama kaydı, [Azure dijital TWINS API 'leri](how-to-use-apis-sdks.md)için erişim izinlerini yapılandırdığınız yerdir. Daha sonra, istemci uygulaması uygulama kaydında kimlik doğrulaması yapacak ve bu nedenle API 'lere yapılandırılmış erişim izinleri verilmeyecektir.
    * Uygulama kaydı için: Azure AD uygulaması için bir *Azure AD uygulama yanıt URL 'si* . `http://localhost` komutunu kullanın. Betik için *ortak bir istemci/yerel (mobil & Masaüstü)* URI 'si ayarlanır.

Betik bir Azure dijital TWINS örneği oluşturur, Azure kullanıcısını örneğe *Azure Digital TWINS Owner (Önizleme)* rolünü atar ve istemci uygulamanızın kullanması Için BIR Azure AD uygulama kaydı ayarlayın.

>[!NOTE]
>Şu anda, bazı kullanıcıların (özellikle kişisel [Microsoft hesaplarındaki kullanıcılar (MSAs)](https://account.microsoft.com/account)) ** _Azure dijital TWINS sahibine (Önizleme)_ rol atamasını**bulabildiği, betikleştirilmiş kurulumla ilgili **bilinen bir sorun** vardır.
>
>Rol atamasını, bu makalenin ilerleyen kısımlarında bulunan [*Kullanıcı rolü atamasını doğrula*](#verify-user-role-assignment) bölümü ile doğrulayabilirsiniz ve gerekirse, [Azure Portal](how-to-set-up-instance-portal.md#set-up-user-access-permissions) veya [CLI](how-to-set-up-instance-cli.md#set-up-user-access-permissions)kullanarak rol atamasını el ile ayarlayabilirsiniz.
>
>Bu sorun hakkında daha fazla bilgi için bkz. [*sorun giderme: Azure dijital TWINS 'de bilinen sorunlar*](troubleshoot-known-issues.md#missing-role-assignment-after-scripted-setup).

Betikten çıktı günlüğü alıntısı aşağıda verilmiştir:

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/deployment-script-output.png" alt-text="PowerShell sürümünün seçimini gösteren pencere Cloud Shell&quot;:::

1. &quot;Dosyaları karşıya yükle/Indir&quot; simgesini seçin ve &quot;karşıya yükle" lightbox="media/how-to-set-up-instance/cloud-shell/deployment-script-output.png":::

Betik başarılı bir şekilde tamamlanırsa, nihai çıktıyı görürsünüz `Deployment completed successfully` . Aksi takdirde, hata iletisini adreslayın ve betiği yeniden çalıştırın. Bu işlem, önceden tamamladığınız adımları atlar ve kaldığınız yerden yeniden giriş istemeye başlar.

Komut dosyası tamamlandıktan sonra artık bir Azure dijital TWINS örneğine, bu dosyayı yönetme izinleri ile çalışmaya hazır olursunuz ve bir istemci uygulamasının bu uygulamaya erişmesi için izinleri ayarlayın.

> [!NOTE]
> Betik şu anda Azure dijital TWINS (*Azure dijital TWINS sahibi (Önizleme)*) içindeki gerekli yönetim rolünü, Cloud Shell betiği çalıştıran kullanıcıya atar. Bu rolü örneği yöneten başka birine atamanız gerekiyorsa, bunu şimdi Azure portal ([yönergeler](how-to-set-up-instance-portal.md#set-up-user-access-permissions)) veya CLI ([yönergeler](how-to-set-up-instance-cli.md#set-up-user-access-permissions)) yoluyla yapabilirsiniz.

## <a name="collect-important-values"></a>Önemli değerleri topla

Azure dijital TWINS örneğiniz ile çalışmaya devam ederseniz, betikte, ihtiyacınız olabilecek bir dizi önemli değer vardır. Bu bölümde, bu değerleri toplamak için [Azure Portal](https://portal.azure.com) kullanacaksınız. Bunları güvenli bir yere kaydetmelisiniz veya daha sonra ihtiyacınız olduğunda bunları bulmak için bu bölüme geri dönebilirsiniz.

Diğer kullanıcılar örnekle programlama yapacaktır, bu değerleri bunlarla de paylaşabilirsiniz.

### <a name="collect-instance-values"></a>Örnek değerlerini topla

[Azure Portal](https://portal.azure.com), Portal arama çubuğunda örneğinizin adını arayarak Azure dijital TWINS örneğinizi bulun.

Bunu seçtiğinizde, örneğin *genel bakış* sayfası açılır. *Adına*, *kaynak grubuna*ve *ana bilgisayar adına göz önünde barının*. Daha sonra örneğinizi belirleyip daha sonra bu örneğe bağlanmanız gerekebilir.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="PowerShell sürümünün seçimini gösteren pencere Cloud Shell&quot;:::

1. &quot;Dosyaları karşıya yükle/Indir&quot; simgesini seçin ve &quot;karşıya yükle":::

### <a name="collect-app-registration-values"></a>Uygulama kayıt değerlerini topla 

Uygulama kaydından, daha sonra [Azure Digital TWINS API 'leri için istemci uygulama kimlik doğrulama kodunu yazmak](how-to-authenticate-client.md)üzere gerekli olan iki önemli değer vardır. 

Bunları bulmak için Azure portal Azure AD uygulama kaydına genel bakış sayfasına gitmek üzere [Bu bağlantıyı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) izleyin. Bu sayfa, aboneliğinizde oluşturulmuş tüm uygulama kayıtlarını gösterir.

Bu listede yeni oluşturduğunuz uygulama kaydını görmeniz gerekir. Ayrıntılarını açmak için seçin:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="PowerShell sürümünün seçimini gösteren pencere Cloud Shell&quot;:::

1. &quot;Dosyaları karşıya yükle/Indir&quot; simgesini seçin ve &quot;karşıya yükle":::

**Sayfanızda gösterilen** *uygulama (istemci) kimliğini* ve *Dizin (kiracı) kimliğini* bir yere göz atın. İstemci uygulamaları için kod yazmak üzere bir kişi değilseniz, bu değerleri olacak kişiyle paylaşmanız gerekir.

## <a name="verify-success"></a>Başarıyı doğrula

Komut dosyası tarafından ayarlanan kaynaklarınızın ve izinlerin oluşturulmasını doğrulamak istiyorsanız, [Azure Portal](https://portal.azure.com)bunlara bakabilirsiniz.

Herhangi bir adımın başarısını doğrulayadıysanız adımı yeniden deneyin. [Azure Portal](how-to-set-up-instance-portal.md) veya [CLI](how-to-set-up-instance-cli.md) yönergelerini kullanarak adımları tek tek gerçekleştirebilirsiniz.

### <a name="verify-instance"></a>Örneği doğrula

Örneğinizin oluşturulduğunu doğrulamak için Azure portal [Azure dijital TWINS sayfasına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) gidin. Bu sayfaya, Portal arama çubuğunda *Azure dijital TWINS* araması yaparak kendiniz ulaşabilirsiniz.

Bu sayfada tüm Azure dijital TWINS örnekleri listelenir. Listede yeni oluşturduğunuz örneğin adını arayın.

Doğrulama başarısız olduysa, [Portal](how-to-set-up-instance-portal.md#create-the-azure-digital-twins-instance) veya [CLI](how-to-set-up-instance-cli.md#create-the-azure-digital-twins-instance)kullanarak bir örnek oluşturmayı yeniden deneyebilirsiniz.

### <a name="verify-user-role-assignment"></a>Kullanıcı rolü atamasını doğrula

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

> [!NOTE]
> Betiğin Şu anda bu gerekli rolü Cloud Shell betiği çalıştıran aynı kullanıcıya atadığını hatırlayın. Bu rolü örneği yöneten başka birine atamanız gerekiyorsa, bunu şimdi Azure portal ([yönergeler](how-to-set-up-instance-portal.md#set-up-user-access-permissions)) veya CLI ([yönergeler](how-to-set-up-instance-cli.md#set-up-user-access-permissions)) yoluyla yapabilirsiniz.

Doğrulama başarısız olduysa, [Portal](how-to-set-up-instance-portal.md#set-up-user-access-permissions) veya [CLI](how-to-set-up-instance-cli.md#set-up-user-access-permissions)kullanarak kendi rol atamasını de yineleyebilirsiniz.

### <a name="verify-app-registration"></a>Uygulama kaydını doğrulama

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

Daha sonra, Azure Digital TWINS izinleri ayarlarının kayıt üzerinde doğru şekilde ayarlandığını doğrulayın. Bunu yapmak için, menü çubuğundan *bildirim* ' ı seçerek uygulama kaydının bildirim kodunu görüntüleyin. Kod penceresinin en altına kaydırın ve altındaki bu alanları bulun `requiredResourceAccess` . Değerler aşağıdaki ekran görüntüsünde olanlarla eşleşmelidir:

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

Bu doğrulama adımlarından biri veya her ikisi başarısız olursa, [portalı](how-to-set-up-instance-portal.md#set-up-access-permissions-for-client-applications) veya [CLI](how-to-set-up-instance-cli.md#set-up-access-permissions-for-client-applications) yönergelerini kullanarak uygulama kaydını oluşturmayı yeniden deneyin.

## <a name="other-possible-steps-for-your-organization"></a>Kuruluşunuz için olası diğer adımlar

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Digital TWINS CLı komutlarını kullanarak örneğiniz için tek REST API çağrılarını test edin: 
* [az DT Reference](https://docs.microsoft.com/cli/azure/ext/azure-iot/dt?view=azure-cli-latest&preserve-view=true)
* [*Nasıl yapılır: Azure dijital TWINS CLı 'sını kullanma*](how-to-use-cli.md)

Ya da bkz. istemci uygulamasının kimlik doğrulama kodunu yazarak istemci uygulamanızı örneğinizle bağlama:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)
