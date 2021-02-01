---
author: mmacy
ms.author: marsma
ms.date: 12/16/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: 53198c663d318a2eb47bcb3207939bbcb1fdd59c
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98763252"
---
Mit Microsoft Authentication Library-Apps (MSAL-Apps) werden Protokollmeldungen generiert, die Sie beim Diagnostizieren von Problemen unterstützen. Eine App kann die Protokollierung mit wenigen Codezeilen konfigurieren und hat benutzerdefinierte Kontrolle über den Detailgrad und darüber, ob personenbezogene Daten und Organisationsdaten protokolliert werden oder nicht. Es wird empfohlen, einen MSAL-Protokollierungsrückruf festzulegen und Benutzern eine Möglichkeit zu bieten, Protokolle zu übermitteln, wenn Authentifizierungsprobleme auftreten.

## <a name="logging-levels"></a>Protokolliergrade

MSAL bietet mehrere Grade für Protokollierungsdetails:

- Error: Gibt an, dass ein Problem aufgetreten ist und ein Fehler generiert wurde. Wird zum Debuggen und Identifizieren von Problemen verwendet.
- Warnung: Es muss nicht unbedingt zu einem Fehler oder Ausfall gekommen sein. Dieser Grad ist für das Diagnostizieren und Ermitteln von Problemen vorgesehen.
- Info: MSAL protokolliert Ereignisse, die für Informationszwecke und nicht unbedingt für das Debuggen vorgesehen sind.
- Verbose: Standard. Msal protokolliert die vollständigen Details des Bibliotheksverhaltens.

## <a name="personal-and-organizational-data"></a>Personenbezogene Daten und Organisationsdaten

Die MSAL-Protokollierung erfasst standardmäßig keine streng vertraulichen personenbezogenen Daten oder Organisationsdaten. Die Bibliothek bietet jedoch die Option, die Protokollierung von personenbezogenen Daten und Organisationsdaten zu aktivieren.

In den folgenden Abschnitten finden Sie weitere Einzelheiten zur MSAL-Fehlerprotokollierung für Ihre Anwendung.