---
title: Azure Izleyici ölçümleri ve otomatik ölçeklendirme için dağıtım API 'Lerini devre dışı bırakma
description: Azure hizmet yönetimi (ASM) veya RDFE dağıtım modelinin Kullanımdan kaldırılmış olarak da bilinen ölçümler ve otomatik ölçeklendirme klasik API 'Ler
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 0169de93b92de179c0ae9a36ff8dba3b7b1dc0fb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102049041"
---
# <a name="azure-monitor-retirement-of-classic-deployment-model-apis-for-metrics-and-autoscale"></a>Azure Izleyici, ölçümler ve otomatik ölçeklendirme için klasik dağıtım modeli API 'Lerinin kullanımdan kaldırılması

Azure Izleyici (önceki adıyla Azure Insights) Şu anda, için otomatik ölçeklendirme ayarlarını oluşturma ve yönetme ve klasik VM 'Ler ile klasik Cloud Services ölçümleri kullanma yeteneği sunuyor. Klasik dağıtım modeli tabanlı API 'lerin orijinal kümesi, tüm bölgelerde tüm Azure genel ve özel bulutlarda **30 haziran 2019 ' den sonra kullanımdan** kaldırılacaktır.   

Aynı işlemler, yıl boyunca bir dizi Azure Resource Manager tabanlı API aracılığıyla desteklenmelidir. Azure portal hem otomatik ölçeklendirme hem de ölçümler için yeni REST API 'Lerini kullanır. Bu Kaynak Yöneticisi API 'Lerine göre yeni bir SDK, PowerShell ve CLı de mevcuttur. İzleme için iş ortağı hizmetlerimiz, Azure Izleyici 'de yeni Kaynak Yöneticisi tabanlı REST API 'Leri kullanır.  

## <a name="who-is-not-affected"></a>Kim etkilenmedi

Otomatik ölçeklendirmeyi Azure portal, [yeni Azure izleyici SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/), POWERSHELL, clı veya Kaynak Yöneticisi şablonları aracılığıyla yönetiyorsanız, hiçbir eylem gerekmez.  

Azure portal veya çeşitli [izleme iş ortağı hizmetleri](../partners.md)aracılığıyla ölçümleri tüketerek, herhangi bir işlem yapmanız gerekmez. Microsoft, yeni API 'lere geçiş yapmak için izleme iş ortaklarıyla birlikte çalışmaktadır.

## <a name="who-is-affected"></a>Kim etkilendi

Aşağıdaki bileşenleri kullanıyorsanız, bu makale sizin için geçerlidir:

- **Klasik Azure Insights SDK 'sı** - [Klasik Azure Insights SDK 'sını](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/)kullanıyorsanız, [.net](https://github.com/azure/azure-libraries-for-net#download) veya [Java](https://github.com/azure/azure-libraries-for-java#download)için yeni Azure izleyici SDK 'sını kullanmaya geçin. [Azure izleyici SDK 'Sı NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)indirin.

- **Klasik otomatik ölçeklendirme** - [Klasik otomatik ölçeklendirme ayarları API 'lerini](/previous-versions/azure/reference/mt348562(v=azure.100)) özel olarak oluşturulmuş araçlardan veya [Klasik Azure Insights SDK 'sını](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/)kullanarak arıyorsanız [Azure izleyici REST API Kaynak Yöneticisi](/rest/api/monitor/autoscalesettings)kullanmaya geçmeniz gerekir.

- **Klasik ölçümler** -özel olarak oluşturulmuş araçlardan [Klasik REST API 'Leri](/previous-versions/azure/reference/dn510374(v=azure.100)) veya  [Klasik Azure Insights SDK 'Sını](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/) kullanarak ölçümler kullanıyorsanız [Azure izleyici REST API Kaynak Yöneticisi](/rest/api/monitor/autoscalesettings)kullanmaya geçmeniz gerekir. 

Kodunuzun veya özel araçlarınızın klasik API 'Leri çağırma konusunda emin değilseniz, aşağıdakilere bakın:

- Kodunuzda veya aracınız içinde başvurulan URI 'yi gözden geçirin. Klasik API 'Ler URI kullanır https://management.core.windows.net . Kaynak Yöneticisi tabanlı API 'Ler ile başlayan yeni URI 'yi kullanmanız gerekir `https://management.azure.com/` .

- Makinenizde derleme adını karşılaştırın. Klasik eski derleme  https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/ .

- Ölçüm veya otomatik ölçeklendirme API 'Lerine erişmek için sertifika kimlik doğrulaması kullanıyorsanız, klasik bir uç nokta ve kitaplık kullanıyorsunuz demektir. Yeni Kaynak Yöneticisi API 'Leri, hizmet sorumlusu veya Kullanıcı sorumlusu aracılığıyla Azure Active Directory kimlik doğrulaması gerektirir.

- Aşağıdaki bağlantılardan herhangi birinde belgelerde başvurulan çağrılar kullanıyorsanız, klasik eski API 'Leri kullanıyorsunuz demektir.

  - [Windows. Azure. Management. Monitoring sınıf kitaplığı](/previous-versions/azure/dn510414(v=azure.100))

  - [İzleme (klasik) .NET](/previous-versions/azure/reference/mt348562(v%3dazure.100))

  - [Imetricoperations arabirimi](/previous-versions/azure/reference/dn802395(v%3dazure.100))

## <a name="why-you-should-switch"></a>Neden geçmeniz gerekir?

Otomatik ölçeklendirme ve ölçümler için mevcut tüm yetenekler yeni API 'Ler aracılığıyla çalışmaya devam edecektir.  

Daha yeni API 'lere geçiş yapmak, tüm izleme hizmetleriniz genelinde tutarlı Azure rol tabanlı erişim denetimi (Azure RBAC) desteği gibi Kaynak Yöneticisi tabanlı yetenekler ile gelir. Ayrıca ölçümler için ek işlevler elde edersiniz: 

- Boyut desteği
- tüm hizmetlerde tutarlı 1 dakikalık ölçüm ayrıntı düzeyi 
- daha iyi sorgulama
- daha yüksek veri saklama (93 gün ölçüm ve 30 gün) 

Genel olarak, Azure 'daki diğer tüm hizmetler gibi Kaynak Yöneticisi tabanlı Azure Izleyici API 'Leri, daha iyi performans, ölçeklenebilirlik ve güvenilirlik ile gelir. 

## <a name="what-happens-if-you-do-not-migrate"></a>Geçiş yapmazsanız ne olur?

### <a name="before-retirement"></a>Kullanımdan kaldırmadan önce

Azure hizmetlerinizin veya iş yüklerinize doğrudan hiçbir etkisi olmayacaktır.  

### <a name="after-retirement"></a>Kullanımdan kaldırıldıktan sonra

Daha önce listelenen klasik API 'lere yapılan çağrılar başarısız olur ve aşağıdakine benzer hata iletileri döndürür:

Otomatik ölçeklendirme için: *Bu API kullanım dışı bırakılmıştır. Otomatik ölçeklendirme ayarlarını yönetmek için Azure portal, Azure Izleyici SDK, PowerShell, CLı veya Kaynak Yöneticisi şablonlarını kullanın*.  

Ölçümler için: *Bu API kullanım dışı bırakılmıştır. Ölçümleri sorgulamak için Azure Izleyici SDK 'Sı, PowerShell, CLı Azure portal kullanın*.

## <a name="email-notifications"></a>E-posta bildirimleri

Aşağıdaki hesap rolleri için e-posta adreslerine bir kullanımdan kaldırma bildirimi gönderildi: 

- Hesap ve hizmet yöneticileri
- Coadministrators  

Sorularınız varsa bizimle iletişime geçin MonitorClassicAPIhelp@microsoft.com .  

## <a name="references"></a>Başvurular

- [Azure Izleyici için daha yeni REST API 'Leri](/rest/api/monitor/) 
- [Daha yeni Azure Izleyici SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)

