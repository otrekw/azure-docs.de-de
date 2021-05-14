---
title: API-Unterstützung in Azure Static Web Apps mit Azure Functions
description: Hier erfahren Sie, welche API-Features von Azure Static Web Apps unterstützt werden.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 6698be44252e7f7baf935bd80a87e2f04781e25e
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108143483"
---
# <a name="api-support-in-azure-static-web-apps-preview-with-azure-functions"></a>API-Unterstützung in Azure Static Web Apps (Vorschauversion) mit Azure Functions

Von Azure Static Web Apps werden serverlose API-Endpunkte über [Azure Functions](../azure-functions/functions-overview.md) bereitgestellt. Durch die Verwendung von Azure Functions werden APIs bedarfsgerecht dynamisch skaliert und bieten folgende Features:

- **Integrierte Sicherheit** mit direktem Zugriff auf Benutzerdaten für die [Authentifizierung und rollenbasierte Autorisierung](user-information.md)
- **Nahtloses Routing**, durch das die _API-Route_ ganz ohne benutzerdefinierte CORS-Regeln auf sichere Weise für die Web-App verfügbar gemacht wird
- **HTTP-Trigger** und Eingabe- und Ausgabebindungen

## <a name="configuration"></a>Konfiguration

API-Endpunkte sind für die Web-App über die _API-Route_ verfügbar. Diese Route ist zwar vorgegeben, Sie können jedoch entscheiden, in welchem Ordner und Projekt sich die zugeordnete Azure Functions-App befinden soll. Dieser Ort kann durch [Bearbeiten der YAML-Datei des Workflows](github-actions-workflow.md#build-and-deploy) geändert werden, die sich im Ordner _.github/workflows_ Ihres Repositorys befindet.

## <a name="constraints"></a>Einschränkungen

Von Azure Static Web Apps wird eine API über Azure Functions bereitgestellt. Die Funktionen von Azure Functions sind auf eine bestimmte Gruppe von Features konzentriert, mit denen Sie eine API für eine Web-App erstellen können und die es der Web-App ermöglichen, eine sichere Verbindung mit der API herzustellen. Für diese Features gelten gewisse Einschränkungen:

- Das API-Routenpräfix muss _api_ sein.
- Bei der API muss es sich um eine Node.js 12-, .NET Core 3.1- oder um eine Python 3.8-Azure Functions-App handeln.
- Routenregeln für API-Funktionen unterstützen nur [Umleitungen](configuration.md#defining-routes) und das [Sichern von Routen mit Rollen](configuration.md#securing-routes-with-roles).
- Trigger sind auf [HTTP](../azure-functions/functions-bindings-http-webhook.md) beschränkt.
  - [Eingabe- und Ausgabebindungen](../azure-functions/functions-triggers-bindings.md#supported-bindings) werden unterstützt.
- Protokolle sind nur verfügbar, wenn Sie Ihrer Functions-App [Application Insights](../azure-functions/functions-monitoring.md) hinzufügen.
- Einige Anwendungseinstellungen werden durch den Dienst verwaltet. Sie können daher keine App-Einstellungen konfigurieren, die mit den folgenden Präfixen beginnen:
    - `APPSETTING_`
    - `AZUREBLOBSTORAGE_`
    - `AZUREFILESSTORAGE_`
    - `AZURE_FUNCTION_`
    - `CONTAINER_`
    - `DIAGNOSTICS_`
    - `DOCKER_`
    - `FUNCTIONS_`
    - `IDENTITY_`
    - `MACHINEKEY_`
    - `MAINSITE_`
    - `MSDEPLOY_`
    - `SCMSITE_`
    - `SCM_`
    - `WEBSITES_`
    - `WEBSITE_`
    - `WEBSOCKET_`
    - `AzureWeb`

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer API zu Azure Static Web Apps (Vorschauversion) mit Azure Functions](add-api.md)
