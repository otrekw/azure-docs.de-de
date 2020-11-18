---
title: 'Tutorial: Erstellen einer sicheren Web-App in Azure App Service | Azure'
description: In diesem Tutorial wird beschrieben, wie Sie eine Web-App mit Azure App Service erstellen, die Authentifizierung aktivieren und Azure-Speicher und Microsoft Graph aufrufen.
services: active-directory, app-service-web, storage, microsoft-graph
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: fcddf99c3a4c53fe25db1ed653983e8ddac0edb7
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428325"
---
# <a name="tutorial-enable-authentication-in-app-service-and-access-storage-and-microsoft-graph"></a>Tutorial: Aktivieren der Authentifizierung in App Service und Zugreifen auf Speicher und Microsoft Graph

In diesem Tutorial wird ein gängiges Anwendungsszenario beschrieben, um Ihnen Folgendes zu vermitteln:

- [(A) Konfigurieren der Authentifizierung für eine Web-App](scenario-secure-app-authentication-app-service.md) und Beschränken des Zugriffs auf die Benutzer Ihrer Organisation
- [(B) Sicheres Zugreifen auf Azure-Speicher](scenario-secure-app-access-storage.md) im Namen der Webanwendung mit verwalteten Identitäten
- (C) Zugreifen auf Daten in Microsoft Graph [im Namen des angemeldeten Benutzers](scenario-secure-app-access-microsoft-graph-as-user.md) oder [im Namen der Webanwendung](scenario-secure-app-access-microsoft-graph-as-app.md) mit verwalteten Identitäten
- [Bereinigen der Ressourcen](scenario-secure-app-clean-up-resources.md), die Sie in diesem Tutorial erstellt haben

:::image type="content" source="./media/scenario-secure-app-overview/web-app.svg" alt-text="Anwendungsszenarien in Microsoft Identity Platform" border="false":::

Zunächst wird beschrieben, wie Sie die Authentifizierung für eine Web-App aktivieren.

> [!div class="nextstepaction"]
> [Konfigurieren der Authentifizierung für eine Web-App](scenario-secure-app-authentication-app-service.md)
