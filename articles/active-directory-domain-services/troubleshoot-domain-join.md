---
title: Azure AD Domain Services etki alanına ekleme sorunlarını giderme | Microsoft Docs
description: Bir VM 'ye katılmayı veya bir uygulamayı Azure Active Directory Domain Services bağlamayı denediğinizde ve yönetilen etki alanına bağlanamadığınızda veya kimlik doğrulayabileceğinizden sık karşılaşılan sorunları nasıl giderebileceğinizi öğrenin.
services: active-directory-ds
author: MicrosoftGuyJFlo
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: joflore
ms.openlocfilehash: ee60b684d64ef49fbb669de8c98203e2df5268bf
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91967520"
---
# <a name="troubleshoot-domain-join-problems-with-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Domain Services yönetilen bir etki alanı ile etki alanı ekleme sorunlarını giderme

Bir sanal makineye (VM) katılmayı veya bir uygulamayı Azure Active Directory Domain Services (Azure AD DS) yönetilen bir etki alanına bağlamayı denediğinizde, yapabağlanamadığınızı belirten bir hata alabilirsiniz. Etki alanına ekleme sorunlarını gidermek için, aşağıdaki noktalarda bir sorun olduğunu gözden geçirin:

* Bir kimlik doğrulama istemi almazsanız, VM veya uygulama Azure AD DS yönetilen etki alanına bağlanamaz.
    * [Etki alanına katılacak bağlantı sorunlarını](#connectivity-issues-for-domain-join)gidermeye başlayın.
* Kimlik doğrulaması sırasında bir hata alırsanız, yönetilen etki alanı bağlantısı başarılı olur.
    * [Etki alanı birleşimi sırasında kimlik bilgileriyle ilgili sorunları](#credentials-related-issues-during-domain-join)gidermeye başlayın.

## <a name="connectivity-issues-for-domain-join"></a>Etki alanına katılacak bağlantı sorunları

VM yönetilen etki alanını bulamazsa, genellikle bir ağ bağlantısı veya yapılandırma sorunu vardır. Sorunu bulmak ve çözmek için aşağıdaki sorun giderme adımlarını gözden geçirin:

1. VM 'nin, yönetilen etki alanı ile aynı veya eşlenmiş bir sanal ağa bağlı olduğundan emin olun. Aksi takdirde, VM, katılabilmek için etki alanını bulup bağlanamaz.
    * VM aynı sanal ağa bağlı değilse, sanal ağ eşleme veya VPN bağlantısının *etkin* olduğunu veya trafiğin doğru şekilde akmasını sağlamak için *bağlı* olduğunu doğrulayın.
1. Yönetilen etki alanının etki alanı adını (gibi) kullanarak etki alanına ping yapmayı deneyin `ping aaddscontoso.com` .
    * Ping yanıtı başarısız olursa, yönetilen etki alanınız için Portal 'daki Genel Bakış sayfasında görüntülenecek etki alanının IP adreslerini ping yapmayı deneyin (örneğin,) `ping 10.0.0.4` .
    * IP adresine başarılı bir şekilde ping işlemi yapabiliyorsanız, DNS yanlış yapılandırılmış olabilir. [Sanal ağ için yönetilen etki alanı DNS sunucularını yapılandırdığınızdan][configure-dns]emin olun.
1. Sanal makinede DNS çözümleyici önbelleğini (gibi) temizlemeye çalışın `ipconfig /flushdns` .

### <a name="network-security-group-nsg-configuration"></a>Ağ güvenlik grubu (NSG) yapılandırması

Yönetilen bir etki alanı oluşturduğunuzda, başarılı etki alanı işlemi için uygun kurallara sahip bir ağ güvenlik grubu da oluşturulur. Ek ağ güvenlik grubu kuralları düzenler veya oluşturursanız, Azure AD DS bağlantı ve kimlik doğrulama hizmetleri sağlamak için gerekli olan bağlantı noktalarını istemeden engelleyebilirsiniz. Bu ağ güvenlik grubu kuralları, parola eşitleme, kullanıcıların oturum açamayacak veya etki alanına katılması sorunları gibi sorunların oluşmasına neden olabilir.

Bağlantı sorunlarıyla karşılaşmaya devam ederseniz, aşağıdaki sorun giderme adımlarını gözden geçirin:

1. Azure portal yönetilen etki alanının sistem durumunu kontrol edin. *AADDS001*için bir uyarıınız varsa, bir ağ güvenlik grubu kuralı erişimi engelliyor demektir.
1. [Gerekli bağlantı noktalarını ve ağ güvenlik grubu kurallarını][network-ports]gözden geçirin. Bağlı olduğunuz VM veya sanal ağa hiçbir ağ güvenlik grubu kuralı, bu ağ bağlantı noktalarını engellemediğinizden emin olun.
1. Herhangi bir ağ güvenlik grubu yapılandırma sorunu çözümlendikten sonra, *AADDS001* uyarısı yaklaşık 2 saat içinde sistem durumu sayfasından kaldırılır. Ağ bağlantısı artık kullanılabilir olduğundan, VM 'yi yeniden birleştirmeyi deneyin.

## <a name="credentials-related-issues-during-domain-join"></a>Etki alanına ekleme sırasında kimlik bilgileri ile ilgili sorunlar

Yönetilen etki alanına katılması için kimlik bilgileri isteyen bir iletişim kutusu alırsanız, sanal makine Azure sanal ağını kullanarak etki alanına bağlanabilir. Etki alanına ekleme işlemi, etki alanına veya yetkilendirmede kimlik doğrulama işlemini, kimlik bilgileri tarafından sağlanan kimlik bilgilerini kullanarak tamamlamaya başarısız olur.

Kimlik bilgileriyle ilgili sorunları gidermek için aşağıdaki sorun giderme adımlarını gözden geçirin:

1. Gibi kimlik bilgilerini belirtmek için UPN biçimini kullanmayı deneyin `dee@contoso.onmicrosoft.com` . Azure AD 'de bu UPN 'nin doğru yapılandırıldığından emin olun.
    * Kiracınızda aynı UPN ön ekine sahip birden fazla kullanıcı varsa veya UPN önekiniz aşırı uzunsa, hesabınız için *sAMAccountName* otomatik olarak oluşturulabilir. Bu nedenle, hesabınız için *sAMAccountName* biçimi, şirket içi etki alanında beklediğiniz veya kullandığınız verilerden farklı olabilir.
1. VM 'Leri yönetilen etki alanına katmak için yönetilen etki alanının bir parçası olan bir kullanıcı hesabının kimlik bilgilerini kullanmayı deneyin.
1. [Parola eşitlemesini][enable-password-sync] etkinleştirdiğinizden ve ilk parola eşitlemenin tamamlanabilmesi için yeterince uzun süre beklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Etki alanı ekleme işleminin bir parçası olarak Active Directory süreçlerini daha ayrıntılı bir şekilde anlamak için bkz. [JOIN ve Authentication sorunları][join-authentication-issues].

VM 'nize yönetilen etki alanına katılma sorunları yaşamaya devam ediyorsanız Yardım ' ı [bulun ve Azure Active Directory için bir destek bileti açın][azure-ad-support].

<!-- INTERNAL LINKS -->
[enable-password-sync]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-dns]: tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network

<!-- EXTERNAL LINKS -->
[join-authentication-issues]: /previous-versions/windows/it-pro/windows-2000-server/cc961817(v=technet.10)
