---
title: Windows için Azure Notification Hubs güvenli gönderimi
description: Azure 'da güvenli anında iletme bildirimleri göndermeyi öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri.
author: sethmanheim
manager: femila
editor: thsomasu
services: notification-hubs
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 09/14/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 98e587103e63cd5cc26eab5b00864d00e0b9007f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96019438"
---
# <a name="send-secure-push-notifications-from-azure-notification-hubs"></a>Azure Notification Hubs güvenli anında iletme bildirimleri gönderin

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

Microsoft Azure anında iletme bildirimi desteği, mobil platformlar için hem tüketici hem de kurumsal uygulamalarda anında iletme bildirimlerinin uygulanmasını büyük ölçüde kolaylaştıran, kullanımı kolay, çok platformlu, ölçeği genişletilmiş bir gönderim altyapısına erişmenizi sağlar.

Yasal düzenlemeler veya güvenlik kısıtlamaları nedeniyle, bazen bir uygulama, bildirime standart anında iletme bildirimi altyapısı üzerinden aktarılamayan bir şey eklemek isteyebilir. Bu öğreticide, istemci aygıtı ile uygulama arka ucu arasında güvenli, kimliği doğrulanmış bir bağlantıyla gizli bilgiler göndererek aynı deneyimin nasıl elde edileceğini açıklanmaktadır.

Yüksek düzeyde, akış şu şekildedir:

1. Uygulama arka ucu:
   * Güvenli yükü arka uç veritabanında depolar.
   * Bu bildirimin KIMLIĞINI cihaza gönderir (güvenli bilgi gönderilmez).
2. Bildirim alırken cihazdaki uygulama:
   * Cihaz, güvenli yük isteyen arka uca iletişim kurar.
   * Uygulama, yükü cihazda bir bildirim olarak gösterebilir.

Önceki akışta (ve bu öğreticide), Kullanıcı oturum açtıktan sonra cihazın yerel depolamada bir kimlik doğrulama belirteci depoladığını varsaytık. Bu, cihazın bu belirteci kullanarak bildirimin güvenli yükünü alabilmesi için tamamen sorunsuz bir deneyim sağlar. Uygulamanız, kimlik doğrulama belirteçlerini cihazda depolamaz veya bu belirteçlerin kullanım tarihi dolmuşsa, bildirim alındıktan sonra cihaz uygulaması, kullanıcıdan uygulamayı başlatması istenmeden ilgili genel bir bildirim görüntülemelidir. Uygulama daha sonra kullanıcının kimliğini doğrular ve bildirim yükünü gösterir.

Bu öğreticide, anında iletme bildirimini güvenli bir şekilde nasıl göndereceğiniz gösterilmektedir. Öğretici [kullanıcılara bildirme](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) öğreticisini oluşturur, bu nedenle öncelikle bu öğreticideki adımları tamamlamalısınız.

> [!NOTE]
> Bu öğreticide, Bildirim Hub 'ınızı [Evrensel Windows platformu uygulamalarına bildirim gönderme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)bölümünde açıklandığı gibi oluşturduğunuzu ve yapılandırdığınızı varsaymaktadır.
> Ayrıca, Windows Phone 8,1 ' nin Windows (Windows Phone değil) kimlik bilgilerini gerektirdiğini ve arka plan görevlerinin Windows Phone 8,0 veya Silverlight 8,1 üzerinde çalışmadığına de unutmayın. Windows Mağazası uygulamaları için, yalnızca uygulama kilit ekranı etkinse bir arka plan görevi aracılığıyla bildirim alabilirsiniz (AppManifest 'teki onay kutusuna tıklayın).

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Windows Phone projesini değiştirme

1. **NotifyUserWindowsPhone** projesinde, anında iletme arka plan görevini kaydetmek için App.xaml.cs öğesine aşağıdaki kodu ekleyin. `OnLaunched()` yönteminin sonuna aşağıdaki kod satırını ekleyin:

    ```csharp
    RegisterBackgroundTask();
    ```

2. Hala App.xaml.cs içinde, aşağıdaki kodu yönteminden hemen sonra ekleyin `OnLaunched()` :

    ```csharp
    private async void RegisterBackgroundTask()
    {
        if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
        {
            var result = await BackgroundExecutionManager.RequestAccessAsync();
            var builder = new BackgroundTaskBuilder();

            builder.Name = "PushBackgroundTask";
            builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
            builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
            BackgroundTaskRegistration task = builder.Register();
        }
    }
    ```

3. `using`App.xaml.cs dosyasının en üstüne aşağıdaki deyimleri ekleyin:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Windows.ApplicationModel.Background;
    ```

4. Visual Studio'daki **Dosya** menüsünde **Tümünü Kaydet**'e tıklayın.

## <a name="create-the-push-background-component"></a>Anında iletme arka plan bileşeni oluşturma

Sonraki adım, anında iletme arka plan bileşeni oluşturmaktır.

1. Çözüm Gezgini, çözümün en üst düzey düğümüne (Bu durumda **çözüm SecurePush** ) sağ tıklayın ve ardından **Ekle**' ye ve ardından **Yeni proje**' ye tıklayın.
2. **Mağaza uygulamaları**' nı genişletin ve ardından **Windows Phone Uygulamalar**' a ve ardından **Windows çalışma zamanı bileşen (Windows Phone)** seçeneğine tıklayın. Projeyi **Pushbackgroundcomponent** olarak adlandırın ve ardından projeyi oluşturmak için **Tamam** ' a tıklayın.

    ![Windows Çalışma Zamanı bileşeni (Windows Phone) Visual C# seçeneği vurgulanmış şekilde yeni proje Ekle iletişim kutusunun ekran görüntüsü.][12]
3. Çözüm Gezgini, **Pushbackgroundcomponent (Windows Phone 8,1)** projesine sağ tıklayın ve ardından **Ekle**' ye ve ardından **sınıf**' a tıklayın. Yeni sınıfı adlandırın `PushBackgroundTask.cs` . Sınıfı oluşturmak için **Ekle** ' ye tıklayın.
4. `PushBackgroundComponent`Ad alanı tanımının tüm içeriğini aşağıdaki kodla değiştirin ve yer tutucusunu, `{back-end endpoint}` arka ucunuzda dağıtım sırasında elde edilen arka uç bitiş noktasıyla değiştirin:

    ```csharp
    public sealed class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public sealed class PushBackgroundTask : IBackgroundTask
        {
            private string GET_URL = "{back-end endpoint}/api/notifications/";

            async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
            {
                // Store the content received from the notification so it can be retrieved from the UI.
                RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                var notificationId = raw.Content;

                // retrieve content
                BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                var httpClient = new HttpClient();
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                ShowToast(notification);

                deferral.Complete();
            }

            private void ShowToast(Notification notification)
            {
                ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                ToastNotification toast = new ToastNotification(toastXml);
                ToastNotificationManager.CreateToastNotifier().Show(toast);
            }
        }
    ```

5. Çözüm Gezgini, **Pushbackgroundcomponent (Windows Phone 8,1)** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**' e tıklayın.
6. Sol taraftaki **Çevrimiçi** öğesine tıklayın.
7. **Arama** kutusuna **Http İstemcisi** yazın.
8. Sonuçlar listesinde, **MICROSOFT http Istemci kitaplıkları**' nı ve ardından, ardından **Install**' ı tıklatın. Yüklemeyi tamamlayın.
9. NuGet **Arama** kutusuna **Json.net** yazın. **JSON.net** paketini yükledikten sonra NuGet Paket Yöneticisi penceresini kapatın.
10. Aşağıdaki `using` deyimlerini dosyanın üst kısmına ekleyin `PushBackgroundTask.cs` :

    ```csharp
    using Windows.ApplicationModel.Background;
    using Windows.Networking.PushNotifications;
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using Windows.UI.Notifications;
    using Windows.Data.Xml.Dom;
    ```

11. Çözüm Gezgini, **NotifyUserWindowsPhone (Windows Phone 8,1)** projesinde, **Başvurular**' a sağ tıklayın ve ardından **Başvuru Ekle...** öğesine tıklayın. Başvuru Yöneticisi iletişim kutusunda **Pushbackgroundcomponent**' un yanındaki kutuyu işaretleyin ve ardından **Tamam**' a tıklayın.
12. Çözüm Gezgini ' de, **NotifyUserWindowsPhone (Windows Phone 8,1)** projesinde **Package. appxmanifest** öğesine çift tıklayın. **Bildirimler**' in altında, **bildirim** ' ı **Evet** olarak ayarlayın.

    ![Paket. appxmanifest ' e odaklanan Çözüm Gezgini penceresinin ekran görüntüsü, bildirim özellikli seçeneği kırmızı olarak seviyelendirilmiş şekilde ayarlanır.][3]
13. Hala **Package. appxbildiriminde**, üstteki **Bildirimler** menüsüne tıklayın. **Kullanılabilir bildirimler** açılır listesinde, **arka plan görevleri**' ne ve ardından **Ekle**' ye tıklayın.
14. **Package. appxmanifest** Içinde, **Özellikler** altında **anında iletme bildirimi**' ni işaretleyin.
15. **Package. appxmanifest** Içinde, **uygulama ayarları** altında, **giriş noktası** alanına **pushbackgroundcomponent. pushbackgroundtask** yazın.

    ![Paketteki Çözüm Gezgini pencerenin ekran görüntüsü, kullanılabilir bildirimler, desteklenen bildirimler, anında bildirimler ve kırmızı renkle özetlenen giriş noktası seçenekleridir.][13]
16. **Dosya** menüsünden **Tümünü Kaydet**'e tıklayın.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için şunları yapın:

1. Visual Studio 'da **Apparka uç** Web API uygulamasını çalıştırın. Bir ASP.NET Web sayfası görüntülenir.
2. Visual Studio 'da **NotifyUserWindowsPhone (Windows Phone 8,1)** Windows Phone uygulamasını çalıştırın. Windows Phone öykünücü çalışır ve uygulamayı otomatik olarak yükler.
3. **NotifyUserWindowsPhone** uygulama kullanıcı arabiriminde, bir Kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değer olmalıdır.
4. **NotifyUserWindowsPhone** uygulama kullanıcı arabiriminde, **oturum aç ve Kaydet**' e tıklayın. Sonra **gönderme gönder**' e tıklayın.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
