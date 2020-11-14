---
title: Azure Machine Learning işlem hatlarını zamanlayın
titleSuffix: Azure Machine Learning
description: Zamanlanan işlem hatları, veri işleme, eğitim ve izleme gibi rutin, zaman alan görevleri otomatikleştirmenizi sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
ms.date: 11/12/2019
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 2c931aa6cd170c72654eb9ced6813d26e21ce5d0
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2020
ms.locfileid: "94630320"
---
# <a name="schedule-machine-learning-pipelines-with-azure-machine-learning-sdk-for-python"></a>Python için Azure Machine Learning SDK ile makine öğrenimi işlem hatlarını zamanlayın

Bu makalede, Azure 'da çalışması için programlı olarak bir işlem hattı zamanlamayı öğreneceksiniz. Geçen süreye veya dosya sistemi değişikliklerine göre bir zamanlama oluşturmayı tercih edebilirsiniz. Zaman tabanlı zamanlamalar, veri kayması için izleme gibi rutin görevlerden yararlanmak için kullanılabilir. Değişiklik tabanlı zamanlamalar, karşıya yüklenen yeni veriler veya düzenlenmekte olan eski veriler gibi düzensiz veya öngörülemeyen değişikliklere tepki vermek için kullanılabilir. Zamanlama oluşturmayı öğrendikten sonra, bunların nasıl alınacağını ve devre dışı bırakılacağı hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree)oluşturun.

* Python için Azure Machine Learning SDK 'nın yüklendiği bir Python ortamı. Daha fazla bilgi için bkz [. eğitim ve dağıtım için yeniden kullanılabilir ortamları oluşturma ve yönetme Azure Machine Learning.](how-to-use-environments.md)

* Yayımlanmış bir işlem hattına sahip Machine Learning çalışma alanı. [Azure MACHINE LEARNING SDK ile makine öğrenimi işlem hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)sırasında yerleşik bir yapılandırma kullanabilirsiniz.

## <a name="initialize-the-workspace--get-data"></a>Veri Al & çalışma alanını başlatma

Bir işlem hattını zamanlamak için çalışma alanınıza bir başvuru, yayımlanan işlem hattının tanımlayıcısı ve zamanlamayı oluşturmak istediğiniz denemenin adı gerekir. Aşağıdaki kodla bu değerleri alabilirsiniz:

```Python
import azureml.core
from azureml.core import Workspace
from azureml.pipeline.core import Pipeline, PublishedPipeline
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

experiments = Experiment.list(ws)
for experiment in experiments:
    print(experiment.name)

published_pipelines = PublishedPipeline.list(ws)
for published_pipeline in  published_pipelines:
    print(f"{published_pipeline.name},'{published_pipeline.id}'")

experiment_name = "MyExperiment" 
pipeline_id = "aaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" 
```

## <a name="create-a-schedule"></a>Zamanlama Oluştur

Bir işlem hattını yineleme temelinde çalıştırmak için bir zamanlama oluşturacaksınız. Bir işlem hattını `Schedule` , denemeyi ve bir tetikleyiciyi ilişkilendirir. Tetikleyici, `ScheduleRecurrence` çalışma veya değişiklik için izlenecek bir dizin belirten bir veri deposu yolu arasındaki beklemeyi açıklayan bir olabilir. Her iki durumda da, işlem hattı tanımlayıcısı ve zamanlamanın oluşturulacağı denemenin adı gerekir.

Python dosyanızın en üstünde, `Schedule` ve sınıflarını içeri aktarın `ScheduleRecurrence` :

```python

from azureml.pipeline.core.schedule import ScheduleRecurrence, Schedule
```

### <a name="create-a-time-based-schedule"></a>Zamana dayalı zamanlama oluşturma

`ScheduleRecurrence`Oluşturucunun, `frequency` Şu dizelerden biri olması gereken bir bağımsız değişkeni vardır: "Minute", "Hour", "Day", "Week" veya "month". Ayrıca `interval` , zamanlamanın ne kadar süre sonra geçmesi gerektiğini belirten bir tamsayı bağımsız değişkeni gerektirir `frequency` . İsteğe bağlı bağımsız değişkenler, [SCHEDULERECURRENCE SDK belgeleri](/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedulerecurrence?preserve-view=true&view=azure-ml-py)içinde açıklandığı gibi başlangıç zamanları hakkında daha fazla bilgi sağlar.

`Schedule`Her 15 dakikada bir çalışmayı başlatan bir oluştur:

```python
recurrence = ScheduleRecurrence(frequency="Minute", interval=15)
recurring_schedule = Schedule.create(ws, name="MyRecurringSchedule", 
                            description="Based on time",
                            pipeline_id=pipeline_id, 
                            experiment_name=experiment_name, 
                            recurrence=recurrence)
```

### <a name="create-a-change-based-schedule"></a>Değişiklik tabanlı zamanlama oluşturma

Dosya değişiklikleri tarafından tetiklenen işlem hatları, zaman tabanlı zamanlamalardan daha verimli olabilir. Örneğin, bir dosya değiştirildiğinde veya bir veri dizinine yeni bir dosya eklendiğinde ön işleme adımı yapmak isteyebilirsiniz. Veri deposundaki herhangi bir değişikliği veya veri deposu içindeki belirli bir dizin içindeki değişiklikleri izleyebilirsiniz. Belirli bir dizini izlemenize sonra, bu dizinin alt dizinlerindeki değişiklikler bir çalıştırmayı _tetiklemez_ .

Bir dosya-reaktif oluşturmak için `Schedule` , `datastore` çağır [. Create](/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedule?preserve-view=true&view=azure-ml-py#&preserve-view=truecreate-workspace--name--pipeline-id--experiment-name--recurrence-none--description-none--pipeline-parameters-none--wait-for-provisioning-false--wait-timeout-3600--datastore-none--polling-interval-5--data-path-parameter-name-none--continue-on-step-failure-none--path-on-datastore-none---workflow-provider-none---service-endpoint-none-)çağrısında parametresini ayarlamanız gerekir. Bir klasörü izlemek için `path_on_datastore` bağımsız değişkenini ayarlayın.

`polling_interval`Bağımsız değişkeni, veri deposunun değişiklikler için denetlenme sıklığını dakika cinsinden belirtmenize olanak tanır.

İşlem hattı bir [DataPath](/python/api/azureml-core/azureml.data.datapath.datapath?preserve-view=true&view=azure-ml-py) [pipelineparametresiyle](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?preserve-view=true&view=azure-ml-py)oluşturulmuşsa, bağımsız değişkenini ayarlayarak bu değişkeni değiştirilen dosyanın adı olarak ayarlayabilirsiniz `data_path_parameter_name` .

```python
datastore = Datastore(workspace=ws, name="workspaceblobstore")

reactive_schedule = Schedule.create(ws, name="MyReactiveSchedule", description="Based on input file change.",
                            pipeline_id=pipeline_id, experiment_name=experiment_name, datastore=datastore, data_path_parameter_name="input_data")
```

### <a name="optional-arguments-when-creating-a-schedule"></a>Zamanlama oluştururken isteğe bağlı bağımsız değişkenler

Daha önce ele alınan bağımsız değişkenlere ek olarak, `status` bağımsız değişkenini `"Disabled"` devre dışı bir zamanlama oluşturacak şekilde ayarlayabilirsiniz. Son olarak, `continue_on_step_failure` işlem hattının varsayılan hata davranışını geçersiz kılacak bir Boole değeri geçirmenize olanak sağlar.

### <a name="use-azure-logic-apps-for-more-complex-workflows"></a>Daha karmaşık iş akışları için Azure Logic Apps kullanın

Azure Logic Apps daha karmaşık iş akışlarını destekler ve Azure Machine Learning işlem hatlarından çok daha geniş ölçüde tümleşiktir. Daha fazla bilgi için bkz. [mantıksal uygulamadan Machine Learning işlem hattının çalıştırılmasını tetikleme](how-to-trigger-published-pipeline.md) .

## <a name="view-your-scheduled-pipelines"></a>Zamanlanan işlem hatlarınızı görüntüleyin

Web tarayıcınızda Azure Machine Learning ' a gidin. Gezinti bölmesinin **uç noktalar** bölümünde **ardışık düzen uç noktaları** ' nı seçin. Bu sizi çalışma alanında yayınlanan işlem hatları listesine götürür.

![AML ardışık düzenleri sayfası](./media/how-to-schedule-pipelines/scheduled-pipelines.png)

Bu sayfada, çalışma alanındaki tüm işlem hatları hakkında özet bilgileri görebilirsiniz: adlar, açıklamalar, durum ve benzeri. İşlem hattınızı tıklatarak detaya gidin. Sonuç sayfasında, işlem hatınızla ilgili daha fazla ayrıntı vardır ve tek tek çalıştırmalar üzerinde ayrıntıya gidebilirsiniz.

## <a name="deactivate-the-pipeline"></a>İşlem hattını devre dışı bırakma

`Pipeline`Yayımlanmış ancak zamanlanmamış bir ' a sahipseniz, şu ile devre dışı bırakabilirsiniz:

```python
pipeline = PublishedPipeline.get(ws, id=pipeline_id)
pipeline.disable()
```

İşlem hattı zamanlanırsa, önce zamanlamayı iptal etmeniz gerekir. Zamanlamadan veya çalıştırarak zamanlamanın tanımlayıcısını alın:

```python
ss = Schedule.list(ws)
for s in ss:
    print(s)
```

`schedule_id`Devre dışı bırakmak istediğiniz bir kez daha çalıştırın:

```python
def stop_by_schedule_id(ws, schedule_id):
    s = next(s for s in Schedule.list(ws) if s.id == schedule_id)
    s.disable()
    return s

stop_by_schedule_id(ws, schedule_id)
```

Daha sonra yeniden çalıştırırsanız `Schedule.list(ws)` boş bir liste almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir işlem hattını iki farklı şekilde zamanlamak için Python için Azure Machine Learning SDK 'sını kullandınız. Bir zamanlama geçen saat zamanına göre yinelenir. Diğer zamanlama, belirtilen bir dosya `Datastore` veya bu depodaki bir dizin içinde değiştirilirse çalışır. İşlem hattını ve bireysel çalıştırmaları incelemek için portalın nasıl kullanılacağını gördünüz. Son olarak, işlem hattının çalışmayı durdurması için zamanlamayı nasıl devre dışı bırakakullanacağınızı öğrendiniz.

Daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Batch Puanlama için Azure Machine Learning işlem hatları kullanma](tutorial-pipeline-batch-scoring-classification.md)

* İşlem [hatları](concept-ml-pipelines.md) hakkında daha fazla bilgi
* [Jupyıter ile Azure Machine Learning araştırma](samples-notebooks.md) hakkında daha fazla bilgi edinin