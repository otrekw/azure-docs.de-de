---
title: Anmeldeinformationen für das Microsoft Identity Platform-Zertifikat
titleSuffix: Microsoft identity platform
description: In diesem Artikel werden die Registrierung für die Anwendungsauthentifizierung und die Verwendung von Zertifikatanmeldeinformationen für diesen Zweck beschrieben.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/12/2020
ms.author: hirsin
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6330621aac78d5e9df52f2cd3ad9c3968bb0120d
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88853379"
---
# <a name="microsoft-identity-platform-application-authentication-certificate-credentials"></a>Microsoft Identity Platform-Zertifikatanmeldeinformationen für die Anwendungsauthentifizierung

Microsoft Identity Platform ermöglicht einer Anwendung, ihre eigenen Anmeldeinformationen für die Authentifizierung zu verwenden, zum Beispiel im [OAuth 2.0-Flow zum Gewähren von Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md) und im [OBO-Flow](v2-oauth2-on-behalf-of-flow.md) (On-Behalf-Of).

Eine Form von Anmeldeinformationen, die eine Anwendung zur Authentifizierung verwenden kann, ist eine [JSON Web Token (JWT)-Assertion](./security-tokens.md#json-web-tokens-jwts-and-claims), die mit einem zur Anwendung gehörigen Zertifikat signiert ist.

## <a name="assertion-format"></a>Assertionformat

Für die Berechnung der Assertion können Sie eine der zahlreichen JWT-Bibliotheken in der Sprache Ihrer Wahl nutzen. Die Informationen sind im [Header](#header), den [Ansprüchen](#claims-payload) und der [Signatur](#signature) des Tokens enthalten.

### <a name="header"></a>Header

| Parameter |  Anmerkung |
| --- | --- |
| `alg` | Muss **RS256** sein. |
| `typ` | Muss **JWT** sein. |
| `x5t` | Die als Base64-Zeichenfolgenwert codierte Hexadezimaldarstellung des X.509-Zertifikathashs (auch als SHA-1-*Fingerabdruck* des Zertifikats bezeichnet). Beim X.509-Zertifikathash `84E05C1D98BCE3A5421D225B140B36E86A3D5534` (Hexadezimaldarstellung) würde der `x5t`-Anspruch beispielsweise `hOBcHZi846VCHSJbFAs26Go9VTQ=` (Base64) lauten. |

### <a name="claims-payload"></a>Ansprüche (Nutzlast)

| Parameter |  Bemerkungen |
| --- | --- |
| `aud` | Audience: Muss `https://login.microsoftonline.com/<your-tenant-id>/oauth2/token` lauten |
| `exp` | Ablaufdatum: Das Datum, an dem das Token abläuft. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt dargestellt, an dem die Gültigkeit des Tokens abläuft. Es wird empfohlen, eine kurze Ablaufzeit von 10 Minuten bis zu einer Stunde zu verwenden.|
| `iss` | Aussteller: Muss die Client-ID (*Anwendungs (Client)-ID* des Clientdiensts) sein |
| `jti` | GUID: Die JWT-ID |
| `nbf` | Nicht vor: Datum, vor dem das Token nicht verwendet werden kann. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Erstellung der Assertion dargestellt. |
| `sub` | Antragsteller: Wie bei Aussteller (`iss`). Muss die Client-ID (*Anwendungs (Client)-ID* des Clientdiensts) sein |

### <a name="signature"></a>Signatur

Zur Berechnung der Signatur wird das Zertifikat wie in der [Spezifikation RFC7519 für JSON-Webtoken](https://tools.ietf.org/html/rfc7519) beschrieben angewendet.

## <a name="example-of-a-decoded-jwt-assertion"></a>Beispiel einer decodierten JWT-Assertion

```JSON
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="example-of-an-encoded-jwt-assertion"></a>Beispiel einer codierten JWT-Assertion

Die folgende Zeichenfolge ist ein Beispiel für eine codierte Assertion. Bei genauer Betrachtung sehen Sie drei Abschnitte, die jeweils durch einen Punkt getrennt sind (`.`):

* Im ersten Abschnitt ist der *Header* codiert.
* Im zweiten Abschnitt sind die *Ansprüche* (Nutzlast) codiert.
* Der letzte Abschnitt enthält die *Signatur*, die mit den Zertifikaten aus dem Inhalt der ersten beiden Abschnitte berechnet wurde.

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-microsoft-identity-platform"></a>Registrieren Ihres Zertifikats bei Microsoft Identity Platform

Über das Azure-Portal können Sie die Zertifikatanmeldeinformationen mit der Clientanwendung in Microsoft Identity Platform verknüpfen. Dazu haben Sie folgende Möglichkeiten:

### <a name="uploading-the-certificate-file"></a>Hochladen der Zertifikatdatei

In der Azure-App-Registrierung für die Clientanwendung:
1. Wählen Sie **Zertifikate & Geheimnisse** aus.
2. Klicken Sie auf **Zertifikat hochladen**, und wählen Sie die Zertifikatdatei zum Hochladen aus.
3. Klicken Sie auf **Hinzufügen**.
  Nachdem das Zertifikat hochgeladen wurde, werden der Fingerabdruck, das Startdatum und der Ablaufzeitpunkt angezeigt.

### <a name="updating-the-application-manifest"></a>Aktualisieren des Anwendungsmanifests

Wenn Sie über ein Zertifikat verfügen, berechnen Sie Folgendes:

- `$base64Thumbprint`: Base64-codierter Wert des Zertifikathashs
- `$base64Value`: Base64-codierter Wert der Zertifikatrohdaten

Geben Sie außerdem eine GUID an, um den Schlüssel im Anwendungsmanifest (`$keyId`) zu identifizieren.

In der Azure-App-Registrierung für die Clientanwendung:
1. Wählen Sie **Manifest** aus, um das Anwendungsmanifest zu öffnen.
2. Ersetzen Sie die Eigenschaft *keyCredentials* gemäß dem folgenden Schema durch Ihre neuen Zertifikatinformationen.

   ```JSON
   "keyCredentials": [
       {
           "customKeyIdentifier": "$base64Thumbprint",
           "keyId": "$keyid",
           "type": "AsymmetricX509Cert",
           "usage": "Verify",
           "value":  "$base64Value"
       }
   ]
   ```
3. Speichern Sie die Änderungen am Anwendungsmanifest, und laden Sie dann das Manifest in Microsoft Identity Platform hoch.

   Da die `keyCredentials`-Eigenschaft mehrere Werte besitzen kann, können Sie mehrere Zertifikate hochladen, um eine vielfältigere Schlüsselverwaltung zu erreichen.

## <a name="next-steps"></a>Nächste Schritte

Das Codebeispiel [.NET Core-Daemon-Konsolenanwendung mit Microsoft Identity Platform](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) auf GitHub zeigt, wie eine Anwendung ihre eigenen Anmeldeinformationen für die Authentifizierung verwendet. Zudem erfahren Sie darin, wie Sie mit dem PowerShell-Cmdlet `New-SelfSignedCertificate` ein [selbstsigniertes Zertifikat erstellen können](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph#optional-use-the-automation-script). Sie können auch die [App-Erstellungsskripts](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/AppCreationScripts-withCert/AppCreationScripts.md) im Beispielrepository verwenden, um Zertifikate zu erstellen, den Fingerabdruck zu berechnen und so weiter.
