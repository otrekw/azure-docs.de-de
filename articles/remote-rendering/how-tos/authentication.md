---
title: Authentifizierung
description: Informationen zur Funktionsweise der Authentifizierung
author: florianborn71
ms.author: flborn
ms.date: 02/12/2019
ms.topic: how-to
ms.openlocfilehash: fbc27f2fa3b7517151c4bcdbec5b146e83dd868e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679336"
---
# <a name="configure-authentication"></a>Konfigurieren der Authentifizierung

Azure Remote Rendering verwendet denselben Authentifizierungsmechanismus wie [Azure Spatial Anchors (ASA)](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp). Clients müssen *AccountKey*, *AuthenticationToken* oder *AccessToken* festlegen, damit die REST-APIs erfolgreich aufgerufen werden können. *AccountKey* kann im Azure-Portal auf der Registerkarte „Schlüssel“ des Remote Rendering-Kontos abgerufen werden. *AuthenticationToken* ist ein Azure AD-Token, das mithilfe der [ADAL-Bibliothek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) abgerufen werden kann. *AccessToken* ist ein MR-Token, das vom Sicherheitstokendienst (STS) von Azure Mixed Reality abgerufen werden kann.

## <a name="authentication-for-deployed-applications"></a>Authentifizierung für bereitgestellte Anwendungen

 Die Verwendung von Kontoschlüsseln wird für ein schnelles Onboarding empfohlen, allerdings nur in den Phasen der Entwicklung und der Prototyperstellung. Es wird dringend angeraten, die Anwendung nicht mit einem eingebetteten Kontoschlüssel an die Produktion zu übergeben. Verwenden Sie stattdessen die benutzer- oder dienstbasierten Azure AD-Authentifizierungsverfahren.

 Die Azure AD-Authentifizierung wird im Abschnitt [Azure AD-Benutzerauthentifizierung](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication) im Artikel zum Dienst [Azure Spatial Anchors (ASA)](https://docs.microsoft.com/azure/spatial-anchors/) beschrieben.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Damit Sie die Zugriffsebene für Anwendungen, Dienste oder Azure AD-Benutzer Ihres Diensts besser steuern können, wurden folgende Rollen für Sie erstellt, die Sie nach Bedarf Ihren Azure Remote Rendering-Konten zuweisen können:

* **Remote Rendering-Administrator**: Bietet dem Benutzer Konvertierungs-, Sitzungsverwaltungs-, Rendering- und Diagnosefunktionen für Azure Remote Rendering.
* **Remote Rendering-Client**: Bietet dem Benutzer Sitzungsverwaltungs-, Rendering- und Diagnosefunktionen für Azure Remote Rendering.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Kontos](create-an-account.md)
* [Verwenden der Azure-Front-End-APIs für die Authentifizierung](frontend-apis.md)
* [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
