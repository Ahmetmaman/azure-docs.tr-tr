---
title: Uygulamaları en son şemaya geçir
description: Mantıksal uygulama iş akışı JSON tanımlarını en son Iş akışı Tanım Dili şema sürümüne geçirme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: 114b8b32d4abb1fd9b7e641625cd1b132470bafd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87281455"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Logic Apps 'i en son şema sürümüne geçirme

Mevcut mantıksal uygulamalarınızı en yeni şemaya taşımak için şu adımları izleyin: 

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

2. Mantıksal uygulamanızın menüsünde **genel bakış**' ı seçin. Araç çubuğunda **Şemayı Güncelleştir**' i seçin.

   > [!NOTE]
   > **Şemayı Güncelleştir**' i seçtiğinizde, Azure Logic Apps geçiş adımlarını otomatik olarak çalıştırır ve kod çıktısını sizin için sağlar. Bu çıktıyı, mantıksal uygulama tanımınızı güncelleştirmek için kullanabilirsiniz. Ancak, aşağıdaki **en iyi yöntemler** bölümünde açıklandığı gibi en iyi yöntemleri izlediğinizden emin olun.

   ![Şemayı Güncelleştir](./media/connectors-schema-migration/update-schema.png)

   Şemayı Güncelleştir sayfası görünür ve yeni şemadaki geliştirmeleri açıklayan bir belgenin bağlantısını gösterir.

## <a name="best-practices"></a>Önerilen uygulamalar

Aşağıda, mantıksal uygulamalarınızı en son şema sürümüne geçirmeye yönelik bazı en iyi yöntemler verilmiştir:

* Geçirilmiş betiği yeni bir mantıksal uygulamaya kopyalayın. Testinizi tamamlayana kadar eski sürümün üzerine yazmaz ve geçirilen uygulamanızın beklendiği gibi çalıştığından emin olun.

* Üretime koymadan **önce** mantıksal uygulamanızı test edin.

* Geçişi tamamladıktan sonra, mümkün olduğunda [yönetilen API 'leri](../connectors/apis-list.md) kullanmak için mantıksal uygulamalarınızı güncelleştirmeye başlayın. Örneğin, Dropbox v1 'yi kullandığınız her yerde Dropbox v2 'yi kullanmaya başlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamalarınızı el ile geçirmeyi](../logic-apps/logic-apps-schema-2016-04-01.md) öğrenin

