---
title: Abrufen der Endpunkte für eine App-Registrierung bei Azure AD
titleSuffix: Microsoft identity platform
description: Hier wird beschrieben, wie Sie die Authentifizierungsendpunkte für eine benutzerdefinierte Anwendung ermitteln, die Sie mit Azure AD entwickeln oder bei Azure AD registrieren.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: ryanwi
ms.openlocfilehash: dc37ce474ad3226f5dbf7e6efd687ddf55f7da03
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82926679"
---
# <a name="how-to-discover-endpoints"></a>Ermitteln von Endpunkten

Sie können die Authentifizierungsendpunkte für Ihre Anwendung im [Azure-Portal](https://portal.azure.com) ermitteln.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **Azure Active Directory** aus.
1. Wählen Sie unter **Verwalten** die Option **App-Registrierungen** und dann **Endpunkte** im oberen Menü aus.

    Die Seite **Endpunkte** wird angezeigt, auf der die Authentifizierungsendpunkte für Ihren Mandanten aufgelistet sind.
    
    Verwenden Sie den Endpunkt, der dem Authentifizierungsprotokoll entspricht, das Sie zusammen mit der **Anwendungs-ID (Client-ID)** zum Erstellen der spezifischen Authentifizierungsanforderung für Ihre Anwendung verwenden.

**Nationale Clouds** (z. B. Azure AD China, Deutschland und US Government) verfügen über ein eigenes App-Registrierungsportal und eigene Azure AD-Authentifizierungsendpunkte. Weitere Informationen finden Sie in der [Übersicht über nationale Clouds](authentication-national-cloud.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Endpunkten in den verschiedenen Azure-Umgebungen finden Sie in der [Übersicht über nationale Clouds](authentication-national-cloud.md).
