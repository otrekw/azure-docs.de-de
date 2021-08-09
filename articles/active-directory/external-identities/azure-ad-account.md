---
title: Azure AD-Kontoidentitätsanbieter
description: Verwenden Sie Azure Active Directory, um es einem externen Benutzer (Gast) zu ermöglichen, sich mit seinem Azure AD-Arbeitskonto bei Ihren Azure AD-Apps anzumelden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d9d7a8e74767982d5089a1308ca36108eb5f106
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111972533"
---
# <a name="azure-active-directory-azure-ad-identity-provider-for-external-identities"></a>Azure Active Directory (Azure AD)-Identitätsanbieter für External Identities

Azure Active Directory ist standardmäßig als Identitätsanbieteroption für die B2B-Zusammenarbeit verfügbar. Wenn ein externer Gastbenutzer ein Azure AD-Konto über ein Arbeits- oder Schulkonto hat, kann er Ihre Einladungen zur B2B-Zusammenarbeit einlösen oder Ihre Benutzerflows für die Registrierung mithilfe des Azure AD-Kontos abschließen.

## <a name="guest-sign-in-using-azure-active-directory-accounts"></a>Gastanmeldung mithilfe von Azure Active Directory-Konten

Die Option „Azure Active Directory“ ist standardmäßig in der Liste der External Identities-Identitätsanbieter verfügbar. Es ist keine weitere Konfiguration erforderlich, damit sich Gastbenutzer mit ihrem Azure AD-Konto über den Einladungsflow oder einen Benutzerflow für die Self-Service-Registrierung anmelden können.

![Azure AD-Konto in der Liste der Identitätsanbieter](media/azure-ad-account/azure-ad-account-identity-provider.png)

### <a name="azure-ad-account-in-the-invitation-flow"></a>Azure AD-Konto im Einladungsflow

Wenn Sie einen [Gastbenutzer zur B2B-Zusammenarbeit einladen](add-users-administrator.md), können Sie sein Microsoft-Konto als E-Mail-Adresse für die Anmeldung angeben.

![Einladen mithilfe eines Azure AD-Kontos](media/azure-ad-account/azure-ad-account-invite.png)

### <a name="azure-ad-account-in-self-service-sign-up-user-flows"></a>Azure AD-Konto in Benutzerflows für die Self-Service-Registrierung

„Azure AD-Konto“ ist einer der verfügbaren Identitätsanbieter für Ihre Benutzerflows für die Self-Service-Registrierung. Benutzer können sich mit ihren eigenen Azure AD-Konten für Ihre Anwendungen registrieren. Zunächst müssen Sie für Ihren Mandanten die [Self-Service-Registrierung aktivieren](self-service-sign-up-user-flow.md). Danach können Sie einen Benutzerflow für die Anwendung einrichten und „Azure Active Directory“ als eine der Anmeldeoptionen auswählen.

![„Azure AD-Konto“ in einem Benutzerflow für die Self-Service-Registrierung](media/azure-ad-account/azure-ad-account-user-flow.png)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von Azure Active Directory B2B Collaboration-Benutzern](add-users-administrator.md)
- [Hinzufügen eines Benutzerflows für die Self-Service-Registrierung zu einer App (Vorschau)](self-service-sign-up-user-flow.md)