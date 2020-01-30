---
title: Deaktivieren der E-Mail-Überprüfung während der Kundenregistrierung
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie die E-Mail-Überprüfung bei der Benutzerregistrierung in Azure Active Directory B2C deaktivieren.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c5b3598e33dac131c8881248a5f4b740a6302e20
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850793"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>Deaktivieren der E-Mail-Überprüfung bei der Benutzerregistrierung in Azure Active Directory B2C

Standardmäßig überprüft Azure Active Directory B2C (Azure AD B2C) die E-Mail-Adresse Ihres Kunden für lokale Konten (Konten für Benutzer, die sich mit E-Mail-Adresse oder Benutzername registrieren). Azure AD B2C stellt sicher, dass gültige E-Mail-Adressen verwendet werden, indem Kunden diese während des Registrierungsvorgangs verifizieren müssen. Außerdem wird verhindert, dass ein böswilliger Akteur mit automatisierten Prozessen betrügerische Konten in Ihren Anwendungen generiert.

Einige Anwendungsentwickler ziehen es vor, die E-Mail-Überprüfung beim Registrierungsvorgang zu überspringen und diese erst später vom Kunden anzufordern. Um dies zu unterstützen, kann Azure AD B2C so konfiguriert werden, dass die E-Mail-Überprüfung deaktiviert wird. Diese Vorgehensweise sorgt für einen einfacheren Registrierungsvorgang und bietet Entwicklern die Flexibilität, zwischen Kunden zu unterscheiden, die ihre E-Mail-Adresse bereits verifiziert haben, und solchen, die dies noch nicht getan haben.

Führen Sie die folgenden Schritte aus, um die E-Mail-Überprüfung zu deaktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Wählen Sie im Hauptmenü über den Filter **Verzeichnis + Abonnement** das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Benutzerflows** aus.
1. Wählen Sie den Benutzerflow aus, dessen E-Mail-Überprüfung deaktiviert werden soll, beispielsweise *B2C_1_signinsignup*.
1. Wählen Sie **Seitenlayouts** aus.
1. Wählen Sie **Registrierungsseite für lokales Konto** aus.
1. Wählen Sie unter **Benutzerattribute** die Option **E-Mail-Adresse** aus.
1. Wählen Sie im Dropdown-Menü **Verifizierung erforderlich** die Option **Nein** aus.
1. Wählen Sie **Speichern** aus. Die E-Mail-Überprüfung ist jetzt für diesen Benutzerflow deaktiviert.

> [!WARNING]
> Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. Wenn Sie die standardmäßige von Azure AD B2C bereitgestellte E-Mail-Überprüfung deaktivieren, empfiehlt es sich, als Ersatz ein anderes Überprüfungssystem zu implementieren.
