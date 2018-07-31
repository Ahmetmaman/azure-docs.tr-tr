---
title: Azure IoT Hub Cihazı Sağlama Hizmeti için cihazı ayarlama
description: Cihaz üretim süreci sırasında IoT Hub Cihazı Sağlama Hizmeti aracılığıyla sağlanacak cihazı ayarlama
author: dsk-2015
ms.author: dkshir
ms.date: 04/02/2018
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: d589c0ece2b36970a31884aa72ee7ab87941a656
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146456"
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub Cihazı Sağlama Hizmeti’ni kullanarak sağlanacak bir cihaz ayarlama

Önceki öğreticide, IoT hub’ınıza cihazlarınızı otomatik olarak sağlamak için Azure IoT Hub Cihazı Sağlama Hizmeti’nin nasıl ayarlanacağını öğrendiniz. Bu öğretici, cihazınızın IOT Hub ile otomatik olarak sağlanmasına imkan vermek için üretim işlemi boyunca cihazınızın ayarlanacağını gösterir. Cihazınız, [Kanıtlama mekanizmasına](concepts-device.md#attestation-mechanism) dayalı olarak, ilk önyüklemenin ve sağlama hizmetiyle bağlantının ardından sağlanır. Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Platforma özgü Cihaz Sağlama Hizmetleri İstemci SDK’sını derleme
> * Güvenlik yapılarını ayıklama
> * Cihaz kayıt yazılımı oluşturma

Bu öğreticide önceki [Bulut kaynaklarını ayarlama](tutorial-set-up-cloud.md) öğreticisinde yer alan yönergeleri kullanarak Cihaz Sağlama Hizmeti örneğinizi ve bir IoT hub’ı oluşturduğunuz kabul edilmektedir.

Bu öğretici, C için Cihaz Sağlama Hizmeti İstemci SDK’sını içeren [C deposu için Azure IoT SDK’ları ve kitaplıkları](https://github.com/Azure/azure-iot-sdk-c)’nı kullanır. SDK şu anda Windows veya Ubuntu uygulamalarında çalıştırılan cihazlar için TPM ve X.509 desteği sağlar. Bu öğretici, Visual Studio 2017 konusunda temel yetkinliğe sahip olunduğu varsayılarak bir Windows geliştirme istemcisi kullanımına dayanır. 

Otomatik sağlama işlemini bilmiyorsanız, devam etmeden önce [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun. 


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* ["C++ ile masaüstü geliştirme"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükünün etkinleştirildiği Visual Studio 2015 veya [Visual Studio 2017](https://www.visualstudio.com/vs/).
* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.



## <a name="build-a-platform-specific-version-of-the-sdk"></a>SDK’nın platforma özgü sürümünü derleme

Cihaz Sağlama Hizmeti İstemci SDK’sı, cihaz kaydı yazılımınızı uygulamanıza yardımcı olur. Ancak bunu kullanabilmeniz için önce geliştirme istemci platformunuza ve kanıtlama mekanizmanıza özgü bir SDK sürümü derlemeniz gerekir. Bu öğreticide, desteklenen bir kanıtlama türü için, Windows geliştirme platformunda Visual Studio 2017’yi kullanan bir SDK derlersiniz:

1. [CMake derleme sisteminin](https://cmake.org/download/) en son sürümünü indirin. Aynı sitede, seçtiğiniz ikili dağıtım sürümünün şifreleme karmasını arayın. İlgili şifreleme karması değerini kullanarak indirilen ikili dağıtımı doğrulayın. Aşağıdaki örnekte, x64 MSI dağıtımı 3.11.4 sürümünün şifreleme karmasını doğrulamak için Windows PowerShell kullanılır:

    ```PowerShell
    PS C:\Users\wesmc\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Users\wesmc\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Bu deponun boyutu şu anda 220 MB kadardır. Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.


3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. SDK'yı kullanacağınız kanıtlama mekanizmasıyla tercih edeceğiniz geliştirme platformuna göre derleyin. Aşağıdaki komutlardan birini kullanın (her komutun sonundaki yan yana iki nokta işaretine dikkat edin). Tamamlandığında CMake, cihazınıza özgü içerikle `/cmake` alt dizinini derler:
 
    - Kanıtlama için TPM simülatörünü kullanan cihazlar için:

        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

    - Başka bir cihaz için (fiziksel TPM/HSM/X.509 veya sanal X.509 sertifikası):

        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```


Şimdi cihaz kayıt kodunuzu derlemek için SDK’yı kullanmaya hazırsınız. 
 
<a id="extractsecurity"></a> 

## <a name="extract-the-security-artifacts"></a>Güvenlik yapılarını ayıklama 

Bir sonraki adım, cihazınız tarafından kullanılan kanıtlama mekanizması için güvenlik yapılarını ayıklamaktır. 

### <a name="physical-devices"></a>Fiziksel cihazlar 

SDK'yı kanıtlama için fiziksel TPM/HSM veya X.509 sertifikası kullanacak şekilde derlemenize bağlı olarak güvenlik yapıtlarını aşağıdaki şekilde alabilirsiniz:

- TPM cihazı için, TPM yongası üreticisinden bu TPM cihazıyla ilişkili **Onay Anahtarı**’nı belirlemeniz gerekir. Onay anahtarını karma hale getirerek TPM cihazınız için benzersiz bir **Kayıt Kimliği** türetebilirsiniz.  

- Bir X.509 cihazı için cihazlarınıza verilmiş sertifikaları almanız gerekir. Sağlama hizmeti, X.509 kanıtlama mekanizmasını kullanan cihazlara erişimi kontrol eden iki kayıt girişi türü sunar. Gerekli sertifikalar, kullanacağınız kayıt türüne göre değişiklik gösterir.

    1. Bireysel kayıtlar: Tek bir cihaz kaydı. Bu kayıt girişi türü için [end-entity, "leaf", certificates](concepts-security.md#end-entity-leaf-certificate) kullanılması gerekir.
    2. Kayıt grupları: Bu kayıt girişi türü için ara veya kök sertifika kullanılması gerekir. Daha fazla bilgi için bkz. [X.509 sertifikalarıyla sağlama hizmetine cihaz erişimini denetleme](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

### <a name="simulated-devices"></a>Sanal cihazlar

SDK'yı kanıtlama için fiziksel TPM veya X.509 sertifikası kullanacak şekilde derlemenize bağlı olarak güvenlik yapıtlarını aşağıdaki şekilde alabilirsiniz:

- Sanal TPM cihazı için:

   1. Windows Komut İstemi açın, `azure-iot-sdk-c` dizinine gidin ve TPM simülatörünü çalıştırın. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu komut penceresini kapatmayın; aşağıdaki Hızlı Başlangıcın sonuna kadar bu simülatörü çalışır durumda tutmanız gerekir. 

      `azure-iot-sdk-c` alt dizininden aşağıdaki komutu çalıştırarak simülatörü başlatın:

      ```cmd/sh
      .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
      ```

      > [!NOTE]
      > Bu adım için Git Bash komut istemini kullanıyorsanız ters eğik çizgileri eğik çizgi olarak değiştirmeniz gerekir: `./provisioning_client/deps/utpm/tools/tpm_simulator/Simulator.exe`.

   2. Visual Studio kullanarak, `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve "Derleme" menüsündeki "Çözümü derle" komutunu kullanarak bunu derleyin.

   3. Visual Studio'nın *Çözüm Gezgini* bölmesinde **Sağlama\_Araçlar** klasörüne gidin. **tpm_device_provision** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 

   4. "Hata Ayıklama" menüsündeki “Başlat” komutlarından birini kullanarak çözümü çalıştırın. Çıktı penceresi, cihaz kaydı için gereken TPM simülatörün **_Kayıt Kimliği_**’ni ve **_Kayıt Anahtarı_**’nı görüntüler. Daha sonra kullanmak için bu değerleri kopyalayın. Bu pencereyi (Kayıt Kimliği ve Onay Anahtarı ile) kapatabilirsiniz, ancak 1. adımda başlattığınız TPM simülatörü penceresini çalışır durumda bırakın.

- Sanal X.509 cihazı için:

  1. Visual Studio kullanarak, `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve "Derleme" menüsündeki "Çözümü derle" komutunu kullanarak bunu derleyin.

  2. Visual Studio'nın *Çözüm Gezgini* bölmesinde **Sağlama\_Araçlar** klasörüne gidin. **dice\_device\_enrollment** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 
  
  3. "Hata Ayıklama" menüsündeki “Başlat” komutlarından birini kullanarak çözümü çalıştırın. Çıktı penceresinde, istendiğinde tek kayıt için **i** girin. Çıktı penceresi, sanal cihazınız için yerel olarak oluşturulmuş X.509 sertifikasını görüntüler. *-----BEGIN CERTIFICATE-----* ile başlayıp ilk *-----END PUBLIC KEY-----* ile biten çıktıyı panoya kopyalayın ve bu iki satırı da dahil ettiğinizden emin olun. Yalnızca çıktı penceresindeki ilk sertifikayı kopyalamanız gerekir.
 
  4. **_X509testcert.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz metin düzenleyicide açın ve panonun içeriklerini bu dosyaya kopyalayın. Cihaz kaydı için daha sonra kullanacağınızdan, dosyayı kaydedin. Kayıt yazılımınız çalıştırıldığında, otomatik sağlamadaki aynı sertifikayı kullanır.    

Cihaz Sağlama Hizmeti’nde cihazınızın kaydı sırasında bu güvenlik yapıları gerekir. Sağlama hizmeti, cihazın önyüklenmesini ve daha sonra buna bağlanmasını bekler. Cihazınız ilk kez önyüklendiğinde, istemci SDK’sı mantığı, cihazdaki güvenlik yapılarını ayıklamak için yonganız (veya simülatör) ile etkileşime geçer ve Cihaz Sağlama hizmetinize kaydı doğrular. 

## <a name="create-the-device-registration-software"></a>Cihaz kayıt yazılımı oluşturma

Son adım, cihazı IoT Hub hizmetine kaydetmek için Cihaz Sağlama Hizmeti istemci SDK’sını kullanan bir kayıt uygulaması yazmaktır. 

> [!NOTE]
> Bu adım için, iş istasyonunuzdan SDK örnek kayıt uygulaması çalıştırılarak gerçekleştirilen bir sanal cihaz kullanıldığı varsayılacaktır. Ancak fiziksel cihaza dağıtım için bir kayıt uygulaması derliyorsanız aynı kavramlar geçerlidir. 

1. Azure portalında, Cihaz Sağlama hizmetiniz için **Genel Bakış** dikey penceresini seçin ve **_Kimlik Kapsamı_** değerini kopyalayın. *Kimlik Kapsamı* genellikle hizmet tarafından oluşturulur ve benzersizliği garanti eder. Bu sabittir ve kayıt kimliklerini benzersiz şekilde tanımlamak için kullanılır.

    ![Portal dikey penceresinden DPS uç nokta bilgilerini ayıklama](./media/tutorial-set-up-device/extract-dps-endpoints.png) 

2. Makinenizdeki Visual Studio *Çözüm Gezgini*'nde **Sağlama\_Örnekler** klasörüne gidin. **prov\_dev\_client\_sample** adlı örnek projeyi seçip **prov\_dev\_client\_sample.c** kaynak dosyasını açın.

3. 1. adımda elde edilen _Kimlik Kapsamı_ değerini `id_scope` değişkenine atayın (left/`[` ve right/`]` ayraçlarını kaldırarak): 

    ```c
    static const char* global_prov_uri = "global.azure-devices-provisioning.net";
    static const char* id_scope = "[ID Scope]";
    ```

    Başvuru için, `IoTHubClient_LL_CreateFromDeviceAuth` IoT Hub istemci kaydı API’sinin, belirlenen Cihaz Sağlama Hizmeti örneğine bağlanmasına olanak sağlayan `global_prov_uri` değişkeni.

4. Aynı dosyadaki **main()** işlevinde, cihazınızın kayıt yazılımı (TPM veya X.509) tarafından kullanılmakta olan kanıtlama mekanizmasıyla eşleşen `hsm_type` değişkenine açıklama ekleyin veya bu değişkenin açıklamasını kaldırın: 

    ```c
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

5. Değişikliklerinizi kaydedin ve "Derleme" menüsünden "Çözümü derle" seçeneğini belirleyerek **prov\_dev\_client\_sample** örneğini yeniden derleyin. 

6. **Sağlama\_Örnekler** klasörünün altında **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. Henüz örnek uygulamayı ÇALIŞTIRMAYIN.

> [!IMPORTANT]
> Henüz cihazı çalıştırmayın/başlatmayın! Cihazı başlatmadan önce ilk olarak Cihaz Sağlama Hizmeti’ne cihazı kaydederek işlemi tamamlamanız gerekir. Aşağıda yer alan Sonraki adımlar, sonraki makale için size yol gösterecektir.

### <a name="sdk-apis-used-during-registration-for-reference-only"></a>Kayıt sırasında kullanılan SDK API’leri (yalnızca başvuru)

Başvuru için SDK, kayıt sırasında kullanmak üzere uygulamanız için aşağıdaki API’leri sağlar. Bu API’ler, cihazınızın önyüklenirken Cihaz Sağlama Hizmetine bağlanmasına ve kaydolmasına yardımcı olur. Buna karşılık cihazınız, IoT Hub örneğinizle bağlantı kurmak için gerekli bilgileri alır:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service.  
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Ayrıca, ilk olarak bir sanal cihaz kullanarak Cihaz Sağlama Hizmeti istemci kaydı uygulamanızı iyileştirmeniz ve hizmet kurulumunu test etmeniz gerektiğini de fark edebilirsiniz. Uygulamanız test ortamında çalıştığında, cihazınız için uygulamayı derleyebilir ve yürütülebilir dosyayı cihazınızın görüntüsüne kopyalayabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada, Cihaz Sağlama ve IoT Hub hizmetleri portalda çalışıyor olabilir. Cihaz sağlama kurulumundan çıkmak ve/veya bu öğretici serisinin tamamlanmasını geciktirmek istiyorsanız, gereksiz yinelenen maliyetler oluşmasını önlemek için bunların kapatılmasını öneririz.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Platforma özgü Cihaz Sağlama Hizmeti İstemci SDK’sını derleme
> * Güvenlik yapılarını ayıklama
> * Cihaz kayıt yazılımı oluşturma

Otomatik sağlama için cihazı Azure IoT Hub Cihazı Sağlama Hizmeti’ne kaydederek IoT hub’ına nasıl sağlanacağı hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [IoT hub’ınıza cihazı sağlama](tutorial-provision-device-to-hub.md)

