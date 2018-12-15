---
title: Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme
description: Azure Active Directory özellikli Azure Kubernetes Service (AKS) kümeleri oluşturma
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/09/2018
ms.author: iainfou
ms.openlocfilehash: 9bdd3060219907f95454bfc9248572f796afd72e
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437617"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme

Azure Kubernetes Service (AKS), Azure Active Directory (AD) kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. Bu yapılandırmada, Azure Active Directory kimlik doğrulama belirtecinizi kullanarak bir AKS kümesine oturum açabilirsiniz. Ayrıca, küme yöneticileri, bir kullanıcı kimliği veya dizin grubu üyeliğine göre Kubernetes rol tabanlı erişim denetimi (RBAC) yapılandırabilirsiniz.

Bu makalede, AKS ve Azure AD için önkoşulları dağıtma ve Azure AD etkin kümesi dağıtma ve AKS kümesinde basit bir RBAC rolü oluşturmak nasıl gösterir.

Aşağıdaki sınırlamalar geçerlidir:

- RBAC kullanmak için mevcut olmayan-etkin RBAC AKS kümeleri şu anda güncelleştirilemiyor.
- *Konuk* kullanıcıların Azure AD'de gibi farklı bir dizin Federasyon oturum açma kullanıyorsanız olarak desteklenmez.

## <a name="authentication-details"></a>Kimlik doğrulama ayrıntıları

Azure AD kimlik doğrulaması, AKS kümelerine Openıd Connect ile sağlanır. Openıd Connect, OAuth 2.0 protokolünü üzerinde yerleşik bir kimlik katmanı olan. Openıd Connect hakkında daha fazla bilgi için bkz: [Open ID connect belgeleri][open-id-connect].

Gelen bir Kubernetes kümesi içinde Web kancası belirteci kimlik doğrulaması kimlik doğrulama belirteçleri doğrulamak için kullanılır. Web kancası belirteci kimlik doğrulaması yapılandırılır ve AKS kümesinin bir parçası yönetilir. Web kancası belirteci kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Web kancası authentication belgeleri][kubernetes-webhook].

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD yapılandırırken iki Azure AD uygulaması yapılandırılır. Bu işlem, bir Azure Kiracı Yöneticisi tarafından tamamlanması gerekir.

## <a name="create-server-application"></a>Sunucu uygulaması oluşturma

İlk Azure AD uygulaması, kullanıcıları Azure AD grup üyeliğini almak için kullanılır.

1. **Azure Active Directory** > **Uygulama kayıtları** > **Yeni uygulama kaydı**’nı seçin.

  Uygulama seçme gibi bir ad vermek **Web uygulaması / API** uygulama türü için biçimlendirilmiş URI değeri girin **oturum açma URL'si**. Seçin **Oluştur** işiniz bittiğinde.

  ![Azure AD kaydı oluşturma](media/aad-integration/app-registration.png)

2. Seçin **bildirim** ve düzenleme `groupMembershipClaims` değerini `"All"`.

  Güncelleştirme tamamlandıktan sonra kaydedin.

  ![Tüm grup üyeliği güncelleştir](media/aad-integration/edit-manifest.png)

3. Azure AD uygulaması geri üzerinde seçin **ayarları** > **anahtarları**.

  Anahtar bir açıklama ekleyin, bir sona erme tarihi seçip **Kaydet**. Anahtar değerini not alın. AKS kümesini Azure AD'yi dağıtma etkin olduğunda, bu değer olarak adlandırılır `Server application secret`.

  ![Uygulama özel anahtarı alma](media/aad-integration/application-key.png)

4. Azure AD uygulamaya dönmek **ayarları** > **gerekli izinler** > **Ekle**  >   **Bir API seçin** > **Microsoft Graph** > **seçin**.

  ![Graph API seçin](media/aad-integration/graph-api.png)

5. Altında **uygulama izinleri** yanında bir onay işareti koyun **dizin verilerini okuma**.

  ![Uygulama graph izinleri ayarlayın](media/aad-integration/read-directory.png)

6. Altında **TEMSİLCİLİ izinler**, bir onay yanına **oturum açın ve kullanıcı profilini okuma** ve **dizin verilerini okuma**. Bir kez yapılan güncelleştirmeler kaydedin.

  ![Uygulama graph izinleri ayarlayın](media/aad-integration/delegated-permissions.png)

  **Done** (Bitti) öğesini seçin.

7. Seçin *Microsoft Graph* API'leri listesinden seçip **izinler**. Geçerli hesabın, bir kiracı Yöneticisi değilse, bu adımı başarısız olur

  ![Uygulama graph izinleri ayarlayın](media/aad-integration/grant-permissions.png)

  İzinler başarıyla verildi, aşağıdaki bildirim portalda görüntülenir:

  ![Bildirim başarıyla izin verildi](media/aad-integration/permissions-granted.png)

8. Uygulamaya dönmek ve Not **uygulama kimliği**. Azure AD etkin AKS kümesi dağıtırken, bu değer olarak adlandırılır `Server application ID`.

  ![Uygulama Kimliği alma](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>İstemci uygulaması oluşturma

İkinci Azure AD uygulaması, Kubernetes CLI (kubectl) ile oturum açarken kullanılır.

1. **Azure Active Directory** > **Uygulama kayıtları** > **Yeni uygulama kaydı**’nı seçin.

  Uygulama seçme gibi bir ad vermek **yerel** uygulama türü için biçimlendirilmiş URI değeri girin **yeniden yönlendirme URI'si**. Seçin **Oluştur** işiniz bittiğinde.

  ![AAD kaydı oluşturma](media/aad-integration/app-registration-client.png)

2. Azure AD uygulamasından seçin **ayarları** > **gerekli izinler** > **Ekle** > **seçin bir API** ve bu belgenin son adımda oluşturduğunuz sunucu uygulamasının adını arayın.

  ![Uygulama izinlerini yapılandırma](media/aad-integration/select-api.png)

3. ' A tıklayın ve uygulamanın yanında bir onay işareti koyun **seçin**.

  ![AKS AAD sunucu uygulaması uç noktası seçin](media/aad-integration/select-server-app.png)

  Seçin **bitti**

4. API sunucunuz listeden seçin ve sonra **izinler**:

  ![İzinleri verme](media/aad-integration/grant-permissions-client.png)

5. Geri AD uygulaması üzerinde Not **uygulama kimliği**. Azure AD etkin AKS kümesi dağıtırken, bu değer olarak adlandırılır `Client application ID`.

  ![Uygulama Kimliğini Al](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Kiracı kimliğini alma

Son olarak, Azure kiracınızın Kimliğini alın. Bu değer, AKS kümesi dağıtırken de kullanılır.

Azure portalından seçin **Azure Active Directory** > **özellikleri** ve Not **dizin kimliği**. Azure AD etkin AKS kümesi dağıtırken, bu değer olarak adlandırılır `Tenant ID`.

![Azure Kiracı Kimliğinizi alma](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Küme dağıtma

Kullanım [az grubu oluşturma] [ az-group-create] AKS kümesi için bir kaynak grubu oluşturmak için komutu.

```azurecli
az group create --name myResourceGroup --location eastus
```

Küme dağıtmanızı [az aks oluşturma] [ az-aks-create] komutu. Değerleri aşağıdaki örnek komutta Azure AD uygulamaları oluştururken toplanan değerlerle değiştirin.

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bir Azure Active Directory hesabı kullanarak AKS kümesiyle kullanılmadan önce bir rol bağlama veya küme rolünü bağlaması oluşturulması gerekir. *Rolleri* vermek için izinleri tanımlamanıza ve *bağlamaları* bunları istediğiniz kullanıcılar için geçerlidir. Bu atamaları, tüm küme üzerinde veya belirtilen bir ad alanı için uygulanabilir. Daha fazla bilgi için [kullanarak RBAC yetkilendirme][rbac-authorization].

İlk olarak, [az aks get-credentials] [ az-aks-get-credentials] komutunu `--admin` yönetici erişimi ile küme oturum açmak için bağımsız değişken.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Ardından, bir Azure AD hesabı için bir ClusterRoleBinding oluşturmak için aşağıdaki bildirimi kullanın. Bu örnekte, kümenin tüm ad alanları için hesap tam erişim sağlar. Gibi bir dosya oluşturun *rbac aad user.yaml*ve aşağıdaki içeriği yapıştırın. Azure AD kiracınızdan bir kullanıcı adını güncelleştirin:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "user@contoso.com"
```

Bağlama kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komutu aşağıdaki örnekte gösterildiği gibi:

```console
kubectl apply -f rbac-aad-user.yaml
```

Bir rol bağlama için bir Azure AD grubunun tüm üyelerini de oluşturulabilir. Azure AD grupları aşağıdaki örnekte gösterildiği gibi grup nesne kimliği ile belirtilir. Gibi bir dosya oluşturun *rbac aad group.yaml*ve aşağıdaki içeriği yapıştırın. Grubun nesne Kimliğini bir Azure AD kiracınız ile güncelleştirin:

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

Bağlama kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komutu aşağıdaki örnekte gösterildiği gibi:

```console
kubectl apply -f rbac-aad-group.yaml
```

RBAC ile bir Kubernetes kümesi güvenliğini sağlama konusunda daha fazla bilgi için bkz. [kullanarak RBAC yetkilendirme][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>Azure AD ile erişim kümesi

Ardından, yönetici olmayan kullanan kullanıcı için bağlam çekme [az aks get-credentials] [ az-aks-get-credentials] komutu.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Herhangi bir kubectl komutunu çalıştırdıktan sonra Azure ile kimlik doğrulaması istenir. İzleyin ekrandaki yönergeleri.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.9.9
aks-nodepool1-79590246-1   Ready     agent     1h        v1.9.9
aks-nodepool1-79590246-2   Ready     agent     1h        v1.9.9
```

İşlem tamamlandıktan sonra kimlik doğrulama belirteci önbelleğe alınır. Yalnızca ne zaman belirtecinin süresi doldu veya yeniden oluşturulduğunda Kubernetes yapılandırma dosyasında oturum reprompted.

Başarıyla oturum açtıktan sonra bir yetkilendirme hata iletisini görüyorsanız, bir konuk (farklı bir dizin Federasyon oturum açma kullanıyorsanız, genellikle böyledir) Azure ad'deki olduğu gibi kullanıcı, oturum açan olduğunu denetleyin.

```console
error: You must be logged in to the server (Unauthorized)
```

## <a name="next-steps"></a>Sonraki Adımlar

Kubernetes kümelerini ile RBAC ile güvenli hale getirme hakkında daha fazla bilgi [kullanarak RBAC yetkilendirme] [ rbac-authorization] belgeleri.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
