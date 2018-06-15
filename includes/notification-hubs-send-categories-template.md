---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/30/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 19352df7abff23ed44521a11e7907c84c8c0327f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "33835857"
---
Bu bölümde, son dakika haberlerini .NET konsol uygulamasından etiketli şablon bildirimleri olarak yollarsınız. 

1. Visual Studio'da yeni bir Visual C# konsol uygulaması oluşturun:
   
      ![Konsol Uygulaması bağlantısı][13]

2. Visual Studio ana menüsünde, **Araçlar** > **Kitaplık Paketi Yöneticisi** > **Paket Yöneticisi Konsolu**’nu seçin ve ardından konsol penceresine aşağıdaki dizeyi girin:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
3. **Enter** tuşunu seçin.  
    Bu eylem [Microsoft.Azure.Notification Hubs NuGet paketi] kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.

4. Program.cs dosyasını açın ve aşağıdaki `using` deyimini ekleyin:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

5. `Program` sınıfında, aşağıdaki yöntemi ekleyin veya zaten mevcutsa değiştirin:
   
    ```csharp
    private static async void SendTemplateNotificationAsync()
    {
        // Define the notification hub.
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");

        // Create an array of breaking news categories.
        var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};

        // Send the notification as a template notification. All template registrations that contain
        // "messageParam" and the proper tags will receive the notifications.
        // This includes APNS, GCM, WNS, and MPNS template registrations.

        Dictionary<string, string> templateParams = new Dictionary<string, string>();

        foreach (var category in categories)
        {
            templateParams["messageParam"] = "Breaking " + category + " News!";
            await hub.SendTemplateNotificationAsync(templateParams, category);
        }
    }
    ```   
   
    Bu kod, dize dizisindeki altı etiketin her biri için bir şablon bildirimi gönderir. Etiketlerin kullanılması, cihazların yalnızca kayıtlı kategoriler için bildirim almasını sağlar.

5. Önceki kodda, `<hub name>` ve `<connection string with full access>` yer tutucularını bildirim hub'ı adınız ve bildirim hub’ınızın panosundaki *DefaultFullSharedAccessSignature* bağlantı dizeniz ile değiştirin.

6. **Ana** yöntemine aşağıdaki satırları ekleyin:
   
    ```csharp
    SendTemplateNotificationAsync();
    Console.ReadLine();
    ```

7. Konsol uygulamasını oluşturun.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs REST interface]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[How to use Notification Hubs from Java or PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet paketi]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
