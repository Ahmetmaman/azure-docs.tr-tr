---
title: Azure Market görüntüleri için güvenlik önerileri | Microsoft Docs
description: Bu makale, Pazar yerinde bulunan görüntüler için öneriler sağlar
services: security
documentationcenter: na
author: terrylanfear
manager: barbkess
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: terrylan
ms.openlocfilehash: 7c317a0b4fea0c981b227bace00c1b8924fd582c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "89536391"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Azure Market görüntüleri için güvenlik önerileri

Görüntünüzün bu güvenlik yapılandırması önerilerini karşılaması gerekir. Bu, Azure Marketi 'nde iş ortağı çözümü görüntüleri için yüksek düzeyde güvenlik sağlanmasına yardımcı olur.

Göndermeden önce, yansımanıza her zaman bir güvenlik açığı algılaması çalıştırın. Yayınlanmış görüntinizdeki bir güvenlik açığı tespit ederseniz, müşterilerinizin her iki güvenlik açığından de zamanında bilgilendirmeniz ve nasıl düzeltilebileceğiniz konusunda bilgi sahibi olmanız gerekir.

## <a name="open-source-based-images"></a>Açık kaynak tabanlı görüntüler

| Kategori | İşaretli |
| -------- | ----- |
| Güvenlik                                                     | Linux dağıtımı için en son güvenlik düzeltme eklerini yükler.                                                                                                                                                                                                              |
| Güvenlik                                                     | Belirli bir Linux dağıtımına yönelik VM görüntüsünü güvenli hale getirmek için sektör kılavuzlarını izleyin.                                                                                                                                                                                     |
| Güvenlik                                                     | Yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktalarıyla en az sayıda parmak izi tutarak saldırı yüzeyini sınırlayın.                                                                                                                                               |
| Güvenlik                                                     | Kötü amaçlı yazılım için kaynak kodu ve ortaya çıkan VM görüntüsünü tarayın.                                                                                                                                                                                                                                   |
| Güvenlik                                                     | VHD görüntüsü yalnızca, etkileşimli oturum açmaya izin verecek olan varsayılan parolalara sahip gerekli kilitli hesapları içerir; arka kapı yok.                                                                                                                                           |
| Güvenlik                                                     | Uygulama işlevsellik bir güvenlik duvarı gereci gibi bunlara dayanmadığı sürece güvenlik duvarı kurallarını devre dışı bırakın.                                                                                                                                                                             |
| Güvenlik                                                     | Test SSH anahtarları, bilinen ana bilgisayar dosyası, günlük dosyaları ve gereksiz sertifikalar gibi tüm hassas bilgileri VHD görüntüsünden kaldırın.                                                                                                                                       |
| Güvenlik                                                     | LVM kullanmaktan kaçının.                                                                                                                                                                                                                                            |
| Güvenlik                                                     | Gerekli kitaplıkların en son sürümlerini ekleyin: </br> -OpenSSL v 1.0 veya üzeri </br> -Python 2,5 veya üzeri (Python 2.6 + kesinlikle önerilir) </br> -Zaten yüklenmemişse Python pyasn1 paketi </br> -d. OpenSSL v 1,0 veya üzeri                                                                |
| Güvenlik                                                     | Bash/Shell geçmiş girdilerini temizleyin.                                                                                                                                                                                                                                             |
| Ağ                                                   | SSH sunucusunu varsayılan olarak dahil edin. SSH 'yi şu seçenekle SSHD config 'e canlı tut olarak ayarlayın: ClientAliveInterval 180.                                                                                                                                                        |
| Ağ                                                   | Tüm özel ağ yapılandırmalarını görüntüden kaldırın. Resolv. conf dosyasını silin: `rm /etc/resolv.conf` .                                                                                                                                                                                |
| Dağıtım                                                   | En son Azure Linux aracısını yükler.</br> -RPM veya Deb paketini kullanarak yükler.  </br> -El ile yükleme işlemini de kullanabilirsiniz, ancak yükleyici paketleri önerilir ve tercih edilir. </br> -Aracıyı GitHub deposundan el ile yüklüyorsanız, önce `waagent` dosyayı kopyalayın `/usr/sbin` ve (kök olarak) çalıştırın: </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Aracı yapılandırma dosyası konumunda yer alır `/etc/waagent.conf` . |
| Dağıtım                                                   | Gerektiğinde Azure desteğinin seri konsol çıkışı ile iş ortaklarımıza sağlayabildiğini ve bulut depolamadan işletim sistemi diski bağlama için yeterli zaman aşımı sağlayıp sağlamamasını sağlayın. Aşağıdaki parametreleri görüntü çekirdeği önyükleme satırına ekleyin: `console=ttyS0 earlyprintk=ttyS0 rootdelay=300` . |
| Dağıtım                                                   | İşletim sistemi diski üzerinde takas bölümü yok. Linux Aracısı tarafından yerel kaynak diskinde oluşturma için değiştirme istenebilir.         |
| Dağıtım                                                   | İşletim sistemi diski için tek bir kök bölüm oluşturun.      |
| Dağıtım                                                   | yalnızca 64 bitlik işletim sistemi.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server tabanlı görüntüler

| Kategori | İşaretli |
|--------- | ----- |
| Güvenlik                                                         | Güvenli bir işletim sistemi temel görüntüsü kullanın. Windows Server tabanlı herhangi bir görüntünün kaynağı için kullanılan VHD, Microsoft Azure ile sunulan Windows Server işletim sistemi görüntülerinden olmalıdır. |
| Güvenlik                                                         | Tüm en son güvenlik güncelleştirmelerini yükler.                                                                                                                                     |
| Güvenlik                                                         | Uygulamalar, yönetici, kök veya yönetici gibi kısıtlı kullanıcı adlarına bağlı olmamalıdır.                                                                |
| Güvenlik                                                         | Hem işletim sistemi sabit sürücülerinde hem de veri sabit sürücülerinde BitLocker Sürücü Şifrelemesi etkinleştirin.                                                             |
| Güvenlik                                                         | Yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktaları etkin olan en az sayıda kaplama tutarak saldırı yüzeyini sınırlayın.                         |
| Güvenlik                                                         | Kötü amaçlı yazılım için kaynak kodu ve ortaya çıkan VM görüntüsünü tarayın.                                                                                                                     |
| Güvenlik                                                         | Windows Server görüntülerini güvenlik güncelleştirmesini otomatik güncelleştirme olarak ayarlayın.                                                                                                                |
| Güvenlik                                                         | VHD görüntüsü yalnızca, etkileşimli oturum açmaya izin verecek olan varsayılan parolalara sahip gerekli kilitli hesapları içerir; arka kapı yok.                             |
| Güvenlik                                                         | Uygulama işlevsellik bir güvenlik duvarı gereci gibi bunlara dayanmadığı sürece güvenlik duvarı kurallarını devre dışı bırakın.                                                               |
| Güvenlik                                                         | Ana bilgisayar dosyaları, günlük dosyaları ve gereksiz Sertifikalar dahil olmak üzere VHD görüntüsünden tüm hassas bilgileri kaldırın.                                              |
| Dağıtım                                                       | yalnızca 64 bitlik işletim sistemi.                            |

Kuruluşunuzun Azure Marketi 'nde görüntüleri olmasa bile, bu önerilere karşı Windows ve Linux görüntü yapılandırmalarının denetlenmesini göz önünde bulundurun.

