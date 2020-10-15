---
title: Azure Event Grid – Problembehandlung bei der Abonnementüberprüfung
description: In diesem Artikel erfahren Sie, wie Sie Probleme bei Abonnementüberprüfungen beheben können.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 48844859013507ab684ef8879b7b85dd6b6fe8cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86118986"
---
# <a name="troubleshoot-azure-event-grid-subscription-validations"></a>Problembehandlung bei der Abonnementüberprüfung für Azure Event Grid
In diesem Artikel erfahren Sie, wie Sie Probleme bei der Überprüfung von Ereignisabonnements beheben. 

> [!IMPORTANT]
> Ausführliche Informationen zur Endpunktüberprüfung für Webhooks finden Sie unter [Webhook-Ereignisbereitstellung](webhook-event-delivery.md).

## <a name="validate-event-grid-event-subscription-using-postman"></a>Überprüfen eines Event Grid-Ereignisabonnements mithilfe von Postman
Im folgenden Beispiel erfahren Sie, wie Sie ein Webhookabonnement eines Event Grid-Ereignisses mit Postman überprüfen: 

![Überprüfen eines Event Grid-Ereignisabonnements mithilfe von Postman](./media/troubleshoot-subscription-validation/event-subscription-validation-postman.png)

Hier ein Beispiel für die Verwendung von **SubscriptionValidationEvent** in JSON:

```json
[
  {
    "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
    "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "subject": "",
    "data": {
      "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    },
    "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
    "eventTime": "2018-01-25T22:12:19.4556811Z",
    "metadataVersion": "1",
    "dataVersion": "1"
  }
]
```

Nachstehend die erfolgreiche Antwort auf den Beispielcode:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Weitere Informationen zur Event Grid-Ereignisüberprüfung für Webhooks finden Sie unter [Endpunktüberprüfung mit Event Grid-Ereignissen](webhook-event-delivery.md#endpoint-validation-with-event-grid-events).


## <a name="validate-event-grid-event-subscription-using-curl"></a>Überprüfen eines Event Grid-Ereignisabonnements mithilfe von Curl 
Mit dem Curl-Befehl im folgenden Beispiel können Sie ein Webhookabonnement eines Event Grid-Ereignisses überprüfen: 

```bash
curl -X POST -d '[{"id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66","topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","subject": "","data": {"validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"},"eventType": "Microsoft.EventGrid.SubscriptionValidationEvent","eventTime": "2018-01-25T22:12:19.4556811Z", "metadataVersion": "1","dataVersion": "1"}]' -H 'Content-Type: application/json' https://{your-webhook-url.com}
```

## <a name="validate-cloud-event-subscription-using-postman"></a>Überprüfen eines Cloudereignisabonnements mithilfe von Postman
Im folgenden Beispiel erfahren Sie, wie Sie ein Webhookabonnement eines Cloudereignisses mit Postman überprüfen: 

![Überprüfen eines Cloudereignisabonnements mithilfe von Postman](./media/troubleshoot-subscription-validation/cloud-event-subscription-validation-postman.png)

Verwenden Sie die **HTTP OPTIONS**-Methode für die Überprüfung mit Cloudereignissen. Weitere Informationen zur Cloudereignisüberprüfung für Webhooks finden Sie unter [Endpunktüberprüfung mit Cloudereignissen](webhook-event-delivery.md#endpoint-validation-with-event-grid-events).

## <a name="error-code-403"></a>Fehlercode: 403
Wenn Ihr Webhook „403 (Forbidden)“ in der Antwort zurückgibt, überprüfen Sie, ob sich Ihr Webhook hinter einem Azure Application Gateway oder einer Web Application Firewall befindet. Wenn dies der Fall ist, müssen Sie die folgenden Firewallregeln deaktivieren und erneut einen HTTP POST-Aufruf ausführen:

  - 920300 (Fehlender Accept-Header in Anforderung; das können wir beheben)
  - 942430 (Eingeschränkte Anomalieerkennung für SQL-Zeichen (Argumente): Anzahl von Sonderzeichen überschritten (12))
  - 920230 (Mehrere URL-Codierungen erkannt)
  - 942130 (Angriff mit Einschleusung von SQL-Befehlen: SQL-Tautologie erkannt.)
  - 931130 (Möglicher RFI-Angriff (Remote File Inclusion) = Domänenexterner Verweis/Link)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihr Problem im [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-eventgrid), oder öffnen Sie ein [Supportticket](https://azure.microsoft.com/support/options/). 
