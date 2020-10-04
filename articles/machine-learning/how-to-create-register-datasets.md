---
title: Verilere erişmek için Azure Machine Learning veri kümeleri oluşturma
titleSuffix: Azure Machine Learning
description: Machine Learning deneme çalıştırmaları için verilerinize erişmek üzere Azure Machine Learning veri kümeleri oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, contperfq1
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/31/2020
ms.openlocfilehash: 58377913d4352038a683c67986ad33e215315457
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91708424"
---
# <a name="create-azure-machine-learning-datasets"></a>Azure Machine Learning veri kümeleri oluşturma



Bu makalede, Azure Machine Learning Python SDK ile yerel veya uzak denemeleri verilerinize erişmek için Azure Machine Learning veri kümeleri oluşturmayı öğreneceksiniz. Veri kümelerinin Azure Machine Learning genel veri erişimi iş akışına uygun olduğunu anlamak için, [güvenli erişim verileri](concept-data.md#data-workflow) makalesine bakın.

Bir veri kümesi oluşturduğunuzda ilgili veri kaynağı konumuna bir başvurunun yanı sıra meta verilerinin bir kopyasını oluşturmuş olursunuz. Veriler mevcut konumunda kaldığı için ek depolama ücreti ödemeniz ve veri kaynaklarınızın bütünlüğünü riske çıkarmazsınız. Ayrıca, veri kümeleri, iş akışı performans hızına yardımcı olan geç değerlendirilir. Veri mağazalarından, genel URL 'Lerden ve [Azure açık veri](../open-datasets/how-to-create-azure-machine-learning-dataset-from-open-dataset.md)kümelerinden veri kümeleri oluşturabilirsiniz.

Düşük kod deneyimi için, [Azure Machine Learning Studio ile Azure Machine Learning veri kümeleri oluşturun.](how-to-connect-data-ui.md#create-datasets)

Azure Machine Learning veri kümeleri ile şunları yapabilirsiniz:

* Veri kümeleri tarafından başvurulan, depolama alanındaki verilerin tek bir kopyasını saklayın.

* Bağlantı dizeleri veya veri yolları hakkında endişelenmeden model eğitimi sırasında verilere sorunsuzca erişin. [Veri kümeleriyle eğitme hakkında daha fazla bilgi edinin](how-to-train-with-datasets.md).

* Veri paylaşma ve diğer kullanıcılarla işbirliği yapma.

## <a name="prerequisites"></a>Önkoşullar

Veri kümeleri oluşturmak ve bunlarla çalışmak için şunlar gerekir:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

* Azureml [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true), azureml veri kümesi paketini içerir.

    * Tümleşik Not defterleri ve SDK 'nın zaten yüklü olduğu tam olarak yapılandırılmış ve yönetilen bir geliştirme ortamı olan [Azure Machine Learning işlem örneği](how-to-create-manage-compute-instance.md)oluşturun.

    **OR**

    * Kendi Jupyter Not defteriniz üzerinde çalışın ve [bu yönergelerle](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)SDK 'yı kendiniz de yüklersiniz.

> [!NOTE]
> Bazı veri kümesi sınıflarının yalnızca 64 bitlik Python ile uyumlu olan [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py&preserve-view=true) paketine bağımlılıkları vardır. Linux kullanıcıları için, bu sınıflar yalnızca şu dağıtımlara göre desteklenir: Red Hat Enterprise Linux (7, 8), Ubuntu (14,04, 16,04, 18,04), Fedora (27, 28), demı (8, 9) ve CentOS (7). Desteklenmeyen kaldırmalar kullanıyorsanız, devam etmek için lütfen [Bu kılavuzu](https://docs.microsoft.com/dotnet/core/install/linux) Izleyerek .net Core 2,1 ' i yükleyebilirsiniz. 

## <a name="compute-size-guidance"></a>İşlem boyutu kılavuzu

Bir veri kümesi oluştururken, işlem işleme gücünden ve verilerinizin belleğindeki boyutunu gözden geçirin. Depolama alanındaki verilerinizin boyutu bir veri çerçevesindeki verilerin boyutuyla aynı değildir. Örneğin, CSV dosyalarındaki veriler bir veri çerçevesinde en fazla 10 x genişleyebilir, bu nedenle 1 GB CSV dosyası bir veri çerçevesinde 10 GB olabilir. 

Verileriniz sıkıştırılmışsa, daha fazla genişleyebilir; sıkıştırılmış Parquet biçiminde depolanan 20 GB görece seyrek veri, bellekte ~ 800 GB 'a genişleyebilir. Parquet dosyaları bir sütunlu biçimde veri depolarsanız, yalnızca sütunların yarısını içeriyorsa yalnızca ~ 400 GB 'yi belleğe yüklemeniz gerekir.

[Azure Machine Learning veri işlemeyi en iyi duruma getirme hakkında daha fazla bilgi edinin](concept-optimize-data-processing.md).

## <a name="dataset-types"></a>Veri kümesi türleri

Kullanıcıların eğitiminde nasıl tükettiği temel alınarak iki veri kümesi türü vardır; Dosya veri kümeleri ve Tabulardataset. Her iki tür de, estimators,, oto ml, hiper sürücü ve işlem hatları içeren Azure Machine Learning eğitim iş akışlarında kullanılabilir. 

### <a name="filedataset"></a>Dosya veri kümesi

Bir [dosya veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py&preserve-view=true) , veri mağazalarınızın veya genel URL 'nizin tek veya birden çok dosyasına başvurur. Verileriniz zaten temizdir ve eğitim denemeleri 'de kullanıma hazırsanız, dosyaları dosya veri kümesi nesnesi olarak işlem dosyalarınıza [indirebilir veya bağlayabilirsiniz](how-to-train-with-datasets.md#mount-vs-download) . 

Kaynak dosyalar herhangi bir biçimde olduğundan, derin öğrenme dahil olmak üzere daha geniş bir makine öğrenimi senaryosu sağlayan herhangi bir biçimde olduğundan, makine öğrenimi iş akışlarınız için dosya veri kümelerini öneririz.

[Python SDK](#create-a-filedataset) veya [Azure Machine Learning Studio](how-to-connect-data-ui.md#create-datasets) ile bir dosya veri kümesi oluşturun.
### <a name="tabulardataset"></a>TabularDataset

[Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py&preserve-view=true) , belirtilen dosya veya dosya listesini ayrıştırarak verileri tablolu biçimde temsil eder. Bu sayede, not defterinizin ayrılmasına gerek kalmadan tanıdık veri hazırlama ve eğitim kitaplıklarıyla çalışabilmeniz için verileri bir Pandas veya Spark veri çerçevesine hazırlama yeteneği sağlar. `TabularDataset`. Csv,. tsv,. Parquet,. jsonl dosyalarından ve [SQL sorgu sonuçlarından](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py&preserve-view=true#&preserve-view=truefrom-sql-query-query--validate-true--set-column-types-none--query-timeout-30-)bir nesne oluşturabilirsiniz.

Tabulardataset ile, verilerdeki bir sütundan veya bir zaman serisi nitelik sağlamak için yol deseninin verilerinin depolandığı her yerde bir zaman damgası belirtebilirsiniz. Bu belirtim zamana göre kolay ve etkili filtrelemeye olanak tanır. Bir örnek için, [NOAA Hava durumu verileri Içeren tablolu zaman serisiyle ılgılı API tanıtımı](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb)bölümüne bakın.

[Python SDK](#create-a-tabulardataset) veya [Azure Machine Learning Studio](how-to-connect-data-ui.md#create-datasets)Ile tabulardataset oluşturun.

>[!NOTE]
> Azure Machine Learning Studio ile oluşturulan oto ml iş akışları Şu anda yalnızca Tabulardataset 'i destekliyor. 

## <a name="access-datasets-in-a-virtual-network"></a>Bir sanal ağdaki veri kümelerine erişme

Çalışma alanınız bir sanal ağda ise, veri kümesini doğrulamayı atlayacak şekilde yapılandırmanız gerekir. Bir sanal ağda veri depoları ve veri kümeleri kullanma hakkında daha fazla bilgi için bkz. [bir çalışma alanını ve ilişkili kaynakları güvenli hale getirme](how-to-secure-workspace-vnet.md#secure-datastores-and-datasets).

<a name="datasets-sdk"></a>

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Azure Machine Learning tarafından erişilebilmesi için, veri kümelerinin [Azure veri depoları](how-to-access-data.md) veya genel Web URL 'lerinde yollardan oluşturulması gerekir. 

Python SDK ile bir [Azure veri deposundan](how-to-access-data.md) veri kümeleri oluşturmak için:

1. `contributor` `owner` Kayıtlı Azure veri deposuna sahip olduğunuzu veya erişiminiz olduğunu doğrulayın.

2. Veri deposundaki yollara başvurarak veri kümesini oluşturun. Birden çok veri mağazasında birden çok yoldan bir veri kümesi oluşturabilirsiniz. İçinden veri kümesi oluşturabileceğiniz dosya sayısı veya veri boyutu için sabit sınır yoktur. 

> [!NOTE]
> Her veri yolu için, depolama hizmetine bir dosya veya klasöre işaret edilip edilmeyeceğini denetlemek için birkaç istek gönderilir. Bu ek yük, performansın düşmesine veya başarısız olmasına neden olabilir. İçindeki 1000 dosya içeren bir klasöre başvuran bir veri kümesi, bir veri yoluna başvurulur. En iyi performansı elde etmek için veri depolarında 100 ' den az yola başvuruda bulunan veri kümesi oluşturmanızı öneririz.

### <a name="create-a-filedataset"></a>Dosya veri kümesi oluşturma

[`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py&preserve-view=true#&preserve-view=truefrom-files-path--validate-true-) `FileDatasetFactory` Dosyaları herhangi bir biçimde yüklemek ve kayıtsız bir dosya veri kümesi oluşturmak için sınıfındaki yöntemini kullanın. 

Depolama alanı bir sanal ağın veya güvenlik duvarının arkasındaysa, `validate=False` yöntebinizdeki parametreyi ayarlayın `from_files()` . Bu, ilk doğrulama adımını atlar ve veri kümenizi bu güvenli dosyalardan oluşturmanıza da emin olmanızı sağlar. [Veri depolarını ve veri kümelerini bir sanal ağda kullanma](how-to-secure-workspace-vnet.md#secure-datastores-and-datasets)hakkında daha fazla bilgi edinin.

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [(datastore, 'animals')]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = ['https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
             'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz']
mnist_ds = Dataset.File.from_files(path=web_paths)
```
Çalışma alanınızdaki deneme genelindeki veri kümelerini yeniden kullanmak ve paylaşmak için [veri kümenizi kaydedin](#register-datasets). 

> [!TIP] 
> Dosyaları yerel bir dizinden karşıya yükleyin ve tek bir yöntemde, [upload_directory ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py&preserve-view=true#methods)genel önizleme yöntemiyle bir dosya veri kümesi oluşturun. Bu yöntem, [deneysel](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py&preserve-view=true#stable-vs-experimental) Önizleme özelliğine sahiptir ve herhangi bir zamanda değişebilir. 
> 
>  Bu yöntem, verileri temel depolama verilerinize yükler ve sonuç olarak depolama maliyetlerine uygulanır. 
### <a name="create-a-tabulardataset"></a>TabularDataset oluşturma

[`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory) `TabularDatasetFactory` . Csv veya. tsv biçimindeki dosyaları okumak ve kayıtlı olmayan bir TabularDataset oluşturmak için sınıfındaki yöntemini kullanın. Birden çok dosyadan okuyorsanız, sonuçlar tek tablolu bir gösterimde toplanacaktır. 

Depolama alanı bir sanal ağın veya güvenlik duvarının arkasındaysa, `validate=False` yöntebinizdeki parametreyi ayarlayın `from_delimited_files()` . Bu, ilk doğrulama adımını atlar ve veri kümenizi bu güvenli dosyalardan oluşturmanıza da emin olmanızı sağlar. [Veri depolarını ve veri kümelerini bir sanal ağda](how-to-secure-workspace-vnet.md#secure-datastores-and-datasets)kullanma hakkında daha fazla bilgi edinin.

Aşağıdaki kod, mevcut çalışma alanını ve istenen veri deposunu ada göre alır. Ardından `path` , yeni bir TabularDataset oluşturmak için veri deposunu ve dosya konumlarını parametreye geçirir `weather_ds` .

```Python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
    
# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)

# create a TabularDataset from 3 file paths in datastore
datastore_paths = [(datastore, 'weather/2018/11.csv'),
                   (datastore, 'weather/2018/12.csv'),
                   (datastore, 'weather/2019/*.csv')]

weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

Varsayılan olarak, bir TabularDataset oluşturduğunuzda, sütun veri türleri otomatik olarak algılanır. Çıkarılan türler beklentilerinizle eşleşmiyorsa, aşağıdaki kodu kullanarak sütun türlerini belirtebilirsiniz. Parametresi `infer_column_type` yalnızca sınırlandırılmış dosyalardan oluşturulmuş veri kümeleri için geçerlidir. [Desteklenen veri türleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py&preserve-view=true).


```Python
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

|INDEKS|PassengerId|Kalan|PClass|Name|Komutu|Yaş|SibSp|Parch|Bilet|Tarifeli havayolu|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|Yanlış|3|Braund, Mr. Owen HARRIS|erkek|22.0|1|0|A/5 21171|7,2500||S
1|2|Doğru|1|Hanler, Mrs. John Bradley (çiçek)...|kadın|38,0|1|0|BILGISAYAR 17599|71,2833|C85|C
2|3|Doğru|3|Heıkkinen, Isabetsizlik. Laina|kadın|26,0|0|0|STON/O2. 3101282|7,9250||S

Çalışma alanınızdaki denemeleri genelinde veri kümelerini yeniden kullanmak ve paylaşmak için [veri kümenizi kaydedin](#register-datasets).

## <a name="create-a-dataset-from-pandas-dataframe"></a>Pandas dataframe 'ten bir veri kümesi oluşturma

Bellek Pandas dataframe 'ten bir TabularDataset oluşturmak için, verileri bir CSV gibi yerel bir dosyaya yazın ve veri kümenizi bu dosyadan oluşturun. Aşağıdaki kod bu iş akışını gösterir.

```python
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required

from azureml.core import Workspace, Dataset
local_path = 'data/prepared.csv'
dataframe.to_csv(local_path)

# upload the local file to a datastore on the cloud

subscription_id = 'xxxxxxxxxxxxxxxxxxxxx'
resource_group = 'xxxxxx'
workspace_name = 'xxxxxxxxxxxxxxxx'

workspace = Workspace(subscription_id, resource_group, workspace_name)

# get the datastore to upload prepared data
datastore = workspace.get_default_datastore()

# upload the local file from src_dir to the target_path in datastore
datastore.upload(src_dir='data', target_path='data')

# create a dataset referencing the cloud location
dataset = Dataset.Tabular.from_delimited_files(datastore.path('data/prepared.csv'))
```

> [!TIP]
> Ortak önizleme yöntemlerine sahip tek bir yöntemle bir WITH Memory Spark veya Pandas dataframe öğesinden bir TabularDataset oluşturun ve kaydedin [`register_spark_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py&preserve-view=true#methods) [`register_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py&preserve-view=true#methods) . Bu kayıt yöntemleri [deneysel](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py&preserve-view=true#stable-vs-experimental) önizleme özellikleridir ve herhangi bir zamanda değişebilir. 
> 
>  Bu yöntemler, verileri temel depolama verilerinize yükler ve sonuç olarak depolama maliyetlerine uygulanır. 

## <a name="register-datasets"></a>Veri kümelerini Kaydet

Oluşturma işlemini gerçekleştirmek için, veri kümelerinizi bir çalışma alanına kaydedin. Çalışma alanınızda [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py&preserve-view=true#&preserve-view=trueregister-workspace--name--description-none--tags-none--create-new-version-false-) başkalarıyla paylaşmak ve çalışma alanınızdaki denemeleri genelinde yeniden kullanmak için veri kümelerini çalışma alanınıza kaydetmek üzere yöntemini kullanın:

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

## <a name="create-datasets-using-azure-resource-manager"></a>Azure Resource Manager kullanarak veri kümeleri oluşturma

Üzerinde [https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-dataset-create-*](https://github.com/Azure/azure-quickstart-templates/tree/master/) veri kümeleri oluşturmak için kullanılabilecek çeşitli şablonlar vardır.

Bu şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma](how-to-create-workspace-template.md).


## <a name="create-datasets-with-azure-open-datasets"></a>Azure açık veri kümeleri ile veri kümeleri oluşturma

[Azure açık veri](https://azure.microsoft.com/services/open-datasets/) kümeleri, daha doğru modeller için makine öğrenimi çözümlerine senaryoya özgü özellikler eklemek için kullanabileceğiniz, seçkin ortak veri kümeleridir. Veri kümeleri, makine öğrenimi modellerini ve zenginleştirme çözümlerini eğitmenize yardımcı olan hava durumu, Census, tatiller, genel güvenlik ve konum için genel etki alanı verilerini içerir. Açık veri kümeleri bulutta Microsoft Azure ve hem SDK hem de Studio 'ya dahildir.

[Azure açık veri kümelerinden Azure Machine Learning veri kümeleri](../open-datasets/how-to-create-azure-machine-learning-dataset-from-open-dataset.md)oluşturmayı öğrenin. 

## <a name="train-with-datasets"></a>Veri kümeleriyle eğitme

ML modellerinizi eğitmek için Machine Learning denemeleri 'te veri kümelerinizi kullanın. [Veri kümeleriyle eğitme hakkında daha fazla bilgi edinin](how-to-train-with-datasets.md)

## <a name="version-datasets"></a>Sürüm veri kümeleri

Yeni bir sürüm oluşturarak aynı ada sahip yeni bir veri kümesini kaydedebilirsiniz. Veri kümesi sürümü, deneme veya gelecekteki bir çoğaltma için veri kümesinin belirli bir sürümünü uygulayabilmeniz için verilerinizin durumuna yer işaretinin bir yoludur. [Veri kümesi sürümleri](how-to-version-track-datasets.md)hakkında daha fazla bilgi edinin.
```Python
# create a TabularDataset from Titanic training data
web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
             'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri kümeleriyle eğitme hakkında](how-to-train-with-datasets.md)bilgi edinin.
* [Tabulardataset ile eğilmesi](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)için otomatik makine öğrenimini kullanın.
* Daha fazla veri kümesi eğitimi örnekleri için bkz. [örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/).
