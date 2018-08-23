---
title: Visual Studio'dan Azure VM için bir Web uygulaması yayımlama
description: Visual Studio'dan Azure sanal makinesi için bir ASP.NET Web uygulaması yayımlama
services: virtual-machines-windows
author: ghogen
manager: douge
tags: azure-service-management
ms.assetid: 70267837-3629-41e0-bb58-2167ac4932b3
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/03/2017
ms.author: ghogen
ms.openlocfilehash: 561de3ac9073fe5cfdfadf2dc61d3f7807bf9e46
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057050"
---
# <a name="publish-an-aspnet-web-app-to-an-azure-vm-from-visual-studio"></a>Visual Studio'dan Azure VM için bir ASP.NET Web uygulaması yayımlama

Bu belgeyi kullanarak bir Azure sanal makinesi (VM) için bir ASP.NET web uygulaması yayımlama açıklanır **Microsoft Azure sanal makineleri** Visual Studio 2017'deki Yayımlama özelliği.  

## <a name="prerequisites"></a>Önkoşullar
Bir Azure sanal makinesi için bir ASP.NET projesi yayımlamak için Visual Studio kullanmak için VM doğru şekilde ayarlanması gerekir.

- Bir ASP.NET web uygulamasını çalıştırmak ve WebDeploy yüklü olması için makine yapılandırılması gerekir.

- VM, yapılandırılmış bir DNS adı olmalıdır. Daha fazla bilgi için [tam etki alanı adını Azure portalında bir Windows VM için oluşturma](portal-create-fqdn.md).

## <a name="publish-your-aspnet-web-app-to-the-azure-vm-using-visual-studio"></a>ASP.NET web uygulamanızı Visual Studio kullanarak Azure VM yayımlama
Aşağıdaki bölümde, Azure sanal makinesi için mevcut bir ASP.NET web uygulamasını yayımlama açıklar.

1. Visual Studio 2017'de, web uygulaması çözümünü açın.
2. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Yayımla...**
3. Yayımlama seçeneklerini bulana kadar kaydırın, sayfanın sağ tarafındaki oku kullanın **Microsoft Azure sanal makineleri**.  

   ![Yayımlama Sayfası - sağ ok]

4. Seçin **Microsoft Azure sanal makineleri** simgesini seçip alt **Yayımla**.

   ![Yayımlama Sayfası - Microsoft Azure sanal makinesi simgesi]

5. (Sanal makinenize bağlı Azure aboneliğiyle) uygun hesabı seçin.  
   - Visual Studio'ya oturum açmadıysanız, hesap listesi, kimliği doğrulanmış hesaplar ile doldurulur.  
   - "Oturum değil veya gereksinim duyduğunuz hesap listede yoksa, Ekle hesabınız..." öğesini seçin ve oturum açmak için istemleri takip edin.  
   ![Azure hesabı Seçici]  

6. Listeden var olan sanal makinelerin uygun sanal Makineyi seçin.

   > [!Note]
   > Bu listeyi doldurmak için biraz zaman alabilir.

   ![Azure VM Seçici]

7. Yayımlamayı başlatmak için Tamam'ı tıklatın.

8. Kimlik bilgileri istendiğinde, kullanıcı ve hedef rights (genellikle yönetici kullanıcı adı ve parola VM'yi oluştururken kullanılan) yayımlama ile yapılandırılmış sanal makine üzerinde bir kullanıcı hesabının parolasını sağlayın.  

   ![WebDeploy oturum açma]

9. Güvenlik sertifikası kabul edin.

   ![Sertifika hatası]

10. Yayımlama işleminin ilerleme durumunu denetlemek için çıkış penceresine bakın.

    ![Çıkış penceresi]

11. Yayımlama başarılı olursa, yeni yayımlanmış sitenin URL'sini açmak için bir tarayıcı başlatır.

**Başarılı!**

Şimdi başarıyla web uygulamanızı bir Azure sanal makinesine yayımladığınız.

## <a name="publish-page-options"></a>Yayımlama Sayfası Seçenekleri

Yayımlama sihirbazını tamamladıktan sonra Yayımlama Sayfası belgede seçili iyi yeni yayımlama profili ile açılır.

### <a name="re-publish"></a>Yeniden yayımlama

Web uygulamanız için güncelleştirmeleri yayımlamak için seçin **Yayımla** Yayımla sayfasında düğme.  
- İstenirse, kullanıcı adı ve parola girin.  
- Yayımlama hemen başlar.

![Yayımlama Sayfası - Yayımla düğmesi]

### <a name="modify-publish-profile-settings"></a>Değiştirme yayımlama profili ayarları

Görüntüleme ve yayımlama profili ayarlarını değiştirmek için **ayarları...** .  

![Yayımlama Sayfası - Ayarlar düğmesi]

Ayarlarınız aşağıdaki gibi görünmelidir:  

![Yayımlama ayarları - bağlantı sayfası]

#### <a name="save-user-name-and-password"></a>Kullanıcı adı ve Parolayı Kaydet
- Her yayımladığınızda kimlik doğrulama bilgileri sağlayarak önlemek için doldurabilirsiniz **kullanıcı adı** ve **parola** alanları ve select **Parolayı Kaydet** kutusu.
- Kullanım **bağlantıyı doğrula** doğru bilgileri girdiğinizden emin onaylamak için düğme.

#### <a name="deploy-to-clean-web-server"></a>Web sunucusu temizlemek için dağıtma

- Web sunucusu her yükleme sonrasında web uygulaması temiz bir kopyasını aldığından emin olmak istiyorsanız (ve önceki bir dağıtımın asılı geçici olarak başka hiçbir dosya bırakılır) denetleyebilirsiniz **hedefteki ek dosyaları Kaldır** onay kutusu **ayarları** sekmesi.

- Uyarı: Bu ayar ile yayımlama web sunucusunda (wwwroot dizinini) mevcut tüm dosyaları siler. Bu seçenek etkinleştirildiğinde yayımlamadan önce makinenin durumu bildiğinizden emin olun. 

![Yayımlama ayarları - Ayarlar sayfası]

## <a name="next-steps"></a>Sonraki adımlar

### <a name="set-up-cicd-for-automated-deployment-to-azure-vm"></a>Azure VM için otomatik dağıtım için CI/CD ayarlama

Visual Studio Team Service ile sürekli teslim işlem hattı ayarlamak için bkz: [dağıtım bir Windows sanal makinesine](https://docs.microsoft.com/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups).

[VM Overview - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSName.png
[IP Address Config - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/IPAddressConfigDNSName.png
[VM Overview - DNS Configured]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSConfigured.png
[Yayımlama Sayfası - sağ ok]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageRightArrow.png
[Yayımlama Sayfası - Microsoft Azure sanal makinesi simgesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageMicrosoftAzureVirtualMachineIcon.png
[Azure hesabı Seçici]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectAccount.png
[Azure VM Seçici]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectVM.png
[WebDeploy oturum açma]: ../../../includes/media/publish-web-app-from-visual-studio/WebDeployLogin.png
[Sertifika hatası]: ../../../includes/media/publish-web-app-from-visual-studio/CertificateError.png
[Çıkış penceresi]: ../../../includes/media/publish-web-app-from-visual-studio/OutputWindow.png
[Yayımlama Sayfası - Yayımla düğmesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPagePublishButton.png
[Yayımlama Sayfası - Ayarlar düğmesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageSettingsButton.png
[Yayımlama ayarları - bağlantı sayfası]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsConnectionPage.png
[Yayımlama ayarları - Ayarlar sayfası]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsSettingsPage.png
