---
title: Azure Logic Apps ile tümleşen bir işlev oluşturma
description: Tweet duyarlılığını kategorilere ayırmak ve duyarlılık düşük olduğunda bildirim göndermek için Azure Logic Apps ve Azure Bilişsel Hizmetler ile tümleşen bir işlev oluşturun.
author: craigshoemaker
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.topic: tutorial
ms.date: 04/27/2020
ms.author: cshoe
ms.custom: devx-track-csharp, mvc, cc996988-fb4f-47
ms.openlocfilehash: feb6b36f8e5e7bbec83d8882552484f68abfd56d
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91537761"
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Azure Logic Apps ile tümleşen bir işlev oluşturma

Azure İşlevleri, Logic Apps Tasarımcısı'nda Azure Logic Apps ile tümleşir. Bu tümleştirme, İşlevlerin bilgi işlem gücünü diğer Azure hizmetleri ve üçüncü taraf hizmetler ile yapılan düzenlemelerde kullanmanıza olanak sağlar. 

Bu öğreticide, Twitter gönderilerinden yaklaşım analizini çalıştırmak için Azure 'da Logic Apps ve bilişsel hizmetler ile Azure Işlevlerinin nasıl kullanılacağı gösterilmektedir. Bir HTTP tetikleyici işlevi, yaklaşım puanına göre yeşil, sarı veya kırmızı olarak kategorilere ayırır. Zayıf duyarlılık algılandığında bir e-posta gönderilir. 

![Logic App Tasarımcısı’nda uygulamanın ilk iki adımını gösteren görüntü](media/functions-twitter-email/00-logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Bilişsel Hizmetler API Kaynağı oluşturun.
> * Tweet duyarlılığını kategorilere ayıran bir işlev oluşturun.
> * Twitter’a bağlanan bir mantıksal uygulama oluşturun.
> * Mantıksal uygulamaya duyarlılık algılama özelliğini ekleyin. 
> * Mantıksal uygulamayı işleve bağlayın.
> * İşlevden alınan yanıta göre bir e-posta gönderin.

## <a name="prerequisites"></a>Önkoşullar

+ Etkin bir [Twitter](https://twitter.com/) hesabı. 
+ Bir [Outlook.com](https://outlook.com/) hesabı (bildirim göndermek için).

> [!NOTE]
> Gmail bağlayıcısını kullanmak istiyorsanız, yalnızca G-Suite iş hesapları bu bağlayıcıyı mantıksal uygulamalarda kısıtlama olmadan kullanabilir. Gmail tüketicisi hesabınız varsa, Gmail bağlayıcısını yalnızca belirli Google-onaylanan uygulamalar ve hizmetlerle kullanabilir ya da [Gmail bağlayıcısında kimlik doğrulaması için kullanmak üzere bir Google istemci uygulaması oluşturabilirsiniz](/connectors/gmail/#authentication-and-bring-your-own-application). Daha fazla bilgi için, bkz. [Azure Logic Apps Google bağlayıcıları Için veri güvenliği ve gizlilik ilkeleri](../connectors/connectors-google-data-security-privacy-policy.md).

+ Bu makalede, başlangıç noktası olarak [Azure portalında ilk işlevinizi oluşturma](functions-create-first-azure-function.md) bölümünde oluşturulan kaynaklar kullanılmaktadır.
Daha önce yapmadıysanız işlev uygulamanızı oluşturmak için bu adımları uygulayın.

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Bilişsel Hizmetler API'leri Azure’da tek kaynaklar halinde kullanılabilir. İzlenmekte olan tweetlerin duyarlılığını algılamak için Metin Analizi API’sini kullanın.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

3. **AI + Machine Learning**  >  **metin analizi**' ne tıklayın. Sonra, tabloda belirtilen ayarları kullanarak kaynağı oluşturun.

    ![Bilişsel kaynak sayfası oluşturma](media/functions-twitter-email/01-create-text-analytics.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | --- |
    | **Ad** | MyCognitiveServicesAccnt | Benzersiz bir hesap adı seçin. |
    | **Konum** | Batı ABD | Size en yakın konumu kullanın. |
    | **Fiyatlandırma katmanı** | F0 | En düşük katman ile başlayın. Çağrılarınız biterse daha yüksek bir katmana ölçeklendirin.|
    | **Kaynak grubu** | myResourceGroup | Bu öğreticideki tüm hizmetler için aynı kaynak grubunu kullanın.|

4. Kaynağınızı oluşturmak için **Oluştur**'a tıklayın. 

5. **Genel bakış** ' a tıklayın ve **uç noktanın** değerini bir metin düzenleyicisine kopyalayın. Bu değer, Bilişsel Hizmetler API’sine bağlantı oluşturulurken kullanılır.

    ![Bilişsel Hizmetler Ayarları](media/functions-twitter-email/02-cognitive-services.png)

6. Sol gezinti sütununda **Anahtarlar**’a tıklayın ve sonra **Anahtar 1** değerini metin düzenleyicisinde bir yere yazın. Anahtarı kullanarak mantıksal uygulamayı Bilişsel Hizmetler API'sine bağlayabilirsiniz. 
 
    ![Bilişsel Hizmetler Anahtarları](media/functions-twitter-email/03-cognitive-serviecs-keys.png)

## <a name="create-the-function-app"></a>İşlev uygulaması oluşturma

Azure Işlevleri, bir Logic Apps iş akışında işleme görevlerinin yükünü boşaltmanız için harika bir yol sağlar. Bu öğretici, bilişsel hizmetlerden Tweet yaklaşım puanlarını işlemek ve bir kategori değeri döndürmek için bir HTTP tetikleyici işlevi kullanır.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-trigger-function"></a>HTTP tetikleyici işlevi oluşturma  

1. **İşlevler** penceresinin sol menüsünde **işlevler**' i seçin ve ardından üst menüden **Ekle** ' yi seçin.

2. **Yeni işlev** penceresinden **http tetikleyicisi**' ni seçin.

    ![HTTP tetikleyici işlevini seçin](./media/functions-twitter-email/06-function-http-trigger.png)

3. **Yeni işlev** sayfasında, **işlev oluştur**' u seçin.

4. Yeni HTTP tetikleyici işlevinizde, sol menüden **kod + test** ' i seçin, `run.csx` dosyanın içeriğini aşağıdaki kodla değiştirin ve ardından **Kaydet**' i seçin:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        string category = "GREEN";
    
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        log.LogInformation(string.Format("The sentiment score received is '{0}'.", requestBody));
    
        double score = Convert.ToDouble(requestBody);
    
        if(score < .3)
        {
            category = "RED";
        }
        else if (score < .6) 
        {
            category = "YELLOW";
        }
    
        return requestBody != null
            ? (ActionResult)new OkObjectResult(category)
            : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    }
    ```

    Bu işlev kodu, istekte alınan duyarlılık puanına göre bir renk kategorisi döndürür. 

5. İşlevi test etmek için üstteki menüden **Test** ' i seçin. **Giriş** sekmesinde, `0.2` **gövdede**bir değer girin ve sonra **Çalıştır**' ı seçin. **Çıkış** sekmesindeki **http yanıtı içeriğinde** **Red** değeri döndürülür. 

    :::image type="content" source="./media/functions-twitter-email/07-function-test.png" alt-text="Proxy ayarlarını tanımlama":::

Artık duyarlılık puanlarını kategorilere ayıran bir işleviniz vardır. bundan sonra, işlevinizi Twitter ve Bilişsel Hizmetler API’niz ile tümleştiren bir mantıksal uygulama oluşturun. 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma   

1. Azure portal, Azure portal sol üst köşesinde bulunan **kaynak oluştur** düğmesine tıklayın.

2. **Web**  >  **mantıksal uygulaması**' na tıklayın.
 
3. Ardından **Ad** alanına `TweetSentiment` gibi bir değer yazın ve tabloda belirtilen ayarları kullanın.

    ![Azure portalda mantıksal uygulama oluşturma](./media/functions-twitter-email/08-logic-app-create.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | ----------------- | ------------ | ------------- |
    | **Ad** | TweetSentiment | Uygulamanız için uygun bir ad seçin. |
    | **Kaynak grubu** | myResourceGroup | Daha önceki ile aynı mevcut kaynak grubunu seçin. |
    | **Konum** | Doğu ABD | Size yakın bir konum seçin. |    

4. Uygun ayar değerlerini girdikten sonra mantıksal uygulamanızı oluşturmak için **Oluştur**’a tıklayın. 

5. Uygulama oluşturulduktan sonra, panoya sabitlenmiş yeni mantıksal uygulamanıza tıklayın. Ardından Logic Apps Tasarımcısı'nda sayfayı aşağı kaydırıp **Boş Mantıksal Uygulama** şablonuna tıklayın. 

    ![Boş Mantıksal Uygulama şablonu](media/functions-twitter-email/09-logic-app-create-blank.png)

Artık Logic Apps Tasarımcısı'nı kullanarak uygulamanıza hizmetler ve tetikleyiciler ekleyebilirsiniz.

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma

İlk olarak Twitter hesabınızla bir bağlantı oluşturun. Mantıksal uygulama tweetleri yoklar ve uygulamanın çalışması tetiklenir.

1. Tasarımcıda **Twitter** hizmetine ve **Yeni bir tweet gönderildiğinde** tetikleyicisine tıklayın. Twitter hesabınızda oturum açın ve Logic Apps’in hesabınızı kullanmasına izin verin.

2. Tabloda belirtilen Twitter tetikleyici ayarlarını kullanın. 

    ![Twitter bağlayıcı ayarları](media/functions-twitter-email/10-tweet-settings.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | ----------------- | ------------ | ------------- |
    | **Arama metni** | #Azure | Seçilen aralıkta yeni tweetler oluşturmak için yeterince popüler olan bir diyez etiketi kullanın. Ücretsiz katmanını kullanırken diyez etiketiniz çok popüler olursa, Bilişsel Hizmetler API'nizdeki işlem kotasını hızlı bir şekilde tüketebilirsiniz. |
    | **Aralık** | 15 | Sıklık birimleri cinsinden Twitter istekleri arasında geçen süre. |
    | **Sıklık** | Dakika | Twitter’ı yoklamak için kullanılan sıklık birimi.  |

3.  Twitter hesabınıza bağlanmak için **Kaydet**’e tıklayın. 

Uygulamanız artık Twitter’a bağlıdır. Ardından, toplanan tweetlerin duyarlılığını algılamak için metin analizine bağlanın.

## <a name="add-sentiment-detection"></a>Duyarlılık algılama ekleme

1. **Yeni Adım**’a ve sonra **Eylem ekle**’ye tıklayın.

2. **Eylem seçin** alanına **Metin Analizi** yazın ve sonra **Duyarlılığı algıla** eylemine tıklayın.
    
    ![Arama kutusunda "Metin Analizi" ve "yaklaşım algılama" eyleminin seçildiği "Eylem Seç" bölümünü gösteren ekran görüntüsü. ](media/functions-twitter-email/11-detect-sentiment.png)

3. `MyCognitiveServicesConnection` gibi bir bağlantı adı yazın, metin düzenleyicide kaydettiğiniz Bilişsel Hizmetler API’nizin ve Bilişsel Hizmetler uç noktasının anahtarını yapıştırın, sonra **Oluştur**’a tıklayın.

    ![Yeni Adım ve sonra Eylem ekle](media/functions-twitter-email/12-connection-settings.png)

4. Sonra, metin kutusuna **Tweet metni** girin ve ardından **yeni adım**' a tıklayın.

    ![Analiz edilecek metni tanımlama](media/functions-twitter-email/13-analyze-tweet-text.png)

Duyarlılık algılaması yapılandırıldıktan sonra işlevinize duyarlılık puanı çıktısını kullanan bir bağlantı ekleyebilirsiniz.

## <a name="connect-sentiment-output-to-your-function"></a>Duyarlılık çıktısını işlevinize bağlama

1. Logic Apps tasarımcısında **yeni adım**  >  **Eylem Ekle**' ye tıklayın, **Azure işlevleri** 'ne filtre uygulayın ve **bir Azure işlevi seçin**' e tıklayın.

    ![Duyarlılığı Algıla](media/functions-twitter-email/14-azure-functions.png)
  
4. Daha önce oluşturduğunuz işlev uygulamasını seçin.

    ![İşlev uygulaması seçili olan "Eylem Seç" bölümünü gösteren ekran görüntüsü.](media/functions-twitter-email/15-select-function.png)

5. Bu öğretici için oluşturduğunuz işlevi seçin.

    ![İşlev seçme](media/functions-twitter-email/16-select-function.png)

4. **İstek Gövdesi**’nde **Puan**’a ve ardından **Kaydet**’e tıklayın.

    ![Puan](media/functions-twitter-email/17-function-input-score.png)

Artık mantıksal uygulamadan bir duyarlılık puanı gönderildiğinde işleviniz tetiklenir. İşlev tarafından mantıksal uygulamaya renk kodlu bir kategori döndürülür. Daha sonra, işlevden **RED** duyarlılık değeri döndürüldüğünde gönderilen bir e-posta bildirimi ekleyin. 

## <a name="add-email-notifications"></a>E-posta bildirimleri ekleme

İş akışının son parçası, duyarlılık puanı _RED_ olduğunda bir e-postanın tetiklenmesidir. Bu makalede bir Outlook.com Bağlayıcısı kullanılmaktadır. Gmail veya Office 365 Outlook bağlayıcısını kullanmak için benzer adımlar gerçekleştirebilirsiniz.   

1. Logic Apps tasarımcısında **yeni adım**  >  **Koşul Ekle**' ye tıklayın. 

    ![Mantıksal uygulamaya koşul ekleyin.](media/functions-twitter-email/18-add-condition.png)

2. **Bir değer seçin** ve ardından **Gövde**’ye tıklayın. **Eşittir**’e tıklayın, **Bir değer seçin**’e tıklayın ve `RED` yazıp **Kaydet**’e tıklayın. 

    ![Koşul için bir eylem seçin.](media/functions-twitter-email/19-condition-settings.png)    

3. **IF TRUE** alanında **Eylem ekle**’ye tıklayın, `outlook.com` ifadesini arayın, **E-posta gönder**’e tıklayın ve Outlook.com hesabınızda oturum açın.

    !["Outlook.com" başlıklı ve "e-posta gönder" eylemi seçili olan "Eğer TRUE" bölümünü gösteren ekran görüntüsü.](media/functions-twitter-email/20-add-outlook.png)

    > [!NOTE]
    > Bir Outlook.com hesabınız yoksa Gmail veya Office 365 Outlook gibi başka bir bağlayıcı seçebilirsiniz

4. **E-posta gönder** eyleminde tabloda belirtilen e-posta ayarlarını kullanın. 

    ![E-posta gönderme eylemi için e-postayı yapılandırın.](media/functions-twitter-email/21-configure-email.png)
    
| Ayar      |  Önerilen değer   | Açıklama  |
| ----------------- | ------------ | ------------- |
| **Amaç** | E-posta adresinizi girin | Bildirimi alan e-posta adresi. |
| **Konu** | Olumsuz tweet duyarlılığı algılandı  | E-posta bildiriminin konu satırı.  |
| **Gövde** | Tweet metni, Konum | **Tweet metni** ve **Konum** parametrelerine tıklayın. |

1. **Kaydet**’e tıklayın.

İş akışı tamamlandığına göre mantıksal uygulamayı etkinleştirip işlevin nasıl çalıştığını görebilirsiniz.

## <a name="test-the-workflow"></a>İş akışını test etme

1. Logic Apps Tasarımcısı'nda, uygulamayı başlatmak için **Çalıştır**'a tıklayın.

2. Sol sütunda **Genel Bakış**’a tıklayarak mantıksal uygulamanın durumuna bakın. 
 
    ![Mantıksal uygulama yürütme durumu](media/functions-twitter-email/22-execution-history.png)

3. (İsteğe bağlı) Yürütme ayrıntılarını görmek için çalışmalardan birine tıklayın.

4. İşlevinize gidin, günlükleri görüntüleyin ve duyarlılık değerlerinin alınıp işlendiğini doğrulayın.
 
    ![İşlev günlüklerini görüntüleme](media/functions-twitter-email/sent.png)

5. Olumsuz olabilecek bir duyarlılık algılandığında bir e-posta alırsınız. Bir e-posta almadıysanız işlev kodunu her zaman RED döndürecek şekilde değiştirebilirsiniz:

    ```csharp
    return (ActionResult)new OkObjectResult("RED");
    ```

    E-posta bildirimlerini doğruladıktan sonra özgün koda geri dönün:

    ```csharp
    return requestBody != null
        ? (ActionResult)new OkObjectResult(category)
        : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    ```

    > [!IMPORTANT]
    > Bu öğreticiyi tamamladıktan sonra mantıksal uygulamayı devre dışı bırakmanız gerekir. Uygulamayı devre dışı bırakarak yürütmeler için sizden ücret alınmasını ve Bilişsel Hizmetler API'nizdeki işlemlerin tükenmesini önlersiniz.

Artık Işlevleri bir Logic Apps iş akışı ile tümleştirmede ne kadar kolay olduğunu gördünüz.

## <a name="disable-the-logic-app"></a>Mantıksal uygulamayı devre dışı bırakma

Mantıksal uygulamayı devre dışı bırakmak için **Genel Bakış** ve sonra ekranın üstündeki **Devre Dışı Bırak**’a tıklayın. Uygulamayı devre dışı bıraktığınızda, uygulama silinmeden mantıksal uygulamanın çalışması ve ücretlerin yansıtılması durdurulur.

![İşlev günlükleri](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir Bilişsel Hizmetler API Kaynağı oluşturun.
> * Tweet duyarlılığını kategorilere ayıran bir işlev oluşturun.
> * Twitter’a bağlanan bir mantıksal uygulama oluşturun.
> * Mantıksal uygulamaya duyarlılık algılama özelliğini ekleyin. 
> * Mantıksal uygulamayı işleve bağlayın.
> * İşlevden alınan yanıta göre bir e-posta gönderin.

İşlevinize yönelik sunucusuz bir API oluşturma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"] 
> [Azure İşlevleri'ni kullanarak sunucusuz bir API oluşturma](functions-create-serverless-api.md)

Logic Apps hakkında daha fazla bilgi için bkz. [Azure Logic Apps](../logic-apps/logic-apps-overview.md).
