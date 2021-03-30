---
title: Azure Service Fabric merkezi gizli dizi deposu
description: Bu makalede, Azure Service Fabric 'da merkezi gizlilikler deposunun nasıl kullanılacağı açıklanır.
ms.topic: conceptual
ms.date: 07/25/2019
ms.openlocfilehash: e9fd435803ad5354b0eb2d4f5de50009a8cbbfe2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88869764"
---
# <a name="central-secrets-store-in-azure-service-fabric"></a>Azure Service Fabric 'da merkezi gizlilikler Mağazası 
Bu makalede, Service Fabric uygulamalarında gizli diziler oluşturmak için Azure Service Fabric 'da merkezi gizlilikler deposunun (CSS) nasıl kullanılacağı açıklanır. CSS, bir parola, belirteç ve anahtar gibi hassas verileri bellekte şifreli olarak tutan bir yerel gizli dizi deposu önbelleğidir.

  > [!NOTE] 
  > İlk kez, SF sürüm 7,1 ' den önce CSS 'yi etkinleştirirken. CU3, etkinleştirme başarısız olabilir ve CSS, Windows kimliği doğrulanmış bir kümede etkinleştirilirse, kalıcı olarak sağlıksız bir durumda kalabilir. CSS herhangi bir kümede etkinleştirilir, ancak `EncryptionCertificateThumbprint` yanlış olarak veya karşılık gelen sertifika yüklü değil veya düğümler ÜZERINDE ACL 'ye bağlı değil. Windows auth kümesi için lütfen 7,1 'e katılın. Devam etmeden önce CU3. Diğer kümeler için lütfen bu ınvaryantları çift işaretleyin veya 7,1 üzerine gelin. CU3.
  
## <a name="enable-central-secrets-store"></a>Merkezi gizli dizi deposunu etkinleştir
CSS 'yi etkinleştirmek için aşağıdaki betiği küme yapılandırmanıza ekleyin `fabricSettings` . CSS için bir küme sertifikası dışında bir sertifika kullanmanızı öneririz. Şifreleme sertifikasının tüm düğümlerde yüklü olduğundan ve `NetworkService` sertifikanın özel anahtarı için okuma iznine sahip olduğundan emin olun.
  ```json
    "fabricSettings": 
    [
        ...
    {
        "name":  "CentralSecretService",
        "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "1"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                },
                 {
                    "name" : "EncryptionCertificateThumbprint",
                    "value": "<thumbprint>"
                 }
                ,
                ],
            },
            ]
     }
        ...
     ]
```
## <a name="declare-a-secret-resource"></a>Gizli bir kaynak bildirin
REST API kullanarak gizli bir kaynak oluşturabilirsiniz.
  > [!NOTE] 
  > Küme Windows kimlik doğrulaması kullanıyorsa, REST isteği güvenli olmayan HTTP kanalı üzerinden gönderilir. Bu öneri, güvenli uç noktalarla x509 tabanlı bir küme kullanmaktır.

`supersecret`REST API kullanarak bir gizli kaynak oluşturmak için, için BIR PUT isteği yapın `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview` . Gizli bir kaynak oluşturmak için küme sertifikası veya yönetici istemci sertifikasına ihtiyacınız vardır.

```powershell
$json = '{"properties": {"kind": "inlinedValue", "contentType": "text/plain", "description": "supersecret"}}'
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint> -Body $json
```

## <a name="set-the-secret-value"></a>Gizli değeri ayarla

Gizli değeri ayarlamak için REST API kullanmak için aşağıdaki betiği kullanın.
```powershell
$Params = '{"properties": {"value": "mysecretpassword"}}'
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
### <a name="examine-the-secret-value"></a>Gizli değeri inceleyin
```powershell
Invoke-WebRequest -CertificateThumbprint <ClusterCertThumbprint> -Method POST -Uri "https:<clusterfqdn>/Resources/Secrets/supersecret/values/ver1/list_value?api-version=6.4-preview"
```
## <a name="use-the-secret-in-your-application"></a>Uygulamanızda gizli dizi kullanın

Service Fabric uygulamanızda gizli dizi kullanmak için bu adımları izleyin.

1. Aşağıdaki kod parçacığına sahip **settings.xml** dosyasına bir bölüm ekleyin. Değerin {} biçiminde olduğunu buraya unutmayın `secretname:version` .

   ```xml
     <Section Name="testsecrets">
      <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
     </Section>
   ```

1. **ApplicationManifest.xml** bölümünü içeri aktarın.
   ```xml
     <ServiceManifestImport>
       <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
       <ConfigOverrides />
       <Policies>
         <ConfigPackagePolicies CodePackageRef="Code">
           <ConfigPackage Name="Config" SectionName="testsecrets" EnvironmentVariableName="SecretPath" />
           </ConfigPackagePolicies>
       </Policies>
     </ServiceManifestImport>
   ```

   Ortam değişkeni, `SecretPath` tüm parolaların depolandığı dizine işaret eder. Bölüm altında listelenen her bir parametre `testsecrets` ayrı bir dosyada depolanır. Uygulama artık gizli anahtarı şu şekilde kullanabilir:
   ```C#
   secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
   ```
1. Gizli dizileri bir kapsayıcıya bağlayın. Kapsayıcının içinde kullanılabilir gizli dizileri oluşturmak için gereken tek değişiklik, `specify` içindeki bir bağlama noktasına ait `<ConfigPackage>` .
Aşağıdaki kod parçacığı değiştirilen **ApplicationManifest.xml**.  

   ```xml
   <ServiceManifestImport>
       <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
       <ConfigOverrides />
       <Policies>
         <ConfigPackagePolicies CodePackageRef="Code">
           <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="C:\secrets" EnvironmentVariableName="SecretPath" />
           <!-- Linux Container
            <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="/mnt/secrets" EnvironmentVariableName="SecretPath" />
           -->
         </ConfigPackagePolicies>
       </Policies>
     </ServiceManifestImport>
   ```
   Gizli dizileri, kapsayıcının içindeki bağlama noktası altında bulunur.

1. Belirterek bir işlem ortam değişkenine gizli dizi bağlayabilirsiniz `Type='SecretsStoreRef` . Aşağıdaki kod parçacığı, `supersecret` sürümü `ver1`ServiceManifest.xmlortam değişkenine bağlamaya bir örnektir `MySuperSecret` . ****

   ```xml
   <EnvironmentVariables>
     <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
   </EnvironmentVariables>
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md)hakkında daha fazla bilgi edinin.
