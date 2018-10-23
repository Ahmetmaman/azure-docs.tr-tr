---
title: Azure Stream Analytics için girişler anlama
description: Bu makalede, başvuru veri girişi için akış girişi karşılaştıran bir Azure Stream Analytics işinde girişleri kavramını açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 219fe9e8f91f9b02e64b3b44ab4301f77820561f
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648964"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Azure Stream Analytics için girişler anlama

Azure Stream Analytics işleri için bir veya daha fazla veri girişlerinin bağlanın. Her bir giriş var olan bir veri kaynağı için bir bağlantı tanımlar. Stream Analytics, olay kaynaklarını olay hub'ları, IOT hub'ı ve Blob Depolama da dahil olmak üzere çeşitli tür gelen verileri kabul eder. Girişlerin her proje için yazdığınız akış SQL sorgusunda adıyla başvurulur. Sorgu, veri blend veya akış verilerini başvuru verileriyle bir arama ile karşılaştırmak için birden çok girişi birleştirme ve sonuçları çıktıları geçirin. 

Stream Analytics, girdi olarak birinci sınıf tümleştirme kaynakları üç tür vardır:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, aynı Azure aboneliğinde, Stream Analytics işi olarak ya da farklı bir abonelikten Canlı çalıştırabilirsiniz.

Kullanabileceğiniz [Azure portalında](stream-analytics-quick-create-portal.md#configure-input-to-the-job), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/New-AzureRmStreamAnalyticsInput), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), ve [Visual Studio](stream-analytics-tools-for-visual-studio-install.md)oluşturun, düzenleyin ve Stream Analytics işi girişleri test etmek için.

## <a name="stream-and-reference-inputs"></a>Stream ve başvuru girişleri
Verileri bir veri kaynağına gönderilir gibi Stream Analytics işi tarafından tüketilen ve gerçek zamanlı olarak işlenen. Girişler, iki tür olarak ayrılmıştır: veri akışı girişleri ve başvuru verisi girişleri.

### <a name="data-stream-input"></a>Giriş veri akışı
Bir veri akışı sınırsız olayları zaman içinde dizisidir. Stream Analytics işleri, en az bir veri akış girişi içermelidir. Event Hubs, IOT hub'ı ve Blob Depolama, veri akışı giriş kaynakları olarak desteklenir. Olay hub'ları birden çok cihaz ve hizmet, olay akışları toplayacak şekilde kullanılır. Bu akışları, sosyal medya Etkinlik Akışları, hisse senedi ticareti bilgileri veya sensörlerden alınan verilerin içerebilir. IOT hub, nesnelerin interneti (IOT) senaryolarında bağlı cihazlardan veri toplamak için iyileştirilmiştir.  BLOB Depolama, günlük dosyaları gibi bir akış olarak toplu veri almak için bir giriş kaynağı kullanılabilir.  

Veri akışı girişleri hakkında daha fazla bilgi için bkz. [Stream Analytics'e giriş olarak veri Stream](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Başvuru veri girişi
Stream Analytics de destekler olarak bilinen giriş *başvuru verileri*. Başvuru verileri olduğu ya da tamamen statik veya yavaş değişir. Genellikle, bağıntı ve aramalar gerçekleştirmek için kullanılır. Örneğin, statik değerleri aramak için bir SQL birleştirme gerçekleştirecek gibi başvuru verilerini veri akış girişine veri birleştirebilirsiniz. Azure Blob Depolama şu anda yalnızca desteklenen giriş başvuru veri kaynağıdır. Başvuru veri kaynağı BLOB boyutu, sorgu karmaşıklığına bağlı olarak bir sınırı 300 MB olan ve ayrılan akış birimi.

Başvuru verisi girişleri hakkında daha fazla bilgi için bkz: [Stream analytics'te aramaları için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)

Bu makalede bir adımdır [Stream Analytics öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/).

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
