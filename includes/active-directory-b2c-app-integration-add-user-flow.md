---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/11/2021
ms.author: mimart
ms.openlocfilehash: fdafb1320c40a44bcdd9a365bd53baec431627d3
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112071328"
---
Wenn ein Benutzer versucht, sich bei Ihrer App anzumelden, initiiert die App über einen [Benutzerflow](../articles/active-directory-b2c/user-flow-overview.md) eine Authentifizierungsanforderung an den Autorisierungsendpunkt. Der Benutzerflow definiert und kontrolliert die Benutzeroberfläche. Wenn der Benutzer den Benutzerflow abschließt, generiert Azure AD B2C ein Token und leitet den Benutzer wieder zur Anwendung um.

Falls noch nicht geschehen, [erstellen Sie einen Benutzerflow oder eine benutzerdefinierte Richtlinie](../articles/active-directory-b2c/add-sign-up-and-sign-in-policy.md).
