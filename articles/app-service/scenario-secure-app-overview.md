---
title: 'Tutorial: Erstellen einer sicheren Web-App in Azure App Service | Azure'
description: In diesem Tutorial wird beschrieben, wie Sie eine Web-App mit Azure App Service erstellen, die Authentifizierung aktivieren, Azure Storage aufrufen und Microsoft Graph aufrufen.
services: active-directory, app-service-web, storage, microsoft-graph
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 04/02/2021
ms.author: ryanwi
ms.reviewer: stsoneff
ms.custom: azureday1
ms.openlocfilehash: 5b94000ce59b3a115e53ecdcd0849b97aae552c5
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106221839"
---
# <a name="tutorial-enable-authentication-in-app-service-and-access-storage-and-microsoft-graph"></a>Tutorial: Aktivieren der Authentifizierung in App Service und Zugreifen auf Speicher und Microsoft Graph

In diesem Tutorial wird ein gängiges Anwendungsszenario beschrieben, in dem Sie Folgendes lernen:

- [Konfigurieren der Authentifizierung für eine Web-App](scenario-secure-app-authentication-app-service.md) und Beschränken des Zugriffs auf Benutzer Ihrer Organisation Siehe A in der Abbildung.
- [Sicheres Zugreifen auf Azure Storage](scenario-secure-app-access-storage.md) für die Web-App mit verwalteten Identitäten Siehe B in der Abbildung.
- Zugreifen auf Daten in Microsoft Graph [für den angemeldeten Benutzer](scenario-secure-app-access-microsoft-graph-as-user.md) oder [für die Webanwendung](scenario-secure-app-access-microsoft-graph-as-app.md) mit verwalteten Identitäten Siehe C in der Abbildung.
- [Bereinigen der Ressourcen](scenario-secure-app-clean-up-resources.md), die Sie in diesem Tutorial erstellt haben

:::image type="content" source="./media/scenario-secure-app-overview/web-app.svg" alt-text="Abbildung mit Anwendungsszenarien in Microsoft Identity Platform" border="false":::

Zunächst wird beschrieben, wie Sie die Authentifizierung für eine Web-App aktivieren.

> [!div class="nextstepaction"]
> [Konfigurieren der Authentifizierung für eine Web-App](scenario-secure-app-authentication-app-service.md)
