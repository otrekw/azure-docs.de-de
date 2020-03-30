---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/27/2020
ms.author: mimart
ms.openlocfilehash: f4944881e6eff3e9a56a74bc997c280f9fff6be8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186992"
---
## <a name="ropc-flow-notes"></a>Hinweise zum ROPC-Flow
In Azure Active Directory B2C (Azure AD B2C) werden die folgenden Optionen unterstützt:

- **Nativer Client**: Die Benutzerinteraktion während der Authentifizierung erfolgt, wenn Code auf einem Gerät auf Benutzerseite ausgeführt wird. Bei dem Gerät kann es sich um eine mobile Anwendung handeln, die unter einem nativen Betriebssystem wie Android oder iOS ausgeführt wird.
- **Flow für öffentlichen Client**: Im API-Aufruf werden nur die von einer Anwendung gesammelten Benutzeranmeldeinformationen gesendet. Die Anmeldeinformationen der Anwendung werden nicht gesendet.
- **Neue Ansprüche hinzufügen**: Der Inhalt des ID-Tokens kann geändert werden, um neue Ansprüche hinzuzufügen.

Folgende Flüsse werden nicht unterstützt:

- **Server-zu-Server**: Das Identitätsschutzsystem benötigt eine zuverlässige IP-Adresse, die vom Aufrufer (dem nativen Client) im Rahmen der Interaktion erfasst wurde. Bei einem serverseitigen API-Aufruf wird nur die IP-Adresse des Servers verwendet. Wenn der dynamische Schwellenwert einer fehlgeschlagenen Authentifizierung überschritten wird, identifiziert das System für den Identitätsschutz eine wiederholt auftretende IP-Adresse als Angreifer.
- **Flow für vertraulichen Client**: Die Anwendungsclient-ID wird überprüft, der geheime Anwendungsschlüssel wird jedoch nicht überprüft.

Beachten Sie bei der Verwendung des ROPC-Flows Folgendes:

- ROPC funktioniert nicht, wenn der Authentifizierungsflow für eine Benutzerinteraktion unterbrochen wird. Beispiele: Ein Kennwort ist abgelaufen oder muss geändert werden, Multi-Factor Authentication ist erforderlich, oder während der Anmeldung müssen weitere Informationen erfasst werden (z. B. Benutzerzustimmung).
- ROPC unterstützt nur lokale Konten. Benutzer können sich nicht mit Verbundidentitätsanbietern wie Microsoft, Google +, Twitter, AD-FS oder Facebook anmelden.
- Die Sitzungsverwaltung, einschließlich „Angemeldet bleiben“, ist nicht anwendbar.
