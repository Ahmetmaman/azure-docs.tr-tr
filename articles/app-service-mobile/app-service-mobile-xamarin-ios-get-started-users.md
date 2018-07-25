---
title: Mobile Apps için kimlik doğrulaması, Xamarin iOS kullanmaya başlayın
description: Kimlik sağlayıcıları, AAD, Google, Facebook, Twitter ve Microsoft gibi çeşitli Xamarin iOS uygulamanızdaki kullanıcıların kimliğini doğrulamak için Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: crdun
ms.openlocfilehash: be6ee88f43254ec3075a64299005d3597af968e7
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224774"
---
# <a name="add-authentication-to-your-xamarinios-app"></a>Xamarin.iOS uygulamanıza kimlik doğrulaması ekleyin
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu konu bir App Service mobil uygulama istemci uygulamanızın kullanıcılarının kimlik doğrulaması yapmayı gösterir. Bu öğreticide, App Service tarafından desteklenen bir kimlik sağlayıcısı kullanarak Xamarin.iOS hızlı başlangıç projesi için kimlik doğrulaması ekleyin. Mobil uygulamanız tarafından yetkili başarıyla yapıldığını ve sonra kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo verilerine erişmek mümkün olacaktır.

Öğreticiyi tamamlamak [bir Xamarin.iOS uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, kimlik doğrulaması uzantı paketi projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama Hizmetleri'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, kullandığımız URL şeması _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. [Azure portalı] uygulama hizmetinizi seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **izin verilen dış yönlendirme URL'leri**, girin `url_scheme_of_your_app://easyauth.callback`.  **Url_scheme_of_your_app** bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

4. **Tamam**’a tıklayın.

5. **Kaydet**’e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. Visual Studio veya Xamarin Studio, bir cihaz veya öykünücü üzerinde istemci projesini çalıştırın. Uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulayın. Hata, hata ayıklayıcı konsoluna kaydedilir. Bu nedenle Visual Studio'da çıktı penceresinde bir hata görürsünüz.

&nbsp;&nbsp;Uygulama Kimliği doğrulanmamış bir kullanıcı olarak mobil uygulama arka ucunuzu erişmeye çünkü bu yetkisiz hatası gerçekleşir. *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, istemci uygulaması isteği kaynaklarına mobil uygulama arka ucundan ile kimliği doğrulanmış bir kullanıcıyı güncelleştirir.

## <a name="add-authentication-to-the-app"></a>Uygulamaya kimlik doğrulaması ekleme
Bu bölümde, veri görüntülemeden önce oturum açma ekranında görüntülenmek üzere bir uygulama değiştirir. Uygulama başlatıldığında uygulama hizmetinize bağlanmak değildir ve herhangi bir veri görüntülenmez. İlk kez sonra kullanıcı oturum açma ekranı görünür yenileme hareketini gerçekleştirir; başarılı oturum açma işleminden sonra Yapılacaklar öğelerinin listesi görüntülenir.

1. İstemci proje dosyasını açın **QSTodoService.cs** ve aşağıdaki using deyimi ve `MobileServiceUser` QSTodoService sınıfa erişimcisi ile:
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Adlı yeni yöntemi ekleyin **doğrulaması** için **QSTodoService** aşağıdaki tanımıyla:

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                AppDelegate.ResumeWithURL = url => url.Scheme == "zumoe2etestapp" && client.ResumeWithURL(url);
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Bir Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, geçirilen değeri değiştirmek **LoginAsync** yukarıda aşağıdakilerden birine: _MicrosoftAccount_, _Twitter_,  _Google_, veya _WindowsAzureActiveDirectory_.

3. Açık **QSTodoListViewController.cs**. Yöntem tanımını değiştirme **ViewDidLoad** çağrısını kaldırma **RefreshAsync()** sonlarında:
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out the call to RefreshAsync
            // await RefreshAsync();
        }
4. Yöntemi Değiştir **RefreshAsync** , kimlik doğrulaması için **kullanıcı** özelliği null. Yöntem tanımını üstüne aşağıdaki kodu ekleyin:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. Açık **AppDelegate.cs**, aşağıdaki yöntemi ekleyin:

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. Açık **Info.plist** gidin, dosya **URL türleri** içinde **Gelişmiş** bölümü. Şimdi Yapılandır **tanımlayıcı** ve **URL şemalarını** URL türünü ve tıklatın **URL türü Ekle**. **URL şemaları** , {url_scheme_of_your_app} ile aynı olması gerekir.
7. Visual Studio, Mac ana ya da Visual Studio Mac için bağlı bir cihaz veya öykünücü hedefleyen istemci projesi çalıştırın. Uygulama veri görüntülendiğini doğrulayın.
   
    Görüntülenecek oturum açma ekranı açacak öğeleri listede aşağı çekerek yenileme hareketi gerçekleştirin. Geçerli kimlik bilgileri başarıyla girdikten sonra uygulama Yapılacaklar öğelerinin listesi görüntülenir ve veri güncelleştirmeleri yapabilirsiniz.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Bir Xamarin.iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
