---
title: Authentifizierung von Clients, die Ereignisse in benutzerdefinierten Event Grid-Themen oder -Domänen veröffentlichen
description: Dieser Artikel beschreibt verschiedene Möglichkeiten zur Authentifizierung von Clients, die Ereignisse in benutzerdefinierten Event Grid-Themen veröffentlichen.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 8b08c46fdeadccfad0471ddfa4f9ba2762a951a0
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837396"
---
# <a name="authenticate-publishing-clients-azure-event-grid"></a>Authentifizieren von Veröffentlichungsclients (Azure Event Grid)
Dieser Artikel enthält Informationen zur Authentifizierung von Clients, die Ereignisse zu Azure Event Grid-Themen oder -Domänen mit **Zugangsschlüssel** oder **SAS-Token (Shared Access Signature)** veröffentlichen. Es wird empfohlen, SAS-Token zu verwenden, Schlüsselauthentifizierung ist aber einfacher zu programmieren und mit vielen vorhandenen Webhookherausgebern kompatibel.  

## <a name="authenticate-using-an-access-key"></a>Authentifizieren mithilfe eines Zugriffsschlüssels
Schlüsselauthentifizierung ist die einfachste Form von Authentifizierung. Sie können den Zugriffsschlüssel als HTTP-Header oder URL-Abfrageparameter übergeben. 

### <a name="access-key-in-a-http-header"></a>Zugriffsschlüssel in einem HTTP-Header
Übergeben Sie den Zugriffsschlüssel als Wert für den HTTP-Header: `aeg-sas-key`.

```
aeg-sas-key: XXXXXXXXXXXXXXXXXX0GXXX/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="access-key-as-a-query-parameter"></a>Zugriffsschlüssel als Abfrageparameter
Sie können auch `aeg-sas-key` als Abfrageparameter angeben. 

```
https://<yourtopic>.<region>.eventgrid.azure.net/api/events?aeg-sas-key=XXXXXXXX53249XX8XXXXX0GXXX/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

Anleitungen zum Abrufen von Zugriffsschlüsseln für ein Thema oder eine Domäne finden Sie unter [Abrufen von Zugriffsschlüsseln](get-access-keys.md).

## <a name="authenticate-using-a-sas-token"></a>Authentifizieren mithilfe eines SAS-Tokens
SAS-Token für eine Event Grid-Ressource schließen die Ressource, eine Ablaufzeit und eine Signatur ein. Das Format des SAS-Tokens ist: `r={resource}&e={expiration}&s={signature}`.

Die Ressource ist der Pfad für das Event Grid-Thema, an das Sie Ereignisse senden. Beispielsweise lautet ein gültiger Ressourcenpfad: `https://<yourtopic>.<region>.eventgrid.azure.net/api/events`. Alle unterstützten API-Versionen finden Sie unter [Microsoft.EventGrid-Ressourcentypen](/azure/templates/microsoft.eventgrid/allversions). 

Generieren Sie zunächst programmgesteuert ein SAS-Token, und verwenden Sie dann den `aeg-sas-token`-Header oder `Authorization SharedAccessSignature`-Header, um die Authentifizierung mit Event Grid auszuführen. 

### <a name="generate-sas-token-programmatically"></a>Programmgesteuertes Generieren von SAS-Token
Das folgende Beispiel erstellt ein SAS-Token für die Verwendung mit Event Grid:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

### <a name="using-aeg-sas-token-header"></a>Verwenden des aeg-sas-token-Headers
Im Folgenden finden Sie ein Beispiel für das Übergeben des SAS-Tokens als Wert für den `aeg-sas-toke`-Header. 

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=XXXXXXXXXXXXX%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

### <a name="using-authorization-header"></a>Verwenden des Autorisierungsheaders
Im Folgenden finden Sie ein Beispiel für das Übergeben des SAS-Tokens als Wert für den `Authorization`-Header. 

```http
Authorization: SharedAccessSignature r=https%3a%2f%2fmytopic.eventgrid.azure.net%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=XXXXXXXXXXXXX%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Authentifizierung mit Ereignishandlern für die Übermittlung von Ereignissen finden Sie unter [Ereignisübermittlungsauthentifizierung](security-authentication.md). 