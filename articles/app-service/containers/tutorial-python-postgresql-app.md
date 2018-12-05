---
title: Azure App Service'te Python ve PostgreSQL web uygulaması oluşturma | Microsoft Docs
description: Azure'da bir PostgreSQL veritabanına bağlantısı olan veri temelli bir Python uygulamasını nasıl çalıştıracağınızı öğrenin.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: 3963e2ffb521a4b4732814e9b2992f4e83af1835
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865633"
---
# <a name="build-a-python-and-postgresql-web-app-in-azure-app-service"></a>Azure App Service'te bir Python ve PostgreSQL web uygulaması derleme

[Linux’ta App Service](app-service-linux-intro.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide veritabanı arka ucu olarak PostgreSQL kullanan veri temelli bir Python web uygulamasının nasıl oluşturulacağı gösterilmektedir. İşiniz bittiğinde, Linux üzerinde App Service'te çalışan bir Django uygulaması vardır.

![Linux üzerinde App Service'te Python Django uygulaması](./media/tutorial-python-postgresql-app/django-admin-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını PostgreSQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Tanılama günlüklerini görüntüleme
> * Uygulamayı Azure portalında yönetme

Bu makaledeki adımları macOS üzerinde izleyebilirsiniz. Linux ve Windows yönergeleri çoğu durumda aynıdır, ancak bu öğreticide farkları konusunda ayrıntıya girilmemiştir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [Python'ı yükleyin](https://www.python.org/downloads/)
3. [PostgreSQL’i yükleyin ve çalıştırın](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Yerel PostgreSQL yüklemesini test etme ve bir veritabanı oluşturma

Yerel bir terminal penceresinde `psql` komutunu çalıştırarak yerel PostgreSQL sunucunuza bağlanın.

```bash
sudo -u postgres psql postgres
```

`unknown user: postgres` gibi bir hata iletisi alırsanız, PostgreSQL yüklemeniz oturum açmış kullanıcı adınız ile yapılandırılabilir. Bunun yerine aşağıdaki komutu deneyin.

```bash
psql postgres
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusunda işletim sisteminiz için yönergeler izlenilerek başlatıldığından emin olun.

Adlı bir veritabanı oluşturun *pollsdb* ve adlı bir ayrı bir veritabanı kullanıcısı ayarlayın *manager* parolayla *Manager*.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Yerel Python uygulaması oluşturma

Bu adımda, yerel Python Django projesi olarak ayarlayın.

### <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Terminal penceresini açın ve bir çalışma dizinine `CD` yazın.

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

Bu örnek depo içeren bir [Django](https://www.djangoproject.com/) uygulama. Alma izleyerek aynı veri temelli uygulaması olan [başlangıç Öğreticisi Django belgelerinde](https://docs.djangoproject.com/en/2.1/intro/tutorial01/). Bu öğreticide, Django öğretmek değildir, ancak nasıl gösterir dağıtın ve App Service'e bir Django uygulaması (veya başka bir veri temelli Python uygulaması) çalıştırın.

### <a name="configure-environment"></a>Ortamı yapılandırma

Bir Python sanal ortamı oluşturun ve veritabanı bağlantı ayarları ayarlamak için bir betik kullanın.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
source ./env.sh

# PowerShell
py -3 -m venv venv
venv\scripts\activate
.\env.ps1
```

Tanımlanan ortam değişkenleri *env.sh* ve *env.ps1* kullanılan _azuresite/settings.py_ veritabanı ayarlarını tanımlamak için.

### <a name="run-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Gerekli paketleri yükleyin [Django geçişlerini çalıştırarak](https://docs.djangoproject.com/en/2.1/topics/migrations/) ve [bir yönetici kullanıcı oluşturma](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user).

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

Django sunucunun Yönetici kullanıcıyı oluşturduktan sonra çalıştırın.

```bash
python manage.py runserver
```

Uygulama tam olarak yüklendiğinde, şu iletiye benzer bir şey görürsünüz:

```bash
Performing system checks...

System check identified no issues (0 silenced).
October 26, 2018 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Bir tarayıcıda `http://localhost:8000` sayfasına gidin. Şu iletiyi görürsünüz `No polls are available.`. 

Gidin `http://localhost:8000/admin` ve son adımda oluşturduğunuz yönetici kullanıcı kullanarak oturum açın. Tıklayın **Ekle** yanındaki **sorular** ve bazı seçenekleri yoklama soru oluşturun.

![Yerel olarak çalışan Python Django uygulaması](./media/tutorial-python-postgresql-app/django-admin-local.png)

Gidin `http://localhost:8000` yeniden ve görüntülenen yoklama soruya bakın.

Django örnek uygulamanın kullanıcı verilerini veritabanında depolar. Yoklama soru ekleme sırasında başarılı olursanız, uygulamanız yerel PostgreSQL veritabanına veri yazıyor demektir.

Django sunucunun dilediğiniz zaman durdurmak için terminale Ctrl + C yazın.

## <a name="create-a-production-postgresql-database"></a>Üretim PostgreSQL veritabanı oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Cloud Shell'de [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) komutuyla bir PostgreSQL sunucusu oluşturun.

Aşağıdaki örnek komutta *\<postgresql_name>* yerine benzersiz bir sunucu adı, *\<admin_username>* ve *\<admin_password>* yerine de kullanmak istediğiniz kullanıcı bilgilerini yazın. Kullanıcı kimlik bilgileri, veritabanı yöneticisi hesabı için geçerli olacaktır. Sunucu adı, PostgreSQL uç noktasının bir parçası olan `https://<postgresql_name>.postgres.database.azure.com` olarak kullanıldığından, adın Azure’daki tüm sunucularda benzersiz olması gerekir.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name B_Gen4_1
```

PostgreSQL için Azure Veritabanı sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "<admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> \<admin_username> ve \<admin_password> değerlerini daha sonra kullanmak üzere aklınızda tutun. Postgre sunucusu ve veritabanlarında oturum açmak için bunlara ihtiyacınız vardır.


### <a name="create-firewall-rules-for-the-postgresql-server"></a>PostgreSQL sunucusu için güvenlik duvarı kuralları oluşturma

Azure kaynaklarından veritabanına erişim izni vermek için Cloud Shell'de aşağıdaki Azure CLI komutunu çalıştırın.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Bu ayar, Azure ağ içindeki tüm IP’lerden ağ bağlantılarına izin verir. Üretim kullanımı için, [yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) en kısıtlayıcı güvenlik duvarı kurallarını yapılandırmayı deneyin.

Cloud Shell'de *\<your_ip_address>* yerine [yerel IPv4 IP adresinizi](http://www.whatsmyip.org/) yazdıktan sonra komutu tekrar çalıştırarak yerel bilgisayarınızdan erişim izni verin.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Python uygulamasını üretim veritabanına bağlama

Bu adımda, Django örnek uygulamanızı oluşturduğunuz PostgreSQL sunucusu için Azure veritabanına bağlanın.

### <a name="create-empty-database-and-user-access"></a>Boş veritabanı oluşturma ve kullanıcı erişimi sağlama

Cloud Shell'de aşağıdaki komutu çalıştırarak veritabanına bağlanın. Yönetici parolanızı girmeniz istendiğinde [PostgreSQL için Azure Veritabanı sunucusu oluşturma](#create-an-azure-database-for-postgresql-server) bölümünde belirttiğiniz parolayı kullanın.

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Yerel Postgres sunucunuzda olduğu gibi, Azure Postgres sunucusunda veritabanı ve kullanıcıyı oluşturun.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

> [!NOTE]
> Yönetici kullanıcıyı kullanmak yenine belirli uygulamalar için kısıtlı izinlere sahip olan veritabanı kullanıcıları oluşturmak en iyi uygulamadır. Bu örnekte, `manager` kullanıcısı _yalnızca_ `pollsdb` veritabanı için tam ayrıcalıklara sahiptir.

### <a name="test-app-connectivity-to-production-database"></a>Uygulamanın üretim veritabanına bağlanıp bağlanmadığını test etme

Yerel terminal penceresinde, veritabanı ortam değişkenlerini değiştirin (çalıştırarak daha önce yapılandırılan *env.sh* veya *env.ps1*):

```bash
# Bash
export DBHOST="<postgresql_name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql_name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"

# PowerShell
$Env:DBHOST = "<postgresql_name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql_name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Azure veritabanı'na Django geçişi çalıştırın ve bir yönetici kullanıcı oluşturun.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Django sunucunun Yönetici kullanıcıyı oluşturduktan sonra çalıştırın.

```bash
python manage.py runserver
```

Gidin `http://localhost:8000` içinde yeniden. Şu iletiyi görürsünüz `No polls are available.` yeniden. 

Gidin `http://localhost:8000/admin` oluşturduğunuz yönetici kullanıcı kullanarak oturum açın ve yoklama soru önce gibi oluşturun.

![Yerel olarak çalışan Python Django uygulaması](./media/tutorial-python-postgresql-app/django-admin-local.png)

Gidin `http://localhost:8000` yeniden ve görüntülenen yoklama soruya bakın. Uygulamanız artık azure'da veritabanına veri yazıyor demektir.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, Postgres’e bağlı Python uygulamasını Azure App Service'e dağıtırsınız.

### <a name="configure-repository"></a>Depoyu yapılandırma

Django doğrular `HTTP_HOST` üst bilgisinde gelen istekler. Django uygulamanız App Service'te çalışmaya uygulamasının tam etki alanı adı izin verilen konakları eklemeniz gerekir. Açık _azuresite/settings.py_ ve bulma `ALLOWED_HOSTS` ayarı. Çizginin değiştirin:

```python
ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
```

Ardından, Django desteklemiyor [üretimde statik dosyaları sunma](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/), bu nedenle bu el ile etkinleştirmeniz gerekir. Bu öğretici için kullandığınız [WhiteNoise](http://whitenoise.evans.io/en/stable/). WhiteNoise paket zaten yer aldığı _requirements.txt_. Django kullanmak için yapılandırmanız yeterlidir. 

İçinde _azuresite/settings.py_, bulma `MIDDLEWARE` ayarlama ve ekleme `whitenoise.middleware.WhiteNoiseMiddleware` ara yazılım listesine hemen altına `django.middleware.security.SecurityMiddleware` ara yazılım. `MIDDLEWARE` Ayarı şöyle görünmelidir:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```

Sonunda _azuresite/settings.py_, aşağıdaki satırları ekleyin.

```python
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

WhiteNoise yapılandırma hakkında daha fazla bilgi için bkz. [WhiteNoise belgeleri](http://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> Veritabanı ayarları bölümü zaten ortam değişkenlerini kullanarak en iyi güvenlik uygulamalarını takip eder. Tam dağıtım önerileri için bkz: [Django belgeleri: dağıtım denetim listesi](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).


Yaptığınız değişiklikleri depoya uygulayın.

```bash
git commit -am "configure for App Service"
```

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma 

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Öğreticinin önceki bölümlerinde, PostgreSQL veritabanınıza bağlanmak üzere ortam değişkenleri tanımladınız.

App Service’te, Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak ortam değişkenlerini _uygulama ayarları_ olarak ayarlayabilirsiniz.

Şu örnek, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtir. 

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 775 bytes | 0 bytes/s, done.
Total 7 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6520eeafcc'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Python deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
. 
. 
. 
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git 
   06b6df4..6520eea  master -> master
```  

App Service dağıtım sunucusu görür _requirements.txt_ depo kökünde ve Python paket Yönetimi otomatik olarak çalıştırır `git push`.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

Dağıtılan web uygulamasına göz atın. Uygulama ilk kez çağrıldığında kapsayıcının indirilmesi ve çalışması gerektiğinden başlatılması biraz uzun sürebilir. Sayfa zaman aşımına uğrar veya hata iletisi görüntülerse, birkaç dakika bekleyip sayfayı yenileyin.

```bash
http://<app_name>.azurewebsites.net
```

Daha önce oluşturduğunuz yoklama soru görmeniz gerekir. 

App Service bakarak deponuzda Django projesi algılar bir _wsgi.py_ her alt dizininde oluşturulduğu tarafından `manage.py startproject` varsayılan olarak. Dosyayı bulur, Django uygulaması yükler. App Service Python uygulamaları nasıl yükler ile ilgili daha fazla bilgi için bkz: [yapılandırma yerleşik Python görüntüsünü](how-to-configure-python.md).

Gidin `<app_name>.azurewebsites.net` ve aynı yönetici kullanıcı, oluşturduğunuz kullanarak oturum açın. İsterseniz, bazı yoklama sorularım oluşturmayı deneyin.

![Yerel olarak çalışan Python Django uygulaması](./media/tutorial-python-postgresql-app/django-admin-azure.png)

**Tebrikler!** Linux için App Service’te bir Python uygulaması çalıştırıyorsunuz.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

Linux üzerinde App Service'te uygulamaları varsayılan bir Docker görüntüsü kapsayıcının içinde çalıştırılır. Kapsayıcı içinde oluşturulan yönlendirilen konsol günlüklerini erişebilirsiniz. Günlükleri almak için önce Cloud Shell'de aşağıdaki komutu çalıştırarak kapsayıcı günlüğü etkinleştirin:

```azurecli-interactive
az webapp log config --name <app_name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Kapsayıcı günlüğe kaydetme etkinleştirildikten sonra günlük akışını görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

## <a name="manage-your-web-app-in-the-azure-portal"></a>Azure portalda web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-python-postgresql-app/app-resource.png)

Portal, varsayılan olarak web uygulamanızın **Genel Bakış** sayfasında görünür. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını PostgreSQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Tanılama günlüklerini görüntüleme
> * Uygulamayı Azure portalında yönetme

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Yerleşik Python görüntüsünü ve hatalarında sorun giderme yapılandırın](how-to-configure-python.md)

