---
title: Azure Key Vault müşteri veri özellikleri | Microsoft Docs
description: Anahtar Kasası'nda müşteri verileri hakkında bilgi edinin
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/22/2018
ms.author: barclayn
ms.openlocfilehash: 807b8a17570dfdcefa07a42719b7b0b24e4f31d7
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46297171"
---
# <a name="azure-key-vault-customer-data-features"></a>Azure Key Vault müşteri veri özellikleri

Azure anahtar kasası oluşturma veya güncelleştirme kasaları, anahtarlara, parolalara, sertifikalar ve yönetilen depolama hesapları sırasında müşteri verilerini alır. Bu müşteri verileri, Azure portalında ve REST API aracılığıyla doğrudan görünür. Müşteri verilerini düzenlenemez veya güncelleştirmeyi veya verileri içeren nesneyi silme silindi.

Key Vault, bir kullanıcı veya uygulama eriştiğinde, sistem erişim günlükleri üretilir. Ayrıntılı erişim günlükleri, Azure Insights kullanan müşteriler için kullanılabilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verileri tanımlama

Azure Key Vault içinde müşteri verileri aşağıdaki bilgileri tanımlar:

- Azure anahtar kasası için erişim ilkeleri içeren nesne kimlikleri kullanıcıları, grupları ve uygulamaları temsil eden
- Sertifika konuları, e-posta adresleri veya diğer kullanıcı ya da Kurumsal tanımlayıcılar içerebilir.
- Sertifika kişileri kullanıcı e-posta adresleri, adları veya telefon numaralarını içermelidir.
- Sertifika verenler, e-posta adresleri, adları, telefon numaraları, hesap kimlik bilgilerini ve Kuruluş ayrıntılarını içerebilir.
- Rasgele etiket, Azure anahtar Kasası'nda nesnelere uygulanabilir. Bu nesneler, kasaları, anahtarları, parolaları, sertifikaları ve depolama hesapları içerir. Kullanılan etiketleri kişisel verileri içerebilir.
- Azure anahtar kasası erişim günlükleri içeren nesne kimlikleri, [UPN](../active-directory/hybrid/plan-connect-userprincipalname.md)ve her REST API çağrısı için IP adresleri
- Azure anahtar kasası tanılama günlükleri, nesne kimlikleri ve IP adresleri için REST API çağrıları içerebilir.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

REST API'leri, portalı deneyimini ve kasalar, anahtarlara, parolalara, sertifikalar ve yönetilen depolama hesabı oluşturmak için kullanılan SDK'ları da güncelleştirin ve bu nesneyi silmek olanağına sahip olursunuz.

Geçici silme, silmeyi sonra 90 gün silinen verileri kurtarmak sağlar. Geçici silme kullanırken, verilerin kalıcı olarak temizleme işlemi gerçekleştirerek bekletme süresi dolmadan 90 gün önce silinebilir. Abonelik ve kasa için yapılandırılmış olması halinde blok temizleme işlemleri, zamanlanmış bekletme süresi bitene kadar verileri kalıcı olarak silmek mümkün değildir.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma

REST API'leri, portal deneyimi ve kasalar, anahtarları, parolaları, sertifikaları oluşturmak için kullanılan ve yönetilen depolama hesapları da SDK'ları görüntülemek ve bu nesneleri dışarı aktarmak sağlar.

Azure anahtar kasası erişim günlüğü için açık isteğe bağlı bir özellik olan her REST API çağrısı için günlükler oluşturur. Bu günlükler, kuruluşunuzun gereksinimlerini karşılayan bekletme ilkenin uygulandığı Aboneliğinize bir depolama hesabına aktarılır.

Kişisel verileri içeren azure anahtar kasası tanılama günlükleri, kullanıcı gizlilik Portalı'nda bir dışarı aktarma isteği yaparak alınabilir. Bu istek Kiracı Yöneticisi tarafından yapılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası Günlüğü](key-vault-logging.md)

- [Azure Key Vault geçici silmeyi genel bakış](key-vault-soft-delete-cli.md)

- [Azure Key Vault anahtar işlemleri](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure Key Vault'a gizli dizi işlemleri](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Azure Key Vault, sertifikalar ve ilkeleri](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Azure Key Vault depolama hesabı işlemleri](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
