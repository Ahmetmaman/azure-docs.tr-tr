---
title: LogDownloader - özel karar alma hizmeti
titlesuffix: Azure Cognitive Services
description: Azure Custom Decision Service tarafından üretilen günlük dosyalarını indirin.
services: cognitive-services
author: marco-rossi29
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: 8c5ab0e297690f1fbdb41a2627dd63c3ea522d1b
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366822"
---
# <a name="logdownloader"></a>LogDownloader

Azure Custom Decision Service tarafından üretilen ve oluşturacak günlük dosyalarını indirin *.gz* deneme tarafından kullanılan dosyalar.

## <a name="prerequisites"></a>Önkoşullar

- Python 3: Yüklü ve yolunuzu. Büyük dosyaları işlemek için 64 bit sürümü öneririz.
- *Mwt/Microsoft-ds* depo: [depoyu Kopyalayalım](https://github.com/Microsoft/mwt-ds).
- *Azure depolama blobu* paket: ilgili yükleme ayrıntılarını, Git [Python için Microsoft Azure depolama Kitaplığı](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Azure depolama bağlantı dizenizi girin *mwt-ds/DataScience/ds.config*: izleyin *my_app_id: my_connectionString* şablonu. Birden çok belirtebilirsiniz `app_id`. Çalıştırdığınızda `LogDownloader.py`, giriş `app_id` içinde bulunamadı `ds.config`, `LogDownloader.py` kullanan `$Default` bağlantı dizesi.

## <a name="usage"></a>Kullanım

Git `mwt-ds/DataScience` çalıştırıp `LogDownloader.py` aşağıdaki kodda ayrıntılı olarak ilgili bağımsız değişkenleriyle:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parametreler

| Girdi | Açıklama | Varsayılan |
| --- | --- | --- |
| `-h`, `--help` | Çıkış ve yardım iletisini gösterir. | |
| `-a APP_ID`, `--app_id APP_ID` | Uygulama Kimliği (diğer bir deyişle, Azure depolama blobu kapsayıcısının adı). | Gerekli |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | (Bir alt klasör oluşturulduğunda) veri yükleme için taban dizini.  | Gerekli |
| `-s START_DATE`, `--start_date START_DATE` | Başlangıç tarihi (dahil), buna indirme *YYYY-AA-GG* biçimi. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | (Dahil) yükleme son tarihi *YYYY-AA-GG* biçimi. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | Kullanılacak üzerine yazma modu. | |
| | `0`: Hiçbir zaman üzerine; BLOB'ları şu anda kullanılmakta olan kullanıcı isteyin. | Varsayılan | |
| | `1`: Kullanıcının dosyaları farklı boyutlarda veya BLOB'ları şu anda kullanılmakta olan devam etmek nasıl isteyin. | |
| | `2`: Her zaman üzerine; şu anda kullanılan blobları indirin. | |
| | `3`: Hiçbir zaman üzerine ve boyutu büyükse, istemeden ekleme; şu anda kullanılan blobları indirin. | |
| | `4`: Hiçbir zaman üzerine ve boyutu büyükse, istemeden ekleme; şu anda kullanılan BLOB'ları atlayın. | |
| `--dry_run` | Yazdırma hangi BLOB'ları, indirmeden indirilip. | `False` |
| `--create_gzip` | Oluşturma bir *gzip* Vowpal Wabbit dosyası. | `False` |
| `--delta_mod_t DELTA_MOD_T` | Bir dosya şu anda kullanımda olup olmadığını saptamak için saniye cinsinden zaman penceresi. | `3600` saniye (`1` saat) |
| `--verbose` | Daha fazla ayrıntı yazdırın. | `False` |
| `-v VERSION`, `--version VERSION` | Kullanılacak günlük yükleyici sürümü. | |
| | `1`: Uncooked günlüklerini (yalnızca geriye dönük uyumluluk için). | Kullanım Dışı |
| | `2`: İşlenmiş günlükler için. | Varsayılan |

### <a name="examples"></a>Örnekler

Bir Azure depolama blob kapsayıcınızdaki tüm veri indirme, Prova için aşağıdaki kodu kullanın:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

1 Ocak 2018'den ile oluşturulan günlükleri indirmek için `overwrite_mode=4`, aşağıdaki kodu kullanın:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Oluşturmak için bir *gzip* dosya indirilen tüm dosyaları birleştirme, aşağıdaki kodu kullanın:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```