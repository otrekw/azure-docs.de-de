---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: 2ff600429cefbfe0c9a4f0522ee49274000029ca
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "98760704"
---
Dieser Artikel gibt einen Überblick über die verschiedenen Fehlerarten und Empfehlungen zur Behandlung von allgemeinen Anmeldefehlern.

## <a name="msal-error-handling-basics"></a>Grundlagen der MSAL-Fehlerbehandlung

Ausnahmen in der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) sind für App-Entwickler vorgesehen, die eine Problembehandlung durchführen, und nicht für Endbenutzer. Ausnahmemeldungen sind nicht lokalisiert.

Bei der Verarbeitung von Ausnahmen und Fehlern können Sie den Ausnahmetyp selbst und den Fehlercode verwenden, um zwischen Ausnahmen zu unterscheiden.  Eine Liste der Fehlercodes finden Sie unter [Azure AD-Authentifizierungs- und Autorisierungsfehlercodes](../articles/active-directory/develop/reference-aadsts-error-codes.md).

Während der Anmeldung treten möglicherweise Fehler bei Zustimmungen, bedingtem Zugriff (MFA, Geräteverwaltung, standortbezogene Einschränkungen), Tokenausstellung und -einlösung sowie Benutzereigenschaften auf.

Im folgenden Abschnitt werden weitere Details zur Fehlerbehandlung für Ihre App erläutert.