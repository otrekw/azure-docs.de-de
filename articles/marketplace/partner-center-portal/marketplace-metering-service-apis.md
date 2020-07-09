---
title: 'Messungsdienst-APIs: Kommerzieller Microsoft-Marketplace'
description: Mit der API für Nutzungsereignisse können Sie Nutzungsereignisse für SaaS-Angebote in Microsoft AppSource und Azure Marketplace ausgeben.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 95eba648219413923ce27d433a5236877c4953f3
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725464"
---
# <a name="marketplace-metering-service-apis"></a>Marketplace-Messungsdienst-APIs

Mit der API für Nutzungsereignisse können Sie Nutzungsereignisse für eine bestimmte erworbene Entität ausgeben. In der Anforderung des Nutzungsereignisses wird auf die Messungsdienstdimension verwiesen, die vom Herausgeber beim Veröffentlichen des Angebots definiert wurde.

## <a name="usage-event"></a>Nutzungsereignis

**POST**: `https://marketplaceapi.microsoft.com/api/usageEvent?api-version=<ApiVersion>`

*Abfrageparameter:*

|            |          |
| ---------- | ---------------------- |
| `ApiVersion` | Die Version des für diese Anforderung zu verwendenden Vorgangs. Die neueste API-Version ist 2018-08-31. |

*Anforderungsheader:*

| Inhaltstyp       | `application/json`    |
| ------------------ | ---------------------------- |
| `x-ms-requestid`     | Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `x-ms-correlationid` | Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `authorization`   | [Abrufen eines Tokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) Hinweis: Stellen Sie beim Senden der HTTP-Anforderung dem Token, das Sie über den angegebenen Link erhalten haben, das Präfix `Bearer` voran. |

>[!Note]
>Bei Plänen für verwaltete Azure-Apps ist `resourceId` das `resourceUsageId`-Element unter dem `billingDetails`-Element des Metadatenobjekts für verwaltete Apps.  Ein Beispielskript zum Abrufen der ID finden Sie unter [Verwenden des Tokens für von Azure verwaltete Identitäten](./marketplace-metering-service-authentication.md#using-the-azure-managed-identities-token).  Bei SaaS-Angeboten ist `resourceId` die SaaS-Abonnement-ID.  Weitere Informationen zu SaaS-Abonnements finden Sie unter [Auflisten von Abonnements](./pc-saas-fulfillment-api-v2.md#list-subscriptions).

*Anforderung:*

```json
{
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

### <a name="responses"></a>Antworten

Code: 200<br>
OK 

```json
{
  "usageEventId": "Unique identifier associated with the usage event",
  "status": "Accepted",
  "messageTime": "Time this message was created in UTC",
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

Code: 400 <br>
Ungültige Anforderung, fehlende oder ungültige Daten, Daten abgelaufen

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "The resourceId is required.",
      "target": "ResourceId",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```

Code: 403<br>
Ungültige Anforderung, fehlende oder ungültige Daten, Daten abgelaufen

```json
{
  "code": "Forbidden",
  "message": "User is not allowed authorized to call this"
}
```

Code: 409<br>
Konflikt, wenn wir den Nutzungsaufruf für die Nutzungsressourcen-ID erhalten, und effektive Nutzung, die bereits vorhanden ist. Die Antwort enthält das Feld `additionalInfo` mit Informationen zur akzeptierten Nachricht.

```json
{
  "code": "Conflict",
  "additionalInfo": {
    "usageEventId": "Unique identifier associated with the usage event",
    "status": "Accepted|NotProcessed|Expired",
    "messageTime": "Time this message was created in UTC",
    "resourceId": "Identifier of the resource against which usage is emitted",
    "quantity": 5.0,
    "dimension": "Dimension identifier",
    "effectiveStartTime": "Time in UTC when the usage event occurred",
    "planId": "Plan associated with the purchased offer"
  }
}
```

## <a name="batch-usage-event"></a>Batchnutzungsereignis

Mit der API für Batchnutzungsereignisse können Sie Nutzungsereignisse gleichzeitig für mehr als eine gekaufte Entität ausgeben. In der Anforderung des Batchnutzungsereignisses wird auf die Messungsdienstdimension verwiesen, die vom Herausgeber beim Veröffentlichen des Angebots definiert wurde.

>[!Note]
>Sie können auf dem kommerziellen Marketplace von Microsoft mehrere SaaS-Angebote registrieren. Jedes registrierte SaaS-Angebot verfügt über eine eindeutige Azure AD-Anwendung, die für Authentifizierungs- und Autorisierungszwecke registriert ist. Die im Batch ausgegebenen Ereignisse sollten zu Angeboten gehören, für die bei der Registrierung die gleiche Azure AD-Anwendung verwendet wurde.

**POST:** `https://marketplaceapi.microsoft.com/api/batchUsageEvent?api-version=<ApiVersion>`

*Abfrageparameter:*

|            |     |
| ---------- | -------------------- |
| `ApiVersion` | Die Version des für diese Anforderung zu verwendenden Vorgangs. Die neueste API-Version ist 2018-08-31. |

*Anforderungsheader:*

| Inhaltstyp       | `application/json`       |
| ------------------ | ------ |
| `x-ms-requestid`     | Eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `x-ms-correlationid` | Eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `authorization`      | [Abrufen eines Tokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) Hinweis: Stellen Sie beim Senden der HTTP-Anforderung dem Token, das Sie über den angegebenen Link erhalten haben, das Präfix `Bearer` voran.  |

*Anforderung:*
```json
{
  "request": [
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    },
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    }
  ]
}
```
### <a name="responses"></a>Antworten

Code: 200<br>
OK

```json
{
  "count": 2,
  "result": [
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    },
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    }
  ]
}
```

Beschreibung des Statuscodes, der in der `BatchUsageEvent`-API-Antwort angegeben ist:

| Statuscode  | BESCHREIBUNG |
| ---------- | -------------------- |
| `Accepted` | Akzeptierter Code. |
| `Expired` | Abgelaufene Nutzung. |
| `Duplicate` | Doppelte Nutzung angegeben. |
| `Error` | Fehlercode |
| `ResourceNotFound` | Die angegebene Nutzungsressource ist ungültig. |
| `ResourceNotAuthorized` | Sie sind nicht autorisiert, die Nutzung für diese Ressource anzugeben. |
| `InvalidDimension` | Die Dimension, für die die Nutzung übergeben wird, ist für dieses Angebot bzw. diesen Plan ungültig. |
| `InvalidQuantity` | Die übergebene Menge ist kleiner als 0. |
| `BadArgument` | Die Eingabe fehlt oder ist falsch formatiert. |

Code: 400<br>
Ungültige Anforderung, fehlende oder ungültige Daten, Daten abgelaufen

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "Invalid data format.",
      "target": "usageEventRequest",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```
Code: 403<br>
Benutzer ist zum Ausführen dieses Aufrufs nicht autorisiert

```json
{
  "code": "Forbidden",
  "message": "User is not allowed to call this"
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Getaktete SaaS-Abrechnung](./saas-metered-billing.md).
