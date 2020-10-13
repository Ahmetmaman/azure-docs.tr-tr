---
title: Uzaktan Izleme çözümü Kullanıcı arabirimine panel ekleme-Azure | Microsoft Docs
description: Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki panoya nasıl yeni bir panel ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.custom: devx-track-js
ms.openlocfilehash: 1dcca8409022ba4cf1f988b7c777e3a1fa511060
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91318373"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki panoya özel panel ekleme

Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki bir pano sayfasına nasıl yeni bir panel ekleyebileceğiniz gösterilmektedir. Makalede şunları açıklanmaktadır:

- Yerel bir geliştirme ortamı hazırlama.
- Web Kullanıcı arabirimindeki bir pano sayfasına yeni panel ekleme.

Bu makaledeki örnek panel, varolan Pano sayfasında görüntülenir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için, yerel geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce, [Uzaktan izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md) makalesindeki adımları tamamlamalısınız.

## <a name="add-a-panel"></a>Panel ekleme

Web Kullanıcı arabirimine bir panel eklemek için paneli tanımlayan kaynak dosyalarını eklemeniz ve sonra paneli görüntülemesi için panoyu değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-panel"></a>Paneli tanımlayan yeni dosyaları ekleyin

Başlangıç yapmanız için, **src/izlenecek yol/bileşenler/sayfalar/Pano/paneller/examplePanel** klasörü şunları içeren bir paneli tanımlayan dosyaları içerir:

**examplePanel.js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

Src/ **izlenecek yol/bileşenler/sayfalar/Pano/paneller/examplePanel** klasörünü **src/Components/Pages/Dashboard/paneller** klasörüne kopyalayın.

**Kaynak/izlenecek yol/bileşenler/sayfalar/Pano/paneller/index.js** dosyasına aşağıdaki dışarı aktarmayı ekleyin:

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>Paneli panoya ekleme

Paneli eklemek için **src/Components/Pages/Dashboard/dashboard.js** değiştirin.

Panellerden içeri aktarmalar listesine örnek paneli ekleyin:

```js
import {
  OverviewPanel,
  AlertsPanel,
  TelemetryPanel,
  AnalyticsPanel,
  MapPanel,
  transformTelemetryResponse,
  chartColorObjects,
  ExamplePanel
} from './panels';
```

Aşağıdaki hücre tanımını sayfa içeriğindeki kılavuza ekleyin:

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-flyout"></a>Açılır menüyü test etme

Web Kullanıcı arabirimi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut, Kullanıcı arabirimini ' de yerel olarak çalıştırır `http://localhost:3000/dashboard` . Yeni paneli görüntülemek için **Pano** sayfasına gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında Web Kullanıcı arabiriminde panolar eklemenize veya özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
