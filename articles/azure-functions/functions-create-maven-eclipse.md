---
title: Eclipse ile Java ile bir Azure işlev uygulaması oluşturma | Microsoft Docs
description: Java ile Eclipse için Azure işlevleri, sunucusuz uygulama oluşturma ve basit bir HTTP yayımlama ile ilgili nasıl yapılır Kılavuzu tetiklenir.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: Azure işlevleri, İşlevler, olay işleme, işlem, sunucusuz mimari, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 7d29cd3801bf997bf5c28ed0845996725aaf1840
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093660"
---
# <a name="create-your-first-function-with-java-and-eclipse-preview"></a>Java ile Eclipse (Önizleme) kullanarak ilk işlevinizi oluşturma

> [!NOTE] 
> Azure İşlevleri için Java şu anda önizleme aşamasındadır.

Bu makalede nasıl oluşturulacağını gösterir bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) işlev projesi Eclipse IDE ve Apache Maven ile test ve hata ayıklamak ve ardından Azure işlevleri'ne dağıtın. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve Eclipse ile bir işlev uygulaması geliştirmek için aşağıdakilerin yüklü olması gerekir:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
-  [Eclipse](https://www.eclipse.org/downloads/packages/), Java ve Maven ile destekler.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

Ayrıca yükleme için önemle tavsiye edilir [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2), çalışan ve Azure işlevlerinde hata ayıklama için yerel bir ortam sağlar. 

## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. Eclipse'te, seçin **dosya** menüsü, ardından **proje**. 
1. Açık **Java projesi** klasöründe **yeni proje** penceresi ve select **Maven projesi**, ardından **sonraki**.
1. Varsayılan değerleri kabul **yeni bir Maven projesi** seçin ve iletişim kutusunda **sonraki**.
1. Seçin **ekleme Archetype** ve girdileri eklemek [azure işlevleri archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - Archetype grubu kimliği: com.microsoft.azure
    - Yapıt kimliği archetype: azure işlevleri archetype
    - Sürüm: Kullanımı en son sürümü [merkezi depo](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Eclipse Maven oluşturma](media/functions-create-first-java-eclipse/functions-create-eclipse.png)  
1. Tıklayın **Tamam** ve ayrıntılarını girin geçerli projenin ve sonunda **son**.

Maven, _artifactId_ adlı yeni bir dosyada proje dosyalarını oluşturur. Oluşturulan kod projesinde basit bir [HTTP ile tetiklenen](/azure/azure-functions/functions-bindings-http-webhook) tetikleme HTTP istek gövdesini yankılayan işlevi.

## <a name="run-functions-locally-in-the-ide"></a>İşlevleri IDE içinde yerel olarak çalıştırma

> [!NOTE]
> [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2) çalıştırın ve işlevleri yerel olarak hata ayıklama için yüklü olması gerekir.

1. Oluşturulan projeye sağ tıklayın ve ardından **Çalıştır** ve **Maven derleme**.
1. İçinde **yapılandırmasını Düzenle** iletişim kutusunda, Enter `package` içinde **hedefleri** ve **adı** alanlarını seçip **çalıştırma**. Derleme ve işlev kodunu paketi.
1. Derleme tamamlandıktan sonra yukarıdaki gibi kullanarak başka bir çalıştırma yapılandırması oluşturun `azure-functions:run` adını ve hedef olarak. Seçin **çalıştırma** işlevi IDE'de çalıştırmak için.

İşiniz bittiğinde, konsol penceresinde çalışma zamanı sonlandırma işlevinizi test etme. İşlevi yalnızca bir ana bilgisayar etkin olduğu ve çalıştırıldığı yerel olarak bir zaman olabilir.

### <a name="debug-the-function-in-eclipse"></a>Hata ayıklayın eclipse'te işlevi

İçinde **Çalıştır** yapılandırma değişikliği önceki adımda ayarladığınız `azure-functions:run` için `mvn azure-functions:run -DenableDebug` ve güncelleştirilmiş yapılandırmayı hata ayıklama modunda işlev uygulamasını başlatmak için çalıştırın.

Seçin **çalıştırma** menü ve açık **hata ayıklama yapılandırmaları**. Seçin **uzak Java uygulaması** ve yeni bir tane oluşturun. Yapılandırmanıza bir ad verin ve ayarlarını doldurun. Bağlantı noktası olan varsayılan işlevi konak tarafından açılmış hata ayıklama bağlantı noktası ile tutarlı olmalıdır `5005`. Kurulum sonrasında, tıklayarak `Debug` hata ayıklama başlatılamıyor.

![Eclipse işlevlerinde hata ayıklama](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

Kesme noktaları ayarlayın ve nesneleri IDE kullanarak işlevinizi denetleyin. İşiniz bittiğinde hata ayıklayıcı ve çalışan işlevi konak durdurun. Yalnızca bir işlev konak aynı anda etkin olduğu ve çalıştırıldığı yerel olarak en olabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. [Azure CLI ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bilgisayarınızın komut istemini kullanarak devam etmeden önce.

```azurecli
az login
```

Kodunuzu yeni işlev uygulama kullanarak bir dağıtın `azure-functions:deploy` Maven hedefi yeni **Çalıştır** yapılandırma.

Dağıtım tamamlandığında, Azure işlev uygulamanıza erişmek için kullanabileceğiniz URL’yi görürsünüz:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Sonraki adımlar

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin.
