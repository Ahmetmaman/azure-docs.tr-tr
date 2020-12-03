---
title: Azure Active Directory portalında toplu Kullanıcı oluşturma | Microsoft Docs
description: Azure Active Directory 'de Azure AD Yönetim Merkezi 'nde toplu olarak Kullanıcı ekleme
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 12/02/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: c653f3e8583ef3aadff26cb2b7a3266555d313a2
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96547823"
---
# <a name="bulk-create-users-in-azure-active-directory"></a>Azure Active Directory Kullanıcıları toplu olarak oluşturma

Azure Active Directory (Azure AD) toplu Kullanıcı oluşturma ve silme işlemlerini destekler ve kullanıcı listelerinin indirilmesini destekler. Azure AD portalından indirebileceğiniz, virgülle ayrılmış değerler (CSV) şablonunu doldurmanız yeterlidir.

## <a name="required-permissions"></a>Gerekli izinler

Yönetim Portalı 'nda kullanıcıları toplu olarak oluşturmak için, genel yönetici veya Kullanıcı Yöneticisi olarak oturum açmış olmanız gerekir.

## <a name="understand-the-csv-template"></a>CSV şablonunu anlama

Azure AD kullanıcılarını toplu olarak başarıyla oluşturmanıza yardımcı olması için toplu karşıya yükleme CSV şablonunu indirin ve girin. İndirmediğiniz CSV şablonu şu örnekteki gibi görünebilir:

![Her satır ve sütunun amacını ve değerlerini açıklayan karşıya yükleme ve çağrı aşımları için elektronik tablo](./media/users-bulk-add/create-template-example.png)

> [!WARNING]
> CSV şablonunu kullanarak yalnızca bir girdi ekliyorsanız, satır 3 ' ü korumanız ve yeni girdinizi satır 4 ' e eklemeniz gerekir.

### <a name="csv-template-structure"></a>CSV şablonu yapısı

İndirilen bir CSV şablonundaki satırlar aşağıdaki gibidir:

- **Sürüm numarası**: sürüm numarasını içeren ilk satır, KARŞıYA yükleme CSV 'ye eklenmelidir.
- **Sütun başlıkları**: sütun başlıklarının biçimi &lt; *öğe adı* &gt; [PropertyName] &lt; *gerekli veya boş* &gt; . Örneğin, `Name [displayName] Required`. Şablonun bazı eski sürümlerinde hafif Çeşitlemeler bulunabilir.
- **Örnekler satırı**: şablona her sütun için kabul edilebilir değer örneklerinin bir satırını ekledik. Örnekler satırını kaldırmalı ve kendi girişlerinizin yerine değiştirmelisiniz.

### <a name="additional-guidance"></a>Ek yönergeler

- Karşıya yükleme şablonunun ilk iki satırı kaldırılmamalıdır veya değiştirilmemelidir veya karşıya yükleme işlenemiyor.
- Önce gerekli sütunlar listelenir.
- Şablona yeni sütun eklenmesini önermiyoruz. Eklediğiniz tüm ek sütunlar yoksayılır ve işlenmez.
- CSV şablonunun en son sürümünü mümkün olduğunca sık indirmeniz önerilir.
- Herhangi bir alandan önce/sonra hiçbir istenmeyen boşluk olmadığından emin olun. **Kullanıcı asıl adı** için, bu tür boşluklar içeri aktarma hatasına neden olur.

## <a name="to-create-users-in-bulk"></a>Toplu olarak Kullanıcı oluşturmak için

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
1. Azure AD 'de **Kullanıcılar**  >  **toplu oluştur**' u seçin.
1. **Toplu kullanıcı oluştur** sayfasında, kullanıcı özelliklerinin geçerli bir virgülle ayrılmış değerler (CSV) dosyasını almak için **İndir** ' i seçin ve sonra oluşturmak istediğiniz kullanıcı ekleme ' yi ekleyin.

   ![İçinde eklemek istediğiniz kullanıcıları listeettiğiniz yerel bir CSV dosyası seçin](./media/users-bulk-add/upload-button.png)

1. CSV dosyasını açın ve oluşturmak istediğiniz her kullanıcı için bir satır ekleyin. Yalnızca **ad**, **Kullanıcı asıl adı**, **ilk parola** ve **blok oturum açma (Evet/Hayır)** değerleri bulunur. Ardından dosyayı kaydedin.

   [![CSV dosyası oluşturulacak kullanıcıların adlarını ve kimliklerini içerir](./media/users-bulk-add/add-csv-file.png)](./media/users-bulk-add/add-csv-file.png#lightbox)

1. **Toplu kullanıcı oluştur** SAYFASıNDA, CSV dosyanızı karşıya yükleyin bölümünde dosyaya gidin. Dosyayı seçip **Gönder**' e TıKLADıĞıNıZDA, CSV dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandıktan sonra, **dosyanın başarıyla karşıya yüklendiğini** görürsünüz. Hatalar varsa, işi gönderebilmeniz için önce bunları çözmeniz gerekir.
1. Dosyanız doğrulamayı geçtiğinde, yeni kullanıcıları içeri aktaran Azure toplu işlemini başlatmak için **Gönder** ' i seçin.
1. İçeri aktarma işlemi tamamlandığında, toplu işlem iş durumunun bir bildirimini görürsünüz.

Hatalar varsa, sonuçlar dosyasını **toplu işlem sonuçları** sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir. Dosya gönderimi, belirtilen şablonla eşleşmelidir ve tam sütun adlarını içermelidir.

## <a name="check-status"></a>Durumu kontrol etme

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

   [![Toplu Işlemler sonuçları sayfasında durum oluşturmayı denetle](./media/users-bulk-add/bulk-center.png)](./media/users-bulk-add/bulk-center.png#lightbox)

Daha sonra, oluşturduğunuz kullanıcıların Azure portal veya PowerShell kullanarak Azure AD kuruluşunda mevcut olup olmadığını kontrol edebilirsiniz.

## <a name="verify-users-in-the-azure-portal"></a>Azure portal kullanıcıları doğrulama

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD Yönetim merkezinde oturum açın](https://aad.portal.azure.com) .
1. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
1. **Yönet** bölümünde **Kullanıcılar**'ı seçin.
1. **Göster** altında, **tüm kullanıcılar** ' ı seçin ve oluşturduğunuz kullanıcıların listelendiğini doğrulayın.

### <a name="verify-users-with-powershell"></a>Kullanıcıları PowerShell ile doğrulama

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Oluşturduğunuz kullanıcıların listelendiğini görmeniz gerekir.

## <a name="bulk-import-service-limits"></a>Toplu içeri aktarma hizmeti sınırları

Kullanıcıları oluşturmak için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 50.000 kullanıcının toplu oluşturulmasına izin verebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcı listesini indir](users-bulk-download.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
