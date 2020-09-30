---
title: Azure Uygulama yapılandırması için .NET Framework ile hızlı başlangıç | Microsoft Docs
description: Bu makalede, Azure Uygulama yapılandırması ile bir .NET Framework uygulama oluşturarak uygulama ayarlarının kodunuzla ayrı olarak depolanmasını ve yönetimini merkezileştirin.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 09/28/2020
ms.author: lcozzens
ms.openlocfilehash: 4edf48b73ee4d50679cbec377ed47749d2c94776
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91569606"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama yapılandırmasıyla .NET Framework uygulaması oluşturma

Bu hızlı başlangıçta, kodınızdan ayrı uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure uygulama yapılandırmasını .NET Framework tabanlı bir konsol uygulamasına katabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/dotnet)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **Configuration Explorer**  >  **Create**  >  Aşağıdaki anahtar-değer çiftlerini eklemek için yapılandırma Gezgini**anahtar değeri** oluştur ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-net-console-app"></a>.NET konsol uygulaması oluşturma

1. Visual Studio 'yu başlatın ve **Dosya**  >  **Yeni**  >  **Proje**' yi seçin.

1. **Yeni proje oluştur**' da **konsol** proje türü ' ne filtre uygulayın ve konsol uygulaması ' na tıklayın **(.NET Framework)**. **İleri**’yi seçin.

1. **Yeni projenizi yapılandırma**bölümünde bir proje adı girin. **Framework**altında **.NET Framework 4.7.1** veya üstünü seçin. **Oluştur**’u seçin.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. **Araştır** sekmesinde, aşağıdaki NuGet paketlerini arayıp projenize ekleyin.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 or later
    System.Configuration.ConfigurationManager version 4.6.0 or later
    ```

1. Projenizin *App.config* dosyasını aşağıdaki gibi güncelleştirin:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   Uygulama yapılandırma deponuzın bağlantı dizesi, ortam değişkeninden okundu `ConnectionString` . `Environment`Bölümünün özelliğindeki öğesinden önce yapılandırma oluşturucuyu ekleyin `MyConfigStore` `configBuilders` `appSettings` .

1. *Program.cs*'i açın ve `Main` çağırarak uygulama yapılandırmasını kullanmak üzere yöntemi güncelleştirin `ConfigurationManager` .

    ```csharp
    static void Main(string[] args)
    {
        string message = System.Configuration.ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
        Console.ReadKey();
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. **App.config** dosyasını, `${ConnectionString}` gerçek bağlantı dizesiyle birlikte uygulama yapılandırma örneğiniz ile değiştirerek güncelleştirin. Azure portalındaki uygulama yapılandırma kaynağının **erişim tuşları** sekmesinde bulabilirsiniz.

1. Konsol uygulamasını derlemek ve çalıştırmak için CTRL + F5 tuşlarına basın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir uygulama yapılandırma deposu oluşturdunuz ve bir .NET Framework konsol uygulamasıyla kullandınız. Değeri `AppSettings` uygulama başlatıldıktan `ConfigurationManager` sonra değişmez. Uygulama yapılandırma .NET Standard yapılandırma sağlayıcısı kitaplığı, ancak aynı zamanda bir .NET Framework uygulamasında da kullanılabilir. Yapılandırma ayarlarını dinamik olarak yenilemek üzere .NET Framework uygulamanızın nasıl etkinleştirileceğini öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-dotnet.md)
