---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: ebc23ce4238c736442fbc4507e858876f9192fd9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "76021106"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Bir Azure Depolama hesabını IoT Hub ilişkilendir

Sanal cihaz uygulaması bir blob 'a dosya yüklediğinde, IoT Hub 'ınız ile ilişkili bir [Azure depolama](../articles/storage/common/storage-account-create.md) hesabınızın olması gerekir. Bir Azure Depolama hesabını IoT Hub ile ilişkilendirdiğinizde, IoT Hub 'ı bir SAS URI 'SI oluşturur. Bir cihaz, bir dosyayı bir blob kapsayıcısına güvenli bir şekilde yüklemek için bu SAS URI 'sini kullanabilir. IoT Hub hizmeti ve cihaz SDK 'ları, SAS URI 'sini oluşturan işlemi koordine edin ve bir dosyayı karşıya yüklemek için kullanılacak bir cihaz için kullanılabilir hale getirir.

[Azure Portal kullanarak dosya karşıya yüklemelerini yapılandırma](../articles/iot-hub/iot-hub-configure-file-upload.md)bölümündeki yönergeleri izleyin. Bir blob kapsayıcısının IoT Hub 'ınız ile ilişkilendirildiğinden ve dosya bildirimlerinin etkinleştirildiğinden emin olun.

![Portalda dosya bildirimlerini etkinleştir](./media/iot-hub-associate-storage/file-notifications-vs2019.png)
