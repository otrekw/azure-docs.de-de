---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/11/2021
ms.author: mimart
ms.openlocfilehash: f4677d9800f0c94d9071838b9addbeb3cd1231c9
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112071329"
---
Der Abmeldeflow umfasst die folgenden Schritte:

1. In der App wählt der Benutzer **Abmelden** aus.
1. Die App entfernt ihre Sitzungscookies, und die Authentifizierungsbibliothek leert ihren Tokencache.
1. Die App leitet den Benutzer an den Azure AD B2C-Abmeldeendpunkt um, um die Azure AD B2C-Sitzung zu beenden.
1. Der Benutzer wird wieder zur App umgeleitet.