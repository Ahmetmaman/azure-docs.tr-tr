---
title: Liste türü tarafından gereksinimleri | Azure
description: Bu makalede, uygulamalarını Azure Market'te yayımlama işlemini anlama çalışılırken yayımlama gereksinimleri iş ortakları ve uygunluk ölçütlerini açıklanır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: 1c850b16492f725114fb23a8503f6345e1509212
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52868676"
---
# <a name="requirements-by-listing-type"></a>Liste türü tarafından gereksinimleri  
Teknik ve pazarlama içeriği gereksinimleri vitrini, Teklif türü ve liste türüne göre değişir. Uyumluluğunuzu doğrulamak için aşağıdaki özellikleri gözden geçirin.  
1. StoreFront gereksinimleri:  
    *   [AppSource](#storefront-requirements-appSource)  
    *   [Azure Market](#storefront-requirements-azure-marketplace)  
2. Liste türü ve Teklif türü gereksinimleri:  
    *   Çözümünüz Sayfası raporu için listeleme türünü belirleme liste türleri ve teklif türleri hakkında daha fazla bilgi için ziyaret edin [docs.microsoft.com/azure/marketplace/determine-your-listing-type](./determine-your-listing-type.md).  

## <a name="storefront-requirements-appsource"></a>StoreFront gereksinimleri: AppSource  
Aşağıdaki tabloda, Appsource'ta yayımlama için önkoşul gereksinimleri açıklanmaktadır.  

| Gereksinim | Ayrıntılar | Gerekli ya da Önerilen |  
|:--- |:--- |:--- |  
| ***Azure Active Directory (Azure AD)*** | Uygulamanızı Azure Active Directory Federasyon çoklu oturum (Azure AD Federasyon SSO) açma etkin onay ile izin vermeniz gerekir.<ul> <li>Federasyon SSO Azure AD'ye etkinleştirme hakkında daha fazla bilgi için yapılandırma çoklu oturum açma, Azure Active Directory'de uygulama Galerisi'nde sayfanın bulunan olmayan uygulamalar için ziyaret edin [docs.microsoft.com/azure/active-directory/ Active-directory-saas-özel-apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).</li> </ul> | Gerekli |   
| ***Microsoft bulut Hizmetleri ile tümleştirme*** | Uygulamanızı Microsoft Power BI, Cortana Intelligence ve Microsoft Azure Hizmetleri gibi diğer Microsoft Cloud services ile tümleşmelidir.<ul> <li>Nesnelerin Internet'i Microsoft Cloud hizmet örneğidir.</li> </ul> | Önerilen |  
| ***Hedef kitle*** | Satır iş kolu kullanıcıları ve işletme sahipleri için uygulamanızın olması gerekir. | Gerekli | 
| ***İş için bir hizmet (SaaS) uygulaması olarak yazılım*** | Uygulamanız aşağıdaki gereksinimleri karşılaması gerekir.<ul> <li>Bir satır iş kolu SaaS uygulaması</li> <li>Odaklanmış iş süreci</li> <li>Hedeflenen iş müşterilerine</li> <li>Kullanıcıların, kullanıcı adı ve parola gibi oturum açmak için iş kimlik bilgilerini etkinleştir</li> </ul> | Gerekli |  
| ***Ücretsiz deneme süresi ve deneme sürümü deneyimi*** | Uygulamanızı bir sırada bir müşteri uygulamanızı sınırlı bir süre için ücretsiz kullanmak için aşağıdaki seçeneklerden içermelidir.<ul> <li>Sağlayan bir `try` müşteriler uygulamanızın AppSource içinden bir deneme başlayabilir şekilde yöntemi</li> <li>Sağlayan bir `request trial` appsource'ta müşterilerin uygulamanızı'nin deneme sürümü isteyebilir şekilde seçeneği</li> </ul>Sağladığınız ücretsiz deneme müşteri uygulamanıza ek ücret ödemeden denemek için önceden ayarlanmış bir süre sağlaması gerekir. | Gerekli |  
| ***Kolayca yapılandırılabilir, kullanıma hazır bir çözüm*** | Uygulamanızı, kolay ve hızlı yapılandırma ve özelleştirme gerekli ayarlanmış olması gerekir. | Gerekli |  
| ***Tedarik Yönetimi*** | CRM'İNİZE aldığınız önce müşteri adayı verilerini mağazada müşteri adayları kabul etmek etkinleştirmeniz gerekir.<ul> <li>Katılımlarını Marketo, Microsoft Dynamics veya Salesforce örnekler</li> </ul> | Gerekli |  
| ***Gizlilik İlkesi ve kullanım koşulları*** | Uygulamanızı genel bir URL kullanarak gizlilik ilkesi sayfanıza bir bağlantı sağlamanız gerekir. Metin olarak yayımlama sırasında kullanım koşullarınızı sağlanması gerekir. | Gerekli |  
| ***Destek*** | Uygulamanızı genel bir URL kullanarak müşteri destek sayfanız için bir bağlantı sağlamanız gerekir. Uygulamanız bir deneme ise, deneme süresi boyunca ek ücret ödemeden desteklemesi gerekir. | Gerekli |  

## <a name="storefront-requirements-azure-marketplace"></a>StoreFront gereksinimleri: Azure Market  
Azure Marketi'nde türlerini listeleme önkoşul gereksinimleri aşağıda verilmiştir.  

| Gereksinim | Ayrıntılar | Liste türü |  
|:--- |:--- |:--- |  
| ***Katılım ilkeleri*** | Azure Marketi katılım ilkeleri, uygulama izlemeniz gerekir.<ul> <li>Katılım ilkeleri hakkında daha fazla bilgi için ziyaret edin sayfasında bulunan Azure Marketi katılım ilkeleri [azure.microsoft.com/support/legal/marketplace/participation-policies](https://azure.microsoft.com/support/legal/marketplace/participation-policies).</li></ul> | list<br />Transact<br />trial |  
| ***Microsoft ile tümleştirme*** | Teklifinizi kullanmak veya Microsoft Azure işlem, ağ veya depolama gibi hizmet türleri genişleten gerekir. Teklifiniz, veritabanları, güvenlik veya ağ gibi var olan bir Azure Marketi kategori ile hizalamanız gerekir.<ul> <li>Market teklifleri hakkında daha fazla bilgi için ziyaret edin sayfasında bulunan Market uygulamaları [azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps).</li> </ul> | list<br />Transact<br />trial |  
| ***Hedef kitle*** | Teklifiniz, BT uzmanları, bulut geliştiricilerine veya diğer teknik müşteri rolleri olması gerekir. | list<br />Transact<br />trial |  
| ***Tedarik Yönetimi*** | Mağazada aldığınız önce müşteri adayı verilerini müşteri adayları kabul etmek CRM (Marketo, Microsoft Dynamics veya Salesforce) etkinleştirmeniz gerekir. | list<br />Transact<br />trial |  
| ***Gizlilik İlkesi ve kullanım koşulları*** | Uygulamanızı genel bir URL kullanarak gizlilik ilkesi sayfanıza bir bağlantı sağlamanız gerekir. Metin olarak yayımlama sırasında kullanım koşullarınızı sağlanması gerekir. | list<br />Transact<br />trial |  
| ***Destek*** | Teklifiniz, genel bir URL kullanarak müşteri destek sayfanız bir bağlantı sağlamanız gerekir. Teklifinizi deneme varsa deneme süresi boyunca ek ücret ödemeden desteklemesi gerekir. | Transact<br />trial |    

## <a name="non-transact-listings"></a>Transact listeleri  
Bu bölümde, liste türünü Transact kullanmayan tüm teklif türleri açıklanmaktadır. 

### <a name="list"></a>Liste  
Liste türü listeleme vitrinler üzerinde aşağıdaki teklif türlerini Market'te içerir.  

| Teklif türü | Vitrin | Ayrıntılar |  
|:---        |:---        |:---     |  
| Danışmanlık Hizmetleri | AppSource | [Gereksinimleri: AppSource: liste: Danışmanlık Hizmetleri](#requirements-appsource-list-consulting-services) |  
| Danışmanlık Hizmetleri | Azure Market | [Gereksinimleri: Azure Market: liste: Danışmanlık Hizmetleri](#requirements-azure-marketplace-list-consulting-services) |  
| Benimle iletişime geçin | AppSource | [](#) |  
| Benimle iletişime geçin | Azure Market | [Gereksinimleri: AppSource: liste: benimle iletişim kurun](#requirements-azure-marketplace-list-contact-me) |  

#### <a name="requirements-appsource-list-consulting-service"></a>Gereksinimleri: AppSource: liste: danışmanlık hizmeti  

| Gereksinimler | Ayrıntılar |  
|:--- |:--- |  
| Hizmet teklifi özellikleri | Danışmanlık hizmetinize aşağıdaki ölçütleri karşılaması gerekir.<ul> <li>Sabit kapsam, sabit süre, sabit fiyat (veya ücretsiz) engagement sunun.</li> <li>Satış öncesi için öncelikle yönlendir.</li> <li>Tek bir müşteriye sınırlayın.</li> <li>Sitede yürütün.</li> </ul> |  
| İş ortağı Danışmanlık Hizmetleri gereksinimleri | Hizmetiniz için ilgili alanda ölçütlere uygun olmalıdır.<table><tr><th>Çözüm Alanı</th><th>Ölçütler</th></tr><tr><td>Müşteri Etkileşimi için Dynamics 365</td><td>Silver veya Gold bulut müşteri ilişkileri yönetimi uzmanlığa sahip.</td></tr><tr><td>Finans ve operasyon, Enterprise edition için Dynamics 365</td><td>Silver veya Gold kurumsal kaynak planlama uzmanlığı ve bulut geliri sondaki 12 ay sonra 25.000 ABD Doları ya da daha fazlasına sahip.</td></tr><tr><td>Dynamics 365 için finans ve operasyon, işletme sürümü</td><td>Bulut Hizmetleri Sağlayıcısı (CSP) ya da dijital iş ortağı, kaydı (DPOR) bir veya daha fazla müşterileri için hizmet.</td></tr><tr><td>Power BI</td><td>Çözüm iş ortağı ölçütlere uygun olmalıdır.</td></tr><tr><td>PowerApps</td><td>Bir iş ortağı gösterimi çözümü vardır.</td></tr></table><ul> <li>Müşteri İlişkileri Yönetimi hakkında daha fazla bilgi için ziyaret edin sayfasında bulunan bulut Müşteri İlişkileri Yönetimi [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency).</li> <li>Kaynak planlama hakkında daha fazla bilgi için ziyaret edin sayfasında bulunan Kurumsal Kaynak planlama [partner.microsoft.com/membership/enterprise-resource-planning-competency](https://partner.microsoft.com/membership/enterprise-resource-planning-competency).</li> <li>CSP hakkında daha fazla bilgi için ziyaret edin sayfasında bulunan bulut hizmeti sağlayıcısı [partner.microsoft.com/cloud-solution-provider](https://partner.microsoft.com/cloud-solution-provider).</li> <li>Dijital kayıtlı iş ortağı DPOR hakkında daha fazla bilgi için ziyaret edin ve iş ortağı ilişkilendirme sayfasında bulunan [partner.microsoft.com/membership/digital-partner-of-record](https://partner.microsoft.com/membership/digital-partner-of-record).</li> <li>Çözüm iş ortağı ölçütleri hakkında daha fazla bilgi için çözüm iş ortağı genel bakış sayfasını ziyaret edin ve Teşvikleri belge bulunan [www.microsoftpartnerserverandcloud.com/_layouts/download.aspx? SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Power%20BI%20Program%20Overview%20%26%20Incentives.pdf).</li> <li>İş ortağı gösterimi hakkında daha fazla bilgi için ziyaret edin sayfasında bulunan iş ortağı gösterimi [powerapps.microsoft.com/partner-showcase](https://powerapps.microsoft.com/partner-showcase).</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Gereksinimleri: Azure Market: liste: danışmanlık hizmeti  

| Gereksinimler | Ayrıntılar |  
|:--- |:--- |  
| Hizmet teklifi özellikleri | Danışmanlık hizmetinize aşağıdaki ölçütleri karşılaması gerekir.<ul> <li>Sabit kapsam, sabit süre, sabit fiyat (veya ücretsiz) engagement sunun.</li> <li>Satış öncesi için öncelikle yönlendir.</li> <li>Tek bir müşteriye sınırlayın.</li> <li>Sitede yürütün.</li> </ul> |  
| İş ortağı Danışmanlık Hizmetleri gereksinimleri | Silver veya gold ilgili alanda aşağıdaki uzmanlıklar biriyle hizmetiniz için aşağıdakiler gereklidir. <table><tr><th>Çözüm Alanı</th><th>Uzmanlığı</th></tr><td>Bulut platformu ve altyapı</td><td>Bulut platformu<br />Veri Merkezi</td><tr><td>Uygulama geliştirme ve ISV</td><td>Uygulama Geliştirme<br />Uygulama Tümleştirme<br />DevOps</td></tr><tr><td>Veri Yönetimi ve analiz</td><td>Veri Analizi<br />Veri Platformu</td></tr></table><ul> <li>Uzmanlıklar aracılığıyla Microsoft iş ortağı sayfasında bulunan ağ uzmanlıklar hakkında daha fazla bilgi için ziyaret [partner.microsoft.com/membership/competencies](https://partner.microsoft.com/membership/competencies).</li> <li>Azure Market danışmanlık sayfasında bulunan hizmetleri listeleme hakkında daha fazla bilgi için ziyaret [docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services).</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---   

### <a name="trial"></a>Deneme  

| Teklif türü | Vitrin | Ayrıntılar |  
|:---        |:---        |:---     |  
| Ücretsiz / SaaS denemesi | AppSource | [Türü gereksinimlerini listelerken: deneme](#listing-type-requirements-trial) |  
| Ücretsiz / SaaS denemesi | Azure Market | [Gereksinimleri: Azure Market: deneme: ücretsiz deneme sürümü / SaaS denemesi](#requirements-azure-marketplace-trial-free-trial-/-saas-trial) |  
| Etkileşimli tanıtım | AppSource | [Türü gereksinimlerini listelerken: deneme](#listing-type-requirements-trial) |  
| Etkileşimli tanıtım | Azure Market | [Gereksinimleri: Azure Market: deneme: etkileşimli tanıtım](#requirements-azure-marketplace-trial-interactive-demo) |  
| Test sürüşü | AppSource | [Türü gereksinimlerini listelerken: deneme](#listing-type-requirements-trial) |  
| Test sürüşü | Azure Market | [Gereksinimleri: Azure Market: deneme: Test sürücü](#requirements-azure-marketplace-trial-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Gereksinimleri: Azure Market: deneme  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Ücretsiz deneme süresi ve deneme sürümü deneyimi | Müşterinizin, uygulamanızı ücretsiz sınırlı bir süre için kullanabilir.<br /><br />Müşterinizin teklifine veya uygulama için herhangi bir lisans ya da Abonelik ücretleri ödemek için gerekli değildir. Müşteri temel alınan Microsoft birinci taraf ürün veya hizmet için ödeme gerekli değildir. Tüm deneme sürümü seçeneği, Azure aboneliğinize dağıtılır. Yönetim ve maliyet iyileştirmesi tek denetim deneme var.<br /><br />Ücretsiz deneme, etkileşimli tanıtım seçin veya test sürüşü gerçekleştirin. Ücretsiz Deneme süreniz, neyin seçin ne olursa olsun, ek ücret ödemeden uygulamayı denemek için önceden ayarlanmış bir süre müşteri sunmalıdır.<ul> <li>Bir test sürüşüne oluşturma işlemine başlamak için bir e-posta Gönder [ amp-testdrive@microsoft.com ](mailto:amp-testdrive@microsoft.com).</li> </ul>Not: Oturum açmak için iş kimlik bilgilerini kullanmak, müşterilerin Azure Market SaaS deneme deneyimleri izin vermeniz gerekir.<ul> <li>Daha fazla bilgi için deneme deneyimleri bölümünde bulunan AppSource ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences).</li> </ul> |  
| Kolayca yapılandırılabilir, kullanıma hazır bir çözüm | Uygulamanızı, kolay ve hızlı yapılandırmak ve ayarlamak olmalıdır. |  
| Kullanılabilirlik / çalışma süresi | En az % 99,9 çalışma süresi, SaaS uygulaması veya platformu olmalıdır. |  
| Azure Active Directory | Teklifinizi Azure Active Directory (Azure AD) ile etkin onay Federasyon çoklu oturum açma (SSO) (Azure AD Federasyon SSO) izin vermeniz gerekir. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Gereksinimleri: Azure Market: deneme: ücretsiz deneme sürümü / SaaS denemesi  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Ürün kullanımı dönüştürme işlemi için otomatikleştirilmiş bir yöntem ile satın alma ödenen önce denemek bir müşteri sağlar. Ayrıca müşteri ve ortak bir Microsoft satış ekipleri etkileşim için kavram kanıtlarını sağlar. | Bir sanal makine ya da çözüm şablonu bir çözümdür.<br /><br />Sunan bir SaaS çözümünüz olduğu ve çok kiracılı bir SaaS ürün sunar.<br /><br />Bir müşteri getirmek için ilk kez çalıştırma deneyimi ve hızlı bir şekilde çalışan var.<br /><br />Tek bir kiracının sahip ancak müşterilerin Konuk kullanıcı olarak ekliyoruz. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Gereksinimleri: Azure Market: deneme: etkileşimli tanıtım  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Müşterileriniz, çözümünüzün karmaşıklığı olmadan kurma eylemi de görmek etkinleştirir. | Çözümünüz içinde deneme süresi elde etmek zor olabilir karmaşık ayarları gerektirir. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Gereksinimleri: Azure Market: deneme: Test sürücü  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Bir müşteri, ürün, satın almadan önce denemek etkinleştirir.<br /><br />Önceden yapılandırılmış ayarları kullanılarak çözümünüzün Kılavuzlu bir deneyim sağlar.<br /><br />Ek avantajlar test sürüşü kullanırken aşağıda verilmiştir.<ul> <li>Kullanıcı aramalarını Marketi'nde 27 yüzdesi, kullanıcılar tarafından yalnızca test sürüşleri show teklifleri için iyileştirilmiştir.</li> <li>Test sürüşleri teklifleri teklifinden %38 daha fazla müşteri adayları oluşturun.</li> <li>Market'te yeni müşteri edinme %36 bir test sürüşüne geçen müşteriler gelir.</li> <li>Test sürüşleri, ürün için ortak satış çalışmalarınızı daha iyi anlamak, Microsoft saha satıcılarına etkinleştirin.</li> </ul> | Çözümünüzde bir sanal makine, çözüm şablonu ya da tek Kiracı SaaS uygulamasıyla veya sağlamayı karmaşık bir işlemdir. <br /><br />Denemenizi Ücretli bir teklif dönüştürmek için bir yöntem yoktur. |  

---

## <a name="transact-specific-listings"></a>Transact-özel listeleri

### <a name="transact"></a>İşlem  

| Teklif türü | Vitrin | Ayrıntılar |   
|:---        |:---        | :--- |  
| Azure uygulamaları: yönetilen uygulama | Azure Market | [Gereksinimleri: Azure Market: Transact: Azure uygulamaları: yönetilen uygulama](#requirements-azure-marketplace-transact-azure-apps-managed-app) |  
| Azure uygulamaları: Çözüm şablonu | Azure Market | [Gereksinimleri: Azure Market: Transact: Azure uygulamaları: Çözüm şablonu](#requirements-azure-marketplace-transact-azure-apps-solution-template) |  
| Kapsayıcılar | Azure Market | [Gereksinimleri: Azure Market: Transact: kapsayıcı](#requirements-azure-marketplace-transact-container) |  
| SaaS uygulama  | Azure Market | [Gereksinimleri: Azure Market: Transact: SaaS uygulaması](#requirements-azure-marketplace-transact-saas-app) |  
| Sanal makine | Azure Market | [Gereksinimleri: Azure Market: Transact: sanal makine](#requirements-azure-marketplace-transact-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Gereksinimleri: Azure Market: Transact: kapsayıcı  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçüm | Destek ya da ücretsiz veya KLG faturalandırma modeli. |  
| Docker tabanlı görüntü | Kapsayıcı görüntünüzü Docker görüntü biçimi temel alarak gerekir ve Azure Container kayıt defterlerinden çekilmesi gerekir. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Gereksinimleri: Azure Market: Transact: SaaS uygulaması  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçüm | Teklifiniz, aylık bir sabit ücretle fiyatlandırılır. Kullanım tabanlı fiyatlandırma ve kullanım tabanlı *true yukarı* seçenekleri şu anda desteklenmiyor. |  
| İptal etme | Teklifinizi istediğiniz zaman müşteri tarafından iptal edilebilir. |  
| İşlem giriş sayfası | Bir Azure ortak markalı işlem giriş sayfası barındırın. Giriş sayfanızı oluşturmak ve SaaS hizmeti hesabınızı yönetmek, müşterilerin sağlar. |  
| SaaS abonelik API | Etkileşime giren bir hizmet oluşturmak, güncelleştirmek ve bir kullanıcı hesabı ve hizmet planını silmek için SaaS abonelikle sağlar. Tüm kritik API değişiklikleri 24 saat içinde desteklenmesi gerekir. Kritik olmayan API değişiklikleri düzenli olarak güncelleştirilir. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Gereksinimleri: Azure Market: Transact: sanal makine  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalama ve ölçüm | Sanal makinenizin KLG ya da Kullandıkça Öde aylık faturalandırma desteklemesi gerekir. |  
| Azure ile uyumlu sanal sabit disk (VHD) | Windows veya Linux Vm'leri oluşturulmalıdır.<ul> <li>Azure ile uyumlu bir VHD (Linux tabanlı) bölümünde bulunan oluşturma Linux VHD'si oluşturma hakkında daha fazla bilgi için ziyaret [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-Compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Bir Windows VHD oluşturma hakkında daha fazla bilgi için Azure ile uyumlu bir VHD (Windows tabanlı) bölümünde bulunan Oluştur ziyaret [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-Compatible-VHD-Windows-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul> |  

## <a name="next-steps"></a>Sonraki adımlar
*   Ziyaret [Azure Market ve AppSource yayımcı Kılavuzu](./marketplace-publishers-guide.md) sayfası.  
 
---  
