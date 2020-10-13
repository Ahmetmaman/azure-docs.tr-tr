---
title: Sorun giderme & bilinen sorunlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning hataları veya hataları bulma ve düzeltme konusunda yardım alın. Bilinen sorunlar, sorun giderme ve geçici çözümler hakkında bilgi edinin.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: troubleshooting, contperfq4
ms.date: 10/02/2020
ms.openlocfilehash: c4250be15b1c4fdc5df81c0f0ba3623dedf6488f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91667274"
---
# <a name="known-issues-and-troubleshooting-in-azure-machine-learning"></a>Azure Machine Learning 'de bilinen sorunlar ve sorun giderme

Bu makale, Azure Machine Learning kullanırken karşılaşabileceğiniz bilinen sorunları gidermenize yardımcı olur. 

Sorun giderme hakkında daha fazla bilgi için bu makalenin sonundaki [sonraki adımlar](#next-steps) bölümüne bakın.

> [!TIP]
> Hatalar veya diğer sorunlar, Azure Machine Learning çalışırken karşılaştığınız [kaynak kotalarının](how-to-manage-quotas.md) sonucu olabilir. 

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

Bazen yardım isterken tanılama bilgilerini sağlayabilmeniz faydalı olabilir. Bazı günlükleri görmek için: 
1. [Azure Machine Learning Studio 'yu](https://ml.azure.com)ziyaret edin. 
1. Sol tarafta **denemeler** ' i seçin. 
1. Bir deneme seçin.
1. Bir çalıştırma seçin.
1. Üstteki **çıktılar + Günlükler**' i seçin.

> [!NOTE]
> Azure Machine Learning, eğitim sırasında (örneğin, oto ml) veya eğitim işini çalıştıran Docker kapsayıcısı gibi çeşitli kaynaklardan günlük bilgileri günlüğe kaydeder. Bu günlüklerin birçoğu açıklanmamıştır. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.


## <a name="installation-and-import"></a>Yükleme ve içeri aktarma
                           
* **PIP yüklemesi: bağımlılıkların tek satırlık yüklemeyle tutarlı olması garanti edilmez:** 

   Tek bir satır olarak yüklediğinizde çalışan bir bağımlılık Çözümleyicisi olmadığından, bu, PIP 'nin bilinen bir sınırlamasıdır. İlk benzersiz bağımlılık, tek bir baktığı tek. 

   Aşağıdaki kodda `azureml-datadrift` ve `azureml-train-automl` tek satırlı bir pınstall kullanılarak yüklenir. 
     ```
       pip install azureml-datadrift, azureml-train-automl
     ```
   Bu örnekte, diyelim ki `azureml-datadrift` 1,0 sürüm > ve `azureml-train-automl` < 1,2 sürümünü gerektiriyor. En son sürümü `azureml-datadrift` 1,3 ise, `azureml-train-automl` daha eski bir sürümün paket gereksiniminden bağımsız olarak her iki paket de 1,3 ' e yükseltilir. 

   Paketleriniz için uygun sürümlerin yüklendiğinden emin olmak için aşağıdaki kodda gibi birden çok satır kullanarak yükleme yapın. Burada sıra bir sorun değildir, çünkü PIP bir sonraki satır çağrısının parçası olarak açıkça indirgendiğinden. Bu nedenle, uygun sürüm bağımlılıkları uygulanır.
    
     ```
        pip install azureml-datadrift
        pip install azureml-train-automl 
     ```
     
* **, Azureml-tren-oto ml-Client yüklenirken açıklama paketinin yüklenmesi garanti edilmez:** 
   
   Model açıklaması etkinken uzak bir oto ml çalıştırma çalışırken, "Lütfen model açıklamaları için azureml-açıkla-model paketi 'ni Install" hata iletisini görürsünüz. Bu bilinen bir sorundur. Geçici bir çözüm olarak aşağıdaki adımlardan birini izleyin:
  
  1. Azureml-açıkla-model yerel olarak yüklenir.
   ```
      pip install azureml-explain-model
   ```
  2. Explainability özelliğini, oto ml yapılandırmasında model_explainability = false geçirerek tamamen devre dışı bırakın.
   ```
      automl_config = AutoMLConfig(task = 'classification',
                             path = '.',
                             debug_log = 'automated_ml_errors.log',
                             compute_target = compute_target,
                             run_configuration = aml_run_config,
                             featurization = 'auto',
                             model_explainability=False,
                             training_data = prepped_data,
                             label_column_name = 'Survived',
                             **automl_settings)
    ``` 
    
* **Panda hataları: genellikle, normal ml denemesi sırasında görüldü:**
   
   Ortamınızı PIP kullanarak el ile kurarken, desteklenmeyen paket sürümlerinin yüklenmesi nedeniyle öznitelik hataları (özellikle Pandas 'tan) fark edebilirsiniz. Bu tür hataları engellemek için [lütfen automl_setup. cmd ' yi kullanarak oto ml SDK 'sını yüklemelisiniz](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md):
   
    1. Bir Anaconda istemi açın ve bir örnek Not defteri kümesi için GitHub deposunu kopyalayın.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
    
    2. Örnek Not defterlerinin ayıklandığı nasıl yapılır kullanımı-azureml/otomatik makine öğrenimi klasörü ve ardından şunu çalıştırın:
    
    ```bash
    automl_setup
    ```
    
* **KeyError: yerel işlem veya Azure Databricks kümesinde oto ml çalıştırırken ' marka '**

    10 Haziran 2020 ' den sonra yeni bir ortam oluşturulduysa, SDK 1.7.0 veya daha önceki bir sürümü kullanılarak, bu hata,, Kopyala-cpuınfo paketindeki bir güncelleştirme nedeniyle başarısız olabilir. (Önbelleğe alınan eğitim görüntülerinin kullanıldığı için, denemeleri Haziran 2020 tarihinde veya öncesinde oluşturulan ortamlar etkilenmemiştir.) Bu sorunu geçici olarak çözmek için aşağıdaki iki adımdan birini gerçekleştirin:
    
    * SDK sürümünü 1.8.0 veya üzeri olarak güncelleştirin (Bu ayrıca des-cpuınfo 'yu 5.0.0 ' ye düşürült):
    
      ```bash
      pip install --upgrade azureml-sdk[automl]
      ```
    
    * Yüklü olan Kopyala-cpuınfo sürümünün 5.0.0 'e düşürme:
    
      ```bash
      pip install py-cpuinfo==5.0.0
      ```
  
* **Hata iletisi: ' PyYAML ' kaldırılamıyor**

    Python için Azure Machine Learning SDK: PyYAML yüklü bir `distutils` projem. Bu nedenle, kısmi bir kaldırma işlemi varsa, hangi dosyaların kendisine ait olduğunu doğru bir şekilde belirleyemedik. Bu hatayı yoksayarak SDK 'Yı yüklemeye devam etmek için şunu kullanın:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **Azure Machine Learning SDK yüklemesi bir özel durumla başarısız oluyor: Modulenotfounyıl: ' ruamel ' veya ' ımporterror: ' ruamel. YAML ' adlı modül yok**
   
   Bu sorun, Python için Azure Machine Learning SDK 'sının yayınlanan tüm sürümleri için Conda temel ortamındaki en son PIP ortamında Python için Azure Machine Learning SDK 'nın yüklenmesiyle (>20.1.1) ile karşılaştı. Aşağıdaki geçici çözümlere başvurun:

    * Conda temel ortamına Python SDK yüklemeden kaçının, bunun yerine Conda ortamınızı oluşturun ve SDK 'Yı yeni oluşturulan kullanıcı ortamına yüklemeniz gerekir. En son PIP, bu yeni Conda ortamında çalışmalıdır.

    * Conda temel ortamından geçiş yapmak istediğiniz Docker 'da görüntü oluşturmak için lütfen Docker dosyasında PIP<= 20.1.1 öğesini sabitleyin.

    ```Python
    conda install -c r -y conda python=3.6.2 pip=20.1.1
    ```
    
* **Paketler yüklenirken databricks hatası**

    Azure Machine Learning SDK yüklemesi, daha fazla paket yüklendiğinde Azure Databricks başarısız olur. Gibi bazı paketler `psutil` çakışmalara neden olabilir. Yükleme hatalarını önlemek için, kitaplık sürümünü dondurarak paketleri yükleme. Bu sorun, Azure Machine Learning SDK 'Sı değil Databricks ile ilgilidir. Bu sorunla diğer kitaplıklarla de karşılaşabilirsiniz. Örnek:
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    Alternatif olarak, Python kitaplıklarıyla ilgili sorunları gidermek için Init betiklerini kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için bkz. [küme kapsamlı init betikleri](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

* **Databricks içeri aktarma hatası: adı `Timedelta` öğesinden `pandas._libs.tslibs` içeri **aktarılamıyor: otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz, Not defterinizde aşağıdaki iki satırı çalıştırın:
    ```
    %sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
    %sh /databricks/python/bin/pip install pandas==0.23.4
    ```

* **Databricks içeri aktarma hatası: ' Pandas. Core. Indexes ' adlı bir modül yok**: otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz:

    1. Azure Databricks kümenize iki paket yüklemek için şu komutu çalıştırın:
    
       ```bash
       scikit-learn==0.19.1
       pandas==0.22.0
       ```
    
    1. Kümeyi ayırın ve Not defterinize yeniden bağlayın.
    
    Bu adımlar sorunu çözmezse, kümeyi yeniden başlatmayı deneyin.

* **Databricks Failtosendtıı**: `FailToSendFeather` Azure Databricks kümesindeki verileri okurken bir hata görürseniz, aşağıdaki çözümlere başvurun:
    
    * `azureml-sdk[automl]`Paketi en son sürüme yükseltin.
    * `azureml-dataprep`Sürüm 1.1.8 veya üstünü ekleyin.
    * `pyarrow`Sürüm 0,11 veya üstünü ekleyin.
    
## <a name="create-and-manage-workspaces"></a>Çalışma alanları oluşturma ve yönetme

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneliğe taşımak veya sahip olunan aboneliğin yeni bir kiracıya taşınması desteklenmez. Bunun yapılması hatalara neden olabilir.

* **Azure Portal**: 
  * SDK 'dan veya Azure portal bir Share bağlantısından çalışma alanınıza doğrudan giderseniz, içindeki abonelik bilgilerine sahip standart **genel bakış** sayfasını görüntüleyemezsiniz. Bu senaryoda, başka bir çalışma alanına geçiş yapamazsınız. Başka bir çalışma alanını görüntülemek için doğrudan [Azure Machine Learning Studio](https://ml.azure.com) 'ya gidin ve çalışma alanı adını arayın.
  * Tüm varlıklar (veri kümeleri, denemeleri, hesaplar vb.) yalnızca [Azure Machine Learning Studio](https://ml.azure.com)'da kullanılabilir. Azure portal mevcut *değildir* .

* **Azure Machine Learning Studio Web portalındaki desteklenen tarayıcılar**: işletim sisteminizle uyumlu en güncel tarayıcıyı kullanmanızı öneririz. Aşağıdaki tarayıcılar desteklenir:
  * Microsoft Edge (yeni Microsoft Edge, en son sürüm. Microsoft Edge eski değil)
  * Safari (en so sürüm, yalnızca Mac)
  * Chrome (en son sürüm)
  * Firefox (en son sürüm)

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

* **Amlcompute oluşturma sorunu**: Azure Machine Learning çalışma ALANıNı, GA sürümünden önce Azure Portal oluşturan bazı kullanıcıların, bu çalışma alanında AmlCompute oluşturamayacak nadir bir şansınız vardır. Hizmette bir destek isteği oluşturabilir veya portal veya SDK aracılığıyla hemen engelini kaldırmak için yeni bir çalışma alanı oluşturabilirsiniz.

* **Azure Container Registry Şu anda kaynak grubu adlarında Unicode karakterleri desteklememektedir**: kaynak grubu adı Unicode karakterler içerdiğinden ACR istekleri başarısız olabilir. Bu sorunu gidermek için, farklı adlandırılmış kaynak grubunda bir ACR oluşturmanız önerilir.

## <a name="work-with-data"></a>Verilerle çalışma

### <a name="overloaded-azurefile-storage"></a>Aşırı yüklenmiş AzureFile depolama

Bir hata alırsanız `Unable to upload project files to working directory in AzureFile because the storage is overloaded` , aşağıdaki geçici çözümleri uygulayın.

Veri aktarımı gibi diğer iş yükleri için dosya paylaşma 'yı kullanıyorsanız, dosya paylaşımının çalıştırmaları için kullanılabilmesi için blob 'ları kullanmak, bu nedenle söz konusu çalışma. İş yükünü iki farklı çalışma alanı arasında da bölebilirsiniz.

### <a name="passing-data-as-input"></a>Verileri giriş olarak geçirme

*  **TypeError: Fılenotfound: böyle bir dosya veya dizin yok**: sağladığınız dosya yolu dosyanın bulunduğu yere değilse bu hata oluşur. Dosyaya başvurduğunuzdan emin olmanız gerekir. bu şekilde, veri kümenizi işlem Hedefinizdeki bağladığınız konum ile tutarlıdır. Belirleyici bir durum sağlamak için, bir veri kümesini bir işlem hedefine bağlamak için soyut yolun kullanılmasını öneririz. Örneğin, aşağıdaki kodda, veri kümesini işlem hedefinin FileSystem kökünün altına bağlamamız gerekir `/tmp` . 
    
    ```python
    # Note the leading / in '/tmp/dataset'
    script_params = {
        '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
    } 
    ```

    Önde gelen eğik çizgi '/' dahil değilseniz, `/mnt/batch/.../tmp/dataset` veri kümesinin bağlanmasını istediğiniz yeri belirtmek için işlem hedefi üzerinde çalışma dizinini (.) ön eki uygulamanız gerekir.

### <a name="mount-dataset"></a>Veri kümesini bağla
* **Veri kümesi başlatması başarısız oldu: bağlama noktasının hazır olması bekleniyor zaman aşımına uğradı**: sorunu azaltmak için ' de yeniden deneme mantığı eklenmiştir `azureml-sdk >=1.12.0` . Önceki azureml-SDK sürümleriniz varsa lütfen en son sürüme yükseltin. Zaten açık ise `azureml-sdk>=1.12.0` , düzeltmenin en son düzeltme ekine sahip olması için lütfen ortamınızı yeniden oluşturun.

### <a name="data-labeling-projects"></a>Veri etiketleme projeleri

|Sorun  |Çözüm  |
|---------|---------|
|Yalnızca blob veri depolarında oluşturulan veri kümeleri kullanılabilir.     |  Bu, geçerli sürümün bilinen bir sınırlamasıdır.       |
|Oluşturulduktan sonra proje, uzun süredir "başlatılıyor" olarak gösterilir.     | Sayfayı el ile yenileyin. Başlatma, saniyede yaklaşık 20 veri noktasında devam etmelidir. Bu, bilinen bir sorundur.         |
|Görüntüleri gözden geçirirken yeni etiketlenmiş görüntüler gösterilmez.     |   Etiketlenmiş tüm görüntüleri yüklemek için **ilk** düğmeyi seçin. **İlk** düğme, listenin önüne geri götürür, ancak etiketlenmiş tüm verileri yükler.      |
|Nesne algılama için etiketleme sırasında Esc tuşuna basmak, sol üst köşede Sıfır boyutlu bir etiket oluşturur. Etiketlerin bu durumda gönderilmesi başarısız oluyor.     |   Yanındaki çapraz işaretine tıklayarak etiketi silin.  |

### <a name="data-drift-monitors"></a><a name="data-drift"></a> Veri kayması izleyicileri

Veri kayması izleyicileri için sınırlamalar ve bilinen sorunlar:

* Geçmiş verileri çözümlemede zaman aralığı, izleyicinin Sıklık ayarı için 31 aralıklarıyla sınırlıdır. 
* Bir özellik listesi belirtilmediği takdirde (kullanılan tüm özellikler) 200 özelliklerinin sınırlaması.
* İşlem boyutu, verileri işleyecek kadar büyük olmalıdır.
* Veri kümenizin, belirli bir izleyici çalıştırması için başlangıç ve bitiş tarihi içinde verileri olduğundan emin olun.
* Veri kümesi izleyicileri yalnızca 50 satır veya daha fazlasını içeren veri kümelerinde çalışır.
* Veri kümesindeki sütunlar veya özellikler, aşağıdaki tabloda yer alan koşullara göre kategorik veya sayısal olarak sınıflandırılır. Özellik bu koşulları karşılamıyorsa (örneğin, >100 benzersiz değerler içeren dize türünde bir sütun), bu özellik Data değişikliklerini algoritmadan bırakılır, ancak yine de profil oluşturulur. 

    | Özellik türü | Veri türü | Koşul | Sınırlamalar | 
    | ------------ | --------- | --------- | ----------- |
    | Kategorik | String, bool, int, float | Özelliğindeki benzersiz değer sayısı 100 ' den az ve satır sayısının %5 ' inden az. | Null, kendi kategorisi olarak değerlendirilir. | 
    | Sayısal | int, float | Özelliğindeki değerler sayısal bir veri türüdür ve kategorik bir özelliğin koşulunu karşılamaz. | Değerin %15 ' i >null ise özellik bırakıldı. | 

* [Bir veri DRI izlemesi](how-to-monitor-datasets.md) oluşturduğunuzda ancak Azure Machine Learning Studio 'Daki veri **kümesi izleyicileri** sayfasında verileri göremiyorsanız, aşağıdakileri deneyin.

    1. Sayfanın üst kısmında doğru tarih aralığını seçtiğinizden emin olun.  
    1. **Veri kümesi izleyicileri** sekmesinde, çalışma durumunu denetlemek için denemeler bağlantısını seçin.  Bu bağlantı tablonun en sağında bulunur.
    1. Çalıştırma başarıyla tamamlanırsa, kaç ölçüm oluşturulduğunu veya bir uyarı mesajı olduğunu görmek için sürücü günlüklerini denetleyin.  Bir deneye tıkladıktan sonra **Çıkış + Günlükler** sekmesinde sürücü günlüklerini bulun.

* SDK `backfill()` işlevi beklenen çıktıyı üretmiyorsa, bunun nedeni bir kimlik doğrulama sorunu olabilir.  Bu işleve geçirilecek bir işlem oluşturduğunuzda, kullanmayın `Run.get_context().experiment.workspace.compute_targets` .  Bunun yerine, bu işleve geçirdiğiniz işlem oluşturmak için aşağıdaki gibi [Serviceprincıpalauthentication](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py&preserve-view=true) kullanın `backfill()` : 

  ```python
   auth = ServicePrincipalAuthentication(
          tenant_id=tenant_id,
          service_principal_id=app_id,
          service_principal_password=client_secret
          )
   ws = Workspace.get("xxx", auth=auth, subscription_id="xxx", resource_group"xxx")
   compute = ws.compute_targets.get("xxx")
   ```

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısı

* **Uzun süreli işlem hazırlama süresi:**

Bir işlem hedefine ilk kez bağlandığınızda ya da daha uzun bir süre sonra bu işlem birkaç dakika olabilir. 

Model veri toplayıcısından, verilerin BLOB depolama hesabınıza gelmesi için 10 dakika kadar sürebilir (genellikle daha az). Aşağıdaki hücrelerin çalışmasını sağlamak için 10 dakika bekleyin.

```python
import time
time.sleep(600)
```

* **Gerçek zamanlı uç noktalar için günlük:**

Gerçek zamanlı uç noktaların günlükleri müşteri verileri. Gerçek zamanlı uç nokta sorunlarını gidermek için, günlükleri etkinleştirmek üzere aşağıdaki kodu kullanabilirsiniz. 

[Bu makaledeki](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights#query-logs-for-deployed-models)Web hizmeti uç noktalarını izleme hakkında daha fazla ayrıntı görüntüleyin.

```python
from azureml.core import Workspace
from azureml.core.webservice import Webservice

ws = Workspace.from_config()
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```
Birden çok kiracınız varsa, önce aşağıdaki kimlik doğrulama kodunu eklemeniz gerekebilir `ws = Workspace.from_config()`

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="the tenant_id in which your workspace resides")
```

## <a name="train-models"></a>Modelleri eğitme

* **Moduleerrors (modül adı yok)**: Azure ML 'de denemeleri gönderirken moduleerrors içinde çalıştırıyorsanız, eğitim betiğinin bir paketin yüklenmesini beklediği ancak eklenmediği anlamına gelir. Paket adı ' nı sağladığınızda, Azure ML paketi eğitim çalıştırınızdan kullanılan ortama yüklenir. 

    Denemeleri göndermek için estimators kullanıyorsanız, paketi `pip_packages` `conda_packages` yüklemek istediğiniz kaynağı temel alarak tahmin aracı 'da veya parametresi aracılığıyla bir paket adı belirtebilirsiniz. Ayrıca, tüm bağımlılıklarınızı kullanarak bir de bir de belirtebilirsiniz `conda_dependencies_file` veya parametresini kullanarak bir txt dosyasındaki tüm PIP gereksinimlerinizi listeleyin `pip_requirements_file` . Tahmin aracı tarafından kullanılan varsayılan görüntüyü geçersiz kılmak istediğiniz bir Azure ML ortamı nesneniz varsa, bu ortamı `environment` tahmin aracı oluşturucusunun parametresi aracılığıyla belirtebilirsiniz.

    Azure ML, TensorFlow, PyTorch, Chainer ve Sköğren için çerçeveye özgü tahminler de sağlar. Bu tahmini kullanımı, çekirdek Framework bağımlılıklarının eğitim için kullanılan ortamda sizin adınıza yüklü olduğundan emin olur. Yukarıda açıklandığı gibi ek bağımlılıklar belirtme seçeneğiniz vardır. 
 
    Azure ML tarafından sağlanan Docker görüntüleri ve içerikleri, [AzureML kapsayıcılarında](https://github.com/Azure/AzureML-Containers)görülebilir.
    Çerçeveye özgü bağımlılıklar ilgili Framework belgelerinde listelenmiştir- [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py&preserve-view=true#&preserve-view=trueremarks), [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py&preserve-view=true#&preserve-view=trueremarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py&preserve-view=true#&preserve-view=trueremarks), [sköğren](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py&preserve-view=true#&preserve-view=trueremarks).

    > [!Note]
    > Belirli bir paketin Azure ML tarafından korunan görüntülere ve ortamlara eklenmek için yeterince yaygın olduğunu düşünüyorsanız, lütfen [AzureML kapsayıcılarında](https://github.com/Azure/AzureML-Containers)GitHub sorununu yükseltin. 
 
* **NameError (ad tanımlı değil), AttributeError (nesne bir özniteliğe sahip değil)**: Bu özel durum eğitim betiklerinden gelmelidir. Tanımlı bir ad veya öznitelik hatası hakkında daha fazla bilgi edinmek için, Azure portal günlük dosyalarına bakabilirsiniz. SDK 'dan, `run.get_details()` hata iletisine bakmak için ' i kullanabilirsiniz. Bu, çalıştırma için oluşturulan tüm günlük dosyalarını da listeler. Lütfen eğitim betiğe göz atın ve çalıştırmayı yeniden göndermeden önce hatayı düzeltemedi. 

* **Horovod kapatıldı**: "AbortedError: Horovod" ile karşılaşırsanız çoğu durumda, bu özel durum Horovod 'nin kapatılmasına neden olan işlemlerden birinde temeldeki özel durum olduğu anlamına gelir. MPı işindeki her bir derecelendirme, Azure ML 'de özel bir günlük dosyası alır. Bu Günlükler adlandırılır `70_driver_logs` . Dağıtılmış eğitim söz konusu olduğunda, `_rank` günlükleri ayırt etmek daha kolay hale getirmek için günlük adlarının ' de sonlanmasını sağlayın. Horovod 'nin kapatılmasına neden olan hatayı tam olarak bulmak için, tüm günlük dosyalarını `Traceback` inceleyin ve driver_log dosyalarının sonundaki bölümüne bakın. Bu dosyalardan biri size gerçek temel özel durumu verecektir. 

* **Çalıştırma veya deneme silme**: denemeleri, [deneme. Arşiv](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment%28class%29?view=azure-ml-py&preserve-view=true#&preserve-view=truearchive--) yöntemi kullanılarak veya Azure Machine Learning Studio istemcisindeki deneme sekmesi görünümünden "Arşiv denemesi" düğmesi aracılığıyla arşivlenebilir. Bu eylem, sorgu ve görünümleri listeleme denemesini gizler, ancak silmez.

    Tek tek denemeleri veya çalıştırmaları kalıcı olarak silme işlemi şu anda desteklenmiyor. Çalışma alanı varlıklarını silme hakkında daha fazla bilgi için bkz. [Machine Learning hizmeti çalışma alanı verilerinizi dışarı veya silme](how-to-export-delete-data.md).

* **Ölçüm belgesi çok büyük**: Azure Machine Learning bir eğitim çalıştırmasında bir kez günlüğe kaydedilebilir ölçüm nesnelerinin boyutunda iç sınırlara sahiptir. Liste değerli bir ölçümü günlüğe kaydederken "ölçüm belgesi çok büyük" hatası ile karşılaşırsanız, listeyi daha küçük parçalara bölmeyi deneyin, örneğin:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    Azure ML, dahili olarak aynı ölçüm adına sahip blokları bitişik bir liste olarak birleştirir.

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

* **Yeni sürümlere yönelik yeni sürümlere yönelik son yükseltme**, önceki paketlerimize sabitlediğimiz eski sürümler arasında uyumsuzluk ve şimdi PIN yaptığımız yeni sürümler arasındaki uyumsuzluk nedeniyle daha eski SDK 'larda, modeller yüklenmeyecek. Şöyle bir hata görürsünüz:
  * Modül bulunamadı: ex. `No module named 'sklearn.decomposition._truncated_svd` ,
  * İçeri aktarma hataları: ex. `ImportError: cannot import name 'RollingOriginValidator'` ,
  * Öznitelik hataları: ex. `AttributeError: 'SimpleImputer' object has no attribute 'add_indicator`
  
  Bu sorunu geçici olarak çözmek için, oto ml SDK eğitim sürümünüze bağlı olarak aşağıdaki iki adımdan birini gerçekleştirin:
  1. Oto ml SDK 'Sı eğitim sürümünüz 1.13.0 büyükse, `pandas == 0.25.1` ve gereklidir `sckit-learn==0.22.1` . Sürüm uyuşmazlığı varsa, aşağıda gösterildiği gibi aşağıdaki sürüme yükselterek scikit-bilgi edinin ve/veya Pandas 'ı yükseltin:
  
  ```bash
     pip install --upgrade pandas==0.25.1
     pip install --upgrade scikit-learn==0.22.1
  ```
  
  2. Oto ml SDK 'Sı eğitim sürümünüz 1.12.0 değerinden küçük veya bu değere eşitse, `pandas == 0.23.4` ve gereklidir `sckit-learn==0.20.3` . Sürüm uyuşmazlığı varsa, scikit-aşağıda gösterildiği gibi doğru sürümü öğrenmek ve/veya Pandas 'yi indirgemeniz gerekir:
  
  ```bash
    pip install --upgrade pandas==0.23.4
    pip install --upgrade scikit-learn==0.20.3
  ```
 
* **Tahmin R2 puanı her zaman sıfırdır**: belirtilen eğitim verileri, son `n_cv_splits`  +  veri noktaları için aynı değeri içeren zaman serisine sahipse bu sorun ortaya çıkar `forecasting_horizon` . Zaman seriniz içinde bu kalıp bekleniyorsa, birincil ölçümünüzün normalleştirilmiş kök ortalama kare hatasına geçiş yapabilirsiniz.
 
* **TensorFlow**: SDK 'nın sürüm 1.5.0 itibariyle, otomatik makine öğrenimi, varsayılan olarak TensorFlow modellerini yüklemez. TensorFlow 'u yüklemek ve otomatik ML denemeleri ile kullanmak için, Conerbağımlılar aracılığıyla TensorFlow = = 1.12.0 ' yi kurun. 
 
   ```python
   from azureml.core.runconfig import RunConfiguration
   from azureml.core.conda_dependencies import CondaDependencies
   run_config = RunConfiguration()
   run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['tensorflow==1.12.0'])
  ```
* **Deneme grafikleri**: otomatik ml denemesi yinelemeleriyle gösterilen ikili sınıflandırma grafikleri (duyarlık-HATıRLA, Roc, kazanç eğrisi vb.), 4/12 ' den beri Kullanıcı arabiriminde doğru işlenmemektedir. Grafik çizimleri Şu anda daha iyi şekilde uygulanan modellerin daha düşük sonuçlarla gösterildiği ters sonuçları gösteriyor. Bir çözüm, araştırma aşamasındadır.

* **Databricks otomatik makine öğrenimi çalıştırmayı iptal et**: Azure Databricks ' de otomatik makine öğrenimi özellikleri kullandığınızda, çalıştırmayı iptal etmek ve yeni bir deneme çalıştırması başlatmak için Azure Databricks kümenizi yeniden başlatın.

* **Databricks otomatik makine öğrenimi için 10 yineleme >**: otomatik makine öğrenimi ayarları 'nda 10 ' dan fazla yineleme sahipseniz, `show_output` `False` çalıştırmayı gönderdiğinizde olarak ayarlayın.

* **Azure MACHINE LEARNING SDK ve otomatik makine öğrenimi Için Databricks pencere öğesi**: Not defterleri HTML pencere öğelerini ayrıştıramadığından, bir databricks not DEFTERINDE Azure Machine Learning SDK pencere öğesi desteklenmez. Azure Databricks Not defteri hücresinizdeki bu python kodunu kullanarak portalda pencere öğesini görüntüleyebilirsiniz:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```
* **automl_setup başarısız olur**: 
    * Windows üzerinde automl_setup bir Anaconda Isteminden çalıştırın. Miniconda yüklemek için [buraya](https://docs.conda.io/en/latest/miniconda.html)tıklayın.
    * Komutunu çalıştırarak Conda 64 bit 'ın, 32 bit yerine, yüklü olduğundan emin olun `conda info` . `platform` `win-64` Windows veya Mac için olmalıdır `osx-64` .
    * Conda 4.4.10 veya üzeri sürümünün yüklü olduğundan emin olun. Komutu ile sürümü kontrol edebilirsiniz `conda -V` . Önceki bir sürümü yüklüyse, şu komutu kullanarak güncelleştirebilirsiniz: `conda update conda` .
    * 'Un `gcc: error trying to exec 'cc1plus'`
      *  `gcc: error trying to exec 'cc1plus': execvp: No such file or directory`Hatayla karşılaşılırsa, komutunu kullanarak derleme Essentials 'ı yüklersiniz `sudo apt-get install build-essential` .
      * Yeni bir Conda ortamı oluşturmak için automl_setup ilk parametre olarak yeni bir ad geçirin. Kullanarak mevcut Conda ortamlarını görüntüleyin `conda env list` ve ile kaldırın `conda env remove -n <environmentname>` .
      
* **automl_setup_linux. sh başarısız**: automl_setup_linus. sh şu hatayla başarısız Ubuntu Linux olur: `unable to execute 'gcc': No such file or directory`-
  1. 53 ve 80 giden bağlantı noktalarının etkinleştirildiğinden emin olun. Azure VM 'de, Azure portalından VM 'yi seçip Ağ ' a tıklayarak bunu yapabilirsiniz.
  2. Şu komutu çalıştırın: `sudo apt-get update`
  3. Şu komutu çalıştırın: `sudo apt-get install build-essential --fix-missing`
  4. `automl_setup_linux.sh`Yeniden çalıştır

* **Configuration. ipynb başarısız olur**:
  * Yerel Conda 'nın automl_setup başarıyla çalıştığından emin olun.
  * Subscription_id doğru olduğundan emin olun. Tüm hizmetler ' i ve ardından abonelikler ' i seçerek Azure portalında subscription_id bulun. "<" ve ">" karakterlerinin subscription_id değere dahil edilmemelidir. Örneğin, `subscription_id = "12345678-90ab-1234-5678-1234567890abcd"` geçerli biçimi vardır.
  * Aboneliğe katkıda bulunan veya sahip erişiminin olduğundan emin olun.
  * Bölgenin desteklenen bölgelerden biri olup olmadığını denetleyin: `eastus2` , `eastus` ,, `westcentralus` `southeastasia` , `westeurope` , `australiaeast` , `westus2` , `southcentralus` .
  * Azure portalını kullanarak bölgeye erişim sağlayın.
  
* **Otomatik Mlconfig içeri aktarma işlemi başarısız oldu**: otomatik makine öğrenimi sürüm 1.0.76 ' de, yeni sürüme güncelleştirmeden önce önceki sürümün kaldırılmasını gerektiren paket değişiklikleri vardı. `ImportError: cannot import name AutoMLConfig`V 1.0.76 'den v 1.0.76 veya üzeri BIR SDK sürümünden yükseltmeden sonra bu hatayla karşılaşırsanız, şunu çalıştırarak hatayı çözün: `pip uninstall azureml-train automl` ve sonra `pip install azureml-train-auotml` . Automl_setup. cmd betiği bunu otomatik olarak yapar. 

* **Workspace.from_config başarısız**: ws = Workspace.from_config () ' çağrıları başarısız olursa-
  1. Configuration. ipynb Not defterinin başarıyla çalıştığından emin olun.
  2. Not defteri, çalıştığı klasör altında olmayan bir klasörden çalıştırıldıysa `configuration.ipynb` , aml_config klasörü ve dosyanın içerdiği config.jsdosyayı yeni klasöre kopyalayın. Workspace.from_config, Not defteri klasörü veya onun üst klasörü için config.jsokur.
  3. Yeni bir abonelik, kaynak grubu, çalışma alanı veya bölge kullanılıyorsa, `configuration.ipynb` Not defterini yeniden çalıştırdığınızdan emin olun. config.jsdoğrudan üzerinde değiştirmek, yalnızca belirtilen abonelik altındaki belirtilen kaynak grubunda çalışma alanı zaten mevcutsa çalışır.
  4. Bölgeyi değiştirmek istiyorsanız, lütfen çalışma alanını, kaynak grubunu veya aboneliği değiştirin. `Workspace.create` , belirtilen bölge farklı olsa da, zaten varsa, bir çalışma alanı oluşturmaz veya güncelleştirmeyecektir.
  
* **Örnek Not defteri başarısız oldu**: örnek bir not defteri, özelliğin, yöntemin veya kitaplığın bulunmadığı bir hata ile başarısız oluyor:
  * Jupyter not defterinde doğru çekirdeğin seçildiğinden emin olun. Çekirdek, Not Defteri sayfasının sağ üst kısmında görüntülenir. Varsayılan değer azure_automl. Çekirdeğin Not defterinin bir parçası olarak kaydedildiğini unutmayın. Bu nedenle, yeni bir Conda ortamına geçerseniz, not defterinde yeni çekirdeği seçmeniz gerekir.
      * Azure Notebooks, Python 3,6 olmalıdır. 
      * Yerel Conda ortamları için, automl_setup ' de belirttiğiniz Conda ortam adı olmalıdır.
  * Not defterinin kullandığınız SDK sürümü için olduğundan emin olun. `azureml.core.VERSION`Jupyter Not defteri hücresinde ÇALıŞTıRARAK SDK sürümünü denetleyebilirsiniz. `Branch`Düğmeye tıklayarak `Tags` ve ardından sürümü seçerek örnek Not defterlerinin önceki sürümünü GitHub 'dan indirebilirsiniz.

* **Windows 'da bir sayısal tuş alma işlemi başarısız oluyor**: bazı Windows ortamları, en son Python sürümü 3.6.8 ile bir sayısal tuş, yükleme hatası görür. Bu sorunu görürseniz Python sürüm 3.6.7 ile deneyin.

* **Sayısal tuş takımı içeri aktarma başarısız**: otomatik ml Conda ortamındaki TensorFlow sürümünü denetleyin. Desteklenen sürümler < 1,13 ' dir. Sürüm >= 1,13 olduğunda TensorFlow 'un ortamdan kaldırılması, TensorFlow sürümünü kontrol edebilir ve aşağıdaki gibi kaldırabilirsiniz.
  1. Bir komut kabuğu başlatın, otomatik ml paketlerinin yüklendiği Conda ortamını etkinleştirin.
  2. `pip freeze` `tensorflow` Bulunursa, listelenen sürüm < 1,13 olmalıdır.
  3. Listelenen sürüm desteklenen bir sürüm değilse, `pip uninstall tensorflow` komut kabuğu 'nda, onay için y girin.

## <a name="deploy--serve-models"></a>Modelleri dağıtma ve sunma

Aşağıdaki hatalar için bu eylemleri gerçekleştirin:

|Hata  | Çözüm  |
|---------|---------|
|Web hizmeti dağıtımında görüntü oluşturma hatası     |  Görüntü yapılandırması için Conda dosyasına bir zar bağımlılığı olarak "pynacl = = 1.2.1" ekleyin       |
|`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`     |   Dağıtımınızda kullanılan VM 'Ler için SKU 'YU daha fazla belleğe sahip olan bir şekilde değiştirin. |
|FPGA hatası     |  FPGA kotası istenene ve onaylanana kadar, Fpg' de modeller dağıtacaksınız. Erişim istemek için kota isteği formunu doldurun: https://aka.ms/aml-real-time-ai       |

### <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>AKS kümesindeki Azure Machine Learning bileşenleri güncelleştiriliyor

Azure Kubernetes hizmet kümesinde yüklü Azure Machine Learning bileşenlere yapılan güncelleştirmeler el ile uygulanmalıdır. 

Kümeyi Azure Machine Learning çalışma alanından ayırarak ve sonra kümeyi çalışma alanına yeniden açarak bu güncelleştirmeleri uygulayabilirsiniz. Kümede TLS etkinse, kümeyi yeniden eklerken TLS/SSL sertifikasını ve özel anahtarı sağlamanız gerekir. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Artık TLS/SSL sertifikasına ve özel anahtara sahip değilseniz veya Azure Machine Learning tarafından oluşturulan bir sertifika kullanıyorsanız, küme kullanımdan çıkarmadan önce, gizli anahtarı kullanarak kümeye bağlanarak dosyaları alabilirsiniz `kubectl` `azuremlfessl` .

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes gizli dizileri temel-64 kodlu biçimde depolar. Ana 64 'nin ve içindeki parolaların bileşenlerinin kodunu vermeden önce kodu çözmelisiniz `cert.pem` `key.pem` `attach_config.enable_ssl` . 

### <a name="detaching-azure-kubernetes-service"></a>Azure Kubernetes hizmeti ayrılıyor

Bir AKS kümesini ayırmak için Machine Learning 'in Azure Machine Learning Studio, SDK veya Azure CLı uzantısının kullanılması AKS kümesini silmez. Kümeyi silmek için bkz. [Azure CLI 'Yı AKS Ile kullanma](/azure/aks/kubernetes-walkthrough#delete-the-cluster).

### <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes hizmeti hatalarında WebServices

Azure Kubernetes hizmetindeki birçok Web hizmeti hatası, kullanılarak kümeye bağlanarak hata ayıklanabilir `kubectl` . `kubeconfig.json`Bir Azure Kubernetes hizmet kümesi için şunu çalıştırarak edinebilirsiniz

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir işten bir işlem hedefinde yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız: 

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, uzaktan yürütme için gönderilen bir ML ardışık düzeninde bir işlem hedefi oluşturmaya veya eklemeye çalıştığınızda bir hata alırsınız.

## <a name="missing-user-interface-items-in-studio"></a>Studio 'da eksik kullanıcı arabirimi öğeleri

Azure rol tabanlı erişim denetimi, Azure Machine Learning gerçekleştirebileceğiniz eylemleri kısıtlamak için kullanılabilir. Bu kısıtlamalar, Kullanıcı arabirimi öğelerinin Azure Machine Learning Studio 'da gösterilmesini engelleyebilir. Örneğin, bir işlem örneği oluşturmayan bir rol atanırsa, işlem örneği oluşturma seçeneği Studio 'da görünmez.

Daha fazla bilgi için bkz. [kullanıcıları ve rolleri yönetme](how-to-assign-roles.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için diğer sorun giderme makalelerine bakın:

* [Azure Machine Learning ile Docker dağıtımı sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Machine Learning işlem hatlarında hata ayıkla](how-to-debug-pipelines.md)
* [Azure Machine Learning SDK 'dan ParallelRunStep sınıfında hata ayıklama](how-to-debug-parallel-run-step.md)
* [VS Code ile bir makine öğrenimi işlem örneğinde etkileşimli hata ayıklama](how-to-debug-visual-studio-code.md)
* [Machine Learning işlem hatları hatalarını ayıklamak için Application Insights kullanın](how-to-debug-pipelines-application-insights.md)
