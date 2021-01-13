---
title: X12-Nachverfolgungsschemas für B2B-Nachrichten
description: Erstellen von Nachverfolgungsschemas zum Überwachen von X12-Nachrichten für Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: 5b2df194761ebc167e67498a985960a4fce35f19
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76905305"
---
# <a name="create-schemas-for-tracking-x12-messages-in-azure-logic-apps"></a>Erstellen von Schemas zur Nachverfolgung von X12-Nachrichten in Azure Logic Apps

Zur Unterstützung bei der Überwachung des Erfolgs, der Fehler und der Nachrichteneigenschaften für B2B-Transaktionen (Business-to-Buiness) können Sie in Ihrem Integrationskonto folgende X12-Nachverfolgungsschemas verwenden:

* X12-Transaktionssatz-Nachverfolgungsschema
* X12-Transaktionssatzbestätigung-Nachverfolgungsschema
* X12-Austausch-Nachverfolgungsschema
* X12-Austauschbestätigung-Nachverfolgungsschema
* X12-Funktionsgruppen-Nachverfolgungsschema
* X12-Funktionsgruppenbestätigung-Nachverfolgungsschema

## <a name="x12-transaction-set-tracking-schema"></a>X12-Transaktionssatz-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "transactionSetControlNumber": "",
      "CorrelationMessageId": "",
      "messageType": "",
      "isMessageFailed": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "needAk2LoopForValidMessages": "",
      "segmentsCount": ""
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Absenderpartners der X12-Nachricht |
| receiverPartnerName | Nein | String | Name des Empfangspartners der X12-Nachricht |
| senderQualifier | Ja | String | Qualifizierer des Absenderpartners |
| senderIdentifier | Ja | String | Bezeichner des Absenderpartners |
| receiverQualifier | Ja | String | Qualifizierer des Empfangspartners |
| receiverIdentifier | Ja | String | Bezeichner des Empfangspartners |
| agreementName | Nein | String | Name der X12-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | Enum | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| interchangeControlNumber | Nein | String | Austauschkontrollnummer |
| functionalGroupControlNumber | Nein | String | Funktionskontrollnummer |
| transactionSetControlNumber | Nein | String | Transaktionssatz-Kontrollnummer |
| CorrelationMessageId | Nein | String | Korrelationsnachrichten-ID, die eine Kombination aus den Elementen {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber} ist. |
| messageType | Nein | String | Transaktionssatz oder Dokumenttyp |
| isMessageFailed | Ja | Boolean | Gibt an, ob die X12-Nachricht fehlgeschlagen ist. |
| isTechnicalAcknowledgmentExpected | Ja | Boolean | Gibt an, ob die technische Bestätigung in der X12-Vereinbarung konfiguriert ist. |
| isFunctionalAcknowledgmentExpected | Ja | Boolean | Gibt an, ob die Funktionsbestätigung in der X12-Vereinbarung konfiguriert ist. |
| needAk2LoopForValidMessages | Ja | Boolean | Gibt an, ob die AK2-Schleife für eine gültige Nachricht erforderlich ist. |
| segmentsCount | Nein | Integer | Anzahl von Segmenten im X12-Transaktionssatz. |
|||||

## <a name="x12-transaction-set-acknowledgment-tracking-schema"></a>X12-Transaktionssatzbestätigung-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "respondingtransactionSetControlNumber": "",
      "respondingTransactionSetId": "",
      "statusCode": "",
      "processingStatus": "",
      "CorrelationMessageId": "",
      "isMessageFailed": "",
      "ak2Segment": "",
      "ak3Segment": "",
      "ak5Segment": ""
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Absenderpartners der X12-Nachricht |
| receiverPartnerName | Nein | String | Name des Empfangspartners der X12-Nachricht |
| senderQualifier | Ja | String | Qualifizierer des Absenderpartners |
| senderIdentifier | Ja | String | Bezeichner des Absenderpartners |
| receiverQualifier | Ja | String | Qualifizierer des Empfangspartners |
| receiverIdentifier | Ja | String | Bezeichner des Empfangspartners |
| agreementName | Nein | String | Name der X12-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | Enum | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| interchangeControlNumber | Nein | String | Austauschkontrollnummer der Funktionsbestätigung. Der Wert wird nur für die Absenderseite aufgefüllt, wenn für die an den Partner gesendeten Nachrichten eine Funktionsbestätigung empfangen wurde. |
| functionalGroupControlNumber | Nein | String | Funktionsgruppen-Kontrollnummer der Funktionsbestätigung. Der Wert wird nur für die Absenderseite aufgefüllt, wenn für die an den Partner gesendeten Nachrichten eine Funktionsbestätigung empfangen wurde. |
| isaSegment | Nein | String | ISA-Segment der Nachricht. Der Wert wird nur für die Absenderseite aufgefüllt, wenn für die an den Partner gesendeten Nachrichten eine Funktionsbestätigung empfangen wurde. |
| gsSegment | Nein | String | GS-Segment der Nachricht. Der Wert wird nur für die Absenderseite aufgefüllt, wenn für die an den Partner gesendeten Nachrichten eine Funktionsbestätigung empfangen wurde. |
| respondingfunctionalGroupControlNumber | Nein | String | Die Austauschkontrollnummer der Antwort |
| respondingFunctionalGroupId | Nein | String | Die Funktionsgruppen-ID der Antwort, die in der Bestätigung AK101 zugeordnet ist. |
| respondingtransactionSetControlNumber | Nein | String | Die Transaktionssatz-Kontrollnummer der Antwort |
| respondingTransactionSetId | Nein | String | Die Transaktionssatz-ID der Antwort an, die in der Bestätigung AK201 zugeordnet ist |
| statusCode | Ja | Boolean | Statuscode für die Transaktionssatzbestätigung. |
| segmentsCount | Ja | Enum | Bestätigungsstatuscode mit diesen zulässigen Werten: `Accepted`, `Rejected` und `AcceptedWithErrors` |
| processingStatus | Ja | Enum | Verarbeitungsstatuscode der Bestätigung mit diesen zulässigen Werten: `Received`, `Generated` und `Sent` |
| CorrelationMessageId | Nein | String | Korrelationsnachrichten-ID, die eine Kombination aus den Elementen {AgreementName}{*GroupControlNumber*}{TransactionSetControlNumber} ist. |
| isMessageFailed | Ja | Boolean | Gibt an, ob die X12-Nachricht fehlgeschlagen ist. |
| ak2Segment | Nein | String | Bestätigung für einen Transaktionssatz in der empfangenen Funktionsgruppe. |
| ak3Segment | Nein | String | Meldet Fehler in einem Datensegment. |
| ak5Segment | Nein | String | Meldet, ob der im AK2-Segment angegebene Transaktionssatz akzeptiert oder abgelehnt wurde (einschließlich des Grunds). |
|||||

## <a name="x12-interchange-tracking-schema"></a>X12-Austausch-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "isa09": "",
      "isa10": "",
      "isa11": "",
      "isa12": "",
      "isa14": "",
      "isa15": "",
      "isa16": ""
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Absenderpartners der X12-Nachricht |
| receiverPartnerName | Nein | String | Name des Empfangspartners der X12-Nachricht |
| senderQualifier | Ja | String | Qualifizierer des Absenderpartners |
| senderIdentifier | Ja | String | Bezeichner des Absenderpartners |
| receiverQualifier | Ja | String | Qualifizierer des Empfangspartners |
| receiverIdentifier | Ja | String | Bezeichner des Empfangspartners |
| agreementName | Nein | String | Name der X12-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | Enum | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| interchangeControlNumber | Nein | String | Austauschkontrollnummer |
| isaSegment | Nein | String | ISA-Segment der Nachricht. |
| isTechnicalAcknowledgmentExpected | Boolean | Gibt an, ob die technische Bestätigung in der X12-Vereinbarung konfiguriert ist.  |
| isMessageFailed | Ja | Boolean | Gibt an, ob die X12-Nachricht fehlgeschlagen ist. |
| isa09 | Nein | String | Austauschdatum des X12-Dokuments. |
| isa10 | Nein | String | Austauschuhrzeit des X12-Dokuments. |
| isa11 | Nein | String | Kontrollstandardsbezeichner für den X12-Austausch. |
| isa12 | Nein | String | Kontrollversionsnummer für den X12-Austausch an. |
| isa14 | Nein | String | X12-Bestätigung wurde angefordert. |
| isa15 | Nein | String | Indikator für Test oder Produktion. |
| isa16 | Nein | String | Elementtrennzeichen |
|||||

## <a name="x12-interchange-acknowledgment-tracking-schema"></a>X12-Austauschbestätigung-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "isaSegment": "",
      "respondingInterchangeControlNumber": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ta102": "",
      "ta103": "",
      "ta105": ""
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Absenderpartners der X12-Nachricht |
| receiverPartnerName | Nein | String | Name des Empfangspartners der X12-Nachricht |
| senderQualifier | Ja | String | Qualifizierer des Absenderpartners |
| senderIdentifier | Ja | String | Bezeichner des Absenderpartners |
| receiverQualifier | Ja | String | Qualifizierer des Empfangspartners |
| receiverIdentifier | Ja | String | Bezeichner des Empfangspartners |
| agreementName | Nein | String | Name der X12-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | Enum | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| interchangeControlNumber | Nein | String | Austauschkontrollnummer der technischen Bestätigung, die von Partnern empfangen wurde. |
| isaSegment | Nein | String | ISA-Segment der technischen Bestätigung, die von Partnern empfangen wurde. |
| respondingInterchangeControlNumber | Nein | String | Austauschkontrollnummer für die technische Bestätigung, die von Partnern empfangen wurde. |
| isMessageFailed | Ja | Boolean | Gibt an, ob die X12-Nachricht fehlgeschlagen ist. |
| statusCode | Ja | Enum | Austauschbestätigungs-Statuscode mit diesen zulässigen Werten: `Accepted`, `Rejected` und `AcceptedWithErrors` |
| processingStatus | Ja | Enum | Bestätigungsstatus mit diesen zulässigen Werten: `Received`, `Generated` und `Sent` |
| ta102 | Nein | String | Austauschdatum |
| ta103 | Nein | String | Austauschuhrzeit |
| ta105 | Nein | String | Austauschhinweiscode |
|||||

## <a name="x12-functional-group-tracking-schema"></a>X12-Funktionsgruppen-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "gsSegment": "",
      "isTechnicalAcknowledgmentExpected": "",
      "isFunctionalAcknowledgmentExpected": "",
      "isMessageFailed": "",
      "gs01": "",
      "gs02": "",
      "gs03": "",
      "gs04": "",
      "gs05": "",
      "gs07": "",
      "gs08": ""
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Absenderpartners der X12-Nachricht |
| receiverPartnerName | Nein | String | Name des Empfangspartners der X12-Nachricht |
| senderQualifier | Ja | String | Qualifizierer des Absenderpartners |
| senderIdentifier | Ja | String | Bezeichner des Absenderpartners |
| receiverQualifier | Ja | String | Qualifizierer des Empfangspartners |
| receiverIdentifier | Ja | String | Bezeichner des Empfangspartners |
| agreementName | Nein | String | Der Name der X12-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | Enum | Richtung des Nachrichtenflusses, entweder „Empfangen“ oder „Senden“. |
| interchangeControlNumber | Nein | String | Austauschkontrollnummer |
| functionalGroupControlNumber | Nein | String | Funktionskontrollnummer |
| gsSegment | Nein | String | GS-Segment der Nachricht. |
| isTechnicalAcknowledgmentExpected | Ja | Boolean | Gibt an, ob die technische Bestätigung in der X12-Vereinbarung konfiguriert ist. |
| isFunctionalAcknowledgmentExpected | Ja | Boolean | Gibt an, ob die Funktionsbestätigung in der X12-Vereinbarung konfiguriert ist. |
| isMessageFailed | Ja | Boolean | Gibt an, ob die X12-Nachricht fehlgeschlagen ist. |
| gs01 | Nein | String | Funktionsbezeichnercode |
| gs02 | Nein | String | Code des Anwendungsabsenders. |
| gs03 | Nein | String | Code des Anwendungsempfängers. |
| gs04 | Nein | String | Datum der Funktionsgruppe. |
| gs05 | Nein | String | Uhrzeit der Funktionsgruppe. |
| gs07 | Nein | String | Code der zuständigen Behörde. |
| gs08 | Nein | String | Der Bezeichnercode für die Version, das Release oder die Branche. |
|||||

## <a name="x12-functional-group-acknowledgment-tracking-schema"></a>X12-Funktionsgruppenbestätigung-Nachverfolgungsschema

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "senderQualifier": "",
      "senderIdentifier": "",
      "receiverQualifier": "",
      "receiverIdentifier": "",
      "agreementName": ""
   },
   "messageProperties": {
      "direction": "",
      "interchangeControlNumber": "",
      "functionalGroupControlNumber": "",
      "isaSegment": "",
      "gsSegment": "",
      "respondingfunctionalGroupControlNumber": "",
      "respondingFunctionalGroupId": "",
      "isMessageFailed": "",
      "statusCode": "",
      "processingStatus": "",
      "ak903": "",
      "ak904": "",
      "ak9Segment": ""
   }
}
```

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
|----------|----------|------|-------------|
| senderPartnerName | Nein | String | Name des Absenderpartners der X12-Nachricht |
| receiverPartnerName | Nein | String | Name des Empfangspartners der X12-Nachricht |
| senderQualifier | Ja | String | Qualifizierer des Absenderpartners |
| senderIdentifier | Ja | String | Bezeichner des Absenderpartners |
| receiverQualifier | Ja | String | Qualifizierer des Empfangspartners |
| receiverIdentifier | Ja | String | Bezeichner des Empfangspartners |
| agreementName | Nein | String | Name der X12-Vereinbarung, nach der Nachrichten aufgelöst werden. |
| direction | Ja | Enum | Richtung des Nachrichtenflusses, die entweder `receive` (Empfangen) oder `send` (Senden) sein kann. |
| interchangeControlNumber | Nein | String | Austauschkontrollnummer, die für die Sendeseite aufgefüllt wird, wenn eine technische Bestätigung von Partnern empfangen wird. |
| functionalGroupControlNumber | Nein | String | Funktionsgruppen-Kontrollnummer der technischen Bestätigung, die für die Sendeseite aufgefüllt wird, wenn eine technische Bestätigung von Partnern empfangen wird. |
| isaSegment | Nein | String | Entspricht der Austauschkontrollnummer, wird aber nur in bestimmten Fällen aufgefüllt. |
| gsSegment | Nein | String | Entspricht der Funktionsgruppen-Kontrollnummer, wird aber nur in bestimmten Fällen aufgefüllt. |
| respondingfunctionalGroupControlNumber | Nein | String | Kontrollnummer der ursprünglichen Funktionsgruppe. |
| respondingFunctionalGroupId | Nein | String | Wird in der Funktionsgruppen-ID der Bestätigung AK101 zugeordnet. |
| isMessageFailed | Boolean | Gibt an, ob die X12-Nachricht fehlgeschlagen ist. |
| statusCode | Ja | Enum | Bestätigungsstatuscode mit diesen zulässigen Werten: `Accepted`, `Rejected` und `AcceptedWithErrors` |
| processingStatus | Ja | Enum | Verarbeitungsstatuscode der Bestätigung mit diesen zulässigen Werten: `Received`, `Generated` und `Sent` |
| ak903 | Nein | String | Anzahl der empfangenen Transaktionssätze. |
| ak904 | Nein | String | Anzahl von Transaktionssätzen an, die in der identifizierten Funktionsgruppe akzeptiert wurden. |
| ak9Segment | Nein | String | Gibt an, ob die im AK1-Segment angegebene Funktionsgruppe akzeptiert oder abgelehnt wurde (einschließlich des Grunds). |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B-Protokollnachverfolgungsschemas

Informationen zu B2B-Protokollnachverfolgungsschemas finden Sie unter:

* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Benutzerdefinierte B2B-Nachverfolgungsschemas](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von B2B-Nachrichten mit Azure Monitor-Protokollen](../logic-apps/monitor-b2b-messages-log-analytics.md)