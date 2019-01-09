---
title: Azure Application Insights içinde kaynaklar, roller ve erişim denetimi | Microsoft Docs
description: Sahipleri, Katkıda Bulunanlar ve okuyucular kuruluşunuzun ınsights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: mbullwin
ms.openlocfilehash: 023f0e560900aa582be1f28e553358adb0c87b1e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118481"
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Kaynaklar, roller ve erişim denetimi Application ınsights

Oku ve erişim verilerinizi azure'da güncelleştirmesi denetleyebilirsiniz [Application Insights][start], kullanarak [Microsoft Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md).

> [!IMPORTANT]
> Kullanıcılara erişim atama **kaynak grubuna veya aboneliğe** uygulama kaynağınızı - kaynak kendisi değil ait olduğu. Ata **Application Insights bileşeni Katılımcısı** rol. Bu web testleri ve uygulama kaynağınızı uyarılarının erişim Tekdüzen denetimi sağlar. [Daha fazla bilgi edinin](#access).

## <a name="resources-groups-and-subscriptions"></a>Kaynakları, grupları ve abonelikler

İlk olarak, bazı tanımları:

* **Kaynak** - Microsoft Azure hizmet örneğini bir. Application Insights kaynağınıza toplar, çözümler ve uygulamanızdan gönderilen telemetri verilerini görüntüler.  Diğer Azure kaynak türlerini, web uygulamaları, veritabanları ve VM'ler içerir.
  
    Kaynaklarınızı görmek için [Azure portalında][portal], oturum açın ve tüm kaynaklar'ı tıklatın. Bir kaynağı bulmak için filtre alanda adının bir kısmını yazın.
  
    ![Azure kaynaklarının listesi](./media/resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Kaynak grubu** ] [ group] -her kaynak bir gruba ait. Bir grup, erişim denetimi için özellikle ilgili kaynakları yönetmek için kullanışlı bir yoldur. Örneğin, bir kaynak grubuna bir Web uygulaması, uygulamayı izlemek için Application Insights kaynağı ve depolama kaynağı dışarı aktarılan verileri tutmak için yerleştirebilirsiniz.

* [**Abonelik** ](https://portal.azure.com) - bir Azure aboneliği için oturum açın, Application Insights veya diğer Azure kaynakları kullanmak için. Her kaynak grubu, fiyat paketinizi seçin ve burada, bir kuruluş aboneliği ise, üyeleri ve bunların erişim izinlerini seçin. bir Azure aboneliğine ait.
* [**Microsoft hesabı** ] [ account] -kullanıcı adı ve Microsoft Azure Abonelikleri, XBox Live, Outlook.com ve diğer Microsoft hizmetlerinde oturum açmak için kullandığınız parola.

## <a name="access"></a> Kaynak grubundaki erişimi denetleme

Uygulamanız için oluşturduğunuz kaynak yanı sıra olduğunu da uyarıları ve web testleri ayrı gizli kaynakları anlamak önemlidir. Aynı eklenmiş [kaynak grubu](#resource-group) uygulamanızla. Ayrıca diğer Azure Hizmetleri orada, Web siteleri veya depolama gibi konumuna.

Bu kaynaklara erişimi denetlemek için bu nedenle önerilir:

* Erişim denetimi **kaynak grubuna veya aboneliğe** düzeyi.
* Ata **Application Insights bileşeni Katılımcısı** kullanıcılara rol. Bu gruptaki diğer hizmetlere erişimi sağlamadan web testleri, uyarılar ve Application Insights kaynaklarını düzenlenmesine olanak sağlar.

## <a name="to-provide-access-to-another-user"></a>Başka bir kullanıcıya erişim sağlamak için

Abonelik veya kaynak grubu sahibi hakları olmalıdır.

Kullanıcının olmalıdır bir [Microsoft Account][account], veya erişim kendi [Kurumsal Microsoft Account](../../active-directory/fundamentals/sign-up-organization.md). Bireylere ve Azure Active Directory'de tanımlanan kullanıcı gruplarına erişim sağlayabilir.

#### <a name="navigate-to-resource-group-or-directly-to-the-resource-itself"></a>Kaynak grubu veya doğrudan kaynak gidin

Seçin **erişim denetimi (IAM)** sol taraftaki menüden.

![Azure portalında erişim ekran denetimi düğmesi](./media/resources-roles-access-control/0001-access-control.png)

Seçin **rol ataması Ekle**

![Ekle düğmesi kırmızı renkte vurgulanmış ekran erişim denetimi menüsü](./media/resources-roles-access-control/0002-add.png)

**İzinleri eklemek** öncelikle belirli aşağıdaki görünüm Application Insights kaynakları için erişim denetimi izinleri gibi kaynak grupları, daha yüksek bir düzeyinden görüntülemekte olduğunuz ederseniz ek uygulama olmayan görürsünüz Insights merkezli roller.

Yerleşik rolleri kullanabileceğiniz tüm Azure rol tabanlı erişim denetimi hakkında bilgileri görüntülemek için [resmi başvuru içeriği](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

![Ekran görüntüsü, erişim denetim kullanıcı rolü listesi](./media/resources-roles-access-control/0003-user-roles.png)

#### <a name="select-a-role"></a>Rol seçin

Uygunsa, biz ilişkili resmi başvuru belgelerinin bağlantısı.

| Rol | Kaynak grubunda |
| --- | --- |
| [Sahip](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) |Kullanıcı erişim dahil her şeyi değiştirebilir. |
| [Katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) |Tüm kaynaklar da dahil olmak üzere her şeyi düzenleyebilirsiniz. |
| [Application Insights bileşeni Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-component-contributor) |Application Insights kaynakları, web testleri ve uyarılar düzenleyebilirsiniz. |
| [Okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) |Görüntüleyebilir ancak değişikliği yok. |
| [Application Insights Snapshot Debugger](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-snapshot-debugger) | Application Insights Snapshot Debugger özelliklerini kullanma izni verir. Bu role sahip ne katkıda bulunan rolü eklendiğini unutmayın. |
| Azure Service Deploy Release Management Contributor | Azure hizmetini dağıtma dağıtma Hizmetleri için katkıda bulunan rolü. |
| [Veri Purger](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#data-purger) | Kişisel verileri temizleme özel rol. Bkz. bizim [kişisel verilere yönelik rehberlik](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data) daha fazla bilgi için.   |
| ExpressRoute yönetici | Delete oluşturabilir ve express rotaları yönetme.|
| [Log Analytics katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-contributor) | Log Analytics katkıda bulunan tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyin. İzleme ayarlarını düzenleme Vm'lere VM uzantısı ekleme içerir; Azure Depolama'dan günlüklerin toplanmasını yapılandırma yapabilmek için depolama hesabı anahtarlarını okuma; oluşturma ve Otomasyon hesapları yapılandırma; çözümler eklenerek; ve tüm Azure kaynaklarında Azure tanılamayı yapılandırma.  |
| [Log Analytics okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader) | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
| masterreader | Her şeyi görüntüleyebilir ancak değişiklik yapamazlar açmasına olanak sağlar. |
| [İzleme katkıda bulunanı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) | Tüm izleme verilerini okuyabilir ve izleme ayarlarını güncelleştirebilir. |
| [İzleme ölçümlerini yayımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-metrics-publisher) | Azure kaynaklarına karşı ölçümleri yayımlama sağlar. |
| [İzleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) | Tüm izleme verilerini okuyabilir. |
| Kaynak İlkesine Katkıda Bulunan (Önizleme) | EA, kaynak ilkesi oluşturma/değiştirme haklarıyla Ea'dan kullanıcılar destek bileti oluşturun ve kaynak/hiyerarşiyi okuma.  |
| [Kullanıcı Erişimi Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) | Diğer kullanıcılara Azure kaynaklarına erişimi yönetmek üzere bir kullanıcı sağlar.|
| [Web sitesi Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#website-contributor) | Web sitelerini (web planlarını değil), ancak onlara yönelik erişimi yönetmenize olanak tanır...|

'Düzenleme' oluşturma, silme ve güncelleştirme içerir:

* Kaynaklar
* Web testleri
* Uyarılar
* Sürekli dışarı aktarma

#### <a name="select-the-user"></a>Kullanıcıyı seçin

İstediğiniz kullanıcının dizinde değilse, bir Microsoft hesabı olan herkes davet edebilirsiniz.
(Bunlar Outlook.com, OneDrive, Windows Phone veya XBox Live gibi hizmetleri kullanıyorsanız, bunlar bir Microsoft hesabınız vardır.)

## <a name="related-content"></a>İlgili içerik

* [Rol tabanlı erişim denetimi azure'da](../../role-based-access-control/role-assignments-portal.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: ../../azure-monitor/app/app-insights-overview.md
