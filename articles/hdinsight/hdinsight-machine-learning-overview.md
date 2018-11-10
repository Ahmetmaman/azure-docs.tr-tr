---
title: Machine learning'e genel bakış - Azure HDInsight
description: Makine öğrenimi HDInsight seçenekleri açıklar.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: hrasheed
ms.openlocfilehash: 2ac108d65b42221189e50987238ad3d7edad1e30
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005351"
---
# <a name="machine-learning-on-hdinsight"></a>Machine learning üzerinde HDInsight

HDInsight, machine learning ile büyük miktarda yapılandırılmış, yapılandırılmamış, (petabaytlarca veya hatta eksabayt) değerli Öngörüler elde etme yeteneği sağlayan büyük veri ve hızla değişen verileri sağlar. HDInsight içinde birden çok makine öğrenimi seçenekleri vardır: SparkML ve MLlib, R, Hive ve Microsoft Bilişsel Araç Seti.

## <a name="sparkml-and-mllib"></a>SparkML ve MLlib

[HDInsight Spark](spark/apache-spark-overview.md) , Azure'da barındırılan bir teklifi [Spark](http://spark.apache.org/), birleşik, açık kaynaklı, büyük veri analizi artırmak üzere bellek içi işlemeyi destekleyen paralel veri işleme çerçevesi. Spark işleme altyapısı hız, kullanım kolaylığı ve Gelişmiş analiz için oluşturulmuştur. Spark'ın dağıtılmış bellek içi hesaplama özellikleri onu kullanılan makine öğrenimi ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim haline getirir. Dağıtılmış bu ortamı algoritmik modelleme özellikleri getirin iki ölçeklenebilir makine öğrenimi kitaplığı: MLlib ve SparkML. MLlib Rdd üzerinde oluşturulan özgün API içerir. SparkML ML işlem hatları oluşturmak için veri çerçevelerini üzerinde oluşturulan üst düzey bir API sağlayan yeni bir pakettir. SparkML MLlib özelliklerinin tümünü henüz olarak desteklemez, ancak MLlib Spark'ın standart machine learning kitaplığı değiştiriyor.

Apache Spark için Microsoft Machine Learning Kitaplığı [MMLSpark](https://github.com/Azure/mmlspark). Bu kitaplık, veri bilimcileri üzerinde Spark daha verimli hale, deneme oranını artırmak ve son teknoloji ürünü makine öğrenimi teknikleri, çok büyük veri kümeleri üzerinde derin öğrenme dahil olmak üzere yararlanmak için tasarlanmıştır. MMLSpark dizin dizeleri gibi ölçeklenebilir ML modelleri oluşturma sırasında alt düzey API'ler SparkML'ın üzerinde bir katman zorlama öğrenme algoritmaları ve özellik vektör derleyerek makine tarafından beklenen bir düzen halinde veri sağlar. Bunlar ve diğer ortak görevler PySpark modeller oluşturmak için MMLSpark kitaplığı basitleştirir.

## <a name="r"></a>R

[R](https://www.r-project.org/) şu anda dünyanın en popüler istatistiksel programlama dilinde olduğu. Bu bir açık kaynak veri görselleştirme ile 2,5 milyondan kullanıcıların ve büyüyen bir topluluk aracıdır. Kendi üzerinde kullanıcı tabanınızı ve 8.000 katkıda bulunulan paketleri ile R machine learning gereken birçok şirketler için büyük olasılıkla bir seçimdir. ML Hizmetleri ile yüksek hacimli veri kümeleri ve modelleri kullanıma hazır olan bir HDInsight kümesi oluşturabilirsiniz. Bu özellik, veri bilimcileri ve tanıdık bir R arabirimiyle ölçeklendirilebilir istatistikçiler sağlar isteğe bağlı HDInsight, aracılığıyla küme kurulum ve Bakım yükü olmadan.

![R server ile tahmini için eğitim](./media/hdinsight-machine-learning-overview/r-training.png)

Bir küme kenar düğümüne kümeye bağlanın ve, R betikleri çalıştırmak için uygun bir yer sağlar.  ScaleR'ın Hadoop Map Reduce kullanarak tüm küme düğümlerine R betikleri çalıştırma seçeneği de veya Spark işlem bağlamlarının.

HDInsight Spark ile şirket ML Hizmetleri ile bir Spark işlem bağlamına kullanarak tüm küme düğümlerine eğitim getirebilen. Tüm mevcut çekirdekler gerektiği şekilde paralel olarak kullanarak doğrudan uç düğümde R betiklerini çalıştırabilirsiniz. Alternatif olarak, kümedeki tüm düğümlere dağıtılmasını işleme hız kazandırın için edge düğümünden kodunuzu çalıştırabilirsiniz. HDInsight Spark ile ML Hizmetleri paralelleştirmek işlevlerini açık kaynak R paketleri, isterseniz de sağlar.

## <a name="azure-machine-learning-and-hive"></a>Azure Machine Learning ve Hive

Azure Machine Learning, Tahmine dayalı analiz modeli yanı sıra, Tahmine dayalı Modellerinizi-hazır web Hizmetleri olarak dağıtmada kullanabileceğiniz tam olarak yönetilen bir hizmet için araçlar sağlar. Azure Machine Learning, oluşturmak, test, kullanıma hazır hale getirme ve Tahmine dayalı modelleri yönetmek için kullanabileceğiniz bulutta kapsamlı Tahmine dayalı analiz çözümüdür. Büyük bir algoritma kitaplığından tercih modelleri oluşturmak için bir web tabanlı studio kullanın ve modelinizi bir web hizmeti olarak kolayca dağıtın.

![Gelişmiş analiz için Hadoop, Microsoft Azure Machine Learning ile erişilebilir yapma](./media/hdinsight-machine-learning-overview/hadoop-azure-ml.png)

Özellikleri kullanarak bir HDInsight Hadoop veri kümesi için oluşturma [Hive sorguları](../machine-learning/team-data-science-process/create-features-hive.md). *Özellik Mühendisliği* öğrenme süreci ham verilerden özellikler oluşturarak öğrenimi algoritmaları, Tahmine dayalı power artırmak çalışır. Azure ML HiveQL sorgu çalıştırmak ve Hive işlenmesi ve kullanarak blob depolama alanında depolanan verilere [verileri içeri aktarma modülü](../machine-learning/studio/import-data.md).

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

[Derin öğrenme](https://www.microsoft.com/en-us/research/group/dltc/) sinir ağları, İnsan beyninin Biyolojik işlemler tarafından ilham kullanan makine öğrenimi daldır. Birçok Araştırmacıları, derin öğrenme yapay zeka geliştirme taahhüdü bir yaklaşım olarak bakın. Konuşulan dili çevirmenler ve görüntü tanıma sistemlerini makine mantık derin öğrenme örnekleridir.

Derin öğrenme kendi işlerinde ilerletmek için ücretsiz, kullanımı kolay, açık kaynak Microsoft geliştirilen [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/). Bu araç seti, çok çeşitli Microsoft ürünlerinin, derin öğrenme uygun ölçekte dağıtma gereksinimi ile dünya çapında ve en son algoritmalar ve teknikler isteyen Öğrenciler tarafından kullanılıyor. 

## <a name="see-also"></a>Ayrıca bkz.

### <a name="scenarios"></a>Senaryolar

* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Mahout ile film önerileri oluşturma](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Hive ve Azure Machine Learning](../machine-learning/team-data-science-process/create-features-hive.md)
* [Hive ve Azure Machine Learning uçtan uca](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Machine learning ile HDInsight üzerinde Spark](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Derin öğrenme kaynakları

* [Spark ile ayrıntılı öğrenme Araç Seti](https://blogs.technet.microsoft.com/machinelearning/2017/04/25/using-microsofts-deep-learning-toolkit-with-spark-on-azure-hdinsight-clusters/)
* [Bilişsel Araç Seti + Tensorflow Spark üzerinde utandırıcı derecede paralel görüntü sınıflandırması](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
