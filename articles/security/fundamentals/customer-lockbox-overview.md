---
title: Microsoft Azure için Müşteri Kasası
description: Microsoft 'un müşteri verilerine erişmesi gerektiğinde bulut sağlayıcısı erişimi üzerinde denetim sağlayan Microsoft Azure için Müşteri Kasası Teknik Genel Bakış.
author: TerryLanfear
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 09/15/2020
ms.openlocfilehash: 01232aa101e2964354acfbeb6cea341a0da33ca6
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96489910"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Microsoft Azure için Müşteri Kasası

> [!NOTE]
> Bu özelliği kullanmak için, kuruluşunuzda en az **Geliştirici** düzeyi olan bir [Azure Destek planına](https://azure.microsoft.com/support/plans/) sahip olmanız gerekir.

Microsoft Azure için Müşteri Kasası müşterilerin, müşteri verilerine yönelik erişim isteklerini gözden geçirmesi ve onaylaması için arabirim sağlar. Bu arabirim Microsoft mühendisinin destek isteği sırasında müşterinin verilerine erişmesi gerektiğinde kullanılır.

Bu makalede, Müşteri Kasası isteklerinin nasıl başlatıldığı, izlendiği ve daha sonraki gözden geçirmeleri ve denetimleri için depolandığı ele alınmaktadır.

Müşteri Kasası artık genel kullanıma sunulmuştur ve şu anda sanal makinelere uzaktan masaüstü erişimi için etkinleştirilmiştir.

## <a name="supported-services-and-scenarios-in-preview"></a>Önizlemede desteklenen hizmetler ve senaryolar

Aşağıdaki hizmetler şu anda Müşteri Kasası için önizleme aşamasındadır:

- API Management
- Azure App Service
- Bilişsel Hizmetler
- Container Kayıt Defteri
- MySQL için Azure Veritabanı
- Azure Databricks
- Azure Data Box
- Azure Veri Gezgini
- Azure Data Factory
- PostgreSQL için Azure Veritabanı
- Azure İşlevleri
- HDInsight
- Azure Kubernetes Service
- Azure İzleyici
- Azure Storage
- Azure SQL DB
- Azure abonelik aktarımları
- Azure Synapse Analytics
- Sanal makineler (şimdi bellek dökümlerine ve yönetilen disklere erişimi de kapsar)

Kuruluşunuzun bu önizleme teklifleri için Müşteri Kasası etkinleştirmek üzere [Azure genel önizlemesi için müşteri kasası](https://aka.ms/customerlockbox/insiderprogram)kaydolun.

## <a name="supported-services-and-scenarios-in-general-availability"></a>Genel kullanıma yönelik desteklenen hizmetler ve senaryolar

Aşağıdaki hizmetler ve senaryolar Şu anda Müşteri Kasası için genel kullanıma yöneliktir.

### <a name="remote-desktop-access-to-virtual-machines"></a>Sanal makinelere uzak masaüstü erişimi

Müşteri Kasası şu anda sanal makinelere uzaktan masaüstü erişimi istekleri için etkinleştirilmiştir. Aşağıdaki iş yükleri desteklenir:
- Hizmet olarak platform (PaaS) - Azure Cloud Services (web rolü ve çalışan rolü)
- Hizmet olarak altyapı (IaaS) - Windows ve Linux (yalnızca Azure Resource Manager)
- Sanal makine ölçek kümesi - Windows ve Linux

> [!NOTE]
> IaaS klasik örnekleri Müşteri Kasası tarafından desteklenmez. IaaS klasik örneklerinde çalışan iş yükleriniz varsa, bunları klasik 'dan Kaynak Yöneticisi dağıtım modellerine geçirmeniz önerilir. Yönergeler için bkz. [IaaS kaynaklarının klasik modelden Azure Resource Manager’a platform destekli geçişi](../../virtual-machines/migration-classic-resource-manager-overview.md).

#### <a name="detailed-audit-logs"></a>Ayrıntılı denetim günlükleri

Uzaktan masaüstü erişimi içeren senaryolarda Microsoft mühendisinin gerçekleştirdiği eylemleri gözden geçirmek için Windows olay günlüklerini kullanabilirsiniz. Olay günlüklerinizi toplamak ve verileri analiz etmek üzere çalışma alanınıza kopyalamak için Azure Güvenlik Merkezi'ni kullanmayı düşünebilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi’nde veri toplama](../../security-center/security-center-enable-data-collection.md).

## <a name="workflow"></a>İş akışı

Aşağıdaki adımlarda Müşteri Kasası isteği için tipik bir iş akışı ana hatlarıyla verilmiştir.

1. Kuruluştaki birisinin Azure iş yüküyle ilgili bir sorun vardır.

2. Bu kişi sorunu giderdikten, ancak bunu düzeltemedi [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac)bir destek bileti açar. Bilet bir Azure müşteri destek mühendisine atanır.

3. Azure Destek Mühendisi, hizmet isteğini inceler ve sorunu çözmeye yönelik sonraki adımları belirler.

4. Destek Mühendisi standart araçları ve telemetrisi kullanarak sorunu gideremez bir sonraki adım, tam zamanında (JıT) erişim hizmeti kullanarak yükseltilmiş izinler isteğidir. Bu istek, özgün Destek Mühendisinden olabilir. Veya, sorun Azure DevOps ekibine ilerletiğinden, bu, farklı bir mühendisden olabilir.

5. Azure mühendisi tarafından erişim isteği gönderildikten sonra, tam zamanında hizmeti, isteği şu şekilde hesaba katmasının önüne değerlendirir:
    - Kaynağın kapsamı
    - İstek sahibinin yalıtılmış bir kimlik mi yoksa Multi-Factor Authentication kullanılarak mı kullanıldığı
    - İzin düzeyleri

    JıT kuralına bağlı olarak, bu istek Iç Microsoft onaylayanlardan onay de içerebilir. Örneğin, onaylayan müşteri destek lideri veya DevOps yöneticisi olabilir.

6. İstek müşteri verilerine doğrudan erişim gerektirdiğinde, bir Müşteri Kasası isteği başlatılır. Örneğin, bir müşterinin sanal makinesine uzak masaüstü erişimi.

    İstek artık **müşteri tarafından bildirildi** durumunda olduğundan, erişim izni vermeden önce müşterinin onayını bekliyor.

7. Müşteri kuruluşunda, Azure aboneliği için [sahip rolüne](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) sahip olan Kullanıcı Microsoft 'tan bir e-posta alır ve bu, bekleyen erişim isteği hakkında bilgilendirmeye yöneliktir. Müşteri Kasası istekler için, bu kişi belirlenen onaylayıcı.

    Örnek e-posta:

    ![Azure Müşteri Kasası-e-posta bildirimi](./media/customer-lockbox-overview/customer-lockbox-email-notification.png)

8. E-posta bildirimi, Azure portal **müşteri kasası** dikey penceresine bir bağlantı sağlar. Bu bağlantıyı kullanarak, belirlenen onaylayan, kuruluşlarının Müşteri Kasası sahip olduğu bekleyen istekleri görüntülemek için Azure portal oturum açar:

    ![Azure Müşteri Kasası-giriş sayfası](./media/customer-lockbox-overview/customer-lockbox-landing-page.png)

   İstek, dört gün boyunca müşteri kuyruğunda kalır. Bu süreden sonra, erişim isteği otomatik olarak sona erer ve Microsoft mühendislerine erişim sağlanmaz.

9. Bekleyen isteğin ayrıntılarını almak için, belirlenen onaylayan, **bekleyen isteklerden** gelen kasa isteğini seçebilir:

    ![Azure Müşteri Kasası-bekleyen isteği görüntüleme](./media/customer-lockbox-overview/customer-lockbox-pending-requests.png)

10. Belirlenen onaylayan, özgün kullanıcı tarafından oluşturulan destek bileti isteğini görüntülemek için **HIZMET ISTEğI kimliğini** de seçebilir. Bu bilgiler, Microsoft Desteği neden bağlı olduğuna ve bildirilen sorunun geçmişine yönelik bağlam sağlar. Örnek:

    ![Azure Müşteri Kasası-destek bileti isteğini görüntüleme](./media/customer-lockbox-overview/customer-lockbox-support-ticket.png)

11. İstek gözden geçirildikten sonra, belirlenen onaylayan **Onayla** veya **Reddet**' i seçer:

    ![Azure Müşteri Kasası-onayla veya Reddet ' i seçin](./media/customer-lockbox-overview/customer-lockbox-approval.png)

    Seçimin bir sonucu olarak:
    - **Onayla**: Microsoft mühendisine erişim verilir. Erişim varsayılan sekiz saatlik bir süre için verilir.
    - **Reddet**: Microsoft mühendisine göre yükseltilmiş erişim isteği reddedilir ve başka bir eylem yapılmaz.

Denetim amaçlarıyla, bu iş akışında gerçekleştirilen eylemler [müşteri kasası istek günlüklerine](#auditing-logs)kaydedilir.

## <a name="auditing-logs"></a>Denetim günlükleri

Müşteri Kasası günlükleri etkinlik günlüklerine depolanır. Azure portal, Müşteri Kasası isteklerle ilgili denetim bilgilerini görüntülemek için **etkinlik günlükleri** ' ni seçin. Belirli eylemler için filtreleyebilirsiniz, örneğin:
- **Kasa İsteğini Reddetme**
- **Kasa İsteği Oluşturma**
- **Kasa İsteğini Onaylama**
- **Kasa Isteği süre sonu**

Örneğin:

![Azure Müşteri Kasası-etkinlik günlükleri](./media/customer-lockbox-overview/customer-lockbox-activitylogs.png)

## <a name="customer-lockbox-integration-with-azure-security-benchmark"></a>Azure Güvenlik Karşılaştırması ile Müşteri Kasası tümleştirmesi

Azure Güvenlik kıyaslaması 'nda Müşteri Kasası uygulanabilirliğini içeren yeni bir temel denetim ([3,13](../benchmarks/security-control-identity-access-control.md#313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios)) sunuyoruz. Müşteriler artık bir hizmetin Müşteri Kasası uygulanabilirliğini gözden geçirmek için kıyaslamaya yararlanabilir.

## <a name="exclusions"></a>Dışlamalar

Aşağıdaki mühendislik desteği senaryolarında Müşteri Kasası istekleri tetiklenmez:

- Microsoft mühendisinin standart çalışma yordamlarının dışında kalan bir eylem gerçekleştirmesi gerekiyor. Örneğin beklenmeyen veya öngörülemeyen senaryolarda hizmetleri kurtarmak veya geri yüklemek için olabilir.

- Microsoft mühendisi sorun giderme işlemi kapsamında Azure platformuna erişiyor ve yanlışlıkla müşteri verilere erişiyor. Örneğin Azure Ağ Ekibi sorun giderme işlemi yapıyor ve bu işlem sonucunda ağ cihazında bir paket yakalanıyor olabilir. Öte yandan müşteri aktarımdaki verileri şifrelediyse, mühendis verileri okuyamaz.

## <a name="next-steps"></a>Sonraki adımlar

Müşteri Kasası, en az **Geliştirici** düzeyi olan bir [Azure Destek planına](https://azure.microsoft.com/support/plans/) sahip tüm müşteriler için otomatik olarak kullanılabilir.

Uygun bir destek planınız varsa, Müşteri Kasası etkinleştirmeniz için herhangi bir işlem yapmanız gerekmez. Müşteri Kasası istekleri, kuruluşunuzdaki bir bilgisayardan dosyalanan bir destek biletini devam etmek için gerekliyse, bu işlem bir Microsoft mühendisi tarafından başlatılır.