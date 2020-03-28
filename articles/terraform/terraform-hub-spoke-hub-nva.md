---
title: Öğretici - Terraform'u kullanarak Azure'da hub sanal ağ cihazı oluşturun
description: Öğretici, diğer tüm ağlar arasında ortak bir bağlantı noktası görevi gören Hub VNet'in oluşturulmasını uygular
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 28ccb89d237cbe21dd0433da5f7fbb32883f6550
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74159255"
---
# <a name="tutorial-create-a-hub-virtual-network-appliance-in-azure-using-terraform"></a>Öğretici: Terraform'u kullanarak Azure'da hub sanal ağ cihazı oluşturun

**VPN aygıtı,** şirket içi ağa harici bağlantı sağlayan bir aygıttır. VPN aygıtı bir donanım aygıtı veya yazılım çözümü olabilir. Yazılım çözümüne örnek olarak, Windows Server 2012'deki Yönlendirme ve Uzaktan Erişim Hizmeti (RRAS) örnek olarak sunulmaktadır. VPN cihazları hakkında daha fazla bilgi için [Siteden Siteye VPN Ağ Geçidi bağlantıları için VPN aygıtları hakkında](/azure/vpn-gateway/vpn-gateway-about-vpn-devices)bilgi alabiliyorum.

Azure, seçilecek çok çeşitli ağ sanal cihazlarını destekler. Bu öğretici için bir Ubuntu görüntüsü kullanılır. Azure'da desteklenen çok çeşitli aygıt çözümleri hakkında daha fazla bilgi edinmek için [Ağ Cihazları ana sayfasına](https://azure.microsoft.com/solutions/network-appliances/)bakın.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hub VNet'i hub konuşan topolojide uygulamak için HCL'yi (HashiCorp Dili) kullanın
> * Terraform'u kullanarak cihaz görevi gören Hub Network Sanal Makinesi'ni oluşturun
> * CustomScript uzantılarını kullanarak yolları etkinleştirmek için Terraform'u kullanma
> * Hub ve Spoke ağ geçidi rota tabloları oluşturmak için Terraform'u kullanma

## <a name="prerequisites"></a>Ön koşullar

1. [Azure'da Terraform ile bir hub ve kollu karma ağ topolojisi oluşturun.](./terraform-hub-spoke-introduction.md)
1. [Azure'da Terraform ile şirket içi sanal ağ oluşturun.](./terraform-hub-spoke-on-prem.md)
1. [Azure'da Terraform ile bir hub sanal ağı oluşturun.](./terraform-hub-spoke-hub-network.md)

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma

1. [Azure portalına](https://portal.azure.com)göz atın.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-hub-network-appliance"></a>Hub ağ cihazını bildirme

Şirket içinde sanal ağ bildiren Terraform yapılandırma dosyasını oluşturun.

1. Bulut Kabuğu'nda, '' `hub-nva.tf`adlı yeni bir dosya oluşturun.

    ```bash
    code hub-nva.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:
    
    ```hcl
    locals {
      prefix-hub-nva         = "hub-nva"
      hub-nva-location       = "CentralUS"
      hub-nva-resource-group = "hub-nva-rg"
    }

    resource "azurerm_resource_group" "hub-nva-rg" {
      name     = "${local.prefix-hub-nva}-rg"
      location = local.hub-nva-location

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_network_interface" "hub-nva-nic" {
      name                 = "${local.prefix-hub-nva}-nic"
      location             = azurerm_resource_group.hub-nva-rg.location
      resource_group_name  = azurerm_resource_group.hub-nva-rg.name
      enable_ip_forwarding = true

      ip_configuration {
        name                          = local.prefix-hub-nva
        subnet_id                     = azurerm_subnet.hub-dmz.id
        private_ip_address_allocation = "Static"
        private_ip_address            = "10.0.0.36"
      }

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_virtual_machine" "hub-nva-vm" {
      name                  = "${local.prefix-hub-nva}-vm"
      location              = azurerm_resource_group.hub-nva-rg.location
      resource_group_name   = azurerm_resource_group.hub-nva-rg.name
      network_interface_ids = [azurerm_network_interface.hub-nva-nic.id]
      vm_size               = var.vmsize

      storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04-LTS"
        version   = "latest"
      }

      storage_os_disk {
        name              = "myosdisk1"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Standard_LRS"
      }

      os_profile {
        computer_name  = "${local.prefix-hub-nva}-vm"
        admin_username = var.username
        admin_password = var.password
      }

      os_profile_linux_config {
        disable_password_authentication = false
      }

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_virtual_machine_extension" "enable-routes" {
      name                 = "enable-iptables-routes"
      location             = azurerm_resource_group.hub-nva-rg.location
      resource_group_name  = azurerm_resource_group.hub-nva-rg.name
      virtual_machine_name = azurerm_virtual_machine.hub-nva-vm.name
      publisher            = "Microsoft.Azure.Extensions"
      type                 = "CustomScript"
      type_handler_version = "2.0"

      settings = <<SETTINGS
        {
            "fileUris": [
            "https://raw.githubusercontent.com/mspnp/reference-architectures/master/scripts/linux/enable-ip-forwarding.sh"
            ],
            "commandToExecute": "bash enable-ip-forwarding.sh"
        }
    SETTINGS

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_route_table" "hub-gateway-rt" {
      name                          = "hub-gateway-rt"
      location                      = azurerm_resource_group.hub-nva-rg.location
      resource_group_name           = azurerm_resource_group.hub-nva-rg.name
      disable_bgp_route_propagation = false

      route {
        name           = "toHub"
        address_prefix = "10.0.0.0/16"
        next_hop_type  = "VnetLocal"
      }

      route {
        name                   = "toSpoke1"
        address_prefix         = "10.1.0.0/16"
        next_hop_type          = "VirtualAppliance"
        next_hop_in_ip_address = "10.0.0.36"
      }

      route {
        name                   = "toSpoke2"
        address_prefix         = "10.2.0.0/16"
        next_hop_type          = "VirtualAppliance"
        next_hop_in_ip_address = "10.0.0.36"
      }

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_subnet_route_table_association" "hub-gateway-rt-hub-vnet-gateway-subnet" {
      subnet_id      = azurerm_subnet.hub-gateway-subnet.id
      route_table_id = azurerm_route_table.hub-gateway-rt.id
      depends_on = ["azurerm_subnet.hub-gateway-subnet"]
    }

    resource "azurerm_route_table" "spoke1-rt" {
      name                          = "spoke1-rt"
      location                      = azurerm_resource_group.hub-nva-rg.location
      resource_group_name           = azurerm_resource_group.hub-nva-rg.name
      disable_bgp_route_propagation = false

      route {
        name                   = "toSpoke2"
        address_prefix         = "10.2.0.0/16"
        next_hop_type          = "VirtualAppliance"
        next_hop_in_ip_address = "10.0.0.36"
      }

      route {
        name           = "default"
        address_prefix = "0.0.0.0/0"
        next_hop_type  = "vnetlocal"
      }

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_subnet_route_table_association" "spoke1-rt-spoke1-vnet-mgmt" {
      subnet_id      = azurerm_subnet.spoke1-mgmt.id
      route_table_id = azurerm_route_table.spoke1-rt.id
      depends_on = ["azurerm_subnet.spoke1-mgmt"]
    }

    resource "azurerm_subnet_route_table_association" "spoke1-rt-spoke1-vnet-workload" {
      subnet_id      = azurerm_subnet.spoke1-workload.id
      route_table_id = azurerm_route_table.spoke1-rt.id
      depends_on = ["azurerm_subnet.spoke1-workload"]
    }

    resource "azurerm_route_table" "spoke2-rt" {
      name                          = "spoke2-rt"
      location                      = azurerm_resource_group.hub-nva-rg.location
      resource_group_name           = azurerm_resource_group.hub-nva-rg.name
      disable_bgp_route_propagation = false

      route {
        name                   = "toSpoke1"
        address_prefix         = "10.1.0.0/16"
        next_hop_in_ip_address = "10.0.0.36"
        next_hop_type          = "VirtualAppliance"
      }

      route {
        name           = "default"
        address_prefix = "0.0.0.0/0"
        next_hop_type  = "vnetlocal"
      }

      tags {
        environment = local.prefix-hub-nva
      }
    }

    resource "azurerm_subnet_route_table_association" "spoke2-rt-spoke2-vnet-mgmt" {
      subnet_id      = azurerm_subnet.spoke2-mgmt.id
      route_table_id = azurerm_route_table.spoke2-rt.id
      depends_on = ["azurerm_subnet.spoke2-mgmt"]
    }

    resource "azurerm_subnet_route_table_association" "spoke2-rt-spoke2-vnet-workload" {
      subnet_id      = azurerm_subnet.spoke2-workload.id
      route_table_id = azurerm_route_table.spoke2-rt.id
      depends_on = ["azurerm_subnet.spoke2-workload"]
    }

    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Terraform ile konuşlu sanal ağlar oluşturun](./terraform-hub-spoke-spoke-network.md)