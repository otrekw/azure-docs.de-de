---
title: Was ist Azure Static Web Apps?
description: Hier finden Sie Informationen zu den wichtigsten Features und Funktionen von Azure Static Web Apps.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 9cd5136d69e4b14aa50a96d20f3187ce88db6e96
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92320489"
---
# <a name="what-is-azure-static-web-apps-preview"></a>Was ist Azure Static Web Apps (Vorschauversion)?

Azure Static Web Apps ist ein Dienst, der auf der Grundlage eines GitHub-Repositorys automatisch umfassende Web-Apps erstellt und in Azure bereitstellt.

:::image type="content" source="media/overview/static-apps-overview.png" alt-text="Übersicht über Static Web Apps":::

Der Workflow von Azure Static Web Apps wurde auf den alltäglichen Workflow von Entwicklern zugeschnitten. Die Erstellung und Bereitstellung von Apps basiert auf GitHub-Interaktionen.

Wenn Sie eine Azure Static Web Apps-Ressource erstellen, wird von Azure im Quellcoderepository der App ein GitHub Actions-Workflow zur Überwachung eines Branchs Ihrer Wahl eingerichtet. Sobald Sie Commits an den überwachten Branch pushen oder Pull Requests für den überwachten Branch akzeptieren, sorgt die GitHub-Aktion automatisch dafür, dass Ihre App und die zugehörige API erstellt und in Azure bereitgestellt werden.

Statische Web-Apps werden in der Regel mithilfe von Bibliotheken und Frameworks wie Angular, React, Svelte, Vue oder Blazor erstellt. Diese Apps enthalten HTML-, CSS-, JavaScript- und Bildressourcen, aus denen sich die Anwendung zusammensetzt. Bei einem herkömmlichen Webserver werden diese Ressourcen von einem einzelnen Server sowie über ggf. erforderliche API-Endpunkte bereitgestellt.

Bei Static Web Apps sind statische Ressourcen von einem herkömmlichen Webserver getrennt und werden stattdessen über Punkte bereitgestellt, die auf der ganzen Welt verteilt sind. Dank dieser Verteilung können Dateien deutlich schneller bereitgestellt werden, da sie dem Endbenutzer physisch näher sind. Darüber hinaus werden API-Endpunkte mithilfe einer [serverlosen Architektur](../azure-functions/functions-overview.md) gehostet, wodurch kein vollwertiger Back-End-Server mehr benötigt wird.

## <a name="key-features"></a>Wichtige Features

- **Webhosting** statischer Inhalte wie HTML-, CSS-, JavaScript- und Bildressourcen.
- **Integrierte API** (unterstützt durch Azure Functions)
- **GitHub-Erstanbieterintegration** mit Auslösung von Build- und Bereitstellungsvorgängen bei Repositoryänderungen
- **Global verteilte** statische Inhalte, um die Entfernung zwischen Inhalten und Benutzern zu verringern
- **Kostenlose SSL-Zertifikate** mit automatischer Verlängerung
- **Benutzerdefinierte Domänen** für App-Anpassungen mit Branding
- **Nahtloses Sicherheitsmodell** mit einem Reverseproxy beim Aufrufen von APIs ganz ohne CORS-Konfiguration
- **Integration von Authentifizierungsanbietern** mit Azure Active Directory, Facebook, Google, GitHub und Twitter
- **Anpassbare Autorisierungsrollendefinition** und -zuweisungen
- **Back-End-Routingregeln** zur vollständigen Kontrolle über die verwendeten Inhalte und Routen
- **Generierte Stagingversionen** auf der Grundlage von Pull Requests, was die Verwendung von Vorschauversionen Ihrer Website ermöglicht, bevor sie veröffentlicht wird

## <a name="what-you-can-do-with-static-web-apps"></a>Verwendungsmöglichkeiten von Static Web Apps

- **Erstellen moderner Webanwendungen** mit JavaScript-Frameworks und -Bibliotheken wie [Angular](getting-started.md?tabs=angular), [React](getting-started.md?tabs=react), [Svelte](/learn/modules/publish-app-service-static-web-app-api/) und [Vue](getting-started.md?tabs=react) oder unter Verwendung von [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor), um Webassemblyanwendungen mit [Azure Functions](apis.md)-Back-End zu erstellen.
- **Veröffentlichen statischer Websites** mit Frameworks wie [Gatsby](publish-gatsby.md), [Hugo](publish-hugo.md), [VuePress](publish-vuepress.md)
- **Bereitstellen von Webanwendungen** mit Frameworks wie [Next.js](deploy-nextjs.md) and [Nuxt.js](deploy-nuxtjs.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen Ihrer ersten statischen Web-App](getting-started.md)