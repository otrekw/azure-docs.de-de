---
title: Identitätsanbieter für externe Identitäten – Azure AD
description: Azure Active Directory B2B-Zusammenarbeit unterstützt die Multi-Factor Authentication (MFA) für den selektiven Zugriff auf Ihre Unternehmensanwendungen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2284d015b451872753dd0855cac42e6f1926545c
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83712161"
---
# <a name="identity-providers-for-external-identities"></a>Identitätsanbieter für externe Identitäten

Ein *Identitätsanbieter* erstellt und verwaltet die Identitätsinformationen und stellt gleichzeitig Authentifizierungsdienste für Anwendungen bereit. Wenn Sie Ihre Apps und Ressourcen für externe Benutzer freigeben, ist Azure AD der Standardidentitätsanbieter für die Freigabe. Dies bedeutet Folgendes: Wenn Sie externe Benutzer einladen, die bereits über ein Azure AD- oder Microsoft-Konto verfügen, können sich diese Benutzer automatisch anmelden, ohne dass Sie weitere Konfigurationsschritte ausführen müssen.

Sie können Benutzern jedoch die Anmeldung mit verschiedenen Identitätsanbietern ermöglichen.

- **Google**: Mit dem Google-Verbund können externe Benutzer Ihre Einladungen einlösen, indem sie sich mit ihren eigenen Gmail-Konten bei Ihren Apps anmelden. Der Google-Verbund kann auch in Ihren Benutzerflows mit Self-Service-Registrierung verwendet werden.
   > [!NOTE]
   > Wenn in der aktuellen Vorschau der Self-Service-Registrierung ein Benutzerflow mit einer App verknüpft ist und Sie einem Benutzer eine Einladung für diese App senden, kann der Benutzer die Einladung nicht mit einem Gmail-Konto einlösen. Um dieses Problem zu umgehen, kann der Benutzer den Self-Service-Registrierungsprozess durchlaufen. Oder er kann die Einladung einlösen, indem er auf eine andere App zugreift oder das Portal „Meine Apps“ auf https://myapps.microsoft.com verwendet.

- **Facebook**: Beim Entwickeln einer App können Sie die Self-Service-Registrierung konfigurieren und den Facebook-Verbund aktivieren, damit sich Benutzer mit ihren eigenen Facebook-Konten für Ihre App registrieren können. Facebook kann nur für Benutzerflows mit Self-Service-Registrierung verwendet werden und steht nicht als Anmeldeoption zur Verfügung, wenn Benutzer Ihre Einladungen einlösen.

- **Direkter Verbund**: Sie können auch einen direkten Verbund mit einem beliebigen externen Identitätsanbieter einrichten, der das SAML- oder WS-Fed-Protokoll unterstützt. Mit dem direkten Verbund können externe Benutzer Ihre Einladungen einlösen, indem sie sich mit ihren eigenen Social Media- oder Unternehmenskonten bei Ihren Apps anmelden. 
   > [!NOTE]
   > Identitätsanbieter für den direkten Verbund können in Ihren Benutzerflows mit Self-Service-Registrierung nicht verwendet werden.


## <a name="how-it-works"></a>Funktionsweise

Mit der Self-Service-Registrierungsfunktion von Azure AD External Identites können sich Benutzer mit ihrem Azure AD-, Google- oder Facebook-Konto anmelden. Um Social Media-Identitätsanbieter in Ihrem Azure AD-Mandanten einzurichten, erstellen Sie bei jedem Identitätsanbieter eine Anwendung und konfigurieren die Anmeldeinformationen. Sie erhalten eine Client- oder App-ID sowie einen geheimen Client- oder App-Schlüssel, den Sie dann zu Ihrem Azure AD-Mandanten hinzufügen können.

Nachdem Sie Ihrem Azure AD-Mandanten einen Identitätsanbieter hinzugefügt haben, ist Folgendes möglich:

- Wenn Sie einen externen Benutzer zu Apps oder Ressourcen in Ihrer Organisation einladen, kann sich der externe Benutzer mit seinem eigenen Konto dieses Identitätsanbieters anmelden.
- Wenn Sie die [Self-Service-Registrierung](self-service-sign-up-overview.md) für Ihre Apps aktivieren, können sich externe Benutzer mit ihren eigenen Konten der von Ihnen hinzugefügten Identitätsanbieter bei Ihren Apps registrieren.

> [!NOTE]
> Azure AD ist standardmäßig für die Self-Service-Registrierung aktiviert. Daher haben Benutzer immer die Möglichkeit, sich mit einem Azure AD-Konto zu registrieren.

Beim Einlösen der Einladung zu oder beim Registrieren bei Ihrer App hat der externe Benutzer die Möglichkeit, sich mit dem sozialen Netzwerk als Identitätsanbieter anzumelden und sich zu authentifizieren:

![Screenshot des Anmeldebildschirms mit den Optionen für Google und Facebook](media/identity-providers/sign-in-with-social-identity.png)

Um für ein optimales Benutzererlebnis bei der Anmeldung zu sorgen, sollten Sie nach Möglichkeit einen Verbund mit Identitätsanbietern einrichten, damit Sie Ihren eingeladenen Gästen eine nahtlose Anmeldung ermöglichen, wenn sie auf Ihre Apps zugreifen.  

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um Informationen zum Hinzufügen von Identitätsanbietern für die Anmeldung bei Ihren Anwendungen zu erhalten:

- [Hinzufügen von Google](google-federation.md) zu Ihrer Liste der sozialen Netzwerke als Identitätsanbieter
- [Hinzufügen von Facebook](facebook-federation.md) zu Ihrer Liste der sozialen Netzwerke als Identitätsanbieter
- [Richten Sie einen direkten Verbund](direct-federation.md) mit jeder Organisation ein, deren Identitätsanbieter das SAML 2.0- oder WS-Fed-Protokoll unterstützt. Für Benutzerflows für die Self-Service-Registrierung ist der direkte Verbund keine Option.
