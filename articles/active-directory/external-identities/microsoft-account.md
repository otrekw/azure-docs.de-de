---
title: Microsoft-Konto (MSA)-Identitätsanbieter in Azure AD
description: Verwenden Sie Azure AD, um es einem externen Benutzer (Gast) zu ermöglichen, sich mit seinem Microsoft-Konto (MSA) bei Ihren Azure AD-Apps anzumelden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25d3380277d0e653fd5cb03069b7d91cb363dd95
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101692900"
---
# <a name="microsoft-account-msa-identity-provider-for-external-identities-preview"></a>Microsoft-Konto (MSA)-Identitätsanbieter für External Identities (Vorschauversion)

> [!NOTE]
> Der Microsoft-Konto-Identitätsanbieter ist eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ihre B2B-Gastbenutzer können ihre eigenen persönlichen Microsoft-Konten ohne weitere Konfiguration für B2B Collaboration verwenden. Gastbenutzer können Ihre B2B Collaboration-Einladungen einlösen oder Ihre Benutzerflows zur Registrierung mit ihrem persönlichen Microsoft-Konto durchführen.

Microsoft-Konten werden von einem Benutzer eingerichtet, um Zugriff auf verbraucherorientierte Microsoft-Produkte und -Clouddienste wie Outlook, OneDrive, MSN, Xbox LIVE oder Microsoft 365 zu erhalten. Das Konto wird im von Microsoft betriebenen Microsoft-Kontosystem für Endverbraucheridentitäten erstellt und gespeichert.

## <a name="guest-sign-in-using-microsoft-accounts"></a>Gastanmeldung mit Microsoft-Konten

Die Option „Microsoft-Konto“ ist standardmäßig in der Liste der External Identities-Identitätsanbieter verfügbar. Es ist keine weitere Konfiguration erforderlich, damit sich Gastbenutzer mit ihrem Microsoft-Konto über den Einladungsflow oder einen Benutzerflow für die Self-Service-Registrierung anmelden können.

![„Microsoft-Konto“ in der Liste der Identitätsanbieter](media/microsoft-account/microsoft-account-identity-provider.png)

### <a name="microsoft-account-in-the-invitation-flow"></a>„Microsoft-Konto“ im Einladungsflow

Wenn Sie einen [Gastbenutzer zu B2B Collaboration einladen](add-users-administrator.md), können Sie sein Microsoft-Konto als E-Mail-Adresse für die Anmeldung angeben.

![Einladen mit einem Microsoft-Konto](media/microsoft-account/microsoft-account-invite.png)

### <a name="microsoft-account-in-self-service-sign-up-user-flows"></a>„Microsoft-Konto“ in Benutzerflows für die Self-Service-Registrierung

„Microsoft-Konto“ ist einer der verfügbaren Identitätsanbieter für Ihre Benutzerflows für die Self-Service-Registrierung. Benutzer können sich mit ihren eigenen Microsoft-Konten für Ihre Anwendungen registrieren. Zunächst müssen Sie für Ihren Mandanten die [Self-Service-Registrierung aktivieren](self-service-sign-up-user-flow.md). Danach können Sie einen Benutzerflow für die Anwendung einrichten und „Microsoft-Konto“ als eine der Anmeldeoptionen auswählen.

![„Microsoft-Konto“ in einem Benutzerflow für die Self-Service-Registrierung](media/microsoft-account/microsoft-account-user-flow.png)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von Azure Active Directory B2B Collaboration-Benutzern](add-users-administrator.md)
- [Hinzufügen eines Benutzerflows für die Self-Service-Registrierung zu einer App (Vorschau)](self-service-sign-up-user-flow.md)