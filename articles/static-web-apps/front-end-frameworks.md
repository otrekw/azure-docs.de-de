---
title: Konfigurieren von Front-End-Frameworks mit Azure Static Web Apps (Vorschauversion)
description: Einstellungen für beliebte Front-End-Frameworks, die für Azure Static Web Apps erforderlich sind
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 07/18/2020
ms.author: cshoe
ms.openlocfilehash: 4b1bc58b6b4a87cd6e5e09e83020a38261b8746f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90905373"
---
# <a name="configure-front-end-frameworks-and-libraries-with-azure-static-web-apps-preview"></a>Konfigurieren von Front-End-Frameworks und Bibliotheken mit Azure Static Web Apps (Vorschauversion)

Azure Static Web Apps erfordert, dass Sie die entsprechenden Konfigurationswerte in der [Buildkonfigurationsdatei](github-actions-workflow.md) für Ihr Front-End-Framework oder Ihre Bibliothek besitzen.

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle listet die Einstellungen für eine Reihe von Frameworks und Bibliotheken auf<sup>1</sup>.

Die Absicht der Tabellenspalten wird durch die folgenden Punkte erläutert:

- **Speicherort für App-Artefakte**: Listet den Wert für `app_artifact_location` auf, dem [Ordner für erstellte Versionen von Anwendungsdateien](github-actions-workflow.md#build-and-deploy).

- **Benutzerdefinierter Buildbefehl**: Wenn das Framework einen anderen Befehl als `npm run build` oder `npm run azure:build` erfordert, können Sie einen [benutzerdefinierten Buildbefehl](github-actions-workflow.md#custom-build-commands) definieren.

| Framework | Speicherort für App-Artefakte | Benutzerdefinierter Buildbefehl |
|--|--|--|
| [Alpine.js](https://github.com/alpinejs/alpine/) | `/` | Nicht zutreffend <sup>2</sup> |
| [Angular](https://angular.io/) | `dist/<APP_NAME>` | `npm run build -- --prod` |
| [Angular Universal](https://angular.io/guide/universal) | `dist/<APP_NAME>/browser` | `npm run prerender` |
| [Aurelia](https://aurelia.io/) | `dist` | – |
| [Backbone.js](https://backbonejs.org/) | `/` | – |
| [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) | `wwwroot` | – |
| [Ember](https://emberjs.com/) | `dist` | – |
| [Flutter](https://flutter.dev/) | `build/web` | `flutter build web` |
| [Framework7](https://framework7.io/) | `www` | `npm run build-prod` |
| [Glimmer](https://glimmerjs.com/) | `dist` | – |
| [HTML](https://developer.mozilla.org/docs/Web/HTML) | `/` | – |
| [Hyperapp](https://hyperapp.dev/) | `/` | – |
| [JavaScript](https://developer.mozilla.org/docs/Web/javascript) | `/` | – |
| [jQuery](https://jquery.com/) | `/` | – |
| [KnockoutJS](https://knockoutjs.com/) | `dist` | – |
| [LitElement](https://lit-element.polymer-project.org/) | `dist` | – |
| [Marko](https://markojs.com/) | `public` | – |
| [Meteor](https://www.meteor.com/) | `bundle` | – |
| [Mithril](https://mithril.js.org/) | `dist` | – |
| [Polymer](https://www.polymer-project.org/) | `build/default` | – |
| [Preact](https://preactjs.com/) | `build` | – |
| [React](https://reactjs.org/) | `build` | – |
| [Stencil](https://stenciljs.com/) | `www` | – |
| [Svelte](https://svelte.dev/) | `public` | – |
| [Three.js](https://threejs.org/) | `/` | – |
| [TypeScript](https://www.typescriptlang.org/) | `dist` | – |
| [Vue.js](https://vuejs.org/) | `dist` | – |

<sup>1</sup> Die obige Tabelle ist nicht als umfassende Liste von Frameworks und Bibliotheken gedacht, die mit Azure Static Web Apps arbeiten.

<sup>2</sup> Nicht zutreffend

## <a name="next-steps"></a>Nächste Schritte

- [Build- und Workflowkonfiguration](github-actions-workflow.md)
