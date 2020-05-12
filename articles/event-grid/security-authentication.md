---
title: Azure Event Grid – Sicherheit und Authentifizierung
description: In diesem Artikel werden verschiedene Methoden zum Authentifizieren des Zugriffs auf Ihre Event Grid Ressourcen (Webhook, Abonnements, benutzerdefinierte Themen) beschrieben.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: babanisa
ms.openlocfilehash: 528c3613549ee49009f99d45e5bd9c2cf1745d78
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82779993"
---
# <a name="authenticating-access-to-azure-event-grid-resources"></a>Authentifizieren des Zugriffs auf Azure Event Grid-Ressourcen
Dieser Artikel enthält Informationen zu den folgenden Szenarien:  

- Authentifizieren von Clients, die mithilfe einer Shared Access Signature (SAS) oder eines Schlüssels Ereignisse in Azure Event Grid-Themen veröffentlichen. 
- Schützen Ihres Webhook-Endpunkts mit Azure Active Directory (Azure AD), um Event Grid für die **Übermittlung** von Ereignissen an den Endpunkt zu authentifizieren.

## <a name="authenticate-publishing-clients-using-sas-or-key"></a>Authentifizieren von Veröffentlichungsclients mit einer SAS oder einem Schlüssel
Benutzerdefinierte Themen verwenden Authentifizierungen über Shared Access Signatures (SAS) oder Schlüssel. Wir empfehlen SAS, aber die Schlüsselauthentifizierung ist einfacher zu programmieren und mit vielen vorhandenen Webhookherausgebern kompatibel.

Sie schließen den Authentifizierungswert in den HTTP-Header ein. Verwenden Sie für SAS den Wert **aeg-sas-token** als Headerwert. Verwenden Sie für die Schlüsselauthentifizierung den Wert **aeg-sas-key** als Headerwert.

### <a name="key-authentication"></a>Schlüsselauthentifizierung

Die Schlüsselauthentifizierung ist die einfachste Form der Authentifizierung. Verwenden Sie das Format: `aeg-sas-key: <your key>`.

Beispielsweise übergeben Sie einen Schlüssel mit:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS-Token

SAS-Token für Event Grid schließen die Ressource, eine Ablaufzeit und eine Signatur ein. Das Format des SAS-Tokens ist: `r={resource}&e={expiration}&s={signature}`.

Die Ressource ist der Pfad für das Event Grid-Thema, an das Sie Ereignisse senden. Beispielsweise lautet ein gültiger Ressourcenpfad: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01`. Alle unterstützten API-Versionen finden Sie unter [Microsoft.EventGrid-Ressourcentypen](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions). 

Sie generieren die Signatur aus einem Schlüssel.

Beispielsweise ist ein gültiger **aeg-sas-token**-Wert:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

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

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Alle Ereignisse oder Daten, die vom Event Grid-Dienst auf den Datenträger geschrieben werden, werden unter Verwendung eines von Microsoft verwalteten Schlüssels verschlüsselt, um die Verschlüsselung der ruhenden Daten zu gewährleisten. Ereignisse und Daten werden außerdem maximal 24 Stunden lang aufbewahrt (in Übereinstimmung mit der [Event Grid-Wiederholungsrichtlinie](delivery-and-retry.md)). Nach 24 Stunden oder nach Ablauf der Ereignislebensdauer (je nachdem, welcher Zeitraum kürzer ist) werden alle Ereignisse und Daten von Event Grid gelöscht.

## <a name="authenticate-event-delivery-to-webhook-endpoints"></a>Authentifizieren der Ereignisübermittlung an Webhook-Endpunkte
In den folgenden Abschnitten wird beschrieben, wie die Übermittlung von Ereignissen an Webhook-Endpunkte authentifiziert wird. Sie müssen unabhängig von der verwendeten Methode einen Mechanismus für einen Überprüfungshandshake verwenden. Weitere Informationen finden Sie unter [Webhook-Ereignisbereitstellung](webhook-event-delivery.md). 

### <a name="using-azure-active-directory-azure-ad"></a>Mit Azure Active Directory (Azure AD)
Sie können Ihren Webhook-Endpunkt mithilfe von Azure Active Directory (Azure AD) schützen, um Event Grid zum Übermitteln von Ereignissen an Ihre Endpunkte zu authentifizieren und zu autorisieren. Sie müssen eine Azure AD-Anwendung erstellen, eine Rolle und einen Dienstprinzipal zur Autorisierung von Event Grid in Ihrer Anwendung erstellen und das Ereignisabonnement zur Verwendung der Azure AD-Anwendung konfigurieren. [Erfahren Sie, wie Sie Azure Active Directory mit Event Grid konfigurieren](secure-webhook-delivery.md).

### <a name="using-client-secret-as-a-query-parameter"></a>Mit dem geheimen Clientschlüssel als Abfrageparameter
Sie können Ihren Webhookendpunkt sichern, indem Sie der Webhook-URL beim Erstellen eines Ereignisabonnements Abfrageparameter hinzufügen. Legen Sie einen dieser Abfrageparameter z. B. ein [Zugriffstoken](https://en.wikipedia.org/wiki/Access_token) oder einen gemeinsamen geheimen Schlüssel als geheimen Clientschlüssel fest. Der Webhook kann das Geheimnis verwenden, um zu erkennen, dass das Ereignis von Event Grid stammt und gültige Berechtigungen aufweist. Event Grid nimmt diese Abfrageparameter in jede Ereignisbereitstellung an den Webhook auf. Wenn der geheime Clientschlüssel aktualisiert wird, muss das Ereignisabonnement ebenfalls aktualisiert werden. Damit bei dieser Geheimnisrotation keine Übermittlungsfehler auftreten, lassen Sie den Webhook für einen begrenzten Zeitraum sowohl alte als auch neue Geheimnisse akzeptieren. 

Da Abfrageparameter geheime Clientschlüssel enthalten können, werden Sie mit besonderer Sorgfalt behandelt. Sie werden verschlüsselt gespeichert und sind für Dienstoperatoren nicht zugänglich. Sie werden nicht im Rahmen der Dienstprotokollierung/Ablaufverfolgung protokolliert. Wenn Sie das Ereignisabonnement bearbeiten, werden die Abfrageparameter nur angezeigt und zurückgegeben, wenn der Parameter [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) in Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) verwendet wird.

Weitere Informationen zum Übermitteln von Ereignissen an Webhooks finden Sie unter [Webhook-Ereignisbereitstellung](webhook-event-delivery.md).

> [!IMPORTANT]
Azure Event Grid unterstützt nur **HTTPS**-Webhook-Endpunkte. 

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
