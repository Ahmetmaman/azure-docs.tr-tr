---
title: Azure Redis Cache kullanan bir Python uygulaması oluşturmaya yönelik hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta Redis Cache kullanan bir Python Uygulaması oluşturmayı öğrenirsiniz
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/11/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: b03139bda44886eba13892b23dc17fd3f030cc3f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639546"
---
# <a name="quickstart-use-azure-redis-cache-with-python"></a>Hızlı başlangıç: Python ile Azure Redis Cache kullanma


## <a name="introduction"></a>Giriş

Bu hızlı başlangıçta, bir önbellek üzerinde okuma ve yazma işlemi gerçekleştirmek üzere Python ile Azure Redis Cache’ye nasıl bağlanılacağı gösterilir. 

![Python testi tamamlandı](./media/cache-python-get-started/cache-python-completed.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [pip](https://pypi.org/project/pip/) ile yüklü [Python 2 veya Python 3 ortamı](https://www.python.org/downloads/). 

## <a name="create-a-redis-cache-on-azure"></a>Azure’da Redis Cache oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="install-redis-py"></a>redis-py yükleyin

[Redis-py](https://github.com/andymccurdy/redis-py) Redis Cache’ye yönelik bir Python arabirimidir. Python paketleri aracı olan *pip*’i kullanarak redis-py paketini yükleyin. 

Aşağıdaki örnekte, yükseltilmiş Yönetici ayrıcalıkları ile çalışan bir Visual Studio 2017 Geliştirici Komut istemi kullanan Windows 10 üzerinde redis-py paketi yüklemek üzere Python3 için *pip3* kullanılır.

    pip3 install redis

![redis-py yükleyin](./media/cache-python-get-started/cache-python-install-redis-py.png)


## <a name="read-and-write-to-the-cache"></a>Önbellek üzerinde okuma ve yazma

Python’u çalıştırın ve komut satırındaki önbelleği kullanarak test edin. `<Your Host Name>` ve `<Your Access Key>` değerlerini Redis Cache’nizin değerleriyle değiştirin. 

```python
>>> import redis
>>> r = redis.StrictRedis(host='<Your Host Name>.redis.cache.windows.net',
        port=6380, db=0, password='<Your Access Key>', ssl=True)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

## <a name="create-a-python-script"></a>Python betiği oluşturma

*PythonApplication1.py* adlı yeni bir betik metni dosyası oluşturun.

Aşağıdaki betiği *PythonApplication1.py* dosyasına ekleyin ve dosyayı kaydedin. Bu betik, önbellek erişimini test eder. `<Your Host Name>` ve `<Your Access Key>` değerlerini Redis Cache’nizin değerleriyle değiştirin. 

```python
import redis

myHostname = "<Your Host Name>.redis.cache.windows.net"
myPassword = "<Your Access Key>"

r = redis.StrictRedis(host=myHostname, port=6380,password=myPassword,ssl=True)

result = r.ping()
print("Ping returned : " + str(result))

result = r.set("Message", "Hello!, The cache is working with Python!")
print("SET Message returned : " + str(result))

result = r.get("Message")
print("GET Message returned : " + result.decode("utf-8"))

result = r.client_list()
print("CLIENT LIST returned : ") 
for c in result:
    print("id : " + c['id'] + ", addr : " + c['addr'])
```

Python ile betiği çalıştırın.

![Python testi tamamlandı](./media/cache-python-get-started/cache-python-completed.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Başka bir öğretici ile devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
>

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu makaledeki yönergelerde *TestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

![Sil](./media/cache-web-app-howto/cache-delete-resource-group.png)

Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Redis Cache kullanan basit bir ASP.NET web uygulaması oluşturun.](./cache-web-app-howto.md)



<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
