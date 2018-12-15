---
title: Otomatik ML denemeleri oluşturma
titleSuffix: Azure Machine Learning service
description: Otomatik machine learning, sizin için bir algoritma seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz seçenekleri öğrenin.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 3dedf5de1ac2c88a9a00fd5f62e0663b840c0fd9
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438532"
---
# <a name="configure-automated-machine-learning-experiments"></a>Otomatik makine öğrenimi denemelerini yapılandırın

Otomatik machine learning bir algoritmaya ve hiperparametreleri sizin için seçer ve dağıtım için hazır bir model oluşturur. Otomatik makine öğrenimi denemelerini yapılandırmak için kullanabileceğiniz birkaç seçenek vardır. Bu kılavuzda, çeşitli yapılandırma ayarlarını tanımlamayı öğrenin.

Otomatik bir makine öğrenimi denemelerini örnekleri görüntülemek için bkz: [Öğreticisi: Otomatik machine learning ile bir sınıflandırma modeli eğitme](tutorial-auto-train-models.md) veya [eğitme modelleri bulutta otomatik machine learning ile](how-to-auto-train-remote.md).

Otomatik machine learning'de kullanılabilen yapılandırma seçenekleri:

* Deneme türünüzü seçin: Sınıflandırma, regresyon veya tahmin
* Veri kaynağı, biçimleri ve verileri getirme
* Hedef işlem seçin: yerel veya uzak
* Otomatik machine learning deneme ayarları
* Bir otomatik makine öğrenimi denemesi çalıştırın
* Model ölçümleri keşfedin
* Kaydolun ve model dağıtma

## <a name="select-your-experiment-type"></a>Deneme türünüzü seçin
Denemenizi başlamadan önce çözümü machine learning sorun türünü belirlemeniz gerekir. Otomatik machine learning, Sınıflandırma, regresyon ve tahmin görev türlerini destekler. 

Genel kullanıma sunulan otomatik makine öğrenimi özelliklerinden çalışırken **tahmin hala genel Önizleme aşamasında olan.**

Otomatik machine learning, otomasyon ve ayarlama işlemi sırasında aşağıdaki algoritmalarını destekler. Bir kullanıcı olarak, algoritma belirtmek gerek yoktur.

Sınıflandırma | Regresyon | Tahmin etme
|-- |-- |--
[Lojistik regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Esnek Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Esnek Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[C-Destek vektör sınıflandırma (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[Doğrusal SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[K Komşuları en yakın](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors)|[LARS Serbest Şekil](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Serbest Şekil](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stokastik aşama (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçları](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)|
[Işık GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|


## <a name="data-source-and-format"></a>Veri kaynağı ve biçimi
Otomatik machine learning, yerel masaüstüne veya Azure Blob Depolama gibi bulutta bulunan verileri destekler. Verilerin scikit okunacağı-desteklenen veri biçimlerinden öğrenin. Verileri okuyabilirsiniz:
* Numpy diziler X (özellikleri) ve y (hedef değişkeni veya olarak da bilinen etiket)
* Pandas dataframe 

Örnekler:

*   Numpy diziler

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

*   Pandas dataframe

    ```python
    import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Uzak işlem üzerinde denemeyi çalıştırmak için veri alma

Denemenizi çalıştırmak için uzak bir işlem kullanıyorsanız, veri getirme, ayrı bir python betiği alınmalıdır `get_data()`. Bu betik, otomatik makine öğrenimi denemesi çalıştırıldığı uzak işlem üzerinde çalıştırılır. `get_data` her yineleme için kablo üzerinden verileri getirme gereksinimini ortadan kaldırır. Olmadan `get_data`, uzak işlem üzerinde çalıştırdığınızda, deneme başarısız olur.

İşte bir örnek `get_data`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection import train_test_split 
from sklearn.preprocessing import LabelEncoder 
def get_data(): # Burning man 2016 data 
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42) 
    return { "X" : df, "y" : y }
```

İçinde `AutoMLConfig` nesne, belirttiğiniz `data_script` parametresi ve yolunu belirtin `get_data` aşağıdakine benzeyen bir komut dosyası:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

`get_data` betik döndürebilirsiniz:

Anahtar | Tür |    İle birbirini dışlayan | Açıklama
---|---|---|---
X | Pandas Dataframe veya Numpy dizisi | data_train, etiket, sütunları |  Tüm özellikleri ile eğitme
Y | Pandas Dataframe veya Numpy dizisi |   etiket   | Veri ile eğitmek için etiket. Sınıflandırma için tamsayı dizisi olmalıdır.
X_valid | Pandas Dataframe veya Numpy dizisi   | data_train, etiket | _İsteğe bağlı_ ile doğrulamak için tüm özellikleri. Belirtilmezse, X eğitimi arasında bölünmüş ve doğrulama
y_valid |   Pandas Dataframe veya Numpy dizisi | data_train, etiket | _İsteğe bağlı_ ile doğrulamak için verileri etiket. Belirtilmezse, y eğitimi arasında bölünmüş ve doğrulama
sample_weight | Pandas Dataframe veya Numpy dizisi |   data_train, etiket, sütunları| _İsteğe bağlı_ her örnek için bir ağırlık değeri. Veri noktaları için farklı ağırlıkları atamak istediğiniz zaman kullanın 
sample_weight_valid | Pandas Dataframe veya Numpy dizisi | data_train, etiket, sütunları |    _İsteğe bağlı_ her doğrulama örneği için bir ağırlık değeri. Belirtilmezse, sample_weight eğitimi arasında bölünmüş ve doğrulama
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Tüm veriler (Özellikler + etiketi) ile eğitme
etiket | dize  | X, y, X_valid, y_valid |  Hangi sütunun data_train etiketi temsil eder.
Sütunları | dize dizisi  ||  _İsteğe bağlı_ özelliklerini beyaz liste sütun
cv_splits_indices   | Tamsayı dizisi ||  _İsteğe bağlı_ dizinler için verileri çapraz doğrulama bölme listesi

### <a name="load-and-prepare-data-using-dataprep-sdk"></a>Yük ve DataPrep SDK'sını kullanarak veri hazırlama
Otomatik makine öğrenimi denemeleri, veri yüklenmesini destekler ve dataprep SDK'sını kullanarak dönüştürür. SDK'sını kullanarak sağlar

>* Birçok dosya türünü parametre çıkarımı (kodlama, ayırıcı, üst bilgiler) ayrıştırma ile yükleme
>* Tür-dönüştürme çıkarımı kullanılarak sırasında dosya yükleniyor
>* MS SQL Server ve Azure Data Lake Storage bağlantı desteği
>* Bir ifade kullanarak sütun ekleme
>* Eksik değerleri impute
>* Sütunu örneğe göre türetme
>* Filtreleme
>* Özel bir Python dönüşümler

Veriler hakkında bilgi edinmek için hazırlık sdk belgelerine başvurun [makale modellemek için veri hazırlama](how-to-load-data.md). Veri hazırlama SDK'sını kullanarak verileri yüklenirken bir örneği aşağıda verilmiştir. 
```python
# The data referenced here was pulled from `sklearn.datasets.load_digits()`.
simple_example_data_root = 'https://dprepdata.blob.core.windows.net/automl-notebook-data/'
X = dprep.auto_read_file(simple_example_data_root + 'X.csv').skip(1)  # Remove the header row.
# You can use `auto_read_file` which intelligently figures out delimiters and datatypes of a file.

# Here we read a comma delimited file and convert all columns to integers.
y = dprep.read_csv(simple_example_data_root + 'y.csv').to_long(dprep.ColumnSelector(term='.*', use_regex = True))
```

## <a name="train-and-validation-data"></a>Veri eğitme ve doğrulama

Ayrı eğitme ve get_data() üzerinden veya doğrudan bir doğrulama belirtebilirsiniz `AutoMLConfig` yöntemi.

## <a name="cross-validation-split-options"></a>Çapraz doğrulama bölme seçeneği

### <a name="k-folds-cross-validation"></a>K hatları çapraz doğrulama

Kullanım `n_cross_validations` ayarı doğrulamaları çapraz sayısını belirtin. Eğitim veri kümesi, içine rastgele bölünür `n_cross_validations` eşit boyutta hatları. Her çapraz doğrulama sırasında yuvarlak bir kat sayısı üzerinde kalan hatları eğitilmiş modelin doğrulama için kullanılır. Bu işlem yinelenir `n_cross_validations` her Katlama doğrulama kümesi olarak bir kez kullanılana kadar yuvarlar. Tüm ortalama puanları `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte Carlo çapraz doğrulama (yani) Yinelenen rastgele alt örnekleme)

Kullanma `validation_size` doğrulama ve kullanımı için kullanılması gereken eğitim veri kümesi yüzdesini belirtmek için `n_cross_validations` doğrulamaları çapraz sayısını belirtmek için. Her sırasında çapraz doğrulama round, bir alt boyutunun `validation_size` kalan veriler üzerine geliştirilen model doğrulama için rastgele seçilir. Son olarak, ortalama puanlar tamamında `n_cross_validations` yuvarlar bildirilir ve ilgili modeli tüm eğitim veri kümesi eğitilebileceği.

### <a name="custom-validation-dataset"></a>Özel doğrulama veri kümesi

Rastgele bölünmüş kabul edilebilir değilse, özel doğrulama veri kümesini kullan (genellikle zaman serisi verileri veya imbalanced verileri). Kendi doğrulama veri kümesi belirtebilirsiniz. Model doğrulama veri kümesi yerine rastgele veri kümesi belirtilen karşı değerlendirilir.

## <a name="compute-to-run-experiment"></a>Denemeyi çalıştırmak için işlem

Daha sonra modeli eğitimi burada belirleyin. Bir otomatik machine learning eğitim denemesini aşağıdaki işlem seçenekleri çalıştırabilirsiniz:
*   Yerel makinenizde yerel Masaüstü veya dizüstü – gibi genel olarak küçük veri kümesi olduğunda ve hala keşif aşamasında demektir.
*   Buluttaki – uzak bir makine [Azure Machine Learning işlem yönetilen](concept-azure-machine-learning-architecture.md#managed-and-unmanaged-compute-targets) kümelerinde Azure sanal makineler, makine öğrenimi modellerini eğitmenize olanağı sağlayan yönetilen bir hizmettir.

Bkz: [GitHub site](https://github.com/Azure/MachineLearningNotebooks/tree/master/automl) örneğin Not Defterleri ile yerel ve uzak hedef işlem.

<a name='configure-experiment'/>

## <a name="configure-your-experiment-settings"></a>Deneme ayarlarınızı yapılandırın

Otomatik makine öğrenimi deneme yapılandırmak için kullanabileceğiniz birkaç seçenek vardır. Bu parametreleri örnekleme tarafından ayarlanan bir `AutoMLConfig` nesne.

Bazı örnekler:

1.  Birincil Metrik yinelemeyle 50 yinelemeler ve 2 çapraz doğrulama hatları sonra sona erdirmek için denemeyi 12.000 saniyede en fazla süresine sahip olarak ağırlıklı AUC kullanarak sınıflandırma deneme.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        X=X, 
        y=y,
        n_cross_validations=2)
    ```
2.  100 yinelemeden sonra sonuna bir regresyon deneme kümesinin bir örnek aşağıda verilmiştir, en çok uzun, her yineleme ile 600 saniye ile 5 çapraz doğrulama hatları.

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

Bu tabloda parametre ayarlarını denemenizi ve varsayılan değerleri için kullanılabilir.

Özellik |  Açıklama | Varsayılan Değer
--|--|--
`task`  |Machine learning sorun türünü belirtin. İzin verilen değerler: <li>Sınıflandırma</li><li>Regresyon</li><li>Tahmin etme</li>    | None |
`primary_metric` |Modelinizi oluşturmak en iyi duruma getirmek istediğiniz ölçümü. Örneğin, doğruluk primary_metric belirtirseniz, en yüksek doğruluğa sahip modeli bulmak için makine öğrenimi görünüyor otomatik. Yalnızca deneme başına bir primary_metric belirtebilirsiniz. İzin verilen değerler: <br/>**Sınıflandırma**:<br/><li> accuracy  </li><li> AUC_weighted</li><li> precision_score_weighted </li><li> balanced_accuracy </li><li> average_precision_score_weighted </li><br/>**Regresyon**: <br/><li> normalized_mean_absolute_error </li><li> spearman_correlation </li><li> normalized_root_mean_squared_error </li><li> normalized_root_mean_squared_log_error</li><li> R2_score  </li> | Sınıflandırma: doğruluğu <br/>Regresyon için: spearman_correlation <br/> |
`experiment_exit_score` |   Hedef değer, primary_metric için ayarlayabilirsiniz. Bir model primary_metric hedef karşılayan bulunduğunda otomatik makine öğrenimi yineleme durdurur ve deneme sona erer. Bu değer (varsayılan) ayarlanmazsa, otomatik makine öğrenimi denemesi yinelemelerde belirtilen yineleme sayısını çalışmaya devam eder. Bir çift değer alır. Hedef hiçbir zaman ulaşırsa, yinelemeler içinde belirtilen yineleme sayısını ulaşana kadar otomatik makine öğrenimi devam eder.| None
`iterations` |En yüksek yineleme sayısı. Her yineleme, bir işlem hattı, sonuçları bir eğitim işini eşittir. İşlem hattı, verileri ön işleme ve modeli ' dir. 250 veya daha yüksek kaliteli modeli almak için kullanın    | 100
`max_concurrent_iterations`|    En fazla paralel olarak çalıştırmak için yineleme sayısı. Bu ayar, yalnızca uzak işlem için çalışır.|   1
`max_cores_per_iteration`   | Kaç tane çekirdeğim işlem hedef tek bir işlem hattını eğitmek için kullanılan gösterir. Birden çok çekirdek algoritması yararlanabilir, bu çok çekirdekli makine performansını artırır. Tüm çekirdekler üzerinde bir makineyi kullanmak için-1 ayarlayabilirsiniz.|  1
`iteration_timeout_minutes` |   Belirli bir yinelemeye geçen süreyi (dakika) miktarını sınırlar. Bir yineleme belirtilen miktarı aşarsa, bu yineleme iptal. Aksi durumda ayarlama, yineleme işlemi tamamlanana kadar çalışmaya devam edecektir. |   None
`n_cross_validations`   |Çapraz doğrulama bölmelerinin sayısı| None
`validation_size`   |Tüm eğitim örnek bir yüzdesi olarak ayarlanmış doğrulama boyutu.|  None
`preprocess` | True/False <br/>Giriş ön işleme gerçekleştirmek için doğru etkinleştirir deneyin. Aşağıdaki ön işleme'nın bir alt kümesidir<li>Eksik verileri: Eksik veri sayısal ortalama, çoğu geçişi ile birlikte metin ile imputes </li><li>Kategorik değerlere: Veri türü sayısal ve benzersiz değerlerin sayısını küçüktür yüzde 5 ', bir seyrek kodlama içine dönüştürür ise </li><li>Tam liste denetimi vb. [GitHub deposu](https://aka.ms/aml-notebooks)</li><br/>Not: veri seyrek ise kullanamazsınız önişle = true |  False | 
`blacklist_models`  | Otomatik makine öğrenimi denemesi çalışır birçok farklı algoritma vardır. Bazı algoritmalar deneme hariç tutmak için yapılandırın. Algoritmalarından kümeniz için iyi çalışmaz farkında olması durumunda yararlıdır. Algoritmalar hariç kaydettiğinizde kaynaklar ve eğitim süresini hesaplayabilirsiniz.<br/>Sınıflandırma için izin verilen değerler<br/><li>LogisticRegression</li><li>SGD</li><li>MultinomialNaiveBayes</li><li>BernoulliNaiveBayes</li><li>SVM</li><li>LinearSVM</li><li>KNN</li><li>DecisionTree</li><li>RandomForest</li><li>ExtremeRandomTrees</li><li>LightGBM</li><li>GradientBoosting</li><li>TensorFlowDNN</li><li>TensorFlowLinearClassifier</li><br/>Regresyon için izin verilen değerler<br/><li>ElasticNet</li><li>GradientBoosting</li><li>DecisionTree</li><li>KNN</li><li>LassoLars</li><li>SGD </li><li>RandomForest</li><li>ExtremeRandomTree</li><li>LightGBM</li><li>TensorFlowLinearRegressor</li><li>TensorFlowDNN</li></li><br/>Tahmin için izin verilen değerler<br/><li>ElasticNet</li><li>GradientBoosting</li><li>DecisionTree</li><li>KNN</li><li>LassoLars</li><li>SGD </li><li>RandomForest</li><li>ExtremeRandomTree</li><li>LightGBM</li><li>TensorFlowLinearRegressor</li><li>TensorFlowDNN</li></li>|   None
`whitelist_models`  | Otomatik makine öğrenimi denemesi çalışır birçok farklı algoritma vardır. Deneme için bazı algoritmalar içerecek şekilde yapılandırın. Algoritmalarından kümeniz için iyi iş farkında olması durumunda yararlıdır. <br/>Sınıflandırma için izin verilen değerler<br/><li>LogisticRegression</li><li>SGD</li><li>MultinomialNaiveBayes</li><li>BernoulliNaiveBayes</li><li>SVM</li><li>LinearSVM</li><li>KNN</li><li>DecisionTree</li><li>RandomForest</li><li>ExtremeRandomTrees</li><li>LightGBM</li><li>GradientBoosting</li><li>TensorFlowDNN</li><li>TensorFlowLinearClassifier</li><br/>Regresyon için izin verilen değerler<br/><li>ElasticNet</li><li>GradientBoosting</li><li>DecisionTree</li><li>KNN</li><li>LassoLars</li><li>SGD </li><li>RandomForest</li><li>ExtremeRandomTree</li><li>LightGBM</li><li>TensorFlowLinearRegressor</li><li>TensorFlowDNN</li></li><br/>Tahmin için izin verilen değerler<br/><li>ElasticNet</li><li>GradientBoosting</li><li>DecisionTree</li><li>KNN</li><li>LassoLars</li><li>SGD </li><li>RandomForest</li><li>ExtremeRandomTree</li><li>LightGBM</li><li>TensorFlowLinearRegressor</li><li>TensorFlowDNN</li></li>|  None
`verbosity` |En ayrıntılı ve kritik olan olan bilgileri günlüğe kaydetme düzeyini denetler en az. Ayrıntı düzeyi, python günlük paketinde tanımlanan aynı değerleri alır. İzin verilen değerler şunlardır:<br/><li>logging.INFO</li><li>günlüğe kaydetme. UYARI</li><li>günlüğe kaydetme. HATA</li><li>günlüğe kaydetme. KRİTİK</li>  | logging.INFO</li> 
`X` | Tüm özellikleri ile eğitme |  None
`y` |   Veri ile eğitmek için etiket. Sınıflandırma için tamsayı dizisi olmalıdır.|  None
`X_valid`|_İsteğe bağlı_ ile doğrulamak için tüm özellikleri. Belirtilmezse, X eğitimi arasında bölünmüş ve doğrulama |   None
`y_valid`   |_İsteğe bağlı_ ile doğrulamak için verileri etiket. Belirtilmezse, y eğitimi arasında bölünmüş ve doğrulama    | None
`sample_weight` |   _İsteğe bağlı_ her örnek için bir ağırlık değeri. Veri noktaları için farklı ağırlıkları atamak istediğiniz zaman kullanın |   None
`sample_weight_valid`   |   _İsteğe bağlı_ her doğrulama örneği için bir ağırlık değeri. Belirtilmezse, sample_weight eğitimi arasında bölünmüş ve doğrulama   | None
`run_configuration` |   RunConfiguration nesnesi.  Uzaktan çalıştırmalar için kullanılır. |None
`data_script`  |    Get_data yöntemi içeren dosyanın yolu.  Uzaktan çalıştırmalar için gereklidir.   |None
`model_explainability` | _İsteğe bağlı_ True/False <br/>  Her yineleme için özellik önem gerçekleştirmek için doğru etkinleştirir deneyin. Deneme tamamlandıktan sonra özellik önem isteğe bağlı olarak bu yineleme etkinleştirmek için belirli bir yinelemeye explain_model() yöntemi de kullanabilirsiniz. | False
`enable_ensembling`|Tüm yinelemeler tamamladıktan sonra ensembling yineleme etkinleştirmek için bayrak.| True 
`ensemble_iterations`|Son topluluğu parçası olarak bir ekrana sığdırılmış işlem hattı Seçtiğimiz yineleme sayısı.| 15
`experiment_timeout_minutes`| Tüm denemeyi çalıştırma alabilir (minues) süre miktarını kısıtlar | None

## <a name="data-pre-processing-and-featurization"></a>Veri ön işleme ve özellik kazandırma sayesinde

Kullanırsanız `preprocess=True`, ön işleme adımları olarak aşağıdaki verileri sizin için otomatik olarak gerçekleştirilir:
1.  Yüksek bir kardinalite veya herhangi bir fark özellik bırakın
    * Hiçbir yararlı bilgiler özellikleriyle, eğitim ve doğrulama kümesinden bırakın. Bu, eksik, tüm değerlerin tüm satırlar boyunca veya son derece yüksek kardinalite (örneğin, karmaları kimlikleri veya GUID'leri) ile aynı değere sahip özellikler içerir.
1.  Eksik değer imputation
    *   Sayısal özellikler için eksik bir sütundaki değerlerin ortalamasını değerlerle impute.
    *   Kategorik özellikler için en sık değeri eksik değerlerle impute.
1.  Ek özellikler oluşturma
    * DateTime özellikleri: Yıl, ay, gün, haftanın günü, yıl, Çeyrek, yıl, saat, dakika, saniye haftanın günü.
    * Metin özellikleri: Word unigram, BI-gram ve üç-gram sayısı vektör hale getirici hakkında temel terimi sıklığı.
1.  Dönüşümler ve kodlamaları
    * Sayısal özellikleri kategorik özelliklerini dönüştürülmüş çok az benzersiz değerlere sahip.
    * Etiket kodlama veya (sık erişimli bir kodlama karma) bağlı olarak kardinalite kategorik özelliklerinin gerçekleştirin.

## <a name="run-experiment"></a>Denemeyi çalıştırma

Denemeyi çalıştırmak ve bir model oluşturmak için gönderin. Geçirmek `AutoMLConfig` için `submit` modeli oluşturmak için yöntemi.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Bağımlılıklar, önce yeni bir makineye yüklenir.  Bu çıkış gösterilmeden önce en fazla 10 dakika sürebilir.
>Ayarı `show_output` için `True` konsolunda gösterilen çıkış sonuçlanıyor.


## <a name="explore-model-metrics"></a>Model ölçümleri keşfedin
Bir not defteri kullanıyorsanız, sonuçlarınızı bir pencere öğesi veya satır içi görüntüleyebilirsiniz. Bkz: [izlemek ve modellerin değerlendirmesi](how-to-track-experiments.md#view-run-details) daha fazla ayrıntı için.


### <a name="classification-metrics"></a>Sınıflandırma ölçümleri
Aşağıdaki ölçümler, her yinelemede sınıflandırma görevi için kaydedilir.

|Birincil Metrik|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|--|
AUC_Macro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Her sınıf için AUC aritmetik ortalamasını makrodur.  | [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "makrosu" =|
AUC_Micro| AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Mikro globably doğru pozitif sonuçlar ve hatalı pozitif sonuçları her sınıftaki birleştirilerek hesaplanır| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | Ortalama "micro" =|
AUC_Weighted  | AUC alıcı çalıştırma özellikleri eğrisi altında alandır. Ağırlıklı ortalamasını puanın ağırlıklı true örnekleri her sınıfta sayısına göre her sınıf için olan| [Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|Ortalama "ağırlıklı" =
accuracy|Doğruluk true etiketlerin tam olarak eşleşen tahmin edilen etiketleri yüzdesi ' dir. |[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Her sınıf ortalama duyarlılık puanı aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "makrosu" =|
average_precision_score_micro|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Mikro doğru pozitif sonuçlar ve hatalı pozitif sonuçları en her kesme tarayan tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "micro" =|
average_precision_score_weighted|Ortalama kesinlik, duyarlık geri çekme eğri Precision bilgisayarlar daha önceki eşiği ağırlık kullanılan bölümden artış ile her Eşikte elde ağırlıklı ortalamasını olarak özetler. Ağırlıklı olan doğru örneklerinin her sınıfta tarafından ağırlıklı, her sınıf için ortalama kesinlik puanı aritmetik ortalaması|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|Ortalama "ağırlıklı" =|
balanced_accuracy|Dengeli doğruluğu her sınıf için geri çağırma aritmetik ortalamasıdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
f1_score_macro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "makrosu" =|
f1_score_micro|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Mikro toplam doğru pozitif sonuçlar, yanlış negatif ve hatalı pozitif sonuç sayımı tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "micro" =|
f1_score_weighted|F1 puanı harmonik duyarlık ve geri çağırma ' dir. Her sınıf için F1 puanı sınıfı sıklığı Ağırlıklı ortalama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|Ortalama "ağırlıklı" =|
log_loss|(ÇOKTERİMLİ) Lojistik regresyon ve bunu uzantıları sinir ağları, negatif log-olasılığını olasılıklara sınıflandırıcının Öngörüler verilen true etiketlerin tanımlandığı gibi kullanılan kaybı işlev budur. True ile tek bir örnek için yt etiket {0,1} ve o yt yp olasılık tahmini = 1, günlük kaybı olan - P oturum (yt&#124;yp) =-(yt log(yp) + (1 - yt) günlük (1 - yp))|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|Normalleştirilmiş makrosu geri çağırma rastgele performans bir puan, 0 ve 1'in bir puan mükemmel performans sahip olacak şekilde normalleştirilmiş makrosu geri çağırma olur. Bu norm_macro_recall tarafından sağlanır: (recall_score_macro - R) = /(1-R), R recall_score_macro (yani, R = 0,5 ikili sınıflandırma için) ve C sınıflı sınıflandırma sorunları R=(1/C) rastgele tahminler elde etmek için beklenen değeri olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "makrosu" = ve ardından (recall_score_macro - R) /(1-R), R recall_score_macro (yani, R = 0,5 ikili sınıflandırma için) ve C sınıflı sınıflandırma sorunları R=(1/C) rastgele tahminler elde etmek için beklenen değeri olduğu|
precision_score_macro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Her sınıf için duyarlık aritmetik ortalamasını makrodur|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "makrosu" =|
precision_score_micro|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Mikro toplam doğru pozitif sonuçlar ve hatalı pozitif sonuç sayımı tarafından genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "micro" =|
precision_score_weighted|Duyarlık aslında bu sınıfta olan belirli bir sınıf olarak etiketlenmiş öğeler yüzdesi ' dir. Ağırlıklı ortalamasını duyarlık true her sınıf örneği sayısını tarafından ağırlıklı, her sınıf için olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|Ortalama "ağırlıklı" =|
recall_score_macro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Aritmetik ortalamasıdır her sınıf için geri çağırma makrosu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "makrosu" =|
recall_score_micro|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Toplam doğru pozitif sonuçlar, yanlış negatif sayılarak Micro genel olarak hesaplanır|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "micro" =|
recall_score_weighted|Geri çağırma doğru olarak etiketlenmiştir öğelerin aslında bir sınıftaki belirli yüzde ' dir. Ağırlıklı ortalamasını true her sınıf örneği sayısını tarafından ağırlıklı, her sınıf için geri çağırma olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|Ortalama "ağırlıklı" =|
weighted_accuracy|Her örnek için verilen ağırlık o örneğin true sınıfında true örnekleri oranını eşit olduğu doğruluğu ağırlıklı doğruluğu olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|Hedef eşit oranda her öğe için söz konusu sınıfın bir vektör sample_weight olduğu|

### <a name="regression-and-forecasting-metrics"></a>Regresyon ve tahmin ölçümleri
Aşağıdaki ölçümler, her yineleme için gerileme veya tahmin görev kaydedilir.

|Birincil Metrik|Açıklama|Hesaplama|Ek parametreler
--|--|--|--|--|
explained_variance|Anlatıldığı farkı, belirli bir veri kümesi çeşitlemesi için matematiksel bir model hesapları oranı ' dir. Sadece, varyans hataları varyansını özgün verilerin içinde azaltma yüzdesi değil. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2 belirleme veya karesi alınmış hataların ortalaması çıkaran bir temel modele kıyasla yüzde azalma katsayısıdır. Hataların ortalaması 0 olduğunda anlatıldığı varyansı için eşittir.|[Hesaplama](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|Spearman bağıntı iki veri kümesi arasındaki ilişkinin monotonicity nonparametric ölçüsüdür. Pearson bağıntı, iki veri kümesini normalde dağıtılmış Spearman bağıntı varsaymaz. Diğer korelasyon katsayısını gibi bunu -1 ve + ile hiçbir bağıntısı olduğunu belirtmek için 0 ile 1 arasında değişiklik gösterir. -1 veya + 1 bağıntılar tam bir monoton ilişki kapsıyor. Pozitif bağıntılar x olarak arttıkça, bu nedenle y yaptığı kapsıyor. Negatif bağıntılar olarak arttıkça, x y azaltır kapsıyor.|[Hesaplama](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|Mutlak hata tahmin ile hedef arasındaki farkı mutlak değeri beklenen değeri anlama|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|Veri aralığı tarafından ayrılmış mean Absolute Error normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Veri aralığına göre Böl|
median_absolute_error|Ortalama mutlak hata tahmin ile hedef arasındaki tüm mutlak farkları ortalamasıdır. Aykırı değerleri için bu kayıp sağlamdır.|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|Veri aralığı tarafından ayrılmış ortalama mutlak hata normalleştirilmiş ortalama mutlak hata olduğu|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Veri aralığına göre Böl|
root_mean_squared_error|Kök ortalama karesi alınmış hata hedef ve tahmin arasındaki karesi alınmış Beklenen fark kare köküdür|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|Normalleştirilmiş kök ortalama karesi alınmış hata kök ortalama karesi alınmış hata veri aralığını tarafından ayrılmış olan|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Veri aralığına göre Böl|
root_mean_squared_log_error|Kök ortalama karesi alınmış günlük hatadır Logaritmik beklenen karesi alınmış hata kare kökünü|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|Noramlized kök ortalama karesi alınmış günlük hatadır veri aralığına göre bölünmüş kök ortalama karesi alınmış günlük hata|[Hesaplama](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Veri aralığına göre Böl|

## <a name="explain-the-model"></a>Modeli açıklayan

Genel kullanıma sunulan otomatik makine öğrenimi özelliklerinden çalışırken **modeli explainability özellik hala genel Önizleme aşamasındadır.**

Otomatik machine learning özellik önem anlamanıza olanak sağlar.  Eğitim işlemi sırasında modeli için genel özellik önem alabilirsiniz.  Sınıflandırma senaryoları için sınıf düzeyi özelliği önem alabilirsiniz.  Doğrulama veri kümesi özelliği önem almak için (X_valid) sağlamanız gerekir.

Özellik önem oluşturmak için iki yolunuz vardır.

*   Bir deney tamamlandıktan sonra kullanabileceğiniz `explain_model` tüm yineleme yöntemi.

    ```
    from azureml.train.automl.automlexplainer import explain_model
    
    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)
    
    #Overall feature importance
    print(overall_imp)
    print(overall_summary) 
    
    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary) 
    ```

*   Tüm yinelemeler için özellik önem görüntülemek için ayarlanmış `model_explainability` bayrak `True` AutoMLConfig içinde.  

    ```
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train, 
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Bunu yaptıktan sonra belirli bir yineleme için özellik önem alınacak retrieve_model_explanation yöntemi kullanabilirsiniz.

    ```
    from azureml.train.automl.automlexplainer import retrieve_model_explanation
    
    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)
    
    #Overall feature importance
    print(overall_imp)
    print(overall_summary) 
    
    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary) 
    ```

Azure portalında çalışma alanınızdaki özellik önem grafiği görselleştirebilirsiniz. Grafik da Jupyter pencere öğesi içinde bir not defteri kullanırken gösterilir. Bilgi edinmek için grafikler hakkında daha fazla başvurmak [Azure ML örnek not defterleri makalesi.](samples-notebooks.md)

```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![özellik önem grafiği](./media/how-to-configure-auto-train/feature-importance.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede model dağıtma](how-to-deploy-and-where.md).

Daha fazla bilgi edinin [otomatik machine learning ile bir sınıflandırma modeli eğitmek nasıl](tutorial-auto-train-models.md) veya [uzak bir kaynağa machine learning kullanarak eğitme otomatik](how-to-auto-train-remote.md). 
