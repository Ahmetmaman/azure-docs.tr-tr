---
title: Azure DevTest Labs 'da Windows sanal makinelerinize bağlanma
description: Windows sanal makinenize bir laboratuvarda nasıl bağlanacağınızı öğrenin (Azure DevTest Labs)
ms.topic: how-to
ms.date: 07/17/2020
ms.openlocfilehash: e1e786daa396548030976159d1b150caa4b24396
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86540717"
---
# <a name="connect-to-a-windows-vm-in-your-lab-azure-devtest-labs"></a>Laboratuvarınızda bir Windows sanal makinesine bağlanma (Azure DevTest Labs)
Bu makalede, laboratuvarınızda bir Windows VM 'ye nasıl bağlanabilmeniz gösterilmektedir. 

## <a name="connect-to-a-windows-vm"></a>Windows VM'ye bağlanma
1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Arama çubuğunda **DevTest Labs**' i arayın ve seçin. 

    :::image type="content" source="./media/connect-windows-virtual-machine/search-select.png" alt-text="DevTest Labs 'i arayın ve seçin":::    
1. Laboratuvarlar listesinden **laboratuvarınızı**seçin.

    :::image type="content" source="./media/connect-windows-virtual-machine/select-lab.png" alt-text="DevTest Labs 'i arayın ve seçin":::            
1. Laboratuvarınızın giriş sayfasında, **sanal makinelerim** LISTESINDEN Windows VM 'nizi seçin. 

    :::image type="content" source="./media/connect-windows-virtual-machine/select-windows-vm.png" alt-text="DevTest Labs 'i arayın ve seçin":::                
1. VM 'nizin **sanal makine** sayfasında, araç çubuğunda **Bağlan** ' ı seçin. VM durdurulmuşsa, VM 'yi başlatmak için önce **Başlat** ' ı seçin.

    :::image type="content" source="./media/connect-windows-virtual-machine/select-connect.png" alt-text="DevTest Labs 'i arayın ve seçin":::                    
1. Edge tarayıcısını kullanıyorsanız, tarayıcının altındaki **INDIRILEN RDP dosyasının** bağlantısını görürsünüz. 

    :::image type="content" source="./media/connect-windows-virtual-machine/rdp-download.png" alt-text="DevTest Labs 'i arayın ve seçin":::                        
1. RDP dosyasını açın ve VM oluştururken yazdığınız VM kimlik bilgilerinizi girin. Şimdi Windows VM 'ye bağlanmanız gerekir. 

## <a name="next-steps"></a>Sonraki Adımlar
[Nasıl yapılır: Linux VM 'ye bağlanma](connect-linux-virtual-machine.md)
