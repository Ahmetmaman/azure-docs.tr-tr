---
title: Saydam bir ağ geçidi cihazı - Azure IOT Edge oluştur | Microsoft Docs
description: Azure IOT Edge cihazı, aşağı akış cihazlardan bilgi işleyebilen saydam bir ağ geçidi olarak kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 29c7fc279aec79750df48c70be7792869e89ae78
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094364"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma

Bu makalede, diğer cihazlar IOT Hub ile iletişim kurmak için saydam bir ağ geçidi olarak çalışması için IOT Edge cihazları yapılandırmaya ayrıntılı yönergeleri sağlar. Bu makalede, terim *IOT Edge ağ geçidi* saydam bir ağ geçidi olarak kullanılan bir IOT Edge cihazı gösterir. Daha ayrıntılı bilgi için bkz. [nasıl bir IOT Edge cihazı ağ geçidi olarak kullanılabilir](./iot-edge-as-gateway.md), kavramsal bir genel bakış sağlar.

>[!NOTE]
>Şu anda:
> * Ağ geçidi, IOT Hub'ından kesilirse, aşağı akış cihazların ağ geçidi ile kimlik doğrulaması yapamaz.
> * Edge özellikli cihazlar IOT Edge ağ geçidi için bağlantı kurulamıyor. 
> * Aşağı Akış cihazları karşıya dosya yükleme kullanamazsınız.

Bir cihaz bir ağ geçidi olarak çalışması, aşağı akış cihazlar için güvenli bir şekilde bağlanabilir olması gerekir. Azure IOT Edge cihazları arasında güvenli bağlantılar kurmak için bir ortak anahtar altyapısı (PKI) kullanmanıza olanak tanır. Bu durumda, biz saydam bir ağ geçidi olarak görev yapan bir IOT Edge cihazına bağlamak için bir aşağı akış cihazı vermiş olursunuz. Makul güvenliğini sağlamak için aşağı akış cihaz, ağ geçitleri ve değil kötü amaçlı olabilecek bir ağ geçidi'ne bağlama cihazlarınızı yalnızca istediğinden sınır cihazı kimliğini onaylamalıdır.

Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Tüm pratik amacıyla bir aşağı akış cihazın IOT Edge ağ geçidi cihazın kendisinde çalışan bir uygulama bile olabilir. 

Ağ geçidi cihazı topolojiniz için gerekli güven sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, etkinleştirmek için kullanacağınız aynı sertifika Kurulumu varsayıyoruz [X.509 CA güvenlik](../iot-hub/iot-hub-x509ca-overview.md) belirli bir IOT hub (IOT hub'ı sahibi CA) ve sertifikaları bir dizi için ilişkili bir X.509 CA sertifikası içerir, IOT Hub'ındaki Sınır cihazı için bu CA ve bir CA ile imzalanmış.

![Ağ geçidi sertifikası Kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

Ağ geçidi bağlantı başlatma sırasında aşağı akış cihaza Edge cihaz CA sertifikasını sunar. Edge cihaz CA sertifika sahibi CA sertifikası tarafından imzalanmış emin olmak için aşağı akış cihaz denetler. Bu işlem, ağ geçidi güvenilir bir kaynaktan gelen onaylamak aşağı akış cihaz sağlar.

Aşağıdaki adımlarda sertifikaları oluşturma ve bunları doğru yerlerde yükleme işleminde size yol.

## <a name="prerequisites"></a>Önkoşullar

Bir ağ geçidi olarak yapılandırmak için Azure IOT Edge cihazı. Geliştirme makinenizde veya bir sanal makine adımları aşağıdaki işletim sistemleri için bir IOT Edge cihazı olarak kullanabilirsiniz:
* [Windows](./how-to-install-iot-edge-windows-with-windows.md)
* [Linux x64](./how-to-install-iot-edge-linux.md)
* [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

Herhangi bir makine sertifikalarını oluşturmak için kullanın ve ardından bunları IOT Edge cihazınıza kopyalayabilirsiniz. 

## <a name="generate-certificates-with-windows"></a>Windows ile sertifikalar oluşturma

Adımları Bu bölümde bir Windows cihazında test sertifikalarınızı oluşturmak için kullanın. IOT Edge Cihazınızda kendisini sertifikalarını oluşturmak veya ayrı bir makine kullanmanız ve son sertifikaları tüm desteklenen işletim sistemi çalıştıran tüm IOT Edge cihazına kopyalayın. 

Bu bölümde oluşturulan sertifikaları yalnızca test amaçlarına yöneliktir. 

### <a name="install-openssl"></a>OpenSSL yükleyin

OpenSSL için Windows sertifikalarını oluşturmak için kullanmakta olduğunuz makineye yükleyin. OpenSSL yükleyebileceğiniz birkaç yolu vardır:

   >[!NOTE]
   >Windows Cihazınızda yüklü OpenSSL zaten varsa bu adımı atlayın ancak bu openssl.exe uygulamanız yol ortam değişkeninde kullanılabilir olduğundan emin olun.

* **Daha kolay:** indirin ve yükleyin [üçüncü taraf OpenSSL ikili](https://wiki.openssl.org/index.php/Binaries), örneğin, gelen [SourceForge bu projede](https://sourceforge.net/projects/openssl/). Tam yolunu openssl.exe, PATH ortam değişkenine ekleyin. 
   
* **Önerilen:** OpenSSL kaynak kodunu indirebilir ve ikili dosyaları makinenizde sizin tarafınızdan veya aracılığıyla derleme [vcpkg](https://github.com/Microsoft/vcpkg). Aşağıda listelenen yönergeleri vcpkg kaynak kodu, derleme ve yükleme OpenSSL kolay adımda Windows makinenize indirmek için kullanın.

   1. Vcpkg yüklemek istediğiniz bir dizine gidin. Bu dizine başvuracağınız  *\<VCPKGDIR >*. İndirme ve yükleme yönergelerini [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Windows için x64 OpenSSL paketi yüklemek için aşağıdaki komutu vcpkg yüklendiğinde bir powershell isteminden çalıştırın. Yükleme, genellikle tamamlanması yaklaşık 5 dakika sürer.

      ```PowerShell
      .\vcpkg install openssl:x64-windows
      ```
   3. Ekleme `<VCPKGDIR>\installed\x64-windows\tools\openssl` , PATH ortam değişkenine böylece openssl.exe dosya çağırma için kullanılabilir.

### <a name="prepare-creation-scripts"></a>Oluşturma betikleri hazırlama

C için Azure IOT cihaz SDK'sını test sertifikalarınızı oluşturmak için kullanabileceğiniz betikleri içerir. Bu bölümde, SDK'sı kopyalama ve PowerShell yapılandırın.

1. Yönetici modunda bir PowerShell penceresi açın. 

2. Üretim dışı sertifikalarını oluşturmak için komut dosyalarını içeren bir git deposunu kopyalayın. Bu betikler, saydam bir ağ geçidini ayarlamak için gerekli sertifikaları oluşturmanıza yardımcı olur. Kullanım `git clone` komutu veya [ZIP indir](https://github.com/Azure/azure-iot-sdk-c/archive/master.zip). 

   ```PowerShell
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Çalışmak istediğiniz dizine gidin. Bu dizine başvuracağınız  *\<WRKDIR >*.  Bu dizindeki tüm dosyaları oluşturulur.

4. Yapılandırma ve komut dosyaları, çalışma dizinine kopyalayın. 

   ```PowerShell
   copy <path>\azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy <path>\azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   ```

   SDK ZIP olarak indirdiğiniz sonra klasör adı `azure-iot-sdk-c-master` ve yolun geri kalanı aynıdır. 

5. Ortam değişkeni OPENSSL_CONF openssl_root_ca.cnf yapılandırma dosyası kullanmak için ayarlayın.

    ```PowerShell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```

6. PowerShell komut dosyalarını çalıştırmak etkinleştirin.

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. PowerShell'in genel ad alanına betikler tarafından kullanılan işlevler, getirin.
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

8. OpenSSL doğru şekilde yüklendiğini doğrulayın ve var olan sertifikalar ile ad çakışmalarını olmayacak emin olun. İlgili sorun varsa betiği nasıl düzeltileceğini sisteminizde açıklamalıdır.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Sertifika oluşturma

Bu bölümde, üç sertifikaları oluşturma ve bunları bir zincirinde bağlayın. Sertifika zinciri dosyasında yerleştirme, bunları, IOT Edge ağ geçidi cihazı ve herhangi bir aşağı akış cihazlarda kolayca yüklemenize olanak tanır.  

1. Sahip CA sertifikası oluşturabilir ve bunu bir ara sertifika sağlayabilirsiniz. Sertifikalar tüm yerleştirilir  *\<WRKDIR >*.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

2. Aşağıdaki komutla, Edge cihaz CA sertifikasını ve özel anahtar oluşturun. Sertifika oluşturma sırasında ve dosya adı için kullanılan ağ geçidi cihazı için bir ad sağlayın. 

   ```PowerShell
   New-CACertsEdgeDevice "<gateway name>"
   ```

3. Sahibi CA sertifikası ara sertifika ve aşağıdaki komutla Edge cihaz CA sertifikasını bir sertifika zinciri oluşturun. 

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway name>"
   ```

   Betik aşağıdaki sertifikaları ve anahtarı oluşturur:
   * `<WRKDIR>\certs\new-edge-device.*`
   * `<WRKDIR>\private\new-edge-device.key.pem`
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="generate-certificates-with-linux"></a>Linux ile sertifikalar oluşturma

Adımları Bu bölümde, bir Linux cihazda test sertifikalarınızı oluşturmak için kullanın. IOT Edge Cihazınızda kendisini sertifikalarını oluşturmak veya ayrı bir makine kullanmanız ve son sertifikaları tüm desteklenen işletim sistemi çalıştıran tüm IOT Edge cihazına kopyalayın. 

### <a name="prepare-creation-scripts"></a>Oluşturma betikleri hazırlama

1. Üretim dışı sertifikalarını oluşturmak için komut dosyalarını içeren bir git deposunu kopyalayın. Bu betikler, saydam bir ağ geçidini ayarlamak için gerekli sertifikaları oluşturmanıza yardımcı olur. 

   ```bash
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

2. Çalışmak istediğiniz dizine gidin. Bu dizine başvuracağınız  *\<WRKDIR >*.  Bu dizindeki tüm dosyaları oluşturulur.
  
3. Yapılandırma ve komut dosyaları, çalışma dizinine kopyalayın.

   ```bash
   cp <path>/azure-iot-sdk-c/tools/CACertificates/*.cnf .
   cp <path>/azure-iot-sdk-c/tools/CACertificates/certGen.sh .
   ```

4. Sağlanan komut dosyasını kullanarak sertifikalarını oluşturmak için OpenSSL yapılandırın. 

   ```bash
   chmod 700 certGen.sh 
   ```

### <a name="create-certificates"></a>Sertifika oluşturma

Bu bölümde, üç sertifikaları oluşturma ve bunları bir zincirinde bağlayın. IOT Edge ağ geçidi cihazınıza ve herhangi bir aşağı akış cihazlarda kolayca yüklemek için sertifika zinciri dosyasında yerleştirme sağlar.  

1.  Sahibi CA sertifikası ve bir ara sertifika oluşturun. Bu sertifikalar yerleştirilir  *\<WRKDIR >*.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Betik aşağıdaki sertifikaları ve anahtarları oluşturur:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>/certs/azure-iot-test-only.intermediate.cert.pem`
   * `<WRKDIR>/private/azure-iot-test-only.root.ca.key.pem`
   * `<WRKDIR>/private/azure-iot-test-only.intermediate.key.pem`

2.  Aşağıdaki komutla, Edge cihaz CA sertifikasını ve özel anahtar oluşturun. Sertifika oluşturma sırasında ve dosya adı için kullanılan ağ geçidi cihazı için bir ad sağlayın. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway name>"
   ```

   Betik aşağıdaki sertifikaları ve anahtarı oluşturur:
   * `<WRKDIR>/certs/new-edge-device.*`
   * `<WRKDIR>/private/new-edge-device.key.pem`

3. Adlı bir sertifika zinciri oluşturmanıza **yeni-edge-cihaz-tam-chain.cert.pem** sahip CA sertifikası, ara sertifika ve Edge cihaz CA sertifikası.

   ```bash
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="install-certificates-on-the-gateway"></a>Sertifika ağ geçidi yükleme

Bir sertifika zinciri yaptığınız, IOT Edge ağ geçidi cihazında yükleyin ve yeni sertifikalar başvurmak için IOT Edge çalışma zamanı yapılandırma gerekir. 

1. Aşağıdaki dosyaları kopyalayın  *\<WRKDIR >*. Bunları herhangi bir IOT Edge cihazı kaydedin. IOT Edge cihazınız olarak hedef dizine başvuracağınız  *\<CERTDIR >*. 

   Sınır cihazı sertifikaları oluşturursa, bu adımı atlayın ve çalışma dizininin yolunu kullanın.

   * Cihaz CA sertifikası-  `<WRKDIR>\certs\new-edge-device-full-chain.cert.pem`
   * Cihaz CA özel anahtarı- `<WRKDIR>\private\new-edge-device.key.pem`
   * Sahibi CA- `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. IOT Edge güvenlik daemon yapılandırma dosyasını açın. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Ayarlama **sertifika** özellikleri IOT Edge cihazında sertifika ve anahtar dosyaları yerleştirdiğiniz config.yaml dosyanın yolu.

```yaml
certificates:
  device_ca_cert: "<CERTDIR>\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "<CERTDIR>\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Ağ geçidine EdgeHub dağıtma

IOT Edge üzerinde bir cihaz ilk kez yüklediğinizde, yalnızca bir sistem modülü otomatik olarak başlar: Edge Aracısı. Cihazınızı bir ağ geçidi olarak çalışmak her iki sistem modüllerini gerekir. Ağ geçidi cihazınıza önce modüllerin dağıtmadıysanız, ikinci bir sistem modülü, Edge hub'ı başlatmak cihazınız için bir dağıtım oluşturun. Dağıtım Sihirbazı'nda modüllerin ekleme yapmazsanız, ancak her iki sistem modüllerini dağıtacağınız boş görünür. 

Komutu ile bir cihaz üzerinde çalışan modüllerine denetleyebilirsiniz `iotedge list`.

1. Azure portalında IoT Hub'ınıza gidin.

2. Git **IOT Edge** ve ağ geçidi olarak kullanacağınız IOT Edge Cihazınızı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **İleri**’yi seçin.

5. İçinde **yolları belirtin** sayfasında, tüm iletileri tüm modüllerden IOT Hub'ına gönderir ve varsayılan bir yol olmalıdır. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. İçinde **gözden geçirme şablonu** sayfasında **Gönder**.

## <a name="route-messages-from-downstream-devices"></a>Aşağı Akış cihazlardan yönlendirme iletileri
IOT Edge çalışma zamanı yalnızca modülleri tarafından gönderilen iletiler gibi aşağı akış cihazlardan gönderilen iletiler yönlendirebilirsiniz. Bu verileri buluta göndermeden önce ağ geçidi üzerinde çalışan bir modüldeki analiz gerçekleştirmenize olanak sağlar. 

Şu anda, aşağı akış cihazlar tarafından gönderilen iletileri yönlendiren bunları modülleri tarafından gönderilen iletilerden farklılaştırılması tarafından yoludur. Tüm modüller tarafından gönderilen iletiler olarak adlandırılan bir sistem özelliği içeren **connectionModuleId** ancak aşağı akış cihazlar tarafından gönderilen iletileri yapın. Bu sistem özelliği içeren herhangi bir iletiyi hariç tutmak için rota WHERE yan tümcesini kullanabilirsiniz. 

Yolu bir modül adı için aşağı akış herhangi bir CİHAZDAN ileti göndermek için kullanılacak `ai_insights`.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

İleti yönlendirme hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar'kurmak](./module-composition.md#declare-routes).

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi olarak çalışan bir IOT Edge cihazına sahip olduğunuza göre ağ geçidi güven ve iletileri göndermek için aşağı akış cihazlarınızı yapılandırmak gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).
