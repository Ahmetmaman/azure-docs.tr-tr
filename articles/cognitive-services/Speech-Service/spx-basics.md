---
title: Konuşma CLı temelleri
titleSuffix: Azure Cognitive Services
description: Konuşma CLı komut aracını, kod olmadan ve en düşük kurulum ile konuşma hizmetiyle çalışmak için kullanmayı öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: bceffe5c53b9cbc863fd9c923ffa4718ebd50436
ms.sourcegitcommit: b437bd3b9c9802ec6430d9f078c372c2a411f11f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91893824"
---
# <a name="learn-the-basics-of-the-speech-cli"></a>Konuşma CLı 'nın temellerini öğrenin

Bu makalede, konuşma hizmetini kod yazmadan kullanmak için bir komut satırı aracı olan konuşma CLı 'nın temel kullanım düzenlerini öğrenirsiniz. Kullanım durumlarınızın yeterince karşılanıp karşılanmadığını görmek için, geliştirme ortamları oluşturmadan veya herhangi bir kod yazmaya gerek kalmadan, konuşma hizmetinin ana özelliklerini hızlıca test edebilirsiniz. Ayrıca, konuşma CLı, üretime hazırlanın ve konuşma hizmetindeki basit iş akışlarını otomatik hale getirmek için `.bat` veya kabuk betikleri kullanılarak kullanılabilir.

[!INCLUDE [](includes/spx-setup.md)]

## <a name="basic-usage"></a>Temel kullanım

Bu bölümde, genellikle ilk kez test ve deneme için yararlı olan birkaç temel SPX komutu gösterilmektedir. Aşağıdaki komutu çalıştırarak araca yerleşik yardımı görüntüleyerek başlayın.

```shell
spx
```

Dikkat: komut parametrelerinin sağında listelenen yardım konuları **bölümüne bakın** . Alt komutlar hakkında ayrıntılı yardım almak için bu komutları girebilirsiniz.

Yardım konularını anahtar sözcüğe göre arayabilirsiniz. Örneğin, konuşma CLı kullanım örneklerinin bir listesini görmek için aşağıdaki komutu girin:

```shell
spx help find --topics "examples"
```

Recognize komutuna yönelik seçenekleri görmek için aşağıdaki komutu girin:

```shell
spx help recognize
```

Şimdi, aşağıdaki komutu çalıştırarak varsayılan mikrofonunuzu kullanarak bazı konuşma tanıma işlemleri gerçekleştirmek için konuşma hizmetini kullanın.

```shell
spx recognize --microphone
```

Komutu girdikten sonra, SPX geçerli etkin giriş cihazında sesi dinlemeye başlayacaktır ve ' ı bastıktan sonra durur `ENTER` . Kaydedilen konuşma daha sonra tanınır ve konsol çıkışında metne dönüştürülür. Metinden konuşmaya birleştirme özelliği, konuşma CLı 'yi kullanmayı da kolaylaştırır. 

Aşağıdaki komutun çalıştırılması girilen metni girdi olarak alır ve birleştirilmiş konuşmayı geçerli etkin çıkış cihazına çıktı olarak alır.

```shell
spx synthesize --text "Testing synthesis using the Speech CLI" --speakers
```

Konuşma tanıma ve birleştirme özelliğine ek olarak konuşma CLı ile konuşma çevirisi de yapabilirsiniz. Yukarıdaki konuşma tanıma komutuna benzer şekilde, varsayılan mikrofonunuzdan ses yakalamak ve hedef dilde metne çeviri gerçekleştirmek için aşağıdaki komutu çalıştırın.

```shell
spx translate --microphone --source en-US --target ru-RU --output file C:\some\file\path\russian_translation.txt
```

Bu komutta, hem kaynak **(çevrilecek**dil) hem de hedef (çevrilecek **dil) dillerini**belirtirsiniz. `--microphone`Bağımsız değişkeninin kullanılması, geçerli etkin giriş cihazında ses dinlemek ve ' a bastıktan sonra duracaktır `ENTER` . Çıktı, bir metin dosyasına yazılan hedef dile bir metin dönüştürmesidir.

> [!NOTE]
> Tüm desteklenen dillerin bir listesi için ilgili yerel ayar kodlarıyla birlikte [dil ve yerel ayar makalesine](language-support.md) bakın.

### <a name="configuration-files-in-the-datastore"></a>Veri deposundaki yapılandırma dosyaları

Konuşma CLı, yapılandırma dosyalarında yerel konuşma CLı veri deposunda depolanan birden çok ayarı okuyup yazabilir ve bir @ symbol kullanan konuşma CLı çağrıları içinde adlandırılır. Konuşma CLı, yeni bir ayarı `./spx/data` geçerli çalışma dizininde oluşturduğu yeni bir alt dizinde kaydetmeye çalışır.
Bir yapılandırma değeri ararken, konuşma CLı geçerli çalışma dizininizde ve sonra `./spx/data` yolunda görünür.
Daha önce, ve değerlerini kaydetmek için veri deposunu `@key` kullandınız `@region` , bu nedenle bunları her bir komut satırı çağrısıyla belirtmeniz gerekmez.
Yapılandırma dosyalarını kendi yapılandırma ayarlarınızı depolamak için de kullanabilir veya çalışma zamanında oluşturulan URL 'Leri ya da diğer dinamik içeriği geçirmek için kullanabilirsiniz.

Bu bölümde, kullanarak komut ayarlarını depolamak ve getirmek için yerel veri deposundaki bir yapılandırma dosyasının kullanımı `spx config` ve seçeneğini kullanarak konuşma CLI 'dan çıktı depolaması gösterilmektedir `--output` .

Aşağıdaki örnek, `@my.defaults` yapılandırma dosyasını temizler, dosyadaki **anahtar** ve **bölge** için anahtar-değer çiftleri ekler ve ' a yapılan çağrıda yapılandırmayı kullanır `spx recognize` .

```shell
spx config @my.defaults --clear
spx config @my.defaults --add key 000072626F6E20697320636F6F6C0000
spx config @my.defaults --add region westus

spx config @my.defaults

spx recognize --nodefaults @my.defaults --file hello.wav
```

Ayrıca, bir yapılandırma dosyasına dinamik içerik yazabilirsiniz. Örneğin, aşağıdaki komut özel bir konuşma modeli oluşturur ve yeni modelin URL 'sini bir yapılandırma dosyasında depolar. Sonraki komut, söz konusu URL 'deki modelin döndürülmeden önce kullanıma hazırlanana kadar bekler.

```shell
spx csr model create --name "Example 4" --datasets @my.datasets.txt --output url @my.model.txt
spx csr model status --model @my.model.txt --wait
```

Aşağıdaki örnek yapılandırma dosyasına iki URL yazar `@my.datasets.txt` .
Bu senaryoda, `--output` bir yapılandırma dosyası oluşturmak veya mevcut bir dosyaya eklemek için isteğe bağlı bir **Add** anahtar sözcüğü içerebilir.


```shell
spx csr dataset create --name "LM" --kind Language --content https://crbn.us/data.txt --output url @my.datasets.txt
spx csr dataset create --name "AM" --kind Acoustic --content https://crbn.us/audio.zip --output add url @my.datasets.txt

spx config @my.datasets.txt
```

Varsayılan yapılandırma dosyalarının kullanımı ( `@spx.default` , `@default.config` ve `@*.default.config` komutuna özgü varsayılan ayarlar için) dahil olmak üzere veri deposu dosyaları hakkında daha fazla bilgi için şu komutu girin:

```shell
spx help advanced setup
```

## <a name="batch-operations"></a>Toplu işlemler

Önceki bölümdeki komutlar, konuşma hizmetinin nasıl çalıştığını hızlı bir şekilde görmek için harika bir yöntemdir. Ancak, kullanım durumlarının karşılanıp karşılanamayacağını değerlendirmek için büyük olasılıkla zaten sahip olduğunuz bir giriş aralığına karşı toplu işlemler gerçekleştirmeniz gerekir, bu da hizmetin çeşitli senaryoları nasıl işlediğini görmenizi sağlar. Bu bölümde nasıl yapılacağı gösterilmektedir:

* Toplu konuşma tanımayı bir ses dosyaları dizininde Çalıştır
* Bir dosya üzerinden yineleme yapın `.tsv` ve toplu iş metin okuma senşü çalıştırın

## <a name="batch-speech-recognition"></a>Toplu konuşma tanıma

Bir ses dosyası dizininiz varsa, toplu konuşma tanımayı hızlı bir şekilde çalıştırmak için konuşma CLı 'yı kolayca kullanabilirsiniz. Aşağıdaki komutu çalıştırarak dizininizle birlikte `--files` komutunu çalıştırın. Bu örnekte, `\*.wav` dizinde bulunan tüm dosyaları tanımak için dizine eklenir `.wav` . Ayrıca, `--threads` tanımayı 10 paralel iş parçacığında çalıştırmak için bağımsız değişkenini belirtin.

> [!NOTE]
> `--threads`Bağımsız değişken, komutlar için sonraki bölümde de kullanılabilir `spx synthesize` ve kullanılabilir Iş parçacıkları CPU 'ya ve geçerli yük yüzdesine göre değişir.

```shell
spx recognize --files C:\your_wav_file_dir\*.wav --output file C:\output_dir\speech_output.tsv --threads 10
```

Tanınan konuşma çıktısı `speech_output.tsv` `--output file` bağımsız değişkenini kullanarak yazılır. Aşağıda çıkış dosyası yapısına bir örnek verilmiştir.

```output
audio.input.id    recognizer.session.started.sessionid    recognizer.recognized.result.text
sample_1    07baa2f8d9fd4fbcb9faea451ce05475    A sample wave file.
sample_2    8f9b378f6d0b42f99522f1173492f013    Sample text synthesized.
```

## <a name="batch-text-to-speech-synthesis"></a>Batch metin okuma senşü

Batch metin okuma 'yı çalıştırmanın en kolay yolu, yeni bir `.tsv` (sekmeyle ayrılmış-değer) dosyası oluşturmak ve `--foreach` konuşma CLI 'de komuttan faydalanır. Aşağıdaki dosyayı göz önünde bulundurun `text_synthesis.tsv` :

```output
audio.output    text
C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
C:\batch_wav_output\wav_2.wav    Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.
```

 Ardından, öğesini işaret etmek için bir komut çalıştırırsınız `text_synthesis.tsv` , her bir alanda sen, `text` ve sonuç olarak karşılık gelen `audio.output` yola bir dosya olarak yazar `.wav` . 

```shell
spx synthesize --foreach in @C:\your\path\to\text_synthesis.tsv
```

Bu komut, `spx synthesize --text Sample text to synthesize --audio output C:\batch_wav_output\wav_1.wav` dosyadaki **her kayıt için** çalıştırmanın eşdeğeridir `.tsv` . Dikkat edilmesi gereken birkaç şey:

* Sütun başlıkları, `audio.output` ve `text` sırasıyla komut satırı bağımsız değişkenlerine karşılık gelir `--audio output` `--text` . Gibi çok parçalı komut satırı bağımsız değişkenleri `--audio output` , boşluk olmadan dosyada, önde gelen tireler olmadan ve dizeleri ayıran noktalarla biçimlendirilmelidir. Örneğin, `audio.output` . Diğer mevcut komut satırı bağımsız değişkenleri, bu model kullanılarak ek sütunlar olarak dosyaya eklenebilir.
* Dosya bu şekilde biçimlendirildiğinde, başka bir bağımsız değişken geçirilmesi gerekmez `--foreach` .
* İçindeki her değeri bir sekmeyle ayırtığınızdan emin olun `.tsv` . **tab**

Ancak, `.tsv` Aşağıdaki örnekte olduğu gibi bir dosyanız varsa, komut satırı bağımsız değişkenleriyle **eşleşmeyen** sütun başlıkları vardır:

```output
wav_path    str_text
C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
C:\batch_wav_output\wav_2.wav    Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.
```

Bu alan adlarını, çağrıda aşağıdaki sözdizimini kullanarak doğru bağımsız değişkenlere geçersiz kılabilirsiniz `--foreach` . Bu, yukarıdaki çağrıdır.

```shell
spx synthesize --foreach audio.output;text in @C:\your\path\to\text_synthesis.tsv
```

## <a name="next-steps"></a>Sonraki adımlar

* SDK ['yı kullanarak konuşma tanımayı](./quickstarts/speech-to-text-from-microphone.md) veya [konuşma senşliğini](./quickstarts/text-to-speech.md) hızlı başlangıçlara tamamlayabilirsiniz.
