---
title: API protokollerini Yapılandırma-Azure Event Grid IoT Edge | Microsoft Docs
description: Event Grid modülünün olası protokol konfigürasyonları hakkında bilgi edinin.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: a9cf6088201ffeaed76d99a9b211e5bcd1ea139a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91322589"
---
# <a name="configure-event-grid-api-protocols"></a>Event Grid API protokollerini yapılandırma

Bu kılavuzda, bir Event Grid modülünün olası protokol yapılandırmalarının örnekleri verilmiştir. Event Grid modülü, yönetim ve çalışma zamanı işlemleri için API 'YI kullanıma sunar. Aşağıdaki tablo, protokolleri ve bağlantı noktalarını yakalar.

| Protokol | Bağlantı noktası | Description |
| ---------------- | ------------ | ------------ |
| HTTP | 5888 | Varsayılan olarak kapalıdır. Yalnızca sınama sırasında yararlı olur. Üretim iş yükleri için uygun değildir.
| HTTPS | 4438 | Varsayılan

Tüm olası yapılandırmalara yönelik [güvenlik ve kimlik doğrulama Kılavuzu '](security-authentication.md) na bakın.

## <a name="expose-https-to-iot-modules-on-the-same-edge-network"></a>Aynı kenar ağında HTTPS 'yi IoT modüllerine kullanıma sunma

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-https-to-other-iot-modules-and-non-iot-workloads"></a>Diğer IoT modüllerine ve IoT olmayan iş yüklerine HTTPS 'yi etkinleştirme

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> **Portbindings** bölümü, iç bağlantı noktalarını kapsayıcı konağın bağlantı noktalarıyla eşlemenizi sağlar. Bu özellik, IoT Edge cihazının herkese açık olması halinde IoT Edge kapsayıcı ağı dışından Event Grid modülüne ulaşmak mümkün hale gelir.

## <a name="expose-http-and-https-to-iot-modules-on-the-same-edge-network"></a>Aynı uç ağdaki IoT modüllerine HTTP ve HTTPS 'yi kullanıma sunma

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-http-and-https-to-other-iot-modules-and-non-iot-workloads"></a>Diğer IoT modüllerine ve IoT olmayan iş yüklerine HTTP ve HTTPS 'yi etkinleştirme

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ],
      "5888/tcp": [
        {
          "HostPort": "5888"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Varsayılan olarak, her IoT modülü köprü ağı tarafından oluşturulan IoT Edge çalışma zamanının bir parçasıdır. Aynı ağ üzerinde bulunan farklı IoT modüllerinin birbirleriyle iletişim kurmasını sağlar. **Portbindings** , bir kapsayıcı iç bağlantı noktasını ana makine üzerinde eşleştirmenize olanak tanıyarak, herkesin Event Grid modülünün bağlantı noktasına dışarıdan erişebilmesini sağlar.

>[!IMPORTANT]
> Bağlantı noktaları IoT Edge ağı dışında erişilebilir hale getirilmese de, istemci kimlik doğrulaması, gerçekte modüle çağrı yapmasına izin verilen kişileri zorlar.
