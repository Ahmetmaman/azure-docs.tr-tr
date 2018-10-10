---
title: Konuşma Cihazları SDK’sını edinme
description: Konuşma cihaz SDK'sı erişin öğrenin.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 91107601055a115efc039ac588f652eab2fba3cf
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887743"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Bilişsel hizmetler konuşma cihaz SDK'sı Al

Konuşma cihaz SDK'sı, sınırlı Önizleme aşamasındadır ve programda kaydedilmesini gerektirir. Şu anda, Microsoft bu ürüne erişim için aday olarak büyük şirketler tercih eder.

## <a name="request-access"></a>Erişim izni iste

Konuşma cihaz SDK'sı erişim elde etmek için:

1. Microsoft konuşma cihaz SDK'sı Git [kayıt formunu](https://aka.ms/sdsdk-signup).
1. Okuma [lisans sözleşmesini](speech-devices-sdk-license.md).
1. Lisans sözleşmesinin koşullarını kabul ediyorsanız **kabul ediyorum**.
1. Formda soruları yanıtlayın.
1. Form gönderilemiyor. 
1. E-posta adresiniz zaten Azure Active Directory (Azure AD) bir parçası değilse, erişim için onaylandıklarında, aşağıdaki örnekte olduğu gibi bir davet e-posta alırsınız. E-posta adresiniz zaten Azure AD'de ise, bir e-posta iletisi Microsoft konuşma takımdan erişim onayından ve, atlayabilirsiniz aldığınız [konuşma cihaz SDK'sını indirin](#download-the-speech-devices-sdk).

## <a name="approval-e-mail"></a>Onay e-postası

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com> 
Subject: You're invited to the Microsoft organization 
```

![e-posta iletisi](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Erişim

Kayıt sırasında sağladığınız e-posta adresiyle Azure AD'ye katılmak için aşağıdaki adımları tamamlayın. Bu işlem için konuşma cihaz SDK'sı erişiminizi [yükleme sitesine](https://shares.datatransfer.microsoft.com/).

1. Aldığınız e-posta iletisinde seçin **Başlarken**. Kuruluşunuz zaten bir Office 365 müşterisi ise, oturum açmanız istenir ve İleri 8. adımına atlayabilirsiniz.

2. Açılır tarayıcı penceresinde seçin **sonraki**.

    ![kimlik doğrulama penceresi](media/speech-devices-sdk/get-sdk-2.png)

3. Zaten yoksa, bir Microsoft hesabı oluşturun. Davet e-posta aldığınız aynı e-posta adresi girin.

    ![Bir Microsoft hesabı oluşturun](media/speech-devices-sdk/get-sdk-3.png)

4. Seçin **sonraki** bir parola oluşturmak için.

5. E-postanızı doğrulamak için istendiğinde, aldığınız davet e-postadan doğrulama kodunu alın.
 
7. Yapıştırın veya e-posta iletisi güvenlik kodunu iletişim kutusuna yazın. Bu örnekte, güvenlik koddur **8406**. **İleri**’yi seçin.

    ![e-posta doğrulama](media/speech-devices-sdk/get-sdk-6.png)
 
8. Erişim paneli uygulama tarayıcıda gördüğünüzde, e-posta adresinizi Azure AD parçası olduğunu doğruladı. Artık konuşma cihaz SDK'sını indirme sitesine erişebilirsiniz.

## <a name="download-the-speech-devices-sdk"></a>Konuşma cihaz SDK'sını indirin

Git [konuşma cihazları SDK indirme sitesi](https://shares.datatransfer.microsoft.com/). Daha önce oluşturduğunuz Microsoft hesabıyla oturum açın. 

![SDK indirme sitesi](media/speech-devices-sdk/get-sdk-7.png)

Konuşma indirmek için ilişkili cihaz SDK'sı, örnek kod ve başvuru kaynakları:

1. Tarayıcıda istendiğinde Aspera Connect aracını yükleyip yeniden açın.

    ![Aspera Connect indirin](media/speech-devices-sdk/get-sdk-8.png)
 
1. Seçin **Evet** Aspera bağlanmak için uygulamaları geçiş yapmak için.

    ![Aspera bağlanmak için geçiş](media/speech-devices-sdk/get-sdk-9.png)
 
1. Seçin **izin** Aspera Bağlan'ı kullanarak dosyaları indirme onaylamak için.

    ![Aspera Bağlan'ı kullanarak indirin](media/speech-devices-sdk/get-sdk-10.png)
 
1. Dosyaları İndirildikten sonra Aspera bağlanma aktarımları penceresini kapatın.

    ![Aspera aktarımları Bağlan penceresi](media/speech-devices-sdk/get-sdk-11.png)
 
Dosyalar varsayılan olarak, karşıdan yüklenir, **indirir** klasör. Bu site dışında artık oturum açabilir. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı ile çalışmaya başlama](speech-devices-sdk-qsg.md)
