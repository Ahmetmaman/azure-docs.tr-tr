---
title: "Hızlı Başlangıç: Java 'da Azure Search dizini oluşturma"
description: Java ve Azure Search REST API 'Leri kullanarak dizin oluşturmayı, verileri yüklemeyi ve sorguları çalıştırmayı açıklar.
services: search
author: jj09
manager: jlembicz
ms.service: search
ms.topic: conceptual
ms.date: 08/26/2018
ms.author: jjed
ms.custom: seodec2018, seo-java-july2019
ms.openlocfilehash: 7172cd01ca881ec3027854444107b0744b65feb3
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489784"
---
# <a name="quickstart-create-an-azure-search-index-in-java"></a>Hızlı Başlangıç: Java 'da Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Arama deneyimi için Azure Search kullanan özel bir Java arama uygulaması derlemeyi öğrenin. Bu öğretici, bu alıştırmada kullanılan nesneleri ve işlemleri oluşturmak için [Azure Search Hizmeti REST API'si](https://msdn.microsoft.com/library/dn798935.aspx)'ni kullanır.

Bu örneği çalıştırmak için, [Azure portalında](https://portal.azure.com) oturum açabileceğiniz bir Azure Search hizmetine sahip olmanız gerekir. Adım adım yönergeler için bkz. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md).

Bu örneği derlemek ve test etmek için aşağıdaki yazılımı kullandık:

* [Java EE Geliştiricileri için Eclipse IDE](https://www.eclipse.org/downloads/packages/release/photon/r/eclipse-ide-java-ee-developers). EE sürümünü indirdiğinizden emin olun. Doğrulama adımlarından biri, yalnızca bu sürümde bulunan bir özelliği gerektirir.
* [JDK 8u181](https://aka.ms/azure-jdks)
* [Apache Tomcat 8.5.33](https://tomcat.apache.org/download-80.cgi#8.5.33)

## <a name="about-the-data"></a>Veriler hakkında
Bu örnek uygulama, [Birleşik Devletler Jeoloji Hizmetleri (USGS)](https://geonames.usgs.gov/domestic/download_data.htm)'nin, veri kümesi boyutunu küçültmek için Rhode Island eyaletinde filtrelenen verilerini kullanır. Akarsular, göller ve zirveler gibi jeolojik özelliklerin yanı sıra, hastaneler ve okullar gibi önemli binaları döndüren bir arama uygulaması derlemek için bu verileri kullanacağız.

Bu uygulamada, **Searchservlet. Java** programı, filtrelenmiş USGS veri kümesini BIR Azure SQL veritabanından alarak Dizin [Oluşturucu](https://msdn.microsoft.com/library/azure/dn798918.aspx) yapısı kullanarak dizini oluşturur ve yükler. Çevrimiçi veri kaynağına yönelik önceden tanımlanmış kimlik bilgileri ve bağlantı bilgileri program kodu içinde sağlanır. Veri erişimi açısından ek yapılandırma gerekli değildir.

> [!NOTE]
> Ücretsiz fiyatlandırma katmanının 10.000 belge limiti altında kalmak için, bu veri kümesi üzerinde filtre uyguladık. Standart katmanı kullanırsanız bu limit uygulanmaz ve daha büyük bir veri kümesini kullanmak için bu kodu değiştirebilirsiniz. Her fiyatlandırma katmanının kapasitesi hakkında ayrıntılı bilgi için bkz: [Limitler ve kısıtlamalar](search-limits-quotas-capacity.md).
> 
> 

## <a name="about-the-program-files"></a>Program dosyaları hakkında
Aşağıdaki listede, bu örnek ile ilgili dosyalar açıklanmaktadır.

* Search. jsp: Kullanıcı arabirimini sağlar
* SearchServlet. Java: Yöntemler sağlar (MVC 'deki bir denetleyiciye benzer)
* SearchServiceClient. Java: HTTP isteklerini işler
* SearchServiceHelper. Java: Statik yöntemler sağlayan bir yardımcı sınıfı
* Document. Java: Veri modeli sağlar
* config. Properties: Arama Hizmeti URL 'sini ayarlar ve`api-key`
* Pod. xml: Maven bağımlılığı

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Hizmet adını ve `api-key` Azure Search hizmetinizi bulun
Tüm REST API çağrıları Azure Search, hizmet URL 'sini ve ' `api-key`yı sağlamanızı gerektirir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Atlama çubuğunda, aboneliğiniz için sağlanan tüm Azure Search hizmetlerini listelemek için **Search hizmeti**'ne tıklayın.
3. Kullanmak istediğiniz hizmeti seçin.
4. Hizmet panosunda, yönetici anahtarlarına erişim için anahtar simgesi ile birlikte önemli bilgiler için kutucuklar görürsünüz.
   
      ![Hizmet panosundan yönetici anahtarlarına nasıl erişediğinin gösterildiği ekran görüntüsü][3]
5. Hizmet URL'sini ve bir yönetici anahtarını kopyalayın. Bunlara daha sonra, bunları **config.properties** dosyasına eklerken ihtiyacınız olacak.

## <a name="download-the-sample-files"></a>Örnek dosyalarını indirme
1. GitHub’da [search-java-indexer-demo](https://github.com/Azure-Samples/search-java-indexer-demo) öğesine gidin.
2. **ZIP'i İndir**'e tıklayın, .zip dosyasını diske kaydedin ve ardından içerdiği tüm dosyaları ayıklayın. Daha sonra projeyi bulmayı kolaylaştırmak için, dosyaları Java çalışma alanınıza ayıklamayı göz önünde bulundurun.
3. Örnek dosyaları salt okunurdur. Klasör özelliklerine sağ tıklayın ve salt okunur özniteliğini kaldırın.

Sonraki tüm dosya değişiklikleri ve çalıştırma deyimleri bu klasördeki dosyalara uygulanır.  

## <a name="import-project"></a>Projeyi içeri aktarma
1. Eclipse'te, **File (Dosya)**  > **Import (İçeri Aktar)**  > **General (Genel)**  > **Existing Projects into Workspace (Var olan Projeleri Çalışma Alanına)** seçeneğini belirleyin.
   
    ![Mevcut projenin nasıl içeri aktarılacağını gösteren ekran görüntüsü][4]
2. **Select root directory (Kök dizini seç)** bölümünde örnek dosyaları içeren klasöre gidin. Proje klasörünü içeren klasörü seçin. Proje, **Projects (Projeler)** listesinde seçili öğe olarak görünmelidir.
   
    ![Projeleri Içeri aktar penceresindeki projeler listesini gösteren ekran görüntüsü][12]
3.           **Son**'a tıklayın.
4. Dosyaları görüntülemek ve düzenlemek için **Proje Gezgini**'ni kullanın. Zaten açık değilse **Pencere** > **Görünümü Göster** > **Proje Gezgini**'ne tıklayın veya açmak için kısayolu kullanın.

## <a name="configure-the-service-url-and-api-key"></a>Hizmet URL 'sini yapılandırın ve`api-key`
1. **Proje Gezgini**'nde, **config. Properties** ' e çift tıklayarak sunucu adı ve `api-key`içeren yapılandırma ayarlarını düzenleyin.
2. Artık `api-key` **config. Properties**' e gireceğiniz değerleri almak için, hizmet URL 'sini ve [Azure Portal](https://portal.azure.com)bulduğunuz Bu makalenin önceki adımlarına bakın.
3. **Config. Properties**dosyasında, "API anahtarı" nı hizmetinize `api-key` yönelik ile değiştirin. Sonra, hizmet adı (URL https://servicename.search.windows.net) 'nin ilk bileşeni "hizmet adı" ' nı aynı dosyada değiştirir.
   
    ![API anahtarının nasıl değiştirileceğini gösteren ekran görüntüsü][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Proje, derleme ve çalışma zamanı ortamlarını yapılandırma
1. Eclipse'te, Proje Gezgini'nde, proje > **Properties (Özellikler)**  > **Project Facets (Proje Modelleri)** öğesine sağ tıklayın.
2. **Dynamic Web Module (Dinamik Web Modülü)** öğesini, **Java**'yı ve **JavaScript**'i seçin.
   
    ![Projeniz için proje modellerinin nasıl seçileceğini gösteren ekran görüntüsü][6]
3. **Uygula**'ya tıklayın.
4. **Window (Pencere)**  > **Preferences (Tercihler)**  > **Server (Sunucu)**  > **Runtime Environments (Çalışma Zamanı Ortamları)**  > **Add... (Ekle...)** öğesini seçin.
5. Apache'yi genişletin ve önceden yüklediğiniz Apache Tomcat sunucusu sürümünü seçin. Sistemimizde sürüm 8 yüklüdür.
   
    ![Çalışma zamanı ortamı penceresinde, Apache Tomcat sürümünüzü seçebileceğiniz ekran görüntüsü][7]
6. Sonraki sayfada Tomcat yükleme dizinini belirtin. Windows bilgisayarda, bu büyük olasılıkla C:\Program Files\Apache Software Foundation\Tomcat *sürüm* olacaktır.
7.           **Son**'a tıklayın.
8. **Window (Pencere)**  > **Preferences (Tercihler)**  > **Java** > **Installed JREs (Yüklü JRE'ler)**  > **Add (Ekle)** seçeneğini belirleyin.
9. **Add JRE (JRE Ekle)** bölümünde **Standard VM (Standart VM)** öğesini seçin.
10.           **İleri**'ye tıklayın.
11. JRE Tanımı'nda, JRE giriş alanında **Directory (Dizin)** seçeneğine tıklayın.
12. **Program Files (Program Dosyaları)**  > **Java**'ya gidin ve daha önce yüklediğiniz JDK'yı seçin. JDK'yı JRE olarak seçmek önemlidir.
13. Yüklü JRE'ler içinde **JDK**'yı seçin. Ayarlarınız aşağıdaki ekran görüntüsüne benzer görünmelidir.
    
    ![Yüklü JRE olarak JDK 'nin nasıl seçileceğini gösteren ekran görüntüsü][9]
14. İsteğe bağlı olarak, uygulamayı bir dış tarayıcı penceresinde açmak için **Window (Pencere)**  > **Web Browser (Web Tarayıcısı)**  > **Internet Explorer**'ı seçin. Dış tarayıcı kullanmanız daha iyi bir Web uygulaması deneyimi sağlar.
    
    ![Internet Explorer 'ın dış Gözatma penceresi olarak nasıl seçileceğini gösteren ekran görüntüsü][8]

Yapılandırma görevlerini tamamladınız. Ardından, projeyi derleyip çalıştırırsınız.

## <a name="build-the-project"></a>Projeyi derleme
1. Proje Gezgini'nde proje adına sağ tıklayın ve projeyi yapılandırmak için **Run As (Farklı Çalıştır)**  > **Maven build... (Maven derlemesi...)** seçeneğini belirleyin.
   
    ![Proje Gezgini penceresinde Maven derlemesini seçme şeklini gösteren ekran görüntüsü][10]
2. Edit Configuration (Yapılandırmayı Düzenle) alanında Targets (Hedefler) için "clean install" ("temiz yükleme") yazın ve ardından **Run (Çalıştır)** düğmesine tıklayın.

Konsol penceresinde durum iletilerinin çıkışı alınır. Projenin hatasız olarak derlendiğini belirten DERLEME BAŞARILI iletisini görmeniz gerekir.

## <a name="run-the-app"></a>Uygulamayı çalıştırma
Bu son adımda, uygulamayı bir yerel sunucu çalışma zamanı ortamında çalıştırırsınız.

Eclipse'te henüz bir sunucu çalışma zamanı ortamı belirtmediyseniz öncelikle bunu yapmanız gerekir.

1. Proje Gezgini'nde **WebContent**'i genişletin.
2. **Search.jsp** > **Run As (Farklı Çalıştır)**  > **Run on Server (Sunucuda Çalıştır)** öğesine sağ tıklayın. Apache Tomcat sunucusunu seçin ve ardından **Run (Çalıştır)** öğesine tıklayın.

> [!TIP]
> Projenizi depolamak için varsayılan olmayan bir çalışma alanı kullandıysanız sunucu başlangıç hatasını önlemek için, **Run Configuration (Yapılandırmayı Çalıştır)** öğesini proje konumunu işaret edecek şekilde değiştirmeniz gerekir. Proje Gezgini'nde **Search.jsp** > **Run As (Farklı Çalıştır)**  > **Run Configurations (Yapılandırmaları Çalıştır)** öğesine sağ tıklayın. Apache Tomcat sunucusunu seçin. **Arguments (Bağımsız Değişkenler)** seçeneğine tıklayın. Projeyi içeren klasörü ayarlamak için **Workspace (Çalışma Alanı)** veya **File System (Dosya Sistemi)** seçeneğine tıklayın.
> 
> 

Uygulamayı çalıştırdığınızda, koşulları girmeniz için arama kutusu sağlayan bir tarayıcı penceresi görmeniz gerekir.

Dizini oluşturması ve yüklemesi için hizmete zaman tanımak amacıyla **Search (Ara)** seçeneğine tıklamadan önce yaklaşık bir dakika bekleyin. HTTP 404 hatası alırsanız yeniden denemeden önce biraz daha uzun süre beklemeniz gerekir.

## <a name="search-on-usgs-data"></a>USGS verilerinde arama
USGS veri kümesi, Rhode Island eyaleti ile ilgili kayıtları içerir. Boş bir arama kutusunda **Search (Ara)** düğmesine tıklarsanız varsayılan seçenek olan ilk 50 girişi alırsınız.

Bir arama terimi girmeniz arama alt yapısına gitmesi gereken bir hedef verir. Bölgesel bir ad girmeyi deneyin. "Roger Williams", Rhode Island'ın ilk valisiydi. Çok sayıda parka, binaya ve okula onun adı verildi.

![USGS verilerinde nasıl arama yapılacağını gösteren ekran görüntüsü][11]

Ayrıca, bu terimlerden herhangi birini de deneyebilirsiniz:

* Pawtucket
* Pembroke
* kaz + şapka

## <a name="next-steps"></a>Sonraki adımlar
Bu, Java ve USGS veri kümesini temel alan ilk Azure Search öğreticisidir. Zaman içinde, özel çözümlerinizde kullanmak isteyebileceğiniz ek arama özellikleri göstermek için bu öğreticiyi genişleteceğiz.

Zaten Azure Search ile ilgili belirli bir altyapınız varsa daha fazla deneyim için ([arama sayfası](search-pagination-page-layout.md)'nı büyütme veya [çok yönlü gezinme](search-faceted-navigation.md) gerçekleştirme gibi), bu örneği dayanak olarak kullanabilirsiniz. Ayrıca, numaralar ekleyerek ve belgeleri gruplayarak arama sonuçları sayfasını da geliştirebilirsiniz. Böylece, kullanıcılar sonuç sayfalarında gezinebilir.

Azure Search'ü ilk kez mi kullanıyorsunuz? Neler yapabileceğinizi anlamak için diğer öğreticileri denemenizi öneririz. Daha fazla kaynak bulmak için [belge sayfamızı](https://azure.microsoft.com/documentation/services/search/) ziyaret edin. 

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
