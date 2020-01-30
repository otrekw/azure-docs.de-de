---
title: Überführen einer Single-Page-App in die Produktion – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Single-Page-Webanwendung (Übergang in die Produktion) erstellen.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 01b923e0d013fab1815456e55eac6036ded772bb
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701840"
---
# <a name="single-page-application-move-to-production"></a>Single-Page-Webanwendung: Überführen in die Produktion

Nachdem Sie nun wissen, wie Sie ein Token abrufen, um Web-APIs aufzurufen, erfahren Sie in diesem Artikel, wie Ihnen der Übergang in die Produktion gelingt.

## <a name="improve-your-app"></a>Verbessern Ihrer App

[Aktivieren Sie die Protokollierung](msal-logging.md), damit Ihre App für die Produktion bereit ist.

## <a name="test-your-integration"></a>Testen Ihrer Integration

Testen Sie Ihre Integration anhand der [Checkliste für die Integration von Microsoft Identity Platform](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Behandlung des Schnellstartbeispiels, wobei der Code zum Anmelden von Benutzern und Abrufen eines Zugriffstokens zum Aufrufen der Microsoft Graph-API mit MSAL.js beschrieben wird.

> [!div class="nextstepaction"]
> [JavaScript-SPA-Tutorial](./tutorial-v2-javascript-spa.md)

Ein Beispiel, in dem veranschaulicht wird, wie Sie mit MSAL.js Token für Ihre eigene Back-End-Web-API abrufen können:

> [!div class="nextstepaction"]
> [SPA mit einem ASP.NET-Back-End](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

Beispiel für die Verwendung von MSAL.js zum Anmelden von Benutzern bei einer App, die bei Azure Active Directory B2C (Azure AD B2C) registriert ist:

> [!div class="nextstepaction"]
> [SPA mit Azure AD B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)
