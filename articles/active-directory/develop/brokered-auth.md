---
title: Brokerauthentifizierung in Android | Azure
titlesuffix: Microsoft identity platform
description: Eine Übersicht über Brokerauthentifizierung und Autorisierung für Android in der Microsoft Identity Platform
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/14/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman, hahamil, brianmel
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5204ad71efa2587341600d2c5c1e5195d15445e
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74843715"
---
# <a name="brokered-authentication-in-android"></a>Brokerauthentifizierung in Android

Sie müssen einen der Authentifizierungsbroker von Microsoft verwenden, um am geräteweiten einmaligen Anmelden (Single Sign-On, SSO) teilnehmen und die Organisationsrichtlinien für bedingten Zugriff erfüllen zu können. Die Integration eines Brokers bietet die folgenden Vorteile:

- Einmaliges Anmelden für Geräte
- Bedingter Zugriff für:
  - Intune-App-Schutz
  - Geräteregistrierung (Workplace Join)
  - Verwaltung mobiler Geräte
- Geräteweite Kontenverwaltung
  -  über Android-AccountManager und Kontoeinstellungen
  - „Geschäftskonto“ – benutzerdefinierter Kontotyp

Unter Android ist der Microsoft-Authentifizierungsbroker eine Komponente, die in der [Microsoft Authenticator-App](https://play.google.com/store/apps/details?id=com.azure.authenticator) und im [Intune-Unternehmensportal](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal) enthalten ist.

> [!TIP]
> Es ist jeweils nur eine den Broker hostende Anwendung als Broker aktiv. Welche Anwendung als Broker aktiv ist, hängt von der Installationsreihenfolge auf dem Gerät ab. Die erste zu installierende oder die letzte auf dem Gerät vorhandene Anwendung wird zum aktiven Broker.

Im folgenden Diagramm wird die Beziehung zwischen Ihrer App, der Microsoft Authentication Library (MSAL) und den Authentifizierungsbrokern von Microsoft veranschaulicht.

![Brokerbereitstellungsdiagramm](./media/brokered-auth/brokered-deployment-diagram.png)

## <a name="installing-apps-that-host-a-broker"></a>Installieren von Apps, die einen Broker hosten

Brokerhosting-Apps können vom Gerätebesitzer jederzeit über den App Store (in der Regel Google Play Store) installiert werden. Einige APIs (Ressourcen) sind jedoch durch Richtlinien für bedingten Zugriff geschützt, die erfordern, dass Geräte:

- Registriert (in den Arbeitsplatz eingebunden) sind und/oder
- Bei der Geräteverwaltung registriert sind oder
- Bei Intune-App-Schutz registriert sind

Wenn auf einem Gerät noch keine Broker-App installiert ist, weist MSAL den Benutzer an, eine zu installieren, sobald die App versucht, ein Token interaktiv abzurufen. Anschließend muss die App den Benutzer durch die entsprechenden Schritte führen, um sicherzustellen, dass das Gerät der erforderlichen Richtlinie entspricht.

## <a name="effects-of-installing-and-uninstalling-a-broker"></a>Auswirkungen der Installation und Deinstallation eines Brokers

### <a name="when-a-broker-is-installed"></a>Installation eines Brokers

Wenn ein Broker auf einem Gerät installiert ist, werden alle nachfolgenden interaktiven Tokenanforderungen (Aufrufe von `acquireToken()`) vom Broker und nicht lokal von MSAL verarbeitet. Ein zuvor für MSAL verfügbarer SSO-Zustand ist für den Broker nicht verfügbar. Folglich muss sich der Benutzer erneut authentifizieren oder ein Konto aus der vorhandenen Liste der dem Gerät bekannten Konten auswählen.

Die Installation eines Brokers erfordert keine erneute Anmeldung des Benutzers. Nur wenn der Benutzer eine `MsalUiRequiredException` auflösen muss, wird die nächste Anforderung an den Broker weitergeleitet. `MsalUiRequiredException` wird aus verschiedenen Gründen ausgelöst und muss interaktiv aufgelöst werden. Nachfolgend werden einige häufige Gründe aufgeführt:

- Der Benutzer hat das dem Konto zugewiesene Kennwort geändert.
- Das Konto des Benutzers erfüllt keine Richtlinie für bedingten Zugriff mehr.
- Der Benutzer hat seine Zustimmung zur Verknüpfung der App mit seinem Konto widerrufen.

### <a name="when-a-broker-is-uninstalled"></a>Deinstallation eines Brokers

Wenn nur eine Brokerhosting-App installiert ist und diese entfernt wird, muss sich der Benutzer erneut anmelden. Bei der Deinstallation des aktiven Brokers werden das Konto und die zugeordneten Token vom Gerät entfernt.

Wenn Intune-Unternehmensportal installiert ist und als aktiver Broker fungiert und Microsoft Authenticator ebenfalls installiert ist, muss sich der Benutzer im Falle der Deinstallation von Intune-Unternehmensportal (des aktiven Brokers) erneut anmelden. Nach der erneuten Anmeldung des Benutzers wird die Microsoft Authenticator-App zum aktiven Broker.

## <a name="integrating-with-a-broker"></a>Integration eines Brokers

### <a name="generating-a-redirect-uri-for-a-broker"></a>Generieren eines Umleitungs-URI für einen Broker

Sie müssen einen Umleitungs-URI registrieren, der mit dem Broker kompatibel ist. Der Umleitungs-URI für den Broker muss den Paketnamen Ihrer App sowie die Base64-codierte Darstellung der Signatur Ihrer App enthalten.

Das Format des Umleitungs-URI lautet wie folgt: `msauth://<yourpackagename>/<base64urlencodedsignature>`

Generieren Sie Ihre Base64-URL-codierte Signatur mithilfe der Signaturschlüssel Ihrer App. Im Folgenden finden Sie einige Beispielbefehle, die Ihre Debugsignaturschlüssel verwenden:

#### <a name="macos"></a>macOS

```bash
keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64
```

#### <a name="windows"></a>Windows

```powershell
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
```

Informationen zum Signieren von Apps finden Sie unter [Signieren Ihrer App](https://developer.android.com/studio/publish/app-signing).

> [!IMPORTANT]
> Verwenden Sie Ihren Produktionssignaturschlüssel für die Produktionsversion Ihrer App.

### <a name="configure-msal-to-use-a-broker"></a>Konfigurieren von MSAL für die Verwendung eines Brokers

Wenn Sie einen Broker in Ihrer App verwenden möchten, müssen Sie nachweisen, dass Sie Ihre Brokerumleitung konfiguriert haben. Fügen Sie beispielsweise Ihren brokeraktivierten Umleitungs-URI ein, und geben Sie an, dass Sie ihn registriert haben, indem Sie Folgendes in Ihre MSAL-Konfigurationsdatei einfügen:

```javascript
"redirect_uri" : "<yourbrokerredirecturi>",
"broker_redirect_uri_registered": true
```

> [!TIP]
> Mit der neuen Benutzeroberfläche für App-Registrierungen im Azure-Portal können Sie den Umleitungs-URI für den Broker generieren. Wenn Sie Ihre App über die ältere Benutzeroberfläche oder über das Microsoft-App-Registrierungsportal registriert haben, müssen Sie möglicherweise im Portal den Umleitungs-URI manuell generieren und die Liste der Umleitungs-URIs aktualisieren.

### <a name="broker-related-exceptions"></a>Brokerbezogene Ausnahmen

MSAL kommuniziert mit dem Broker auf zwei Arten:

- Brokergebundener Dienst
- Android-AccountManager

MSAL verwendet zuerst den brokergebundenen Dienst, weil für das Aufrufen dieses Diensts keine Android-Berechtigungen erforderlich sind. Wenn die Bindung an den gebundenen Dienst fehlschlägt, verwendet MSAL die Android-AccountManager-API. MSAL geht nur so vor, wenn Ihrer App bereits die Berechtigung `"READ_CONTACTS"` erteilt wurde.

Wenn Sie eine `MsalClientException` mit dem Fehlercode `"BROKER_BIND_FAILURE"` erhalten, haben Sie zwei Möglichkeiten:

- Bitten Sie den Benutzer, die Energieoptimierung für die Microsoft Authenticator-App und das Intune-Unternehmensportal zu deaktivieren.
- Bitten Sie den Benutzer, die Berechtigung `"READ_CONTACTS"` zu erteilen.
