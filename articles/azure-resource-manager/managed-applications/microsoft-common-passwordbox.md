---
title: PasswordBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. PasswordBox Kullanıcı arabirimi öğesini açıklar. Kullanıcıların yönetilen uygulamaları dağıttığı zaman gizli bir değer sağlamasına olanak sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 7902ea2e30dec20e57d88344dc9507aec3993600
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87064106"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft. Common. PasswordBox Kullanıcı arabirimi öğesi

Bir parolayı sağlamak ve onaylamak için kullanılabilen bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft-common-passwordbox.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"p4ssw0rd"
```

## <a name="remarks"></a>Açıklamalar

- Bu öğe `defaultValue` özelliği desteklemiyor.
- Uygulamasının uygulama ayrıntıları için `constraints` bkz. [Microsoft. Common. TextBox](microsoft-common-textbox.md).
- `options.hideConfirmation` **True** olarak ayarlanırsa, kullanıcının parolasını onaylamak için ikinci metin kutusu gizlenir. Varsayılan değer **false** şeklindedir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
