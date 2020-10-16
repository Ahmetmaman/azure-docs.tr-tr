---
title: Azure Key Vault müşteri verileri özellikleri-Azure Key Vault | Microsoft Docs
description: Kasaların, anahtarların, parolaların, sertifikaların ve yönetilen depolama hesaplarının oluşturulması veya güncelleştirilmesi sırasında Azure Key Vault aldığı müşteri verileri hakkında bilgi edinin.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: reference
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 9b320c765b258473acd6fa553831b4f075cca9ba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983235"
---
# <a name="azure-key-vault-customer-data-features"></a>Azure Key Vault müşteri verileri özellikleri

Azure Key Vault, kasaların, yönetilen HSM havuzlarının, anahtarların, parolaların, sertifikaların ve yönetilen depolama hesaplarının oluşturulması veya güncelleştirilmesi sırasında müşteri verilerini alır. Bu müşteri verileri doğrudan Azure portal ve REST API üzerinden görünür. Verileri içeren nesne güncelleştirilerek veya silinerek müşteri verileri düzenlenebilir veya silinebilir.

Sistem erişim günlükleri, bir kullanıcı veya uygulama Key Vault eriştiğinde oluşturulur. Ayrıntılı erişim günlükleri, Azure Insights kullanan müşteriler tarafından kullanılabilir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verilerini tanımlama

Aşağıdaki bilgiler Azure Key Vault içindeki müşteri verilerini tanımlar:

- Azure Key Vault erişim ilkeleri, kullanıcıları, grupları veya uygulamaları temsil eden nesne kimliklerini içerir
- Sertifika konuları e-posta adresleri veya diğer Kullanıcı veya kuruluş tanımlayıcıları içerebilir
- Sertifika kişileri kullanıcı e-posta adreslerini, adlarını veya telefon numaralarını içerebilir
- Sertifika verenler e-posta adresleri, adlar, telefon numaraları, hesap kimlik bilgileri ve kuruluş ayrıntıları içerebilir
- Azure Key Vault içindeki nesnelere rastgele Etiketler uygulanabilir. Bu nesneler kasa, anahtar, gizli dizi, sertifika ve depolama hesapları içerir. Kullanılan Etiketler kişisel veri içeriyor olabilir
- Azure Key Vault erişim günlükleri her bir REST API çağrısının nesne kimliklerini, [UPN](../../active-directory/hybrid/plan-connect-userprincipalname.md)'LERINI ve IP adreslerini içerir
- Azure Key Vault tanılama günlükleri, REST API çağrılar için nesne kimlikleri ve IP adresleri içerebilir

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Kasalar, anahtarlar, gizlilikler, sertifikalar ve yönetilen depolama hesapları oluşturmak için kullanılan REST API 'Leri, Portal deneyimi ve SDK 'Lar, bu nesneleri güncelleştirebilir ve silebilir.

Geçici silme, silme işleminden sonra 90 gün boyunca silinen verileri kurtarmanızı sağlar. Geçici silme kullanılırken, veriler temizleme süresi dolmadan 90 gün önce kalıcı olarak silinmiş olabilir. Kasa veya abonelik temizleme işlemlerini engelleyecek şekilde yapılandırıldıysa, zamanlanan Bekletme dönemi geçene kadar verileri kalıcı olarak silmek mümkün değildir.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Kasalar, anahtarlar, gizlilikler, sertifikalar ve yönetilen depolama hesapları oluşturmak için kullanılan REST API 'Leri, Portal deneyimi ve SDK 'Lar, bu nesneleri görüntülemenize ve dışa aktarmaya da imkan tanır.

Azure Key Vault erişim günlüğü, her REST API çağrısı için günlük oluşturmak üzere açık olan isteğe bağlı bir özelliktir. Bu Günlükler, kuruluşunuzun gereksinimlerini karşılayan bekletme ilkesini uyguladığınız aboneliğinizdeki bir depolama hesabına aktarılır.

Kişisel verileri içeren Azure Key Vault tanılama günlükleri, Kullanıcı Gizlilik portalında dışarı aktarma isteği yapılarak alınabilir. Bu isteğin kiracı yöneticisi tarafından yapılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Günlüğe kaydetme Azure Key Vault](logging.md)

- [Azure Key Vault geçici silmeye genel bakış](soft-delete-cli.md)

- [Azure Key Vault anahtar işlemleri](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure Key Vault gizli işlemler](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Sertifikaları ve ilkeleri Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Depolama hesabı işlemlerini Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
