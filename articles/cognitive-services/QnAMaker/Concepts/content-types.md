---
title: İçerik türleri-Soru-Cevap Oluşturma
description: İçerik türleri arasında PDF, DOCX ve TXT gibi birçok standart yapılandırılmış belge bulunur.
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 50924ce035d9a7dc7778cf45668dc993ee5486e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776995"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Bilgi tabanına ekleyebileceğiniz belgelerin içerik türleri
İçerik türleri arasında PDF, DOC ve TXT gibi birçok standart yapılandırılmış belge bulunur.

## <a name="file-and-url-data-types"></a>Dosya ve URL veri türleri

Aşağıdaki tabloda, Soru-Cevap Oluşturma tarafından desteklenen içerik ve dosya biçimlerinin türleri özetlenmektedir.

|Kaynak Türü|İçerik Türü| Örnekler|
|--|--|--|
|URL|SSS<br> (Düz, bölümler ile veya konu başlığı ile)<br>Destek sayfaları <br> (Tek sayfalı nasıl yapılır makaleleri, sorun giderme makaleleri vb.)|[Düz SSS](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[Bağlantılarla Ilgili SSS](https://www.microsoft.com/en-us/software-download/faq),<br> [Konularda SSS giriş sayfası](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Destek makalesi](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/BELGE|Sık<br> Ürün el kitabı,<br> Broşürler<br> T<br> El ilanı Ilkesi,<br> Destek Kılavuzu,<br> Yapılandırılmış QnA,<br> vb.|**Çoklu açma olmadan**<br>[Yapılandırılmış QnA.docx](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Örnek ürün Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Örnek semi-structured.docx](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Örnek beyaz paper.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**Çoklu açma**:<br>[Surface Pro (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Contoso avantajları (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Contoso avantajları (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|* Excel|Yapılandırılmış QnA dosyası<br> (RTF, HTML desteği dahil)|**Çoklu açma olmadan**:<br>[Örnek QnA FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)<br><br>**Çoklu açma**:<br>[Yapılandırılmış basit FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx)<br>[Surface dizüstü bilgisayar FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-Surface-Pro.xlsx)|
|* TXT/TSV|Yapılandırılmış QnA dosyası|[Örnek Chit-Chat. tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Veri kaynağınız için kimlik doğrulaması gerekiyorsa, bu içeriği Soru-Cevap Oluşturma almak için aşağıdaki yöntemleri göz önünde bulundurun:

* Dosyayı el ile indirin ve Soru-Cevap Oluşturma alın
* Kimliği doğrulanmış [SharePoint konumundan](../how-to/add-sharepoint-datasources.md) dosya Ekle

## <a name="url-content"></a>URL içeriği

Soru-Cevap Oluşturma **URL** aracılığıyla iki tür belge içeri aktarılabilir:

* SSS URL 'Leri
* Destek URL 'Leri

Her tür beklenen bir biçimi gösterir.

## <a name="file-based-content"></a>Dosya tabanlı içerik

[Soru-cevap oluşturma portalında](https://www.qnamaker.ai)ortak bir kaynaktan veya yerel dosya sisteminizden bir bilgi tabanına dosya ekleyebilirsiniz.

## <a name="content-format-guidelines"></a>İçerik biçimi yönergeleri

Farklı dosyalar için [Biçim yönergeleri](../reference-document-format-guidelines.md) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Hangi bilgilerin bir [soru ve yanıt (QnA) çiftinde](question-answer-set.md)depolandığını anlayın.
