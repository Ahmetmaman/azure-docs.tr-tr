---
title: Azure Instance Metadata Service
description: Azure Instance Metadata Service hakkında bilgi edinin ve BT 'nin şu anda çalışan sanal makine örnekleri hakkında nasıl bilgi sağladığını öğrenin.
services: virtual-machines
author: KumariSupriya
manager: paulmey
ms.service: virtual-machines
ms.subservice: monitoring
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/29/2020
ms.author: sukumari
ms.reviewer: azmetadatadev
ms.openlocfilehash: ea11e2f5f8d89381723011686de9e22639997c01
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90974145"
---
# <a name="azure-instance-metadata-service-imds"></a>Azure Instance Metadata Service (ıMDS)

Azure Instance Metadata Service (IMDS), çalışmakta olan sanal makine örnekleri hakkında bilgi sağlar ve sanal makinelerinizi yönetmek ve yapılandırmak için kullanılabilir.
Bu bilgiler SKU, depolama, ağ yapılandırması ve yaklaşan bakım olaylarını içerir. Kullanılabilir verilerin tüm listesi için bkz. [metadata API 'leri](#metadata-apis).
Instance Metadata Service, sanal makine ve sanal makine ölçek kümesi örnekleri çalıştırmak için kullanılabilir. Tüm API 'ler, [Azure Resource Manager](/rest/api/resources/)kullanılarak oluşturulan/yönetilen VM 'leri destekler. Yalnızca Atsınanan ve ağ uç noktaları, klasik (ARM olmayan) VM 'Leri destekler ve yalnızca sınırlı bir ölçüde bu şekilde attest eder.

Azure 'un ıMDS, iyi bilinen yönlendirilemeyen IP adresinde () bulunan bir REST uç noktasıdır `169.254.169.254` ve yalnızca VM 'nin içinden erişilebilir. VM ve ıDS arasındaki iletişim, Konağı hiçbir şekilde bırakmıyor.
ISE 'yi sorgularken ve ile aynı işlem yaparken, HTTP istemcilerinizin VM içindeki Web proxy 'lerini atlaması en iyi uygulamadır `169.254.169.254` [`168.63.129.16`](../../virtual-network/what-is-ip-address-168-63-129-16.md) .

## <a name="security"></a>Güvenlik

Instance Metadata Service uç noktasına yalnızca, yönlendirilemeyen bir IP adresi üzerinde çalışan sanal makine örneği içinden erişilebilir. Ayrıca, üst bilgiyle bir istek `X-Forwarded-For` hizmet tarafından reddedilir.
Ayrıca `Metadata: true` , gerçek isteğin istenmeden yeniden yönlendirmenin bir parçası olmamasını sağlamak için istekler bir üst bilgi içermelidir.

> [!IMPORTANT]
> Instance Metadata Service hassas veriler için bir kanal değil. Uç noktası, sanal makine üzerindeki tüm işlemlere açıktır. Bu hizmet aracılığıyla sunulan bilgiler, sanal makine içinde çalışan tüm uygulamalara paylaşılan bilgiler olarak düşünülmelidir.

## <a name="usage"></a>Kullanım

### <a name="accessing-azure-instance-metadata-service"></a>Azure Instance Metadata Service erişme

Instance Metadata Service erişmek için [Azure Resource Manager](/rest/api/resources/) veya [Azure Portal](https://portal.azure.com)bir VM oluşturun ve aşağıdaki örnekleri izleyin.
[Azure örnek meta verileri örneklerinde](https://github.com/microsoft/azureimds), IMDS 'yi sorgulama hakkında daha fazla örnek bulabilirsiniz.

Aşağıda, bir örneğin tüm meta verilerini almaya yönelik örnek kod, belirli veri kaynağına erişmek için, bkz. [metadata API](#metadata-apis) bölümü. 

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2020-06-01"
```

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
    "compute": {
        "azEnvironment": "AZUREPUBLICCLOUD",
        "isHostCompatibilityLayerVm": "true",
        "location": "westus",
        "name": "examplevmname",
        "offer": "Windows",
        "osType": "linux",
        "placementGroupId": "f67c14ab-e92c-408c-ae2d-da15866ec79a",
        "plan": {
            "name": "planName",
            "product": "planProduct",
            "publisher": "planPublisher"
        },
        "platformFaultDomain": "36",
        "platformUpdateDomain": "42",
        "publicKeys": [{
                "keyData": "ssh-rsa 0",
                "path": "/home/user/.ssh/authorized_keys0"
            },
            {
                "keyData": "ssh-rsa 1",
                "path": "/home/user/.ssh/authorized_keys1"
            }
        ],
        "publisher": "RDFE-Test-Microsoft-Windows-Server-Group",
        "resourceGroupName": "macikgo-test-may-23",
        "resourceId": "/subscriptions/8d10da13-8125-4ba9-a717-bf7490507b3d/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/virtualMachines/examplevmname",
        "securityProfile": {
            "secureBootEnabled": "true",
            "virtualTpmEnabled": "false"
        },
        "sku": "Windows-Server-2012-R2-Datacenter",
        "storageProfile": {
            "dataDisks": [{
                "caching": "None",
                "createOption": "Empty",
                "diskSizeGB": "1024",
                "image": {
                    "uri": ""
                },
                "lun": "0",
                "managedDisk": {
                    "id": "/subscriptions/8d10da13-8125-4ba9-a717-bf7490507b3d/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
                    "storageAccountType": "Standard_LRS"
                },
                "name": "exampledatadiskname",
                "vhd": {
                    "uri": ""
                },
                "writeAcceleratorEnabled": "false"
            }],
            "imageReference": {
                "id": "",
                "offer": "UbuntuServer",
                "publisher": "Canonical",
                "sku": "16.04.0-LTS",
                "version": "latest"
            },
            "osDisk": {
                "caching": "ReadWrite",
                "createOption": "FromImage",
                "diskSizeGB": "30",
                "diffDiskSettings": {
                    "option": "Local"
                },
                "encryptionSettings": {
                    "enabled": "false"
                },
                "image": {
                    "uri": ""
                },
                "managedDisk": {
                    "id": "/subscriptions/8d10da13-8125-4ba9-a717-bf7490507b3d/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
                    "storageAccountType": "Standard_LRS"
                },
                "name": "exampleosdiskname",
                "osType": "Linux",
                "vhd": {
                    "uri": ""
                },
                "writeAcceleratorEnabled": "false"
            }
        },
        "subscriptionId": "8d10da13-8125-4ba9-a717-bf7490507b3d",
        "tags": "baz:bash;foo:bar",
        "version": "15.05.22",
        "vmId": "02aab8a4-74ef-476e-8182-f6d2ba4166a6",
        "vmScaleSetName": "crpteste9vflji9",
        "vmSize": "Standard_A3",
        "zone": ""
    }
}
```

### <a name="data-output"></a>Veri çıkışı

Varsayılan olarak, Instance Metadata Service JSON biçimindeki () verileri döndürür `Content-Type: application/json` . Ancak, bazı API 'Ler isteniyorsa verileri farklı biçimlerde döndürebiliyor.
Aşağıdaki tablo, API 'Lerin destekleyebileceği diğer veri biçimlerinin bir başvurusudur.

API | Varsayılan veri biçimi | Diğer biçimler
--------|---------------------|--------------
/attested | json | yok
/identity | json | yok
/Instance | json | metin
/scheduledevents | json | yok

Varsayılan olmayan bir yanıt biçimine erişmek için istenen biçimi istekte bir sorgu dizesi parametresi olarak belirtin. Örneğin:

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> /Metadata/Instance içindeki yaprak düğümleri için `format=json` çalışmıyor. `format=text`Varsayılan biçim JSON olduğundan bu sorguların açıkça belirtilmesi gerekir.

### <a name="versioning"></a>Sürüm Oluşturma

Instance Metadata Service sürümlenmiş ve HTTP isteğindeki API sürümünün belirtilmesi zorunludur.

Desteklenen API sürümleri şunlardır: 
- 2017-03-01
- 2017-04-02
- 2017-08-01 
- 2017-10-01
- 2017-12-01 
- 2018-02-01
- 2018-04-02
- 2018-10-01
- 2019-02-01
- 2019-03-11
- 2019-04-30
- 2019-06-01
- 2019-06-04
- 2019-08-01
- 2019-08-15
- 2019-11-01
- 2020-06-01

Yeni sürüm yayınlandığında, tüm bölgelere dağıtım biraz zaman alabilir.

Daha yeni sürümler eklendikçe, betiklerinizin belirli veri biçimlerinde bağımlılıkları varsa, daha eski sürümlere uyumluluk için yine de erişilebilir.

Sürüm belirtilmediğinde, en yeni Desteklenen sürümlerin listesiyle birlikte bir hata döndürülür.

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek, sürüm belirtilmediğinde hata koşulunu gösterir, yanıt okunabilirlik için oldukça yazdırılır.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance"
```

**Response**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

## <a name="metadata-apis"></a>Meta veri API 'Leri

Metadata Service, farklı veri kaynaklarını temsil eden birden çok API içerir.

API | Açıklama | Sunulan sürüm
----|-------------|-----------------------
/attested | Bkz. [Atsınanan veriler](#attested-data) | 2018-10-01
/identity | Bkz. [erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
/Instance | Bkz. [örnek API 'si](#instance-api) | 2017-04-02
/scheduledevents | Bkz. [zamanlanan olaylar](scheduled-events.md) | 2017-08-01

## <a name="instance-api"></a>Örnek API 'SI

Örnek API 'SI, sanal makine, ağ ve depolama dahil olmak üzere VM örnekleri için önemli meta verileri kullanıma sunar. Aşağıdaki kategorilere örnek/işlem aracılığıyla erişilebilir:

Veriler | Açıklama | Sunulan sürüm
-----|-------------|-----------------------
azEnvironment | VM 'nin çalıştığı Azure ortamı | 2018-10-01
customData | Bu özellik şu anda devre dışı. Bu belge kullanılabilir hale geldiğinde güncelleştirilecek | 2019-02-01
isHostCompatibilityLayerVm | VM 'nin konak uyumluluk katmanında çalışıp çalışmadığına göre tanımlar | 2020-06-01
location | VM 'nin çalıştığı Azure bölgesi | 2017-04-02
name | VM adı | 2017-04-02
teklif | VM görüntüsü için teklif bilgileri ve yalnızca Azure görüntü Galerisi 'nden dağıtılan görüntülerde bulunur | 2017-04-02
osType | Linux veya Windows | 2017-04-02
Placementgroupıd | Sanal makine ölçek kümesinin [yerleştirme grubu](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) | 2017-08-01
plan | Bir Azure Market görüntüsü ise VM için ad, ürün ve yayımcı içeren [plan planlayın](/rest/api/compute/virtualmachines/createorupdate#plan) | 2018-04-02
platformUpdateDomain |  VM 'nin çalıştığı [etki alanını güncelleştirme](manage-availability.md) | 2017-04-02
platformFaultDomain | VM 'nin çalıştığı [hata etki alanı](manage-availability.md) | 2017-04-02
sağlayıcısını | VM sağlayıcısı | 2018-10-01
publicKeys | VM ve yollara atanan [ortak anahtarların koleksiyonu](/rest/api/compute/virtualmachines/createorupdate#sshpublickey) | 2018-04-02
yayımcı | VM görüntüsünün yayımcısı | 2017-04-02
resourceGroupName | Sanal makineniz için [kaynak grubu](../../azure-resource-manager/management/overview.md) | 2017-08-01
resourceId | Kaynağın [tam](/rest/api/resources/resources/getbyid) kimliği | 2019-03-11
isteyin | VM görüntüsü için belirli SKU | 2017-04-02
securityProfile. secureBootEnabled | SANAL makinede UEFı güvenli önyükleme özelliğinin etkinleştirilip etkinleştirilmediğini belirler | 2020-06-01
securityProfile. Virtualaltpmenabled | Sanal Güvenilir Platform Modülü (TPM) VM 'de etkin olup olmadığını belirler | 2020-06-01
storageProfile | [Depolama profilini](#storage-metadata) gör | 2019-06-01
subscriptionId | Sanal makine için Azure aboneliği | 2017-08-01
etiketler | Sanal makineniz için [Etiketler](../../azure-resource-manager/management/tag-resources.md)  | 2017-08-01
tagsList | Daha kolay programlı ayrıştırma için JSON dizisi olarak biçimlendirilen Etiketler  | 2019-06-04
sürüm | VM görüntüsünün sürümü | 2017-04-02
Kimliği | VM için [benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) | 2017-04-02
vmScaleSetName | Sanal makine ölçek kümesinin [sanal makine ölçek kümesi adı](../../virtual-machine-scale-sets/overview.md) | 2017-12-01
vmSize | [VM boyutu](../sizes.md) | 2017-04-02
bölge | Sanal makinenizin [kullanılabilirlik bölgesi](../../availability-zones/az-overview.md) | 2017-12-01

### <a name="sample-1-tracking-vm-running-on-azure"></a>Örnek 1: Azure 'da çalışan Izleme sanal makinesi

Hizmet sağlayıcı olarak, yazılımınızı çalıştıran VM 'lerin sayısını izlemeniz veya VM 'nin benzersizliği izlemesi gereken aracılardan sahip olmanız gerekebilir. Bir VM 'nin benzersiz KIMLIĞINI alabilmesi için `vmId` Instance Metadata Service alanı kullanın.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Response**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="sample-2-placement-of-containers-data-partitions-based-faultupdate-domain"></a>Örnek 2: kapsayıcıların yerleştirilmesi, veri bölümleri tabanlı hata/güncelleştirme etki alanı

Bazı senaryolarda, farklı veri çoğaltmalarının yerleştirilmesi önemli öneme sahiptir. Örneğin, bir [Orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) aracılığıyla yapılan [çoğaltma yerleşimi](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) veya KAPSAYıCı yerleştirmesi, `platformFaultDomain` ve `platformUpdateDomain` VM 'nin üzerinde çalışıp çalışmadığını bilmeniz gerekebilir.
Bu kararları vermek için örneklerin [kullanılabilirlik alanları](../../availability-zones/az-overview.md) de kullanabilirsiniz.
Bu verileri doğrudan Instance Metadata Service aracılığıyla sorgulayabilirsiniz.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Response**

```text
0
```

### <a name="sample-3-getting-more-information-about-the-vm-during-support-case"></a>Örnek 3: destek talebi sırasında VM hakkında daha fazla bilgi edinme

Hizmet sağlayıcı olarak, VM hakkında daha fazla bilgi edinmek istediğiniz bir destek çağrısı alabilirsiniz. Müşterinin işlem meta verilerinin paylaşılmasını sormak, destek uzmanı 'nın Azure 'daki sanal makine türü hakkında bilgi sahibi olmak için temel bilgiler sağlayabilir.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute?api-version=2019-06-01"
```

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
        "dataDisks": [
          {
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
              "uri": ""
            },
            "lun": "0",
            "managedDisk": {
              "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
              "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
              "uri": ""
            },
            "writeAcceleratorEnabled": "false"
          }
        ],
        "imageReference": {
          "id": "",
          "offer": "UbuntuServer",
          "publisher": "Canonical",
          "sku": "16.04.0-LTS",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": "30",
          "diffDiskSettings": {
            "option": "Local"
          },
          "encryptionSettings": {
            "enabled": "false"
          },
          "image": {
            "uri": ""
          },
          "managedDisk": {
            "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
            "storageAccountType": "Standard_LRS"
          },
          "name": "exampleosdiskname",
          "osType": "Linux",
          "vhd": {
            "uri": ""
          },
          "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
}
```

### <a name="sample-4-getting-azure-environment-where-the-vm-is-running"></a>Örnek 4: VM 'nin çalıştığı Azure ortamını alma

Azure 'da [Azure Kamu](https://azure.microsoft.com/overview/clouds/government/)gibi çeşitli bağımsız bulutlar vardır. Bazen bazı çalışma zamanı kararları almak için Azure ortamına ihtiyacınız vardır. Aşağıdaki örnekte, bu davranışı nasıl sağlayabileceğiniz gösterilmektedir.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Response**

```text
AzurePublicCloud
```

Azure ortamının bulutu ve değerleri aşağıda listelenmiştir.

 Bulut   | Azure ortamı
---------|-----------------
[Tüm genel kullanıma açık Azure bölgeleri](https://azure.microsoft.com/regions/)     | AzurePublicCloud
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | AzureUSGovernmentCloud
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | AzureChinaCloud
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | AzureGermanCloud

## <a name="network-metadata"></a>Ağ meta verileri 

Ağ meta verileri, örnek API 'sinin bir parçasıdır. Aşağıdaki ağ kategorileri, örnek/ağ uç noktası üzerinden kullanılabilir.

Veriler | Açıklama | Sunulan sürüm
-----|-------------|-----------------------
IPv4/Privateıpaddress | VM 'nin yerel IPv4 adresi | 2017-04-02
IPv4/Publicıpaddress | VM 'nin genel IPv4 adresi | 2017-04-02
alt ağ/adres | VM 'nin alt ağ adresi | 2017-04-02
alt ağ/ön ek | Alt ağ ön eki, örnek 24 | 2017-04-02
IPv6/IPAddress | VM 'nin yerel IPv6 adresi | 2017-04-02
macAddress | VM MAC adresi | 2017-04-02

> [!NOTE]
> Tüm API yanıtları JSON dizeleridir. Aşağıdaki örnek yanıtların hepsi okunabilirlik için oldukça yazdırılır.

#### <a name="sample-1-retrieving-network-information"></a>Örnek 1: ağ bilgilerini alma

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="sample-2-retrieving-public-ip-address"></a>Örnek 2: genel IP adresi alınıyor

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

## <a name="storage-metadata"></a>Depolama meta verileri

Depolama meta verileri örnek/işlem/storageProfile uç noktası altında örnek API 'sinin bir parçasıdır.
VM ile ilişkili depolama disklerinin ayrıntılarını sağlar. 

Bir sanal makinenin depolama profili üç kategoriye ayrılmıştır: görüntü başvurusu, işletim sistemi diski ve veri diskleri.

Görüntü başvurusu nesnesi, işletim sistemi görüntüsüyle ilgili aşağıdaki bilgileri içerir:

Veriler    | Açıklama
--------|-----------------
kimlik      | Kaynak kimliği
teklif   | Platform veya Market görüntüsü teklifi
yayımcı | Görüntü yayımcısı
isteyin     | Görüntü SKU 'su
sürüm | Platform veya Market görüntüsünün sürümü

İşletim sistemi diski nesnesi, VM tarafından kullanılan işletim sistemi diski hakkında aşağıdaki bilgileri içerir:

Veriler    | Açıklama
--------|-----------------
önbelleği | Önbelleğe alma gereksinimleri
createOption | VM 'nin nasıl oluşturulduğu hakkında bilgi
diffDiskSettings | Kısa ömürlü disk ayarları
diskSizeGB | Diskin GB cinsinden boyutu
image   | Kaynak Kullanıcı görüntüsü sanal sabit diski
'yi     | Diskin mantıksal birim numarası
managedDisk | Yönetilen disk parametreleri
name    | Disk adı
sahip     | Sanal sabit disk
Writeivatorenabled | Diskte writeAccelerator etkin olup olmadığı

Veri diskleri dizisi, VM 'ye bağlı veri disklerinin bir listesini içerir. Her veri diski nesnesi şu bilgileri içerir:

Veriler    | Açıklama
--------|-----------------
önbelleği | Önbelleğe alma gereksinimleri
createOption | VM 'nin nasıl oluşturulduğu hakkında bilgi
diffDiskSettings | Kısa ömürlü disk ayarları
diskSizeGB | Diskin GB cinsinden boyutu
encryptionSettings | Disk için şifreleme ayarları
image   | Kaynak Kullanıcı görüntüsü sanal sabit diski
managedDisk | Yönetilen disk parametreleri
name    | Disk adı
osType  | Diske dahil edilen işletim sisteminin türü
sahip     | Sanal sabit disk
Writeivatorenabled | Diskte writeAccelerator etkin olup olmadığı

Aşağıdaki örnek, sanal makinenin depolama bilgilerinin nasıl sorgulanalınacağını gösterir.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/storageProfile?api-version=2019-06-01"
```

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
    "dataDisks": [
      {
        "caching": "None",
        "createOption": "Empty",
        "diskSizeGB": "1024",
        "image": {
          "uri": ""
        },
        "lun": "0",
        "managedDisk": {
          "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
          "storageAccountType": "Standard_LRS"
        },
        "name": "exampledatadiskname",
        "vhd": {
          "uri": ""
        },
        "writeAcceleratorEnabled": "false"
      }
    ],
    "imageReference": {
      "id": "",
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04.0-LTS",
      "version": "latest"
    },
    "osDisk": {
      "caching": "ReadWrite",
      "createOption": "FromImage",
      "diskSizeGB": "30",
      "diffDiskSettings": {
        "option": "Local"
      },
      "encryptionSettings": {
        "enabled": "false"
      },
      "image": {
        "uri": ""
      },
      "managedDisk": {
        "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
        "storageAccountType": "Standard_LRS"
      },
      "name": "exampleosdiskname",
      "osType": "Linux",
      "vhd": {
        "uri": ""
      },
      "writeAcceleratorEnabled": "false"
    }
}
```

## <a name="vm-tags"></a>VM etiketleri

VM etiketleri örnek/işlem/etiket uç noktası altına örnek API 'sini içerir.
Etiketler, bunları bir taksonomi halinde mantıksal olarak düzenlemek için Azure sanal makinenize uygulanmış olabilir. Bir VM 'ye atanan Etiketler aşağıdaki istek kullanılarak alınabilir.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Response**

```text
Department:IT;Environment:Test;Role:WebRole
```

`tags`Alan, etiketleri noktalı virgülle ayrılmış bir dizedir. Bu çıktı, etiketlerde noktalı virgül kullanılıyorsa bir sorun olabilir. Etiketleri programlı olarak ayıklamak için bir Ayrıştırıcı yazılmışsa, alanına güvenmelisiniz `tagsList` . `tagsList`Alan, sınırlandırıcı olmayan BIR JSON dizisidir ve sonuç olarak daha kolay ayrıştırılabilir.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/tagsList?api-version=2019-06-04"
```

**Response**

```json
[
  {
    "name": "Department",
    "value": "IT"
  },
  {
    "name": "Environment",
    "value": "Test"
  },
  {
    "name": "Role",
    "value": "WebRole"
  }
]
```

## <a name="attested-data"></a>Atsınanan veriler

Instance Metadata Service tarafından sunulan senaryonun bir kısmı, belirtilen verilerin Azure 'dan geldiğini garanti etmek sağlamaktır. Market görüntülerinin Azure 'da çalıştığından emin olmak için bu bilgilerin bir parçasını imzalıyoruz.

### <a name="sample-1-getting-attested-data"></a>Örnek 1: atsınanan verileri alma

> [!NOTE]
> Tüm API yanıtları JSON dizeleridir. Aşağıdaki örnek yanıtlar okunabilirlik için oldukça yazdırılır.

**İstek**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"
```

Api sürümü zorunlu bir alandır. Desteklenen API sürümleri için [kullanım bölümüne](#usage) bakın.
Nonce, isteğe bağlı 10 basamaklı bir dizedir. Sağlanmazsa, ıDS geçerli UTC zaman damgasını yerinde döndürür.

> [!NOTE]
> IDS 'nin önbelleğe alma mekanizması nedeniyle, önceden önbelleğe alınmış bir nonce değeri döndürülebilir.

**Response**

> [!NOTE]
> Yanıt bir JSON dizesidir. Aşağıdaki örnek yanıt, okunabilirlik için oldukça yazdırılır.

```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

İmza blobu, belgenin [PKCS7](https://aka.ms/pkcs7) imzalı bir sürümüdür. SANAL makineye özgü bazı ayrıntılarla birlikte imzalama için kullanılan sertifikayı içerir. ARM VM 'Leri için buna VMID, SKU, nonce, SubscriptionID, belgenin oluşturulması ve süre sonu için zaman damgası ve görüntüyle ilgili plan bilgileri dahildir. Plan bilgileri yalnızca Azure Market görüntüleri için doldurulur. Klasik (ARM olmayan) VM 'Ler için yalnızca VMID 'nin doldurulması garanti edilir. Sertifika yanıttan ayıklanabilir ve yanıtın geçerli olduğunu ve Azure 'dan geldiğini doğrulamak için kullanılır.
Belge aşağıdaki alanları içerir:

Veriler | Açıklama
-----|------------
nonce | İsteğe bağlı olarak istekle birlikte sağlanmış bir dize. Bir nonce sağlanmazsa, geçerli UTC zaman damgası kullanılır
plan | [Azure Marketi görüntü planı](/rest/api/compute/virtualmachines/createorupdate#plan). Plan kimliğini (adı), ürün görüntüsünü veya teklifi (ürün) ve yayımcı kimliğini (yayımcı) içerir.
zaman damgası/createdOn | İmzalanmış belgenin oluşturulduğu zamana ilişkin UTC zaman damgası
zaman damgası/expiresOn | İmzalanan belgenin süresi dolduğu zaman için UTC zaman damgası
Kimliği |  VM için [benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)
subscriptionId | Üzerinde sunulan sanal makine için Azure aboneliği `2019-04-30`
isteyin | VM görüntüsü için özel SKU, ' de tanıtılan `2019-11-01`

> [!NOTE]
> Klasik (ARM olmayan) VM 'Ler için yalnızca VMID 'nin doldurulması garanti edilir.

### <a name="sample-2-validating-that-the-vm-is-running-in-azure"></a>Örnek 2: VM 'nin Azure 'da çalıştığı doğrulanıyor

Market satıcıları, yazılımlarının yalnızca Azure 'da çalışmak üzere lisanslanmasını sağlamak istiyor. Başka biri VHD 'yi şirket içine kopyalarsa, bu durumda bu, bunu tespit etme yeteneğine sahip olmalıdır. Instance Metadata Service çağırarak Market satıcıları yalnızca Azure 'dan gelen yanıtı garanti eden imzalı verileri alabilir.

> [!NOTE]
> JQ 'ın yüklenmesini gerektirir.

**İstek**

```bash
# Get the signature
curl --silent -H Metadata:True --noproxy "*" "http://169.254.169.254/metadata/attested/document?api-version=2019-04-30" | jq -r '.["signature"]' > signature
# Decode the signature
base64 -d signature > decodedsignature
# Get PKCS7 format
openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
# Get Public key out of pkc7
openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
# Get the intermediate certificate
curl -s -o intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
openssl x509 -inform der -in intermediate.cer -out intermediate.pem
# Verify the contents
openssl smime -verify -in sign.pk7 -inform pem -noverify
```

**Response**

```json
Verification successful
{
  "nonce": "20181128-001617",
  "plan":
    {
      "name": "",
      "product": "",
      "publisher": ""
    },
  "timeStamp":
    {
      "createdOn": "11/28/18 00:16:17 -0000",
      "expiresOn": "11/28/18 06:16:17 -0000"
    },
  "vmId": "d3e0e374-fda6-4649-bbc9-7f20dc379f34",
  "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
  "sku": "RS3-Pro"
}
```

İmzanın Microsoft Azure olduğunu doğrulayın ve hata için sertifika zincirini denetleyin.

```bash
# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain, for Azure China 21Vianet the intermediate certificate will be from DigiCert Global Root CA
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

> [!NOTE]
> IDS 'nin önbelleğe alma mekanizması nedeniyle, önceden önbelleğe alınmış bir nonce değeri döndürülebilir.

İmzalı belgedeki nonce, ilk istekte bir nonce parametresi sağladıysanız karşılaştırılabilir.

> [!NOTE]
> Genel bulut ve bağımsız bulutu için sertifika farklı olacaktır.

Bulut | Sertifika
------|------------
[Tüm genel kullanıma açık Azure bölgeleri](https://azure.microsoft.com/regions/) | *. metadata.azure.com
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)          | *. metadata.azure.us
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)     | *. metadata.azure.cn
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                | *. metadata.microsoftazure.de

> [!NOTE]
> İmzalanmak üzere kullanılan sertifika etrafında bilinen bir sorun var. Sertifikalar, genel bulut için tam olarak eşleşen bir eşleşmeyebilir `metadata.azure.com` . Bu nedenle, sertifika doğrulaması herhangi bir alt etki alanından ortak bir ada izin verilmelidir `.metadata.azure.com` .

Doğrulama sırasında ağ kısıtlamalarına bağlı olarak ara sertifikanın indirilebileceği durumlarda ara sertifika sabitlenebilir. Ancak, Azure Standart PKI uygulamasına göre sertifikaların üzerine alınır. Geçiş gerçekleştiğinde sabitlenmiş sertifikaların güncellenmesi gerekir. Ara sertifikayı güncelleştirme değişikliği planlandığı zaman, Azure bloguna güncelleştirilecek ve Azure müşterileri bilgilendirilir. Ara Sertifikalar [burada](https://www.microsoft.com/pki/mscorp/cps/default.htm)bulunabilir. Her bir bölgenin ara sertifikaları farklı olabilir.

> [!NOTE]
> Azure Çin 21Vianet için ara sertifika, balya yerine DigiCert genel kök CA 'dan olacaktır.
Ayrıca, kök zincir yetkilisinin bir parçası olarak Azure Çin ara sertifikalarını sabitlediğiniz takdirde, ara sertifikaların güncellenmesi gerekecektir.

## <a name="managed-identity-via-metadata-service"></a>Metadata Service aracılığıyla yönetilen kimlik

Bir sistem tarafından atanan yönetilen kimlik VM 'de etkinleştirilebilir veya bir veya daha fazla kullanıcı atanmış yönetilen kimlik VM 'ye atanabilir.
Daha sonra, Yönetilen kimlikler için belirteçler Instance Metadata Service istenebilir. Bu belirteçler, Azure Key Vault gibi diğer Azure hizmetleriyle kimlik doğrulamak için kullanılabilir.

Bu özelliği etkinleştirmeye yönelik ayrıntılı adımlar için bkz. [erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="scheduled-events-via-metadata-service"></a>Metadata Service üzerinden Zamanlanan Olaylar
Zamanlanmış olayların durumunu meta veri hizmeti aracılığıyla elde edebilirsiniz, ardından Kullanıcı bu olaylar üzerinde yürütülecek bir eylem kümesi belirtebilir.  Ayrıntılar için [zamanlanan olaylar](scheduled-events.md) bakın. 

## <a name="regional-availability"></a>Bölgesel Kullanılabilirlik

Hizmet, tüm Azure bulutlarında **genel kullanıma sunulmuştur** .

## <a name="sample-code-in-different-languages"></a>Farklı dillerdeki örnek kod

VM içinde farklı diller kullanarak meta veri hizmeti çağırma örnekleri:

Dil      | Örnek
--------------|----------------
Bash          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
C#            | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Başlayın            | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Java          | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
NodeJS        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
Perl          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
PowerShell    | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Puppet        | https://github.com/keirans/azuremetadata
Python        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
Ruby          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb

## <a name="error-and-debugging"></a>Hata ve hata ayıklama

Veri öğesi bulunamadı veya hatalı oluşturulmuş bir istek varsa, Instance Metadata Service standart HTTP hataları döndürür. Örneğin:

HTTP durum kodu | Neden
----------------|-------
200 TAMAM |
400 Hatalı İstek | `Metadata: true`Yaprak düğüm sorgulanırken başlık veya eksik parametre eksik `format=json`
404 Bulunamadı | İstenen öğe yok
405 yöntemine Izin verilmiyor | Yalnızca `GET` istekler destekleniyor
410 gitti | En fazla 70 saniye boyunca bir süre sonra yeniden deneyin
429 Çok Fazla İstek Var | API Şu anda saniyede en çok 5 sorgu destekliyor
500 hizmet hatası     | Bir süre sonra yeniden dene

### <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

1. Hatayı alıyorum `400 Bad Request, Required metadata header not specified` . Bu ne anlama geliyor?
   * Instance Metadata Service, üstbilginin `Metadata: true` isteğe geçirilmesini istiyor. Bu üstbilginin REST çağrısına geçirilmesi Instance Metadata Service erişim sağlar.
1. Neden Sanal makinem için işlem bilgilerini alamıyorum?
   * Şu anda Instance Metadata Service yalnızca Azure Resource Manager oluşturulan örnekleri destekliyor. Gelecekte, bulut hizmeti VM 'Leri için destek eklenebilir.
1. Sanal makinmi Azure Resource Manager bir sırada oluşturdum. İşlem meta veri bilgilerini neden göremiyorum?
   * Sep 2016 ' den sonra oluşturulan tüm VM 'Ler için, işlem meta verilerini görmeye başlamak üzere bir [etiket](../../azure-resource-manager/management/tag-resources.md) ekleyin. Daha eski VM 'Ler için (2016. sürümden önce oluşturulur), meta verileri yenilemek için VM örnekleri için uzantı veya veri diskleri ekleyin/kaldırın.
1. Yeni sürüm için doldurulmuş tüm verileri görmüyorum
   * Sep 2016 ' den sonra oluşturulan tüm VM 'Ler için, işlem meta verilerini görmeye başlamak üzere bir [etiket](../../azure-resource-manager/management/tag-resources.md) ekleyin. Daha eski VM 'Ler için (2016. sürümden önce oluşturulur), meta verileri yenilemek için VM örnekleri için uzantı veya veri diskleri ekleyin/kaldırın.
1. Hatayı neden alıyorum `500 Internal Server Error` `410 Resource Gone` ?
   * İsteğinizi üstel geri dönüş sistemine veya [geçici hata işlemede](/azure/architecture/best-practices/transient-faults)açıklanan diğer yöntemlere göre yeniden deneyin. Sorun devam ederse, VM için Azure portal bir destek sorunu oluşturun.
1. Bu, sanal makine ölçek kümesi örnekleri için mi çalışıyor?
   * Evet meta veri hizmeti ölçek kümesi örnekleri için kullanılabilir.
1. Sanal makine ölçek kümelerinde etiketlerimi güncelleştirdim ancak tek örnekli VM 'lerden farklı örneklerde görünmüyor musunuz?
   * Şu anda ölçek kümeleri için olan Etiketler yalnızca bir yeniden başlatma, yeniden görüntü veya örneğe disk değişikliği üzerinde VM 'yi gösterir.
1. Hizmete yönelik çağrımın isteği zaman aşımına uğradı mı?
   * Meta veri çağrılarının, VM 'nin birincil ağ kartına atanan birincil IP adresinden yapılması gerekir. Ayrıca, rotalarınızı değiştirdiğiniz durumda, sanal makinenizin yerel yönlendirme tablosunda 169.254.169.254/32 adresi için bir yol olmalıdır.
   * <details>
        <summary>Yönlendirme tablonuz doğrulanıyor</summary>

        1. Yerel yönlendirme tablonuzu gibi bir komutla dökümünü alın `netstat -r` ve ıMDS girişini (ör.) arayın:
            ```console
            ~$ netstat -r
            Kernel IP routing table
            Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
            default         _gateway        0.0.0.0         UG        0 0          0 eth0
            168.63.129.16   _gateway        255.255.255.255 UGH       0 0          0 eth0
            169.254.169.254 _gateway        255.255.255.255 UGH       0 0          0 eth0
            172.16.69.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
            ```
        1. İçin bir yolun mevcut olduğunu doğrulayın `169.254.169.254` ve ilgili ağ arabirimine (ör.) göz önünde bulunduğunu unutmayın `eth0` .
        1. Yönlendirme tablosunda karşılık gelen arabirim için arabirim yapılandırması dökümünü alın (yapılandırma dosyasının tam adı değişebilir)
            ```console
            ~$ cat /etc/netplan/50-cloud-init.yaml
            network:
            ethernets:
                eth0:
                    dhcp4: true
                    dhcp4-overrides:
                        route-metric: 100
                    dhcp6: false
                    match:
                        macaddress: 00:0d:3a:e4:c7:2e
                    set-name: eth0
            version: 2
            ```
        1. Dinamik IP kullanıyorsanız MAC adresini aklınızda edin. Statik IP kullanıyorsanız, listelenen IP 'leri ve/veya MAC adresini görebilirsiniz.
        1. Arabirimin VM 'nin birincil NIC 'sine ve birincil IP 'ye karşılık geldiğini doğrulayın. Birincil NIC/IP 'yi Azure portalında ağ yapılandırmasına bakarak veya [Azure CLI ile](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-show)arayarak bulabilirsiniz. Ortak ve özel IP 'Leri (CLI kullanılıyorsa MAC adresini) aklınızda edin. PowerShell CLı örneği:
            ```powershell
            $ResourceGroup = '<Resource_Group>'
            $VmName = '<VM_Name>'
            $NicNames = az vm nic list --resource-group $ResourceGroup --vm-name $VmName | ConvertFrom-Json | Foreach-Object { $_.id.Split('/')[-1] }
            foreach($NicName in $NicNames)
            {
                $Nic = az vm nic show --resource-group $ResourceGroup --vm-name $VmName --nic $NicName | ConvertFrom-Json
                Write-Host $NicName, $Nic.primary, $Nic.macAddress
            }
            # Output: ipexample606 True 00-0D-3A-E4-C7-2E
            ```
        1. Bunlar eşleşmiyorsa, yönlendirme tablosunu, birincil NIC/IP 'nin hedeflediği şekilde güncelleştirin.
    </details>

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Geri bildiriminizi ve açıklamalarınızı üzerine gönderin https://feedback.azure.com .

Hizmete yönelik destek almak için, uzun süreden sonra meta veri yanıtını geçirebileceğiniz VM için Azure portal bir destek sorunu oluşturun.
Sorun türünü kullanın `Management` ve `Instance Metadata Service` Kategori olarak öğesini seçin.

![Örnek meta veri desteği](./media/instance-metadata-service/InstanceMetadata-support.png "Ekran görüntüsü: Instance Metadata Service sorun yaşadığınızda bir destek talebi açma")

## <a name="next-steps"></a>Sonraki Adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:
1. [VM için bir erişim belirteci alın](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).
1. [Zamanlanan Olaylar](scheduled-events.md)