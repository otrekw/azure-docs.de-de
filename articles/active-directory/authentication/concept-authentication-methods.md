---
title: Authentifizierungsmethoden und -funktionen – -Azure Active Directory
description: Erfahren Sie mehr über die verschiedenen in Azure Active Directory verfügbaren Authentifizierungsmethoden und -funktionen, um Anmeldeereignisse zu verbessern und sicher zu gestalten
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.collection: M365-identity-device-management
ms.custom: contperfq4
ms.openlocfilehash: 05283c02c2e5f95e22beb6fbeaad7a99e42ee6aa
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87540834"
---
# <a name="what-authentication-and-verification-methods-are-available-in-azure-active-directory"></a>Welche Authentifizierungs- und Prüfmethoden stehen in Azure Active Directory zur Verfügung?

Bei der Anmeldung bei Konten in Azure Active Directory (Azure AD) hat ein Benutzer verschiedene Möglichkeiten, sich zu authentifizieren. Benutzername und Kennwort sind die gängigste Art und Weise, in der ein Benutzer bisher seine Anmeldeinformationen bereitgestellt hat. Dank moderner Authentifizierungs- und Sicherheitsfunktionen in Azure AD kann dieses Basiskennwort durch zusätzliche Authentifizierungsmethoden ergänzt oder ersetzt werden.

Ein Benutzer in Azure AD kann für seine Authentifizierung eine der folgenden Authentifizierungsmethoden verwenden:

* Herkömmlicher Benutzername und Kennwort
* Kennwortlose Anmeldung mit der Microsoft Authenticator-App
* OATH-Hardwaretoken oder FIDO2-Sicherheitsschlüssel
* SMS-basierte kennwortlose Anmeldung

Viele Konten in Azure AD sind für die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) oder für Azure Multi-Factor Authentication (MFA) aktiviert. Zu diesen Funktionen gehören zusätzliche Prüfmethoden, z. B ein Telefonanruf oder Sicherheitsfragen. Es ist empfehlenswert, Benutzer mehrere Prüfmethoden registrieren zu lassen. Wenn eine Methode für einen Benutzer nicht verfügbar ist, kann er sich mit einer anderen Methode authentifizieren.

In der folgenden Tabelle sind die Methoden aufgeführt, die für die primäre oder sekundäre Authentifizierung verfügbar sind:

| Methode | Primäre Authentifizierung | Sekundäre Authentifizierung |
| --- | --- | --- |
| [Kennwort](#password) | Ja | |
| [Microsoft Authenticator-App](#microsoft-authenticator-app) | Ja (Vorschau) | MFA und SSPR |
| [FIDO2-Sicherheitsschlüssel (Vorschau)](#fido2-security-keys) | Ja | Nur MFA |
| [Oath-Softwaretoken](#oath-software-tokens) | Nein | MFA |
| [OATH-Hardwaretoken (Vorschau)](#oath-hardware-tokens-preview) | Ja | MFA |
| [SMS](#phone-options) | Ja (Vorschau) | MFA und SSPR |
| [Sprachanruf](#phone-options) | Nein | MFA und SSPR |
| [Sicherheitsfragen](#security-questions) | Nein | Nur SSPR |
| [E-Mail-Adresse](#email-address) | Nein | Nur SSPR |
| [App-Kennwörter](#app-passwords) | Nein | MFA nur in bestimmten Fällen |

In diesem Artikel werden diese verschiedenen Authentifizierungs- und Prüfmethoden, die in Azure AD verfügbar sind, sowie mögliche speziellen Einschränkungen beschrieben.

![Verwendete Authentifizierungsmethoden auf dem Anmeldebildschirm](media/concept-authentication-methods/overview-login.png)

## <a name="password"></a>Kennwort

Ein Azure AD-Kennwort wird häufig als eine der primären Authentifizierungsmethoden verwendet. Die Authentifizierungsmethode über Kennwort kann nicht deaktiviert werden.

Auch wenn Sie eine Authentifizierungsmethode wie die [SMS-basierte Anmeldung](howto-authentication-sms-signin.md) verwenden und der Benutzer sein Kennwort nicht zum Anmelden verwendet, bleibt ein Kennwort als verfügbare Authentifizierungsmethode erhalten.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator-App

Die Microsoft Authenticator-App bietet eine zusätzliche Sicherheitsstufe für Ihr Azure AD-Geschäfts-, Schul- oder Uni-Konto bzw. Ihr Microsoft-Konto und ist verfügbar für [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) und [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6). Mit der Microsoft Authenticator-App können sich Benutzer bei der Anmeldung ohne Kennwort authentifizieren. Alternativ können Sie die App bei der Self-Service-Kennwortzurücksetzung (SSPR) oder bei Azure Multi-Factor Authentication-Ereignissen als zusätzliche Prüfoption verwenden.

Benutzer erhalten dann eine Benachrichtigung über die mobile App und können den Vorgang genehmigen oder ablehnen. Sie können aber auch mit der Microsoft Authenticator-App einen OATH-Prüfcode generieren und auf einem Anmeldebildschirm eingeben. Wenn Sie sowohl die Benachrichtigung als auch den Prüfcode aktivieren, können Benutzer, die die Microsoft Authenticator-App registrieren, ihre Identität anhand von beiden Methoden bestätigen.

Informationen zur Verwendung der Microsoft Authenticator-App an einer Anmeldeaufforderung (anstelle der Kombination aus Benutzername und Kennwort) finden Sie unter [Aktivieren der kennwortlosen Anmeldung mit der Microsoft Authenticator-App (Vorschau)](howto-authentication-passwordless-phone.md).

> [!NOTE]
> Wenn Benutzer SSPR aktivieren, haben sie keine Möglichkeit, die mobile App zu registrieren. Stattdessen können sie die mobile App auf [https://aka.ms/mfasetup](https://aka.ms/mfasetup) oder im Rahmen der kombinierten Registrierung von Sicherheitsinformationen auf [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) registrieren.

### <a name="notification-through-mobile-app"></a>Benachrichtigung über mobile App

Die Microsoft Authenticator-App kann dazu beitragen, nicht autorisierten Zugriff auf Konten zu verhindern und betrügerische Transaktionen zu stoppen, indem sie eine Benachrichtigung an Ihr Smartphone oder Tablet sendet. Benutzer sehen die Benachrichtigung und wählen **Bestätigen** aus, wenn der Vorgang rechtmäßig ist. Andernfalls können sie **Verweigern** auswählen.

![Screenshot der Webbrowser-Eingabeaufforderung als Beispiel für die Benachrichtigung der Microsoft Authenticator-App zum Abschließen des Anmeldevorgangs](media/tutorial-enable-azure-mfa/azure-multi-factor-authentication-browser-prompt.png)

> [!NOTE]
> Wenn Mitarbeiter Ihrer Organisation nach China reisen oder dort arbeiten müssen, funktioniert die Methode *Benachrichtigung über mobile App* auf Android-Geräten in diesem Land nicht, da in dieser Region Google Play Services (einschließlich Pushbenachrichtigungen) blockiert sind. iOS-Benachrichtigungen funktionieren jedoch. Daher sollten Benutzern von Android-Geräten alternative Authentifizierungsmethoden zur Verfügung gestellt werden.

### <a name="verification-code-from-mobile-app"></a>Prüfcode über die mobile App

Die Microsoft Authenticator-App kann als Softwaretoken zum Generieren eines OATH-Prüfcodes verwendet werden. Nachdem Sie Benutzernamen und Kennwort eingegeben haben, geben Sie im Anmeldebildschirm den in der Authenticator-App generierten Code ein. Der Überprüfungscode kann als zweite Authentifizierungsmethode eingegeben werden.

Benutzer verfügen möglicherweise über eine Kombination aus bis zu fünf OATH-Hardwaretoken oder Authentifizierungsanwendungen wie die Microsoft Authenticator-App, die für die jederzeitige Verwendung konfiguriert sind.

> [!WARNING]
> Wenn bei der Self-Service-Kennwortzurücksetzung nur eine Methode zum Zurücksetzen erforderlich ist und Sie ein Höchstmaß an Sicherheit gewährleisten möchten, steht den Benutzern als einzige Option nur der Prüfcode zur Verfügung.
>
> Wenn zwei Methoden erforderlich sind, können Benutzer zum Zurücksetzen neben allen anderen aktivierten Methoden entweder eine Benachrichtigung oder einen Prüfcode verwenden.

## <a name="fido2-security-keys"></a>FIDO2-Sicherheitsschlüssel

Die FIDO-Allianz (Fast IDentity Online) fördert offene Standards für die Authentifizierung und trägt zur Reduzierung der Verwendung von Kennwörtern als Authentifizierungsmethode bei. FIDO2 ist der aktuelle Standard, der den Webauthentifizierungsstandard (WebAuthn) beinhaltet.

Informationen zum Verwenden von FIDO2-Sicherheitsschlüsseln an einer Anmeldeaufforderung (anstelle einer Kombination aus Benutzername und Kennwort) finden Sie unter [Aktivieren der kennwortlosen Anmeldung mit FIDO2-Sicherheitsschlüsseln (Vorschau)](howto-authentication-passwordless-security-key.md).

Benutzer können einen FIDO2-Sicherheitsschlüssel registrieren und dann auf dem Anmeldebildschirm als Hauptauthentifizierungsmethode auswählen. Bei diesen FIDO2-Sicherheitsschlüsseln handelt es sich in der Regel um USB-Geräte, es können aber auch Bluetooth- oder NFC-Geräte sein. Mit einem Hardwaregerät, das für die Authentifizierung sorgt, erhöht sich die Sicherheit eines Kontos, da es kein Kennwort gibt, das verfügbar gemacht oder erraten werden kann.

FIDO2-Sicherheitsschlüssel in Azure AD sind derzeit in der Vorschau verfügbar. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="oath-tokens"></a>OATH-Token

OATH TOTP (Time-based One Time Password) ist ein offener Standard, der angibt, wie Einmalkennwortcodes (one-time password, OTP) generiert werden. OATH TOTP kann entweder mit Hardware oder Software zum Generieren der Codes implementiert werden. OATH HOTP, ein anderer Standard zum Generieren von Code, wird in Azure AD nicht unterstützt.

### <a name="oath-software-tokens"></a>OATH-Softwaretoken

Bei OATH-Softwaretoken handelt es sich in der Regel um Anwendungen (z. B. die Microsoft Authenticator-App und andere Authentifizierungs-Apps). Azure AD generiert den geheimen Schlüssel (bzw. Ausgangswert), der in der App eingegeben und zum Generieren des jeweiligen Einmalkennworts (OTP) verwendet wird.

Die Authentizierungs-App generiert automatisch Codes, wenn sie für Push-Benachrichtigungen eingerichtet wurde. Daher verfügt der Benutzer über eine Sicherung, selbst wenn sein Gerät keine Verbindung herstellen kann. Anwendungen von Drittanbietern, die OATH TOTP zum Generieren von Codes verwenden, können ebenfalls zum Einsatz kommen.

Einige OATH TOTP-Hardwaretoken sind programmierbar, d. h., sie sind nicht mit einem geheimen Schlüssel oder Ausgangswert vorprogrammiert. Diese programmierbaren Hardwaretoken können mithilfe des geheimen Schlüssels oder Ausgangswerts, der aus dem Softwaretoken-Setupflow abgerufen wird, eingerichtet werden. Kunden können diese Token bei dem Anbieter ihrer Wahl erwerben und den geheimen Schlüssel bzw. den Ausgangswert beim Setupvorgang ihres Anbieters verwenden.

### <a name="oath-hardware-tokens-preview"></a>OATH-Hardwaretoken (Vorschau)

Azure AD unterstützt die Verwendung von OATH TOTP SHA-1-Token, die den Code alle 30 oder 60 Sekunden aktualisieren. Kunden können diese Token beim Anbieter ihrer Wahl erwerben.

OATH TOTP-Hardwaretoken sind in der Regel mit einem geheimen, im Token vorprogrammierten Schlüssel oder Anfangswert versehen. Diese Schlüssel müssen entsprechend der Beschreibung in den folgenden Schritten in Azure AD eingegeben werden. Geheime Schlüssel sind auf 128 Zeichen beschränkt, was möglicherweise nicht mit allen Token kompatibel ist. Der geheime Schlüssel darf nur die Zeichen *a-z* oder *A-Z* und die Ziffern *1-7* enthalten und muss in *Base32* codiert sein.

Programmierbare OATH TOTP-Hardwaretoken, die mit einem neuen Ausgangswert versehen werden können, können ebenfalls im Softwaretoken-Setupflow mit Azure AD eingerichtet werden.

OATH-Hardwaretoken werden in der öffentlichen Vorschau unterstützt. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![Hochladen von OATH-Token auf das Blatt „MFA > OATH-Token“](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

Sobald Sie Token erworben haben, müssen Sie diese in einem Format mit kommagetrennten Werten (CSV) einschließlich UPN, Seriennummer, geheimem Schlüssel, Zeitintervall, Hersteller und Modell hochladen, wie im folgenden Beispiel gezeigt:

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567abcdef1234567abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Stellen Sie sicher, dass Sie die Kopfzeile in Ihre CSV-Datei einschließen.

Nach der ordnungsgemäßen Formatierung als CSV-Datei kann sich ein Administrator im Azure-Portal anmelden, zu **Azure Active Directory > Sicherheit > MFA > OATH-Token** navigieren und die resultierende CSV-Datei hochladen.

Je nach Größe der CSV-Datei kann die Verarbeitung ein paar Minuten dauern. Wählen Sie die Schaltfläche **Aktualisieren** aus, um den aktuellen Status abzurufen. Wenn die Datei Fehler enthält, können Sie eine CSV-Datei herunterladen, in der alle zu behebenden Fehler aufgeführt sind. Die Feldnamen in der heruntergeladenen CSV-Datei unterscheiden sich von der hochgeladenen Version.

Sobald alle Fehler behoben sind, kann der Administrator die einzelnen Schlüssel aktivieren, indem er für das Token **Aktivieren** auswählt und das in dem Token angezeigte Einmalkennwort eingibt.

Benutzer verfügen möglicherweise über eine Kombination aus bis zu fünf OATH-Hardwaretoken oder Authentifizierungsanwendungen wie die Microsoft Authenticator-App, die für die jederzeitige Verwendung konfiguriert sind.

## <a name="phone-options"></a>Telefonoptionen

Informationen zur direkten Authentifizierung mit Textnachrichten finden Sie unter [Konfigurieren und Aktivieren der SMS-basierten Authentifizierung von Benutzern (Vorschau)](howto-authentication-sms-signin.md). Die SMS-basierte Anmeldung eignet sich hervorragend für Außendienstmitarbeiter. Bei der SMS-basierten Anmeldung brauchen sich Benutzer keine Benutzernamen und Kennwörter zu merken, um auf Anwendungen und Dienste zuzugreifen. Der Benutzer gibt stattdessen seine registrierte Mobiltelefonnummer ein, empfängt eine Textnachricht mit einem Prüfcode und gibt diesen Code im Anmeldebildschirm ein.

Benutzer können sich als zweite Authentifizierungsmethode auch selbst anhand einer Mobiltelefonnummer oder geschäftlichen Telefonnummer bestätigen, z. B. bei der Azure Multi-Factor Authentication oder der Self-Service-Kennwortzurücksetzung (SSPR).

Für eine ordnungsgemäße Funktion müssen Telefonnummern im Format *+Landesvorwahl Telefonnummer* vorliegen (Beispiel: *+1 4251234567*).

> [!NOTE]
> Zwischen Landes-/Ortsvorwahl und Telefonnummer muss sich ein Leerzeichen befinden.
>
> Bei der Kennwortzurücksetzung werden Nebenstellen nicht unterstützt. Selbst bei der Angabe im Format *+1 4251234567X12345* werden Nebenstellen vor dem Anruf entfernt.

### <a name="mobile-phone-verification"></a>Überprüfung mit Mobiltelefonnummer

Bei Azure Multi-Factor Authentication oder SSPR stehen dem Benutzer zwei Optionen zur Auswahl: Empfang einer Textnachricht mit einem Prüfcode, der im Anmeldebildschirm eingegeben wird, oder Telefonanruf mit der Aufforderung, einen definierten PIN-Code einzugeben.

Wenn ein Benutzer nicht möchte, dass seine Mobiltelefonnummer im Verzeichnis sichtbar ist, sie aber dennoch für die Kennwortzurücksetzung verwenden möchte, sollten Administratoren die Telefonnummer nicht in das Verzeichnis übernehmen. Der Benutzer sollte stattdessen über die kombinierte Registrierung von Sicherheitsinformationen auf [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) das Attribut **Authentifizierungstelefon** für sich eingeben. Administratoren können diese Informationen im Profil des Benutzers anzeigen, sie werden jedoch nicht an anderer Stelle veröffentlicht.

![Screenshot des Azure-Portals: Authentifizierungsmethoden mit übernommener Telefonnummer](media/concept-authentication-methods/user-authentication-methods.png)

Microsoft kann keine Bereitstellung konsistenter SMS- oder Sprachaufforderungen für Azure Multi-Factor Authentication durch dieselbe Nummer garantieren. Im Interesse unserer Benutzer können wir jederzeit Kurzcodes hinzuzufügen oder entfernen, wenn wir Routenanpassungen zur Verbesserung der Zustellbarkeit von SMS vornehmen. Microsoft unterstützt keine Kurzcodes für Länder/Regionen außerhalb der USA und Kanadas.

#### <a name="text-message-verification"></a>Überprüfung mit Textnachricht

Bei der Überprüfung mit Textnachricht wird bei der Self-Service-Kennwortzurücksetzung oder bei Azure Multi-Factor Authentication eine SMS mit einem Prüfcode an die Mobiltelefonnummer gesendet. Zum Abschließen des Anmeldevorgangs muss der bereitgestellte Prüfcode im Anmeldebildschirm eingegeben werden.

#### <a name="phone-call-verification"></a>Überprüfung mit Telefonanruf

Bei der Überprüfung mit Telefonanruf erfolgt bei der Self-Service-Kennwortzurücksetzung oder bei Azure Multi-Factor Authentication ein automatischer Sprachanruf an die vom Benutzer registrierte Telefonnummer. Zum Abschließen des Anmeldevorgangs wird der Benutzer aufgefordert, seine PIN-Nummer, gefolgt von #, auf der Tastatur einzugeben.

### <a name="office-phone-verification"></a>Überprüfung mit geschäftlicher Rufnummer

Das Attribut für die geschäftliche Rufnummer wird vom Azure AD-Administrator verwaltet und kann nicht vom Benutzer selbst registriert werden.

Bei der Überprüfung mit Telefonanruf erfolgt bei der Self-Service-Kennwortzurücksetzung oder bei Azure Multi-Factor Authentication ein automatischer Sprachanruf an die vom Benutzer registrierte Telefonnummer. Zum Abschließen des Anmeldevorgangs wird der Benutzer aufgefordert, seine PIN-Nummer, gefolgt von #, auf der Tastatur einzugeben.

### <a name="troubleshooting-phone-options"></a>Problembehandlung bei Telefonoptionen

Wenn bei der telefonischen Authentifizierung für Azure AD Probleme auftreten, überprüfen Sie die folgenden Schritte zur Problembehebung:

* Gesperrte Anrufer-ID auf einem einzelnen Gerät.
   * Überprüfen Sie alle gesperrten Telefonnummern, die auf dem Gerät konfiguriert sind.
* Falsche Telefonnummer oder falsche Landes-/Ortsvorwahl oder Verwechslung von persönlicher und geschäftlicher Telefonnummer.
   * Beheben Sie das Problem in Bezug auf das Benutzerobjekt und die konfigurierten Authentifizierungsmethoden. Stellen Sie sicher, dass die richtigen Telefonnummern registriert sind.
* Falsche PIN eingegeben.
   * Vergewissern Sie sich, dass der Benutzer die richtige PIN, die für sein Konto registriert ist, verwendet hat.
* Anruf an Voicemail weitergeleitet.
   * Stellen Sie sicher, dass der Benutzer sein Telefon eingeschaltet hat und dieser Dienst in seiner Region verfügbar ist, oder verwenden Sie die alternative Methode.
* Benutzer ist gesperrt
   * Veranlassen Sie, dass ein Azure AD-Administrator den Benutzer im Azure-Portal entsperrt.
* SMS ist auf dem Gerät nicht abonniert.
   * Lassen Sie den Benutzer die Methoden ändern oder SMS auf dem Gerät aktivieren.
* Fehler beim Telekommunikationsanbieter (z. B. Telefoneingabe nicht erkannt, Problem mit fehlenden MFV-Tönen, gesperrte Anrufer-ID auf mehreren Geräten oder SMS auf mehreren Geräten gesperrt).
   * Microsoft verwendet mehrere Telekommunikationsanbieter, um Telefonanrufe und SMS-Nachrichten für die Authentifizierung weiterzuleiten. Wenn eines der oben genannten Probleme auftritt, bitten Sie den Benutzer, die Methode innerhalb von 5 Minuten mindestens fünfmal zu verwenden, und halten Sie die Informationen dieses Benutzers bereit, wenn Sie sich an den Microsoft-Support wenden.

## <a name="security-questions"></a>Sicherheitsfragen

Sicherheitsfragen werden bei einem Anmeldeereignis nicht als Authentifizierungsmethode verwendet. Sicherheitsfragen können vielmehr bei der Self-Service-Kennwortzurücksetzung (SSPR) verwendet werden, um die eigene Identität zu bestätigen. Die Verwendung von Sicherheitsfragen bei Administratorkonten sind bei der Self-Service-Kennwortzurücksetzung nicht als Prüfmethode zulässig.

Wenn sich Benutzer für die Self-Service-Kennwortzurücksetzung registrieren, werden sie aufgefordert, die gewünschten Authentifizierungsmethoden auszuwählen. Bei der Auswahl von Sicherheitsfragen müssen die Benutzer aus den diversen verfügbaren Fragen für die Eingabeaufforderung eine Auswahl treffen und ihre Antworten bereitstellen.

![Screenshot des Azure-Portals mit Authentifizierungsmethoden und Optionen für Sicherheitsfragen](media/concept-authentication-methods/security-questions-authentication-method.png)

> [!NOTE]
> Sicherheitsfragen werden privat und sicher in einem Benutzerobjekt im Verzeichnis gespeichert und können nur während der Registrierung von Benutzern beantwortet werden. Ein Administrator hat keine Möglichkeit, die Fragen oder Antworten eines Benutzers zu lesen oder zu ändern.

Sicherheitsfragen können weniger sicher als andere Methoden sein, da einige Personen unter Umständen die Antworten auf die Fragen eines anderen Benutzers kennen. Wenn Sie Sicherheitsfragen bei der Self-Service-Kennwortzurücksetzung verwenden, wird empfohlen, diese in Verbindung mit einer anderen Methode zu verwenden. Ein Benutzer kann aufgefordert werden, die Microsoft Authenticator-App oder die Authentifizierung über Telefon zu verwenden, um beim SSPR-Prozess seine Identität nachzuweisen, und Sicherheitsfragen nur dann auszuwählen, wenn sein Telefon oder das registrierte Gerät nicht verfügbar ist.

### <a name="predefined-questions"></a>Vordefinierte Fragen

Die folgenden vordefinierten Sicherheitsfragen stehen als Prüfmethode bei der Self-Service-Kennwortzurücksetzung zur Verfügung. Alle Sicherheitsfragen sind in alle Office 365-Sprachen übersetzt und lokalisiert und basieren auf dem Gebietsschema, das im Browser des Benutzers eingestellt ist:

* In welcher Stadt haben Sie Ihren ersten Partner bzw. Ihre erste Partnerin kennengelernt?
* In welcher Stadt haben sich Ihre Eltern kennengelernt?
* In welcher Stadt lebt Ihr Bruder/Ihre Schwester?
* Wie heißt die Geburtsstadt Ihres Vaters?
* In welcher Stadt haben Sie Ihre erste Arbeitsstelle angetreten?
* Wie heißt die Geburtsstadt Ihrer Mutter?
* In welcher Stadt haben Sie den Neujahrstag 2000 verbracht?
* Wie hieß Ihr Lieblingslehrer bzw. Ihre Lieblingslehrerin in der Grundschule mit Nachnamen?
* Bei welcher Universität haben Sie sich beworben, dort jedoch nicht studiert?
* An welchem Ort fand Ihre Hochzeitsfeier statt?
* Wie lautet der 2. Vorname Ihres Vaters?
* Was ist Ihr Lieblingsgericht?
* Wie lautet der Vor- und Nachname Ihrer Großmutter mütterlicherseits?
* Wie lautet der 2. Vorname Ihrer Mutter?
* In welchem Monat und Jahr ist Ihre älteste Schwester bzw. Ihr ältester Bruder geboren? (Beispiel: November 1985)
* Wie lautet der 2. Vorname Ihrer ältesten Schwester bzw. Ihres ältesten Bruders?
* Wie lautet der Vor- und Nachname Ihres Großvaters väterlicherseits?
* Wie lautet der 2. Vorname Ihrer jüngsten Schwester bzw. Ihres jüngsten Bruders?
* Auf welche Schule sind Sie in der sechsten Klasse gegangen?
* Wie lautet der Vor- und Nachname Ihres besten Freundes bzw. Ihrer besten Freundin in Ihrer Kindheit?
* Wie lautet der Vor- und Nachname Ihres ersten Partners bzw. Ihrer ersten Partnerin?
* Wie lautete der Nachname des Lehrers bzw. der Lehrerin Ihres Lieblingsfachs?
* Welche Marke und welches Modell hatte Ihr erstes Auto oder Motorrad?
* Wie heißt die erste Schule, die Sie besucht haben?
* In welchem Krankenhaus wurden Sie geboren?
* In welcher Straße haben Sie als Kind zuerst gewohnt?
* Wer war der Held Ihrer Kindheit?
* Wie hieß Ihr Lieblingskuscheltier?
* Wie hieß Ihr erstes Haustier?
* Welchen Spitznamen hatten Sie als Kind?
* Was war in der Schulzeit Ihre Lieblingssportart?
* Was war Ihr erster Job?
* Wie lauten die letzten vier Ziffern der Telefonnummer in Ihrer Kindheit?
* Was wollten Sie als Kind später einmal werden?
* Welches ist die berühmteste Person, die Sie kennen gelernt haben?

### <a name="custom-security-questions"></a>Benutzerdefinierte Sicherheitsfragen

Für noch mehr Flexibilität können Sie auch Ihre eigenen benutzerdefinierten Sicherheitsfragen definieren. Die Länge von benutzerdefinierten Sicherheitsfragen ist auf 200 Zeichen begrenzt.

Benutzerdefinierte Sicherheitsfragen werden nicht automatisch lokalisiert, wie das bei den Standardsicherheitsfragen der Fall ist. Alle benutzerdefinierten Fragen werden auch bei abweichendem Gebietsschema im Browser des Benutzers in der Sprache angezeigt, in der sie in der administrativen Benutzeroberfläche eingegeben wurden. Wenn Sie lokalisierte Fragen benötigen, verwenden Sie die vordefinierten Fragen.

### <a name="security-question-requirements"></a>Anforderungen an Sicherheitsfragen

Sowohl für Standardsicherheitsfragen als auch für benutzerdefinierte Sicherheitsfragen gelten die folgenden Anforderungen und Einschränkungen:

* Die Mindestanzahl von Zeichen für Antworten ist drei Zeichen.
* Die maximale Zeichenanzahl für Antworten ist 40 Zeichen.
* Benutzer können dieselbe Frage nicht mehrmals beantworten.
* Benutzer können nicht dieselbe Antwort für mehrere Fragen angeben.
* Bei der Definition von Fragen und Antworten kann ein beliebiger Zeichensatz verwendet werden (einschließlich Unicode).
* Die Anzahl der definierten Fragen muss größer als oder gleich der Anzahl der für die Registrierung erforderlichen Fragen sein.

## <a name="email-address"></a>E-Mail-Adresse

Eine E-Mail-Adresse kann nicht als direkte Authentifizierungsmethode verwendet werden. Die E-Mail-Adresse steht nur bei der Self-Service-Kennwortzurücksetzung (SSPR) als Prüfoption zur Verfügung. Wenn bei der Self-Service-Kennwortzurücksetzung (SSPR) die E-Mail-Adresse ausgewählt wird, wird eine E-Mail an den Benutzer gesendet, um den Authentifizierungs-/Prüfvorgang abzuschließen.

Bei der Registrierung für SSPR gibt der Benutzer die zu verwendende E-Mail-Adresse an. Es empfiehlt sich, nicht das geschäftliche E-Mail-Konto, sondern ein anderes E-Mail-Konto zu verwenden, um bei der Self-Service-Kennwortzurücksetzung den Zugriff auf die E-Mails zu gewährleisten.

## <a name="app-passwords"></a>App-Kennwörter

Bestimmte ältere, nicht browserbasierte Apps kennen keine Pausen oder Unterbrechungen im Authentifizierungsvorgang. Wenn ein Benutzer für die mehrstufige Authentifizierung aktiviert ist und versucht, eine dieser älteren, nicht browserbasierten Apps zu verwenden, kann er sich in der Regel nicht erfolgreich authentifizieren. Ein App-Kennwort bietet den Benutzern die Möglichkeit, sich erfolgreich bei älteren, nicht browserbasierten Apps ohne Unterbrechung zu authentifizieren.

Standardmäßig können Benutzer keine App-Kennwörter erstellen. Wenn Sie Benutzern erlauben müssen, App-Kennwörter zu erstellen, aktivieren Sie für die Azure Multi-Factor Authentication-Eigenschaften des Benutzers unter **Diensteinstellungen** die Option *Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei Anwendungen gestatten, die nicht auf Browsern basieren*.

![Screenshot des Azure-Portals mit den Diensteinstellungen für die mehrstufige Authentifizierung und der Erlaubnis für den Benutzer, App-Kennwörter zu erstellen](media/concept-authentication-methods/app-password-authentication-method.png)

Wenn Sie Azure Multi-Factor Authentication über Richtlinien für bedingten Zugriff und nicht über die mehrstufige Authentifizierung pro Benutzer erzwingen, können Sie keine App-Kennwörter erstellen. Moderne Anwendungen, die Richtlinien für bedingten Zugriff für die Zugriffssteuerung verwenden, benötigen keine App-Kennwörter.

Wenn Ihre Organisation im Verbund mit Azure AD das einmalige Anmelden (SSO) nutzt und Sie Azure Multi-Factor Authentication verwenden, gelten folgende Überlegungen:

* Das App-Kennwort wird von Azure AD überprüft; somit wird der Verbund umgangen. Der Verbund wird nur beim Einrichten von App-Kennwörtern verwendet. Die Kennwörter der SSO-Verbundbenutzer werden in der Organisations-ID gespeichert. Wenn ein Benutzer das Unternehmen verlässt, muss diese Information mit DirSync der Organisations-ID hinzugefügt werden. Nach Deaktivierungs- oder Löschereignissen kann die Synchronisierung bis zu drei Stunden dauern, sodass sich das Deaktivieren bzw. Löschen von App-Kennwörtern in Azure AD verzögert.
* Lokale Einstellungen für die Clientzugriffssteuerung werden von App-Kennwörtern nicht berücksichtigt.
* Für App-Kennwörter ist keine lokale Funktion zur Protokollierung oder Überwachung der Authentifizierung verfügbar.
* Bestimmte erweiterte Architekturentwürfe setzen möglicherweise die Verwendung einer Kombination aus Organisationsbenutzernamen und Kennwörtern und App-Kennwörtern bei der mehrstufigen Authentifizierung voraus, je nachdem, wo die Authentifizierung erfolgt.
    * Bei Clients, die sich bei einer lokalen Infrastruktur authentifizieren, verwenden Sie einen Organisationsbenutzernamen und ein Organisationskennwort.
    * Für Clients, die bei Azure AD authentifizieren, verwenden Sie das App-Kennwort.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einstieg finden Sie im Tutorial zur [Self-Service-Kennwortzurücksetzung (SSPR)][tutorial-sspr] und im Tutorial zu [Azure Multi-Factor Authentication][tutorial-azure-mfa].

Weitere Informationen zu SSPR-Konzepten finden Sie unter [So funktioniert's: Self-Service-Kennwortzurücksetzung in Azure AD][concept-sspr].

Weitere Informationen zu MFA-Konzepten finden Sie unter [Funktionsweise von Azure Multi-Factor Authentication][concept-mfa].

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
