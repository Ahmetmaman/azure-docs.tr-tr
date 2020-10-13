---
title: Azure statik Web Apps önizleme ile ön uç çerçeveleri yapılandırma
description: Azure statik Web Apps için gereken popüler ön uç çerçeveleri ayarları
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 07/18/2020
ms.author: cshoe
ms.openlocfilehash: 4b1bc58b6b4a87cd6e5e09e83020a38261b8746f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90905364"
---
# <a name="configure-front-end-frameworks-and-libraries-with-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemesi ile ön uç çerçeveleri ve kitaplıkları yapılandırma

Azure statik Web Apps, ön uç çatısı veya kitaplığınız için [derleme yapılandırma dosyasında](github-actions-workflow.md) uygun yapılandırma değerlerinin olmasını gerektirir.

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tablo, bir dizi çerçeve ve kitaplık için ayarları listeler<sup>1</sup>.

Tablo sütunlarının amacı aşağıdaki öğeler tarafından açıklanmıştır:

- **Uygulama yapıt konumu**: `app_artifact_location` [uygulama dosyalarının oluşturulan sürümlerinin klasörü](github-actions-workflow.md#build-and-deploy)olan için değerini listeler.

- **Özel derleme komutu**: Framework veya öğesinden farklı bir komut gerektirdiğinde `npm run build` `npm run azure:build` , [özel bir yapı komutu](github-actions-workflow.md#custom-build-commands)tanımlayabilirsiniz.

| Framework | Uygulama yapıtı konumu | Özel derleme komutu |
|--|--|--|
| [Alpine.js](https://github.com/alpinejs/alpine/) | `/` | yok <sup>2</sup> |
| [Angular](https://angular.io/) | `dist/<APP_NAME>` | `npm run build -- --prod` |
| [Angular evrensel](https://angular.io/guide/universal) | `dist/<APP_NAME>/browser` | `npm run prerender` |
| [Aurelia](https://aurelia.io/) | `dist` | yok |
| [Backbone.js](https://backbonejs.org/) | `/` | yok |
| [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) | `wwwroot` | yok |
| [Ember](https://emberjs.com/) | `dist` | yok |
| [Flutter](https://flutter.dev/) | `build/web` | `flutter build web` |
| [Framework7](https://framework7.io/) | `www` | `npm run build-prod` |
| [Göz ayırıcı](https://glimmerjs.com/) | `dist` | yok |
| [HTML](https://developer.mozilla.org/docs/Web/HTML) | `/` | yok |
| [Hiper uygulama](https://hyperapp.dev/) | `/` | yok |
| [JavaScript](https://developer.mozilla.org/docs/Web/javascript) | `/` | yok |
| [jQuery](https://jquery.com/) | `/` | yok |
| [KnockoutJS](https://knockoutjs.com/) | `dist` | yok |
| [LitElement](https://lit-element.polymer-project.org/) | `dist` | yok |
| [Marko dili](https://markojs.com/) | `public` | yok |
| [Meteor](https://www.meteor.com/) | `bundle` | yok |
| [Mıthril](https://mithril.js.org/) | `dist` | yok |
| [Polimer](https://www.polymer-project.org/) | `build/default` | yok |
| [Ön işlem](https://preactjs.com/) | `build` | yok |
| [React](https://reactjs.org/) | `build` | yok |
| [Kalıbı](https://stenciljs.com/) | `www` | yok |
| [Svelte](https://svelte.dev/) | `public` | yok |
| [Three.js](https://threejs.org/) | `/` | yok |
| [TypeScript](https://www.typescriptlang.org/) | `dist` | yok |
| [Vue.js](https://vuejs.org/) | `dist` | yok |

<sup>1</sup> yukarıdaki tablo, Azure statik Web Apps birlikte çalışan çerçeve ve kitaplıkların ayrıntılı bir listesi olmak üzere tasarlanmamıştır.

<sup>2</sup> uygulanamaz

## <a name="next-steps"></a>Sonraki adımlar

- [Derleme ve iş akışı yapılandırması](github-actions-workflow.md)
