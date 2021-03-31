---
title: include dosyası
description: include dosyası
services: iot-hub
ms.service: iot-hub
author: robinsh
ms.topic: include
ms.date: 02/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 7f1f7d6f9ab6036fbcfcd1d19e175302bbd1a2a8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "87298789"
---
## <a name="customize-and-extend-the-device-management-actions"></a>Cihaz yönetim eylemlerini özelleştirme ve genişletme

IoT çözümlerinizi tanımlı cihaz yönetim desenleri kümesini genişletebilir veya cihaz ikizi ve buluttan cihaza yöntem temel sürümlerini kullanarak özel desenleri etkinleştirebilir. Bazı cihaz yönetim eylemlerine örnek olarak fabrika sıfırlaması, üretici yazılımı güncelleştirmesi, yazılım güncelleştirmesi, güç yönetimi, ağ ve bağlantı yönetimi ve veri şifreleme dahildir.

## <a name="device-maintenance-windows"></a>Cihaz bakımı pencereleri

Genellikle cihazları, kesintileri ve kapalı kalma süresini en aza indiren bir zamanda eylemleri gerçekleştirecek şekilde yapılandırırsınız. Cihaz bakımı pencereleri, bir cihazın yapılandırmasını güncelleştirmesi gereken zamanı tanımlamak için yaygın olarak kullanılan bir modeldir. Arka uç çözümleriniz, cihazınızda bir bakım penceresi sağlayan bir ilkeyi tanımlamak ve etkinleştirmek için Device ikizi 'ın istenen özelliklerini kullanabilir. Bir cihaz, bakım penceresi ilkesini aldığında, ilkenin durumunu raporlamak için Device ikizi 'ın bildirilen özelliğini kullanabilir. Arka uç uygulaması daha sonra cihaz ikizi sorgularını kullanarak cihazların ve her ilkenin uyumluluğuna karşı test edebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihazda Uzaktan yeniden başlatmanın tetiklenmesi için doğrudan bir yöntem kullandınız. Bildirilen özellikleri cihazdan en son yeniden başlatma zamanını raporlamak için kullandınız ve cihazın buluttan en son yeniden başlatma zamanını bulması için cihazı ikizi sorguladı.

AIR üretici yazılımı güncelleştirmesi üzerinden uzak gibi IoT Hub ve cihaz yönetim desenleriyle çalışmaya devam etmek için bkz. [nasıl yapılır bir bellenim güncelleştirmesi](../articles/iot-hub/tutorial-firmware-update.md).

IoT çözümünüzü genişletmeyi ve birden çok cihazda Yöntem çağrılarını zamanlamayı öğrenmek için bkz. [zamanlama ve yayınlama işleri](../articles/iot-hub/iot-hub-node-node-schedule-jobs.md).