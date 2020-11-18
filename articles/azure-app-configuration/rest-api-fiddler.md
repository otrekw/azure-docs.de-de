---
title: Azure Active Directory-REST-API – Testen mit Fiddler
description: Verwenden von Fiddler zum Testen der Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 3766567fe58e8d2eb86556d3defa7a85efd9b2fb
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423679"
---
# <a name="test-the-azure-app-configuration-rest-api-using-fiddler"></a>Testen der Azure App Configuration-REST-API mit Fiddler

Zum Testen der REST-API mit [Fiddler](https://www.telerik.com/fiddler) müssen Sie die zur [Authentifizierung](./rest-api-authentication-hmac.md) erforderlichen HTTP-Header in Ihre Anforderungen einfügen. So konfigurieren Sie Fiddler zum Testen der REST-API und automatischen Generieren der Authentifizierungsheader:

1. Stellen Sie sicher, dass TLS 1.2 ein zulässiges Protokoll ist:
    1. Wechseln Sie zu **Tools** > **Options** > **HTTPS** (Tools > Optionen > HTTPS).
    1. Stellen Sie sicher, dass **Decrypt HTTPS traffic** (HTTPS-Datenverkehr entschlüsseln) aktiviert ist.
    1. Fügen Sie der Liste der Protokolle **tls1.2** hinzu, falls nicht vorhanden.
1. Öffnen Sie **Fiddler Script Editor** (Fiddler-Skript-Editor), oder drücken Sie **STRG+R** in Fiddler.
1. Fügen Sie innerhalb der `Handlers`-Klasse vor der `OnBeforeRequest`-Funktion den folgenden Code hinzu.

    ```js
    static function SignRequest(oSession: Session, credential: String, secret: String) {
        var utcNow = DateTimeOffset.UtcNow.ToString("r", System.Globalization.DateTimeFormatInfo.InvariantInfo);
        var contentHash = ComputeSHA256Hash(oSession.RequestBody);
        var stringToSign = oSession.RequestMethod.ToUpperInvariant() + "\n" + oSession.PathAndQuery + "\n" + utcNow +";" + oSession.hostname + ";" + contentHash;
        var signature = ComputeHMACHash(secret, stringToSign);

        oSession.oRequest.headers["x-ms-date"] = utcNow;
        oSession.oRequest.headers["x-ms-content-sha256"] = contentHash;
        oSession.oRequest.headers["Authorization"] = "HMAC-SHA256 Credential=" + credential + "&SignedHeaders=x-ms-date;host;x-ms-content-sha256&Signature=" + signature;
    }

    static function ComputeSHA256Hash(content: Byte[]) {
        var sha256 = System.Security.Cryptography.SHA256.Create();
        try {
            return Convert.ToBase64String(sha256.ComputeHash(content));
        }
        finally {
            sha256.Dispose();
        }
    }

    static function ComputeHMACHash(secret: String, content: String) {
        var hmac = new System.Security.Cryptography.HMACSHA256(Convert.FromBase64String(secret));
        try {
            return Convert.ToBase64String(hmac.ComputeHash(System.Text.Encoding.ASCII.GetBytes(content)));
        }
        finally {
            hmac.Dispose();
        }
    }
    ```

1. Fügen Sie am Ende der `OnBeforeRequest`-Funktion den folgenden Code hinzu. Aktualisieren Sie den Zugriffsschlüssel, wie im TODO-Kommentar angegeben.

    ```js
    if (oSession.isFlagSet(SessionFlags.RequestGeneratedByFiddler) &&
        oSession.hostname.EndsWith(".azconfig.io", StringComparison.OrdinalIgnoreCase)) {

        // TODO: Replace the following placeholders with your access key
        var credential = "<Credential>"; // Id
        var secret = "<Secret>"; // Value

        SignRequest(oSession, credential, secret);
    }
    ```
1. Verwenden Sie den [Composer von Fiddler](https://docs.telerik.com/fiddler/Generate-Traffic/Tasks/CreateNewRequest), um eine Anforderung zu generieren und zu senden.
