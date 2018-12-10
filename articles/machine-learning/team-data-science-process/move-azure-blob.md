---
title: Verileri Azure Blob depolamadan - Team Data Science Process taşıma
description: İçin ve Azure Blob depolamadan/depolamaya veri taşıma
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a37c19ac0d3c053644b2f1f970ef9f84eac2f1df
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139866"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>İçin ve Azure Blob depolamadan/depolamaya veri taşıma

Team Data Science Process veri veya alınan farklı depolama ortamları işlenen ya da işlemin her aşamasında en uygun şekilde analiz için çeşitli yüklenmiş olmasını gerektirir.

## <a name="different-technologies-for-moving-data"></a>Verileri taşımak için farklı teknolojiler

Aşağıdaki makaleleri ve farklı teknolojileri kullanarak Azure Blob depolama alanından veri taşıma konusunda açıklanmaktadır.

* [Azure Depolama Gezgini](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](move-data-to-azure-blob-using-azcopy.md)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Hangi sizin için en iyi bir yöntemdir, senaryoya bağlıdır. [Azure Machine learning'de Gelişmiş analiz senaryoları](plan-sample-scenarios.md) makale Gelişmiş analiz işleminde kullanılan veri bilimi iş akışları çeşitli için ihtiyacınız olan kaynakları belirlemenize yardımcı olur.

> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="using-azure-data-factory"></a>Azure Data Factory'yi kullanma

Alternatif olarak, kullandığınız [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) için: 

* oluşturun ve Azure blob depolamadan veri yükleyen işlem hattı zamanlama 
* yayımlanan bir Azure Machine Learning web hizmeti ile geçirin, 
* Tahmine dayalı analiz sonuçlarını almak ve 
* Sonuçları depolama alanına yükleyin. 

Daha fazla bilgi için [Azure Data Factory ve Azure Machine Learning kullanarak öngörülebilir komut zincirleri oluşturma](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Karşıya yükleme/veri indirmeden önce Azure depolama hesabı adını ve hesap anahtarınızı bilmeniz gerekir.

* Bir Azure aboneliğini ayarlama hakkında bilgi için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler için ve hesap ve anahtar bilgilerini almak için bkz: [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md).

