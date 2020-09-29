---
title: Azure Güvenlik Merkezi 'nde veri koleksiyonu | Microsoft Docs
description: Bu makalede, bir Log Analytics aracısının nasıl yükleneceği ve toplanan verilerin depolandığı bir Log Analytics çalışma alanının nasıl ayarlanacağı açıklanır.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 04/27/2020
ms.author: memildin
ms.openlocfilehash: 92c73fed84910e525378aa18e02456960acf9911
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447283"
---
# <a name="data-collection-in-azure-security-center"></a>Azure Güvenlik Merkezinde veri toplama
Güvenlik Merkezi, Azure sanal makinelerinizden (VM), sanal makine ölçek kümelerinden, IaaS kapsayıcılarından ve Azure olmayan (Şirket içi) bilgisayarların yanı sıra güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Log Analytics Aracı kullanılarak toplanır. Bu verilere örnek olarak şunlar verilebilir: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan süreçler, makine adı, IP adresleri ve oturum açmış kullanıcı.

Eksik güncelleştirmelere görünürlük sağlamak için veri toplama gerekir, yanlış yapılandırılmış işletim sistemi güvenlik ayarları, uç nokta koruma durumu ve sistem durumu ve tehdit koruması. 

Bu makalede, bir Log Analytics aracısının nasıl yükleneceği ve toplanan verilerin depolandığı bir Log Analytics çalışma alanının nasıl ayarlanacağı açıklanır. Veri toplamayı etkinleştirmek için her iki işlem de gereklidir. 

> [!NOTE]
> - Veri toplama yalnızca Işlem kaynakları (VM 'Ler, sanal makine ölçek kümeleri, IaaS kapsayıcıları ve Azure dışı bilgisayarlar) için gereklidir. Aracıları sağlamasanız bile Azure Güvenlik Merkezi 'nden faydalanabilirsiniz; Ancak, güvenlik sınırlı olur ve yukarıda listelenen yetenekler desteklenmez.  
> - Desteklenen platformların listesi için bkz. [Azure Güvenlik Merkezi 'Nde desteklenen platformlar](security-center-os-coverage.md).
> - Yeni veya mevcut bir çalışma alanı kullanmanıza bakılmaksızın verileri Log Analytics depolama, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="enable-automatic-provisioning-of-the-log-analytics-agent"></a>Log Analytics aracısının otomatik sağlamasını etkinleştir <a name="auto-provision-mma"></a>

Makinelerden verileri toplamak için Log Analytics aracısının yüklü olması gerekir. Aracının yüklenmesi otomatik olarak yapılabilir (önerilir) veya aracıyı el ile yükleyebilirsiniz. Varsayılan olarak, otomatik sağlama kapalıdır.

Otomatik sağlama açık olduğunda, Güvenlik Merkezi, desteklenen tüm Azure VM 'Lere ve oluşturulan tüm yeni sanal makinelere Log Analytics aracısını dağıtır. Otomatik sağlama önerilir, ancak gerekirse aracıyı el ile yükleyebilirsiniz (bkz. [Log Analytics aracısının el ile yüklenmesi](#manual-agent)).



Log Analytics aracısının otomatik sağlamasını etkinleştirmek için:

1. Güvenlik Merkezi 'nin menüsünde **fiyatlandırma & ayarları**' nı seçin.
1. Uygun aboneliği seçin.
1. **Veri toplama** sayfasında, **otomatik sağlamayı** **Açık**olarak ayarlayın.
1. **Kaydet**’i seçin.

    :::image type="content" source="./media/security-center-enable-data-collection/enable-automatic-provisioning.png" alt-text="Log Analytics aracısının otomatik olarak sağlanması etkinleştiriliyor":::

>[!TIP]
> Bir çalışma alanının sağlanması gerekiyorsa, aracı yüklemesi 25 dakikaya kadar sürebilir.

Makinelerinize dağıtılan Aracı, Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırması, uç nokta koruması ve ek güvenlik uyarıları oluşturma ile ilgili ek öneriler sağlayabilir.

>[!NOTE]
> Otomatik sağlamanın **kapalı** olarak ayarlanması, aracısının zaten sağlandığı Azure vm 'lerinden Log Analytics aracısını kaldırmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.

>[!NOTE]
> - Önceden var olan bir yüklemeyi sağlama hakkında yönergeler için, bkz. [Otomatik sağlama, önceden var olan aracı yüklemesi durumlarında](#preexisting).
> - El ile sağlama hakkında yönergeler için bkz. [Log Analytics Agent uzantısını El Ile yüklemeye](#manual-agent).
> - Otomatik sağlamayı kapatma hakkında yönergeler için bkz. [otomatik sağlamayı devre dışı](#offprovisioning)bırakma.
> - PowerShell kullanarak güvenlik merkezi 'ni ekleme hakkında yönergeler için bkz. [PowerShell kullanarak Azure Güvenlik Merkezi 'Ni otomatik hale](security-center-powershell-onboarding.md)getirme.
>

## <a name="workspace-configuration"></a>Çalışma alanı yapılandırması
Güvenlik Merkezi tarafından toplanan veriler, Log Analytics çalışma alanlarında depolanır. Verileriniz, Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında veya oluşturduğunuz var olan bir çalışma alanında depolanan Azure VM 'lerinden toplanabilir. 

Çalışma alanı yapılandırması abonelik başına ayarlanır ve birçok abonelik aynı çalışma alanını kullanabilir.

### <a name="using-a-workspace-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan çalışma alanını kullanma

Güvenlik Merkezi, verileri depolamak için otomatik olarak varsayılan bir çalışma alanı oluşturabilir. 

Güvenlik Merkezi tarafından oluşturulan bir çalışma alanı seçmek için:

1. **Varsayılan çalışma alanı yapılandırması**altında, Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını kullan ' ı seçin.
    :::image type="content" source="./media/security-center-enable-data-collection/workspace-selection.png" alt-text="Log Analytics aracısının otomatik olarak sağlanması etkinleştiriliyor"::: 

1. **Kaydet**’e tıklayın.<br>
    Güvenlik Merkezi, bu coğrafi konum içinde yeni bir kaynak grubu ve varsayılan çalışma alanı oluşturur ve aracıyı bu çalışma alanına bağlar. Çalışma alanı ve kaynak grubu için adlandırma kuralı:<br>
   **Çalışma alanı: DefaultWorkspace-[abonelik-KIMLIĞI]-[coğrafi] <br> kaynak grubu: DefaultResourceGroup-[coğrafi]**

   Abonelik birden çok geoloa 'dan VM 'Ler içeriyorsa, güvenlik merkezi birden çok çalışma alanı oluşturur. Veri gizliliği kurallarını sürdürmek için birden çok çalışma alanı oluşturulur.
1. Güvenlik Merkezi, abonelik için ayarlanan fiyatlandırma katmanı uyarınca çalışma alanında bir güvenlik merkezi çözümünü otomatik olarak etkinleştirir. 

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan çalışma alanlarının Log Analytics fiyatlandırma katmanı, güvenlik merkezi faturalandırmasını etkilemez. Güvenlik Merkezi faturaları her zaman bir çalışma alanına yüklenmiş olan Güvenlik Merkezi ilkelerine ve çözümlerine göre belirlenir. Azure Defender olmayan abonelikler için Güvenlik Merkezi, varsayılan çalışma alanında *Securitycenterfree* çözümüne izin vermez. Güvenlik Merkezi, Azure Defender ile abonelikler için varsayılan çalışma alanında *güvenlik* çözümüne izin vermez.
> Log Analytics veri depolama, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Mevcut Log Analytics Hesapları hakkında daha fazla bilgi için bkz. [var olan Log Analytics müşterileri](./faq-azure-monitor-logs.md).

### <a name="using-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanma

Zaten mevcut bir Log Analytics çalışma alanınız varsa aynı çalışma alanını kullanmak isteyebilirsiniz.

Mevcut Log Analytics çalışma alanınızı kullanmak için, çalışma alanında okuma ve yazma izinlerinizin olması gerekir.

> [!NOTE]
> Mevcut çalışma alanında etkinleştirilen çözümler, kendisine bağlı olan Azure VM 'lerine uygulanır. Ücretli çözümler için bu, ek ücretler oluşmasına neden olabilir. Veri gizliliği konuları için, seçtiğiniz çalışma alanınızın doğru coğrafi bölgede bulunduğundan emin olun.
> Verileri Log Analytics 'te depolamak, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Mevcut bir Log Analytics çalışma alanını seçmek için:

1. **Varsayılan çalışma alanı yapılandırması**altında **başka bir çalışma alanı kullan**' ı seçin.
    :::image type="content" source="./media/security-center-enable-data-collection/use-another-workspace.png" alt-text="Log Analytics aracısının otomatik olarak sağlanması etkinleştiriliyor"::: 

2. Açılan menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Çekme menüsünde Tüm aboneliklerinizde bulunan tüm çalışma alanları kullanılabilir. Daha fazla bilgi için bkz. [çapraz abonelik çalışma alanı seçimi](security-center-enable-data-collection.md#cross-subscription-workspace-selection) . Çalışma alanına erişmek için izninizin olması gerekir.
   >
   >

3. **Kaydet**’i seçin.
4. **Kaydet**' i seçtikten sonra, daha önce varsayılan bir çalışma alanına bağlı olan Izlenen VM 'leri yeniden yapılandırmak isteyip istemediğiniz sorulur.

   - Yeni çalışma alanı ayarlarının yalnızca yeni VM 'Lere uygulanmasını istiyorsanız **Hayır** ' ı seçin. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Log Analytics Aracısı yüklü olmayan yeni bulunan VM 'Ler.
   - Yeni çalışma alanı ayarlarının tüm VM 'Lere uygulanmasını istiyorsanız **Evet** ' i seçin. Ayrıca, bir güvenlik merkezi tarafından oluşturulan çalışma alanına bağlı her VM yeni hedef çalışma alanına yeniden bağlanır.

   > [!NOTE]
   > Evet ' i seçerseniz, tüm VM 'Ler yeni hedef çalışma alanına yeniden bağlanana kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmemelidir. Çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - İşlemi iptal etmek için **Iptal 'i**seçin.

     ![İzlenen VM 'Leri yeniden yapılandırma seçeneklerini gözden geçirin][3]

5. Çalışma alanında Azure Defender 'ın etkin olup olmayacağını seçin.

    Mevcut bir çalışma alanını kullanmak için, çalışma alanının fiyatlandırma katmanını ayarlayın. Bu, bir güvenlik merkezi çözümünü zaten mevcut değilse çalışma alanına yükler.

    1. Güvenlik Merkezi ana menüsünde, **fiyatlandırma & ayarları**' nı seçin.
     
    1. Aracıyı bağlanacağınız çalışma alanını seçin.

    1. **Azure Defender** veya **Azure Defender kapalı**' yı seçin.

   
   >[!NOTE]
   >Çalışma alanında zaten bir **güvenlik** veya **securitycenterücretsiz** çözümü etkinse, fiyatlandırma otomatik olarak ayarlanır. 


## <a name="cross-subscription-workspace-selection"></a>Çapraz abonelik çalışma alanı seçimi
Verilerinizin kaydedileceği bir çalışma alanı seçtiğinizde, tüm aboneliklerinizde tüm çalışma alanları kullanılabilir. Abonelikler arası çalışma alanı seçme özelliği sayesinde farklı aboneliklerde çalışan sanal makinelerden veri toplayabilir ve bunları istediğiniz çalışma alanında depolayabilirsiniz. Bu seçim, kuruluşunuzda merkezi bir çalışma alanı kullandığınızda ve bunu güvenlik veri koleksiyonu için de kullanmak istediğinizde kullanışlıdır. Çalışma alanlarını yönetme hakkında daha fazla bilgi için bkz. [çalışma alanı erişimini yönetme](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access).



## <a name="data-collection-tier"></a>Veri toplama katmanı
Azure Güvenlik Merkezi'nde seçtiğiniz veri koleksiyonu katmanı yalnızca güvenlik olaylarının Log Analytics çalışma alanınızda depolanma durumunu etkiler. Log Analytics Aracısı yine de Azure Güvenlik Merkezi 'nin tehdit koruması için gereken güvenlik olaylarını toplayıp analiz ederek, Log Analytics çalışma alanınızda (varsa) hangi güvenlik olaylarının depolanmasını istediğinizi tercih eder. Güvenlik olaylarını çalışma alanınızda depolamayı seçerek çalışma alanınızda bu olaylarla ilgili araştırma, arama ve denetim işlemi gerçekleştirebilirsiniz. 
> [!NOTE]
> Verileri Log Analytics 'te depolamak, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).
> 
> Çalışma alanınızda depolanacak dört olay kümesinden, abonelikleriniz ve çalışma alanlarınız için doğru filtreleme ilkesini seçebilirsiniz: 

- **Hiçbiri** – güvenlik olay depolamayı devre dışı bırakın. Bu varsayılan ayardır.
- **Minimum** : olay birimini en aza indirmek isteyen müşteriler için daha küçük bir olay kümesi.
- **Ortak** – bu, çoğu müşteriyi karşılayan ve bunlara tam denetim izi veren bir olay kümesidir.
- **Tüm olaylar – tüm** olayların depolandığından emin olmak isteyen müşteriler için.


> [!NOTE]
> Bu güvenlik olayları kümeleri yalnızca Azure Defender ile kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
Bu kümeler tipik senaryolara yönelik olarak tasarlanmıştır. Uygulamadan önce ihtiyaçlarınıza uygun olanı değerlendirdiğinizden emin olun.
>
>

**Ortak** ve **En düşük** olay kümelerine ait olan olayları öğrenmek için, her bir olayın filtrelenmemiş sıklığı ve kullanımları hakkında bilgi edinmek üzere müşteriler ve sektör standartları ile çalıştık. Bu işlemde aşağıdaki yönergeleri kullandık:

- **En az** -bu küme, yalnızca başarılı bir ihlal ve çok düşük bir birimi olan önemli olayları gösterebilen olayları kapsadığından emin olun. Örneğin, bu küme kullanıcı başarılı ve başarısız oturum açma bilgilerini (olay kimlikleri 4624, 4625) içerir, ancak bu, denetim için önemli olan oturum açma bilgilerini içermez ancak algılama için anlamlı ve görece yüksek hacimdir. Bu küme veri hacminin çoğu, oturum açma olayları ve işlem oluşturma olayıdır (olay KIMLIĞI 4688).
- **Ortak** -bu küme içinde tam bir kullanıcı denetim izi sağlayın. Örneğin, bu küme hem Kullanıcı oturum açmaları hem de Kullanıcı imzası içerir (olay KIMLIĞI 4634). Güvenlik grubu değişiklikleri, anahtar etki alanı denetleyicisi Kerberos işlemleri ve sektör kurumları tarafından önerilen diğer olaylar gibi denetim eylemlerini de ekledik.

Çok düşük hacimmiş olan olaylar, tüm olaylar üzerinde seçim yapmak için kullanılan ana işlem, birimi azaltmaktır ve belirli olayları filtreleyememektedir.

Her bir küme için güvenlik ve uygulama dolabı olay kimliklerinin tamamen bir dökümü aşağıda verilmiştir:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En az | 1102, 4624, 4625, 4657, 4663, 4688, 4700, 4702, 4719, 4720, 4722, 4723, 4724, 4727, 4728, 4732, 4735, 4737, 4739, 4740, 4754, 4755, |
| | 4756, 4767, 4799, 4825, 4946, 4948, 4956, 5024, 5033, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222 |
| Common | 1, 299, 300, 324, 340, 403, 404, 410, 411, 412, 413, 431, 500, 501, 1100, 1102, 1107, 1108, 4608, 4610, 4611, 4614, 4622, |
| |  4624, 4625, 4634, 4647, 4648, 4649, 4657, 4661, 4662, 4663, 4665, 4666, 4667, 4688, 4670, 4672, 4673, 4674, 4675, 4689, 4697, |
| | 4700, 4702, 4704, 4705, 4716, 4717, 4718, 4719, 4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4733, 4732, 4735, 4737, |
| | 4738, 4739, 4740, 4742, 4744, 4745, 4746, 4750, 4751, 4752, 4754, 4755, 4756, 4757, 4760, 4761, 4762, 4764, 4767, 4768, 4771, |
| | 4774, 4778, 4779, 4781, 4793, 4797, 4798, 4799, 4800, 4801, 4802, 4803, 4825, 4826, 4870, 4886, 4887, 4888, 4893, 4898, 4902, |
| | 4904, 4905, 4907, 4931, 4932, 4933, 4946, 4948, 4956, 4985, 5024, 5033, 5059, 5136, 5137, 5140, 5145, 5632, 6144, 6145, 6272, |
| | 6273, 6278, 6416, 6423, 6424, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222, 26401, 30004 |

> [!NOTE]
> - Grup ilkesi nesnesi (GPO) kullanıyorsanız, denetim ilkeleri Işlem oluşturma olayı 4688 ' nı ve olay 4688 ' nin içindeki *commandLine* alanını etkinleştirmeniz önerilir. Işlem oluşturma olayı 4688 hakkında daha fazla bilgi için Güvenlik Merkezi 'nin [SSS](faq-data-collection-agents.md#what-happens-when-data-collection-is-enabled)bölümüne bakın. Bu denetim ilkeleri hakkında daha fazla bilgi için bkz. [Denetim Ilkesi önerileri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)için veri toplamayı etkinleştirmek üzere, Güvenlik Merkezi, tüm uygulamalara izin vermek için denetim modunda yerel bir AppLocker ilkesi yapılandırır. Bu, AppLocker 'ın Güvenlik Merkezi tarafından toplanan ve yararlanılabilir olayları oluşturmasına neden olur. Bu ilkenin zaten yapılandırılmış bir AppLocker ilkesinin bulunduğu makinelerde yapılandırılmaması gerektiğini unutmayın. 
> - Windows Filtre Platformu [olay kimliği 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156)' i toplamak Için, [Denetim filtreleme platformu bağlantısını](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) etkinleştirmeniz gerekir (auditpol/set/subcategory: "Platform bağlantısı filtreleniyor"/Success: etkinleştir)
>

Filtreleme ilkenizi seçmek için:
1. **Veri toplama** sayfasında, **ek ham veri-Windows Güvenlik olaylarını depola**altında filtreleme ilkenizi seçin.
 
1. **Kaydet**’i seçin.
    :::image type="content" source="./media/security-center-enable-data-collection/data-collection-tiers.png" alt-text="Log Analytics aracısının otomatik olarak sağlanması etkinleştiriliyor":::

### <a name="automatic-provisioning-in-cases-of-a-pre-existing-agent-installation"></a>Önceden var olan bir aracı yüklemesi durumlarında otomatik sağlama <a name="preexisting"></a> 

Aşağıdaki kullanım örnekleri, zaten bir aracı veya uzantının yüklü olduğu durumlarda otomatik sağlama 'nın nasıl çalıştığını belirtir. 

- Log Analytics Aracısı makineye yüklendi, ancak uzantı olarak değil (doğrudan aracı)<br>
Log Analytics Aracısı doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, güvenlik merkezi Log Analytics aracı uzantısını yükler ve Log Analytics aracısını en son sürüme yükseltecektir.
Yüklü aracı zaten yapılandırılmış çalışma alanına (ler) rapor etmeye devam eder ve ek olarak güvenlik merkezi 'nde yapılandırılan çalışma alanına rapor eder (Windows makinelerde çoklu barındırma desteklenir).
Yapılandırılmış çalışma alanı bir kullanıcı çalışma alanı ise (Güvenlik Merkezi 'nin varsayılan çalışma alanı değil), bu çalışma alanına raporlama yapan VM 'Ler ve bilgisayarlardan gelen olayları işlemeye başlamak için Güvenlik Merkezi 'nin "güvenlik/" securityFree "çözümünü yüklemeniz gerekir.<br>
<br>
Linux makineler için, aracı çoklu barındırma henüz desteklenmiyor. bu nedenle, mevcut bir aracı yüklemesi algılanırsa, otomatik sağlama gerçekleşmez ve makinenin yapılandırması değiştirilmez.
<br>
Aboneliklerdeki mevcut makineler için, eklendi Mart 2019 ' den önce, mevcut bir aracı algılandığında, Log Analytics Aracısı uzantısı yüklenmez ve makine etkilenmeyecektir. Bu makineler için, bu makinelerdeki aracı yükleme sorunlarını gidermek için "makinelerinizdeki izleme Aracısı sistem durumu sorunlarını çözün" önerisine bakın.

  
- System Center Operations Manager Aracısı makineye yüklendi<br>
Güvenlik Merkezi, Log Analytics aracı uzantısını mevcut Operations Manager yan yana yükleyecek. Mevcut Operations Manager Aracısı normal olarak Operations Manager sunucusuna rapor etmeye devam edecektir. Operations Manager Aracısı ve Log Analytics Aracısı, bu işlem sırasında en son sürüme güncellenecek ortak çalışma zamanı kitaplıklarını paylaşır. Operations Manager Agent 2012 sürümü **yüklüyse, otomatik sağlamayı etkinleştirmeyin.**<br>

- Önceden var olan bir VM uzantısı var<br>
    - Izleme Aracısı bir uzantı olarak yüklendiğinde, uzantı yapılandırması raporlamaya yalnızca tek bir çalışma alanına izin verir. Güvenlik Merkezi, mevcut kullanıcı çalışma alanları bağlantılarını geçersiz kılmaz. Güvenlik Merkezi, "güvenlik" veya "securityFree" çözümünün yüklenmiş olması şartıyla, zaten bağlı olan çalışma alanındaki VM 'den güvenlik verilerini depolar. Güvenlik Merkezi bu işlemdeki en son sürüme uzantı sürümünü yükseltebilir.  
    - Var olan uzantının hangi çalışma alanına veri gönderdiğini görmek için, [Azure Güvenlik Merkezi ile bağlantıyı doğrulamak](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/)üzere testi çalıştırın. Alternatif olarak, Log Analytics çalışma alanlarını açabilir, bir çalışma alanı seçebilir, sanal makineyi seçebilir ve Log Analytics Aracı bağlantısına bakabilirsiniz. 
    - Log Analytics aracısının istemci iş istasyonlarında yüklü olduğu bir ortamınız varsa ve var olan bir Log Analytics çalışma alanına raporlama yaptıysanız, işletim sisteminizin desteklendiğinden emin olmak için [Azure Güvenlik Merkezi tarafından desteklenen işletim sistemlerinin](security-center-os-coverage.md) listesini gözden geçirin. Daha fazla bilgi için bkz. [var olan Log Analytics müşterileri](./faq-azure-monitor-logs.md).
 
### <a name="turn-off-automatic-provisioning"></a>Otomatik sağlamayı devre dışı bırakma <a name="offprovisioning"></a>
Log Analytics aracısının otomatik hazırlanmasını devre dışı bırakmak için:

1. Portalda Güvenlik Merkezi 'nin menüsünde, **fiyatlandırma & ayarları**' nı seçin.
2. Uygun aboneliği seçin.

    :::image type="content" source="./media/security-center-enable-data-collection/select-subscription.png" alt-text="Log Analytics aracısının otomatik olarak sağlanması etkinleştiriliyor":::

3. **Veri toplamayı**seçin.
4. Otomatik sağlamayı devre dışı bırakmak için **Otomatik sağlama**altında **kapalı** ' yı seçin.
5. **Kaydet**’i seçin. 


Otomatik sağlama devre dışı bırakıldığında (kapalı), varsayılan çalışma alanı yapılandırma bölümü görüntülenmez.

Daha önce aracılardan sonra otomatik sağlamayı devre dışı bırakırsanız yeni VM 'lerde sağlanmamıştır.

 
> [!NOTE]
>  Otomatik sağlamayı devre dışı bırakmak, aracının sağlandığı Azure VM 'lerinden Log Analytics aracısını kaldırmaz. OMS uzantısını kaldırma hakkında bilgi için, bkz. [Güvenlik Merkezi tarafından yüklenen OMS uzantılarını kaldırma nasıl yaparım?](faq-data-collection-agents.md#remove-oms).
>
    
## <a name="manual-agent-provisioning"></a>El ile aracı sağlama <a name="manual-agent"></a>
 
Log Analytics aracısını el ile yüklemek için birkaç yol vardır. El ile yüklerken, otomatik sağlamayı devre dışı bıraktığınızdan emin olun.

### <a name="operations-management-suite-vm-extension-deployment"></a>Operations Management Suite VM Uzantısı dağıtımı 

Güvenlik Merkezi 'nin sanal makinelerinizden güvenlik verilerini toplayabilmesi ve öneriler ve uyarılar sağlaması için Log Analytics aracısını el ile yükleyebilirsiniz.

1. Otomatik sağlamayı devre dışı bırakın.

1. İsteğe bağlı olarak, bir çalışma alanı oluşturun.

1. Log Analytics aracısını yüklemekte olduğunuz çalışma alanında Azure Defender 'ı etkinleştirin:

    1. Güvenlik Merkezi 'nin menüsünde **fiyatlandırma & ayarları**' nı seçin.

    1. Aracıyı yüklemekte olduğunuz çalışma alanını ayarlayın. Çalışma alanının, güvenlik merkezi 'nde kullandığınız abonelikte olduğundan ve çalışma alanında okuma/yazma izinlerine sahip olduğunuzdan emin olun.

    1. Azure Defender 'ı açık olarak ayarlayın ve **Kaydet**' i seçin.

       >[!NOTE]
       >Çalışma alanında zaten bir **güvenlik** veya **securitycenterücretsiz** çözümü etkinse, fiyatlandırma otomatik olarak ayarlanır. 

1. Aracıları Kaynak Yöneticisi şablonu kullanarak yeni VM 'lerde dağıtmak istiyorsanız, Log Analytics Aracısı 'nı yükleyebilirsiniz:

   a.  [Windows için Log Analytics aracısını yükler](../virtual-machines/extensions/oms-windows.md)
    
   b.  [Linux için Log Analytics aracısını yükler](../virtual-machines/extensions/oms-linux.md)

1. Mevcut VM 'lerde uzantıları dağıtmak için [Azure sanal makineleri hakkında veri toplama](../azure-monitor/learn/quick-collect-azurevm.md)bölümündeki yönergeleri izleyin.

   > [!NOTE]
   > **Olay ve performans verilerini topla** bölümü isteğe bağlıdır.
   >

1. Uzantıyı dağıtmak üzere PowerShell 'i kullanmak için, sanal makineler belgelerindeki yönergeleri kullanın:

    - [Windows makineleri için](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

    - [Linux makineleri için](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#azure-cli-deployment)



> [!NOTE]
> PowerShell kullanarak güvenlik merkezi 'ni ekleme hakkında yönergeler için bkz. [PowerShell kullanarak Azure Güvenlik Merkezi 'Ni otomatik hale](security-center-powershell-onboarding.md)getirme.

## <a name="troubleshooting"></a>Sorun giderme

-   Otomatik sağlama yükleme sorunlarını belirlemek için bkz. [Aracı sistem durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).

-  İzleme Aracısı ağ gereksinimlerini belirlemek için bkz. [İzleme Aracısı ağ gereksinimlerini giderme](security-center-troubleshooting-guide.md#mon-network-req).
-   El ile ekleme sorunlarını belirlemek için bkz. [Operations Management Suite ekleme sorunlarını giderme](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).

- Izlenmeyen VM 'Leri ve bilgisayar sorunlarını belirlemek için:

    Makine Log Analytics Aracısı uzantısını çalıştırmadığından, bir VM veya bilgisayar Güvenlik Merkezi tarafından izlenmeyen. Bir makinede yerel bir aracı zaten yüklü olabilir, örneğin OMS doğrudan Aracısı veya System Center Operations Manager Aracısı. Bu aracılar Güvenlik Merkezi 'nde tam olarak desteklenmediğinden, bu aracıları içeren makineler izlenmeyen olarak tanımlanmıştır. Tüm güvenlik merkezi 'nin yetilerinden tam olarak yararlanabilmek için Log Analytics Aracısı uzantısı gereklidir.

    Güvenlik Merkezi 'nin otomatik sağlama için başlatılan VM 'Leri ve bilgisayarları başarılı bir şekilde izleyememesinin nedenleri hakkında daha fazla bilgi için bkz. [Aracı sistem durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenlik merkezi 'nde veri toplama ve otomatik sağlama işlemlerinin nasıl çalıştığı gösteriliyordu. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

* [Azure Güvenlik Merkezi ile ilgili SSS](faq-general.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.



<!--Image references-->
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
