---
title: Ve Azure Depolama Gezgini ile Blob depolama alanından verileri taşıma | Microsoft Docs
description: Azure Depolama Gezgini’ni kullanarak Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 7d4fc17c466f9f7187ca28c847631254d6600ead
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447011"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Ve Azure Depolama Gezgini'ni kullanarak Azure Blob depolama alanından verileri taşıma
Azure Depolama Gezgini, Microsoft'un Windows, macOS ve Linux'ta Azure depolama verileriyle çalışmanıza olanak sağlayan ücretsiz bir araçtır. Bu konuda, yüklemek ve Azure blob depolama alanından verileri indirmek için nasıl kullanılacağını açıklar. Aracı indirilebileceğini [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Tarafından sağlanan betikleri ile oluşturulan VM kullanıyorsanız [azure'da veri bilimi sanal makineleri](virtual-machines.md), ardından Azure Depolama Gezgini VM üzerinde zaten yüklü.
> 
> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu belge, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Karşıya yükleme/veri indirmeden önce Azure depolama hesabı adını ve hesap anahtarınızı bilmeniz gerekir. 

* Bir Azure aboneliğini ayarlama hakkında bilgi için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler için ve hesap ve anahtar bilgilerini almak için bkz: [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md). Bu anahtarı Azure Depolama Gezgini aracı ile hesaba bağlanmak için gereken depolama hesabınızın erişim anahtarı not edin.
* Azure Depolama Gezgini aracını indirilebileceğini [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/). Varsayılan yükleme işlemi sırasında kabul edin.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanma
Aşağıdaki adımlar, nasıl Azure Depolama Gezgini'ni kullanarak verileri karşıya yükleme/indirme belgeleyin. 

1. Microsoft Azure Depolama Gezgini'ni başlatın.
2. Ortaya çıkarmak için **hesabınızda oturum açın...**  seçin **Azure hesap ayarları** simgesine ve ardından **Hesap Ekle** ve kimlik bilgilerini girin. ![Bir Azure depolama hesabı ekleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Ortaya çıkarmak için **Azure Storage'a Bağlan** seçin **Azure Storage'a Bağlan** simgesi. ![Azure depolamaya Bağlan](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Azure depolama hesabınızın erişim anahtarını girin **Azure Storage'a Bağlan** Sihirbazı'nı ve ardından **sonraki**. ![Azure depolamaya Bağlan](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Depolama hesabı adını girin **hesap adı** kutusuna ve ardından **sonraki**. ![Dış depolama Ekle](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Eklediğiniz depolama hesabını listelenmiş olmalıdır. Bir depolama hesabında blob kapsayıcısı oluşturmak için **Blob kapsayıcıları** seçin, hesap düğümünde **Blob kapsayıcısı Oluştur**ve bir ad girin.
7. Verileri bir kapsayıcıya yüklemek için hedef kapsayıcıyı seçin ve **karşıya** düğmesi.![ Depolama hesapları](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Tıklayarak **...**  sağındaki **dosyaları** kutusunda, dosya sisteminden karşıya yükleyin ve bir veya birden çok dosya seçin **karşıya** dosyalar karşıya yüklenirken başlamaya.![ Dosyaları karşıya yükleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Verileri, blob indirmek ve'ı tıklatın, karşılık gelen kapsayıcı seçerek indirmek için **indirme**. ![Dosyaları indirme](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

