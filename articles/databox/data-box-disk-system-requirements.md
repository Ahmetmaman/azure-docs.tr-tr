---
title: Microsoft Azure Data Box Disk sistem gereksinimleri| Microsoft Docs
description: Azure Data Box Disk'in yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 02/22/2021
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 750ed8f65db04199ea284e69693bced65a1dc8d9
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703142"
---
::: zone target="docs"

# <a name="azure-data-box-disk-system-requirements"></a>Azure Data Box Disk sistem gereksinimleri

Bu makalede Microsoft Azure Data Box Disk çözümünüz ve Data Box Disk’e bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Data Box Disk’i dağıtmadan önce bilgileri dikkatlice gözden geçirmeniz ve dağıtım ile sonraki işlemler sırasında gerektikçe tekrar başvurmanız önerilir.

Sistem gereksinimleri disklere, desteklenen depolama hesaplarına ve depolama türlerine bağlanan istemciler için desteklenen platformları içerir.

::: zone-end

::: zone target="chromeless"

## <a name="review-prerequisites"></a>Önkoşulları inceleyin

1. [Öğretici: Azure Data Box Disk sipariş etme](data-box-disk-deploy-ordered.md) bölümünden yararlanarak Data Box Disk’inizi sipariş etmiş olmanız gerekir. Disklerinizi ve disk başına bir bağlantı kablosunu teslim almış olmanız gerekir.
2. Verileri kopyalayabilmeniz için kullanabileceğiniz bir istemci bilgisayarınızın olması gerekir. İstemci bilgisayarınızda:

    - Desteklenen bir işletim sistemi çalıştırmanız gerekir.
    - Diğer gerekli yazılımların yüklü olması gerekir.

::: zone-end

## <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

Data Box Disk’e bağlı istemciler aracılığıyla disk kilidini açma ve veri kopyalama işlemi için desteklenen işletim sistemlerinin listesi aşağıda verilmiştir.

| **İşletim sistemi** | **Sınanan sürümler** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows (64 bit) |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Windows istemcileri için gereken diğer yazılımlar

Windows istemcisi için aşağıdakilerin de yüklenmesi gerekir.

| **Yazılım**| **Sürüm** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5.1|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Linux istemcileri için gereken diğer yazılımlar

Linux istemcisi için Data Box Disk araç takımı aşağıdaki gerekli yazılımları yükler:

- dislocker
- OpenSSL

## <a name="supported-connection"></a>Desteklenen bağlantı

Verileri içeren istemci bilgisayarın USB 3.0 veya üzeri bir bağlantı noktasına sahip olması gerekir. Diskler verilen kablo ile bu istemciye bağlanır.

## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Data Box Disk için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Depolama hesabı** | **Notlar** |
| --- | --- |
| Klasik | Standart |
| Genel Amaçlı  |Standart; hem V1 hem de V2 desteklenir. Hem sık hem de seyrek erişim katmanları desteklenir. |
| Blob depolama hesabı | |

> [!IMPORTANT]
> Azure Blob depolamada ağ dosya sistemi (NFS) 3,0 protokol desteği Data Box Disk desteklenmez.

## <a name="supported-storage-types-for-upload"></a>Karşıya yükleme için desteklenen depolama türleri

Data Box Disk kullanılarak Azure’a yükleme için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Dosya biçimi** | **Notlar** |
| --- | --- |
| Azure Blok blobu | |
| Azure sayfa blobu  | |
| Azure Dosyaları  | |
| Yönetilen Diskler | |

::: zone target="docs"

## <a name="next-step"></a>Sonraki adım

* [Azure Data Box Diskinizi dağıtma](data-box-disk-deploy-ordered.md)

::: zone-end

