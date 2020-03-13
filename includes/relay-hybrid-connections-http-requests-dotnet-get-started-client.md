---
title: include dosyası
description: include dosyası
services: service-bus-relay
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 08/16/2018
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: ce29cd03de46e1d93d7f1f28f9f5184cd59a57e7
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79199888"
---
### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Relay’i oluştururken "İstemci Kimlik Doğrulaması Gerektirir" seçeneğini devre dışı bıraktıysanız dilediğiniz tarayıcıyla Karma Bağlantılar URL’sine istek gönderebilirsiniz. Korumalı uç noktalara erişmek için burada gösterilen `ServiceBusAuthorization` üst bilgisinde bir belirteç oluşturup geçirmeniz gerekir.

Visual Studio'da yeni bir **Konsol Uygulaması (.NET Framework)** projesi oluşturun.

### <a name="add-the-relay-nuget-package"></a>Geçiş NuGet paketini ekleme

1. Yeni oluşturulan projeye sağ tıklayıp **NuGet Paketlerini Yönet**'i seçin.
2. **Ön sürümü dahil et** seçeneğini işaretleyin. 
3. **Göz at**'ı seçip **Microsoft.Azure.Relay** araması yapın. Arama sonuçlarında **Microsoft Azure Geçişi**'ni seçin.
4. Sürüm alanında **2.0.0-preview1-20180523** girişini seçin. 
5. Yüklemeyi tamamlamak için **Yükle**'yi seçin. İletişim kutusunu kapatın.

### <a name="write-code-to-send-requests"></a>İstek göndermek için kod yazma

1. Program.cs dosyasının üst tarafındaki `using` deyimlerini aşağıdaki `using` deyimleriyle değiştirin:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Net.Http;
    using Microsoft.Azure.Relay;
    ```
2. Karma bağlantı ayrıntıları için sabitleri `Program` sınıfına ekleyin. Köşeli ayraçlar içindeki yer tutucuları, karma bağlantıyı oluştururken aldığınız değerlerle değiştirin. Tam ad alanı adını kullandığınızdan emin olun.
   
    ```csharp
    private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```
3. `Program` sınıfına aşağıdaki yöntemi ekleyin:
   
    ```csharp
    private static async Task RunAsync()
    {
        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                KeyName, Key);
        var uri = new Uri(string.Format("https://{0}/{1}", RelayNamespace, ConnectionName));
        var token = (await tokenProvider.GetTokenAsync(uri.AbsoluteUri, TimeSpan.FromHours(1))).TokenString;
        var client = new HttpClient();
        var request = new HttpRequestMessage()
        {
            RequestUri = uri,
            Method = HttpMethod.Get,
        };
        request.Headers.Add("ServiceBusAuthorization", token);
        var response = await client.SendAsync(request);
        Console.WriteLine(await response.Content.ReadAsStringAsync());        Console.ReadLine();
    }
    ```
4. Aşağıdaki kod satırını `Main` sınıfındaki `Program` yöntemine ekleyin.
   
    ```csharp
    RunAsync().GetAwaiter().GetResult();
    ```
   
    Program.cs aşağıdaki gibi görünmelidir:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Net.Http;
    using Microsoft.Azure.Relay;
   
    namespace Client
    {
        class Program
        {
            private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
            private const string ConnectionName = "{HybridConnectionName}";
            private const string KeyName = "{SASKeyName}";
            private const string Key = "{SASKey}";
   
            static void Main(string[] args)
            {
                RunAsync().GetAwaiter().GetResult();
            }
   
            private static async Task RunAsync()
            {
               var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                KeyName, Key);
                var uri = new Uri(string.Format("https://{0}/{1}", RelayNamespace, ConnectionName));
                var token = (await tokenProvider.GetTokenAsync(uri.AbsoluteUri, TimeSpan.FromHours(1))).TokenString;
                var client = new HttpClient();
                var request = new HttpRequestMessage()
                {
                    RequestUri = uri,
                    Method = HttpMethod.Get,
                };
                request.Headers.Add("ServiceBusAuthorization", token);
                var response = await client.SendAsync(request);
                Console.WriteLine(await response.Content.ReadAsStringAsync());
            }
        }
    }
    ```

