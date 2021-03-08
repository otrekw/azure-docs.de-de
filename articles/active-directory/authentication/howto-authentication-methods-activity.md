---
title: 'Aktivität für Authentifizierungsmethoden: Azure Active Directory'
description: Übersicht über die Authentifizierungsmethoden, die von Benutzern für die Anmeldung und Kennwortzurücksetzung registriert werden.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 02/25/2021
ms.author: justinha
author: sopand
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbe69967d84777091aec0bbbf1626b98f5018d0e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692908"
---
# <a name="authentication-methods-activity"></a>Aktivität für Authentifizierungsmethoden 

Das neue Aktivitätsdashboard für Authentifizierungsmethoden ermöglicht Administratoren die Überwachung der Registrierung und Nutzung von Authentifizierungsmethoden in der gesamten Organisation. Dank dieser Berichtsfunktion kann Ihre Organisation besser nachvollziehen, welche Methoden registriert und wie sie verwendet werden.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="permissions-and-licenses"></a>Berechtigungen und Lizenzen

Die folgenden Rollen können auf „Nutzung und Erkenntnisse“ zugreifen:

- Meldet Reader
- Sicherheitsleseberechtigter
- Sicherheitsadministrator
- Globaler Administrator

 Für den Zugriff auf den Verbrauch und die Erkenntnisse ist eine Azure AD Premium P1- oder P2-Lizenz erforderlich. Informationen zur Lizenzierung für Azure AD Multi-Factor Authentication und Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) finden Sie auf der [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="how-it-works"></a>Funktionsweise

So greifen Sie auf „Authentifizierungsmethoden: Nutzung und Erkenntnisse“ zu:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf **Azure Active Directory** > **Sicherheit** > **Authentifizierungsmethoden** > **Aktivität**.
1. Der Bericht enthält zwei Registerkarten: **Registrierung** und **Verwendung**.

   ![Übersicht über die Aktivität für Authentifizierungsmethoden](media/how-to-authentication-methods-usage-insights/registration-usage-tabs.png)

## <a name="registration-details"></a>Registrierungsdetails

Auf der Registerkarte [**Registrierung**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade) erfahren Sie, wie viele Benutzer die mehrstufige Authentifizierung, die kennwortlose Authentifizierung und die Self-Service-Kennwortzurücksetzung verwenden können. 

Klicken Sie auf eine der folgenden Optionen, um eine vorgefilterte Liste mit Details zur Benutzerregistrierung anzuzeigen:

- **Benutzer bereit für Azure MFA** zeigt eine Aufschlüsselung der Benutzer, für die beide der folgenden Punkte zutreffen:
  - Sie sind für eine sichere Authentifizierungsmethode registriert. 
  - Sie diese Methode dank einer Richtlinie für die mehrstufige Authentifizierung verwenden. 
  
  Benutzer, die für eine mehrstufige Authentifizierung außerhalb von Azure AD registriert sind, sind in dieser Zahl nicht berücksichtigt. 
- **Benutzer bereit für kennwortlose Authentifizierung** zeigt eine Aufschlüsselung der Benutzer, die für die Anmeldung ohne Kennwort (unter Verwendung von FIDO2, Windows Hello for Business oder einer kennwortlosen Anmeldung per Telefon mit der Microsoft Authenticator-App) registriert sind. 
- **Benutzer bereit für Self-Service-Kennwortzurücksetzung** zeigt eine Aufschlüsselung der Benutzer, die ihr eigenes Kennwort zurücksetzen können. Benutzer können ihr Kennwort zurücksetzen, wenn beide der folgenden Punkte zutreffen:
  - Sie sind für genügend Methoden registriert, um die Richtlinie für die Self-Service-Kennwortzurücksetzung ihrer Organisation zu erfüllen. 
  - Ihnen wurde das Zurücksetzen ihres Kennworts ermöglicht. 

  ![Screenshot: Benutzer, die sich registrieren können](media/how-to-authentication-methods-usage-insights/users-capable.png)

**Registrierte Benutzer nach Authentifizierungsmethode** zeigt, wie viele Benutzer für die jeweilige Authentifizierungsmethode registriert sind. Klicken Sie auf eine Authentifizierungsmethode, um zu sehen, wer für diese Methode registriert ist.

![Screenshot: Registrierte Benutzer](media/how-to-authentication-methods-usage-insights/users-registered.png)

**Recent registrations by authentication method** (Kürzlich durchgeführte Registrierungen nach Authentifizierungsmethode) gibt Aufschluss über die Anzahl erfolgreicher und nicht erfolgreicher Registrierungen (sortiert nach Authentifizierungsmethode). Klicken Sie auf eine Authentifizierungsmethode, um aktuelle Registrierungsereignisse für diese Methode anzuzeigen.

![Screenshot: Kürzlich durchgeführte Registrierungen](media/how-to-authentication-methods-usage-insights/recently-registered.png)

## <a name="usage-details"></a>Nutzungsdetails

Der Bericht **Verwendung** gibt Aufschluss darüber, welche Authentifizierungsmethoden für die Anmeldung und die Kennwortzurücksetzung verwendet werden.

![Screenshot: Seite „Verwendung“](media/how-to-authentication-methods-usage-insights/usage-page.png)

**Anmeldungen nach Authentifizierungsanforderung** zeigt die Anzahl erfolgreicher interaktiver Benutzeranmeldungen, die für die einstufige Authentifizierung bzw. für die mehrstufige Authentifizierung in Azure AD erforderlich waren. Anmeldungen, bei denen eine mehrstufige Authentifizierung durch einen Drittanbieter erzwungen wurde, sind hier nicht enthalten.

![Screenshot: Anmeldungen nach Authentifizierungsanforderung](media/how-to-authentication-methods-usage-insights/sign-ins-protected.png)

**Anmeldungen nach Authentifizierungsmethode** zeigt die Anzahl erfolgreicher und nicht erfolgreicher interaktiver Benutzeranmeldungen nach verwendeter Authentifizierungsmethode. Anmeldungen, bei denen die Authentifizierungsanforderung durch einen Anspruch im Token erfüllt wurde, sind hier nicht enthalten.

![Screenshot: Anmeldungen nach Methode](media/how-to-authentication-methods-usage-insights/sign-ins-by-method.png)

**Anzahl von Kennwortzurücksetzungen und Kontoentsperrungen** zeigt die Anzahl erfolgreicher Kennwortänderungen und -zurücksetzungen (per Self-Service und durch den Administrator) im Zeitverlauf.

![Screenshot: Zurücksetzungen und Entsperrungen](media/how-to-authentication-methods-usage-insights/password-changes.png)

**Kennwortzurücksetzungen nach Authentifizierungsmethode** zeigt die Anzahl erfolgreicher und nicht erfolgreicher Authentifizierungen beim Kennwortzurücksetzungsflow nach Authentifizierungsmethode.

![Screenshot: Zurücksetzungen nach Methode](media/how-to-authentication-methods-usage-insights/resets-by-method.png)

## <a name="user-registration-details"></a>Details zur Benutzerregistrierung 

Mit den Steuerelementen oben in der Liste können Sie nach einem Benutzer suchen und die Liste der Benutzer nach den angezeigten Spalten filtern.

Der Bericht mit den Registrierungsdetails enthält für jeden Benutzer die folgenden Informationen:

- Benutzerprinzipalname
- Name
- MFA möglich („Möglich“, „Nicht möglich“)
- Kennwortlos möglich („Möglich“, „Nicht möglich“)
- SSPR registriert („Registriert“, „Nicht registriert“)
- SSPR aktiviert („Aktiviert“, „Nicht aktiviert“)
- SSPR möglich („Möglich“, „Nicht möglich“) 
- Registrierte Methoden (E-Mail, Mobiltelefon, alternatives Mobiltelefon, Telefon (geschäftlich), Microsoft Authenticator-Pushbenachrichtigung, softwarebasierte Einmalkennung, FIDO2, Sicherheitsschlüssel, Sicherheitsfragen)

  ![Screenshot: Details zur Benutzerregistrierung](media/how-to-authentication-methods-usage-insights/registration-details.png)

## <a name="registration-and-reset-events"></a>Registrierungs- und Zurücksetzungsereignisse 

In den **Registrierungs- und Zurücksetzungsereignissen** werden Registrierungs- und Zurücksetzungsereignisse der letzten 24 Stunden, sieben Tage oder 30 Tage mit folgenden Angaben angezeigt:

- Date
- Benutzername
- Benutzer 
- Feature („Registrierung“, „Zurücksetzung“)
- Verwendete Methode (App-Benachrichtigung, App-Code, Telefonanruf, geschäftlicher Telefonanruf, Anruf an alternatives Mobiltelefon, SMS, E-Mail, Sicherheitsfragen)
- Status („Erfolg“, „Fehler“)
- Fehlerursache (Erläuterung)

  ![Screenshot: Seite „Verwendung“](media/how-to-authentication-methods-usage-insights/registration-and-reset-logs.png)

## <a name="limitations"></a>Einschränkungen

TAP-Registrierungen (Temporary Access Pass, befristeter Zugriffspass) werden auf der Registrierungsregisterkarte des Berichts nicht berücksichtigt, da sie nur für kurze Zeit gültig sind.

## <a name="next-steps"></a>Nächste Schritte

- [Arbeiten mit der Nutzungsbericht-API für Authentifizierungsmethoden](/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Auswählen von Authentifizierungsmethoden für Ihr Unternehmen](concept-authentication-methods.md)
- [Kombinierte Registrierung](concept-registration-mfa-sspr-combined.md)
