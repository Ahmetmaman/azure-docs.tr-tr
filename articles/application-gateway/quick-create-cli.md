---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure CLI’sini kullanmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: quickstart
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 99c3975c6ab2c7a20dfbab519dae575a2a61465f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32160367"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI

Azure Application Gateway ile bağlantı noktalarına dinleyiciler atayarak ve arka uç havuzuna kaynaklar ekleyerek uygulama web trafiğinizi belirli kaynaklara yönlendirebilirsiniz.

Bu hızlı başlangıç, arka uç havuzunda iki sanal makine bulunan bir uygulama ağ geçidini hızla oluşturmak üzere Azure CLI’sini kullanmayı göstermektedir. Ardından doğru bir şekilde çalışıp çalışmadığından emin olmak için test edersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Daima kaynak grubunda kaynaklar oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

Diğer kaynaklarla iletişim kurmak için uygulama ağ geçidine yönelik bir sanal makine oluşturmanız gerekir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de sanal makineler içindir. 

[az network vnet create](/cli/azure/vnet#az_vnet_create) komutunu kullanarak sanal ağı ve alt ağı oluşturun. [az network public-ip create](/cli/azure/public-ip#az_public_ip_create) komutunu kullanarak bir genel IP adresi oluşturun.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucular olarak kullanılacak iki sanal makine oluşturacaksınız. 

### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere NGINX de yükleyin. cloud-init yapılandırma dosyasını kullanarak NGINX’i yükleyin ve Linux sanal makinesinde 'Merhaba Dünya' Node.js uygulaması çalıştırın. 

Geçerli kabuğunuzda cloud-init.txt adlı bir dosya oluşturup aşağıdaki yapılandırmayı kopyalayıp kabuğa yapıştırın. Başta birinci satır olmak üzere cloud-init dosyasının tamamını doğru bir şekilde kopyaladığınızdan emin olun:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

[az network nic oluşturma](/cli/azure/network/nic#az_network_nic_create) ile ağ arabirimlerini oluşturun. [az vm create](/cli/azure/vm#az_vm_create) komutunu kullanarak sanal makineleri oluşturun.

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

[az network application-gateway create](/cli/azure/application-gateway#az_application_gateway_create) komutunu kullanarak bir uygulama ağ geçidi oluşturun. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Ağ arabirimlerinin özel IP adresleri, uygulama ağ geçidinin arka uç havuzunda sunucu olarak eklenir.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

Uygulama ağ geçidinin oluşturulması 30 dakikaya kadar sürebilir. Uygulama ağ geçidi oluşturulduktan sonra şu özellikleri görürsünüz:

- *appGatewayBackendPool*: Bir uygulama ağ geçidi en az bir arka uç adres havuzuna sahip olmalıdır.
- *appGatewayBackendHttpSettings*: İletişim için 80 numaralı bağlantı noktasının ve HTTP protokolünün kullanıldığını belirtir.
- *appGatewayHttpListener*: *appGatewayBackendPool* ile ilişkili varsayılan dinleyicidir.
- *appGatewayFrontendIP*: *appGatewayHttpListener*’a *myAGPublicIPAddress*’i atar.
- *kural 1*: *appGatewayHttpListener* ile ilişkili varsayılan yönlendirme kuralıdır.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidini oluşturmak için NGINX’in yüklenmesi gerekli değildir ancak NGINX’i bu hızlı başlangıçta uygulama ağ geçidinin başarılı bir şekilde oluşturulup oluşturulmadığını doğrulamak için yüklediniz. Uygulama ağ geçidinin genel IP adresini almak için [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) komutunu kullanın. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
``` 

![Uygulama ağ geçidini test etme](./media/quick-create-cli/application-gateway-nginxtest.png)

Tarayıcıyı yenilediğinizde diğer VM’nin adının göründüğünü görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlk olarak uygulama ağ geçidiyle oluşturulan kaynakları keşfedin ve ardından artık gerekmediğinde kaynak grubu, uygulama ağ geçidi ve tüm ilgili kaynakları kaldırmak için [az group delete](/cli/azure/group#az_group_delete) komutunu kullanabilirsiniz.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```
 
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)

