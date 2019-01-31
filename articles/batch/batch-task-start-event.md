---
title: Azure Batch görev başlangıç olayı | Microsoft Docs
description: Başvuru için Batch görev başlangıç olayı.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: d50a0a7082e409084fd966370934a638ca9bb013
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474437"
---
# <a name="task-start-event"></a>Görev başlangıç olayı

 Bu olay, bir görev bir işlem düğümünde başlatmak için Zamanlayıcı tarafından zamanlandıktan sonra yayınlanır. Görev yeniden deneme veya yeniden kuyruğa bu olay yeniden aynı görevi, ancak yeniden deneme sayısı için yayılan ve sistem görev sürümü buna göre güncelleştirilir unutmayın.


 Aşağıdaki örnek, bir görevin başlangıç olayı gösterir.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|iş kimliği|String|Görevi içeren işi kimliği.|
|id|String|Görevin kimliği.|
|taskType|String|Görev türü. Bu, 'bir iş yöneticisi görevi, belirten JobManager' olabilir veya bir iş yöneticisi görevi değil belirten'User ' olabilir.|
|systemTaskVersion|Int32|Bu görevde iç yeniden deneme sayacı toplanır. Dahili olarak Batch hizmeti hesap için geçici bir sorun için bir görevi yeniden deneyebilirsiniz. Bu sorunları iç zamanlama hatalar içerebilir veya girişimleri, kurtarılır işlem düğümleri hatalı durumda.|
|[nodeInfo](#nodeInfo)|Karmaşık Tür|İşlem düğümünde görevin çalıştırıldığı hakkındaki bilgileri içerir.|
|[multiInstanceSettings](#multiInstanceSettings)|Karmaşık Tür|Görev birden çok işlem düğümü gerektiren çok örnekli görev olduğunu belirtir.  Bkz: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) Ayrıntılar için.|
|[Kısıtlamaları](#constraints)|Karmaşık Tür|Bu görevin geçerli yürütme kısıtlamaları.|
|[executionInfo](#executionInfo)|Karmaşık Tür|Görevin yürütülmesi hakkında bilgi içerir.|

###  <a name="nodeInfo"></a> nodeInfo

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|poolId|String|Görevin çalıştırıldığı Havuz kimliği.|
|NodeId|String|Görevin çalıştırıldığı düğümün kimliği.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|numberOfInstances|Int|Görevin gerektirdiği işlem düğüm sayısı.|

###  <a name="constraints"></a> Kısıtlamaları

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Maksimum görev yeniden deneme sayısı. Çıkış kodu sıfır değilse toplu işlem hizmeti görevi yeniden dener.<br /><br /> Bu değer özel yeniden deneme sayısını kontrol edin. Batch hizmeti görevi bir kez dener ve sonra bu sınıra kadar yeniden deneyebilir. Örneğin, en fazla yeniden deneme sayısı 3, Batch çalışır bir görevin en fazla ise 4 (bir ilk deneme ve 3 yeniden deneme) zaman.<br /><br /> En fazla yeniden deneme sayısının 0 olması durumunda toplu işlem hizmeti görevleri yeniden denemez.<br /><br /> En fazla yeniden deneme sayısı -1 olması durumunda, Batch hizmeti görevleri sınır olmaksızın yeniden dener.<br /><br /> Varsayılan değer 0 (yeniden deneme yok) ' dir.|

###  <a name="executionInfo"></a> executionInfo

|Öğe adı|Type|Notlar|
|------------------|----------|-----------|
|RetryCount|Int32|Görev Batch hizmeti tarafından yeniden deneme sayısı. Belirtilen MaxTaskRetryCount kadar bir sıfır olmayan çıkış kodu ile çıkılıyorsa görevi yeniden denenir|
