---
title: Yönetilen sanal ağ
description: Azure SYNAPSE Analytics 'te yönetilen sanal ağı açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 949b7e55569cc6fceacc37677ed06a28bb85d7c2
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98116373"
---
# <a name="azure-synapse-analytics-managed-virtual-network"></a>Azure SYNAPSE Analytics yönetilen sanal ağı

Bu makalede, yönetilen sanal Ağa gelen Azure SYNAPSE Analytics açıklanmaktadır.

## <a name="managed-workspace-virtual-network"></a>Yönetilen çalışma alanı sanal ağı

Azure SYNAPSE çalışma alanınızı oluştururken Microsoft Azure Sanal Ağ ilişkilendirmeyi seçebilirsiniz. Çalışma alanınız ile ilişkili sanal ağ Azure SYNAPSE tarafından yönetiliyor. Bu sanal ağa *yönetilen çalışma alanı sanal ağı* adı verilir.

Yönetilen çalışma alanı sanal ağı, size dört şekilde değer sağlar:

- Yönetilen bir çalışma alanı sanal ağı ile sanal ağı Azure SYNAPSE ile yönetme yükünü devreedebilirsiniz.
- Azure SYNAPSE Management trafiğinin Sanal ağınızı girmelerini sağlamak için kendi sanal ağlarınızda gelen NSG kurallarını yapılandırmanız gerekmez. Bu NSG kurallarının yanlış yapılandırılması, müşteriler için hizmet kesintilerine neden olur.
- En yüksek yük temelinde Spark kümeleriniz için bir alt ağ oluşturmanız gerekmez.
- Yönetilen çalışma uç noktaları ile birlikte yönetilen çalışma alanı sanal ağı, veri taşmakla karşı koruma sağlar. Yönetilen özel uç noktaları yalnızca kendisiyle ilişkili bir yönetilen çalışma alanı sanal ağı olan bir çalışma alanında oluşturabilirsiniz.

Yönetilen bir çalışma alanı sanal ağı ile ilişkili bir çalışma alanı oluşturmak, çalışma alanınızın diğer çalışma alanlarından yalıtılmış olmasını sağlar. Azure SYNAPSE, bir çalışma alanında çeşitli analitik yetenekler sağlar: veri tümleştirme, sunucusuz Apache Spark havuzu, adanmış SQL havuzu ve sunucusuz SQL Havuzu.

Çalışma alanınızın yönetilen bir çalışma alanı sanal ağı varsa, veri tümleştirme ve Spark kaynakları buna dağıtılır. Yönetilen bir çalışma alanı sanal ağı aynı zamanda Spark etkinlikleri için Kullanıcı düzeyinde yalıtım sağlar çünkü her Spark kümesi kendi alt ağında yer almaktadır.

Adanmış SQL havuzu ve sunucusuz SQL havuzu çok kiracılı yeteneklerdir ve bu nedenle yönetilen çalışma alanı sanal ağının dışında bulunur. Adanmış SQL havuzu ve sunucusuz SQL havuzu için çalışma alanı içi iletişim Azure özel bağlantıları kullanır. Bu özel bağlantılar, onunla ilişkili bir yönetilen çalışma alanı sanal ağı ile bir çalışma alanı oluşturduğunuzda sizin için otomatik olarak oluşturulur.

>[!IMPORTANT]
>Çalışma alanı oluşturulduktan sonra bu çalışma alanı yapılandırmasını değiştiremezsiniz. Örneğin, kendisiyle ilişkili bir yönetilen çalışma alanı sanal ağı olmayan bir çalışma alanını yeniden yapılandıramaz ve bir sanal ağı onunla ilişkilendirebilirsiniz. Benzer şekilde, bir çalışma alanını onunla ilişkili yönetilen bir çalışma alanı sanal ağıyla yeniden yapılandıramazsınız ve sanal ağın ilişkisini kaldırın.

## <a name="create-an-azure-synapse-workspace-with-a-managed-workspace-virtual-network"></a>Yönetilen çalışma alanı sanal ağı ile Azure SYNAPSE çalışma alanı oluşturma

Daha önce yapmadıysanız, ağ kaynak sağlayıcısını kaydedin. Kaynak sağlayıcısı kaydı, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. [Kayıt](../../azure-resource-manager/management/resource-providers-and-types.md)sırasında kaynak sağlayıcıları listesinden *Microsoft. Network* ' ü seçin.

İlişkili bir yönetilen çalışma alanı sanal ağı olan bir Azure SYNAPSE çalışma alanı oluşturmak için Azure portal **ağ** sekmesini seçin ve **yönetilen sanal ağı etkinleştir** onay kutusunu işaretleyin.

Onay kutusunu işaretlenmemiş olarak bırakırsanız, çalışma alanınızın kendisiyle ilişkili bir sanal ağı olmayacaktır.

>[!IMPORTANT]
>Özel bağlantıları yalnızca yönetilen çalışma alanı sanal ağı olan bir çalışma alanında kullanabilirsiniz.

![Yönetilen çalışma alanı sanal ağını etkinleştir](./media/synapse-workspace-managed-vnet/enable-managed-vnet-1.png)


Azure portal **Genel Bakış ' ı** seçerek Azure SYNAPSE çalışma alanınızın yönetilen bir çalışma alanı sanal ağıyla ilişkili olup olmadığını kontrol edebilirsiniz.

![Azure portal çalışma alanına genel bakış](./media/synapse-workspace-managed-vnet/enable-managed-vnet-2.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE çalışma alanı](../quickstart-create-workspace.md) oluşturma

[Yönetilen özel uç noktalar](./synapse-workspace-managed-private-endpoints.md) hakkında daha fazla bilgi edinin

[Veri kaynaklarınızda yönetilen özel uç noktalar oluşturun](./how-to-create-managed-private-endpoints.md)