---
title: API zum Löschen eines Berichts
description: Verwenden Sie diese API, um alle Berichts- und Berichtsausführungs-Datensätze für kommerzielle Marketplace-Analyseberichte zu löschen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 7c39f8bc0db44f1d8aa885969ca09d90b0dcd332
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102583627"
---
# <a name="delete-report-api"></a>API zum Löschen eines Berichts

Bei der Ausführung löscht diese API alle Berichts- und Berichtsausführungs-Datensätze.

**Anforderungssyntax**

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| Delete | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport/{Report ID}` |
|||

**Anforderungsheader**

| Header | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken in der Form `Bearer <token>` |
| Inhaltstyp | Zeichenfolge | `Application/JSON` |
||||

**Pfadparameter**

Keine

**Abfrageparameter**

| Parametername | Erforderlich | Zeichenfolge | Beschreibung |
| ------------ | ------------- | ------------- | ------------- |
| `reportId` | Ja | Zeichenfolge | ID des Berichts, der geändert wird |
|||||

**Glossar**

Keine

**Antwort**

Die Antwortnutzdaten sind wie folgt strukturiert:

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
| `ReportId` | Eindeutige UUID des gelöschten Berichts |
| `ReportName` | Name, der dem Bericht während der Erstellung gegeben wurde |
| `Description` | Beschreibung, die während der Erstellung des Berichts angegeben wurde |
| `QueryId` | Abfrage-ID, die zum Zeitpunkt der Erstellung des Berichts übergeben wurde |
| `Query` | Abfragetext, der für diesen Bericht ausgeführt wird |
| `User` | Benutzer-ID, die zum Erstellen des Berichts verwendet wird |
| `CreatedTime` | Zeitpunkt, zu dem der Bericht erstellt wurde. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ModifiedTime` | Uhrzeit der letzten Änderung des Berichts. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `StartTime` | Uhrzeit, zu der die Berichtsausführung beginnt. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ReportStatus` | Status der Berichtsausführung. Die möglichen Werte sind „Angehalten“, „Aktiv“ und „Inaktiv“. |
| `RecurrenceInterval` | Wiederholungsintervall, das bei der Erstellung des Berichts angegeben wurde |
| `RecurrenceCount` | Anzahl der Wiederholungen, die bei der Erstellung des Berichts angegeben wurde |
| `CallbackUrl` | Rückruf-URL, die in der Anforderung angegeben wurde |
| `Format` | Format der Berichtsdateien |
|||
