---
title: Identitätsanbieter für externe Identitäten – Azure AD
description: Erfahren Sie, wie Sie Azure AD als Standardidentitätsanbieter für die Freigabe für externe Benutzer verwenden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42ed42a1fc9a2750cc928b5fd03eb4b32f770276
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644084"
---
# <a name="identity-providers-for-external-identities"></a>Identitätsanbieter für externe Identitäten

Ein *Identitätsanbieter* erstellt und verwaltet die Identitätsinformationen und stellt gleichzeitig Authentifizierungsdienste für Anwendungen bereit. Wenn Sie Ihre Apps und Ressourcen für externe Benutzer freigeben, ist Azure AD der Standardidentitätsanbieter für die Freigabe. Dies bedeutet Folgendes: Wenn Sie externe Benutzer einladen, die bereits über ein Azure AD- oder Microsoft-Konto verfügen, können sich diese Benutzer automatisch anmelden, ohne dass Sie weitere Konfigurationsschritte ausführen müssen.

Sie können Benutzern jedoch die Anmeldung mit verschiedenen Identitätsanbietern ermöglichen.

- **Google**: Mit dem Google-Verbund können externe Benutzer Ihre Einladungen einlösen, indem sie sich mit ihren eigenen Gmail-Konten bei Ihren Apps anmelden. Der Google-Verbund kann auch in Ihren Benutzerflows mit Self-Service-Registrierung verwendet werden.
   > [!IMPORTANT]
   > **Am 4. Januar 2021** wird Google [die Unterstützung für die WebView-Anmeldung einstellen](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html). Wenn Sie einen Google-Verbund oder die Self-Service-Registrierung mit Gmail verwenden, sollten Sie [Ihre nativen Branchenanwendungen auf Kompatibilität testen](google-federation.md#deprecation-of-webview-sign-in-support).

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
