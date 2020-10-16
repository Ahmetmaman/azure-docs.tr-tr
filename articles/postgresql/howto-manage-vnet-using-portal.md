---
title: Sanal ağ kurallarını kullanma-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: VNet hizmet uç noktalarını ve kurallarını oluşturma ve yönetme PostgreSQL için Azure veritabanı-Azure portal kullanarak tek sunucu
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 8ff1800bc699a7fb29f64b63a3098225921628df
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91710847"
---
# <a name="create-and-manage-vnet-service-endpoints-and-vnet-rules-in-azure-database-for-postgresql---single-server-by-using-the-azure-portal"></a>PostgreSQL için Azure veritabanı 'nda VNet hizmet uç noktaları ve VNet kuralları oluşturma ve yönetme-Azure portal kullanarak tek sunucu
Sanal ağ (VNet) Hizmetleri uç noktaları ve kuralları, bir sanal ağın özel adres alanını PostgreSQL için Azure veritabanı sunucusuna genişletir. PostgreSQL için Azure veritabanı VNet hizmeti uç noktalarına yönelik sınırlamalar da dahil olmak üzere, bkz. [PostgreSQL Için Azure veritabanı sunucu VNET hizmet uç noktaları](concepts-data-access-and-security-vnet.md). VNet hizmet uç noktaları, PostgreSQL için Azure veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> VNet hizmet uç noktaları için destek yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucular içindir.
> VNet eşlemesi söz konusu olduğunda trafik, hizmet uç noktaları içeren bir ortak VNet ağ geçidi üzerinden akar ve eşe akışı gerekiyorsa, ağ geçidi VNet 'teki Azure sanal makinelerinin PostgreSQL için Azure veritabanı sunucusuna erişmesine izin vermek için lütfen bir ACL/VNet kuralı oluşturun.


## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>VNet kuralı oluşturma ve Azure portal hizmet uç noktalarını etkinleştirme

1. PostgreSQL sunucusu sayfasında, ayarlar başlığı altında **bağlantı güvenliği** ' ne tıklayarak PostgreSQL Için Azure veritabanı bağlantı güvenliği bölmesini açın. 

2. Azure hizmetleri denetimine erişime Izin ver ' in **kapalı**olarak ayarlandığından emin olun.

> [!Important]
> Denetimi açık olarak bırakırsanız, Azure PostgreSQL veritabanı sunucunuz herhangi bir alt ağdan gelen iletişimi kabul eder. Denetim kümesini açık olarak bırakmak, bir güvenlik noktasından aşırı erişim sağlayabilir. PostgreSQL için Azure veritabanı 'nın sanal ağ kuralı özelliğiyle birlikte Microsoft Azure Sanal Ağ Hizmeti uç noktası özelliği, güvenlik yüzeyi alanınızı azaltabilir.

3. Ardından, **var olan sanal ağı ekleme**' ye tıklayın. Mevcut bir VNet 'iniz yoksa bir tane oluşturmak için **+ Yeni sanal ağ oluştur ' a** tıklayabilirsiniz. Bkz [. hızlı başlangıç: Azure Portal kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   :::image type="content" source="./media/howto-manage-vnet-using-portal/1-connection-security.png" alt-text="Azure portal-bağlantı güvenliği ' ne tıklayın":::

4. VNet kural adı girin, abonelik, sanal ağ ve alt ağ adını seçin ve ardından **Etkinleştir**' e tıklayın. Bu, alt ağdaki VNet hizmet uç noktalarını **Microsoft. SQL** hizmet etiketi kullanılarak otomatik olarak sunar.

   :::image type="content" source="./media/howto-manage-vnet-using-portal/2-configure-vnet.png" alt-text="Azure portal-bağlantı güvenliği ' ne tıklayın" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
    [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
    Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. VNet ve Azure hizmet kaynakları farklı aboneliklerdeyse, kaynakların aynı Active Directory (AD) kiracısı altında olması gerekir. Her iki aboneliğin de **Microsoft. SQL** kaynak sağlayıcısı 'nın kayıtlı olduğundan emin olun. Daha fazla bilgi için [Resource-Manager-kayıt][resource-manager-portal] bölümüne bakın

   > [!IMPORTANT]
   > Hizmet uç noktalarını yapılandırmadan önce hizmet uç noktası yapılandırmalarına ve noktalara ilişkin bu makalenin okunması kesinlikle önerilir. **Sanal ağ hizmeti uç noktası:** [Sanal ağ hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) , özellik değerleri bir veya daha fazla resmi Azure hizmet türü adı içeren bir alt ağıdır. VNet Hizmetleri uç noktaları, SQL veritabanı adlı Azure hizmetine başvuran **Microsoft. SQL**hizmet türü adını kullanır. Bu hizmet etiketi, Azure SQL veritabanı, PostgreSQL için Azure veritabanı ve MySQL Hizmetleri için de geçerlidir. **Microsoft. SQL** hizmet etiketi bir sanal ağ hizmeti uç noktasına uygulandığında, Azure SQL veritabanı, PostgreSQL Için Azure veritabanı ve alt ağdaki MySQL sunucuları Için Azure veritabanı dahil olmak üzere tüm Azure veritabanı hizmetleri için hizmet uç noktası trafiğini yapılandırır. 
   > 

5. Etkinleştirildikten sonra **Tamam** ' a tıkladığınızda, VNET hizmet uç noktalarının sanal ağ kuralıyla birlikte etkinleştirildiğini görürsünüz.

   :::image type="content" source="./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png" alt-text="Azure portal-bağlantı güvenliği ' ne tıklayın":::

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, [VNET hizmet uç noktalarını etkinleştirmek ve Azure CLI kullanarak PostgreSQL Için Azure veritabanı için BIR VNET kuralı oluşturmak](howto-manage-vnet-using-cli.md)üzere komut dosyası oluşturabilirsiniz.
- PostgreSQL için Azure veritabanı sunucusuna bağlanma konusunda yardım için bkz. [PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](./concepts-connection-libraries.md)

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md