---
title: 'Hızlı başlangıç: .NET ile Tablo API - Azure Cosmos DB'
description: Bu hızlı başlangıçta Azure portalı ve .NET ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: sngun
ms.openlocfilehash: 629adfe558aec71e156e50c75aa0891eac5a8bcf
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240173"
---
# <a name="quickstart-build-a-table-api-app-with-net-sdk-and-azure-cosmos-db"></a>Hızlı başlatma: .NET SDK ve Azure Cosmos DB ile Tablo API uygulaması oluşturma 

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
>  

Bu hızlı başlangıçta GitHub’dan bir örneği kopyalayarak bir uygulama oluşturmak için .NET ve Azure Cosmos DB [Tablo API’sini](table-introduction.md) nasıl kullanacağınız gösterilmektedir. Bu hızlı başlangıçta ayrıca Azure Cosmos DB hesabı oluşturma ve web tabanlı Azure portalında tablo ve varlıklar oluşturmak için Veri Gezgini’ni kullanma da gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Visual Studio 2019 yüklü değilseniz, **ücretsiz** [Visual Studio 2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-table-add-sample-data](../../includes/cosmos-db-create-table-add-sample-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

   ```bash
   md "C:\git-samples"
   ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

   ```bash
   cd "C:\git-samples"
   ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

   ```bash
   git clone https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started.git
   ```

## <a name="open-the-sample-application-in-visual-studio"></a>Örnek uygulamayı Visual Studio'da açma

1. Visual Studio'da **Dosya** menüsünden **Aç**'ı ve ardından **Proje/Çözüm**'ü seçin. 

   ![Çözümü açma](media/create-table-dotnet/azure-cosmosdb-open-solution.png) 

2. Örnek uygulamayı klonladığınız klasöre gidin ve TableStorage.sln dosyasını açın.

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalda](https://portal.azure.com/)**Bağlantı Dizesi**’ne tıklayın. Pencerenin sağ tarafındaki kopyala düğmesini kullanarak **PRIMARY CONNECTION STRING**'i kopyalayın.

   ![Bağlantı Dizesi bölmesindeki PRIMARY CONNECTION STRING’i görüntüleyin ve kopyalayın](./media/create-table-dotnet/connection-string.png)

2. Visual Studio'da **Settings.json** dosyasını açın. 

3. **PORTALDAN BIRINCIL BAĞLANTI** STRING'ini StorageConnectionString değerine yapıştırın. Dizeyi tırnak işareti içinde yapıştırın.

   ```csharp
   {
      "StorageConnectionString": "<Primary connection string from Azure portal>"
   }
   ```

4. **Settings.json** dosyasını kaydetmek için CTRL+S tuşuna basın.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="build-and-deploy-the-app"></a>Uygulama derleme ve dağıtma

1. Visual Studio'da **Solution Explorer'daki** **CosmosTableSamples** projesine sağ tıklayın ve **ardından NuGet Paketlerini Yönet'e**tıklayın. 

   ![NuGet Paketlerini yönetme](media/create-table-dotnet/azure-cosmosdb-manage-nuget.png)

2. NuGet **Gözat** kutusuna Microsoft.Azure.Cosmos.Table yazın. Cosmos DB Table API istemci kitaplığı görüntülenir. Bu kitaplığın şu anda .NET Framework ve .NET Standard için kullanılabildiğini unutmayın. 
   
   ![NuGet Göz at sekmesi](media/create-table-dotnet/azure-cosmosdb-nuget-browse.png)

3. **Microsoft.Azure.Cosmos.Table** kitaplığını yüklemek için **Yükle'yi** tıklatın. Bunu yaptığınızda Azure Cosmos DB Tablo API paketi ve tüm bağımlılıklar yüklenir.

4. Uygulamanın tamamını çalıştırdığınızda, örnek veriler tablo varlığına eklenir ve sonunda silinir, böylece tüm örneği çalıştırdığınızda eklenen verileri görmezsiniz. Ancak, verileri görüntülemek için bazı kesme noktaları ekleyebilirsiniz. DosyaBasicSamples.cs açın ve 52. **Breakpoint** **Insert Breakpoint** 55. satıra da bir kesme noktası ekleyin.

   ![Kesme noktası ekleme](media/create-table-dotnet/azure-cosmosdb-breakpoint.png) 

5. Uygulamayı çalıştırmak için F5'e basın. Konsol penceresi Azure Cosmos DB'de yeni tablo veritabanının adını (bu durumda demoa13b1) görüntüler. 
    
   ![Konsol çıktısı](media/create-table-dotnet/azure-cosmosdb-console.png)

   İlk kesme noktasına ulaştığınızda Azure portalında Veri Gezgini'ne dönün. **Yenile** düğmesine tıklayın, demo* tablosunu genişletin ve **Varlıklar**'a tıklayın. Sağdaki **Varlıklar** sekmesinde Walter Harp için eklenmiş olan yeni varlık gösterilir. Yeni varlığın telefon numarasının 425-555-0101 olduğuna dikkat edin.

   ![Yeni varlık](media/create-table-dotnet/azure-cosmosdb-entity.png)
    
   Projeyi çalıştırırken Ayarlar.json dosyasının bulunamamasını belirten bir hata alırsanız, proje ayarlarına aşağıdaki XML girişini ekleyerek sorunu çözebilirsiniz. CosmosTableSamples'e sağ tıklayın, CosmosTableSamples.csproj'u edin ve aşağıdaki itemGroup'u ekleyin: 

   ```csharp
     <ItemGroup>
       <None Update="Settings.json">
         <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
       </None>
     </ItemGroup>
   ```

6. Veri Gezgini'nde **Varlıklar** sekmesini kapatın.
    
7. Uygulamayı bir sonraki kesme noktasına kadar çalıştırmak için F5'e basın. 

    Kesme noktasına ulaştığınızda Azure portalına dönün, yeniden **Varlıklar**'a tıklayarak **Varlıklar** sekmesini açın ve telefon numarasının 425-555-0105 olarak güncelleştirildiğine dikkat edin.

8. Uygulamayı çalıştırmak için F5'e basın. 
 
   Uygulama varlıkları Tablo API'si tarafından desteklenmeyen gelişmiş örnek uygulamada kullanılmak üzere ekler. Uygulama ardından örnek uygulama tarafından oluşturulan tabloyu siler.

9. Konsol penceresinde uygulamanın yürütülmesini sonlandırmak için Enter'a basın. 
  

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine içeri aktarma](table-import.md)

