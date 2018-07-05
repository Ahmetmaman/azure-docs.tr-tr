---
title: İstemci kitaplıklarının Azure Analysis Services'a bağlanmak için gerekli | Microsoft Docs
description: İstemci kitaplıklarının Azure Analysis Services bağlanmak istemci uygulamaları ve araçları için gereken açıklar
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 04b8ac9437b2859480b96ff5b46b34518dcc0883
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447277"
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Azure Analysis Services'a bağlanmak için istemci kitaplıkları

İstemci kitaplıkları, istemci uygulama ve araçların Analysis Services sunucularına bağlanmak için gereklidir. 

## <a name="download-the-latest-client-libraries-windows-installer"></a>En son istemci kitaplıkları (Windows Yükleyici) indirin  

|İndirme  |Ürün sürümü  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.0.1.492      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    15.0.1.492      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   15.1.0.0    |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    15.1.0.0     |

## <a name="amo-and-adomd-nuget-packages"></a>AMO ve ADOMD (NuGet paketlerini)

Analysis Services Management Objects'in (AMO) ve ADOMD istemci kitaplıkları vardır ve yüklenebilir paketleri olarak kullanılabilir [NuGet.org](https://www.nuget.org/). Windows Installer'ı kullanmak yerine NuGet başvuruları geçiş önerilir. 

|Paket  | Ürün sürümü  | 
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    15.1.0.0      |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   15.1.0.0      |

NuGet paket bütünleştirilmiş kodlarının AssemblyVersion semantic versioning izleyin: önemli. KÜÇÜK. DÜZELTME EKİ. Olsa bile farklı bir sürümünü (MSI yükle sonuçlanır) GAC'deki NuGet başvuruları beklenen sürüm yükleyin. Her sürüm için düzeltme eki artırılır. AMO veya ADOMD sürümleri eşitlenmiş olarak tutulur.

## <a name="understanding-client-libraries"></a>İstemci kitaplıkları anlama

Analysis Services veri sağlayıcıları olarak da bilinen üç istemci kitaplıklarını kullanın. ADOMD.NET ve Analysis Services Management Objects'in (AMO) yönetilen istemci kitaplıkları vardır. Analysis Services OLE DB sağlayıcısı (MSOLAP DLL), bir yerel istemci kitaplığıdır. Genellikle, tüm üç aynı anda yüklenir. **Azure Analysis Services, tüm üç kitaplıkları'nın son sürümlerini gerektirir**. 

Power BI Desktop ve Excel gibi Microsoft istemci uygulamaları, istemci kitaplıklarının üçünü yükleyin ve yeni sürümler kullanılabilir olduğunda bunları güncelleştirin. Sürüme veya güncelleştirme sıklığına bağlı olarak, bazı istemci kitaplıklarının Azure Analysis Services için gereken en son sürümlerine sahip olmayabilir. Aynı durum AsCmd, TOM, ADOMD.NET gibi özel uygulamalar ve diğer arabirimler için de geçerlidir. Bu uygulamalar, el ile veya programlama kitaplıkları yüklenmesi gerekir. El ile yükleme için istemci kitaplıkları SQL Server özellik paketleri'nde dağıtılabilir paketleri olarak dahil edilir. Ancak, bu istemci kitaplıkları, SQL Server sürümüne bağlıdır ve en son olmayabilir.  

İstemci bağlantıları için istemci kitaplıkları, bir Azure Analysis Services sunucusundan bir veri kaynağına bağlanmak için gereken veri sağlayıcıları farklıdır. Veri kaynağı bağlantıları hakkında daha fazla bilgi için bkz: [veri kaynağı bağlantıları](analysis-services-datasource.md).

## <a name="client-library-types"></a>İstemci Kitaplığı türleri

### <a name="analysis-services-ole-db-provider-msolap"></a>Analysis Services OLE DB sağlayıcısı (MSOLAP) 

 Analysis Services OLE DB sağlayıcısı (MSOLAP), Analysis Services veritabanı bağlantıları için yerel bir istemci kitaplığıdır. Ayrıca, ADOMD.NET hem de veri sağlayıcısına bağlantı istekleri devreden AMO, tarafından dolaylı olarak kullanılır. OLE DB sağlayıcısı doğrudan uygulama kodundan de çağırabilirsiniz.  
  
 Analysis Services OLE DB sağlayıcısı, araçları ve Analiz Hizmetleri veritabanları erişmek için kullanılan istemci uygulamaları tarafından otomatik olarak yüklenir. Analysis Services verilerine erişmek için kullanılan bilgisayarlara yüklenmesi gerekir.  
  
 OLE DB sağlayıcıları, genellikle bağlantı dizelerini belirtilir. OLE DB sağlayıcısı için başvuruda bulunmak için farklı bir terminoloji bir Analysis Services bağlantı dizesini kullanır: MSOLAP. \<sürüm > .dll.

### <a name="amo"></a>AMO  

 AMO, sunucu yönetimi ve veri tanımı için kullanılan bir yönetilen istemci kitaplığıdır. Yüklü ve araçları ve istemci uygulamaları tarafından kullanılan. Örneğin, SQL Server Management Studio (SSMS) AMO Analysis Services'e bağlanmak için kullanır. AMO kullanarak bağlantı oluşan genellikle minimal `“data source=\<servername>”`. Bağlantı kurulduktan sonra veritabanı koleksiyonları ve büyük nesneler ile çalışmak için API'yi kullanın. SSDT hem SSMS AMO bir Analysis Services örneğine bağlanmak için kullanın.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET Analysis Services veri sorgulamak için kullanılan bir yönetilen veri istemci kitaplığıdır. Yüklü ve araçları ve istemci uygulamaları tarafından kullanılan. 
  
 Bir veritabanına bağlanırken, tüm üç kitaplıkları için bağlantı dizesi özellikleri benzerdir. Neredeyse tüm bağlantı dizesi kullanarak için ADOMD.NET tanımlamak [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) AMO ve Analysis Services OLE DB sağlayıcısı (MSOLAP) için de çalışır. Daha fazla bilgi için bkz. [bağlantı dizesi özellikleri &#40;Analysis Services&#41;](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a> İstemci kitaplığı sürümü belirleme   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  `C:\Program Files\Microsoft Analysis Services\AS OLEDB\` kısmına gidin. Birden fazla klasör varsa, daha yüksek bir sayı seçin.
  
2.  Sağ **msolap.dll** > **özellikleri** > **ayrıntıları**. Dosya adı msolap140.dll ise, en son sürümden daha eski ve yükseltilmelidir.
    
    ![İstemci Kitaplığı ayrıntıları](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>AMO

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\` kısmına gidin. Birden fazla klasör varsa, daha yüksek bir sayı seçin.
2. Sağ **Microsoft.AnalysisServices** > **özellikleri** > **ayrıntıları**.  

### <a name="adomd"></a>ADOMD

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\` kısmına gidin. Birden fazla klasör varsa, daha yüksek bir sayı seçin.
2. Sağ **Microsoft.AnalysisServices.AdomdClient** > **özellikleri** > **ayrıntıları**.  


## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)
