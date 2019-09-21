---
title: Terraytest kullanarak Azure 'da Teraform modüllerini test etme
description: Terraform modüllerinizi test etmek için Terratest’i kullanmayı öğrenin.
services: terraform
ms.service: azure
keywords: terraform, devops, depolama hesabı, azure, terratest, birim testi, tümleştirme testi
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 637bb01bff625989e392d5d711ebd5cdef5c0e09
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169638"
---
# <a name="test-terraform-modules-in-azure-by-using-terratest"></a>Terraytest kullanarak Azure 'da Teraform modüllerini test etme

> [!NOTE]
> Bu makaledeki örnek kod sürüm 0,12 (ve üzeri) ile çalışmaz.

Azure Terrayform modüllerini yeniden kullanılabilir, birleştirilebilir ve test edilebilir bileşenler oluşturmak için kullanabilirsiniz. Terırform modülleri, kod işlemi olarak altyapıyı uygulama konusunda yararlı olan kapsülleme dahil ediyor.

Terrayform modülleri oluştururken kalite güvencesi uygulamanız önemlidir. Ne yazık ki, Terrayform modüllerinde birim testlerinin ve tümleştirme testlerinin nasıl yazılacağını açıklayan sınırlı belge mevcuttur. Bu öğretici, [Azure Terkform modüllerimizi](https://registry.terraform.io/browse?provider=azurerm)oluştururken benimsediğimiz bir test altyapısını ve en iyi uygulamaları tanıtır.

En popüler test altyapılarına baktık ve Terlarform modüllerimizi test etmek için kullanılacak [terrampatest](https://github.com/gruntwork-io/terratest) ' i seçtik. Terratest, Go kitaplığı olarak uygulanır. Terraytest, HTTP istekleri yapma ve belirli bir sanal makineye erişmek için SSH kullanma gibi ortak altyapı testi görevlerine yönelik bir yardımcı işlev ve desen koleksiyonu sağlar. Aşağıdaki listede, Terraytest kullanmanın önemli avantajlarından bazıları açıklanmaktadır:

- **Altyapıyı denetlemek için kullanışlı yardımcıları sağlar**. Bu özellik, gerçek altyapınızı gerçek ortamda doğrulamak istediğiniz zamanlar işinize yarar.
- **Klasör yapısı açıkça düzenlenir**. Test çalışmalarınız net bir şekilde düzenlenir ve [Standart Teraform modül klasörü yapısını](https://www.terraform.io/docs/modules/create.html#standard-module-structure)izler.
- **Tüm test çalışmaları go 'da yazılmıştır**. Terrayform kullanan geliştiricilerin çoğu geliştirici geliştiricilerdedir. Bir go geliştiriciyseniz, Terraytest kullanmak için başka bir programlama dili öğrenmeniz gerekmez. Ayrıca, Terraytest 'te test çalışmalarını çalıştırmak için gereken tek bağımlılıklar Go ve terımform ' dur.
- **Altyapı yüksek düzeyde genişletilebilir**. Azure 'a özgü özellikler de dahil olmak üzere Terktest 'in üzerine ek işlevler genişletebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu uygulamalı makale, platformdan bağımsızdır. Windows, Linux veya MacOS 'ta Bu makalede kullandığımız kod örneklerini çalıştırabilirsiniz. 

Başlamadan önce aşağıdaki yazılımları yükleyebilirsiniz:

- **Go programlama dili**: Terrayform test çalışmaları [Go](https://golang.org/dl/)olarak yazılmıştır.
- **dep**: [dep](https://github.com/golang/dep#installation), Go’nun bağımlılık yönetimi aracıdır.
- **Azure CLI**: [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) , Azure kaynaklarını yönetmek için kullanabileceğiniz bir komut satırı aracıdır. (Terrayform, hizmet sorumlusu aracılığıyla veya [Azure CLI aracılığıyla](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html)Azure 'da kimlik doğrulamasını destekler.)
- **Mage**: Terlartest çalışmalarını çalıştırmanın nasıl basitleştireceğinizi göstermek için [Mage yürütülebilirini](https://github.com/magefile/mage/releases) kullanıyoruz. 

## <a name="create-a-static-webpage-module"></a>Statik bir web sayfası modülü oluşturma

Bu öğreticide, Azure depolama blobuna tek bir HTML dosyası yükleyerek statik bir Web sayfası sağlayan bir Teraform modülü oluşturacaksınız. Bu modül, kullanıcıların, modülün döndürdüğü bir URL aracılığıyla Web sayfasına dünya erişimi olmasını sağlar.

> [!NOTE]
> [Gopath](https://github.com/golang/go/wiki/SettingGOPATH) konumunuz altında bu bölümde açıklanan tüm dosyaları oluşturun.

İlk olarak, gopath `staticwebpage` `src` klasörünüz altında adlı yeni bir klasör oluşturun. Bu öğreticinin genel klasör yapısı aşağıdaki örnekte gösterilmiştir. Yıldız `(*)` işaretiyle işaretlenen dosyalar bu bölümde birincil odadır.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

Statik Web sayfası modülü üç girişi kabul eder. Girişler şu şekilde `./variables.tf`bildirilmiştir:

```hcl
variable "location" {
  description = "The Azure region in which to create all resources."
}

variable "website_name" {
  description = "The website name to use to create related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static home page HTML in your local file system."
  default     = "index.html"
}
```

Makalenin önceki kısımlarında belirtildiği gibi, bu modül ayrıca içinde `./outputs.tf`bildirilen bir URL 'yi de verir:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Modülün ana mantığı dört kaynak sağlar:
- **kaynak grubu**: Kaynak grubunun `website_name` adı tarafından `-staging-rg`eklenen giriştir.
- **depolama hesabı**: Depolama hesabının `website_name` adı, tarafından `data001`eklenen giriştir. Depolama hesabının AD kısıtlamalarına bağlı kalmak için, modül tüm özel karakterleri kaldırır ve tüm depolama hesabı adındaki küçük harfleri kullanır.
- **ad kapsayıcısı düzeltildi**: Kapsayıcı adlandırılır `wwwroot` ve depolama hesabında oluşturulur.
- **tek HTML dosyası**: HTML dosyası `html_path` girişten okundu ve konumuna `wwwroot/index.html`yüklendi.

Statik web sayfası modülünün mantığı `./main.tf` içinde uygulanır:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = "${var.location}"
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = "${azurerm_resource_group.main.name}"
  location                 = "${azurerm_resource_group.main.location}"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = "${azurerm_resource_group.main.name}"
  storage_account_name  = "${azurerm_storage_account.main.name}"
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = "${azurerm_resource_group.main.name}"
  storage_account_name   = "${azurerm_storage_account.main.name}"
  storage_container_name = "${azurerm_storage_container.main.name}"
  source                 = "${var.html_path}"
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Birim testi

Terraytest, tümleştirme testleri için tasarlanmıştır. Bu amaçla, Terktest gerçek bir ortamda gerçek kaynakları sağlar. Bazen, özellikle de temin edilecek çok sayıda kaynağınız olduğunda, tümleştirme test işleri çok büyük olabilir. Önceki bölümde başvurduğumuz depolama hesabı adlarını dönüştüren mantık iyi bir örnektir. 

Ancak, gerçekten herhangi bir kaynak sağlanması gerekmez. Yalnızca adlandırma dönüştürme mantığının doğru olduğundan emin olmak istiyoruz. Terktest esnekliği sayesinde birim testlerini kullanabiliriz. Birim testleri yerel çalışan test çalışmalardır (ancak internet erişimi zorunlu olsa da). Birim test çalışmaları yürütülür `terraform init` ve `terraform plan` bu çıktıyı `terraform plan` ayrıştırmaya yönelik komutlar ve Karşılaştırılacak öznitelik değerlerini arar.

Bu bölümün geri kalanında, depolama hesabı adlarını dönüştürmek için kullanılan mantığın doğru olduğundan emin olmak üzere bir birim testi uygulamak için Terraytest kullanma yöntemleri açıklanmaktadır. Yalnızca bir yıldız `(*)`işaretiyle işaretlenmiş dosyalarda ilgileniyoruz.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

İlk olarak, yer tutucu olarak adlandırılan `./test/fixtures/storage-account-name/empty.html` boş bir HTML dosyası kullanıyoruz.

Dosya `./test/fixtures/storage-account-name/main.tf` , test çalışması çerçevesidir. Aynı zamanda birim testlerinin girişi `website_name`olan bir girişi kabul eder. Logic aþaðýda gösterilmektedir:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = "${var.website_name}"
  html_path    = "empty.html"
}
```

Ana bileşen, içindeki `./test/storage_account_name_unit_test.go`birim testlerinin uygulamasıdır.

Geliştiriciler, büyük olasılıkla birim testinin, türünde `*testing.T`bir bağımsız değişken kabul ederek klasik bir go test işlevinin imzasıyla eşleşip eşleşmediğini fark eder.

Birim testinin gövdesinde, değişkende `testCases` (`key` giriş olarak ve `value` beklenen çıkış olarak) tanımlanan toplam beş durum vardır. Her birim test çalışması için önce test armatürü klasörünü `terraform init` (`./test/fixtures/storage-account-name/`) çalıştırıp hedefleyin. 

Ardından, belirli `terraform plan` test çalışması girişi kullanan bir komut (' de `website_name` `tfOptions`tanımına göz atın) sonucu `./test/fixtures/storage-account-name/terraform.tfplan` kaydeder (genel klasör yapısında listelenmez).

Bu sonuç dosyası resmi Terrampaplan ayrıştırıcısı kullanılarak kod okunabilir bir yapıya ayrıştırılır.

Şimdi ilgilendiğimiz öznitelikleri (Bu durumda, ' `name` `azurerm_storage_account`nin) inceleyeceğiz ve sonuçları beklenen çıktıyla karşılaştırıyoruz:

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify the test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions.Vars, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Birim testlerini çalıştırmak için komut satırında aşağıdaki adımları gerçekleştirin:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables are present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

Geleneksel go test sonucu yaklaşık bir dakika içinde döndürülür.

### <a name="integration-test"></a>Tümleştirme testi

Birim testlerinin aksine, tümleştirme testlerinin, uçtan uca bir perspektifle gerçek bir ortama kaynak sağlaması gerekir. Terraytest, bu tür bir görevle iyi bir iş yapar. 

Terrayform modülleri için en iyi uygulamalar, `examples` klasörü yüklemeyi içerir. Klasör `examples` , bazı uçtan uca örnekler içerir. Gerçek verilerle çalışmayı önlemek için bu örnekleri tümleştirme testleri olarak test etme. Bu bölümde, aşağıdaki klasör yapısında bir yıldız `(*)` işaretiyle işaretlenmiş üç dosyaya odaklanıyoruz:

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Örneklerle başlayalım. `./examples/` Klasöründe adlı `hello-world/` yeni bir örnek klasör oluşturulur. Burada, karşıya yüklenecek basit bir HTML sayfası sağlıyoruz: `./examples/hello-world/index.html`.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demonstrate Terratest.</p>
</body>
</html>
```

Terrayform örneği `./examples/hello-world/main.tf` , birim testinde gösterilenle benzerdir. Önemli bir farklılık vardır: örnek, karşıya yüklenen HTML 'nin URL 'sini de adlı `homepage`bir Web sayfası olarak yazdırır.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = "${var.website_name}"
}

output "homepage" {
  value = "${module.staticwebpage.homepage_url}"
}
```

Tümleştirme testi dosyasında `./test/hello_world_example_test.go`terraytest ve klasik go test işlevlerini yeniden kullanırız.

Birim testlerinin aksine, tümleştirme testleri Azure 'da gerçek kaynakları oluşturur. Bu nedenle, adlandırma çakışmalarını önlemek için dikkatli olmanız gerekir. (Depolama hesabı adları gibi bazı genel benzersiz adlara özel dikkat ödeyin.) Bu nedenle, test mantığının ilk adımı, terraytest tarafından sunulan `websiteName` `UniqueId()` işlevini kullanarak rastgele bir oluşturma işlemi kullanmaktır. Bu işlev, küçük harf, büyük harf veya sayı içeren bir rastgele ad üretir. `tfOptions``./examples/hello-world/` klasörü hedefleyen tüm terlarform komutlarını yapar. Ayrıca, bunun `websiteName`rastgele ayarlanmış `website_name` olduğundan emin olur.

Ardından birer birer `terraform init`, `terraform apply` ve `terraform output` yürütülür. Terraytest tarafından sunulan başka `HttpGetWithCustomValidation()`bir yardımcı işlevi kullanıyoruz. HTML 'nin tarafından `homepage` `terraform output`döndürülen çıkış URL 'sine yüklendiğinden emin olmak için yardımcı işlevini kullanırız. HTTP get durum kodunu ile `200` karşılaştırdık ve HTML içeriğindeki bazı anahtar sözcükleri arayıyoruz. Son olarak, Go'nun `defer` özelliğinden yararlanılarak `terraform destroy` komutunun yürütülmesi "taahhüt edildi".

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent a naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify the test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output, and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demonstrate Terratest.")
    })
}
```

Tümleştirme testlerini çalıştırmak için komut satırında aşağıdaki adımları gerçekleştirin:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables are present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

Geleneksel go test sonucu yaklaşık iki dakika içinde döndürülür. Ayrıca, bu komutları yürüterek hem birim testlerini hem de tümleştirme testlerini çalıştırabilirsiniz:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

Tümleştirme Testleri birim testlerinden çok daha uzun sürer (beş birim servis talebi için bir dakika ile karşılaştırıldığında bir tümleştirme çalışması için iki dakika). Ancak, bir senaryoda birim testlerini veya tümleştirme testlerini mi kullanacağınızı karardır. Genellikle, Terkform HCL işlevlerini kullanarak karmaşık mantık için birim testleri kullanmayı tercih ediyoruz. Genellikle bir kullanıcının uçtan uca perspektifi için tümleştirme testlerini kullanırız.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Terratest çalışmalarının yürütülmesini basitleştirmek için mage kullanma 

Azure Cloud Shell ' de test çalışmalarını çalıştırmak, kolay bir görev değildir. Farklı dizinlere gitmeniz ve farklı komutlar yürütmeniz gerekir. Cloud Shell kullanmaktan kaçınmak için, projemizdeki derleme sistemini tanıttık. Bu bölümde, iş için bir go derleme sistemi olan Mage kullanacağız.

Mage `magefile.go` 'ın gerektirdiği tek şey projenizin kök dizininde (aşağıdaki örnekte ile `(+)` işaretlenir):

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Bir örneği `./magefile.go`aşağıda verilmiştir. Bu derleme betiğine git ile yazılan beş derleme adımını uyguladık:
- `Clean`: Bu adım, test yürütmeleri sırasında oluşturulan tüm oluşturulan ve geçici dosyaları kaldırır.
- `Format`: Adım çalışır `terraform fmt` ve `go fmt` kod tabanınızı biçimlendirir.
- `Unit`: Adım, `TestUT_*` `./test/` klasörü altındaki tüm birim testlerini (işlev adı kuralını kullanarak) çalıştırır.
- `Integration`: Adım öğesine `Unit`benzerdir, ancak birim testleri yerine, tümleştirme testlerini yürütür (`TestIT_*`).
- `Full`: Adım `Clean` ,`Format`,, ve`Integration` sırayla çalışır. `Unit`

```go
// +build mage

// Build a script to format and run tests of a Terraform module project
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// The default target when the command executes `mage` in Cloud Shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build and test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Tam test paketini yürütmek için aşağıdaki komutları kullanabilirsiniz. Kod, önceki bölümde kullandığımız çalışan adımlara benzerdir. 

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only required when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables are present
GoPath/src/staticwebpage$ mage
```

Son komut satırını ek Mage adımları ile değiştirebilirsiniz. Örneğin, veya `mage unit` `mage clean`kullanabilirsiniz. `dep` Komutları ve`az login` ImageFile içine eklemek iyi bir fikirdir. Kodu burada göstermedik. 

Mage ile, go paket sistemini kullanarak da adımları paylaşabilirsiniz. Bu durumda, yalnızca ortak bir uygulamaya başvurarak ve bağımlılıkları (`mg.Deps()`) bildirerek tüm modüllerinizin tamamında ımagefiles 'ı basitleştirebilirsiniz.

**Seçim Kabul testlerini çalıştırmak için hizmet sorumlusu ortam değişkenlerini ayarlama**
 
Testlerden önce yürütmek `az login` yerine, hizmet sorumlusu ortam değişkenlerini ayarlayarak Azure kimlik doğrulamasını tamamlayabilirsiniz. Terrayform [, ortam değişkeni adlarının bir listesini](https://www.terraform.io/docs/providers/azurerm/index.html#testing)yayımlar. (Bu ortam değişkenlerinden yalnızca ilk dördü gereklidir.) Terrayform Ayrıca [, bu ortam değişkenlerinin değerinin](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html)nasıl alınacağını açıklayan ayrıntılı yönergeler yayımlar.

## <a name="next-steps"></a>Sonraki adımlar

* Terraytest hakkında daha fazla bilgi için bkz. [Terlartest GitHub sayfası](https://github.com/gruntwork-io/terratest).
* Mage hakkında daha fazla bilgi için [Mage GitHub sayfasına](https://github.com/magefile/mage) ve [Mage Web sitesine](https://magefile.org/)bakın.
