---
title: Windows VM 'de Uzak Masaüstü Hizmetleri veya yönetici parolasını sıfırlama | Microsoft Docs
description: Azure portal veya Azure PowerShell kullanarak bir Windows sanal makinesinde hesap parolasını veya Uzak Masaüstü Hizmetleri sıfırlamayı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/25/2019
ms.author: genli
ms.openlocfilehash: 80bcac36271d86334e11cb3cd161cd96777e6fe5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87832459"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Windows VM 'de Uzak Masaüstü Hizmetleri veya yönetici parolasını sıfırlama
Bir Windows sanal makinesine (VM) bağlanamıyorsanız, yerel yönetici parolanızı sıfırlayabilir veya Uzak Masaüstü Hizmetleri yapılandırmayı sıfırlayabilirsiniz (Windows etki alanı denetleyicilerinde desteklenmez). Parolayı sıfırlamak için Azure portalı veya Azure PowerShell'deki VM Erişimi uzantısını kullanın. VM'de oturum açtıktan sonra yerel yönetici parolasını sıfırlayın.  
PowerShell kullanıyorsanız, [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/) olduğundan ve Azure aboneliğinizde oturum açmış olduğunuzdan emin olun. Ayrıca [klasik dağıtım modeliyle oluşturulmuş olan VM'lerde bu adımları da gerçekleştirebilirsiniz](/previous-versions/azure/virtual-machines/windows/classic/reset-rdp).

Uzak Masaüstü Hizmetleri ve kimlik bilgileri sıfırlamasını gerçekleştirmek için aşağıdaki yöntemleri kullanabilirsiniz:

- [Azure portal kullanarak sıfırlayın](#reset-by-using-the-azure-portal)

- [VMAccess uzantısını ve PowerShell 'i kullanarak sıfırlama](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Azure portal kullanarak sıfırlayın

İlk olarak, [Azure Portal](https://portal.azure.com) oturum açın ve ardından sol menüdeki **sanal makineler** ' i seçin. 

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlayın**

1. Windows VM 'nizi seçin ve ardından **destek + sorun giderme**altında **Parolayı Sıfırla** ' yı seçin. **Parolayı Sıfırla** penceresi görüntülenir.

2. **Parolayı Sıfırla**' yı seçin, bir Kullanıcı adı ve parola girin ve ardından **Güncelleştir**' i seçin. 

3. Sanal makinenize yeniden bağlanmayı deneyin.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlama**

Bu işlem, sanal makinede Uzak Masaüstü hizmeti 'ni etkinleştirir ve varsayılan RDP bağlantı noktası 3389 için bir güvenlik duvarı kuralı oluşturur.

1. Windows VM 'nizi seçin ve ardından **destek + sorun giderme**altında **Parolayı Sıfırla** ' yı seçin. **Parolayı Sıfırla** penceresi görüntülenir. 

2. **Yalnızca yapılandırmayı Sıfırla** ' yı seçin ve ardından **Güncelleştir**' i seçin. 

3. Sanal makinenize yeniden bağlanmayı deneyin.

## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>VMAccess uzantısını ve PowerShell 'i kullanarak sıfırlama

İlk olarak, [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/) olduğundan ve [Connect-azaccount](/powershell/module/az.accounts/connect-azaccount) cmdlet 'ini kullanarak Azure aboneliğinizde oturum açmış olduğunuzdan emin olun.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlayın**

- Yönetici parolasını veya Kullanıcı adını [set-Azvmaccessextenma](/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet 'i ile sıfırlayın. `typeHandlerVersion`1 sürümü kullanım dışı olduğundan, ayar 2,0 veya daha büyük olmalıdır. 

    ```powershell
    $SubID = "<SUBSCRIPTION ID>" 
    $RgName = "<RESOURCE GROUP NAME>" 
    $VmName = "<VM NAME>" 
    $Location = "<LOCATION>" 
 
    Connect-AzAccount 
    Select-AzSubscription -SubscriptionId $SubID 
    Set-AzVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
    ```

    > [!NOTE] 
    > VM 'nizin geçerli yerel yönetici hesabından farklı bir ad girerseniz, VMAccess uzantısı bu ada sahip bir yerel yönetici hesabı ekler ve belirtilen parolanızı bu hesaba atar. SANAL makinenizin yerel yönetici hesabı varsa, VMAccess uzantısı parolayı sıfırlar. Hesap devre dışı bırakılmışsa, VMAccess uzantısı bunu etkinleştirecektir.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlama**

1. [Set-Azvmaccessextenma](/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet 'ı ile sanal makinenize uzaktan erişimi sıfırlayın. Aşağıdaki örnek, `myVMAccess` kaynak grubunda ADLı VM 'de adlı erişim uzantısını sıfırlar `myVM` `myResourceGroup` :

    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
    ```

    > [!TIP]
    > Herhangi bir noktada, VM yalnızca tek bir VM erişim aracısına sahip olabilir. VM erişim Aracısı özelliklerini ayarlamak için `-ForceRerun` seçeneğini kullanın. `-ForceRerun`' I kullandığınızda, önceki komutlarda kullanmış OLABILECEĞINIZ VM erişim Aracısı için aynı adı kullandığınızdan emin olun.

1. Hala sanal makinenize uzaktan bağlanamıyorsanız, bkz. [Windows tabanlı Azure sanal makinesine yönelik uzak masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Windows etki alanı denetleyicisiyle olan bağlantıyı kaybederseniz, bir etki alanı denetleyicisi yedeğinden geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM erişimi uzantısı yanıt vermezse ve parolayı sıfırlayamazsa, [yerel Windows parolasını çevrimdışı olarak sıfırlayabilirsiniz](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem daha gelişmiş bir yöntemdir ve sorunlu sanal makınenın sanal sabit diskini başka bir sanal makineye bağlamanız gerekir. Önce Bu makalede belgelenen adımları izleyin ve çevrimdışı parola sıfırlama yöntemini yalnızca bu adımlar çalışmazsa deneyin.

- [Azure VM uzantıları ve özellikleri hakkında bilgi edinin](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

- [RDP veya SSH ile bir Azure sanal makinesine bağlanın](/previous-versions/azure/dn535788(v=azure.100)).

- [Windows tabanlı bir Azure sanal makinesine yönelik uzak masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).