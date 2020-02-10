---
title: Benutzerdefinierte Nachverfolgungsschemas für B2B-Nachrichten
description: Erstellen von benutzerdefinierten Nachverfolgungsschemas zum Überwachen von B2B-Nachrichten in Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: c82f9cbfaf2e23ddaa5e4b05f4aac4795d3e16a9
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76903051"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-a"></a>Erstellen benutzerdefinierter Nachverfolgungsschemas zur Überwachung von End-to-End-Workflows in Azure Logic Apps

Azure Logic Apps verfügt über eine integrierte Nachverfolgung, die Sie für Teile Ihres Workflows aktivieren können. Sie können jedoch eine benutzerdefinierte Nachverfolgung einrichten, bei der Ereignisse vom Anfang bis zum Ende des Workflows protokolliert werden, z. B. Workflows, die eine Logik-App, BizTalk Server, SQL Server oder eine andere Ebene enthalten. Dieser Artikel enthält benutzerdefinierten Code, den Sie in den Ebenen außerhalb Ihrer Logik-App verwenden können.

## <a name="custom-tracking-schema"></a>Benutzerdefiniertes Nachverfolgungsschema

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": ,
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {}
      }
   ]
}
```

| Eigenschaft | Erforderlich | type | Beschreibung |
|----------|----------|------|-------------|
| sourceType | Ja | String | Der Typ der Ausführungsquelle mit den folgenden zulässigen Werten: `Microsoft.Logic/workflows`, `custom` |
| source | Ja | Zeichenfolge oder JToken | Wenn der Quelltyp `Microsoft.Logic/workflows` ist, müssen die Quellinformationen diesem Schema folgen. Wenn der Quelltyp `custom` ist, ist das Schema ist ein JToken. |
| systemId | Ja | String | System-ID der Logik-App |
| runId | Ja | String | Ausführungs-ID der Logik-App |
| operationName | Ja | String | Name des Vorgangs, z. B. Aktion oder Trigger |
| repeatItemScopeName | Ja | String | Elementnamen wiederholen, wenn sich die Aktion innerhalb einer `foreach`- oder `until`-Schleife befindet. |
| repeatItemIndex | Ja | Integer | Zeigt an, dass sich die Aktion in einer `foreach`- oder `until`-Schleife befindet, und ist die Indexzahl des wiederholten Elements. |
| trackingId | Nein | String | Überwachungs-ID zum Korrelieren der Nachrichten |
| correlationId | Nein | String | Korrelations-ID zum Korrelieren der Nachrichten |
| clientRequestId | Nein | String | Der Client kann diese Eigenschaft auffüllen, um Nachrichten zu korrelieren |
| eventLevel | Ja | String | Ebene des Ereignisses |
| eventTime | Ja | Datetime | Zeitpunkt des Ereignisses im UTC-Format: *YYYY-MM-DDTHH:MM:SS.00000Z* |
| recordType | Ja | String | Der Typ des Nachverfolgungseintrags mit nur diesem zulässigen Wert: `custom` |
| Datensatz (record) | Ja | JToken | Benutzerdefinierter Eintragstyp nur im JToken-Format |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B-Protokollnachverfolgungsschemas

Informationen zu B2B-Protokollnachverfolgungsschemas finden Sie unter:

* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschemas](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum [Überwachen von B2B-Nachrichten mit Azure Monitor-Protokollen](../logic-apps/monitor-b2b-messages-log-analytics.md)