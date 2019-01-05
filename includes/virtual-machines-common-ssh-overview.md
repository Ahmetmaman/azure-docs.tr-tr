---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 12/21/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: bca78e2963f19b60071b1b27c8dc65c76818e10e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54029337"
---
## <a name="overview-of-ssh-and-keys"></a>SSH ve anahtarları'na genel bakış

[SSH](https://www.ssh.com/ssh/) güvenli bağlantılar üzerinden güvenli oturum açma işlemleri izin veren bir şifreli bağlantı protokolüdür. SSH Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. SSH kendisini şifreli bir bağlantı sağlar, ancak SSH bağlantıları ile Parolaları kullanarak hala VM yanılma saldırıları veya parolalar tahmin için savunmasız bırakır. Genel-özel anahtar çifti olarak da bilinen kullanarak SSH kullanarak VM'ye bağlanma daha güvenli ve tercih edilen bir yöntem olan *SSH anahtarları*. 

* *Ortak anahtar* Linux VM'nize veya ortak anahtar şifrelemesi ile kullanmak istediğiniz herhangi bir hizmeti yerleştirilir.

* *Özel anahtarı* yerel sisteminizde kalır. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

Linux (ortak anahtarı olan) VM'nize, uzak VM bağlanmak için bir SSH istemcisi kullandığınızda, istemci özel anahtara sahip olduğundan emin olmak için test eder. İstemci, özel anahtarı varsa, verilen sanal makine erişimine izin. 

Kuruluşunuzun güvenlik ilkelerine bağlı olarak, birden çok Azure Vm'lerini ve hizmetlerini erişmek için tek genel-özel anahtar çifti yeniden kullanabilirsiniz. Her VM'ye veya hizmete erişmek istediğiniz için ayrı bir çift anahtar gerekmez. 

Ortak anahtarınız herkesle paylaşılabilir, ancak özel anahtarınız, yalnızca sizin (veya yerel güvenlik altyapınız) sahip olmanız gerekir.