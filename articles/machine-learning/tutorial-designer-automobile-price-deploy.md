---
title: 'Öğretici: tasarımcı ile ML modellerini dağıtma'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, Azure Machine Learning tasarımcısında tahmine dayalı analiz çözümünün nasıl oluşturulacağı gösterilmektedir. Sürükle ve bırak modüllerini kullanarak makine öğrenimi modelini eğitme, Puanlama ve dağıtma.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 06/28/2020
ms.custom: designer
ms.openlocfilehash: a4923e48c890a50d642d937f014e466e998171cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90896632"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer"></a>Öğretici: tasarımcı ile makine öğrenimi modeli dağıtma


Diğer kullanıcılara bunu kullanma şansı vermek için [öğreticinin birinci](tutorial-designer-automobile-price-train-score.md) kısmında geliştirilen tahmine dayalı modeli dağıtabilirsiniz. Birinci bölümde, modelinizi eğitilolursunuz. Şimdi, kullanıcı girişine göre yeni tahmin oluşturma zamanı. Öğreticinin bu bölümünde şunları yapmanız gerekir:

> [!div class="checklist"]
> * Gerçek zamanlı bir çıkarım işlem hattı oluşturun.
> * Bir ınıri sınırlama kümesi oluşturun.
> * Gerçek zamanlı bitiş noktasını dağıtın.
> * Gerçek zamanlı uç noktayı test edin.

## <a name="prerequisites"></a>Önkoşullar

Tasarımcı 'da makine öğrenimi modelinin nasıl eğeceğinizi ve puanlandıralınacağını öğrenmek için [öğreticiden birinin bir kısmını](tutorial-designer-automobile-price-train-score.md) doldurun.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı bir çıkarım işlem hattı oluşturma

İşlem hattınızı dağıtmak için öncelikle eğitim işlem hattını gerçek zamanlı bir çıkarım ardışık düzenine dönüştürmeniz gerekir. Bu işlem, eğitim modüllerini kaldırır ve istekleri işlemek için Web hizmeti girişleri ve çıkışları ekler.

### <a name="create-a-real-time-inference-pipeline"></a>Gerçek zamanlı bir çıkarım işlem hattı oluşturma

1. İşlem hattı tuvalinin üzerinde, **çıkarım ardışık düzen**  >  **gerçek zamanlı çıkarım işlem hattı**' nı seçin.

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/tutorial2-create-inference-pipeline.png"alt-text="Ardışık düzen oluştur düğmesinin nerede bulunacağını gösteren ekran görüntüsü":::

    İşlem hatlarınız şu şekilde görünmelidir: 

   ![Dağıtım için hazırlandıktan sonra işlem hattının beklenen yapılandırmasını gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/real-time-inference-pipeline.png)

    **Çıkarım işlem hattı oluştur**' u seçtiğinizde birkaç şey meydana gelir:
    
    * Eğitilen model, modül paletinde bir **veri kümesi** modülü olarak depolanır. **Veri kümelerim**altında bulabilirsiniz.
    * Model ve **bölünmüş verileri** **eğitme** gibi eğitim modülleri kaldırılır.
    * Kaydedilen eğitilen model ardışık düzene geri eklenir.
    * **Web hizmeti girişi** ve **Web hizmeti çıkış** modülleri eklendi. Bu modüller, Kullanıcı verilerinin işlem hattına girdiği ve verilerin döndürüldüğü yeri gösterir.

    > [!NOTE]
    > Varsayılan olarak, **Web hizmeti girişi** , tahmine dayalı işlem hattını oluşturmak için kullanılan eğitim verileriyle aynı veri şemasını bekler. Bu senaryoda, şemaya bir değer dahildir. Ancak, fiyat tahmin sırasında bir faktör olarak kullanılmaz.
    >

1. **Gönder**' i seçin ve aynı işlem hedefini kullanın ve birinci bölümde kullandığınız deneyin.

    İlk çalıştırıldır, işlem hattının çalışmasının tamamlanması 20 dakikaya kadar sürebilir. Varsayılan işlem ayarlarının minimum düğüm boyutu 0 ' dır ve bu, tasarımcının boşta kaldıktan sonra kaynakları ayırması gerektiği anlamına gelir. İşlem kaynakları zaten ayrıldığından tekrarlanan işlem hattı çalıştırmaları daha az zaman alır. Ayrıca tasarımcı, verimliliği artırmak için her modül için önbelleğe alınmış sonuçları kullanır.

1. **Dağıt**'ı seçin.

## <a name="create-an-inferencing-cluster"></a>Bir ınıri yalıtma kümesi oluşturma

Görüntülenen iletişim kutusunda, modelinizi dağıtmak için mevcut herhangi bir Azure Kubernetes hizmeti (AKS) kümesi arasından seçim yapabilirsiniz. AKS kümeniz yoksa, bir tane oluşturmak için aşağıdaki adımları kullanın.

1. **İşlem** sayfasına git görüntülenen Iletişim kutusunda **işlem** ' i seçin.

1. Gezinti şeridinde, **çıkarım kümeleri**  >  **+ Yeni**' yi seçin.

    ![Yeni çıkarım kümesi bölmesine nasıl alınacağını gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/new-inference-cluster.png)
   
1. Çıkarım kümesi bölmesinde yeni bir Kubernetes hizmeti yapılandırın.

1. **İşlem adı**için *aks-COMPUTE* girin.
    
1. **Bölge**için kullanılabilen bir yakın bölge seçin.

1. **Oluştur**’u seçin.

    > [!NOTE]
    > Yeni bir AKS hizmeti oluşturmak yaklaşık 15 dakika sürer. **Çıkarım kümeleri** sayfasında sağlama durumunu kontrol edebilirsiniz.
    >

## <a name="deploy-the-real-time-endpoint"></a>Gerçek zamanlı uç noktayı dağıtma

AKS hizmetiniz sağlamayı tamamladıktan sonra, dağıtımı tamamlamaya yönelik gerçek zamanlı ıntıma ardışık düzenine geri dönün.

1. Tuvalin üzerinde **Dağıt** ' ı seçin.

1. **Yeni gerçek zamanlı uç noktayı dağıt**' ı seçin. 

1. Oluşturduğunuz AKS kümesini seçin.

1. **Dağıt**'ı seçin.
    
    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/setup-endpoint.png"alt-text="Ardışık düzen oluştur düğmesinin nerede bulunacağını gösteren ekran görüntüsü":::

    Dağıtım bittikten sonra tuvalin üzerindeki başarı bildirimi görüntülenir. İşlem birkaç dakika sürebilir.

## <a name="test-the-real-time-endpoint"></a>Gerçek zamanlı uç noktayı test etme

Dağıtım bittikten sonra, **uç noktalar** sayfasına giderek gerçek zamanlı uç noktanızı test edebilirsiniz.

1. **Uç noktalar** sayfasında, dağıttığınız uç noktayı seçin.

    ![Son oluşturulan bitiş noktası vurgulanmış şekilde gerçek zamanlı uç noktalar sekmesini gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/endpoints.png)

1. **Test**'i seçin.

1. Test verilerini el ile belirtebilir veya oto dolgulu örnek verileri kullanabilir ve **Test**' i seçebilirsiniz.

    Portal, uç noktaya bir test isteği gönderir ve sonuçları gösterir. Giriş verileri için bir fiyat değeri oluşturulsa da tahmin değeri oluşturmak için kullanılmaz.

    ![Fiyat vurgulanmış olarak gerçek zamanlı uç noktanın puanlanmış etiketle nasıl test leneceğini gösteren ekran görüntüsü](./media/tutorial-designer-automobile-price-deploy/test-endpoint.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, tasarımcıda makine öğrenimi modeli oluşturma, dağıtma ve kullanma bölümündeki temel adımları öğrendiniz. Diğer sorun türlerini çözümlemek için tasarımcıyı nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için diğer örnek işlem hatlarımıza bakın.

> [!div class="nextstepaction"]
> [Tasarımcı örnekleri](samples-designer.md)
