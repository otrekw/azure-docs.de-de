---
title: include file
description: include file
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: ae9b749f62dc74a6cb0dfea3701fd4ebac11c188
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107931655"
---
## <a name="enable-logging"></a>Aktivieren der Protokollierung

Zum Debuggen und für die Problembehandlung von Authentifizierungsfehlern stellt MSAL eine integrierte Unterstützung für die Protokollierung bereit. Die Protokollierung in den einzelnen Bibliotheken wird in den folgenden Artikeln behandelt:

:::row:::
    :::column:::
        - [Protokollierung in MSAL.NET](../articles/active-directory/develop/msal-logging-dotnet.md)
        - [Protokollierung in MSAL für Android](../articles/active-directory/develop/msal-logging-android.md)
        - [Protokollierung in MSAL.js](../articles/active-directory/develop/msal-logging-js.md)
    :::column-end:::
    :::column:::
        - [Protokollierung in MSAL für iOS/macOS](../articles/active-directory/develop/msal-logging-ios.md)
        - [Protokollierung in MSAL für Java](../articles/active-directory/develop/msal-logging-java.md)
        - [Protokollierung in MSAL für Python](../articles/active-directory/develop/msal-logging-python.md)
    :::column-end:::
:::row-end:::

Hier einige Vorschläge für Daten, deren Erfassung sich lohnen kann:

- Benutzer bitten bei Problemen möglicherweise um Unterstützung. Eine bewährte Vorgehensweise besteht darin, Protokolle zu erfassen und temporär zu speichern. Geben Sie einen Speicherort an, an den Benutzer die Protokolle hochladen können. MSAL stellt Protokollierungserweiterungen zur Verfügung, mit denen sich detaillierte Authentifizierungsinformationen erfassen lassen.

- Falls Telemetriedaten verfügbar sind, aktivieren Sie die Sammlung von Telemetriedaten mithilfe der MSAL, um zu ermitteln, wie sich Benutzer bei Ihrer App anmelden.


## <a name="validate-your-integration"></a>Überprüfen Ihrer Integration

Testen Sie Ihre Integration anhand der [Checkliste für die Integration von Microsoft Identity Platform](../articles/active-directory/develop/identity-platform-integration-checklist.md).

## <a name="build-for-resilience"></a>Erstellen resilienter Lösungen

Erfahren Sie, wie Sie die Resilienz in Ihrer App erhöhen. Details hierzu finden Sie unter [Erhöhen der Resilienz bei der Authentifizierung und Autorisierung in von Ihnen entwickelten Anwendungen](../articles/active-directory/fundamentals/resilience-app-development-overview.md).
