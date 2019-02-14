---
title: En iyi uygulamalar - soru-cevap Oluşturucu
titlesuffix: Azure Cognitive Services
description: Bilgi bankanızı artırmak ve uygulama/sohbet Robotu ait son kullanıcılara daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/13/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9ea62d731cf0c16c17f3c2e4f3e1954661289934
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245550"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası en iyi yöntemleri
[Bilgi Bankası geliştirme yaşam döngüsü](../Concepts/development-lifecycle-knowledge-base.md) başlangıçtan bitişe kadar KB yönetme konusunda size yol gösterir. Bilgi bankanızı artırmak ve uygulama/sohbet Robotu ait son kullanıcılara daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.

## <a name="extraction"></a>Ayıklama
Soru-cevap Oluşturucu hizmetini sürekli olarak içerik ve desteklenen dosya ve HTML biçimleri listesini genişleterek Bankalarıyla ayıklamak algoritmaları gelişiyor. İzleyin [yönergeleri](../Concepts/data-sources-supported.md) için belge türüne göre veri ayıklama. 

Genel olarak, SSS sayfaları, tek başına ve diğer bilgilerle birleşik olması gerekir. Ürün kılavuzlarını, NET başlıklar ve tercihen dizin sayfası olması gerekir. 

## <a name="chit-chat"></a>Chit sohbet
Botunuzun daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici, botunuzun yapmak için chit sohbet ekleme az çaba. Kolayca 3 önceden tanımlanmış inancı chit sohbet veri kümeleri, KB oluştururken ekleyin ve bunları dilediğiniz zaman değiştirin. Bilgi edinmek için nasıl [chit sohbet eklemek için KB](../How-To/chit-chat-knowledge-base.md). 

### <a name="choosing-a-personality"></a>Bir kişilik seçme
Chit sohbet için 3 önceden tanımlanmış inancı desteklenir: 

|İnancı|
|--|
|Professional|
|Arkadaş|
|Comic|

Yanıtları aralıktan biçimsel resmi olmayan ve saygısız. En yakın botunuz için istediğiniz sesi birlikte hizalanır kişilik seçmeniz gerekir. Veri kümelerini görüntüleme ve botunuza ilişkin temel olarak hizmet veren bir seçin ve sonra yanıtları özelleştirin. 

### <a name="edit-bot-specific-questions"></a>Bot özgü soruları Düzenle
Sohbet chit veri kümesinin parçası olan ve genel yanıtlar oturum doldurulmuş bazı bot özgü soruları vardır. Bu yanıtlar, en iyi bot ayrıntılarınızı yansıtacak şekilde değiştirin. 

Aşağıdaki sohbet chit Bankalarıyla daha belirgin hale öneririz:

* Kimsiniz?
* Ne yapabilirsiniz?
* Kaç yaşındasınız?
* Kim, oluşturduğunuz?
* Merhaba
   

## <a name="rankingscoring"></a>Derecelendirme ve puanlı
Soru-cevap Oluşturucu desteklediği derecelendirme özellikleri en iyi kullanımı kuran emin olun. Bunun yapılması olasılığına artıracak belirli kullanıcı sorgusu ile uygun bir yanıt yanıtlandığı.

### <a name="choosing-a-threshold"></a>Bir eşiği seçme
İçin KB gereksinimlerinize göre değiştirebilirsiniz ancak eşik olarak kullanılan varsayılan güvenilirlik puanı 50 ' dir. Her KB farklı olduğundan, test edin ve en iyi eşiği için KB uygun seçin gerekir. Daha fazla bilgi edinin [güvenilirlik puanı](../Concepts/confidence-score.md). 


### <a name="add-alternate-questions"></a>Diğer sorular ekleyin
[Diğer sorular](../How-To/edit-knowledge-base.md) bir kullanıcı sorgu ile bir eşleşme olasılığını artırın. Diğer sorular, aynı soruyu istenebilir birden çok yol olduğunda yararlıdır. Word-style ve cümle yapısı içinde bu değişiklikleri içerebilir.

|Özgün sorgu|Diğer sorgular|Değiştir| 
|--|--|--|
|Kullanılabilir park olan?|Araba park var mı?|cümle yapısı|
 |Merhaba|Yo<br>Hey var!|Word stil veya argo kullanımlar|

<a name="#use-metadata-filters"></a>

### <a name="use-metadata-tags-to-filter-questions-and-answers"></a>Filtre sorular ve yanıtlar için meta veri etiketleri kullanma

[Meta veri](../How-To/edit-knowledge-base.md) tabanlı meta veri etiketleri kullanıcı sorgunun sonuçlarını daraltmak için özelliği ekler. Sorgu aynı olsa bile, Bilgi Bankası yanıt meta veri etiketine göre değişebilir. Örneğin, *"bulunan park olduğu"* Restoran dalın konumu farklıdır - diğer bir deyişle, meta veriler, farklı bir yanıt olabilir *konumu: Seattle* karşı *konumu: Redmond*.

### <a name="use-synonyms"></a>Eş anlamlıları kullanma
Eş Anlamlılar İngilizce dilinde bazı desteği olsa da, büyük küçük harf duyarsız kullanın [word değişiklikleri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) farklı biçiminde anahtar sözcükleri eş anlamlılar eklemek için. Eş Anlamlılar soru-cevap Oluşturucu hizmet düzeyinde eklendi ve hizmette tüm bilgi bankalarından tarafından paylaşılan.

|Özgün word|Eş anlamlılar|
|--|--|
|Satın alma|satın al<br>netbanking<br>NET bankacılık|

### <a name="use-distinct-words-to-differentiate-questions"></a>Sorular ayırt etmek için farklı sözcükler kullanın
Farklı bir gereksinim her soruyu ele Bilgi Bankası'nda soru kullanıcı sorguyla eşleşen, soru-cevap Oluşturucu eşleşme derecesi algoritmaları en iyi şekilde çalışır. Yineleme sorular arasında ayarlanmış aynı kelimenin doğru yanıt sözcükleri ile verilen kullanıcı sorgusu için seçilen olasılığını azaltır. 

Örneğin, aşağıdaki soruları ile iki ayrı Bankalarıyla olabilir:

|Bankalarıyla|
|--|
|Park olduğu *konumu*|
|atm olduğu *konumu*|

Bu iki Bankalarıyla çok benzer kelimelerinizle tümcecik oluşturulmuş olduğundan, bu gibi tümcecik oluşturulmuş çok sayıda kullanıcı sorgu çok benzer puanları neden olabilecek *"nerede olduğunu `<x>` konumu"*. Sorgularla gibi açıkça ayırt etmek bunun yerine, deneyin *"park olduğu"* ve *"atm olduğu"*, "Konum" gibi sözcükleri önleme tarafından KB kadar fazla soruyu içinde olabilir. 


## <a name="collaborate"></a>İşbirliği yapın
Soru-cevap Oluşturucu, kullanıcılara sağlayan [işbirliği](../How-to/collaborate-knowledge-base.md) Bilgi Bankası üzerinde. Kullanıcıların bilgi bankalarından erişmek için soru-cevap Oluşturucu Azure kaynak grubuna erişim gerekir. Bazı kuruluşlar Bilgi Bankası düzenleme ve Bakım dış ve Azure kaynaklarına erişim hala koruyabilmesini isteyebilirsiniz. Bu düzenleyici onaylayan modeli iki özdeş ayarlayarak yapılır [soru-cevap Oluşturucu Hizmetleri](../How-to/set-up-qnamaker-service-azure.md) farklı Aboneliklerde ve bir düzenleme test döngüsü için seçme. Test tamamlandıktan sonra Bilgi Bankası içerikleri ile aktarılır bir [içeri / dışarı aktarma](../Tutorials/migrate-knowledge-base.md) son Bilgi Bankası yayımlama ve uç noktayı güncelleştirmek onaylayanın soru-cevap Oluşturucu hizmeti için işlem.

## <a name="active-learning"></a>Etkin öğrenme

[Etkin öğrenme](../How-to/improve-knowledge-base.md) çok çeşitli kalite ve kullanıcı tabanlı sorgular miktarını sahip olduğunda, diğer sorular önerme en iyi bir iş yapar. Etkin durumda katılmak istemci uygulamalar kullanıcı sorgularına izin ver önemlidir Gerçekliğinde olmadan geri bildirim döngüsü öğrenme.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-to/edit-knowledge-base.md)
