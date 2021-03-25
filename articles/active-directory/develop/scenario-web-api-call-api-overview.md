---
title: Erstellen einer Web-API, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine Web-API erstellen, die Downstream-Web-APIs aufruft (Übersicht).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/03/2021
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 376c61f6a5ba94492cac26950465c61e3d8fe4ed
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102038559"
---
# <a name="scenario-a-web-api-that-calls-web-apis"></a>Szenario: Web-API, die Web-APIs aufruft

Hier erfahren Sie, wie Sie eine Web-API erstellen, die Web-APIs aufruft.

## <a name="prerequisites"></a>Voraussetzungen

Dieses Szenario, in dem eine geschützte Web-API andere Web-APIs aufruft, baut auf dem [Szenario: Geschützte Web-API](scenario-protected-web-api-overview.md) auf.

## <a name="overview"></a>Übersicht

- Ein Web-, Desktop-, Mobil- oder SPA-Client (Single-Page Application, Single-Page-Webanwendung), der nicht im zugehörigen Diagramm dargestellt ist, ruft eine geschützte Web-API auf und stellt in seinem HTTP-Header „Authorization“ (Autorisierung) ein JWT-Bearertoken (JSON Web Token) bereit.
- Die geschützte Web-API überprüft das Token und verwendet die Methode `AcquireTokenOnBehalfOf` der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL), um ein weiteres Token von Azure Active Directory (Azure AD) anzufordern, sodass die geschützte Web-API im Auftrag des Benutzers eine zweite Web-API oder eine Downstream-Web-API aufrufen kann. Durch `AcquireTokenOnBehalfOf` wird das Token bei Bedarf aktualisiert.
![Diagramm einer Web-API, die eine Web-API aufruft](media/scenarios/web-api.svg)

## <a name="specifics"></a>Besonderheiten

Der auf die API-Berechtigungen bezogene Teil der App-Registrierung ist klassisch. Die App-Konfiguration beinhaltet die Verwendung des OBO-Flusses (On-Behalf-Of) von OAuth 2.0, um das JWT-Bearertoken gegen ein Token für eine Downstream-API auszutauschen. Dieses Token wird zum Tokencache hinzugefügt, wo es in den Controllern der Web-API verfügbar ist, und kann dann automatisch ein Token abrufen, um Downstream-APIs aufzurufen.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [App-Registrierung](scenario-web-api-call-api-app-registration.md).
