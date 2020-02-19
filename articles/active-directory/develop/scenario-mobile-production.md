---
title: Vorbereiten von mobilen Apps auf die Produktion, die Web-APIs aufrufen | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft. (Vorbereiten von Apps auf die Produktion.)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1ea19b8b76f4eb4a2c984f0e39eb0fd373c8b83c
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132391"
---
# <a name="prepare-mobile-apps-for-production"></a>Vorbereiten von mobilen Apps auf die Produktion

In diesem Artikel wird beschrieben, wie Sie die Qualität und Zuverlässigkeit Ihrer mobilen App erhöhen, bevor Sie diese in die Produktion überführen.

## <a name="handle-errors"></a>Fehlerbehandlung

Beim Vorbereiten von mobilen Apps für die Produktion können verschiedene Fehlerbedingungen auftreten. In den Hauptszenarien werden Sie sich mit verborgenen (stillen) Ausfällen und dem Ausweichen auf eine Interaktion befassen. Weitere Fehlerzustände sind Netzwerkstörungen, Dienstausfälle, fehlende Administratoreinwilligungen und weitere Fälle, die auf das jeweilige Szenario bezogen sind.

Für jeden MSAL-Typ (Microsoft Authentication Library, Microsoft Authentifizierungsbibliothek) können Sie Beispielcode und Wiki-Inhalte finden, die beschreiben, wie Fehlerbedingungen zu behandeln sind:

- [Android-Wiki für MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [iOS-Wiki für MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [Wiki für MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigate-and-investigate-issues"></a>Entschärfen und Untersuchen von Problemen

Sammeln Sie Daten, um Probleme in Ihrer App besser zu diagnostizieren. Informationen über die Art von Daten, die Sie sammeln können, finden Sie unter [Protokollierung in MSAL-Anwendungen](https://docs.microsoft.com/azure/active-directory/develop/msal-logging).

Hier einige Vorschläge für Daten, deren Erfassung sich lohnen kann:

- Benutzer bitten bei Problemen möglicherweise um Unterstützung. Eine bewährte Vorgehensweise besteht darin, Protokolle zu erfassen und temporär zu speichern. Geben Sie einen Speicherort an, an den Benutzer die Protokolle hochladen können. MSAL stellt Protokollierungserweiterungen zur Verfügung, mit denen sich detaillierte Authentifizierungsinformationen erfassen lassen.

- Falls Telemetriedaten verfügbar sind, aktivieren Sie die Sammlung von Telemetriedaten mithilfe der MSAL, um zu ermitteln, wie sich Benutzer bei Ihrer App anmelden.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

Weitere Beispiele zum Testen finden Sie unter [Öffentliche Desktop- und mobile Client-Apps](sample-v2-code.md#desktop-and-mobile-public-client-apps).
