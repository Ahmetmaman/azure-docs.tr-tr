---
title: LUIS uygulamalarında varlık ekleme
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) uygulamalarında varlıklar (anahtar, uygulamanızın etki alanı veri) ekleyin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.openlocfilehash: 178f4314f9ede86444ee60fd2a64f85dc283080b
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138540"
---
# <a name="create-entities-without-utterances"></a>Konuşma olmadan varlık oluşturma

Varlık, bir sözcük veya tümcecik ayıklanan istediğiniz utterance içinde temsil eder. Bir utterance birçok varlığın veya hiçbiri hiç içerebilir. Bir varlık (yerlerde, öğeleri, kişiler, olayları veya kavramları) benzer nesnelerinin bir koleksiyonunu içeren bir sınıfı temsil eder. Bazen uygulamanızın görevini gerçekleştirmek gerekli olan ve bilgi ıntent'e ilgili varlıkları anlatmaktadır. 

Önce veya oluşturma amacı dışında varlıklar oluşturabilirsiniz.

Ekleme, düzenleme veya LUIS uygulamanızda varlıklarını silme **varlıkları listesi** üzerinde **varlıkları** sayfası. LUIS, iki ana tür varlıklar sunar: [önceden oluşturulmuş varlıklarla](luis-reference-prebuilt-entities.md)ve kendi [özel varlıklar](luis-concept-entity-types.md#types-of-entities).

Bir varlık oluşturduktan sonra bir amaç'ın örnek utterance olarak etiketlenebilir **hedefi** Ayrıntıları sayfası. 

## <a name="add-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

Bir uygulamaya eklenen ortak önceden oluşturulmuş varlıklar *numarası* ve *datetimeV2*. 

1. Uygulamanızda, gelen **derleme** bölümünden **varlıkları** sol bölmesinde.
 
1. Üzerinde **varlıkları** sayfasında **önceden oluşturulmuş varlıklar ekleme**.

1. İçinde **önceden oluşturulmuş varlıklar ekleme** iletişim kutusunda **numarası** ve **datetimeV2** önceden oluşturulmuş varlıklar. Ardından **Bitti**'yi seçin.

    ![Önceden oluşturulmuş varlık iletişim kutusu Ekle ekran görüntüsü](./media/add-entities/list-of-prebuilt-entities.png)

## <a name="add-simple-entities"></a>Basit bir varlık ekleme

Bir varlığın tek bir kavramı açıklanmaktadır. Şirket bölüm adlarını gibi ayıklar bir varlık oluşturmak için aşağıdaki yordamı kullanın. *İnsan Kaynakları* veya *Operations*.   

1. Uygulamanızda seçin **derleme** bölümüne ve ardından **varlıkları** sol panelde, ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `Location` içinde **varlık adı** kutusunda **basit** gelen **varlık türü** listeleyin ve ardından **Bitti**.

    Bu varlık oluşturulduktan sonra varlık içeren örnek konuşma sahip tüm hedefleri için gidin. Örnek utterance metni seçin ve metin varlık olarak işaretleyin. 

    A [tümcecik listesi](luis-concept-feature.md) genellikle bir varlığın sinyal artırmak için kullanılır.

## <a name="add-regular-expression-entities"></a>Normal ifade varlıkları ekleyin

Bir normal ifade varlık sağladığınız bir normal ifadeye göre utterance verileri dışarı çıkarmak için kullanılır. 

1. Uygulamanızda seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `Human resources form name` içinde **varlık adı** kutusunda **normal ifade** gelen **varlık türü** listesinde, normal ifade girin`hrf-[0-9]{6}`ve ardından **Bitti**. 

    Bu normal ifade sabit karakterleriyle `hrf-`, daha sonra 6 basamaklı bir formu temsil etmek için bir insan kaynakları form sayı.

## <a name="add-hierarchical-entities"></a>Hiyerarşik bir varlık ekleme

Hiyerarşik bir varlık, bağlamsal öğrenilen ve kavramsal olarak ilişkili varlıkları kategorisidir. Aşağıdaki örnekte, kaynak ve hedef konumları varlık içerir. 

Utterance içinde `Move John Smith from Seattle to Cairo`, Seattle kaynak konumu ve Cairo hedef konumu. Her kelime sırasını ve sözcük seçenek utterance bağlamsal olarak farklı ve öğrenilen konumdur.

Hiyerarşik bir varlık eklemek için aşağıdaki adımları tamamlayın: 

1. Uygulamanızda seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `Location` içinde **varlık adı** kutusuna ve ardından **hiyerarşik** gelen **varlık türü** listesi.

    ![Hiyerarşik varlık ekleme](./media/add-entities/hier-location-entity-creation.png)

1. Seçin **alt öğe Ekle**yazıp enter `Origin` içinde **alt #1** kutusu. 

1. Seçin **alt öğe Ekle**yazıp enter `Destination` içinde **alt 2** kutusu. **Done** (Bitti) öğesini seçin.

    >[!CAUTION]
    >Tek bir uygulamada tüm varlıklar üzerinde alt varlık adlarının benzersiz olması gerekir. Alt varlıklar aynı ada sahip iki farklı hiyerarşik varlıklar içerebilir. 

    Bu varlık oluşturulduktan sonra varlık içeren örnek konuşma sahip tüm hedefleri için gidin. Örnek utterance metni seçin ve metin varlık olarak işaretleyin. 

## <a name="add-composite-entities"></a>Bileşik varlık ekleme

Bileşik bir varlık oluşturarak farklı türlerde varlıklar arasında ilişkiler tanımlayabilirsiniz. Aşağıdaki örnekte, varlık, normal bir ifade ve önceden oluşturulmuş bir varlık adını içerir.  

Utterance içinde `Send hrf-123456 to John Smith`, metin `hrf-123456` bir insan kaynakları için eşleşen [normal ifade](#add-regular-expression-entities) ve `John Smith` ile önceden oluşturulmuş varlık personName ayıklanır. Her varlık, daha büyük bir üst varlığa bir parçasıdır. 

1. Uygulamanızda seçin **varlıkları** sol gezinti bölmesinden **derleme** bölümüne ve ardından **önceden oluşturulmuş bir varlık ekleme**.

1. Önceden oluşturulmuş varlık ekleme **PersonName**. Yönergeler için [önceden oluşturulmuş varlıklar ekleme](#add-prebuilt-entity). 

1. Seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `SendHrForm` içinde **varlık adı** kutusuna ve ardından **bileşik** gelen **varlık türü** listesi.

1. Seçin **alt öğe Ekle** yeni bir alt öğe eklemek için.

1. İçinde **alt #1**, varlığı seçin **numarası** listeden.

1. İçinde **alt 2**, varlığı seçin **İnsan Kaynakları form adı** listeden. 

1. **Done** (Bitti) öğesini seçin.

## <a name="add-patternany-entities"></a>Pattern.Any varlık ekleme

[Pattern.Any](luis-concept-entity-types.md) varlıklardır yalnızca geçerli [desenleri](luis-how-to-model-intent-pattern.md), amacı değildir. Bu varlık türü, değişken uzunluğu ve sözcük seçimi varlıklarının sonuna Bul LUIS yardımcı olur. Bu varlık içindeki bir desenle kullanıldığından LUIS son varlık utterance şablonda olduğu bilir.

Bir uygulama varsa bir `FindHumanResourcesForm` hedefi, ayıklanan form başlığı ile hedefi tahmini da etkileyebilir. Formu başlığında sözcüklerdir açıklamak için bir desen içindeki bir Pattern.any kullanın. LUIS tahmin utterance ile başlar. İlk olarak, utterance işaretli ve varlıkları bulunduğunda deseni işaretli ve eşleşen varlıklar için eşleşen. 

Utterance içinde `Where is Request relocation from employee new to the company on the server?`, başlık sona ereceği ve utterance geri kalanını başladığı bağlamsal olarak açık olduğundan form başlığı olduğunu. Başlıklar, sözcük noktalama işaretleri ile karmaşık ifadeleri tek bir sözcük dahil olmak üzere herhangi bir sırada ve nonsensical sözcük sıralama olabilir. Bir desen, bir varlık oluşturmak, tam ve tam varlık ayıkladığınız sağlar. Başlık bulunduğunda `FindHumanResourcesForm` amacı, desenin amacı olduğundan tahmin.

1. Gelen **derleme** bölümünden **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

1. İçinde **varlık Ekle** iletişim kutusu, varlık adı kutuya girin ve seçin **Pattern.any** olarak **varlık türü**.

    Pattern.any varlık kullanmak için üzerinde bir desen Ekle **desenleri** sayfasında **uygulama performansını** doğru küme ayracı sözdizimi bölümündeki `Where is **{HumanResourcesFormTitle}** on the server?`.

    Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](luis-concept-patterns.md#explicit-lists) kullanın. 

## <a name="add-a-role-to-pattern-based-entity"></a>Bir rol için desen tabanlı varlık Ekle

Adlandırılmış alt bağlamını temel alan bir varlığın rolüdür. İçin karşılaştırılabilir bir [hiyerarşik](#add-hierarchical-entities) varlık ancak rolleri yalnızca kullanıldığı [desenleri](luis-how-to-model-intent-pattern.md). 

Kaynak ve hedef şehirler, fark hiyerarşik varlığı aynı örneği kullanarak, bir rolü kaynağı yerine bir hiyerarşik alt olarak adlandırılır olduğu. 

Bir rol için söz dizimi **{Entityname:Rolename}** burada varlık adının ardından bir iki nokta üst üste sonra rol adı. Örneğin, `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. Gelen **derleme** bölümünden **varlıkları** sol bölmesinde.

1. **Create new entity** (Yeni varlık oluştur) öğesini seçin. Adını `LocationUsingRoles`. Tür seçin **basit** seçip **Bitti**. 

1. Seçin **varlıkları** sol panelden yeni varlık seçip **LocationUsingRoles** önceki adımda oluşturduğunuz.

1. İçinde **rol adı** metin kutusuna rol adını girin `Origin` girin. İkinci bir rol adını ekleyin `Destination`. 

    ![Kaynak rolü konumu varlığa ekleme işleminin ekran görüntüsü](./media/add-entities/roles-enter-role-name-text.png)

## <a name="add-list-entities"></a>Liste varlık ekleme

Liste varlık ilgili sözcükler sabit, kapalı bir kümesini temsil eder. 

İnsan Kaynakları bir uygulama için tüm Departmanlar için eş anlamlı sözcükler yanı sıra tüm Departmanlar listesi olabilir. Bir varlık oluşturduğunuzda tüm değerleri bilmeniz gerekmez. Daha fazla, eş anlamlı sözcüklerle gerçek kullanıcı konuşma gözden geçirdikten sonra ekleyebilirsiniz.

1. Gelen **derleme** bölümünden **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

1. İçinde **varlık Ekle** iletişim kutusuna `Department` içinde **varlık adı** kutusunda ve seçin **listesi** olarak **varlık türü**. **Done** (Bitti) öğesini seçin.
  
1. Liste varlık sayfası normalleştirilmiş adları eklemenize olanak sağlar. İçinde **değerleri** metin gibi liste için bir bölüm adı girin `HumanResources` klavyedeki Enter tuşuna basın. 

1. Normalleştirilmiş değeri sağında her öğenin sonunda klavyedeki Enter'tuşuna basarak eş anlamlı sözcükler girin.

1. Daha fazla normalleştirilmiş öğeleri listesini istiyorsanız seçin **önerilir** seçenekleri görmek için [anlam sözlük](luis-glossary.md#semantic-dictionary).

    ![Kaynak rolü konumu varlığa ekleme işleminin ekran görüntüsü](./media/add-entities/hr-list-2.png)


1. Normalleştirilmiş bir değer olarak ekleyin veya seçmek için önerilen listesinden bir öğe seçin **tüm eklemek** tüm öğeleri eklenecek. 
    Değerler şu JSON biçimini kullanarak var olan bir liste varlığa içeri aktarabilirsiniz:

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```


## <a name="change-entity-type"></a>Varlık türünü değiştir

LUIS, ekleme veya kaldırma, varlık oluşturmak için gerekenler bilmediği varlık türünü değiştirmek izin vermez. Türü değiştirmek için biraz daha farklı bir adla doğru türde yeni bir varlık oluşturmak iyidir. Varlık oluşturulduktan sonra eski etiketli varlık adı her utterance içinde kaldırıp yeni varlık adı ekleyin. Tüm sesleri relabeled sonra eski varlığı silin. 

## <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzen oluşturma

Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="train-your-app-after-changing-model-with-entities"></a>Varlıkları modeliyle değiştirdikten sonra uygulamanızı eğitin

Ekleme, düzenleme veya varlıkları Kaldır sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş varlıklarla ilgili daha fazla bilgi için bkz. [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. 

Varlık JSON uç nokta sorgu yanıtına görüntülenme şeklini hakkında daha fazla bilgi için bkz: [veri ayıklama](luis-concept-data-extraction.md)

Hedefleri ve konuşma varlıklarını ekledikten sonra temel bir LUIS uygulaması sahip. Bilgi nasıl [eğitme](luis-how-to-train.md), [test](luis-interactive-test.md), ve [yayımlama](luis-how-to-publish-app.md) uygulamanızı.
 
