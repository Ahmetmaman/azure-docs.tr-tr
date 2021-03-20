---
title: Bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs
description: Azure portal kullanarak bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturmayı öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: cac812a9c38fc1dedfd31659a626b122f9527e63
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87289398"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasından özel görüntü oluşturma

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, Azure portal kullanarak bir VHD dosyasından özel görüntü oluşturma işleminde size yol gösterir:

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.

1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  

1. Laboratuvarın ana bölmesinde **yapılandırma ve ilkeler**' i seçin. 

1. **Yapılandırma ve ilkeler** bölmesinde **özel görüntüler**' i seçin.

1. **Özel görüntüler** bölmesinde **+ Ekle**' yi seçin.

    ![Özel görüntü ekle](./media/devtest-lab-create-template/add-custom-image.png)

1. Özel görüntünün adını girin. Bu ad, bir VM oluştururken temel görüntüler listesinde görüntülenir.

1. Özel görüntünün açıklamasını girin. Bu açıklama, bir VM oluştururken temel görüntüler listesinde görüntülenir.

1. **Işletim sistemi türü** Için, **Windows** veya **Linux**' u seçin.

    - **Windows**' u seçerseniz makinede *Sysprep* 'in çalıştırılıp çalıştırılmadığını onay kutusunu kullanarak belirtin. 
    - **Linux**' u seçerseniz, makinede *kaldırma* işlemi yapılıp çalıştırılmadığını onay kutusunu kullanarak belirtin. 

1. Açılan menüden bir **VHD** seçin. Bu, yeni özel görüntüyü oluşturmak için kullanılacak VHD 'dir. Gerekirse, **PowerShell kullanarak BIR VHD 'Yi karşıya yüklemeyi** seçin.

1. Ayrıca, özel görüntü oluşturmak için kullanılan görüntü lisanslı bir görüntü değilse (Microsoft tarafından yayımlanan) bir plan adı, teklif planı ve yayımcı planı da girebilirsiniz.

   - **Plan adı:** Bu özel görüntünün oluşturulduğu Market görüntüsünün (SKU) adını girin 
   - **Teklif planı:** Bu özel görüntünün oluşturulduğu Market görüntüsünün ürününü (teklifini) girin 
   - **Yayımcıyı planla:** Bu özel görüntünün oluşturulduğu Market görüntüsünün yayımcısını girin

   > [!NOTE]
   > Özel bir görüntü oluşturmak için kullandığınız görüntü lisanslı bir görüntü **değilse** , bu alanlar boştur ve tercih ederseniz doldurulabilir. Görüntü, lisanslı bir görüntü **ise** , alanlar plan bilgileri ile otomatik olarak doldurulur. Bu durumda değiştirmeye çalışırsanız bir uyarı iletisi görüntülenir.
   >
   >

1. Özel görüntüyü oluşturmak için **Tamam ' ı** seçin.

Birkaç dakika sonra, özel görüntü oluşturulur ve laboratuvarın depolama hesabı içinde depolanır. Laboratuvar kullanıcısı yeni bir VM oluşturmak istediğinde, görüntü temel görüntüler listesinde kullanılabilir.

![Temel görüntüler listesinde kullanılabilen özel görüntü](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller mi?](./devtest-lab-faq.md#blog-post)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızda VM ekleme](./devtest-lab-add-vm.md)
