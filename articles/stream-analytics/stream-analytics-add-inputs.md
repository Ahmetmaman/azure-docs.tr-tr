---
title: Azure Stream Analytics için girişleri anlayın
description: Bu makalede, akış girişini başvuru veri girişi ile karşılaştıran bir Azure Stream Analytics işinde giriş kavramı açıklanır.
ms.service: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 442c5a1174c4a91ea9401315bb3e518e4fe6cc4e
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102183922"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Azure Stream Analytics için girişleri anlayın

Azure Stream Analytics işleri bir veya daha fazla veri girişi 'ne bağlanır. Her giriş, var olan bir veri kaynağına yönelik bağlantıyı tanımlar. Stream Analytics, Event Hubs, IoT Hub ve BLOB depolama dahil olmak üzere çeşitli türlerdeki olay kaynaklarından gelen verileri kabul eder. Girişler, her iş için yazdığınız akış SQL sorgusunda adı tarafından başvurulur. Sorguda, verileri Blend veya başvuru verilerine arama ile akış verilerini karşılaştırmak ve sonuçları çıkışlara geçirmek için birden çok girişi birleştirebilirsiniz. 

Stream Analytics giriş olarak dört tür kaynakla birinci sınıf tümleştirmeye sahiptir:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 
- [Azure Data Lake Storage 2. Nesil](../storage/blobs/data-lake-storage-introduction.md) 

Bu giriş kaynakları, Stream Analytics işiniz veya farklı bir abonelikle aynı Azure aboneliğinde bulunabilir.

[Azure Portal](stream-analytics-quick-create-portal.md#configure-job-input), [Azure POWERSHELL](/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.NET API](/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST API](/rest/api/streamanalytics/2016-03-01/inputs)ve [Visual Studio 'yu](stream-analytics-tools-for-visual-studio-install.md) kullanarak Stream Analytics iş girişleri oluşturabilir, düzenleyebilir ve test edebilirsiniz.

## <a name="stream-and-reference-inputs"></a>Akış ve başvuru girişleri
Veriler bir veri kaynağına gönderildiği için Stream Analytics işi tarafından kullanılır ve gerçek zamanlı olarak işlenir. Girişler iki türe ayrılır: veri akışı girişleri ve başvuru verisi girişleri.

### <a name="data-stream-input"></a>Veri akışı girişi
Veri akışı zaman içinde sınırsız bir olay sırasıdır. Stream Analytics işlerinin en az bir veri akışı girişi içermesi gerekir. Event Hubs, IoT Hub, Azure Data Lake Storage 2. ve BLOB depolama alanı veri akışı giriş kaynakları olarak desteklenir. Event Hubs, birden fazla cihazdan ve hizmetten olay akışlarını toplamak için kullanılır. Bu akışlar sosyal medya etkinlik akışları, hisse senedi bilgileri veya sensörlerden alınan veriler içerebilir. IoT Hub 'Ları Nesnelerin İnterneti (IoT) senaryolarında bağlı cihazlardan veri toplamak için iyileştirilmiştir.  BLOB depolama, günlük dosyaları gibi bir akış olarak toplu verileri almak için bir giriş kaynağı olarak kullanılabilir.  

Akış veri girişleri hakkında daha fazla bilgi için bkz. [Stream Analytics giriş olarak akış verileri](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Başvuru veri girişi
Stream Analytics Ayrıca *başvuru verileri* olarak bilinen girişi destekler. Başvuru verileri tamamen statiktir ya da yavaş değişir. Genellikle bağıntı ve arama gerçekleştirmek için kullanılır. Örneğin, statik değerleri aramak için bir SQL JOIN gerçekleştirdiğinize çok benzer şekilde, veri akışı girişinde verileri başvuru verileri verilerine katabilirsiniz. Azure Blob depolama, Azure Data Lake Storage 2. ve Azure SQL veritabanı şu anda başvuru verileri için giriş kaynağı olarak desteklenmektedir. Sorgu karmaşıklığı ve ayrılan akış birimlerine bağlı olarak, başvuru veri kaynağı Blobları boyut olarak 300 MB 'a kadar bir sınıra sahiptir (daha fazla ayrıntı için başvuru verileri belgelerinin [boyut sınırlaması](stream-analytics-use-reference-data.md#size-limitation) bölümüne bakın).

Başvuru verileri girişleri hakkında daha fazla bilgi için bkz. [Stream Analytics aramalar için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)