---
title: 'Geschützte Web-API: Übersicht'
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen (Übersicht).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 4e530f76c8301dc74f73b675befa6f0710aedab7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026627"
---
# <a name="scenario-protected-web-api"></a>Szenario: Geschützte Web-API

In diesem Szenario erfahren Sie, wie Sie eine Web-API verfügbar machen. Sie erfahren auch, wie Sie die API-App schützen, sodass nur authentifizierte Benutzer darauf zugreifen können.

Sie müssen entweder authentifizierten Benutzern mit Arbeits-, Schul- und Unikonten oder Benutzern mit persönlichen Microsoft-Konten die Nutzung Ihrer Web-API erlauben.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Besonderheiten

Beachten Sie beim Schützen Ihrer Web-APIs die folgenden speziellen Informationen:

- Ihre App-Registrierung muss mindestens einen *Gültigkeitsbereich* oder eine *Anwendungsrolle* verfügbar machen.
  - Gültigkeitsbereiche werden von Web-APIs verfügbar gemacht, die im Namen eines Benutzers aufgerufen werden.
  - Anwendungsrollen werden von Web-APIs verfügbar gemacht, die von Daemonanwendungen aufgerufen werden (die Ihre Web-API in ihrem eigenen Auftrag aufrufen).
- Wenn Sie eine neue Web-API-App-Registrierung erstellen, wählen Sie die [Zugriffstokenversion](reference-app-manifest.md#accesstokenacceptedversion-attribute) aus, die von Ihrer Web-API für `2` akzeptiert wird. Bei älteren Web-APIs kann die akzeptierte Tokenversion `null` lauten, aber dieser Wert schränkt die Anmeldezielgruppe nur auf Organisationen ein, und persönliche Microsoft-Konten (MSA) werden nicht unterstützt.
- Die Codekonfiguration für die Web-API muss das Token überprüfen, das beim Aufrufen der Web-API verwendet wird.
- Der Code in den Controlleraktionen muss die Rollen oder Bereiche im Token überprüfen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [App-Registrierung](scenario-protected-web-api-app-registration.md)
