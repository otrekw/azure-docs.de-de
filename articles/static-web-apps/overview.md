---
title: Was ist Azure Static Web Apps?
description: Hier finden Sie Informationen zu den wichtigsten Features und Funktionen von Azure Static Web Apps.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 04/01/2021
ms.author: cshoe
ms.openlocfilehash: c0a4156fb0015c6d319700c82f6285dbf0ed1443
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107947877"
---
# <a name="what-is-azure-static-web-apps-preview"></a>Was ist Azure Static Web Apps (Vorschauversion)?

Azure Static Web Apps ist ein Dienst, der auf der Grundlage eines Coderepositorys automatisch umfassende Web-Apps erstellt und in Azure bereitstellt.

:::image type="content" source="media/overview/azure-static-web-apps-overview.png" alt-text="Übersichtsdiagramm für Azure Static Web Apps":::

Der Workflow von Azure Static Web Apps wurde auf den alltäglichen Workflow von Entwicklern zugeschnitten. Die Erstellung und Bereitstellung von Apps basiert auf Codeänderungen.

Wenn Sie eine Azure Static Web Apps-Ressource erstellen, interagiert Azure direkt mit GitHub oder Azure DevOps, um einen Branch Ihrer Wahl zu überwachen. Sobald Sie Commits an den überwachten Branch pushen oder Pull Requests für den überwachten Branch akzeptieren, wird automatisch ein Build ausgeführt, und Ihre App und die API werden in Azure bereitgestellt.

Statische Web-Apps werden in der Regel mithilfe von Bibliotheken und Frameworks wie Angular, React, Svelte, Vue oder Blazor erstellt, für die kein serverseitiges Rendering erforderlich ist. Diese Apps enthalten HTML-, CSS-, JavaScript- und Bildressourcen, aus denen sich die Anwendung zusammensetzt. Bei einem herkömmlichen Webserver werden diese Ressourcen von einem einzelnen Server sowie über ggf. erforderliche API-Endpunkte bereitgestellt.

Bei Static Web Apps sind statische Ressourcen von einem herkömmlichen Webserver getrennt und werden stattdessen über Punkte bereitgestellt, die auf der ganzen Welt verteilt sind. Dank dieser Verteilung können Dateien deutlich schneller bereitgestellt werden, da sie dem Endbenutzer physisch näher sind. Darüber hinaus werden API-Endpunkte mithilfe einer [serverlosen Architektur](../azure-functions/functions-overview.md) gehostet, wodurch kein vollwertiger Back-End-Server mehr benötigt wird.

## <a name="key-features"></a>Wichtige Features

- **Webhosting** statischer Inhalte wie HTML-, CSS-, JavaScript- und Bildressourcen.
- **Integrierte API** (unterstützt durch Azure Functions)
- **GitHub- und Azure DevOps-Erstanbieterintegration** mit Auslösung von Build- und Bereitstellungsvorgängen bei Repositoryänderungen
- **Global verteilte** statische Inhalte, um die Entfernung zwischen Inhalten und Benutzern zu verringern
- **Kostenlose SSL-Zertifikate** mit automatischer Verlängerung
- **Benutzerdefinierte Domänen** für App-Anpassungen mit Branding
- **Nahtloses Sicherheitsmodell** mit einem Reverseproxy beim Aufrufen von APIs ganz ohne CORS-Konfiguration
- **Integration von Authentifizierungsanbietern** mit Azure Active Directory, Facebook, Google, GitHub und Twitter
- **Anpassbare Autorisierungsrollendefinition** und -zuweisungen
- **Back-End-Routingregeln** zur vollständigen Kontrolle über die verwendeten Inhalte und Routen
- **Generierte Stagingversionen** auf der Grundlage von Pull Requests, was die Verwendung von Vorschauversionen Ihrer Website ermöglicht, bevor sie veröffentlicht wird

## <a name="what-you-can-do-with-static-web-apps"></a>Verwendungsmöglichkeiten von Static Web Apps

- **Erstellen moderner Webanwendungen** mit JavaScript-Frameworks und -Bibliotheken wie [Angular](getting-started.md?tabs=angular), [React](getting-started.md?tabs=react), [Svelte](/learn/modules/publish-app-service-static-web-app-api/) und [Vue](getting-started.md?tabs=vue) oder unter Verwendung von [Blazor](./deploy-blazor.md), um Webassemblyanwendungen mit [Azure Functions](apis.md)-Back-End zu erstellen.
- **Veröffentlichen statischer Websites** mit Frameworks wie [Gatsby](publish-gatsby.md), [Hugo](publish-hugo.md), [VuePress](publish-vuepress.md)
- **Bereitstellen von Webanwendungen** mit Frameworks wie [Next.js](deploy-nextjs.md) and [Nuxt.js](deploy-nuxtjs.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen Ihrer ersten statischen Web-App](getting-started.md)
