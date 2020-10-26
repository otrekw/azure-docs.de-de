---
title: 'Azure Active Directory: Kennwortlose Anmeldung mit der Microsoft Authenticator-App'
description: Aktivieren der kennwortlosen Anmeldung bei Azure AD mit der Microsoft Authenticator-App (Vorschauversion)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/29/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 053a489993c31344b96e83253c88eed93b27b145
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91964824"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Aktivieren der kennwortlosen Anmeldung mit der Microsoft Authenticator-App (Vorschauversion)

Mit der Microsoft Authenticator-App können sich Benutzer bei jedem beliebigen Azure AD-Konto anmelden, ohne ein Kennwort zu verwenden. Ähnlich wie die Technologie von [Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-identity-verification) nutzt Microsoft Authenticator die schlüsselbasierte Authentifizierung, um die Verwendung von Benutzeranmeldeinformationen zu ermöglichen, die an ein Gerät gebunden sind und auf biometrischen Daten oder einer PIN beruhen. Diese Authentifizierungsmethode kann auf jeder Geräteplattform (einschließlich Plattformen für mobile Geräte) und mit jeder App oder Website verwendet werden, die in Microsoft-Authentifizierungsbibliotheken integriert ist.

![Beispiel für eine browserbasierte Anmeldung, die der Benutzer bestätigen muss](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Einem Benutzer, der die Anmeldung per Telefon über die Microsoft Authenticator-App aktiviert hat, wird nach dem Eingeben eines Benutzernamens keine Aufforderung zur Eingabe eines Kennworts angezeigt, sondern eine Meldung, in der er aufgefordert wird, in der App auf eine Zahl zu tippen. In der App muss der Benutzer auf die richtige Nummer tippen, **Genehmigen** auswählen und dann seine PIN eingeben oder seinen biometrischen Schlüssel verwenden, um den Anmeldevorgang abzuschließen.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, damit die kennwortlose Anmeldung per Telefon mit der Microsoft Authenticator-App verwendet werden kann:

- Azure Multi-Factor Authentication mit Pushbenachrichtigungen als Überprüfungsmethode zulässig
- Aktuelle Version von Microsoft Authenticator auf Geräten mit iOS 8.0 oder höher oder Android 6.0 oder höher installieren.

> [!NOTE]
> Wenn Sie die Vorschauversion der kennwortlosen Anmeldung mit der Microsoft Authenticator-App mithilfe von Azure AD PowerShell aktiviert haben, wurde sie für Ihr gesamtes Verzeichnis aktiviert. Wenn Sie die Aktivierung mit dieser neuen Methode vornehmen, wird die PowerShell-Richtlinie dadurch ersetzt. Es wird empfohlen, für alle Benutzer in Ihrem Mandanten die Aktivierung über das neue Menü *Authentifizierungsmethoden* vorzunehmen. Andernfalls können sich Benutzer, die nicht in der neuen Richtlinie enthalten sind, nicht mehr ohne Kennwort anmelden.

## <a name="enable-passwordless-authentication-methods"></a>Aktivieren von Methoden zur kennwortlosen Authentifizierung

Um die kennwortlose Authentifizierung in Azure AD zu verwenden, aktivieren Sie zunächst die kombinierte Registrierung, und aktivieren Sie anschließend die Benutzer für die kennwortlose Methode.

### <a name="enable-the-combined-registration-experience"></a>Aktivieren der kombinierten Registrierung

Die Registrierungsfunktionen für Methoden zur kennwortlosen Authentifizierung sind von der kombinierten Registrierungsfunktion abhängig. Damit die Benutzer die kombinierte Registrierung selbst abschließen können, führen Sie die Schritte zum [Aktivieren der kombinierten Registrierung von Sicherheitsinformationen](howto-registration-mfa-sspr-combined.md) durch.

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Aktivieren von Authentifizierungsmethoden für die kennwortlose Anmeldung per Telefon

Mit Azure AD können Sie auswählen, welche Authentifizierungsmethoden während des Anmeldevorgangs verwendet werden können. Benutzer registrieren sich anschließend für die Methoden, die sie verwenden möchten.

Führen Sie die folgenden Schritte aus, um die Authentifizierungsmethode für die kennwortlose Anmeldung per Telefon zu aktivieren:

1. Melden Sie sich mit einem globalen *Administratorkonto* im [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie *Azure Active Directory* , und wählen sie es aus. Navigieren Sie anschließend zu **Sicherheit** > **Authentifizierungsmethoden** > **Authentifizierungsmethodenrichtlinie (Vorschau)** .
1. Wählen Sie unter **Kennwortlose Anmeldung per Telefon** die folgenden Optionen aus:
   1. **Aktivieren** : „Ja“ oder „Nein“
   1. **Ziel** : „Alle Benutzer“ oder „Benutzer auswählen“
1. Wählen Sie **Speichern** aus, um die neue Richtlinie anzuwenden.

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Benutzerregistrierung und Verwaltung der Microsoft Authenticator-App

Wenn die kennwortlose Authentifizierungsmethode zur Verwendung in Azure AD verfügbar ist, müssen sich Benutzer nun mithilfe der folgenden Schritte für die kennwortlose Authentifizierungsmethode registrieren:

1. Navigieren Sie zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) .
1. Melden Sie sich an, und fügen Sie anschließend die Authenticator-App hinzu, indem Sie **Methode hinzufügen > Authenticator-App** und anschließend **Hinzufügen** auswählen.
1. Folgen Sie den Anweisungen zum Installieren und Konfigurieren der Microsoft Authenticator-App auf Ihrem Gerät.
1. Wählen Sie **Fertig** aus, um die Authenticator-Konfiguration abzuschließen.
1. Wählen Sie in der **Microsoft Authenticator** -App im Dropdownmenü für das registrierte Konto die Option **Anmeldung per Telefon aktivieren** aus.
1. Folgen Sie den Anweisungen in der App, um die Registrierung für die kennwortlose Anmeldung per Telefon für das Konto abzuschließen.

Organisationen können ihre Benutzer auf den Artikel [Anmelden an Ihren Konten per Microsoft Authenticator-App](../user-help/user-help-auth-app-sign-in.md) verweisen, der weitere Unterstützung für die Konfiguration der Microsoft Authenticator-App und die Aktivierung der Anmeldung per Telefon bietet.

> [!NOTE]
> Benutzer, die gemäß der Richtlinie nicht für die Verwendung der Anmeldung per Telefon berechtigt sind, können sie in der Microsoft Authenticator-App nicht mehr aktivieren.  

## <a name="sign-in-with-passwordless-credential"></a>Anmelden mit kennwortlosen Anmeldeinformationen

Ein Benutzer kann die kennwortlose Anmeldung erst dann verwenden, wenn ein Administrator das Feature für den jeweiligen Mandanten aktiviert und der Benutzer die Microsoft Authenticator-App aktualisiert hat, um die Anmeldung per Telefon zu aktivieren.

Um mit der Verwendung der Anmeldung per Telefon zu beginnen, müssen Benutzer, nachdem sie auf der Anmeldeseite einen Benutzernamen eingegeben und **Weiter** ausgewählt haben, unter Umständen **Weitere Anmeldemethoden** und anschließend **Anforderung in meiner Microsoft Authenticator-App genehmigen** auswählen. Benutzern wird daraufhin eine Zahl angezeigt, und sie werden in der Microsoft Authenticator-App aufgefordert, die entsprechende Zahl für die Authentifizierung auszuwählen, statt ihr Kennwort zu verwenden. Nachdem Benutzer die kennwortlose Anmeldung per Telefon verwendet haben, werden sie aufgefordert, sie wieder zu verwenden, bis sie eine andere Methode auswählen.

![Beispiel für eine Browseranmeldung mit der Microsoft Authenticator-App](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Bekannte Probleme

Die folgenden bekannten Probleme können in der aktuellen Vorschauversion auftreten.

### <a name="not-seeing-option-for-passwordless-phone-sign-in"></a>Die Option für die kennwortlose Anmeldung per Telefon wird nicht angezeigt

Wenn bei einem Benutzer die Überprüfung der kennwortlosen Anmeldung per Telefon aussteht und er versucht, sich erneut anzumelden, wird dem Benutzer stattdessen unter Umständen nur eine Option zum Eingeben eines Kennworts angezeigt. Öffnen Sie Microsoft Authenticator, und reagieren Sie auf alle Benachrichtigungsaufforderungen, um die kennwortlose Anmeldung per Telefon weiter zu verwenden.

### <a name="federated-accounts"></a>Verbundkonten

Wenn ein Benutzer kennwortlose Anmeldeinformationen aktiviert hat, verwenden Azure AD-Anmeldungen „login_hint“ nicht mehr, um den Standort der Verbundanmeldung für den Benutzer zu beschleunigen. Diese Logik verhindert, dass ein Benutzer in einem Hybridmandanten zur Überprüfung der Anmeldung zu Active Directory-Verbunddiensten (AD FS) weitergeleitet wird, ohne dass der Benutzer einen zusätzlichen Schritt zum Klicken auf „Stattdessen Ihr Kennwort verwenden“ ausführt.

### <a name="azure-mfa-server"></a>Azure MFA-Server

Endbenutzer, für die MFA über den lokalen Azure MFA-Server einer Organisation aktiviert ist, können weiterhin einen einzelnen Satz von Anmeldeinformationen für die Anmeldung per Smartphone ohne Kennwort erstellen und verwenden. Wenn der Benutzer versucht, mehrere Installationen (fünf oder mehr) von Microsoft Authenticator mit den Anmeldeinformationen zu aktualisieren, kann diese Änderung zu einem Fehler führen.  

### <a name="device-registration"></a>Geräteregistrierung

Eine der Voraussetzungen für die Erstellung dieser neuen, sicheren Anmeldeinformationen ist, dass das Gerät, auf dem die Microsoft Authenticator-App installiert ist, auch im Azure AD-Mandanten für einen einzelnen Benutzer registriert sein muss. Aufgrund der aktuellen Einschränkungen, die für die Registrierung von Geräten gelten, kann ein Gerät nur in einem einzigen Mandanten registriert werden. Dies bedeutet, dass nur ein Geschäfts-, Schul- oder Unikonto in der Microsoft Authenticator-App für die Anmeldung per Smartphone aktiviert werden kann.

> [!NOTE]
> Die Geräteregistrierung ist nicht dasselbe wie die Geräteverwaltung oder „MDM“. Sie ordnet nur eine Geräte-ID und eine Benutzer-ID zusammen im Azure AD-Verzeichnis zu.  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure AD-Authentifizierung und kennwortlosen Methoden finden Sie in den folgenden Artikeln:

* [Wie funktioniert die kennwortlose Authentifizierung?](concept-authentication-passwordless.md)
* [Informationen zur Geräteregistrierung](../devices/overview.md#getting-devices-in-azure-ad)
* [Informationen zu Microsoft Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
