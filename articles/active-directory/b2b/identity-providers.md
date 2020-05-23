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
ms.openlocfilehash: 395473e70137ead2b7185d54d165f82e9b3f1f04
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595709"
---
# <a name="identity-providers-for-external-identities"></a>Identitätsanbieter für externe Identitäten

Ein *Identitätsanbieter* erstellt und verwaltet die Identitätsinformationen und stellt gleichzeitig Authentifizierungsdienste für Anwendungen bereit. Wenn Sie Ihre Apps und Ressourcen für externe Benutzer freigeben, ist Azure AD der Standardidentitätsanbieter für die Freigabe. Dies bedeutet Folgendes: Wenn Sie externe Benutzer einladen, die bereits über ein Azure AD- oder Microsoft-Konto verfügen, können sich diese Benutzer automatisch anmelden, ohne dass Sie weitere Konfigurationsschritte ausführen müssen.

Sie können Benutzern jedoch die Anmeldung mit verschiedenen Identitätsanbietern ermöglichen. Sie können beispielsweise einen Verbund mit sozialen Netzwerken als Identitätsanbieter einrichten, die von Azure AD unterstützt werden, einschließlich Google und Facebook. Sie können auch einen Verbund mit einem beliebigen externen Identitätsanbieter einrichten, der das SAML- oder WS-Fed-Protokoll unterstützt. Mit dem Verbund für externe Identitätsanbieter können Sie externen Benutzern die Möglichkeit bieten, sich mit ihren vorhandenen Konten für soziale Netzwerke oder Unternehmen bei Ihren Apps anzumelden.

## <a name="how-it-works"></a>Funktionsweise

Azure Active Directory for External Identities ist für den Verbund mit Google und Facebook vorkonfiguriert. Wenn Sie diese Identitätsanbieter in Ihrem Azure AD-Mandanten einrichten möchten, müssen Sie bei jedem Identitätsanbieter eine Anwendung erstellen und Anmeldeinformationen konfigurieren. Sie erhalten eine Client- oder App-ID sowie einen geheimen Client- oder App-Schlüssel, den Sie dann zu Ihrem Azure AD-Mandanten hinzufügen können.

Nachdem Sie Ihrem Azure AD-Mandanten einen Identitätsanbieter hinzugefügt haben, ist Folgendes möglich:

- Wenn Sie einen externen Benutzer zu Apps oder Ressourcen in Ihrer Organisation einladen, kann sich der externe Benutzer mit seinem eigenen Konto dieses Identitätsanbieters anmelden.
- Wenn Sie die [Self-Service-Registrierung](self-service-sign-up-overview.md) für Ihre Apps aktivieren, können sich externe Benutzer mit ihren eigenen Konten der von Ihnen hinzugefügten Identitätsanbieter bei Ihren Apps registrieren. 

Beim Einlösen der Einladung zu oder beim Registrieren bei Ihrer App hat der externe Benutzer die Möglichkeit, sich mit dem sozialen Netzwerk als Identitätsanbieter anzumelden und sich zu authentifizieren:

![Screenshot des Anmeldebildschirms mit den Optionen für Google und Facebook](media/identity-providers/sign-in-with-social-identity.png)

Um für ein optimales Benutzererlebnis bei der Anmeldung zu sorgen, sollten Sie nach Möglichkeit einen Verbund mit Identitätsanbietern einrichten, damit Sie Ihren eingeladenen Gästen eine nahtlose Anmeldung ermöglichen, wenn sie auf Ihre Apps zugreifen.  

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um Informationen zum Hinzufügen von Identitätsanbietern für die Anmeldung bei Ihren Anwendungen zu erhalten:

- [Hinzufügen von Google](google-federation.md) zu Ihrer Liste der sozialen Netzwerke als Identitätsanbieter
- [Hinzufügen von Facebook](facebook-federation.md) zu Ihrer Liste der sozialen Netzwerke als Identitätsanbieter
- [Richten Sie einen direkten Verbund](direct-federation.md) mit jeder Organisation ein, deren Identitätsanbieter das SAML 2.0- oder WS-Fed-Protokoll unterstützt. Für Benutzerflows für die Self-Service-Registrierung ist der direkte Verbund keine Option.
