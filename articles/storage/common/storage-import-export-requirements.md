---
title: Azure içeri/dışarı aktarma hizmeti için gereksinimler | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti için yazılım ve donanım gereksinimleri öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 58997b20c01f33037a5e5e149caa59e1630373ff
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75978466"
---
# <a name="azure-importexport-system-requirements"></a>Azure içeri/dışarı aktarma sistem gereksinimleri

Bu makalede, Azure içeri/dışarı aktarma hizmeti için önemli gereksinimler açıklanır. İçeri/dışarı aktarma hizmeti kullanmak ve ardından geri gerekirse işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Sabit sürücüleri aşağıdaki WAImportExport aracını kullanarak hazırlamanız **BitLocker Sürücü şifrelemesini destekleyen 64-bit işletim sistemi** desteklenir.


|Platform |Sürüm |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |

## <a name="other-required-software-for-windows-client"></a>Windows İstemcisi için gerekli diğer yazılım

|Platform |Sürüm |
|---------|---------|
|.NET Framework    | 4.5.1       |
| BitLocker        |  _          |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Azure içeri/dışarı aktarma hizmeti, aşağıdaki türlerde depolama hesapları destekler:

- Standart Genel Amaçlı v2 depolama hesapları (çoğu senaryo için önerilir)
- Blob Depolama hesapları
- Genel Amaçlı v1 depolama hesapları (klasik veya Azure Resource Manager dağıtımları),

Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabı genel bakış](storage-account-overview.md).

Her iş için veya yalnızca bir depolama hesabından veri aktarmak için kullanılabilir. Diğer bir deyişle, bir tek içeri/dışarı aktarma işi birden çok depolama hesabında yayılamaz. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [bir depolama hesabının nasıl oluşturulacağını](storage-account-create.md).

> [!IMPORTANT]
> Azure içeri dışarı aktarma hizmeti, depolama hesaplarını desteklemiyor burada [sanal ağ hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md) özelliği etkinleştirildi. 

## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Aşağıdaki listede yer alan depolama türlerinde Azure içeri/dışarı aktarma hizmeti ile desteklenir.


|İş  |Depolama hizmeti |Desteklenen  |Desteklenmiyor  |
|---------|---------|---------|---------|
|İçeri Aktarma     |  Azure Blob depolama <br><br> Azure dosya depolama       | Desteklenen blok Blobları ve sayfa blobları <br><br> Desteklenen dosyalar          |
|Dışarı Aktar     |   Azure Blob depolama       | Blok blobları, sayfa blobları ve ekleme BLOB'ları desteklenir         | Azure dosyaları desteklenmiyor


## <a name="supported-hardware"></a>Desteklenen donanım

Azure içeri/dışarı aktarma hizmeti için veri kopyalamak için desteklenen disk gerekir.

### <a name="supported-disks"></a>Desteklenen disk

Aşağıdaki listede yer alan disk, içeri/dışarı aktarma hizmeti ile kullanım için desteklenir.


|Disk türü  |Boyut  |Desteklenen |
|---------|---------|---------|
|SSD    |   2,5"      |SATA ııı          |
|HDD     |  2,5"<br>3,5"       |SATA II, SATA III         |

Aşağıdaki disk türleri desteklenmez:
- USBs.
- Yerleşik USB bağdaştırıcısı ile harici HDD.
- Harici bir HDD 'nin kasasını içinde olan diskler.

Tek içeri/dışarı aktarma işi sahip olabilir:
- En fazla 10 HDD/SSD.
- HDD/SSD her boyuttaki bir karışımını.

Çok sayıda sürücü birden fazla iş arasında yayılabilir ve oluşturulabilen iş sayısı üst sınırı yoktur yoktur. İçeri aktarma işleri için yalnızca ilk veri hacmi sürücüsünde işlenir. Veri birimi NTFS ile biçimlendirilmiş olması gerekir.

Zaman sabit sürücüleri hazırlama ve WAImportExport aracını kullanarak veri kopyalama, dış bir USB bağdaştırıcısı kullanabilirsiniz. En çok kullanıma hazır bir USB 3.0 veya üzeri bağdaştırıcıları çalışması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport Aracı'nı ayarlama](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure içeri dışarı aktarma REST API örneği](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)
