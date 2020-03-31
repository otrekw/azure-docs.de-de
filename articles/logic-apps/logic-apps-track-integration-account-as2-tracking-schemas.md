---
title: AS2-Nachverfolgungsschemas für B2B-Nachrichten
description: Erstellen von Nachverfolgungsschemas zum Überwachen von AS2-Nachrichten in Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: bccf69362279afd9e8148b20b61ff3ea9b472a03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906959"
---
# <a name="create-schemas-for-tracking-as2-messages-in-azure-logic-apps"></a>Erstellen von Schemas zur Nachverfolgung von AS2-Nachrichten in Azure Logic Apps

Zur Unterstützung bei der Überwachung des Erfolgs, der Fehler und der Nachrichteneigenschaften für B2B-Transaktionen (Business-to-Buiness) können Sie in Ihrem Integrationskonto folgende AS2-Nachverfolgungsschemas verwenden:

* AS2-Nachrichten-Nachverfolgungsschema
* Nachverfolgungsschema für AS2 Message Disposition Notification (MDN)

## <a name="as2-message-tracking-schema"></a>AS2-Nachrichten-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "dispositionType": "",
      "fileName": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isMessageEncrypted": "",
      "isMessageCompressed": "",
      "correlationMessageId": "",
      "incomingHeaders": {},
      "outgoingHeaders": {},
      "isNrrEnabled": "",
      "isMdnExpected": "",
      "mdnType": ""
    }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Partners des AS2-Nachrichtenabsenders |
| receiverPartnerName | Nein | String | Name des Partners des AS2-Nachrichtenempfängers |
| as2To | Ja | String | Name des Empfängers der AS2-Nachricht aus den Headern der AS2-Nachricht |
| as2From | Ja | String | Name des Absenders der AS2-Nachricht aus den Headern der AS2-Nachricht |
| agreementName | Nein | String | Name der AS2-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | String | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| messageId | Nein | String | AS2-Nachrichten-ID aus den Headern der AS2-Nachricht |
| dispositionType | Nein | String | MDN-Dispositionstypwert (Message Disposition Notification) |
| fileName | Nein | String | Dateiname aus dem Header der AS2-Nachricht |
| isMessageFailed | Ja | Boolean | Gibt an, ob die AS2-Nachricht fehlgeschlagen ist. |
| isMessageSigned | Ja | Boolean | Gibt an, ob die AS2-Nachricht signiert wurde. |
| isMessageEncrypted | Ja | Boolean | Gibt an, ob die AS2-Nachricht verschlüsselt wurde. |
| isMessageCompressed | Ja | Boolean | Gibt an, ob die AS2-Nachricht komprimiert wurde. |
| correlationMessageId | Nein | String | ID der AS2-Nachricht zum Korrelieren von Nachrichten mit MDNs |
| incomingHeaders | Nein | Wörterbuch von JToken | Headerdetails von eingehenden AS2-Nachrichten |
| outgoingHeaders | Nein | Wörterbuch von JToken | Headerdetails von ausgehenden AS2-Nachrichten |
| isNrrEnabled | Ja | Boolean | Gibt an, ob der Standardwert verwendet werden soll, wenn der Wert unbekannt ist. |
| isMdnExpected | Ja | Boolean | Gibt an, ob der Standardwert verwendet werden soll, wenn der Wert unbekannt ist. |
| mdnType | Ja | Enum | Zulässige Werte: `NotConfigured`, `Sync` und `Async` |
|||||

## <a name="as2-mdn-tracking-schema"></a>AS2-MDN-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "originalMessageId": "",
      "dispositionType": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isNrrEnabled": "",
      "statusCode": "",
      "micVerificationStatus": "",
      "correlationMessageId": "",
      "incomingHeaders": {
      },
      "outgoingHeaders": {
      }
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Partners des AS2-Nachrichtenabsenders |
| receiverPartnerName | Nein | String | Name des Partners des AS2-Nachrichtenempfängers |
| as2To | Ja | String | Name des Partners, der die AS2-Nachricht erhält |
| as2From | Ja | String | Name des Partners, der die AS2-Nachricht sendet |
| agreementName | Nein | String | Name der AS2-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | String | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| messageId | Nein | String | ID der AS2-Nachricht |
| originalMessageId | Nein | String | ID der ursprünglichen AS2-Nachricht |
| dispositionType | Nein | String | MDN-Dispositionstypwert |
| isMessageFailed | Ja | Boolean | Gibt an, ob die AS2-Nachricht fehlgeschlagen ist. |
| isMessageSigned | Ja | Boolean | Gibt an, ob die AS2-Nachricht signiert wurde. |
| isNrrEnabled | Ja | Boolean | Gibt an, ob der Standardwert verwendet werden soll, wenn der Wert unbekannt ist. |
| statusCode | Ja | Enum | Zulässige Werte: `Accepted`, `Rejected` und `AcceptedWithErrors` |
| micVerificationStatus | Ja | Enum | Zulässige Werte: `NotApplicable`, `Succeeded` und `Failed` |
| correlationMessageId | Nein | String | Korrelations-ID, bei der es sich um die ID der ursprünglichen Nachricht handelt, für die MDN konfiguriert ist |
| incomingHeaders | Nein | Wörterbuch von JToken | Headerdetails von eingehenden Nachrichten |
| outgoingHeaders | Nein | Wörterbuch von JToken | Headerdetails von ausgehenden Nachrichten |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B-Protokollnachverfolgungsschemas

Informationen zu B2B-Protokollnachverfolgungsschemas finden Sie unter:

* [X12-Nachverfolgungsschemas](logic-apps-track-integration-account-x12-tracking-schema.md)
* [Benutzerdefinierte B2B-Nachverfolgungsschemas](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von B2B-Nachrichten mit Azure Monitor-Protokollen](../logic-apps/monitor-b2b-messages-log-analytics.md)