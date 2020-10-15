---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/11/2020
ms.author: mimart
ms.openlocfilehash: 4e03d076fc9ed7d87326a4c8b4f8bf6c0b21c750
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "79126734"
---
## <a name="disable-email-verification"></a>Deaktivieren der E-Mail-Überprüfung

Standardmäßig überprüft Azure Active Directory B2C (Azure AD B2C) die E-Mail-Adresse Ihres Kunden für lokale Konten (Konten für Benutzer, die sich mit E-Mail-Adresse oder Benutzername registrieren). Azure AD B2C stellt sicher, dass gültige E-Mail-Adressen verwendet werden, indem Kunden diese während des Registrierungsvorgangs verifizieren müssen. Außerdem wird verhindert, dass böswillige Akteure mit automatisierten Prozessen betrügerische Konten in Ihren Anwendungen generieren.

Einige Anwendungsentwickler ziehen es vor, die E-Mail-Überprüfung beim Registrierungsvorgang zu überspringen und diese erst später vom Kunden anzufordern. Um dies zu unterstützen, kann Azure AD B2C so konfiguriert werden, dass die E-Mail-Überprüfung deaktiviert wird. Diese Vorgehensweise sorgt für einen einfacheren Registrierungsvorgang und bietet Entwicklern die Flexibilität, zwischen Kunden zu unterscheiden, die ihre E-Mail-Adresse bereits verifiziert haben, und solchen, die dies noch nicht getan haben.

> [!WARNING]
> Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. Wenn Sie die standardmäßige von Azure AD B2C bereitgestellte E-Mail-Überprüfung deaktivieren, empfiehlt es sich, als Ersatz ein anderes Überprüfungssystem zu implementieren.
