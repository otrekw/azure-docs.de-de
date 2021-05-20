---
title: Kennwortlose Anmeldung mit Sicherheitsschlüsseln – Azure Active Directory
description: Aktivieren der kennwortlosen Anmeldung mit FIDO2-Sicherheitsschlüsseln bei Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 05/04/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: c164b52682d6f4aef2db70a5724f3f74db68c53f
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108746445"
---
# <a name="enable-passwordless-security-key-sign-in"></a>Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln 

Sicherheitsschlüssel bieten Unternehmen, die aktuell Kennwörter verwenden und über eine freigegebene PC-Umgebung verfügen, eine nahtlose Möglichkeit für Mitarbeiter, sich ohne Eingabe eines Benutzernamens oder Kennworts zu authentifizieren. Sicherheitsschlüssel ermöglichen den Mitarbeitern eine bessere Produktivität und bieten eine höhere Sicherheit.

In diesem Dokument liegt der Schwerpunkt auf der Aktivierung der auf Sicherheitsschlüsseln basierenden kennwortlosen Authentifizierung. Am Ende dieses Artikels können Sie sich mit Ihrem Azure AD-Konto mithilfe eines FIDO2-Sicherheitsschlüssels bei webbasierten Anwendungen anmelden.

## <a name="requirements"></a>Requirements (Anforderungen)

- [Azure AD Multi-Factor Authentication](howto-mfa-getstarted.md)
- Aktivieren der [kombinierten Registrierung von Sicherheitsinformationen](concept-registration-mfa-sspr-combined.md)
- Kompatible [FIDO2-Sicherheitsschlüssel](concept-authentication-passwordless.md#fido2-security-keys)
- WebAuthN erfordert Windows 10, Version 1903 oder höher**

Um Sicherheitsschlüssel für die Anmeldung bei Web-Apps und-Diensten verwenden zu können, benötigen Sie einen Browser, der das WebAuthN-Protokoll unterstützt. Hierzu zählen Microsoft Edge, Chrome, Firefox und Safari.


## <a name="prepare-devices"></a>Vorbereiten von Geräten

Für in Azure AD eingebundene Geräte eignet sich besonders Windows 10, Version 1903 oder höher.

Auf hybrid in Azure AD eingebundenen Geräten muss Windows 10, Version 2004 oder höher, ausgeführt werden.

## <a name="enable-passwordless-authentication-method"></a>Aktivieren von Methoden zur kennwortlosen Authentifizierung

### <a name="enable-the-combined-registration-experience"></a>Aktivieren der kombinierten Registrierung

Die Registrierungsfunktionen für Methoden zur kennwortlosen Authentifizierung sind von der kombinierten Registrierungsfunktion abhängig. Führen Sie die im Artikel [Aktivieren der kombinierten Registrierung von Sicherheitsinformationen](howto-registration-mfa-sspr-combined.md) aufgeführten Schritte aus, um die kombinierte Registrierung zu aktivieren.

### <a name="enable-fido2-security-key-method"></a>Aktivieren der Methode „FIDO2-Sicherheitsschlüssel“

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Authentifizierungsmethoden** > **Authentifizierungsmethodenrichtlinie**.
1. Wählen Sie unter der Methode **FIDO2-Sicherheitsschlüssel** die folgenden Optionen aus:
   1. **Aktivieren**: „Ja“ oder „Nein“
   1. **Ziel**: „Alle Benutzer“ oder „Benutzer auswählen“
1. **Speichern** Sie die Konfiguration.


### <a name="fido-security-key-optional-settings"></a>Optionale Einstellungen für FIDO-Sicherheitsschlüssel 

Es gibt einige optionale Einstellungen für die Verwaltung von Sicherheitsschlüsseln pro Mandant.  

![Screenshot: FIDO2-Sicherheitsschlüsseloptionen](media/howto-authentication-passwordless-security-key/optional-settings.png) 

**Allgemein**

- **Self-Service-Einrichtung zulassen** sollte auf **Ja** festgelegt bleiben. Wenn diese Einstellung auf „Nein“ festgelegt ist, können Ihre Benutzer keinen FIDO-Schlüssel über das MySecurityInfo-Portal registrieren, auch wenn dies durch die Richtlinie für Authentifizierungsmethoden aktiviert ist.  
- Die **Nachweis erzwingen**-Einstellung auf **Ja** erfordert, dass die FIDO-Sicherheitsschlüsselmetadaten mit der FIDO Alliance Metadata Service veröffentlicht und überprüft werden, und sie müssen auch den zusätzlichen Überprüfungstest von Microsoft bestehen. Weitere Informationen finden Sie unter [Was ist ein Microsoft-kompatibler Sicherheitsschlüssel?](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key)

**Schlüsseleinschränkungsrichtlinie**

- **Erzwingung von Schlüsseleinschränkungen** sollte nur auf **Ja** festgelegt werden, wenn Ihre Organisation nur bestimmte FIDO-Sicherheitsschlüssel zulassen oder nicht zulassen möchte, die durch ihre AAGuids identifiziert werden. Sie können mit Ihrem Sicherheitsschlüsselanbieter zusammenarbeiten, um die AAGuids ihrer Geräte zu bestimmen. Wenn der Schlüssel bereits registriert ist, kann AAGUID auch durch Anzeigen der Details der Authentifizierungsmethode des Schlüssels pro Benutzer gefunden werden. 

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Benutzerregistrierung und Verwaltung von FIDO2-Sicherheitsschlüsseln

1. Navigieren Sie zu [https://myprofile.microsoft.com](https://myprofile.microsoft.com).
1. Melden Sie sich an, falls Sie dies noch nicht getan haben.
1. Klicken Sie auf **Sicherheitsinformation**.
   1. Wenn der Benutzer bereits mindestens eine Azure AD Multi-Factor Authentication-Methode registriert hat, kann er sofort einen FIDO2-Sicherheitsschlüssel registrieren.
   1. Wenn der Benutzer keine Azure AD Multi-Factor Authentication-Methode registriert hat, muss er eine solche hinzufügen.
1. Fügen Sie einen FIDO2-Sicherheitsschlüssel hinzu, indem Sie auf **Methode hinzufügen** klicken und dann **Sicherheitsschlüssel** auswählen.
1. Wählen Sie **USB-Gerät** oder **NFC-Gerät** aus.
1. Halten Sie Ihren Schlüssel bereit, und wählen Sie **Weiter** aus.
1. Im daraufhin angezeigten Feld wird der Benutzer aufgefordert, eine PIN für den Sicherheitsschlüssel zu erstellen/einzugeben und dann die erforderliche Geste für den Schlüssel biometrisch oder per Touch auszuführen.
1. Der Benutzer gelangt wieder zurück zur Oberfläche für die kombinierte Registrierung und wird aufgefordert, einen aussagekräftigen Namen für den Schlüssel anzugeben, damit der Benutzer diesen von den anderen unterscheiden kann. Klicken Sie auf **Weiter**.
1. Klicken Sie auf **Fertig**, um den Vorgang abzuschließen.

## <a name="sign-in-with-passwordless-credential"></a>Anmelden mit kennwortlosen Anmeldeinformationen

Im nachfolgenden Beispiel hat der Benutzer bereits einen FIDO2-Sicherheitsschlüssel bereitgestellt. Der Benutzer kann sich im Web mit seinem FIDO2-Sicherheitsschlüssel in einem unterstützten Browser unter Windows 10, Version 1903 oder höher, anmelden.

![Anmeldung mit Sicherheitsschlüssel in Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="troubleshooting-and-feedback"></a>Problembehandlung und Feedback

Wenn Sie Feedback geben möchten oder Probleme mit diesem Feature auftreten, teilen Sie uns dies in folgenden Schritten über die Windows-Feedback-Hub-App mit:

1. Starten Sie **Feedback-Hub**, und stellen Sie sicher, dass Sie angemeldet sind.
1. Senden Sie Ihr Feedback unter der folgenden Kategorisierung:
   - Kategorie: Sicherheit und Datenschutz
   - Unterkategorie: FIDO
1. Verwenden Sie die Option **Problem reproduzieren**, um Protokolle zu erfassen.

## <a name="known-issues"></a>Bekannte Probleme

### <a name="security-key-provisioning"></a>Bereitstellung von Sicherheitsschlüsseln

Die Administratorbereitstellung und Bereitstellungsaufhebung von Sicherheitsschlüsseln ist nicht verfügbar.

### <a name="cached-logon-on-hybrid-azure-ad-joined-devices"></a>Zwischengespeicherte Anmeldung für hybride, in Azure AD eingebundene Geräte

Bei der zwischengespeicherten Anmeldung mit FIDO2-Schlüsseln kommt es für hybride, in Azure AD eingebundene Geräte unter Windows 10, Version 20H2 zu einem Fehler. Dadurch können sich Benutzer nicht anmelden, wenn keine Sichtverbindung mit dem lokalen Domänencontroller verfügbar ist. Dies wird derzeit geprüft.

### <a name="upn-changes"></a>Änderungen des Benutzerprinzipalnamens

Wir arbeiten derzeit an der Unterstützung eines Features, das eine Änderung des Benutzerprinzipalnamens auf Geräten mit Azure AD Hybrid- und Azure AD-Einbindung ermöglicht. Wenn der Benutzerprinzipalname eines Benutzers geändert wird, können Sie die FIDO2-Sicherheitsschlüssel nicht mehr ändern, um dies zu berücksichtigen. Zur Lösung dieses Problems muss das Gerät zurückgesetzt werden, und der Benutzer muss die Registrierung erneut ausführen.

## <a name="next-steps"></a>Nächste Schritte

[Anmelden mit FIDO2-Sicherheitsschlüsseln bei Windows 10-Geräten](howto-authentication-passwordless-security-key-windows.md)

[Aktivieren der FIDO2-Authentifizierung für lokale Ressourcen](howto-authentication-passwordless-security-key-on-premises.md)

[Erfahren Sie mehr über die Geräteregistrierung](../devices/overview.md)

[Erfahren Sie mehr über Azure AD Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
