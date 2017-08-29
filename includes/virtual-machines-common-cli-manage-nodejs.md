Kaynak gruplarını kullanarak Azure kaynakları ve iş yükleri dağıtmak amacıyla Azure CLI’yı Resource Manager komutları ve şablonlarıyla kullanabilmeniz için önce Azure’lu bir hesaba sahip olmanız gerekir. Hesabınız yoksa [buradan ücretsiz Azure denemesi](https://azure.microsoft.com/pricing/free-trial/) edinebilirsiniz.

Zaten Azure CLI’yı yükleyip aboneliğinizi bağlamadıysanız [Azure CLI Yükleme](../articles/cli-install-nodejs.md) konusunu inceleyin, `azure config mode arm` ile modu `arm` olarak ayarlayın ve `azure login` komutuyla Azure’a bağlanın.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- Azure CLI 10: Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız (bu makale)
- [Azure CLI 2.0](../articles/virtual-machines/linux/cli-manage.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Azure CLI’daki Temel Azure Resource Manager komutları
Bu makalede, Azure aboneliğinizdeki kaynakları (ağırlıklı olarak VM’ler) yönetmek ve bunlarla etkileşim kurmak için Azure CLI ile kullanmak isteyebileceğiniz temel komutlar ele alınmıştır.  Belirli komut satırı anahtarları ve seçenekleri hakkında daha ayrıntılı yardım almak için `azure <command> <subcommand> --help` veya `azure help <command> <subcommand>` yazarak çevrimiçi komut yardımını ve seçeneklerini kullanabilirsiniz.

> [!NOTE]
> Bu örnekler, genel olarak Resource Manager’da VM dağıtımları için önerilmeyen şablon tabanlı işlemleri içermez. Bilgi edinmek için bkz. [Azure CLI’yı Azure Resource Manager ile kullanma](../articles/xplat-cli-azure-resource-manager.md) ve [Azure Resource Manager şablonlarını ve Azure CLI’yı kullanarak sanal makine dağıtma ve yönetme](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

| Görev | Resource Manager |
| --- | --- | --- |
| En temel VM’yi oluşturma |`azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(`azure vm image list` komutundan `image-urn` öğesini edinin. Örnekler için [bu makaleye](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın.) |
| Linux VM oluşturma |`azure  vm create [options] <resource-group> <name> <location> -y "Linux"` |
| Windows VM oluşturma |`azure  vm create [options] <resource-group> <name> <location> -y "Windows"` |
| VM'leri listeleme |`azure  vm list [options]` |
| VM hakkında bilgi alma |`azure  vm show [options] <resource_group> <name>` |
| VM başlatma |`azure vm start [options] <resource_group> <name>` |
| VM durdurma |`azure vm stop [options] <resource_group> <name>` |
| Bir VM’yi serbest bırakma |`azure vm deallocate [options] <resource-group> <name>` |
| Bir VM’yi yeniden başlatma |`azure vm restart [options] <resource_group> <name>` |
| VM silme |`azure vm delete [options] <resource_group> <name>` |
| VM yakalama |`azure vm capture [options] <resource_group> <name>` |
| Kullanıcı görüntüsünden VM oluşturma |`azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>` |
| Özelleştirilmiş diskten VM oluşturma |`azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>` |
| Bir VM’ye veri diski ekleme |`azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]` |
| Bir VM’den veri diski kaldırma |`azure  vm disk detach [options] <resource-group> <vm-name> <lun>` |
| Bir VM’ye genel bir uzantı ekleme |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Bir VM’ye VM Erişimi uzantısı ekleme |`azure vm reset-access [options] <resource-group> <name>` |
| Bir VM’ye Docker uzantısı ekleme |`azure  vm docker create [options] <resource-group> <name> <location> <os-type>` |
| VM uzantısı kaldırma |`azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>` |
| VM kaynaklarının kullanımını alma |`azure vm list-usage [options] <location>` |
| Tüm kullanılabilir VM boyutlarını alma |`azure vm sizes [options]` |

## <a name="next-steps"></a>Sonraki adımlar
* Temel VM yönetiminin ötesindeki CLI komutlarına ilişkin ek örnekler istiyorsanız bkz. [Azure CLI’yı Azure Resource Manager ile kullanma](../articles/virtual-machines/azure-cli-arm-commands.md).
