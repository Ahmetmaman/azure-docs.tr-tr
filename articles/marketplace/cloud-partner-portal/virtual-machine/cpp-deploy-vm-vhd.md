---
title: Azure Market vhd'lerinizden VM dağıtma | Microsoft Docs
description: Azure ile dağıtılan bir VHD'den VM kaydedileceği açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 9157ce7f8f16bc60a6d5c16fa992a5402cf2d7ad
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53190739"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Vhd'lerinizden VM dağıtma

Bu bölümde, bir sanal makine (VM) dağıtmak nasıl bir Azure ile dağıtılan sanal sabit diskten (VHD) açıklanmaktadır.  Gereken araçları ve bunları bir kullanıcı VM görüntüsü oluşturun ve ardından PowerShell betiklerini kullanarak Azure'a dağıtmak için nasıl kullanılacağını listeler.

Sanal sabit disklerinizin (VHD) yükledikten sonra — genelleştirilmiş işletim sistemi VHD'si ve sıfır veya daha fazla veri diski VHD'leri — Azure depolama hesabınızın, bunları bir kullanıcı VM görüntüsü olarak kaydedebilirsiniz. Ardından bu görüntüyü test edebilirsiniz. İşletim sistemi VHD'si genelleştirilmiş olduğundan VHD URL'sini sağlayarak VM'yi doğrudan dağıtamazsınız.

VM görüntüleri hakkında daha fazla bilgi edinmek için şu blog gönderilerine bakın:

- [VM görüntüsü](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM görüntüsü PowerShell 'Nasıl yapılır'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)


## <a name="prerequisite-install-the-necessary-tools"></a>Önkoşul: gerekli araçları yükleyin

Zaten yapmadıysanız, Azure PowerShell ve Azure CLI aşağıdaki yönergeleri kullanarak yükleyin:

- [PowerShellGet ile Windows üzerindeki Azure PowerShell'i yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)
- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="deployment-steps"></a>Dağıtım adımları

Oluşturma ve bir kullanıcı VM görüntüsü dağıtmak için aşağıdaki adımları kullanın:

1. Yakalama ve görüntüyü Genelleştirme kapsar kullanıcı VM görüntüsü oluşturun. 
2. Sertifikaları oluşturun ve bunları yeni bir Azure Key Vault'ta depolar. VM güvenli bir WinRM bağlantı kurmak için bir sertifika gereklidir.  Bir Azure Resource Manager şablonu ve bir Azure PowerShell Betiği sağlanır. 
3. Sağlanan şablon ve betik kullanarak bir kullanıcı VM görüntüsünden VM dağıtın.

Sanal makinenizin dağıtıldıktan sonra hazır olduğunuz [VM görüntünüzü sertifika](./cpp-certify-vm.md).

2.  Tıklayın **yeni** araması **şablon dağıtımı**, ardından **düzenleyicide kendi şablonunuzu oluşturun**.  <br/>
  ![Azure portalında VHD'yi dağıtım şablonu oluşturma](./media/publishvm_021.png)

3. Bunu kopyalayıp [JSON şablonunu](./cpp-deploy-json-template.md) Düzenleyicisi ve tıklatın **Kaydet**. <br/>
  ![Azure portalında VHD'yi dağıtım şablonunu Kaydet](./media/publishvm_022.png)

4. Görüntülenen için parametre değerlerini sağlayın **özel dağıtım** özellik sayfaları.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parametre**              |   **Açıklama**                                                            |
   |  -------------              |   ---------------                                                            |
   | Kullanıcı depolama hesabı adı   | Genelleştirilmiş VHD bulunduğu depolama hesabı adı                    |
   | Kullanıcı depolama kapsayıcısı adı | Genelleştirilmiş VHD bulunduğu kapsayıcı adı                          |
   | Genel IP için DNS adı      | Genel IP DNS adı                                                           |
   | Yönetici kullanıcı adı             | Yeni VM için yönetici hesabının kullanıcı adı                                  |
   | Yönetici Parolası              | Yeni VM için yönetici hesabının parolası                                  |
   | İşletim Sistemi Türü                     | VM işletim sistemi: `Windows` \| `Linux`                                    |
   | Abonelik Kimliği             | Seçili abonelik tanımlayıcısı                                      |
   | Konum                    | Dağıtım coğrafi konum                                        |
   | VM Boyutu                     | [Azure VM boyutu](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), örneğin `Standard_A2` |
   | Genel IP adresi adı      | Genel IP adresi adı                                               |
   | VM Adı                     | Yeni bir VM adı                                                           |
   | Sanal ağ adı        | Sanal makine tarafından kullanılan sanal ağ adı                                   |
   | NIC adı                    | Sanal ağ çalıştıran ağ arabirim kartı adı               |
   | VHD URL'si                     | İşletim sistemi diski VHD URL'si tamamlayın                                                     |
   |  |  |
            
5. Bu değerleri sizin sağlamanız sonra tıklayın **satın alma**. 

Azure dağıtım başlayacak: Belirtilen depolama hesabı yolu içinde belirtilen yönetilmeyen VHD ile yeni bir VM oluşturur.  Tıklayarak ilerleme durumunu Azure portalında izleyebilirsiniz **sanal makineler** portalının sol taraftaki.  VM oluşturulduğunda durumu gelen değiştirecek `Starting` için `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Powershell'den VM dağıtma

Yeni oluşturulan genelleştirilmiş VM görüntüsünden büyük bir VM'yi dağıtmak için aşağıdaki cmdlet'leri kullanın.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```


## <a name="next-steps"></a>Sonraki adımlar

Sonra [bir kullanıcı VM görüntüsü oluşturma](cpp-create-user-image.md) çözümünüz için.

