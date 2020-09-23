---
title: 'POISSON regresyon: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Poposıya regresyon modeli oluşturmak için Poisson regresyon modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 07/13/2020
ms.openlocfilehash: a1a09357c7f80d4af0198a33a2e0007782ef232f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90905247"
---
# <a name="poisson-regression"></a>Poisson Regresyonu

Bu makalede Azure Machine Learning tasarımcısında bir modül açıklanmaktadır.

Bir işlem hattında Poisson regresyon modeli oluşturmak için bu modülü kullanın. POISSON regresyon, sayısal değerleri tahmin etmek için, genellikle sayımlar için tasarlanmıştır. Bu nedenle, yalnızca tahmin etmeye çalıştığınız değerler aşağıdaki koşullara uyması durumunda regresyon modelinizi oluşturmak için bu modülü kullanmanız gerekir:

- Yanıt değişkeninin bir [Poisson dağılımı](https://en.wikipedia.org/wiki/Poisson_distribution)vardır.  

- Sayımlar negatif olamaz. Bu yöntemi negatif etiketlerle kullanmayı denerseniz yöntem başarısız olur.

- POISSON dağılımı ayrı bir dağıtımdır; Bu nedenle, bu yöntemin tam sayı olmadan kullanılması anlamlı değildir.

> [!TIP]
> Hedef bir sayı değilse, Poisson gerileme büyük olasılıkla uygun bir yöntem değildir. [Tasarımcıda diğer regresyon modüllerini](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference#machine-learning-algorithms)deneyin. 

Regresyon yöntemini ayarladıktan sonra, tahmin etmek istediğiniz değerin örneklerini içeren bir veri kümesini kullanarak modeli eğmeniz gerekir. Daha sonra eğitilen model, tahminleri yapmak için kullanılabilir.

## <a name="more-about-poisson-regression"></a>Poisson gerileme hakkında daha fazla bilgi

POISSON regresyon, genellikle sayıları modellemek için kullanılan özel bir gerileme Analizi türüdür. Örneğin, bu senaryolarda Poisson gerileme yararlı olacaktır:

- Uçak fışıklarından ilişkili birlikte bulunan kods sayısını Modellendirme

- Bir olay sırasında acil durum hizmeti çağrılarının sayısını tahmin etme

- Bir promosyonu izleyen müşteri sorguları sayısını yansıtma

- Yedek tablo oluşturma

Yanıt değişkeninin bir Poisson dağılımı olduğundan, model, en az kareler regresyonından, veriler ve olasılık dağılımı hakkında farklı varsayımlar yapar. Bu nedenle, Poisson modellerinin diğer regresyon modellerinden farklı yorumlanması gerekir.

## <a name="how-to-configure-poisson-regression"></a>Poisson gerilemesini yapılandırma

1. İşlem hattınızı tasarımcı 'da **Poisson regresyon** modülünü ekleyin. Bu modülü, **regresyon** kategorisinde **Machine Learning algoritmalarda**bulabilirsiniz.

2. Doğru türdeki eğitim verilerini içeren bir veri kümesi ekleyin. 

    Gerileme işlemine eğleştirmek için kullanmadan önce giriş veri kümesini normalleştirmek üzere [verileri normalleştirip](normalize-data.md) kullanmanızı öneririz.

3. **Poıslik regresyon** modülünün sağ bölmesinde, model **oluşturma modu** seçeneğini ayarlayarak modelin nasıl eğitilmesini istediğinizi belirtin.  
  
    - **Tek parametre**: modeli nasıl yapılandırmak istediğinizi biliyorsanız bağımsız değişken olarak belirli bir değer kümesi sağlayın.
  
    - **Parametre aralığı**: en iyi parametrelerden emin değilseniz, [model hiper parametreleri ayarla](tune-model-hyperparameters.md) modülünü kullanarak bir parametre tarama işlemi yapın. En iyi yapılandırmayı bulmak için, eğitmen, belirttiğiniz birden çok değer üzerinde yinelenir.
  
4. **İyileştirme toleransı**: iyileştirme sırasında tolerans aralığını tanımlayan bir değer yazın. Değer ne kadar düşükse, sığdırma daha yavaş ve daha doğru olur.

5. **L1 düzenleme Weight** ve **L2 düzenleme Weight**: L1 ve L2 düzenleme için kullanılacak değerleri yazın. *Düzenleme* , eğitim verilerinden bağımsız olan modelin yönlerine yönelik kısıtlamalar ekler. Düzenleme, çok fazla yer olmaması için yaygın olarak kullanılır. 

    - Hedef, mümkün olduğunca seyrek bir model olması halinde L1 düzenleme yararlıdır.

        L1 düzenleme, Learner 'in en aza indirmeye çalıştığı kayıp ifadesinden ağırlık vektörünün L1 ağırlığı çıkarılarak yapılır. L1 norm, sıfır olmayan Koordinat sayısı olan L0 normu için iyi bir seçimdir.

    - L2 düzenleme, ağırlık vektöründeki tüm tek koordinatları çok fazla büyümeye engel olur. Hedef, küçük Toplam ağırlıklarla bir modele sahip olmak için L2 düzenleme yararlıdır.

    Bu modülde, L1 ve L2 regularizations birleşimini uygulayabilirsiniz. L1 ve L2 düzenleme birleştiren parametre değerlerinin büyüklüğü hakkında bir ceza değeri getirebilirsiniz. Learner, kaybı en aza indirmek için zorunluluğunu getirir içinde ceza puanı en aza indirmeye çalışır.

    L1 ve L2 düzenleme hakkında iyi bir tartışma için bkz. [L1 ve L2 düzenleme for Machine Learning](https://msdn.microsoft.com/magazine/dn904675.aspx).

6. **L-BFGS Için bellek boyutu**: model sığdırma ve iyileştirme için ayrılacak bellek miktarını belirtin.

     L-BFGS, en iyi duruma getirme için özel bir yöntemdir: Brohden – FLET, – Goldfarb – shanno (BFGS) algoritmasını temel alır. Yöntemi, sonraki adım yönünü hesaplamak için sınırlı miktarda bellek (L) kullanır.

     Bu parametreyi değiştirerek, bir sonraki adımın hesaplaması için depolanan geçmiş konumların ve degradelerin sayısını etkileyebilirsiniz.

7. Eğitim veri kümesini ve eğitilen modeli eğitim modülleriyle birine bağlayın: 

    - **Tek parametreye**oluşturma ve ayarlama **modunu** ayarlarsanız, [model eğitme](train-model.md) modülünü kullanın.

    - **Parametre aralığına** **oluşturma** sağlayan bir mod ayarlarsanız, [model hiper parametrelerini ayarla](tune-model-hyperparameters.md) modülünü kullanın.

    > [!WARNING]
    > 
    > - [Modeli Eğiteetmek](train-model.md)için bir parametre aralığı geçirirseniz, parametre aralığı listesindeki yalnızca ilk değeri kullanır.
    > 
    > - Tek bir parametre değerleri kümesini [ayarlama modeli hiper parametreleri](tune-model-hyperparameters.md) modülüne geçirirseniz, her parametre için bir dizi ayar beklerken, değerleri yoksayar ve öğrenici için varsayılan değerleri kullanır.
    > 
    > - **Parametre aralığı** seçeneğini belirleyip herhangi bir parametre için tek bir değer girerseniz, belirtilen tek değer, diğer parametrelerin bir değer aralığı üzerinde değişse bile, tarama boyunca kullanılır.

8.  İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

Eğitim tamamlandıktan sonra:

+ Eğitilen modelin anlık görüntüsünü kaydetmek için eğitim modülünü seçin ve sağ paneldeki **çıktılar + Günlükler** sekmesine geçin. Simge **yazmaç veri kümesine**tıklayın.  Kaydedilmiş modeli modül ağacında bir modül olarak bulabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
