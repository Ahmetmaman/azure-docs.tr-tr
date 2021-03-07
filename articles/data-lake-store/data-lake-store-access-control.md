---
title: Data Lake Storage 1. | erişim denetimine genel bakış | Microsoft Docs
description: Azure Data Lake Storage 1. erişim denetimi modelinin temel bilgileri hakkında bilgi edinin.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: aa0da5721c577957b101ac8a2d9346c0536f0a88
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102424147"
---
# <a name="access-control-in-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. Nesil'de erişim denetimi

Azure Data Lake Storage 1., bu sırayla, POSIX erişim denetimi modelinden türetilen bir erişim denetimi modeli uygular. Bu makalede Data Lake Storage 1. için erişim denetimi modelinin temelleri özetlenmektedir. 

## <a name="access-control-lists-on-files-and-folders"></a>Dosyalar ve klasörler üzerindeki erişim denetimi listeleri

İki tür erişim denetim listesi (ACL) vardır: **Erişim ACL’leri** ve **Varsayılan ACL’ler**.

* **Erişim ACL’leri**: Bunlar bir nesneye erişimi denetler. Hem dosyalar hem de klasörler Erişim ACL’lerine sahiptir.

* **Varsayılan ACL’ler**: Bir klasör ile ilişkili olan ACL’lerin o klasör altında oluşturulan tüm alt öğelere ilişkin Erişim ACL’lerini belirleyen bir "şablonudur". Dosyalar Varsayılan ACL’ye sahip değildir.


Hem Erişim ACL'leri hem de Varsayılan ACL'ler aynı yapıdadır.

> [!NOTE]
> Bir üst öğe üzerindeki Varsayılan ACL’nin değiştirilmesi zaten var olan alt öğelerin Erişim ACL’sini veya Varsayılan ACL’sini etkilemez.
>
>

## <a name="permissions"></a>İzinler

Dosya sistemi nesnesi üzerinde **Okuma**, **Yazma** ve **Yürütme** izinleri bulunur ve bunlar aşağıdaki tabloda gösterildiği gibi dosyalar ve klasörler üzerinde kullanılabilir:

|            |    Dosya     |   Klasör |
|------------|-------------|----------|
| **Okuma (R)** | Bir dosyanın içeriğini okuyabilir | Klasörün içeriğini listelemek için **Okuma** ve **Yürütme** izinlerini gerektirir|
| **Yazma (W)** | Bir dosyaya yazabilir veya ekleyebilir | Bir klasörde alt öğeler oluşturmak için **Yazma** ve **Yürütme** gerektirir |
| **Yürütme (X)** | Data Lake Storage 1. bağlamında hiçbir şey anlamına gelmez | Bir klasörün alt öğelerini geçirmek için gereklidir |

### <a name="short-forms-for-permissions"></a>İzinlerin kısaltmaları

**RWX**, **Okuma + Yazma + Yürütme** için kullanılır. **Okuma=4**, **Yazma=2** ve **Yürütme=1** olup toplamları izinleri temsil eden daha da kısaltılmış bir sayısal biçim mevcuttur. Bazı örnekler aşağıda verilmiştir.

| Sayısal biçim | Kısa biçim |      Anlamı     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Okuma + Yazma + Yürütme |
| 5            | `R-X`        | Okuma + Yürütme         |
| 4            | `R--`        | Okuma                   |
| 0            | `---`        | İzin yok         |


### <a name="permissions-do-not-inherit"></a>İzinler devralınmaz

Data Lake Storage 1. tarafından kullanılan POSIX stili modelde bir öğe için izinler öğenin kendisinde depolanır. Diğer bir deyişle, bir öğenin izinleri üst öğelerinden devralınamaz.

## <a name="common-scenarios-related-to-permissions"></a>İzinlerle ilgili yaygın senaryolar

Aşağıda, Data Lake Storage 1. hesapta belirli işlemleri gerçekleştirmek için hangi izinlerin gerekli olduğunu anlamanıza yardımcı olacak bazı yaygın senaryolar verilmiştir.

| İşlem | Nesne              |    /      | Ağına   | Portland   | Data.txt       |
|-----------|---------------------|-----------|------------|-------------|----------------|
| Okuma      | Data.txt            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Sonuna ekle | Data.txt            |   `--X`   |   `--X`    |  `--X`      | `-W-`          |
| Sil    | Data.txt            |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Oluştur    | Data.txt            |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Liste      | /                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Liste      | Ağına           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Liste      | /Seattle/Portland/  |   `--X`   |   `--X`    |  `R-X`      | `---`          |


> [!NOTE]
> Önceki iki koşul geçerli oldukça dosyayı silmek için dosya üzerinde yazma izinleri gerekli değildir.
>
>


## <a name="users-and-identities"></a>Kullanıcılar ve kimlikler

Her dosya ve klasör bu kimlikler için farklı izinlere sahiptir:

* Sahip olan kullanıcı
* Sahip olan grup
* Adlandırılmış kullanıcılar
* Adlandırılmış gruplar
* Diğer tüm kullanıcılar

Kullanıcıların ve grupların kimlikleri, Azure Active Directory (Azure AD) kimlikleridir. Aksi belirtilmedikçe, Data Lake Storage 1. bağlamında bir "Kullanıcı", bir Azure AD kullanıcısı veya bir Azure AD güvenlik grubu anlamına gelebilir.

### <a name="the-super-user"></a>Süper kullanıcı

Bir süper kullanıcı Data Lake Storage 1. hesabındaki tüm kullanıcıların en fazla haklarına sahiptir. Süper kullanıcı:

* **Tüm** dosya ve klasörlerde RWX İzinlerine sahiptir.
* Herhangi bir dosya veya klasörün izinlerini değiştirebilir.
* Herhangi bir dosya veya klasörün sahibi olan kullanıcıyı ya da grubu değiştirebilir.

Bir Data Lake Storage 1. hesabı için **sahip** rolünün parçası olan tüm kullanıcılar otomatik olarak bir süper kullanıcılardır.

### <a name="the-owning-user"></a>Sahip olan kullanıcı

Öğeyi oluşturan kullanıcı otomatik olarak öğenin sahibi olan kullanıcıdır. Sahip olan kullanıcı şunları yapabilir:

* Sahip olunan bir dosyanın izinlerini değiştirme.
* Sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi oldukça, sahip olunan bir dosyanın sahibi olan grubunu değiştirme.

> [!NOTE]
> Sahip olan kullanıcı bir dosya veya klasörün sahibi olan kullanıcıyı *değiştiremez*. Bir dosya veya klasörün sahibi olan kullanıcıyı yalnızca süper kullanıcılar değiştirebilir.
>
>

### <a name="the-owning-group"></a>Sahip olan grup

**Arka Plan**

POSIX ACL’lerinde her kullanıcı bir "birincil grup" ile ilişkilendirilir. Örneğin, "gamze" adlı kullanıcı "finans" grubuna ait olabilir. Gamze ayrıca birden fazla gruba ait olabilir, ancak bir grup her zaman birincil grubu olarak atanır. POSIX’te Gamze bir dosya oluşturduğunda o dosyanın sahibi olan grup birincil grubu olarak ayarlanır (bu örnekte "finans" grubudur). Aksi takdirde sahip olan grup, diğer kullanıcılar/gruplar için atanan izinlere benzer şekilde davranır.

Data Lake Storage 1. olan kullanıcılarla ilişkili bir "birincil grup" olmadığından, sahip olan grup aşağıda gösterildiği gibi atanır.

**Yeni bir dosya veya klasör için sahip olan grup atanıyor**

* **Olay 1**: Kök klasör "/". Bu klasör Data Lake Storage 1. bir hesap oluşturulduğunda oluşturulur. Bu durumda sahip olan Grup, hepsi sıfır GUID 'sine ayarlanır.  Bu değer herhangi bir erişime izin vermez.  Bu, bir grup atanıncaya kadar bir yer tutucudur.
* **Olay 2** (Diğer her olay): Yeni bir olay oluşturulduğunda sahip olan grup üst klasörden kopyalanır.

**Sahip olan grubu değiştirme**

Sahip olan grup aşağıdakiler tarafından değiştirilebilir:
* Herhangi bir süper kullanıcı.
* Sahip olan kullanıcı aynı zamanda hedef grubun üyesi ise sahip olan kullanıcı.

> [!NOTE]
> Sahip olan grup, bir dosya veya klasörün ACL’lerini *değiştiremez*.
>
> 2018 Eylül tarihinde veya daha önce oluşturulan hesaplar için, sahip olan Grup, yukarıdaki **1. durum** için kök klasör olması durumunda hesabı oluşturan kullanıcıya ayarlanmıştır.  Tek bir kullanıcı hesabı, sahip olan grup aracılığıyla izin sağlamak için geçerli değildir, bu nedenle bu varsayılan ayar tarafından izin verilmez. Bu izni, geçerli bir kullanıcı grubuna atayabilirsiniz.


## <a name="access-check-algorithm"></a>Erişim denetimi algoritması

Aşağıdaki sözde kod Data Lake Storage 1. hesapları için erişim denetimi algoritmasını temsil eder.

```
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or folder
  # Note: the "sticky bit" is not illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

  # Handle the owning user. Note that mask IS NOT used.
  entry = get_acl_entry( path, OWNER )
  if (user == entry.identity)
      return ( (desired_perms & entry.permissions) == desired_perms )

  # Handle the named users. Note that mask IS used.
  entries = get_acl_entries( path, NAMED_USER )
  for entry in entries:
      if (user == entry.identity ) :
          mask = get_mask( path )
          return ( (desired_perms & entry.permmissions & mask) == desired_perms)

  # Handle named groups and owning group
  member_count = 0
  perms = 0
  entries = get_acl_entries( path, NAMED_GROUP | OWNING_GROUP )
  for entry in entries:
    if (user_is_member_of_group(user, entry.identity)) :
      member_count += 1
      perms | =  entry.permissions
  if (member_count>0) :
    return ((desired_perms & perms & mask ) == desired_perms)
 
  # Handle other
  perms = get_perms_for_other(path)
  mask = get_mask( path )
  return ( (desired_perms & perms & mask ) == desired_perms)
```

### <a name="the-mask"></a>Maske

Erişim denetimi algoritmasında gösterildiği gibi, maske **adlandırılmış kullanıcılar**, **sahip olan grup** ve **adlandırılmış gruplar** için erişimi sınırlandırır.  

> [!NOTE]
> Yeni bir Data Lake Storage 1. hesabı için kök klasörün ("/") erişim ACL 'SI için maske varsayılan olarak RWX 'tir.
>
>

### <a name="the-sticky-bit"></a>Yapışkan bit

Yapışkan bit POSIX dosya sisteminin daha gelişmiş bir özelliğidir. Data Lake Storage 1. bağlamında, yapışkan bitin gerekli olacağı pek olası bir olasılıktır. Özet ' te, yapışkan bit bir klasörde etkinse, alt öğe yalnızca alt öğenin sahibi olan kullanıcı tarafından silinebilir veya yeniden adlandırılabilir.

Yapışkan bit Azure portalında gösterilmez.

## <a name="default-permissions-on-new-files-and-folders"></a>Yeni dosya ve klasörlerde varsayılan izinler

Var olan bir klasör altında yeni bir dosya ya da klasör oluşturulduğunda üst klasördeki Varsayılan ACL aşağıdakileri belirler:

- Bir alt klasörün Varsayılan ACL’si ve Erişim ACL’si.
- Bir alt dosyanın Erişim ACL’si (dosyaları Varsayılan ACL’ye sahip değildir).

### <a name="umask"></a>umask

Bir dosya veya klasör oluştururken, varsayılan ACL 'Lerin alt öğede nasıl ayarlandığını değiştirmek için umask kullanılır. umask, **sahip olan Kullanıcı**, **sahip olan grup** ve **diğer** için RWX değeri içeren üst klasörlerdeki 9 bitlik bir değerdir.

Azure Data Lake Storage 1. için umask, 007 olarak ayarlanan sabit bir değerdir. Bu değer şunu yapar

| umask bileşeni     | Sayısal biçim | Kısa biçim | Anlamı |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | Sahip olan kullanıcı için, üst öğenin varsayılan ACL 'sini alt öğenin erişim ACL 'sine kopyalayın | 
| umask.owning_group  |    0         |   `---`      | Sahip olan grup için üst öğenin varsayılan ACL 'sini alt öğenin erişim ACL 'sine kopyalayın | 
| uımask. other         |    7         |   `RWX`      | Diğer bir deyişle, alt öğenin erişim ACL 'sindeki tüm izinleri kaldırın |

Azure Data Lake Storage 1. tarafından kullanılan umask değeri, varsayılan ACL 'nin gösterdiği değerden bağımsız olarak, diğer alt klasörlerde hiçbir şekilde varsayılan olarak iletilmediği anlamına gelir. 

Aşağıdaki sözde kod, bir alt öğe için ACL 'Ler oluştururken umask 'in nasıl uygulanacağını gösterir.

```
def set_default_acls_for_new_child(parent, child):
    child.acls = []
    for entry in parent.acls :
        new_entry = None
        if (entry.type == OWNING_USER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_user))
        elif (entry.type == OWNING_GROUP) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_group))
        elif (entry.type == OTHER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.other))
        else :
            new_entry = entry.clone(perms = entry.perms )
        child_acls.add( new_entry )
```

## <a name="common-questions-about-acls-in-data-lake-storage-gen1"></a>Data Lake Storage 1. ACL 'Lerle ilgili genel sorular

### <a name="do-i-have-to-enable-support-for-acls"></a>ACL desteğini etkinleştirmem gerekiyor mu?

Hayır. Data Lake Storage 1. bir hesap için ACL 'Ler aracılığıyla erişim denetimi her zaman açıktır.

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Bir klasörü ve içindekileri yinelemeli olarak silmek için hangi izinler gereklidir?

* Üst klasör **Yazma + Yürütme** izinlerine sahip olmalıdır.
* Silinecek klasör ve içindeki her klasör **Okuma + Yazma + Yürütme** izinlerini gerektirir.

> [!NOTE]
> Klasörlerdeki dosyaları silmek için Yazma izni gerekmez. Ayrıca, "/" kök klasör **hiçbir zaman** silinemez.
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a>Bir dosyanın veya klasörün sahibi kimdir?

Bir dosyayı veya klasörü oluşturan kişi bunların sahibi olur.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Oluşturma sırasında bir dosyanın veya klasörün sahibi olan grubu olarak hangi grup ayarlanır?

Sahip olan grup, yeni dosya veya klasörün oluşturulduğu üst klasörün sahibi olan gruptan kopyalanır.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Bir dosyanın sahibiyim, ancak gereken RWX izinlerine sahip değilim. Ne yapmalıyım?

Sahip olan kullanıcı kendisine gerekli olan her türlü RWX iznini vermek için dosyanın izinlerini değiştirebilir.

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Azure portalında ACL’lere baktığımda kullanıcı adlarını görüyorum, ancak API’lere baktığımda GUID’leri görüyorum, bunun nedeni nedir?

ACL’lerdeki girişler, Azure AD’de kullanıcılara karşılık gelen GUID’ler olarak depolanır. API’ler GUID’leri olduğu gibi döndürür. Azure portalı mümkün olduğunda GUID’leri kolay adlara çevirerek ACL’lerin daha kolay kullanılmasını sağlamaya çalışır.

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a>Azure portalını kullanırken neden bazen ACL’lerde GUID’leri görüyorum?

Kullanıcı artık Azure AD’de mevcut değilse bir GUID gösterilir. Bu genellikle, kullanıcı şirketten ayrıldığında veya Azure AD’de kullanıcının hesabı silindiğinde gerçekleşir. Ayrıca, ACL 'Leri ayarlamak için doğru KIMLIĞI kullandığınızdan emin olun (aşağıdaki sorudaki Ayrıntılar).

### <a name="when-using-service-principal-what-id-should-i-use-to-set-acls"></a>Hizmet sorumlusu kullanılırken, ACL 'Leri ayarlamak için hangi KIMLIĞI kullanmalıyım?

Azure portalında **Azure Active Directory-> kurumsal uygulamalar** ' a gidin ve uygulamanızı seçin. **Genel bakış** sekmesi BIR nesne kimliği görüntülemelidir ve veri erişimi Için ACL 'ler eklenirken (uygulama kimliği değil) kullanılması gereken şeydir.

### <a name="does-data-lake-storage-gen1-support-inheritance-of-acls"></a>Data Lake Storage 1. ACL 'Leri devralmayı destekliyor mu?

Hayır, ancak üst klasör altında yeni oluşturulan alt dosyalara ve klasöre yönelik ACL’yi ayarlamak için Varsayılan ACL’ler kullanılabilir.

### <a name="what-are-the-limits-for-acl-entries-on-files-and-folders"></a>Dosya ve klasörlerdeki ACL girdilerinin sınırları nelerdir?

32 ACL 'Ler dosya ve Dizin başına ayarlanabilir. Erişim ve varsayılan ACL 'Lerin her biri kendi 32 ACL girdisi sınırına sahiptir. Mümkünse ACL atamaları için güvenlik gruplarını kullanın. Grupları kullanarak, dosya veya dizin başına en fazla ACL girişi sayısını aşmanız daha az olabilir.

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>POSIX erişim denetimi modeli hakkında daha fazla bilgiyi nereden bulabilirim?

* [Linux üzerinde POSIX Erişim Denetim Listeleri](https://www.linux.com/news/posix-acls-linux)
* [Bir izin Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)
* [POSIX SSS](https://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](https://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1 2013](https://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)
* [POSIX 1003.1 2016](https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)
* [Ubuntu üzerinde POSIX ACL](https://help.ubuntu.com/community/FilePermissionsACLs)
* [Linux üzerinde erişim denetim listelerini kullanan ACL](https://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Data Lake Storage 1. genel bakış](data-lake-store-overview.md)
