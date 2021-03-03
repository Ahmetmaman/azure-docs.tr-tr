---
title: Data Box Gateway sistem gereksinimlerini Microsoft Azure | Microsoft Docs
description: Azure Data Box Gateway için yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/01/2021
ms.author: alkohli
ms.openlocfilehash: e7c8653b39a3e0333ff6e98783a6e9a1437dba22
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101739220"
---
# <a name="azure-data-box-gateway-system-requirements"></a>Azure Data Box Gateway sistem gereksinimleri

Bu makalede, Microsoft Azure Data Box Gateway çözümünüz ve Azure Data Box Gateway bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Data Box Gateway dağıtmadan önce bilgileri dikkatlice incelemenizi ve ardından dağıtım ve sonraki işlemler sırasında gerektiği şekilde geri başvurmalarını öneririz. 

Data Box Gateway sanal cihazının sistem gereksinimleri şunlardır:

- **Konaklar Için yazılım gereksinimleri** -desteklenen platformları, yerel yapılandırma kullanıcı arabirimi için TARAYıCıLARı, SMB istemcilerini ve cihaza bağlanan konaklara yönelik ek gereksinimleri açıklar.
- **Cihaz Için ağ gereksinimleri** -sanal cihazın çalışması için tüm ağ gereksinimleri hakkında bilgi sağlar.


## <a name="specifications-for-the-virtual-device"></a>Sanal cihaz belirtimleri

Data Box Gateway için temel ana bilgisayar sistemi, sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabiliyor:

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Sanal işlemciler (çekirdekler)   | En az 4 |
| Bellek  | En az 8 GB. En az 16 GB önerilir. |
| Kullanılabilirlik|Tek düğüm|
| Diskler| İşletim sistemi diski: 250 GB <br> Veri diski: En az 2 TB, ölçülü kaynak sağlamalı ve SSD destekli olmalıdır|
| Ağ arabirimleri|1 veya daha çok sanal ağ arabirimi|


## <a name="supported-os-for-clients-connected-to-device"></a>Cihaza bağlı istemciler için desteklenen işletim sistemi

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Cihaza erişen istemciler için desteklenen protokoller

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-virtualization-platforms-for-device"></a>Cihaz için desteklenen sanallaştırma platformları

| **İşletim sistemi/platform**  |**Sürümler**   |**Notlar**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016 <br> 2019 |         |
|VMware ESXi     | 6.0 <br> 6.5 <br> 6.7       |VMware araçları desteklenmez.         |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]


## <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Yerel Web Kullanıcı arabirimi için desteklenen tarayıcılar

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Ağ bağlantı noktası gereksinimleri

Aşağıdaki tabloda SMB, bulut veya Yönetim trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenmektedir. Bu tabloda, veya *gelen* *içinde* , gelen istemci, cihazınıza erişim talep ettiği yöne başvurur. *Out* veya *Outbound* , dağıtım ötesinde Data Box Gateway cihazınızın verileri dışarıdan gönderdiği yönü ifade eder. Örneğin, Internet 'e giden.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL desenleri

Ağ yöneticileri, genellikle gelen ve giden trafiği filtrelemek için URL desenlerine göre gelişmiş güvenlik duvarı kuralları yapılandırabilir. Data Box Gateway cihazınız ve Data Box Gateway hizmeti Azure Service Bus, Azure Active Directory Access Control, depolama hesapları ve Microsoft Update sunucuları gibi diğer Microsoft uygulamalarına bağlıdır. Bu uygulamalarla ilişkili URL desenleri güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerinin değiştirebileceğini anlamak önemlidir. Bu sırayla, ağ yöneticisinin Data Box Gateway için güvenlik duvarı kurallarını izleyip ve gerektiğinde güncelleştirmesi gerekir.

Çoğu durumda, sabit IP adreslerini Data Box Gateway temel alarak, giden trafik için güvenlik duvarı kurallarınızı ayarlamanızı öneririz. Bununla birlikte, güvenli ortamlar oluşturmak için gerekli olan gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - Cihaz (kaynak) IP 'Leri her zaman bulut özellikli tüm ağ arabirimlerine ayarlanmalıdır.
> - Hedef IP 'Ler, [Azure veri MERKEZI IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)olarak ayarlanmalıdır.

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-azure-government"></a>Azure Kamu için URL desenleri

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/data-box-edge-gateway-gov-url-patterns-firewall.md)]

## <a name="internet-bandwidth"></a>Internet bant genişliği

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Sonraki adım

* [Azure Data Box Gateway dağıtın](data-box-gateway-deploy-prep.md)

