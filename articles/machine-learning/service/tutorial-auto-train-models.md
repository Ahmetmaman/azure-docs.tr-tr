---
title: 'Regresyon modeli öğretici: Otomatik ML'
titleSuffix: Azure Machine Learning service
description: Otomatik machine learning kullanarak machine learning modeli oluşturmayı öğrenin. Azure Machine Learning veri ön işleme, algoritma seçimi ve hiper parametre seçimi otomatik bir şekilde sizin için gerçekleştirebilirsiniz. Daha sonra son modelin Azure Machine Learning hizmeti ile dağıtılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 419e2cd8444d391c15c55c60c1cf7e086470b848
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55730987"
---
# <a name="tutorial-use-automated-machine-learning-to-build-your-regression-model"></a>Öğretici: Otomatik makine öğrenimi, regresyon modeli derler

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. Önceki öğreticide, [NYC taksi verileri regresyon modelleme için hazırlanmış](tutorial-data-prep.md).

Artık Azure Machine Learning hizmeti ile modelinizi oluşturmaya başlamak hazırsınız. Öğreticinin bu bölümünde, hazırlanan verileri kullanabilir ve otomatik olarak taksi taksi fiyatlarını tahmin etmek için regresyon modeli oluşturun. Otomatik makine öğrenimi özellikleri hizmeti kullanarak, makine öğrenimi hedefleri ve kısıtlamaları tanımlar. Öğrenme sürecinde otomatik makine başlatın. Ardından algoritması seçme ve sizin için gerçekleştirilecek ayarlama hiper parametre izin verin. En iyi modeli, bu ölçütü temel alan bulana kadar otomatik makine teknik öğrenme algoritmaları ve hiperparametreleri çok sayıda yinelenir.

![Akış Diyagramı](./media/tutorial-auto-train-models/flow2.png)

Bu öğreticide, aşağıdaki görevleri öğrenin:

> [!div class="checklist"]
> * Bir Python ortamını ayarlama ve SDK paketlerini içeri aktarın.
> * Bir Azure Machine Learning hizmeti çalışma alanında yapılandırın.
> * Autotrain bir regresyon modeli.
> * Modeli yerel olarak özel parametrelerle çalıştırın.
> * Sonuçları inceleyin.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

>[!NOTE]
> Bu makalede kod, Azure Machine Learning SDK sürüm 1.0.0 ile test edilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Atlamak [geliştirme ortamınızı ayarlama](#start) not defteri adımları okuyun veya not defterini alma ve Azure not defterleri veya kendi notebook sunucusu üzerinde çalıştırmak için aşağıdaki yönergeleri kullanın. İhtiyacınız olacak not defteri çalıştırmak için:

* [Veri hazırlama öğreticisini çalıştırma](tutorial-data-prep.md).
* Aşağıdakilerin yüklü olan bir Python 3.6 Not Defteri sunucusu:
    * İle Python için Azure Machine Learning SDK'sı `automl` ve `notebooks` ek özellikler
    * `matplotlib`
* Öğretici not defteri
* Bir machine learning çalışma alanı
* Not Defteri ile aynı dizinde çalışma alanı için yapılandırma dosyası

Bu Önkoşullar aşağıdaki bölümlerde birinden alın.

* Kullanım [Azure Not Defterleri](#azure)
* Kullanım [kendi not defteri sunucusu](#server)

### <a name="azure"></a>Azure not defterleri kullanın: Bulutta ücretsiz Jupyter Not Defterleri

Azure not defterleri ile başlamak kolaydır! [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) zaten yüklü olan ve sizin için yapılandırılmış [Azure not defterleri](https://notebooks.azure.com/). Yükleme ve gelecekteki güncelleştirmelerin otomatik olarak Azure Hizmetleri yönetilir.

Aşağıdaki adımları tamamladıktan sonra Çalıştır **öğreticiler/regresyon-bölüm 2-otomatik-ml.ipynb** Not Defteri, **Başlarken** proje.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

### <a name="server"></a>Kendi Jupyter notebook sunucusu kullanma

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.  Adımları tamamladıktan sonra Çalıştır **öğreticiler/regresyon-bölüm 2-otomatik-ml.ipynb** dizüstü bilgisayar.

1. Tamamlamak [Azure Machine Learning Python hızlı](quickstart-create-workspace-with-python.md) Miniconda ortam oluşturma ve bir çalışma alanı oluşturun.
1. Yükleme `automl` ve `notebooks` ortamı kullanarak ek özellikler `pip install azureml-sdk[automl,notebooks]`.
1. Yükleme `maplotlib` kullanarak `pip install maplotlib`.
1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook

## <a name="start"></a>Set up your development environment

All the setup for your development work can be accomplished in a Python notebook. Setup includes the following actions:

* Install the SDK
* Import Python packages
* Configure your workspace

### Install and import packages

If you are following the tutorial in your own Python environment, use the following to install necessary packages.

```shell
pip install azureml-sdk[automl,notebooks] matplotlib
```

Bu öğreticide gerekli olacak Python paketlerini içeri aktarın:

```python
import azureml.core
import pandas as pd
from azureml.core.workspace import Workspace
from azureml.train.automl.run import AutoMLRun
import time
import logging
import os
```

### <a name="configure-workspace"></a>Çalışma alanını yapılandırma

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. A `Workspace` Azure abonelik ve kaynak bilgilerini kabul eden bir sınıftır. Ayrıca, model çalıştırmalarınızı izlemeye yarayan bir bulut kaynağı oluşturur.

`Workspace.from_config()`, **aml_config/config.json** dosyasını okur ve ayrıntıları `ws` adlı nesneye yükler.  Bu öğreticideki kodun kalanında `ws` kullanılır.

Bir çalışma nesnesi oluşturduktan sonra deneme için bir ad belirtin. Oluşturun ve çalışma alanı ile yerel bir dizine kaydedin. Tüm çalıştırmalar geçmişini belirtilen deneme altında ve kaydedilen [Azure portalında](https://portal.azure.com).


```python
ws = Workspace.from_config()
# choose a name for the run history container in the workspace
experiment_name = 'automated-ml-regression'
# project folder
project_folder = './automated-ml-regression'

output = {}
output['SDK version'] = azureml.core.VERSION
output['Subscription ID'] = ws.subscription_id
output['Workspace'] = ws.name
output['Resource Group'] = ws.resource_group
output['Location'] = ws.location
output['Project Directory'] = project_folder
pd.set_option('display.max_colwidth', -1)
pd.DataFrame(data=output, index=['']).T
```

## <a name="explore-data"></a>Verileri inceleme

Önceki öğreticide oluşturduğunuz veri akış nesnesi kullanın. Özetlemek gerekirse, bu öğreticinin 1 bölümüne bir machine learning modeli kullanılabilecek şekilde NYC taksi verileri temizlendi. Şimdi, veri kümesinde çeşitli özelliklerini kullanmak ve özellikler ve taksi seyahat fiyatı arasındaki ilişkileri oluşturmak otomatik bir modeli izin. Açın ve veri akışı çalıştırma ve sonuçları gözden geçirin:


```python
import azureml.dataprep as dprep

file_path = os.path.join(os.getcwd(), "dflows.dprep")

package_saved = dprep.Package.open(file_path)
dflow_prepared = package_saved.dataflows[0]
dflow_prepared.get_profile()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min</th>
      <th>Maks</th>
      <th>Sayı</th>
      <th>Eksik sayısı</th>
      <th>Sayısı eksik</th>
      <th>Eksik yüzde</th>
      <th>Hata sayısı</th>
      <th>Boş sayısı</th>
      <th>%0,1 quantile</th>
      <th>%1 quantile</th>
      <th>%5 quantile</th>
      <th>% 25 quantile</th>
      <th>% 50 quantile</th>
      <th>% 75 quantile</th>
      <th>% 95 quantile</th>
      <th>% 99 quantile</th>
      <th>%99,9 quantile</th>
      <th>Ortalama</th>
      <th>Standart sapma</th>
      <th>Varyans</th>
      <th>Komutunu</th>
      <th>Basıklık</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Satıcı</th>
      <td>FieldType.STRING</td>
      <td>1</td>
      <td>VTS</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_weekday</th>
      <td>FieldType.STRING</td>
      <td>Cuma</td>
      <td>Çarşamba</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>2.90047</td>
      <td>2.69355</td>
      <td>9.72889</td>
      <td>16</td>
      <td>19.3713</td>
      <td>22.6974</td>
      <td>23</td>
      <td>23</td>
      <td>14.2731</td>
      <td>6.59242</td>
      <td>43.46</td>
      <td>-0.693723</td>
      <td>-0.570403</td>
    </tr>
    <tr>
      <th>pickup_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>4.99701</td>
      <td>4.95833</td>
      <td>14.1528</td>
      <td>29.3832</td>
      <td>44.6825</td>
      <td>56.4444</td>
      <td>58.9909</td>
      <td>59</td>
      <td>29.427</td>
      <td>17.4333</td>
      <td>303.921</td>
      <td>0.0120999</td>
      <td>-1.20981</td>
    </tr>
    <tr>
      <th>pickup_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.28131</td>
      <td>5</td>
      <td>14.7832</td>
      <td>29.9293</td>
      <td>44.725</td>
      <td>56.7573</td>
      <td>59</td>
      <td>59</td>
      <td>29.7443</td>
      <td>17.3595</td>
      <td>301.351</td>
      <td>-0.0252399</td>
      <td>-1.19616</td>
    </tr>
    <tr>
      <th>dropoff_weekday</th>
      <td>FieldType.STRING</td>
      <td>Cuma</td>
      <td>Çarşamba</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>dropoff_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>2.57153</td>
      <td>2</td>
      <td>9.58795</td>
      <td>15.9994</td>
      <td>19.6184</td>
      <td>22.8317</td>
      <td>23</td>
      <td>23</td>
      <td>14.2105</td>
      <td>6.71093</td>
      <td>45.0365</td>
      <td>-0.687292</td>
      <td>-0.61951</td>
    </tr>
    <tr>
      <th>dropoff_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.44383</td>
      <td>4.84694</td>
      <td>14.1036</td>
      <td>28.8365</td>
      <td>44.3102</td>
      <td>56.6892</td>
      <td>59</td>
      <td>59</td>
      <td>29.2907</td>
      <td>17.4108</td>
      <td>303.136</td>
      <td>0.0222514</td>
      <td>-1.2181</td>
    </tr>
    <tr>
      <th>dropoff_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.07801</td>
      <td>5</td>
      <td>14.5751</td>
      <td>29.5972</td>
      <td>45.4649</td>
      <td>56.2729</td>
      <td>59</td>
      <td>59</td>
      <td>29.772</td>
      <td>17.5337</td>
      <td>307.429</td>
      <td>-0.0212575</td>
      <td>-1.226</td>
    </tr>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>E</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0781</td>
      <td>-73.7459</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.0578</td>
      <td>-73.9639</td>
      <td>-73.9656</td>
      <td>-73.9508</td>
      <td>-73.9255</td>
      <td>-73.8529</td>
      <td>-73.8302</td>
      <td>-73.8238</td>
      <td>-73.7697</td>
      <td>-73.9123</td>
      <td>0.0503757</td>
      <td>0.00253771</td>
      <td>0.352172</td>
      <td>-0.923743</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5755</td>
      <td>40.8799</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.632</td>
      <td>40.7117</td>
      <td>40.7115</td>
      <td>40.7213</td>
      <td>40.7565</td>
      <td>40.8058</td>
      <td>40.8478</td>
      <td>40.8676</td>
      <td>40.8778</td>
      <td>40.7649</td>
      <td>0.0494674</td>
      <td>0.00244702</td>
      <td>0.205972</td>
      <td>-0.777945</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0857</td>
      <td>-73.7209</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.0775</td>
      <td>-73.9875</td>
      <td>-73.9882</td>
      <td>-73.9638</td>
      <td>-73.935</td>
      <td>-73.8755</td>
      <td>-73.8125</td>
      <td>-73.7759</td>
      <td>-73.7327</td>
      <td>-73.9202</td>
      <td>0.0584627</td>
      <td>0.00341789</td>
      <td>0.623622</td>
      <td>-0.262603</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5835</td>
      <td>40.8797</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.5973</td>
      <td>40.6928</td>
      <td>40.6911</td>
      <td>40.7226</td>
      <td>40.7567</td>
      <td>40.7918</td>
      <td>40.8495</td>
      <td>40.868</td>
      <td>40.8787</td>
      <td>40.7583</td>
      <td>0.0517399</td>
      <td>0.00267701</td>
      <td>0.0390404</td>
      <td>-0.203525</td>
    </tr>
    <tr>
      <th>Yolcuların</th>
      <td>FieldType.DECIMAL</td>
      <td>1</td>
      <td>6</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1.</td>
      <td>1.</td>
      <td>1.</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>6</td>
      <td>2.39249</td>
      <td>1.83197</td>
      <td>3.3561</td>
      <td>0.763144</td>
      <td>-1.23467</td>
    </tr>
    <tr>
      <th>uzaklık</th>
      <td>FieldType.DECIMAL</td>
      <td>0.01</td>
      <td>32.34</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0108744</td>
      <td>0.743898</td>
      <td>0.738194</td>
      <td>1.243</td>
      <td>2.40168</td>
      <td>4.74478</td>
      <td>10.5136</td>
      <td>14.9011</td>
      <td>21.8035</td>
      <td>3.5447</td>
      <td>3.2943</td>
      <td>10.8524</td>
      <td>1.91556</td>
      <td>4.99898</td>
    </tr>
    <tr>
      <th>maliyet</th>
      <td>FieldType.DECIMAL</td>
      <td>0.1</td>
      <td>88</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>6148.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.33837</td>
      <td>5.00491</td>
      <td>5</td>
      <td>6.93129</td>
      <td>10.524</td>
      <td>17.4811</td>
      <td>33.2343</td>
      <td>50.0093</td>
      <td>63.1753</td>
      <td>13.6843</td>
      <td>9.66571</td>
      <td>93.426</td>
      <td>1.78518</td>
      <td>4.13972</td>
    </tr>
  </tbody>
</table>

Sütunlar ekleyerek deneme için verileri hazırlama `dflow_x` bizim model oluşturmaya yönelik özellikler için. Tanımladığınız `dflow_y` bizim tahmin değeri olarak **maliyet**:

```python
dflow_X = dflow_prepared.keep_columns(['pickup_weekday','pickup_hour', 'distance','passengers', 'vendor'])
dflow_y = dflow_prepared.keep_columns('cost')
```

### <a name="split-the-data-into-train-and-test-sets"></a>Verileri train bölme ve kümelerini test

Artık verileri eğitim halinde bölme ve kullanarak test kümelerini `train_test_split` işlevi `sklearn` kitaplığı. Bu işlev x, verileri riske **özellikleri**, veri kümesi için model eğitiminin ve y **tahmin etmek için değerleri**, test için veri kümesi. `test_size` Parametresi test ayrılacak veri yüzdesini belirler. `random_state` Parametre, eğitin ve test bölmeleri her zaman kararlı ve böylelikle bu çekirdek rastgele oluşturucuya ayarlar:

```python
from sklearn.model_selection import train_test_split

x_df = dflow_X.to_pandas_dataframe()
y_df = dflow_y.to_pandas_dataframe()

x_train, x_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=223)
# flatten y_train to 1d array
y_train.values.flatten()
```

Bu adımın amacı modeli eğitmek için doğru doğruluğu ölçmek üzere kullanılan henüz tamamlanan modeli test etmek için veri noktalarına sahip olmaktır. Diğer bir deyişle, iyi eğitilen bir modelin verilerden Öngörüler zaten görüldü henüz doğru bir şekilde yapabilir olmalıdır. Artık gerekli paketleri vardır ve veri modelinizin autotraining için hazır.

## <a name="automatically-train-a-model"></a>Otomatik olarak bir modeli eğitme

Otomatik olarak bir modeli eğitmek için aşağıdaki adımları uygulayın:
1. Denemeyi çalıştırma ayarlarını tanımlayın. Eğitim verilerinizi yapılandırmasına ekleyin ve eğitim işlemini denetleyen ayarları değiştirin.
1. Modeli ayarlama denemeyi gönderme. Denemeyi gönderdikten sonra işlemi farklı makine öğrenimi algoritmaları ve hiper parametre ayarlarını, tanımlanan kısıtlamalara gezinir. En uygun model doğruluğu ölçüm iyileştirerek seçer.

### <a name="define-settings-for-autogeneration-and-tuning"></a>Serilerinin otomatik oluşturulması ve ayarlamak için ayarlarını tanımlayın

Deneme parametreyi tanımlayın ve ayarları serilerinin otomatik oluşturulması ve ayarlamak için model. Tam listesini görüntüleyin [ayarları](how-to-configure-auto-train.md). Bu varsayılan ayarlar ile deneme gönderme yaklaşık 10-15 dakika alır, ancak daha kısa bir süre çalıştırmak istiyorsanız ya da azaltmak `iterations` veya `iteration_timeout_minutes`.


|Özellik| Bu öğreticideki değer |Açıklama|
|----|----|---|
|**iteration_timeout_minutes**|10|Her yineleme için dakika cinsinden süre. Toplam çalışma zamanı azaltmak için bu değeri azaltın.|
|**iterations**|30|Yineleme sayısı. Her yinelemede verilerinizle yeni bir machine learning modeli eğitilir. Bu toplam çalıştırma süresi etkileyen birincil bir değerdir.|
|**primary_metric**| spearman_correlation | İyileştirmek istediğiniz ölçüm. Bu ölçüm temelinde en uygun model seçilir.|
|**preprocess**| True | Kullanarak **True**, deneme (sayısal, vb. için metin dönüştürme eksik veri işleme) girdi verilerini önceden işlenir|
|**Ayrıntı düzeyi**| logging.INFO | Günlüğe kaydetme düzeyini denetler.|
|**n_cross_validations**|5|Doğrulama verileri belirtilmediğinde gerçekleştirmek için çapraz doğrulama bölmelerini sayısı.|



```python
automl_settings = {
    "iteration_timeout_minutes" : 10,
    "iterations" : 30,
    "primary_metric" : 'spearman_correlation',
    "preprocess" : True,
    "verbosity" : logging.INFO,
    "n_cross_validations": 5
}
```

Bir parametre olarak, tanımlanan eğitim ayarlarını kullanmak bir `AutoMLConfig` nesne. Ayrıca, eğitim verilerinizi ve türü olan model belirtin `regression` bu durumda.

```python
from azureml.train.automl import AutoMLConfig

# local compute
automated_ml_config = AutoMLConfig(task = 'regression',
                             debug_log = 'automated_ml_errors.log',
                             path = project_folder,
                             X = x_train.values,
                             y = y_train.values.flatten(),
                             **automl_settings)
```

### <a name="train-the-automatic-regression-model"></a>Otomatik bir regresyon modeli eğitme

Denemeyi yerel olarak çalıştırmak için başlatın. Tanımlanan geçirmek `automated_ml_config` deneme nesne. Çıkış kümesine `True` deneme sırasında ilerleme durumunu görüntülemek için:


```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, experiment_name)
local_run = experiment.submit(automated_ml_config, show_output=True)
```

Güncelleştirmeleri gösterilen çıktıya denemeyi çalışırken dinamik. Her yineleme için model türü, çalışma süresini ve eğitim doğruluğu bakın. Alan `BEST` eğitim puan en iyi çalışan parçaları temel ölçüm türünüz üzerinde.

    Parent Run ID: AutoML_02778de3-3696-46e9-a71b-521c8fca0651
    *******************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    *******************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler ExtremeRandomTrees                0:00:08       0.9447    0.9447
             1   StandardScalerWrapper GradientBoosting         0:00:09       0.9536    0.9536
             2   StandardScalerWrapper ExtremeRandomTrees       0:00:09       0.8580    0.9536
             3   StandardScalerWrapper RandomForest             0:00:08       0.9147    0.9536
             4   StandardScalerWrapper ExtremeRandomTrees       0:00:45       0.9398    0.9536
             5   MaxAbsScaler LightGBM                          0:00:08       0.9562    0.9562
             6   StandardScalerWrapper ExtremeRandomTrees       0:00:27       0.8282    0.9562
             7   StandardScalerWrapper LightGBM                 0:00:07       0.9421    0.9562
             8   MaxAbsScaler DecisionTree                      0:00:08       0.9526    0.9562
             9   MaxAbsScaler RandomForest                      0:00:09       0.9355    0.9562
            10   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            11   MaxAbsScaler LightGBM                          0:00:09       0.9553    0.9602
            12   MaxAbsScaler DecisionTree                      0:00:07       0.9484    0.9602
            13   MaxAbsScaler LightGBM                          0:00:08       0.9540    0.9602
            14   MaxAbsScaler RandomForest                      0:00:10       0.9365    0.9602
            15   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            16   StandardScalerWrapper ExtremeRandomTrees       0:00:49       0.9171    0.9602
            17   SparseNormalizer LightGBM                      0:00:08       0.9191    0.9602
            18   MaxAbsScaler DecisionTree                      0:00:08       0.9402    0.9602
            19   StandardScalerWrapper ElasticNet               0:00:08       0.9603    0.9603
            20   MaxAbsScaler DecisionTree                      0:00:08       0.9513    0.9603
            21   MaxAbsScaler SGD                               0:00:08       0.9603    0.9603
            22   MaxAbsScaler SGD                               0:00:10       0.9602    0.9603
            23   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            24   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            25   MaxAbsScaler SGD                               0:00:09       0.9603    0.9603
            26   TruncatedSVDWrapper ElasticNet                 0:00:09       0.9602    0.9603
            27   MaxAbsScaler SGD                               0:00:12       0.9413    0.9603
            28   StandardScalerWrapper ElasticNet               0:00:07       0.9603    0.9603
            29    Ensemble                                      0:00:38       0.9622    0.9622

## <a name="explore-the-results"></a>Sonuçları inceleme

Otomatik eğitim Jupyter pencere öğesi veya deneme geçmişini inceleyerek sonuçları keşfedin.

### <a name="option-1-add-a-jupyter-widget-to-see-results"></a>1. seçenek: Sonuçları görmek için Jupyter pencere öğesi ekleyin

Jupyter not defteri kullanırsanız, bir grafik ve tablo tüm sonuçları görmek için bu Jupyter not defteri pencere öğesini kullanın:


```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Jupyter pencere öğesi çalıştırması ayrıntıları](./media/tutorial-auto-train-models/automl-dash-output.png)
![Jupyter pencere öğesi çizim](./media/tutorial-auto-train-models/automl-chart-output.png)

### <a name="option-2-get-and-examine-all-run-iterations-in-python"></a>2. seçenek: Alma ve Python çalışma tüm yinelemelerde inceleyin

Ayrıca, her deneme geçmişini almak ve her bir yineleme çalıştırma bireysel ölçümleri keşfedin. RMSE (root_mean_squared_error) çalıştırmak için tek tek her modeli inceleyerek, çoğu yinelemeler makul bir kenar boşluğu ($3-4) içinde taksi adil maliyetini tahmin etmek bakın.

```python
children = list(local_run.get_children())
metricslist = {}
for run in children:
    properties = run.get_properties()
    metrics = {k: v for k, v in run.get_metrics().items() if isinstance(v, float)}
    metricslist[int(properties['iteration'])] = metrics

rundata = pd.DataFrame(metricslist).sort_index(1)
rundata
```

<div>
<style scoped> .dataframe tbody tr th: yalnızca-of-type {Dikey Hizala: Orta;}

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1.</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>explained_variance</th>
      <td>0.811037</td>
      <td>0.880553</td>
      <td>0.398582</td>
      <td>0.776040</td>
      <td>0.663869</td>
      <td>0.875911</td>
      <td>0.115632</td>
      <td>0.586905</td>
      <td>0.851911</td>
      <td>0.793964</td>
      <td>...</td>
      <td>0.850023</td>
      <td>0.883603</td>
      <td>0.883704</td>
      <td>0.880797</td>
      <td>0.881564</td>
      <td>0.883708</td>
      <td>0.881826</td>
      <td>0.585377</td>
      <td>0.883123</td>
      <td>0.886817</td>
    </tr>
    <tr>
      <th>mean_absolute_error</th>
      <td>2.189444</td>
      <td>1.500412</td>
      <td>5.480531</td>
      <td>2.626316</td>
      <td>2.973026</td>
      <td>1.550199</td>
      <td>6.383868</td>
      <td>4.414241</td>
      <td>1.743328</td>
      <td>2.294601</td>
      <td>...</td>
      <td>1.797402</td>
      <td>1.415815</td>
      <td>1.418167</td>
      <td>1.578617</td>
      <td>1.559427</td>
      <td>1.413042</td>
      <td>1.551698</td>
      <td>4.069196</td>
      <td>1.505795</td>
      <td>1.430957</td>
    </tr>
    <tr>
      <th>median_absolute_error</th>
      <td>1.438417</td>
      <td>0.850899</td>
      <td>4.579662</td>
      <td>1.765210</td>
      <td>1.594600</td>
      <td>0.869883</td>
      <td>4.266450</td>
      <td>3.627355</td>
      <td>0.954992</td>
      <td>1.361014</td>
      <td>...</td>
      <td>0.973634</td>
      <td>0.774814</td>
      <td>0.797269</td>
      <td>1.147234</td>
      <td>1.116424</td>
      <td>0.783958</td>
      <td>1.098464</td>
      <td>2.709027</td>
      <td>1.003728</td>
      <td>0.851724</td>
    </tr>
    <tr>
      <th>normalized_mean_absolute_error</th>
      <td>0.024908</td>
      <td>0.017070</td>
      <td>0.062350</td>
      <td>0.029878</td>
      <td>0.033823</td>
      <td>0.017636</td>
      <td>0.072626</td>
      <td>0.050219</td>
      <td>0.019833</td>
      <td>0.026105</td>
      <td>...</td>
      <td>0.020448</td>
      <td>0.016107</td>
      <td>0.016134</td>
      <td>0.017959</td>
      <td>0.017741</td>
      <td>0.016076</td>
      <td>0.017653</td>
      <td>0.046293</td>
      <td>0.017131</td>
      <td>0.016279</td>
    </tr>
    <tr>
      <th>normalized_median_absolute_error</th>
      <td>0.016364</td>
      <td>0.009680</td>
      <td>0.052101</td>
      <td>0.020082</td>
      <td>0.018141</td>
      <td>0.009896</td>
      <td>0.048538</td>
      <td>0.041267</td>
      <td>0.010865</td>
      <td>0.015484</td>
      <td>...</td>
      <td>0.011077</td>
      <td>0.008815</td>
      <td>0.009070</td>
      <td>0.013052</td>
      <td>0.012701</td>
      <td>0.008919</td>
      <td>0.012497</td>
      <td>0.030819</td>
      <td>0.011419</td>
      <td>0.009690</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_error</th>
      <td>0.047968</td>
      <td>0.037882</td>
      <td>0.085572</td>
      <td>0.052282</td>
      <td>0.065809</td>
      <td>0.038664</td>
      <td>0.109401</td>
      <td>0.071104</td>
      <td>0.042294</td>
      <td>0.049967</td>
      <td>...</td>
      <td>0.042565</td>
      <td>0.037685</td>
      <td>0.037557</td>
      <td>0.037643</td>
      <td>0.037513</td>
      <td>0.037560</td>
      <td>0.037465</td>
      <td>0.072077</td>
      <td>0.037249</td>
      <td>0.036716</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_log_error</th>
      <td>0.055353</td>
      <td>0.045000</td>
      <td>0.110219</td>
      <td>0.065633</td>
      <td>0.063589</td>
      <td>0.044412</td>
      <td>0.123433</td>
      <td>0.092312</td>
      <td>0.046130</td>
      <td>0.055243</td>
      <td>...</td>
      <td>0.046540</td>
      <td>0.041804</td>
      <td>0.041771</td>
      <td>0.045175</td>
      <td>0.044628</td>
      <td>0.041617</td>
      <td>0.044405</td>
      <td>0.079651</td>
      <td>0.042799</td>
      <td>0.041530</td>
    </tr>
    <tr>
      <th>r2_score</th>
      <td>0.810900</td>
      <td>0.880328</td>
      <td>0.398076</td>
      <td>0.775957</td>
      <td>0.642812</td>
      <td>0.875719</td>
      <td>0.021603</td>
      <td>0.586514</td>
      <td>0.851767</td>
      <td>0.793671</td>
      <td>...</td>
      <td>0.849809</td>
      <td>0.880142</td>
      <td>0.880952</td>
      <td>0.880586</td>
      <td>0.881347</td>
      <td>0.880887</td>
      <td>0.881613</td>
      <td>0.548121</td>
      <td>0.882883</td>
      <td>0.886321</td>
    </tr>
    <tr>
      <th>root_mean_squared_error</th>
      <td>4.216362</td>
      <td>3.329810</td>
      <td>7.521765</td>
      <td>4.595604</td>
      <td>5.784601</td>
      <td>3.398540</td>
      <td>9.616354</td>
      <td>6.250011</td>
      <td>3.717661</td>
      <td>4.392072</td>
      <td>...</td>
      <td>3.741447</td>
      <td>3.312533</td>
      <td>3.301242</td>
      <td>3.308795</td>
      <td>3.297389</td>
      <td>3.301485</td>
      <td>3.293182</td>
      <td>6.335581</td>
      <td>3.274209</td>
      <td>3.227365</td>
    </tr>
    <tr>
      <th>root_mean_squared_log_error</th>
      <td>0.243184</td>
      <td>0.197702</td>
      <td>0.484227</td>
      <td>0.288349</td>
      <td>0.279367</td>
      <td>0.195116</td>
      <td>0.542281</td>
      <td>0.405559</td>
      <td>0.202666</td>
      <td>0.242702</td>
      <td>...</td>
      <td>0.204464</td>
      <td>0.183658</td>
      <td>0.183514</td>
      <td>0.198468</td>
      <td>0.196067</td>
      <td>0.182836</td>
      <td>0.195087</td>
      <td>0.349935</td>
      <td>0.188031</td>
      <td>0.182455</td>
    </tr>
    <tr>
      <th>spearman_correlation</th>
      <td>0.944743</td>
      <td>0.953618</td>
      <td>0.857965</td>
      <td>0.914703</td>
      <td>0.939846</td>
      <td>0.956159</td>
      <td>0.828187</td>
      <td>0.942069</td>
      <td>0.952581</td>
      <td>0.935477</td>
      <td>...</td>
      <td>0.951287</td>
      <td>0.960335</td>
      <td>0.960195</td>
      <td>0.960279</td>
      <td>0.960288</td>
      <td>0.960323</td>
      <td>0.960161</td>
      <td>0.941254</td>
      <td>0.960293</td>
      <td>0.962158</td>
    </tr>
    <tr>
      <th>spearman_correlation_max</th>
      <td>0.944743</td>
      <td>0.953618</td>
      <td>0.953618</td>
      <td>0.953618</td>
      <td>0.953618</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>0.956159</td>
      <td>...</td>
      <td>0.960303</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.960335</td>
      <td>0.962158</td>
    </tr>
  </tbody>
</table>
<p>12 × 30 sütunları satırlar</p>
</div>

## <a name="retrieve-the-best-model"></a>En iyi modeli alma

Bizim yinelemeler en iyi işlem hattını seçin. `get_output` Metodunda `automl_classifier` son çağırma sığdırmak için en iyi çalıştırmanın ve ekrana sığdırılmış modeli döndürür. Aşırı kullanarak `get_output`, en iyi çalıştırmanın alabilir ve herhangi bir ekrana sığdırılmış modeli günlüğe ölçüm veya belirli bir yineleme:

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="test-the-best-model-accuracy"></a>En iyi modeli doğruluk testi

Taksi fares tahmin etmek için test veri kümesi üzerinde Öngörüler çalıştırma için en iyi modeli kullanın. İşlev `predict` en iyi modeli kullanır ve y değerleri tahmin **geçirmek maliyet**, gelen `x_test` veri kümesi. Yazdırma değerlerinden maliyet tahmin ilk 10 `y_predict`:

```python
y_predict = fitted_model.predict(x_test.values)
print(y_predict[:10])
```

Gerçek maliyet değerlere kıyasla tahmin edilen maliyet değerleri görselleştirmek için bir dağılım grafiğinde noktalara oluşturun. Aşağıdaki kod `distance` özellik x ekseni ve seyahat `cost` y ekseni olarak. Her bir seyahat uzaklık değeri bir tahmin edilen maliyet varyansını Karşılaştırılacak ilk 100 öngörülen ve gerçek maliyet değerleri ayrı seri olarak oluşturulur. Çizim İnceleme uzaklığı/maliyet neredeyse doğrusal bir ilişkidir ve çoğu durumda çok yakın gerçek maliyet değerlerin aynı seyahat uzaklığı için tahmin edilen maliyet değerler gösterilir.

```python
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(14, 10))
ax1 = fig.add_subplot(111)

distance_vals = [x[4] for x in x_test.values]
y_actual = y_test.values.flatten().tolist()

ax1.scatter(distance_vals[:100], y_predict[:100], s=18, c='b', marker="s", label='Predicted')
ax1.scatter(distance_vals[:100], y_actual[:100], s=18, c='r', marker="o", label='Actual')

ax1.set_xlabel('distance (mi)')
ax1.set_title('Predicted and Actual Cost/Distance')
ax1.set_ylabel('Cost ($)')

plt.legend(loc='upper left', prop={'size': 12})
plt.rcParams.update({'font.size': 14})
plt.show()
```

![Tahmin dağılım grafiğinde noktalara](./media/tutorial-auto-train-models/automl-scatter-plot.png)

Hesapla `root mean squared error` sonuçları. Kullanım `y_test` veri çerçevesi. Tahmin edilen değerlere karşılaştırılacak bir listeye dönüştürün. İşlev `mean_squared_error` iki dizi değerlerini alır ve bunlar arasındaki ortalama karesi alınmış hata hesaplar. Sonuç kare kökünü alma sağlar bir hata y değişken aynı birimde **maliyet**. Bu kabaca ne kadar taksi taksi Öngörüler gerçek fares gösterir:

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

    3.2204936862688798

Ortalama mutlak tamamlanma hata (MAPE) tam hesaplamak için aşağıdaki kodu çalıştırın `y_actual` ve `y_predict` veri kümeleri. Bu ölçüm her öngörülen ve gerçek değerinin farkını tüm arasındaki mutlak bir farkı hesaplar. Ardından, TOPLA gerçek değerlerin toplamının yüzde ifade:

```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.10545153869569586

    Model Accuracy:
    0.8945484613043041

Son tahmin doğruluğunu ölçümleri modeli oldukça taksi fares veri kümesinin özelliklerinden genellikle içinde + - $3.00 tahmin etmeye yönelik en iyi olduğuna bakın. Geleneksel makine öğrenme modeli geliştirme sürecinde yüksek kaynak kullanımı yoğun ve çalıştırmak ve modelleri onlarca sonuçları karşılaştırmak için önemli bir etki alanı bilgilerini ve saat yatırım gerektirir. Otomatik makine öğrenimini kullanarak, birçok farklı modelde senaryonuzda hızlı bir şekilde test etmek için harika bir yoludur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Makine öğrenimi öğreticisinde bu otomatik, aşağıdaki görevleri yapar:

> [!div class="checklist"]
> * Bir çalışma alanı ve hazırlanmış veri bir deneme için yapılandırılmış.
> * Bir otomatik gerileme modelini kullanarak yerel olarak özel parametrelerle eğitim.
> * Keşfedilen ve gözden geçirilmiş eğitim sonuçları.

[Modelinizi](tutorial-deploy-models-with-aml.md) Azure Machine Learning ile.
