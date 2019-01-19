---
title: Azure içeri/dışarı aktarma işleri için tanılama ve hata kurtarma | Microsoft Docs
description: Ayrıntılı günlük kaydı için Microsoft Azure içeri/dışarı aktarma hizmeti işleri özelliğini etkinleştirmeyi öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 0d58a384a6ca7c249a3b0e8b690095202fe373a2
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413626"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri için tanılama ve hata kurtarma
İşlenen her bir sürücü için Azure içeri/dışarı aktarma hizmeti ilişkili depolama hesabında bir hata günlüğü oluşturur. Ayarlayarak ayrıntılı günlük kaydını etkinleştirebilirsiniz `LogLevel` özelliğini `Verbose` çağırırken [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) veya [güncelleştirme işi özellikleri](/rest/api/storageimportexport/jobs#Jobs_Update) operations.

 Varsayılan olarak, adlı bir kapsayıcı için günlükleri yazılır `waimportexport`. Farklı bir ad ayarlayarak belirtebilirsiniz `DiagnosticsPath` çağrılırken özellik `Put Job` veya `Update Job Properties` operations. Günlükleri aşağıdaki adlandırma kuralıyla blok blobları olarak depolanır: `waies/jobname_driveid_timestamp_logtype.xml`.

 Bir işi için kayıtlar URI'sini çağırarak alabilirsiniz [alma işi](/rest/api/storageimportexport/jobs) işlemi. Ayrıntılı günlük URI'sini döndürülür `VerboseLogUri` özelliği için hata günlüğünü URI döndürülür ancak her sürücü için `ErrorLogUri` özelliği.

Aşağıdaki sorunları belirlemek için günlük verilerini kullanabilirsiniz.

## <a name="drive-errors"></a>Sürücü hataları

Aşağıdaki öğeler sürücüsü hataları sınıflandırılan:

-   Uygulama ayarlarına erişme veya bildirim dosyasını okuma hataları

-   Yanlış BitLocker anahtarları

-   Sürücü okuma/yazma hataları

## <a name="blob-errors"></a>BLOB hataları

Aşağıdaki öğeler, blob hataları olarak sınıflandırılan:

-   Yanlış veya çakışan blob veya adları

-   Eksik dosyaları

-   Blob bulunamadı

-   Kesilmiş dosyaları (disk üzerinde dosyaların bildiriminde belirtilenden daha küçük)

-   Bozuk dosya içeriğini (için ile bir MD5 sağlama toplamı eşleşmezliği algıladı, içeri aktarma işlerinde)

-   Bozuk blob meta verileri ve özellik dosyalarını (ile bir MD5 sağlama toplamı eşleşmezliği algıladı)

-   Blob özelliklerini ve/veya meta veri dosyaları için yanlış şema

Genel İş hala tamamlanırken burada bazı bölümlerini bir içeri veya dışarı aktarma işi başarıyla tamamlanması olmayan durumlar olabilir. Bu durumda, karşıya yükleyin veya ağ üzerinden veri eksik parçalarını indirin veya veri aktarmak için yeni bir proje oluşturabilirsiniz. Bkz: [Azure içeri/dışarı aktarma aracı başvurusu](storage-import-export-tool-how-to-v1.md) verileri ağ üzerinden onarma hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
