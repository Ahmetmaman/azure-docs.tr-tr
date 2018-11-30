---
title: Örnek verileri Azure HDInsight Hive tablolarındaki | Microsoft Docs
description: Örnekleme (Hadopop) Azure HDInsight Hive tablolarındaki verileri aşağı
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 793431e6e81712bae5033eff1eecddbf448ce120
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446934"
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Azure HDInsight Hive tablolarındaki örnek veriler
Bu makalede, aşağı örnek bunu analiz için daha kolay yönetilebilir bir boyuta indirmeyi Hive sorgularını kullanarak Azure HDInsight Hive tablolarında depolanan veriler açıklar. Üç bu özellik kullanılan örnekleme yöntemleri kapsar:

* Tekdüzen rastgele örnekleme
* Gruplara göre rastgele örnekleme
* Stratified örnekleme

**Neden verilerinizi örnek?**
Veri kümesini analiz etmek için planlama büyükse, genellikle aşağı örnek veriler için daha küçük ancak temsilcisi ve daha kolay yönetilebilir bir boyutunu azaltmak için iyi bir fikir olduğunu. Aşağı örnekleme, özellik Mühendisliği veri anlama ve keşfetme kolaylaştırır. Team Data Science Process içinde rolünü hızlı prototip oluşturma veri işleme işlevleri ve makine öğrenimi modellerini etkinleştirmektir.

Bir adımda bu örnekleme görevdir [Team Data Science işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Hive sorguları göndermek nasıl
Hive sorguları, Hadoop küme baş düğümü üzerinde Hadoop komut satırı konsolundan gönderilebilir. Bunu yapmak için Hadoop küme baş düğümüne oturum, Hadoop komut satırı konsolu açın ve buradan Hive sorguları göndermek. Hadoop komut satırı konsolunda Hive sorguları gönderme ile ilgili yönergeler için bkz: [Hive sorguları göndermek için nasıl](move-hive-tables.md#submit).

## <a name="uniform"></a> Tekdüzen rastgele örnekleme
Tekdüzen rastgele örnekleme, her satırda bir veri kümesi örnekleniyor eşit bir olasılığını olduğunu gösterir. Ek alan rand() veri kümesine iç sorgu "Seç" ve "seçin" dış sorgu bu koşul, rastgele alan ekleyerek uygulanabilir.

Örnek bir sorgu aşağıda verilmiştir:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Burada, `<sample rate, 0-1>` kullanıcıları örneklemek istediğiniz kayıtları oranını belirtir.

## <a name="group"></a> Gruplara göre rastgele örnekleme
Örnekleme kategorik veriler, dahil etmek veya hariç tüm örnekleri için bazı kategorik değişkenin değerini istediğinizde. Bu tür bir örnekleme "grubu tarafından örnekleme" adı verilir. Örneğin, Kategorik bir değişken varsa "*durumu*" kayıtların her durumundan veya Örneklendi ve birlikte olmasını istediğiniz, NY, MA, CA, NJ ve PA gibi değerler vardır.

Gruplandırma ölçütü bu örnekleri örnek bir sorgu aşağıdadır:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratified örnekleme
Elde edilen örnekler üst popülasyon oldukları gibi aynı oranı mevcut olan kategorik değerlere sahip olduğunda rastgele örnekleme Kategorik bir değişkene göre stratified. Yukarıdaki olarak aynı örneği kullanarak verilerinizi durumlara göre aşağıdaki gözlemlere sahip olduğunu varsayalım: NJ varsa 100 gözlemleri, NY olan 60 gözlemleri ve WA 300 gözlemler. 0,5 olmasını stratified örnekleme oranını belirtin, ardından alınan örnek yaklaşık 50, 30 ve 150 gözlemleri NJ, NY ve WA sırasıyla olmalıdır.

Örnek bir sorgu aşağıda verilmiştir:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Hive kullanılabilir daha gelişmiş örnekleme yöntemler hakkında daha fazla bilgi için bkz: [LanguageManual örnekleme](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

