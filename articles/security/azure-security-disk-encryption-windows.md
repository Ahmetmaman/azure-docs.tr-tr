---
title: Azure Disk şifrelemesi Windows Iaas Vm'leri için etkinleştirme | Microsoft Docs
description: Bu makalede, Microsoft Azure Disk şifrelemesi için Windows Iaas Vm'leri etkinleştirme hakkında yönergeler sağlar.
services: security
documentationcenter: na
author: mestew
manager: MBaldwin
ms.assetid: 8b3905c8-844f-4ec7-ad95-b386e9843053
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2018
ms.author: mstewart
ms.openlocfilehash: 772ab272603d0f8e0badf899c439592b20b7c8a3
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391749"
---
#  <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Windows Iaas Vm'leri için Azure Disk şifrelemesini etkinleştirme 

Çok sayıda disk şifreleme senaryoları etkinleştirebilirsiniz ve adımları senaryoya göre değişiklik gösterebilir. Aşağıdaki bölümlerde, Windows Iaas Vm'leri için daha ayrıntılı senaryoları kapsar. Disk şifreleme kullanmadan önce [Azure Disk şifrelemesi önkoşulları](/articles/security/azure-security-disk-encryption-prerequisites.md) tamamlanması gerekir. 

Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya diskler şifrelenir önce yedekleyin. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için Set-AzureRmVMDiskEncryptionExtension cmdlet'ini kullanabilirsiniz. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../backup/backup-azure-vms-encryption.md) makalesi. 

>[!WARNING]
>Şifreleme parolaları bölgesel sınırlar arası yoksa emin olmak için Azure Disk şifrelemesi anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın. 


## <a name="enable-encryption-on-new-iaas-vms-created-from-the-marketplace"></a>Marketinden oluşturulan yeni Iaas Vm'leri şifrelemeyi etkinleştirin
Marketten bir Resource Manager şablonu kullanarak azure'da yeni Iaas Windows VM'de disk şifrelemeyi etkinleştirebilirsiniz. Yeni bir şifrelenmiş Windows Windows Server 2012 Galerisi görüntüsünü kullanarak VM şablonu oluşturur.

1. Üzerinde [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), tıklayın **azure'a Dağıt**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **satın alma** şifrelemesi etkin olduğu yeni bir Iaas VM dağıtmak için.

3. Şablonu dağıttıktan sonra tercih ettiğiniz yöntemi kullanarak VM şifreleme durumunu doğrulayın:
     - Azure CLI'yı kullanarak doğrulama [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutu. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
         ```

     - Azure PowerShell ile kimlik doğrulamasını [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet'i. 

         ```azurepowershell-interactive
         Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
         ```

     -  Sanal Makineyi seçin ve ardından tıklayarak **diskleri** altında **ayarları** şifreleme durumu Portalı'nda doğrulamak için başlık. Grafiğin altında **şifreleme**, etkin olup olmadığını göreceksiniz. 
           ![Azure portalı - Disk şifreleme etkin](./media/azure-security-disk-encryption/disk-encryption-fig2.png) Market senaryosundaki Azure AD İstemci Kimliğini kullanarak yeni sanal makineler için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama | 
| --- | --- |
| adminUserName | Sanal makine için yönetici kullanıcı adı. |
| adminPassword | Sanal makine için yönetici kullanıcı parolası. |
| newStorageAccountName | İşletim sistemi ve veri VHD'leri depolamak için depolama hesabının adıdır. |
| vmSize | VM boyutu. Şu anda yalnızca standart A, D ve G serisi desteklenmektedir. |
| virtualNetworkName | VM NIC ait olması gereken Vnet'in adı. |
| subnetName | VM NIC ait olmalıdır bir sanal ağ içindeki alt ağ adı. |
| Aadclientıd | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultURL | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının URL'si. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName).VaultURI` veya Azure CLI `az keyvault show --name "MySecureVault" --query properties.vaultUri` |
| KeyEncryptionKeyURL | (İsteğe bağlı) oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. </br> </br>KeyEncryptionKeyURL isteğe bağlı bir parametredir. Anahtar kasanızda veri şifreleme anahtarının (parola) daha fazla koruma için kendi KEK getirebilirsiniz. |
| keyVaultResourceGroup | Anahtar kasasının kaynak grubu. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |


## <a name="bkmk_RunningWinVM"></a> Var olan veya Iaas Windows Vm'leri çalıştırma şifrelemeyi etkinleştir
Bu senaryoda, bir şablon, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. Aşağıdaki bölümlerde, Azure Disk şifrelemesi etkinleştirme konusunda daha ayrıntılı olarak açıklanmaktadır. 

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzureRmVMDiskEncryptionExtension komut karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Var olan veya Azure PowerShell ile sanal makine çalıştırma şifrelemeyi etkinleştir 
Kullanım [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) azure'da çalıştırılan Iaas sanal makine şifrelemesini etkinleştirmek için cmdlet'i. PowerShell cmdlet'lerini kullanarak Azure Disk şifrelemesi ile şifrelemeyi etkinleştirme hakkında daha fazla bilgi için bkz. blog gönderileri [Azure PowerShell - bölüm 1 ile Azure Disk şifrelemesi keşfedin](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) ve [Azure Disk şifrelemesi keşfedin Azure PowerShell ile - bölüm 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifreleme:** aşağıdaki betiği değişkenlerinizi başlatır ve Set-AzureRmVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci gizli anahtarı zaten önkoşul olarak oluşturulmuş olmalıdır. My-AAD-client-ID ve My-AAD-CLIENT-secret MySecureRg, MySecureVM, MySecureVault değerleriniz ile değiştirin.
     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifreleme:** Azure Disk şifrelemesi var olan bir anahtar şifreleme etkinleştirirken oluşturulan disk şifreleme gizli dizileri sarmalamak için anahtar kasanızdaki belirtmenize olanak sağlar. Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = ‘MyExtraSecureVM’;
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskleri şifrelenmiş doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet'i. 
     ```azurepowershell-interactive
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Disk şifreleme devre dışı bırak:** şifreleme devre dışı bırakmak için [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet'i. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Var olan veya Azure CLI ile sanal makine çalıştırma şifrelemeyi etkinleştir
Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Diskleri şifrelenmiş doğrulayın:** Iaas VM şifreleme durumunu denetlemek için kullanmak [az vm şifreleme show](/cli/azure/vm/encryption#az-vm-encryption-show) komutu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Şifreleme devre dışı bırak:** şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Bu komut, yönetilen disk tabanlı Vm'leri karşı yapılan bir yedekleme ve bu parametre belirtildi kadar başarısız olacak. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 

### <a name="bkmk_RunningWinVMwRM"> </a>Resource Manager şablonu kullanma
Disk şifrelemesi kullanarak Azure'da Iaas Windows Vm'leri çalıştırma ya da varolan etkinleştirebilirsiniz [çalışan bir Windows VM şifreleme için Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).


1. Azure Hızlı Başlangıç şablonu üzerinde **azure'a Dağıt**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **satın alma** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştırmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` veya Azure CLI komutunu ' az keyvault list--resource-group "MySecureGroup" |Convertfrom-JSON'|
|  KeyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. |
| SequenceVersion | BitLocker işlemi dizisi sürümü. Bu sürüm numarası, aynı sanal makinede her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |


## <a name="bkmk_VHDpre"> </a>VHD ve şifreleme anahtarları müşteri şifrelenmiş oluşturulan yeni Iaas VM'ler
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde Resource Manager şablonu ve CLI komutları daha ayrıntılı olarak açıklanmaktadır. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra Set-AzureRmVMDiskEncryptionExtension cmdlet - skipVmBackup parametresini belirterek yönetilen disklerini şifrelemek için kullanılabilir. Set-AzureRmVMDiskEncryptionExtension komut karşı temel alan bir yönetilen disk Vm'leri yedekleme yapılan ve bu parametre belirtilen kadar başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 



### <a name="bkmk_VHDprePSH"> </a> Azure PowerShell ile önceden şifrelenmiş VHD'ler ile VM'lerin şifrele
Disk şifreli VHD'nizi PowerShell cmdlet'ini kullanarak şifreleyebilirsiniz [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples). Aşağıdaki örnek, bazı ortak parametreler sunar. 

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResouceGroupName "MySecureRG"
```


### <a name="bkmk_VHDpreRM"> </a>Iaas VM'ler bir Resource Manager şablonu ile önceden şifrelenmiş VHD'ler ile şifreleme 
Disk şifrelemesi kullanarak şifrelenmiş VHD'nizi etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Azure Hızlı Başlangıç şablonu üzerinde **azure'a Dağıt**.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **Oluştur** yeni Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Aşağıdaki tabloda, şifrelenmiş bir VHD için Resource Manager şablon parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| newStorageAccountName | Şifrelenmiş işletim sistemi VHD'SİNİN depolanacağı depolama hesabı adı. Bu depolama hesabı zaten aynı kaynak grubunda ve aynı konumdaki VM oluşturulmuş olmalıdır. |
| osVhdUri | Depolama hesabı işletim sistemi VHD URI'si. |
| osType | İşletim sistemi ürün türü (Windows/Linux). |
| virtualNetworkName | VM NIC ait olması gereken Vnet'in adı. Ad zaten aynı kaynak grubunda ve aynı konumdaki VM oluşturulmuş olmalıdır. |
| subnetName | VM NIC ait olması gereken bir VNet üzerinde alt ağın adı. |
| vmSize | VM boyutu. Şu anda yalnızca standart A, D ve G serisi desteklenmektedir. |
| keyVaultResourceID | Azure Resource Manager'daki anahtar kasası kaynağı tanımlayan ResourceId. PowerShell cmdlet'ini kullanarak elde edeceğinizi `(Get-AzureRmKeyVault -VaultName &lt;MyKeyVaultName&gt; -ResourceGroupName &lt;MyResourceGroupName&gt;).ResourceId` veya Azure CLI komutunu kullanarak `az keyvault show --name "MySecureVault" --query id`|
| keyVaultSecretUrl | Anahtar Kasası'nda ayarladığınız disk şifreleme anahtarı URL'si. |
| keyVaultKekUrl | Oluşturulan disk şifreleme anahtarını şifrelemek için anahtar şifreleme anahtarı URL'si. |
| vmName | Iaas VM adı. |

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin
Yapabilecekleriniz [Windows PowerShell kullanarak bir VM için yeni bir disk ekleyin](../virtual-machines/windows/attach-disk-ps.md), veya [Azure portalı üzerinden](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Windows VM'ler için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir dizisi sürümü belirtilmelidir. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. Bazı durumlarda yeni eklenen veri diski tarafından Azure Disk şifrelemesi uzantısını otomatik olarak şifrelenebilir. Bu meydana gelirse, Set-AzureRmVmDiskEncryptionExtension cmdlet'i yeni dizisi sürümü ile yeniden çalıştırmanızı öneririz.
 

-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifreleme:** aşağıdaki betiği değişkenlerinizi başlatır ve Set-AzureRmVMDiskEncryptionExtension cmdlet'ini çalıştırır. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci gizli anahtarı zaten önkoşul olarak oluşturulmuş olmalıdır. My-AAD-client-ID ve My-AAD-CLIENT-secret MySecureRg, MySecureVM, MySecureVault değerleriniz ile değiştirin. -VolumeType parametre veri diskleri ve işletim sistemi diski için ayarlanır. 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId  –SequenceVersion $sequenceVersion;
    ```
- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifreleme:** Azure Disk şifrelemesi var olan bir anahtar şifreleme etkinleştirirken oluşturulan disk şifreleme gizli dizileri sarmalamak için anahtar kasanızdaki belirtmenize olanak sağlar. Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır. Veri diskleri ve işletim sistemi diski haricindeki şifreliyorsanız - VolumeType parametresi eklemeniz gerekebilir. 

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Şifrelemeyi etkinleştirmek için komutu çalıştırdığınızda Azure CLI komutunu otomatik olarak sizin için yeni bir sıra sürüm sağlayacaktır. 
-  **İstemci gizli anahtarı kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **İstemci gizli anahtarı sarmalama KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault"--key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```


## <a name="enable-encryption-using-azure-ad-client-certificate-based-authentication"></a>Azure AD İstemci sertifikası tabanlı kimlik doğrulaması kullanarak şifrelemeyi etkinleştirir.
İstemci sertifikası kimlik doğrulaması ile veya kek'siz kullanabilirsiniz. Betikleri gerektiren [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md) getirildiğinden. PowerShell betikleri kullanmadan önce sertifikayı anahtar kasasına yüklenmiş ve VM'ye dağıttınız olmanız gerekir. KEK KEK çok kullanıyorsanız, zaten mevcut olmalıdır. Daha fazla bilgi için [sertifika tabanlı kimlik doğrulaması için Azure AD](azure-security-disk-encryption-prerequisites.md#bkmk_Cert) önkoşulları makalesindeki bölümüne bakın.


### <a name="enable-encryption-using-certificate-based-authentication-with-azure-powershell"></a>Azure PowerShell ile sertifika tabanlı kimlik doğrulaması kullanarak şifrelemeyi etkinleştir

```powershell
## Fill in 'MySecureRg', 'My-AAD-client-ID', 'MySecureVault, and ‘MySecureVM’.

$rgName = 'MySecureRg';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = ‘MySecureVM’;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

# Fill in the certificate path and the password so the thumbprint can be set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
```
  
### <a name="enable-encryption-using-certificate-based-authentication-and-a-kek-with-azure-powershell"></a>Sertifika tabanlı kimlik doğrulaması ve bir KEK ile Azure PowerShell kullanarak şifrelemeyi etkinleştir

```powershell
# Fill in 'MySecureRg', 'My-AAD-client-ID', 'MySecureVault,, 'MySecureVM’, and "KEKName.

$rgName = 'MySecureRg';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$keyEncryptionKeyName ='KEKName';
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

## Fill in the certificate path and the password so the thumbprint can be read and set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint and a KEK

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

## <a name="disable-encryption"></a>Şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLI kullanarak şifreleme devre dışı bırakabilir veya bir Resource Manager şablonu ile. 

- **Azure PowerShell ile disk şifrelemesini devre dışı bırak:** şifreleme devre dışı bırakmak için [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet'i. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

- **Azure CLI ile şifreleme devre dışı bırak:** şifreleme devre dışı bırakmak için [az vm şifreleme devre dışı](/cli/azure/vm/encryption#az-vm-encryption-disable) komutu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Resource Manager şablonu ile şifreleme devre dışı bırakın:** 

    1. Tıklayın **azure'a Dağıt** gelen [Windows VM çalıştırma disk şifreleme devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) şablonu.
    2. Abonelik, kaynak grubu, konum, VM, yasal koşulları ve Sözleşmesi'ni seçin.
    3.  Tıklayın **satın alma** çalışan bir Windows VM'de disk şifreleme devre dışı bırakmak için. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Linux için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux.md)