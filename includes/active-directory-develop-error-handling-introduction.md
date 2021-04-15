---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: 2ff600429cefbfe0c9a4f0522ee49274000029ca
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98760704"
---
Dieser Artikel gibt einen Überblick über die verschiedenen Fehlerarten und Empfehlungen zur Behandlung von allgemeinen Anmeldefehlern.

## <a name="msal-error-handling-basics"></a>Grundlagen der MSAL-Fehlerbehandlung

Ausnahmen in der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) sind für App-Entwickler vorgesehen, die eine Problembehandlung durchführen, und nicht für Endbenutzer. Ausnahmemeldungen sind nicht lokalisiert.

Bei der Verarbeitung von Ausnahmen und Fehlern können Sie den Ausnahmetyp selbst und den Fehlercode verwenden, um zwischen Ausnahmen zu unterscheiden.  Eine Liste der Fehlercodes finden Sie unter [Azure AD-Authentifizierungs- und Autorisierungsfehlercodes](../articles/active-directory/develop/reference-aadsts-error-codes.md).

Während der Anmeldung treten möglicherweise Fehler bei Zustimmungen, bedingtem Zugriff (MFA, Geräteverwaltung, standortbezogene Einschränkungen), Tokenausstellung und -einlösung sowie Benutzereigenschaften auf.

Im folgenden Abschnitt werden weitere Details zur Fehlerbehandlung für Ihre App erläutert.