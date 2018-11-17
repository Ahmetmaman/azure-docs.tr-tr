---
title: Tek bir deneyden birden çok modeller oluşturun | Microsoft Docs
description: Birden çok makine öğrenimi modelleri ve web hizmeti uç noktaları aynı algoritmayı ancak farklı bir eğitim veri kümeleri oluşturmak için PowerShell kullanın.
services: machine-learning
documentationcenter: ''
author: hning86
ms.custom: (previous ms.author haining)
ms.author: amlstudiodocs
manager: mwinkle
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.openlocfilehash: e7ed548ac0561c023a6287b8d70f19c4d0e4ef3e
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51820554"
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>PowerShell kullanarak bir denemeden çok sayıda Machine Learning modeli ve web hizmeti uç noktası oluşturma
Bir ortak makine öğrenimi sorunu şu şekildedir: aynı eğitim iş akışı, sahip ve aynı algoritmayı kullanan çok sayıda model oluşturmak istiyorsunuz. Ancak giriş olarak farklı bir eğitim veri kümesi olmasını istersiniz. Bu makalede tek bir denemede kullanarak uygun ölçekte Azure Machine Learning Studio'da bunun nasıl yapılacağını gösterir.

Örneğin, bir genel bisiklet kiralama franchise işletme sahibi varsayalım. Geçmiş verileri temel alan kiralama talep tahmin etmek için regresyon modeli oluşturmak istiyorsunuz. Dünya genelinde 1.000 kiralama konumları vardır ve her konum için bir veri kümesi derledik. Bunlar, tarih, saat, hava durumu ve trafiği gibi her bir konuma özgü önemli özellikleri içerir.

Bir kez tüm konumlar arasında tüm veri kümelerini birleştirilmiş bir sürümünü kullanarak modelinizi eğitmek. Ancak, konumların her biri benzersiz bir ortamı sahip. Bu nedenle ayrı ayrı her konum için veri kümesini kullanarak, regresyon modeli eğitmek için daha iyi bir yaklaşım olacaktır. Bu şekilde, her eğitilen model dikkate farklı depolama boyutları, birim, Coğrafya, nüfus, bisiklet dostu trafiği ortam ve daha fazla sürebilir.

En iyi yaklaşım olabilir, ancak Azure Machine Learning'de her bir benzersiz bir konumu temsil eden 1.000 eğitim denemeleri oluşturmak istemediğiniz. Her deneme aynı bileşenleri eğitim veri kümesi dışında olacağından zor bir görev olmasının yanı sıra, ayrıca verimsiz hatırlıyorum.

Neyse ki, bunu kullanarak gerçekleştirebilirsiniz [Azure Machine Learning yeniden eğitme API](retrain-models-programmatically.md) ve görev ile otomatikleştirme [Azure Machine Learning PowerShell](powershell-module.md).

> [!NOTE]
> Örneğinizi daha hızlı çalışır hale getirmek için 1000 konumlardan 10 sayısını azaltın. Ancak aynı ilke ve yordamlar 1.000 konumları için geçerlidir. Ancak, 1.000 veri kümelerinden eğitmek istiyorsanız, paralel olarak aşağıdaki PowerShell komut dosyalarını çalıştırmak isteyebilirsiniz. Bunu nasıl yapacağınız bu makalenin kapsamı dışındadır, ancak PowerShell örneklerini çoklu iş parçacığı Internet'te bulabilirsiniz.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Eğitim deneme ayarlama
Örnek [eğitim denemesini](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) alanında [Cortana Intelligence Galerisi](http://gallery.cortanaintelligence.com). Bu deneme açın, [Azure Machine Learning Studio](https://studio.azureml.net) çalışma.

> [!NOTE]
> Bu örnek ile birlikte izlemek için ücretsiz bir çalışma alanı yerine standart çalışma kullanmak isteyebilirsiniz. Toplam 10 uç noktalar - için - her müşteri için bir uç nokta oluşturun ve ücretsiz bir çalışma alanı 3 uç noktalar ile sınırlı olduğundan, standart bir çalışma alanı gerektirir. Yalnızca bir ücretsiz çalışma alanı varsa, komut yalnızca th konumlar için izin verecek şekilde değiştirmeniz yeterlidir.
> 
> 

Denemeyi kullanan bir **Veri Al** eğitim veri kümesi içeri aktarmak için modül *customer001.csv* bir Azure depolama hesabından. Tüm bisiklet kiralama konumlardan toplanan eğitim veri kümeleri ve bunların arasında değişen dosya adlarına sahip aynı blob depolama konumunda depolanan varsayalım *rentalloc001.csv* için *rentalloc10.csv*.

![image](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Unutmayın bir **Web hizmeti çıkış** modülü eklendi **modeli eğitme** modülü.
Bu deneyde, bir web hizmeti olarak dağıtıldığında, uç nokta çıkış eğitilen model .ilearner dosya biçiminde döndürür ilişkili.

Ayrıca URL tanımlayan bir web hizmeti parametresini ayarlamak unutmayın, **verileri içeri aktarma** modülü kullanır. Bu, her konum için modeli eğitmek için tek bir eğitim veri kümesi belirtmek için parametreyi kullanmanıza olanak sağlar.
Bu tamamlayabilirdik farklı yöntemleri vardır. Bir SQL Azure veritabanı'ndan veri almak için bir web hizmeti parametresi ile bir SQL sorgusu kullanabilirsiniz. Ya da bir **Web hizmeti girişini** modülü bir veri kümesinde web hizmetine geçirilecek.

![image](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Şimdi, şimdi varsayılan değerini kullanarak bu eğitim denemesini çalıştırma *rental001.csv* eğitim veri kümesi olarak. Çıkışı görüntülerseniz **değerlendir** Modülü (çıkış ve Seç'e tıklayın **Görselleştir**), iyi bir performans elde etmenize gördüğünüz *AUC* 0.91 =. Bu noktada, bu eğitim denemesini dışında bir web hizmeti dağıtmaya hazır olursunuz.

## <a name="deploy-the-training-and-scoring-web-services"></a>Eğitim ve puanlama web hizmetlerini dağıtma
Eğitim web hizmeti dağıtmak için **Web hizmetinin ayarı** deneme tuvalinin ' düğmesine tıklayın ve belirleyin **Web hizmeti Dağıt**. Bu web hizmeti "Bisiklet kiralama Eğitim" çağırın.

Şimdi Puanlama web hizmeti dağıtmak gerekir.
Bunu yapmak için tıklatın **Web hizmetinin ayarı** seçin ve tuval aşağıda **Tahmine dayalı Web hizmeti**. Bu, bir Puanlama deneme oluşturur.
Bir web hizmeti olarak çalışması için birkaç küçük ayarlamalar yapmanız gerekir. "Cnt" etiket sütunu giriş verileri kaldırın ve çıkış yalnızca örnek kimliği ve karşılık gelen tahmin edilen değer sınırlayabilirsiniz.

Bu iş kendiniz kaydetmek için açabilirsiniz [Tahmine dayalı denemeye](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) galerisinde zaten hazırlandı.

Web hizmetini dağıtmak için Tahmine dayalı denemeye çalıştırın, ardından **Web hizmeti Dağıt** tuvalin altındaki düğme. Puanlama web hizmeti "Bisiklet kiralama Puanlama" olarak adlandırın.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>PowerShell ile 10 aynı web hizmeti uç noktası oluşturma
Bu web hizmeti bir varsayılan uç nokta ile birlikte gelir. Ancak bu yana güncelleştirilemiyor varsayılan uç nokta olarak değil. Yapmanız gerekenler 10 ek uç noktalar, her konum için bir tane oluşturmaktır. PowerShell ile bunu yapabilirsiniz.

İlk olarak, PowerShell ortamı ayarlayın:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Ardından, aşağıdaki PowerShell komutunu çalıştırın:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Artık 10 uç noktası oluşturuldu ve bunların tümü aynı eğitilmiş içeren üzerinde modeli eğitilir *customer001.csv*. Bunları Azure portalında görüntüleyebilirsiniz.

![image](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>PowerShell kullanarak bir eğitim veri kümeleri kullanılacak uç noktalarını güncelleştir
Sonraki adım her müşterinin tek tek veri benzersiz olarak eğitilen modelleri ile uç noktaları güncelleştirmektir. Ancak bu modellerinden üretmek gereken ilk **bisiklet kiralama eğitim** web hizmeti. Geri dönelim **bisiklet kiralama eğitim** web hizmeti. BES bitim 10 kez 10 farklı bir eğitim veri kümeleri ile 10 farklı modelleri oluşturmak için çağırmanız gerekir. Kullanım **InovkeAmlWebServiceBESEndpoint** Bunu yapmak için PowerShell cmdlet'i.

Blob depolama hesabınız için kimlik bilgilerini sağlamanız gerekecektir `$configContent`. Yani, alanları, `AccountName`, `AccountKey`, ve `RelativeLocation`. `AccountName` Hesap adlarınızla biri görüldüğü olabilir **Azure portalında** (*depolama* sekmesi). Bir depolama hesabı,'a tıkladığınızda, `AccountKey` tuşlarına basarak bulunabilir **erişim anahtarlarını Yönet** düğmesinin altındaki ve kopyalama *birincil erişim anahtarı*. `RelativeLocation` Yeni bir modeli depolanacağı depolama alanınızı göreli bir yol olduğu. Örneğin, yol `hai/retrain/bike_rental/` adlı bir kapsayıcı için aşağıdaki betiği nokta `hai`, ve `/retrain/bike_rental/` alt. Şu anda, alt klasör portal kullanıcı Arabirimi üzerinden oluşturamazsınız ancak vardır [çeşitli Azure depolama gezginleri](../../storage/common/storage-explorers.md) bunu izin verir. Depolamanızda gibi yeni eğitilen modeller (.iLearner dosyaları) depolamak için yeni bir kapsayıcı oluşturmak önerilir: depolama sayfanızdan tıklayın **Ekle** düğmesinin altındaki ve adlandırın `retrain`. Özet olarak, aşağıdaki komut dosyası için gerekli değişiklikleri ilgilidir `AccountName`, `AccountKey`, ve `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> BES uç nokta bu işlem için desteklenen tek moddur. RRS eğitilen modelleri üretmek için kullanılamaz.
> 
> 

10 farklı BES iş yapılandırma json dosyalarını oluşturmak yerine, yukarıdaki görebileceğiniz gibi dinamik olarak yapılandırma dizesi yerine oluşturun. Kendisine akış *jobConfigString* parametresinin **InvokeAmlWebServceBESEndpoint** cmdlet'i. Gerçekten diskte bir kopyasını tutmaya gerek yoktur.

Her şey yolunda giderse, bir süre sonra 10 .iLearner dosya gelen görmelisiniz *model001.ilearner* için *model010.ilearner*, Azure depolama hesabınızdaki. Web hizmeti uç noktalarını kullanarak bu modelleri ile Puanlama 10 güncelleştirmeye hazır artık **Patch-AmlWebServiceEndpoint** PowerShell cmdlet'i. Program aracılığıyla daha önce oluşturduğunuz varsayılan olmayan uç noktaları yalnızca düzeltme yeniden unutmayın.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Bu oldukça hızlı bir şekilde çalışması gerekir. Yürütme sona erdiğinde, başarıyla oluşturuldu 10 Tahmine dayalı web hizmeti uç noktası. Her biri benzersiz bir tek eğitim denemesini tümünden kiralama konuma belirli veri kümesi eğitilmiş eğitilen bir modelin içerir. Bunu doğrulamak için kullanarak bu uç noktalarına çağrı yapma deneyebilirsiniz **InvokeAmlWebServiceRRSEndpoint** cmdlet'i, bunları aynı girdi verileriyle sağlama. Modelleri farklı eğitim kümeleriyle eğitilir olduğundan farklı tahmin sonuçlarını görmek beklemeniz gerekir.

## <a name="full-powershell-script"></a>Tam PowerShell Betiği
Tam kaynak kodu listesi aşağıda verilmiştir:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
