---
title: Konuşmayı metne sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Konuşmayı metin hizmeti hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: panosper
ms.openlocfilehash: 0fe5b1350fdcf53eeda11f6de430d8d02a24fdbf
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2020
ms.locfileid: "91397768"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Konuşmayı metne sık sorulan sorular

Bu SSS 'de sorularınıza yanıt bulamazsanız, [diğer destek seçeneklerine](support.md)göz atın.

## <a name="general"></a>Genel

**S: bir taban çizgisi modeliyle özel bir konuşma ve metin modeli arasındaki fark nedir?**

Y **: temel bir model**, Microsoft 'a ait veriler kullanılarak eğitilmiştir ve bulutta zaten dağıtılır. Bir modeli, belirli çevresel gürültü veya dile sahip belirli bir ortama daha iyi uyacak şekilde uyarlamak için özel bir model kullanabilirsiniz. Fabrika zeminleri, otomobiller veya gürültülü Streets bir uyarlanan model gerektirir. Bioloji, fizik, radiolojisi, ürün adları ve özel kısaltmalar gibi konular, bir uyarlanan dil modeli gerektirir.

**S: bir temel modeli kullanmak istersem nereden başlayabilirim?**

**A**: ilk olarak bir [abonelik anahtarı](overview.md#try-the-speech-service-for-free)alın. Önceden dağıtılan taban çizgisi modellerine REST çağrıları yapmak isterseniz [REST API 'lerine](rest-apis.md)bakın. WebSockets kullanmak istiyorsanız [SDK 'yı indirin](speech-sdk.md).

**S: her zaman özel bir konuşma modeli mi kullanmam gerekir?**

Y **: Hayır**. Uygulamanız genel, günlük dil kullanıyorsa, bir modeli özelleştirmeniz gerekmez. Uygulamanız çok az veya arka plan gürültüsünü olmayan bir ortamda kullanılıyorsa, bir modeli özelleştirmeniz gerekmez.

Portalda temel ve özelleştirilmiş modeller dağıtabilir ve ardından bunlarla aynı şekilde doğruluk testleri çalıştırabilirsiniz. Bu özelliği, bir taban çizgisi modelinin doğruluğunu özel bir modele göre ölçmek için kullanabilirsiniz.

**S: veri kümesi veya modelim için işleme tamamlandığında nasıl öğrenebilirim?**

Y **: Şu**anda, tablodaki modelin veya veri kümesinin durumu bilinmenin tek yoludur. İşlem tamamlandığında durum **başarılı**olur.

**S: birden fazla model oluşturabilir miyim?**

**A**Y: koleksiyonunuzda sahip olduğunuz model sayısında sınır yoktur.

**S: bir hata yaptık. Nasıl yaparım? veri içeri aktarma veya model oluşturma işlemini iptal ediyor mu?**

Y **: Şu anda, bir akustik**veya dil uyarlama işlemini geri alamazsınız. İçeri aktarılan verileri ve modelleri, Terminal durumundayken silebilirsiniz.

**S: arama ve dikte modeliyle konuşma modeli arasındaki fark nedir?**

Y **: konuşma**hizmetinde birden fazla temel model arasından seçim yapabilirsiniz. Konuşma modeli, konuşma stilinde konuşulan konuşmayı tanımayı yararlı olur. Bu model, bu telefon görüşmeleri için idealdir. Arama ve dikte modeli, sesli tetiklenen uygulamalar için idealdir. Evrensel model, her iki senaryoyu ele almak için amaçlar 'in yeni bir modelidir. Evrensel model şu anda çoğu yerel olarak konuşma modelinin kalite düzeyinin üzerinde veya üzerinde.

**S: var olan modelmi güncelleştirebilir (model yığınlama)?**

Y **: varolan**bir modeli güncelleştiremezsiniz. Bir çözüm olarak, eski veri kümesini yeni veri kümesiyle ve readapt ile birleştirin.

Eski veri kümesi ve yeni veri kümesi tek bir. zip dosyasında (akustik veriler için) veya bir. txt dosyasında (dil verileri için) birleştirilmelidir. Uyarlama tamamlandığında yeni, güncelleştirilmiş modelin yeni bir uç nokta almak için yeniden dağıtılması gerekir

**S: bir taban çizgisinin yeni bir sürümü kullanılabilir olduğunda, dağıtımım otomatik olarak güncelleştirilir mi?**

**A**: dağıtımlar otomatik olarak güncellenmeyecektir.

Temel V 1.0 içeren bir model uyardıysanız ve dağıttıysanız, bu dağıtım olduğu gibi kalır. Müşteriler, dağıtılan modelin yetkisini alabilir, ana hat 'in daha yeni bir sürümünü kullanarak readapt ve yeniden dağıtın.

**S: modelimi indirebilir ve yerel olarak çalıştırabilir miyim?**

Y **: modeller**yerel olarak indirilemez ve yürütülemez.

**S: isteklerim günlüğe kaydediliyor mi?**

**A**: varsayılan olarak istekler günlüğe kaydedilmez (ses veya döküm değil). Gerekirse, izlemeyi etkinleştirmek için [özel bir uç nokta oluşturduğunuzda](how-to-custom-speech-deploy-model.md) , *Bu uç nokta seçeneğinde günlük içeriği* ' ni seçebilirsiniz. Ardından istekler, güvenli depolama alanında Azure 'da günlüğe kaydedilir.

**S: isteklerim kısıtlandı mı?**

Y **: bkz**. [konuşma Hizmetleri kotaları ve limitleri](speech-services-quotas-and-limits.md).

**S: çift kanallı ses için nasıl ücret ödersiniz?**

Y: her **bir**kanalı ayrı ayrı gönderirseniz (her kanal kendi dosyasında), her bir dosyanın süresi üzerinden ücretlendirilirsiniz. Her bir kanalla çoğullanmış tek bir dosya gönderirseniz, tek dosya süresince ücretlendirilirsiniz. Fiyatlandırma hakkında ayrıntılı bilgi için lütfen Azure bilişsel [Hizmetler fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)bakın.

> [!IMPORTANT]
> Özel konuşma hizmetini kullanmanızı engelleyen başka gizlilik endişeleriniz varsa, destek kanallarından birine başvurun.

## <a name="increasing-concurrency"></a>Eşzamanlılık artırma
Bkz. [konuşma Hizmetleri kotaları ve sınırları](speech-services-quotas-and-limits.md).


## <a name="importing-data"></a>Verileri içeri aktarma

**S: bir veri kümesinin boyutuyla ilgili sınır nedir ve neden sınırla?**

Y **: sınır**, http karşıya yüklemesi için bir dosyanın boyutu kısıtlamasından kaynaklanır. Gerçek sınır için [konuşma Hizmetleri kotaları ve limitlerini](speech-services-quotas-and-limits.md) inceleyin.

**S: daha büyük bir metin dosyasını karşıya yükleyebilmem için metin dosyalarımı ZIP yapabilir miyim?**

Y **: Hayır**. Şu anda yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**S: veri raporu, başarısız olan bildirimler olduğunu söylüyor. Sorun nedir?**

**A**: bir dosyadaki çıkışların yüzde 100 ' unun karşıya yüklenememesi bir sorun değildir. Bir akustik veya dil veri kümesindeki (örneğin, yüzde 95 ' den fazla) en büyük çoğunluğunun başarıyla içeri aktarılmışsa, veri kümesi kullanılabilir olabilir. Ancak, neden başarısız olduğunu anlamaya ve sorunları nasıl giderebileceğinizi öneririz. Biçimlendirme hataları gibi yaygın sorunların çoğu düzeltilme kolaydır.

## <a name="creating-an-acoustic-model"></a>Akustik model oluşturma

**S: ne kadar akustik verilere ihtiyacım var?**

Y **: 30**dakika ile bir saat ve akustik veri arasında başlangıç yapmanız önerilir.

**S: hangi verileri toplamalıyım?**

Y **: uygulama**senaryosuna yakın olan verileri toplayın ve mümkün olduğunca büyük küçük harf kullanın. Veri toplama, hedef uygulamayla ve kullanıcılarla cihaz ya da cihaz, ortam ve tür hoparlörleriyle eşleşmelidir. Genel olarak, verileri mümkün olduğunca geniş bir hoparlörden toplayabilirsiniz.

**S: akustik verileri nasıl toplamalıyım?**

Y **: tek başına bir veri**toplama uygulaması oluşturabilir veya raf dışı ses kaydı yazılımını kullanabilirsiniz. Ayrıca, uygulamanızın ses verilerini günlüğe kaydettiği ve ardından verileri kullandığı bir sürümünü de oluşturabilirsiniz.

**S: verileri kendi kendinize uyarlama konusunda bir işlem yapmam gerekir mi?**

Y **: Evet**! Kendiniz düzenleyebilir veya profesyonel bir döküm hizmeti kullanabilirsiniz. Bazı kullanıcılar profesyonel onayları tercih eder ve diğerleri bu kişilerin kendi kendilerini kullanmasını ister.

## <a name="accuracy-testing"></a>Doğruluk testi

**S: özel bir dil modeli kullanarak özel akustik modelmin çevrimdışı testini yapabilir miyim?**

Y **: Evet**, çevrimdışı testi ayarlarken açılan menüden özel dil modelini seçmeniz yeterlidir.

**S: özel bir akustik model kullanarak özel dil modelimin çevrimdışı testini yapabilir miyim?**

Y **: Evet**, çevrimdışı testi ayarlarken açılan menüden özel akustik modeli seçmeniz yeterlidir.

**S: sözcük hata oranı nedir (WER) ve nasıl hesaplanır?**

Y **: wer**, konuşma tanıma için değerlendirme ölçümdür. WER, ekleme, silme ve değiştirmeler içeren toplam hata sayısı olarak sayılır, bu da başvuru dökümünde toplam sözcük sayısına bölünür. Daha fazla bilgi için bkz. [sözcük hata oranı](https://en.wikipedia.org/wiki/Word_error_rate).

**S: doğruluk testinin sonuçlarının iyi olup olmadığını Nasıl yaparım?.**

Y **: sonuçlar**, taban çizgisi modeliyle özelleştirdiğiniz model arasında bir karşılaştırma gösterir. Özelleştirmeyi bir süre için temel modeli oluşturmak üzere hedefleyebilirsiniz.

**S: bir geliştirme olup olmadığını görmek için bir temel modelin WER özelliğini Nasıl yaparım?.**

Y **: çevrimdışı**test sonuçları özel modelin taban çizgisi doğruluğunu ve taban çizgisinin iyileştirmesini gösterir.

## <a name="creating-a-language-model"></a>Dil modeli oluşturma

**S: ne kadar metin verisi karşıya yüklenemem gerekir?**

Y **: uygulamanızda**kullanılan sözlük ve tümceciklerin başlangıç dili modellerinden farklı olmasına bağlıdır. Tüm yeni kelimeler, bu sözcüklerin kullanımının mümkün olduğu sayıda örnek sağlamak yararlı olur. Uygulamanızda kullanılan yaygın ifadeler için, dil verilerinde ifadeler de dahil olmak üzere, sisteme bu koşulları da dinleyebildiğini söylediğinden de yararlı olur. Dil veri kümesinde en az 100 ve genellikle birkaç yüz ya da daha fazla utlik olması yaygındır. Ayrıca, bazı sorgu türlerinin diğerlerinden daha yaygın olması bekleniyorsa, ortak sorguların birden çok kopyasını veri kümesine ekleyebilirsiniz.

**S: yalnızca bir sözcük listesini karşıya yükleyebilir miyim?**

Y **: sözcüklerin bir listesini**karşıya yüklemek, sözcükleri sözlüğüne ekler, ancak sözcüklerin tipik olarak nasıl kullanıldığını sisteme öğretmez. Tam veya kısmi bir şekilde (kullanıcıların söyledikleri nesnelerin cümleler veya tümceleri), dil modelinin yeni kelimeleri ve nasıl kullanıldığını öğrenebilirsiniz. Özel dil modeli yalnızca sisteme yeni sözcükler eklemek için değil, aynı zamanda uygulamanız için bilinen sözcüklerin olasılığını ayarlamak için de uygundur. Tam dıklarýn sağlanması sistemin daha iyi öğrenilmesine yardımcı olur.

## <a name="tenant-model-custom-speech-with-microsoft-365-data"></a>Kiracı modeli (Microsoft 365 verilerle Özel Konuşma Tanıma)

**S: kiracı modeline hangi bilgiler dahildir ve nasıl oluşturulur?**

Y **:** Kiracı modeli, kuruluşunuzdaki herkes tarafından görülemeyen [ortak grup](https://support.microsoft.com/office/learn-about-microsoft-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2) e-postaları ve belgeleri kullanılarak oluşturulmuştur.

**S: kiracı modeli tarafından hangi konuşma deneyimleri geliştirildi?**

Y **:** Kiracı modeli etkinleştirildiğinde, oluşturulup yayımlandığında, konuşma hizmeti kullanılarak oluşturulan tüm kurumsal uygulamalarda tanımayı geliştirmek için kullanılır; Bu Ayrıca, kuruluşa üyelik belirten bir Kullanıcı Azure AD belirteci de iletir.

Dikte ve PowerPoint resim yazısı gibi Microsoft 365 yerleşik konuşma deneyimleri, konuşma hizmeti uygulamalarınız için bir kiracı modeli oluşturduğunuzda değiştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun giderme](troubleshooting.md)
- [Sürüm notları](releasenotes.md)
