---
title: Gelişmiş senaryolar için yazar girişi betiği
titleSuffix: Azure Machine Learning entry script authoring
description: Dağıtım sırasında ön ve son işleme için Azure Machine Learning girişi betikleri yazmayı öğrenin.
author: gvashishtha
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 09/17/2020
ms.author: gopalv
ms.openlocfilehash: 0146c6c003e3c22b63b5fde5c8979a9d7c112b69
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91261400"
---
# <a name="advanced-entry-script-authoring"></a>Gelişmiş giriş betiği yazma

Bu makalede, özel kullanım durumları için giriş betikleri yazma gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Azure Machine Learning ile dağıtmayı planladığınız eğitilen bir makine öğrenimi modeli zaten var. Model dağıtımı hakkında daha fazla bilgi edinmek için [Bu öğreticiye](how-to-deploy-and-where.md)bakın.

## <a name="automatically-generate-a-swagger-schema"></a>Otomatik olarak Swagger şeması oluştur

Web hizmetiniz için otomatik olarak bir şema oluşturmak üzere, tanımlı tür nesnelerinden biri için kurucudaki girişin ve/veya çıktının bir örneğini sağlayın. Tür ve örnek, otomatik olarak şemayı oluşturmak için kullanılır. Azure Machine Learning, dağıtım sırasında Web hizmeti için bir [Openapı](https://swagger.io/docs/specification/about/) (Swagger) belirtimi oluşturur.

Bu türler Şu anda destekleniyor:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python nesnesi

Şema oluşturmayı kullanmak için, `inference-schema` Bağımlılıklar dosyanıza 1.1.0 veya üzeri bir açık kaynaklı paket sürümü ekleyin. Bu paket hakkında daha fazla bilgi için bkz [https://github.com/Azure/InferenceSchema](https://github.com/Azure/InferenceSchema) .. Uygun Swagger otomatik Web hizmeti tüketimi oluşturmak için, Puanlama betiği çalıştırması () işlevinin API şekli olmalıdır:
* "StandardPythonParameterType" türünde, adlandırılmış girişler, PandasDataframeParameterTypes içeren bir ilk parametre.
* "StandardPythonParameterType" türünde, iç içe olmayan GlobalParameter adlı isteğe bağlı ikinci parametre.
* "StandardPythonParameterType" türünde bir sözlük döndürür. Bu, PandasDataFrameParameterTypes içeren iç içe geçmiş olabilir.
`input_sample` `output_sample` Web hizmetinin istek ve yanıt biçimlerini temsil eden ve değişkenlerinde giriş ve çıkış örnek biçimlerini tanımlayın. Bu örnekleri işlevindeki giriş ve çıkış işlevi Dekoratörleri içinde kullanın `run()` . Aşağıdaki scikit-örnek, şema oluşturmayı kullanır.



```python
#Example: scikit-learn and Swagger
import json
import numpy as np
import os
from sklearn.externals import joblib
from sklearn.linear_model import Ridge

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    # AZUREML_MODEL_DIR is an environment variable created during deployment. Join this path with the filename of the model file.
    # It holds the path to the directory that contains the deployed model (./azureml-models/$MODEL_NAME/$VERSION).
    # If there are multiple models, this value is the path to the directory containing all deployed models (./azureml-models).
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_mnist_model.pkl')

    # If your model were stored in the same directory as your score.py, you could also use the following:
    # model_path = os.path.abspath(os.path.join(os.path.dirname(__file_), 'sklearn_mnist_model.pkl')

    # Deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
output_sample = np.array([3726.995])


@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

## <a name="power-bi-compatible-endpoint"></a>Power BI uyumlu uç nokta 

Aşağıdaki örnekte, API şeklinin yukarıdaki yönergeye göre nasıl tanımlanacağı gösterilmektedir. Bu yöntem, dağıtılan Web hizmetinin Power BI tüketmesi için desteklenir. ([Power BI Web hizmetini kullanma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/power-bi/service-machine-learning-integration).)

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from sklearn.linear_model import Ridge

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.standard_py_parameter_type import StandardPythonParameterType
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # Replace filename if needed.
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_regression_model.pkl')
    # Deserialize the model file back into a sklearn model.
    model = joblib.load(model_path)

# providing 3 sample inputs for schema generation
numpy_sample_input = NumpyParameterType(np.array([[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]],dtype='float64'))
pandas_sample_input = PandasParameterType(pd.DataFrame({'name': ['Sarah', 'John'], 'age': [25, 26]}))
standard_sample_input = StandardPythonParameterType(0.0)

# This is a nested input sample, any item wrapped by `ParameterType` will be described by schema
sample_input = StandardPythonParameterType({'input1': numpy_sample_input, 
                                            'input2': pandas_sample_input, 
                                            'input3': standard_sample_input})

sample_global_parameters = StandardPythonParameterType(1.0) #this is optional
sample_output = StandardPythonParameterType([1.0, 1.0])

@input_schema('inputs', sample_input)
@input_schema('global_parameters', sample_global_parameters) #this is optional
@output_schema(sample_output)
def run(inputs, global_parameters):
    try:
        data = inputs['input1']
        # data will be convert to target format
        assert isinstance(data, np.ndarray)
        result = model.predict(data)
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```


## <a name="binary-ie-image-data"></a><a id="binary-data"></a> İkili (ör. resim) verileri

Modeliniz bir görüntü gibi ikili verileri kabul ediyorsa, `score.py` dağıtımınız için kullanılan dosyayı ham http isteklerini kabul edecek şekilde değiştirmeniz gerekir. Ham verileri kabul etmek için, `AMLRequest` giriş betiğinizdeki sınıfını kullanın ve `@rawhttp` işleve dekoratör ekleyin `run()` .

`score.py`İkili verileri kabul eden bir örneği aşağıda verilmiştir:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse


def init():
    print("This is init()")


@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs.
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real-world solution, you would load the data from reqBody
        # and send it to the model. Then return the response.

        # For demonstration purposes, this example just returns the posted data as the response.
        return AMLResponse(reqBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> `AMLRequest`Sınıfı `azureml.contrib` ad alanıdır. Bu ad alanındaki varlıklar, hizmeti geliştirmek için çalışmamız sırasında sıklıkla değişir. Bu ad alanındaki her türlü şey, Microsoft tarafından tam olarak desteklenmeyen bir önizleme olarak düşünülmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekirse, aşağıdaki komutu kullanarak bileşenleri yükleyebilirsiniz:
>
> ```shell
> pip install azureml-contrib-services
> ```

`AMLRequest`Sınıfı yalnızca Score.py içindeki ham gönderilen verilere erişmenize izin verir, hiçbir istemci tarafı bileşeni yoktur. Bir istemciden verileri normal olarak nakledersiniz. Örneğin, aşağıdaki Python kodu bir resim dosyasını okur ve verileri gönderir:

```python
import requests
# Load image data
data = open('example.jpg', 'rb').read()
# Post raw data to scoring URI
res = requests.post(url='<scoring-uri>', data=data, headers={'Content-Type': 'application/octet-stream'})
```

<a id="cors"></a>

## <a name="cross-origin-resource-sharing-cors"></a>Çıkış noktaları arası kaynak paylaşımı (CORS)

Çıkış noktaları arası kaynak paylaşımı, bir Web sayfasındaki kaynakların başka bir etki alanından istenmesinin izin vermesinin bir yoludur. CORS, istemci isteğiyle gönderilen ve hizmet yanıtıyla döndürülen HTTP üstbilgileri aracılığıyla çalışmaktadır. CORS ve geçerli üstbilgiler hakkında daha fazla bilgi için bkz. Vikipde [çıkış noktaları arası kaynak paylaşma](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) .

Model dağıtımınızı CORS 'yi destekleyecek şekilde yapılandırmak için, `AMLResponse` giriş betiğinizdeki sınıfını kullanın. Bu sınıf, yanıt nesnesindeki üst bilgileri ayarlamanıza olanak sağlar.

Aşağıdaki örnek, `Access-Control-Allow-Origin` giriş betiğindeki yanıtın üst bilgisini ayarlar:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs.
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real-world solution, you would load the data from reqBody
        # and send it to the model. Then return the response.

        # For demonstration purposes, this example
        # adds a header and returns the request body.
        resp = AMLResponse(reqBody, 200)
        resp.headers['Access-Control-Allow-Origin'] = "http://www.example.com"
        return resp
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> `AMLResponse`Sınıfı `azureml.contrib` ad alanıdır. Bu ad alanındaki varlıklar, hizmeti geliştirmek için çalışmamız sırasında sıklıkla değişir. Bu ad alanındaki her türlü şey, Microsoft tarafından tam olarak desteklenmeyen bir önizleme olarak düşünülmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekirse, aşağıdaki komutu kullanarak bileşenleri yükleyebilirsiniz:
>
> ```shell
> pip install azureml-contrib-services
> ```


> [!WARNING]
> Azure Machine Learning, Puanlama hizmetini çalıştıran kapsayıcılara yalnızca POST ve GET isteklerini yönlendirir. Bu, seçenek istekleri ön uçuş CORS isteklerini kullanan tarayıcılar nedeniyle hatalara neden olabilir.
> 


## <a name="load-registered-models"></a>Kayıtlı modelleri yükle

Giriş betiğinizdeki modelleri bulmanın iki yolu vardır:
* `AZUREML_MODEL_DIR`: Model konumunun yolunu içeren bir ortam değişkeni.
* `Model.get_model_path`: Kayıtlı model adı kullanılarak model dosyasının yolunu döndüren bir API.

#### <a name="azureml_model_dir"></a>AZUREML_MODEL_DIR

AZUREML_MODEL_DIR, hizmet dağıtımı sırasında oluşturulan bir ortam değişkenidir. Dağıtılan model konumunu bulmak için bu ortam değişkenini kullanabilirsiniz.

Aşağıdaki tabloda, dağıtılan modellerin sayısına bağlı olarak AZUREML_MODEL_DIR değeri açıklanmaktadır:

| Dağıtım | Ortam değişkeni değeri |
| ----- | ----- |
| Tek model | Modeli içeren klasörün yolu. |
| Birden çok model | Tüm modelleri içeren klasörün yolu. Modeller bu klasörde ad ve sürüm tarafından bulunur ( `$MODEL_NAME/$VERSION` ) |

Model kaydı ve dağıtımı sırasında modeller AZUREML_MODEL_DIR yoluna yerleştirilir ve özgün dosya adları korunur.

Giriş betiğinizdeki bir model dosyasının yolunu almak için, ortam değişkenini aradığınız dosya yoluyla birleştirin.

**Tek model örneği**
```python
# Example when the model is a file
model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_regression_model.pkl')

# Example when the model is a folder containing a file
file_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'my_model_folder', 'sklearn_regression_model.pkl')
```

**Birden çok model örneği**

Bu senaryoda, çalışma alanına iki model kaydedilir:

* `my_first_model`: Bir dosya ( `my_first_model.pkl` ) içerir ve yalnızca bir sürüm () vardır `1` .
* `my_second_model`: Bir dosya ( `my_second_model.pkl` ) içerir ve iki sürüm vardır: `1` ve `2` .

Hizmet dağıtıldığında her iki model de dağıtım işleminde sağlanır:

```python
first_model = Model(ws, name="my_first_model", version=1)
second_model = Model(ws, name="my_second_model", version=2)
service = Model.deploy(ws, "myservice", [first_model, second_model], inference_config, deployment_config)
```

Hizmeti barındıran Docker görüntüsünde, `AZUREML_MODEL_DIR` ortam değişkeni modellerin bulunduğu dizini içerir.
Bu dizinde, modellerin her biri dizin yolunda bulunur `MODEL_NAME/VERSION` . `MODEL_NAME`, Kayıtlı modelin adıdır ve `VERSION` modelin sürümüdür. Kayıtlı modeli oluşturan dosyalar bu dizinlerde depolanır.

Bu örnekte, yollar `$AZUREML_MODEL_DIR/my_first_model/1/my_first_model.pkl` ve olur `$AZUREML_MODEL_DIR/my_second_model/2/my_second_model.pkl` .


```python
# Example when the model is a file, and the deployment contains multiple models
first_model_name = 'my_first_model'
first_model_version = '1'
first_model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), first_model_name, first_model_version, 'my_first_model.pkl')
second_model_name = 'my_second_model'
second_model_version = '2'
second_model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), second_model_name, second_model_version, 'my_second_model.pkl')
```

### <a name="get_model_path"></a>get_model_path

Bir modeli kaydettiğinizde, kayıt defterinde modeli yönetmek için kullanılan bir model adı sağlarsınız. Bu adı [model.get_model_path ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py&preserve-view=true#&preserve-view=trueget-model-path-model-name--version-none---workspace-none-) yöntemiyle birlikte kullanarak, model dosyasının veya yerel dosya sistemindeki dosyaların yolunu alabilirsiniz. Bir klasörü veya dosya koleksiyonunu kaydettiğinizde, bu API bu dosyaları içeren dizinin yolunu döndürür.

Bir modeli kaydettiğinizde, buna bir ad verirsiniz. Ad, modelin yerel olarak ya da hizmet dağıtımı sırasında yerleştirildiği yere karşılık gelir.

## <a name="framework-specific-examples"></a>Çerçeveye özgü örnekler

Belirli makine öğrenimi kullanım örnekleri için daha fazla giriş betiği örnekleri aşağıda bulunabilir:

* [PyTorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch)
* [TensorFlow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/tensorflow)
* [Keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* [AutoML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features)
* [ONNX](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)

## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Web hizmetini güncelleştirme](how-to-deploy-update-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [TLS kullanarak Azure Machine Learning aracılığıyla web hizmetinin güvenliğini sağlama](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)
