---
title: API zum Fortsetzen von Berichtsausführungen
description: Verwenden Sie diese API, um die geplante Ausführung eines angehaltenen Analyseberichts im kommerziellen Marketplace fortzusetzen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 4a11783b28352cb62c5a3c0d38e45dcdc47a8d86
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102583485"
---
# <a name="resume-report-executions-api"></a>API zum Fortsetzen von Berichtsausführungen

Durch die Ausführung dieser API können Sie die geplante Ausführung eines angehaltenen Analyseberichts im kommerziellen Marketplace fortsetzen.

**Anforderungssyntax**

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| PUT | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport/resume/{reportId}` |
|||

**Anforderungsheader**

| Header | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |
||||

**Pfadparameter**

Keine

**Abfrageparameter**

| Parametername | Erforderlich | Typ | Beschreibung |
| ------------ | ------------- | ------------- | ------------- |
| `reportId` | Ja | Zeichenfolge | ID des Berichts, der geändert wird |
|||||

**Glossar**

Keine

**Antwort**

Die Antwortnutzdaten sind wie folgt im JSON-Format strukturiert:

Antwortcode: 200, 400, 401, 403, 404, 500

Antwortnutzlast:

```json
{
  "Value": [
    {
      "ReportId": "string",
      "ReportName": "string",
      "Description": "string",
      "QueryId": "string",
      "Query": "string",
      "User": "string",
      "CreatedTime": "string",
      "ModifiedTime": "string",
      "StartTime": "string",
      "ReportStatus": "string",
      "RecurrenceInterval": 0,
      "RecurrenceCount": 0,
      "CallbackUrl": "string",
      "Format": "string"
    }
  ],
  "TotalCount": 0,
  "Message": "string",
  "StatusCode": 0
}
```

**Glossar**

| Parameter | Beschreibung |
| ------------ | ------------- |
| `ReportId` | Universell eindeutiger Bezeichner (Universally Unique Identifier, UUID) des fortgesetzten Berichts |
| `ReportName` | Name, der dem Bericht während der Erstellung gegeben wurde |
| `Description` | Beschreibung, die während der Erstellung des Berichts angegeben wurde |
| `QueryId` | Abfrage-ID, die zum Zeitpunkt der Erstellung des Berichts übergeben wurde |
| `Query` | Abfragetext, der für diesen Bericht ausgeführt wird |
| `User` | Benutzer-ID, die zum Erstellen des Berichts verwendet wurde |
| `CreatedTime` | Zeitpunkt, zu dem der Bericht erstellt wurde. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ModifiedTime` | Uhrzeit der letzten Änderung des Berichts. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `StartTime` | Uhrzeit, zu der die Berichtsausführung beginnt. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ReportStatus` | Status der Berichtsausführung. Die möglichen Werte sind „Angehalten“, „Aktiv“ und „Inaktiv“. |
| `RecurrenceInterval` | Wiederholungsintervall, das bei der Erstellung des Berichts angegeben wurde |
| `RecurrenceCount` | Wiederholungsanzahl, die bei der Erstellung des Berichts angegeben wurde |
| `CallbackUrl` | Rückruf-URL, die in der Anforderung angegeben wurde |
| `Format` | Format der Berichtsdateien |
|||
