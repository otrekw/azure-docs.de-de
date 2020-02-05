---
title: Verschieben einer Daemon-App, die Web-APIs aufruft, in die Produktion – Microsoft Identity Platform | Azure
description: Hier erfahren Sie, wie Sie eine Daemon-App, die Web-APIs aufruft, in die Produktion verschieben.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: db5f52c95daf4e93c140b4c93f39dad19971319d
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775204"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Daemon-App, die Web-APIs aufruft (Übergang in die Produktion)

Da Sie nun wissen, wie Sie ein Token für einen Dienst-zu-Dienst-Aufruf abrufen und verwenden, erfahren Sie im Folgenden, wie Sie Ihre App in die Produktion bringen.

## <a name="deployment---multitenant-daemon-apps"></a>Bereitstellung: mehrinstanzenfähige Daemon-Apps

Wenn Sie ein unabhängiger Softwarehersteller sind, der eine Daemon-App erstellt, die in mehreren Mandanten ausgeführt werden kann, müssen Sie sicherstellen, dass der Mandantenadministrator folgende Aufgaben ausführt:

- Er muss einen Dienstprinzipal für die Anwendung bereitstellen.
- Er muss der Anwendung Zustimmung gewähren.

Sie müssen Ihren Kunden erklären, wie diese Vorgänge durchgeführt werden. Weitere Informationen finden Sie im Abschnitt zum [Anfordern der Zustimmung für einen gesamten Mandanten](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

Im Folgenden finden Sie Links zu weiteren Informationen:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

- Schnellstart: [Abrufen eines Tokens und Aufrufen der Microsoft Graph-API über eine Konsolen-App anhand der Identität der App](./quickstart-v2-netcore-daemon.md).
- In der Dokumentation finden Sie Informationen zum:
  - Instanziieren von [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder).
  - Aufrufen von [AcquireTokenForClient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder).
- Weitere Beispiele und Tutorials:
  - [microsoft-identity-platform-console-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) ist eine einfache .NET Core-Daemon-Konsolenanwendung, die die Benutzer eines Mandanten anzeigt, die Microsoft Graph abfragen.

    ![Beispieltopologie für Daemon-App](media/scenario-daemon-app/daemon-app-sample.svg)

    Dasselbe Beispiel zeigt auch eine Variante mit Zertifikaten:

    ![Beispieltopologie für Daemon-App: Zertifikate](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [microsoft-identity-platform-aspnet-webapp-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) ist eine ASP.NET MVC-Webanwendung, die Daten von Microsoft Graph mithilfe der Identität der Anwendung (statt im Namen eines Benutzers) synchronisiert. Das Beispiel veranschaulicht auch den Vorgang für die Administratoreinwilligung.

    ![Topologie](media/scenario-daemon-app/damon-app-sample-web.svg)

# <a name="pythontabpython"></a>[Python](#tab/python)

Arbeiten Sie den Schnellstart [Abrufen eines Tokens und Aufrufen der Microsoft Graph-API über eine Python-Konsolen-App anhand der Identität einer App](./quickstart-v2-python-daemon.md) durch.

# <a name="javatabjava"></a>[Java](#tab/java)

MSAL Java ist derzeit als öffentliche Vorschauversion verfügbar. Weitere Informationen finden Sie in den [MSAL Java-Dev-Beispielen](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples).

---
