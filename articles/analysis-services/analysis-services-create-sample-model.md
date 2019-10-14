---
title: Öğretici-Azure Analysis Services sunucuya örnek model ekleme | Microsoft Docs
description: Bu öğretici derste, Azure Analysis Services'e örnek model eklemeyi öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: tutorial
ms.date: 03/13/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9a9721b05fbd478d108f06c36017ee444f721d28
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298682"
---
# <a name="tutorial-add-a-sample-model-from-the-portal"></a>Öğretici: Portaldan bir örnek model ekleme

Bu öğreticide, sunucunuza örnek Adventure Works tablolu model veritabanını ekleyeceksiniz. Bu örnek model, Adventure Works İnternet Satışları (1200) örnek veri modelinin eksiksiz bir sürümüdür. Örnek model, model yönetimini test etmek, araçlar ve istemci uygulamalarıyla bağlantı kurmak ve model verilerini sorgulamak için yararlıdır. Bu öğreticide aşağıdaki işlemler için [Azure Portal](https://portal.azure.com) ve [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) kullanılır: 

> [!div class="checklist"]
> * Sunucuya tamamlanmış bir örnek tablolu veri modeli ekleme 
> * SSMS ile modele bağlanma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Bir Azure Analysis Services sunucusu. Daha fazla bilgi için bkz. [Sunucu oluşturma - portal](analysis-services-create-server.md).
- Sunucu yöneticisi izinleri
- [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)


## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Portalda](https://portal.azure.com/) oturum açın.

## <a name="add-a-sample-model"></a>Örnek model ekleme

1. Sunucu **Genel Bakış**'ta **Yeni model**'e tıklayın.

    ![Örnek model oluşturma](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. **Yeni model** > **Veri kaynağı seç** alanında, **Örnek veriler**'in seçili olduğunu doğrulayın ve ardından **Ekle**'ye tıklayın.

    ![Örnek verileri seçme](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. **Genel Bakış**'ta, `adventureworks` örnek modelinin eklendiğini doğrulayın.

    ![Örnek verileri seçme](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek modeliniz önbellek kaynaklarını kullanır. Test için örnek modelinizi kullanmıyorsanız, bunu sunucunuzdan kaldırmalısınız.

Bu adımlarda, SSMS kullanarak sunucudan modelin nasıl silineceği açıklanır.

1. SSMS > **Nesne Gezgini**'nde **Bağlan** > **Analysis Services**'e tıklayın.

2. **Sunucuya Bağlan** alanına sunucu adını yapıştırın, ardından **Kimlik doğrulaması** alanında **Active Directory - MFA ile Evrensel desteği**'ni seçin, kullanıcı adınızı girin ve **Bağlan**'a tıklayın.

    ![Oturum aç](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. **Nesne Gezgini**’nde `adventureworks` örnek veritabanına sağ tıklayın ve sonra da **Sil**’e tıklayın.

    ![Örnek veritabanını silme](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Sonraki adımlar 

Bu öğreticide, sunucunuza temel bir örnek model eklemeyi öğrendiniz. Artık bir model veritabanınız olduğuna göre, SQL Server Management Studio'dan bu veritabanına bağlanabilir ve kullanıcı rollerini ekleyebilirsiniz. Daha fazla bilgi edinmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Sunucu yönetici ve kullanıcı rollerini yapılandırma](analysis-services-database-users.md)


