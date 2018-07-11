---
title: Azure portalında bir Windows VM için FQDN oluşturma | Microsoft Docs
description: FQDN, Azure portalında sanal makine için bir kaynak yöneticisi tabanlı veya tam etki alanı adı oluşturma öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79b3e3ba01076dcfe5820fb832fa1839398c6c04
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927614"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Bir Windows VM için Azure portalını kullanarak bir tam etki alanı adı oluşturma

İçinde bir sanal makine (VM) oluştururken [Azure portalında](https://portal.azure.com), sanal makine için bir genel IP kaynağı otomatik olarak oluşturulur. Bu IP adresi, sanal Makineye uzaktan erişmek için kullanın. Portal oluşturma değil ancak bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), veya FQDN, oluşturabileceğiniz bir sanal makine oluşturulduktan sonra. Bu makalede, bir DNS adı veya FQDN oluşturma adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>Bir FQDN oluşturma
Bu makalede, bir VM zaten oluşturmuş olduğunuzu varsayar. Gerekirse, [portalda VM oluşturma](quick-create-portal.md) veya [Azure PowerShell ile](quick-create-powershell.md). Sanal makinenizin çalışır duruma geldikten sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık uzaktan bu DNS adı gibi Uzak Masaüstü Protokolü (RDP) kullanarak VM'ye bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
VM'NİZİN genel bir IP ve DNS adı olan, ortak uygulama çerçeveleri veya IIS, SQL ve SharePoint gibi hizmetleri dağıtabilirsiniz.

Ayrıca daha fazla ilgili bilgi edinebilirsiniz [Kaynak Yöneticisi'ni kullanarak](../../azure-resource-manager/resource-group-overview.md) Azure dağıtımlarınızı oluşturma ipuçları için.

