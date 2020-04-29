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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: cf66757d28a3883664aaacd85baad9cc0dea6956
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537201"
---
# <a name="scenario-protected-web-api"></a>Szenario: Geschützte Web-API

In diesem Szenario erfahren Sie, wie Sie eine Web-API verfügbar machen. Sie erfahren auch, wie Sie die API-App schützen, sodass nur authentifizierte Benutzer darauf zugreifen können.

Sie müssen entweder authentifizierten Benutzern mit Arbeits-, Schul- und Unikonten oder Benutzern mit persönlichen Microsoft-Konten die Nutzung Ihrer Web-API erlauben.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Besonderheiten

Beachten Sie beim Schützen Ihrer Web-APIs die folgenden speziellen Informationen:

- Ihre App-Registrierung muss mindestens einen Gültigkeitsbereich verfügbar machen. Die Tokenversion, die von Ihrer Web-API akzeptiert wird, hängt von der Zielgruppe für die Anmeldung ab.
- Die Codekonfiguration für die Web-API muss das Token überprüfen, das beim Aufrufen der Web-API verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [App-Registrierung](scenario-protected-web-api-app-registration.md)
