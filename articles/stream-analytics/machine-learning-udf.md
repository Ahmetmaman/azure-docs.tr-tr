---
title: Azure Stream Analytics Azure Machine Learning ile tümleştirin
description: Bu makalede, bir Azure Stream Analytics işinin Azure Machine Learning modelleriyle nasıl tümleştirileceği açıklanır.
author: sidramadoss
ms.author: sidram
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/21/2020
ms.custom: devx-track-js
ms.openlocfilehash: c35d5d2f63f4a7abe80a0ff19e5994013355c386
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98020528"
---
# <a name="integrate-azure-stream-analytics-with-azure-machine-learning-preview"></a>Azure Stream Analytics Azure Machine Learning ile tümleştirme (Önizleme)

Akış girişi verilerinizde gerçek zamanlı Puanlama ve tahmin işlemleri yapmak için Azure Stream Analytics işlerinizde Kullanıcı tanımlı bir işlev (UDF) olarak makine öğrenimi modelleri uygulayabilirsiniz. [Azure Machine Learning](../machine-learning/overview-what-is-azure-ml.md) , modellerinizi geliştirmek, eğitme ve dağıtmak Için TensorFlow, scikit-öğren veya PyTorch gibi popüler açık kaynak araçlarından birini kullanmanıza olanak sağlar.

## <a name="prerequisites"></a>Önkoşullar

Machine Learning modelini Stream Analytics işinize bir işlev olarak eklemeden önce aşağıdaki adımları uygulayın:

1. [Modelinizi bir Web hizmeti olarak dağıtmak](../machine-learning/how-to-deploy-and-where.md)için Azure Machine Learning kullanın.

2. Puanlama betiğinizin bir şema belirtimi oluşturmak için Azure Machine Learning tarafından kullanılan [örnek girişler ve çıktılar](../machine-learning/how-to-deploy-and-where.md) olması gerekir. Stream Analytics, Web hizmetinizin işlev imzasını anlamak için şemayı kullanır. Bu [örnek Swagger tanımını](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/AzureML/swagger-example.json) , doğru ayarlamış olduğunuzdan emin olmak için bir başvuru olarak kullanabilirsiniz.

3. Web hizmetinizin JSON seri hale getirilmiş verileri kabul ettiğinden ve döndürdüğünden emin olun.

4. Modelinizi [Azure Kubernetes hizmetinde](../machine-learning/how-to-deploy-and-where.md#choose-a-compute-target) yüksek ölçekli üretim dağıtımları için dağıtın. Web hizmeti, işinizden gelen istek sayısını işleyemediğinde, Stream Analytics işinizin performansı düşecek ve bu da gecikmeyi etkiler. Azure Container Instances dağıtılan modeller yalnızca Azure portal kullandığınızda desteklenir. [Azure Machine Learning Designer](../machine-learning/concept-designer.md) kullanılarak oluşturulan modeller henüz Stream Analytics desteklenmemektedir.

## <a name="add-a-machine-learning-model-to-your-job"></a>İşinize bir makine öğrenme modeli ekleyin

Stream Analytics işinize doğrudan Azure portal veya Visual Studio Code Azure Machine Learning işlevleri ekleyebilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. Azure portal Stream Analytics işinize gidin ve **iş topolojisi** altında **işlevler** ' i seçin. Ardından, + açılan menü **Ekle** menüsünden **Azure Machine Learning hizmet** ' i seçin.

   ![Azure Machine Learning UDF ekleyin](./media/machine-learning-udf/add-azure-machine-learning-udf.png)

2. **Azure Machine Learning hizmeti işlev** formunu aşağıdaki özellik değerleriyle doldur:

   ![Azure Machine Learning UDF yapılandırma](./media/machine-learning-udf/configure-azure-machine-learning-udf.png)

### <a name="visual-studio-code"></a>Visual Studio Code

1. Stream Analytics projenizi Visual Studio Code açın ve **işlevler** klasörüne sağ tıklayın. Sonra **Işlev Ekle**' yi seçin. Açılan listeden **MACHINE LEARNING UDF** ' i seçin.

   :::image type="content" source="media/machine-learning-udf/visual-studio-code-machine-learning-udf-add-function.png" alt-text="VS Code UDF ekleme":::

   :::image type="content" source="media/machine-learning-udf/visual-studio-code-machine-learning-udf-add-function-2.png" alt-text="VS Code Azure Machine Learning UDF ekleyin":::

2. İşlev adını girin ve CodeLens 'teki **aboneliklerinizden Seç** ' i kullanarak yapılandırma dosyasındaki ayarları doldurabilirsiniz.

   :::image type="content" source="media/machine-learning-udf/visual-studio-code-machine-learning-udf-function-name.png" alt-text="VS Code Azure Machine Learning UDF seçin":::

   :::image type="content" source="media/machine-learning-udf/visual-studio-code-machine-learning-udf-configure-settings.png" alt-text="VS Code Azure Machine Learning UDF yapılandırma":::

Aşağıdaki tabloda, Stream Analytics Azure Machine Learning hizmet işlevlerinin her bir özelliği açıklanmaktadır.

|Özellik|Açıklama|
|--------|-----------|
|İşlev diğer adı|Sorgunuzun işlevini çağırmak için bir ad girin.|
|Abonelik|Azure aboneliğiniz..|
|Azure Machine Learning çalışma alanı|Modelinizi bir Web hizmeti olarak dağıtmak için kullandığınız Azure Machine Learning çalışma alanı.|
|Dağıtımlar|Modelinizi barındıran Web hizmeti.|
|İşlev imzası|API 'nin şema belirtiminden çıkarılan Web hizmetinizin imzası. İmzanız yüklenemezse, otomatik olarak şemayı oluşturmak için Puanlama betiğinizdeki örnek giriş ve çıkış sağladıysanız emin olun.|
|Bölüm başına paralel istek sayısı|Bu, yüksek ölçekli üretilen işi iyileştirmek için gelişmiş bir yapılandırmadır. Bu sayı, işinizin her bir bölümünden Web hizmetine gönderilen eşzamanlı istekleri temsil eder. Altı akış birimi (SU) ve alt düzeyde olan işlerin bir bölümü vardır. 12 SUs içeren işlerin iki bölümü vardır, 18 SUs üç bölüme sahiptir ve bu şekilde devam eder.<br><br> Örneğin, işiniz iki bölümden oluşabilir ve bu parametreyi dört olarak ayarlarsanız, işiniz Web hizmetinize kadar olan sekiz eşzamanlı istek olacaktır. Bu genel önizleme sırasında, bu değerin varsayılan değeri 20 ' dir ve güncelleştirilemez.|
|En fazla toplu iş sayısı|Bu, yüksek ölçekli üretilen işi iyileştirmeye yönelik gelişmiş bir yapılandırmadır. Bu sayı, Web hizmetinize gönderilen tek bir istekte birlikte toplanmış en fazla olay sayısını temsil eder.|

## <a name="supported-input-parameters"></a>Desteklenen giriş parametreleri

Stream Analytics sorgunuz bir Azure Machine Learning UDF 'yi istediğinde, iş Web hizmetine JSON serileştirilmiş istek oluşturur. İstek, modele özgü bir şemaya dayalıdır. [Otomatik olarak bir şema oluşturmak](../machine-learning/how-to-deploy-and-where.md)için Puanlama betiğinizdeki bir örnek giriş ve çıkış sağlamanız gerekir. Şema, Stream Analytics, sayısal tuş y, Pandas ve PySpark gibi desteklenen veri türlerinden herhangi biri için JSON serileştirilmiş istek oluşturulmasına olanak sağlar. Birden çok giriş olayı tek bir istekte birlikte toplu olarak oluşturulabilir.

Aşağıdaki Stream Analytics sorgusu, bir Azure Machine Learning UDF çağırma örneğidir:

```SQL
SELECT udf.score(<model-specific-data-structure>)
INTO output
FROM input
```

Stream Analytics, yalnızca Azure Machine Learning işlevleri için bir parametre geçirmeyi destekler. Machine Learning UDF 'ye giriş olarak geçirmeden önce verilerinizi hazırlamanız gerekebilir. Null girişler işin başarısız olmasına neden olacağı için ML UDF girişinin null olmadığından emin olmanız gerekir.

## <a name="pass-multiple-input-parameters-to-the-udf"></a>UDF 'ye birden çok giriş parametresi geçirme

Makine öğrenimi modellerine yapılan girişlerin en yaygın örnekleri, sayısal olarak y dizileri ve veri çerçevelerdir. JavaScript UDF kullanarak bir dizi oluşturabilir ve yan tümcesini kullanarak JSON serileştirilmiş bir veri çerçevesi oluşturabilirsiniz `WITH` .

### <a name="create-an-input-array"></a>Giriş dizisi oluşturma

*N* sayıda girişi kabul eden BIR JavaScript UDF oluşturabilir ve Azure Machine Learning UDF 'nize giriş olarak kullanılabilecek bir dizi oluşturabilirsiniz.

```javascript
function createArray(vendorid, weekday, pickuphour, passenger, distance) {
    'use strict';
    var array = [vendorid, weekday, pickuphour, passenger, distance]
    return array;
}
```

İşinize JavaScript UDF 'sini ekledikten sonra, aşağıdaki sorguyu kullanarak Azure Machine Learning UDF 'nizi çağırabilirsiniz:

```SQL
WITH 
ModelInput AS (
#use JavaScript UDF to construct array that will be used as input to ML UDF
SELECT udf.createArray(vendorid, weekday, pickuphour, passenger, distance) as inputArray
FROM input
)

SELECT udf.score(inputArray)
INTO output
FROM ModelInput
#validate inputArray is not null before passing it to ML UDF to prevent job from failing
WHERE inputArray is not null
```

Aşağıdaki JSON bir örnek istedir:

```JSON
{
    "data": [
        ["1","Mon","12","1","5.8"],
        ["2","Wed","10","2","10"]
    ]
}
```

### <a name="create-a-pandas-or-pyspark-dataframe"></a>Pandas veya PySpark DataFrame oluşturma

`WITH`Yan tümcesini kullanarak aşağıda gösterildiği gibi Azure MACHINE LEARNING UDF 'nize girdi olarak GEÇIRILEBILECEK JSON serileştirilmiş bir veri çerçevesi oluşturabilirsiniz.

Aşağıdaki sorgu, gerekli alanları seçerek ve veri çerçevesini Azure Machine Learning UDF için girdi olarak kullanan bir DataFrame oluşturur.

```SQL
WITH 
Dataframe AS (
SELECT vendorid, weekday, pickuphour, passenger, distance
FROM input
)

SELECT udf.score(Dataframe)
INTO output
FROM Dataframe
```

Aşağıdaki JSON, önceki sorgudan gelen bir istek örneğidir:

```JSON
{
    "data": [{
            "vendorid": "1",
            "weekday": "Mon",
            "pickuphour": "12",
            "passenger": "1",
            "distance": "5.8"
        }, {
            "vendorid": "2",
            "weekday": "Tue",
            "pickuphour": "10",
            "passenger": "2",
            "distance": "10"
        }
    ]
}
```

## <a name="optimize-the-performance-for-azure-machine-learning-udfs"></a>Azure Machine Learning UDF 'Leri için performansı iyileştirme

Modelinizi Azure Kubernetes hizmetine dağıttığınızda, [kaynak kullanımını tespit etmek için modelinizin profilini](../machine-learning/how-to-deploy-profile-model.md)oluşturabilirsiniz. Ayrıca, istek hızlarını, yanıt sürelerini ve hata hızlarını anlamak için [dağıtımlarınız Için App Insights 'ı etkinleştirebilirsiniz](../machine-learning/how-to-enable-app-insights.md) .

Yüksek olay işleme içeren bir senaryonuz varsa, düşük uçtan uca gecikme süreleriyle en iyi performansı elde etmek için Stream Analytics aşağıdaki parametreleri değiştirmeniz gerekebilir:

1. En fazla yığın sayısı.
2. Bölüm başına paralel istek sayısı.

### <a name="determine-the-right-batch-size"></a>Doğru toplu iş boyutunu belirleme

Web hizmetinizi dağıttıktan sonra, 50 'den başlayan ve yüzlerce sırada artırarak değişen toplu iş boyutlarına örnek istek gönderirsiniz. Örneğin, 200, 500, 1000, 2000 vb. Belirli bir toplu iş boyutundan sonra yanıtın gecikme süresinin arttığına dikkat edin. Yanıt gecikme süresinin arttığı nokta, işiniz için en fazla toplu iş sayısı olmalıdır.

### <a name="determine-the-number-of-parallel-requests-per-partition"></a>Bölüm başına paralel istek sayısını belirleme

En uygun ölçekleme sırasında, Stream Analytics işiniz Web hizmetinize birden çok paralel istek gönderebilmelidir ve birkaç milisaniye içinde yanıt alamaz. Web hizmeti yanıtının gecikmesi, Stream Analytics işinizin gecikme ve performansını doğrudan etkileyebilir. İşinizden Web hizmetine yapılan çağrı uzun sürerse, büyük olasılıkla bir gecikme süresi gecikmede bir artış görürsünüz ve ayrıca biriktirme listesindeki giriş olaylarının sayısında bir artış görebilirsiniz.

Bu gecikme süresini engellemek için, Azure Kubernetes hizmeti (AKS) kümenizin [doğru sayıda düğüm ve çoğaltmalarla](../machine-learning/how-to-deploy-azure-kubernetes-service.md#using-the-cli)sağlanmış olduğundan emin olun. Web hizmetinizin yüksek oranda kullanılabilir olması ve başarılı yanıtları döndürmesi önemlidir. İşiniz Web hizmetinizden bir hizmet kullanılamıyor yanıtı (503) alırsa, üstel geri alma ile sürekli olarak yeniden dener. Başarılı (200) ve hizmet kullanılamıyor (503) dışındaki herhangi bir yanıt, işinizin başarısız durumuna geçmesine neden olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Stream Analytics JavaScript kullanıcı tanımlı işlevleri](stream-analytics-javascript-user-defined-functions.md)
* [Stream Analytics işinizi Azure Machine Learning Studio (klasik) işlevle ölçeklendirin](stream-analytics-scale-with-machine-learning-functions.md)
