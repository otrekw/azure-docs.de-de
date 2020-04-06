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
ms.openlocfilehash: 0b7c5b42ac6291c6687337ba8d6a9d35830b9bda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79236250"
---
# <a name="authenticating-access-to-event-grid-resources"></a>Authentifizieren des Zugriffs auf Event Grid-Ressourcen

Azure Event Grid verfügt über drei Authentifizierungsarten:

* Webhook-Ereignisbereitstellung
* Ereignisabonnements
* Veröffentlichung benutzerdefinierter Themen

## <a name="webhook-event-delivery"></a>Webhook-Ereignisbereitstellung

Ein Webhook ist eine der vielen Möglichkeiten, um Ereignisse aus Azure Event Grid zu empfangen. Wenn ein neues Ereignis bereit ist, sendet der Event Grid-Dienst per POST-Vorgang eine HTTP-Anforderung an den konfigurierten Endpunkt, wobei das Ereignis im Anforderungstext enthalten ist.

Wie viele andere Dienste, die Webhooks unterstützen, müssen Sie bei Event Grid nachweisen, das Sie im Besitz Ihres Webhookendpunkts sind. Vorher wird mit dem Bereitstellen von Ereignissen für diesen Endpunkt nicht begonnen. Diese Anforderung verhindert, dass ein böswilliger Benutzer Ihren Endpunkt mit Ereignissen überschwemmt. Wenn Sie einen der drei unten angegebenen Azure-Dienste verwenden, wird diese Überprüfung automatisch von der Azure-Infrastruktur durchgeführt:

* Azure Logic Apps mit [Event Grid-Connector](https://docs.microsoft.com/connectors/azureeventgrid/)
* Azure Automation über [Webhook](../event-grid/ensure-tags-exists-on-new-virtual-machines.md)
* Azure Functions mit [Event Grid-Trigger](../azure-functions/functions-bindings-event-grid.md)

Falls Sie einen anderen Typ von Endpunkt nutzen, z. B. eine auf einem HTTP-Trigger basierende Azure-Funktion, muss Ihr Endpunktcode an einem Überprüfungshandshake mit Event Grid beteiligt sein. Event Grid unterstützt zwei Methoden zur Überprüfung des Abonnements.

1. **ValidationCode-Handshake (programmgesteuert)** : Wenn Sie den Quellcode für Ihren Endpunkt kontrollieren, wird diese Methode empfohlen. Zum Zeitpunkt der Erstellung des Ereignisabonnements sendet Event Grid ein Ereignis zur Überprüfung des Abonnements an Ihren Endpunkt. Das Schema dieses Ereignisses ähnelt dem aller anderen Event Grid-Ereignisse. Der Datenteil dieses Ereignisses umfasst eine `validationCode`-Eigenschaft. Ihre Anwendung überprüft, ob es sich bei der Überprüfungsanforderung um ein erwartetes Ereignisabonnement handelt, und gibt den Überprüfungscode an Event Grid zurück. Dieser Handshakemechanismus wird in allen Event Grid-Versionen unterstützt.

2. **ValidationURL-Handshake (manuell)** : In bestimmten Fällen können Sie nicht auf den Quellcode des Endpunkts zugreifen, um den ValidationCode-Handshake zu implementieren. Wenn Sie beispielsweise einen Drittanbieterdienst nutzen (z. B. [Zapier](https://zapier.com) oder [IFTTT](https://ifttt.com/)), können Sie unter Umständen nicht programmgesteuert mit dem Überprüfungscode antworten.

   Ab Version 2018-05-01-preview unterstützt Event Grid einen manuellen Überprüfungshandshake. Wenn Sie ein Ereignisabonnement mit einem SDK oder Tool erstellen, für die diese neue API-Version (2018-05-01-preview oder höher) verwendet wird, sendet Event Grid im Datenteil des Abonnementüberprüfungsereignisses eine `validationUrl`-Eigenschaft. Um den Handshake abzuschließen, suchen Sie diese URL in den Ereignisdaten und senden Sie ihr manuell eine GET-Anforderung. Sie können entweder einen REST-Client oder Ihren Webbrowser verwenden.

   Die angegebene URL ist fünf Minuten lang gültig. Während dieser Zeit lautet der Bereitstellungsstatus des Ereignisabonnements `AwaitingManualAction`. Wenn Sie die manuelle Überprüfung nicht innerhalb von 5 Minuten abschließen, wird der Bereitstellungsstatus auf `Failed` eingestellt. Sie müssen das Ereignisabonnement erneut erstellen, bevor Sie mit der manuellen Überprüfung beginnen.

    Dieser Authentifizierungsmechanismus erfordert auch, dass der Webhookendpunkt einen HTTP-Statuscode von 200 zurückgibt, um sicherzustellen, dass das POST für das Überprüfungsereignis akzeptiert wurde, bevor er in den manuellen Überprüfungsmodus gewechselt wird. Mit anderen Worten: Wenn der Endpunkt 200 zurückgibt, aber programmgesteuert keine Überprüfungsantwort zurückgibt, wird der Modus in den manuellen Überprüfungsmodus überführt. Wenn innerhalb von fünf Minuten ein GET-Vorgang auf die Überprüfungs-URL folgt, gilt der Überprüfungshandshake als erfolgreich.

> [!NOTE]
> Die Verwendung von selbstsignierten Zertifikaten wird nicht unterstützt. Verwenden Sie stattdessen ein Zertifikat von einer Zertifizierungsstelle.

### <a name="validation-details"></a>Überprüfungsdetails

* Zum Zeitpunkt der Erstellung/Aktualisierung des Ereignisabonnements sendet Event Grid ein Abonnementüberprüfungsereignis an den Zielendpunkt. 
* Das Ereignis enthält den Headerwert „aeg-event-type: SubscriptionValidation“.
* Der Hauptteil des Ereignisses weist dasselbe Schema wie andere Event Grid-Ereignisse auf.
* Die eventType-Eigenschaft des Ereignisses lautet `Microsoft.EventGrid.SubscriptionValidationEvent`.
* Die Dateneigenschaft des Ereignisses enthält eine `validationCode`-Eigenschaft mit einer zufällig generierten Zeichenfolge. Beispiel: „validationCode: acb13…“.
* Die Ereignisdaten enthalten auch die `validationUrl`-Eigenschaft mit einer URL für die manuelle Überprüfung des Abonnements.
* Das Array enthält ausschließlich das Validierungsereignis. Andere Ereignisse werden in einer separaten Anforderung gesendet, nachdem Sie den Validierungscode zurückgegeben haben.
* Die EventGrid-DataPlane-SDKs verfügen über Klassen, die den Daten des Abonnementüberprüfungsereignisses und der Abonnementüberprüfungsantwort entsprechen.

Ein Beispiel für „SubscriptionValidationEvent“ finden Sie im folgenden Beispiel:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=512d38b6-c7b8-40c8-89fe-f46f9e9622b6&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1A1A1A1A"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Senden Sie wie im folgenden Beispiel gezeigt den Validierungscode zurück an die Eigenschaft „validationResponse“, um den Besitz des Endpunkts nachzuweisen:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Sie müssen den Antwortstatuscode „HTTP 200 OK“ zurückgeben. „HTTP 202 Akzeptiert“ wird nicht als gültige Antwort zur Überprüfung des Event Grid-Abonnements erkannt. Die HTTP-Anforderung muss innerhalb von 30 Sekunden abgeschlossen werden. Wenn der Vorgang nicht innerhalb von 30 Sekunden abgeschlossen wird, wird er abgebrochen und kann nach 5 Sekunden wiederholt werden. Wenn alle Versuche zu Fehlern führen, wird dies als Handshake-Validierungsfehler gewertet.

Alternativ können Sie das Abonnement manuell überprüfen, indem Sie eine GET-Anforderung an die Überprüfungs-URL senden. Das Ereignisabonnement bleibt im Status „Ausstehend“, bis es überprüft wurde. Die Überprüfungs-URL verwendet den Port 553. Wenn die Firewallregeln Port 553 blockieren, müssen die Regeln für einen erfolgreichen manuellen Handshake möglicherweise angepasst werden.

Ein Beispiel für die Handhabung des Handshakes zur Abonnementüberprüfung finden Sie in einem [C#-Beispiel](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs).

### <a name="checklist"></a>Checkliste

Wenn während der Erstellung des Ereignisabonnements eine Fehlermeldung der Art „Fehler beim Versuch, den angegebenen Endpunkt https:\//your-endpoint-here zu überprüfen. Ausführlichere Informationen finden Sie unter https:\//aka.ms/esvalidation.“ angezeigt wird, ist dies ein Hinweis darauf, dass für den Überprüfungshandshake ein Fehler aufgetreten ist. Überprüfen Sie Folgendes, um diesen Fehler zu beheben:

* Haben Sie die Kontrolle über den am Zielendpunkt ausgeführten Anwendungscode? Haben Sie beispielsweise beim Schreiben einer Azure Function, die auf einem HTTP-Trigger basiert, Zugriff auf den Anwendungscode, um Änderungen daran vorzunehmen?
* Wenn Sie Zugriff auf den Anwendungscode haben, sollten Sie den ValidationCode-basierten Handshakemechanismus wie im obigen Beispiel implementieren.

* Wenn Sie keinen Zugriff auf den Anwendungscode haben (z.B. bei Nutzung eines Drittanbieterdiensts, der Webhooks unterstützt), können Sie den manuellen Handshakemechanismus verwenden. Vergewissern Sie sich, dass Sie die API-Version „2018-05-01-preview“ oder eine spätere Version verwenden (installieren Sie die eventgrid-Erweiterung für die Azure-Befehlszeilenschnittstelle), um den validationUrl-Wert im Überprüfungsereignis zu erhalten. Rufen Sie den Wert der `validationUrl`-Eigenschaft ab, und navigieren Sie im Webbrowser zu dieser URL, um den Handshake für die manuelle Validierung durchzuführen. Wenn die Überprüfung erfolgreich durchgeführt wurde, sollte im Webbrowser eine Meldung mit einem entsprechenden Hinweis angezeigt werden. Sie sehen dann, dass „provisioningState“ für das Ereignisabonnement auf „Succeeded“ (Erfolgreich) festgelegt ist. 

### <a name="event-delivery-security"></a>Sicherheit für die Ereignisbereitstellung

#### <a name="azure-ad"></a>Azure AD

Sie können Ihren Webhook-Endpunkt mithilfe von Azure Active Directory sichern, um Event Grid zum Veröffentlichen von Ereignissen an Ihre Endpunkte zu authentifizieren und autorisieren. Sie müssen eine Azure Active Directory-Anwendung erstellen, eine Rolle und einen Dienstprinzipal zur Autorisierung von Event Grid in Ihrer Anwendung erstellen und das Ereignisabonnement zur Verwendung der Azure AD-Anwendung konfigurieren. [Erfahren Sie, wie Sie Azure Active Directory mit Event Grid konfigurieren](secure-webhook-delivery.md).

#### <a name="query-parameters"></a>Abfrageparameter
Sie können Ihren Webhookendpunkt sichern, indem Sie der Webhook-URL beim Erstellen eines Ereignisabonnements Abfrageparameter hinzufügen. Legen Sie einen dieser Abfrageparameter als Geheimnis fest, z. B. als ein [Zugriffstoken](https://en.wikipedia.org/wiki/Access_token). Der Webhook kann das Geheimnis verwenden, um zu erkennen, dass das Ereignis von Event Grid stammt und gültige Berechtigungen aufweist. Event Grid nimmt diese Abfrageparameter in jede Ereignisbereitstellung an den Webhook auf.

Wenn Sie das Ereignisabonnement bearbeiten, werden die Abfrageparameter nur angezeigt und zurückgegeben, wenn der Parameter [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) in Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) verwendet wird.

Abschließend ist es wichtig zu beachten, dass Azure Event Grid nur HTTPS-Webhook-Endpunkte unterstützt.

## <a name="event-subscription"></a>Ereignisabonnement

Um ein Ereignis abonnieren zu können, müssen Sie nachweisen, dass Sie Zugriff auf die Ereignisquelle und den Ereignishandler haben. Wie Sie nachweisen, dass Sie einen Webhook besitzen, wurde im vorherigen Abschnitt beschrieben. Wenn Sie einen Ereignishandler verwenden, der kein Webhook ist (z.B. einen Event Hub oder Warteschlangenspeicher), benötigen Sie Schreibzugriff auf diese Ressource. Durch diese Berechtigungsüberprüfung wird verhindert, dass ein nicht autorisierter Benutzer Ereignisse an Ihre Ressource sendet.

Sie benötigen die Berechtigung **Microsoft.EventGrid/EventSubscriptions/Write** für die Ressource, die als Ereignisquelle verwendet wird. Sie benötigen diese Berechtigung, da Sie ein neues Abonnement im Gültigkeitsbereich der Ressource schreiben. Die angeforderte Ressource variiert abhängig davon, ob Sie ein Systemthema oder ein benutzerdefiniertes Thema abonnieren. In diesem Abschnitt werden beide Typen beschrieben.

### <a name="system-topics-azure-service-publishers"></a>Systemthemen (Azure-Dienstherausgeber)

Für Systemthemen benötigen Sie die Berechtigung zum Schreiben eines neuen Abonnements im Gültigkeitsbereich der Ressource, die das Ereignis veröffentlicht. Das Format der Ressource ist: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`.

Beispiel: Um ein Ereignis im Speicherkonto **myacct** zu abonnieren, benötigen Sie die Berechtigung Microsoft.EventGrid/EventSubscriptions/Write für: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`.

### <a name="custom-topics"></a>Benutzerdefinierte Themen

Für benutzerdefinierte Themen benötigen Sie die Berechtigung zum Schreiben eines neuen Ereignisabonnements im Gültigkeitsbereich des Event Grid-Themas. Das Format der Ressource ist: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`.

Beispiel: Um das benutzerdefinierte Thema **mytopic** zu abonnieren, benötigen Sie die Berechtigung Microsoft.EventGrid/EventSubscriptions/Write für: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`.

## <a name="custom-topic-publishing"></a>Veröffentlichung benutzerdefinierter Themen

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

Die Ressource ist der Pfad für das Event Grid-Thema, an das Sie Ereignisse senden. Beispielsweise lautet ein gültiger Ressourcenpfad: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`.

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

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
