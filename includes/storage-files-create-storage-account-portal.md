---
title: storage-files-create-storage-account-portal
description: Azure Dosyaları için depolama hesabı oluşturun.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: d4054760c77a7a70b7ed84a9f95b88a3bcf2bda3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76020949"
---
Depolama hesabı, Azure dosya paylaşımını veya bloblar veya sorgular gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda paylaşım olabilir. Bir paylaşım, depolama hesabının kapasite limitlerine kadar sınırsız sayıda dosyayı depolayabilir.

Depolama hesabı oluşturmak için:

1. Sol menüde, **+** kaynak oluşturmak için seçin.
2. Arama kutusuna **depolama hesabı** girin, **Depolama hesabı - blob, dosya, tablo, kuyruk** seçeneğini belirleyin ve **Oluştur**’u seçin.
    ![Kaynak araması iletişim kutusunda, depolama hesabı girişinin nasıl göründüğüne dair ekran görüntüsü](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. **Ad** alanına *mystorageacct* komutunu yazın ve benzersiz bir ad olduğunu belirten yeşil onay işareti görüntüleninceye kadar komutun sonuna birkaç rastgele sayı yazın. Depolama hesabı adı tamamen küçük harflerden oluşmalı ve genel olarak benzersiz olmalıdır. Depolama hesabınızın adını not edin. Daha sonra bu adı kullanacaksınız. 
4. **Dağıtım modelinde,** **Kaynak Yöneticisi'nin**varsayılan değerini bırakın. Azure Resource Manager ve klasik dağıtım modeli arasındaki farklılıklar hakkında daha fazla bilgi edinmek için bkz. [Dağıtım modellerini ve kaynaklarınızın durumunu anlama](../articles/azure-resource-manager/management/deployment-models.md).
5. **Hesap türü** bölümünde **StorageV2** seçeneğini belirleyin. Farklı türlerde depolama hesapları hakkında daha fazla bilgi edinmek için bkz. [Azure depolama hesaplarını anlama](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
6. **Performans** bölümünde, varsayılan **Standart depolama** değerini değiştirmeden bırakın. Azure Dosyaları şu anda yalnızca standart depolama alanını destekler; Azure Premium Depolama’yı seçseniz de dosya paylaşımınız standart depolama alanında depolanır.
7. **Çoğaltma** bölümünde **Yerel olarak yedekli depolama (LRS)** seçeneğini belirleyin. 
8. **Güvenli aktarım gereklidir** bölümünde her zaman **Etkin** seçeneğini belirlemenizi öneririz. Bu seçenek hakkında daha fazla bilgi için bkz. [Aktarım sırasında şifrelemeyi anlama](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
9. **Abonelik** bölümünde, depolama hesabını oluşturmak için kullanılan aboneliği seçin. Yalnızca bir aboneliğiniz varsa bu, varsayılan değer olmalıdır.
10. **Kaynak grubu**’nda **Yeni oluştur**’u seçin. Ad için *myResourceGroup* girin.
11. **Konum** bölümünde **Doğu ABD**’yi seçin.
12. **Sanal ağlar** bölümünde varsayılan seçeneği **Devre dışı** olarak değiştirmeden bırakın. 
13. Depolama hesabının kolayca bulunması için **Panoya sabitle**’yi seçin.
14. İşleminiz bittiğinde dağıtımı başlatmak için **Oluştur**’a tıklayın.