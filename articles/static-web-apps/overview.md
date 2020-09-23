---
title: Statik Web Uygulamaları nedir?
description: Azure statik Web Apps 'nin temel özellikleri ve işlevleri.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 086feb3aa7c9a38756fded58538eb22e094a5960
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906800"
---
# <a name="what-is-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemesi nedir?

Azure statik Web Apps, tam yığın Web uygulamalarını otomatik olarak oluşturup bir GitHub deposundan Azure 'a dağıtan bir hizmettir.

:::image type="content" source="media/overview/static-apps-overview.png" alt-text="Statik Web Apps genel bakış":::

Azure statik Web Apps iş akışı, geliştiricinin günlük iş akışına göre tasarlanmıştır. Uygulamalar GitHub etkileşimini temel alarak oluşturulup dağıtılır.

Bir Azure Statik Web Uygulamaları kaynağı oluşturduğunuzda Azure uygulamanın kaynak kodu deposunda bir GitHub Actions iş akışı ayarlar ve bu iş akışı seçtiğiniz dalı izler. Değişiklikleri izlenen dala her gönderişinizde veya çekme isteklerini kabul ettiğinizde, GitHub eylemi uygulamanızı ve API 'sini Azure 'a otomatik olarak oluşturur ve dağıtır.

Statik Web uygulamaları, genellikle angular, tepki, svelte, Vue veya Blazor gibi kitaplıklar ve çerçeveler kullanılarak oluşturulmuştur. Bu uygulamalar, uygulamayı oluşturan HTML, CSS, JavaScript ve görüntü varlıklarını içerir. Geleneksel bir Web sunucusu ile, bu varlıklar, gerekli tüm API uç noktaları ile birlikte tek bir sunucudan sunulur.

Statik Web Apps, statik varlıklar geleneksel bir Web sunucusundan ayrılır ve bunun yerine dünyanın dört bir yanındaki coğrafi olarak dağıtılan noktalarından sunulur. Dosyalar son kullanıcılara fiziksel olarak daha yakında bulunduğu için bu dağıtım yöntemi dosyaların daha hızlı sunulmasına olanak tanır. Ayrıca, API uç noktaları [sunucusuz bir mimari](../azure-functions/functions-overview.md)kullanılarak barındırılır ve tamamen bir arka uç sunucusu gereksinimini ortadan kaldırır.

## <a name="key-features"></a>Önemli özellikler

- HTML, CSS, JavaScript ve görüntüler gibi statik içerik için **Web barındırma** .
- Azure Işlevleri tarafından sunulan **TÜMLEŞIK API** desteği.
- Depo değişikliklerinin derlemeleri ve dağıtımları tetikleyen **birinci sınıf GitHub tümleştirmesi** .
- **Genel olarak dağıtılmış** statik içerik, içeriği kullanıcılarınıza yaklaştırır.
- Otomatik olarak yenilenen **ÜCRETSIZ SSL sertifikaları**.
- Uygulamanıza markalı özelleştirmeler sağlamak için **özel etki alanları** .
- CORS yapılandırması gerektirmeyen API 'Ler çağrılırken ters proxy ile **sorunsuz güvenlik modeli** .
- Azure Active Directory, Facebook, Google, GitHub ve Twitter ile **kimlik doğrulama sağlayıcısı tümleştirmeleri** .
- **Özelleştirilebilir yetkilendirme rolü tanımı** ve atamaları.
- **Arka uç yönlendirme kuralları** , size kullandığınız içerik ve yollar üzerinde tam denetimi etkinleştirir.
- Çekme istekleri tarafından desteklenen **üretilen hazırlama sürümleri** , yayımlamadan önce sitenizin önizleme sürümlerini etkinleştirir.

## <a name="what-you-can-do-with-static-web-apps"></a>Statik Web Apps ile yapabilecekleriniz

- JavaScript çerçeveleri ve bir [Azure işlevleri](apis.md) arka ucu Ile WebAssembly uygulamaları [React](getting-started.md?tabs=react)oluşturmak Için [Svelte](https://docs.microsoft.com/learn/modules/publish-app-service-static-web-app-api/) [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) [kullanma gibi](getting-started.md?tabs=angular)JavaScript [Vue](getting-started.md?tabs=react)çerçeveleri ve kitaplıkları ile **Modern Web uygulamaları** oluşturun.
- [Gatsby](publish-gatsby.md), [kugo](publish-hugo.md), [vuepress](publish-vuepress.md)gibi çerçeveler ile **statik siteler yayımlayın** .
- [Next.js](deploy-nextjs.md) ve [Nuxt.js](deploy-nuxtjs.md)gibi çerçeveler ile **Web uygulamaları dağıtın** .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk statik uygulamanızı oluşturma](getting-started.md)
