---
title: API zum Abrufen eines Berichts
description: Verwenden Sie diese API, um Analyseberichte abzurufen, die im Partner Center geplant wurden.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: smannepalle
ms.author: smannepalle
ms.reviewer: sroy
ms.date: 3/08/2021
ms.openlocfilehash: d9248559f785ffb3b63e1f3d275e33a205cb682f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340162"
---
# <a name="get-report-api"></a>API zum Abrufen eines Berichts

Diese API ruft alle geplanten Berichte ab.

**Anforderungssyntax**

| **Methode** | **Anforderungs-URI** |
| --- | --- |
| GET | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport?reportId={Report ID}&reportName={Report Name}&queryId={Query ID} ` |

**Anforderungsheader**

| **Header** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |

**Pfadparameter**

Keine

**Abfrageparameter**

| **Parametername** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| `reportId` | Nein | Zeichenfolge | Filter, mit dem Sie ausschließlich Details zu Berichten mit dem in diesem Argument angegebenen `reportId` abrufen können. |
| `reportName` | Nein | Zeichenfolge | Filter, mit dem Sie ausschließlich Details zu Berichten mit dem in diesem Argument angegebenen `reportName` abrufen können. |
| `queryId` | Nein | boolean | Vordefinierte Systemabfragen in die Antwort einschließen |

**Glossar**

Keine

**Antwort**

Die Antwortnutzlast ist wie folgt im JSON-Format strukturiert:

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
      " RecurrenceCount": 0,
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

In dieser Tabelle sind die Schlüsseldefinitionen der Elemente in der Antwort aufgeführt.

| **Parameter** | **Beschreibung** |
| --- | --- |
| `ReportId` | Eindeutige UUID des erstellten Berichts |
| `ReportName` | Name für den Bericht in den Anforderungsnutzdaten |
| `Description` | Beim Erstellen des Berichts angegebene Beschreibung |
| `QueryId` | Abfrage-ID, die zum Zeitpunkt der Erstellung des Berichts übergeben wurde |
| `Query` | Abfragetext, der für diesen Bericht ausgeführt wird |
| `User` | Benutzer-ID, die zum Erstellen des Berichts verwendet wird |
| `CreatedTime` | Zeitpunkt, zu dem der Bericht erstellt wurde. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ModifiedTime` | Uhrzeit der letzten Änderung des Berichts. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `StartTime` | Zeitpunkt, zu dem die Ausführung beginnt. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ReportStatus` | Status der Berichtsausführung. Die möglichen Werte sind „Angehalten“, „Aktiv“ und „Inaktiv“. |
| `RecurrenceInterval` | Wiederholungsintervall, das bei der Erstellung des Berichts angegeben wurde |
| `RecurrenceCount` | Anzahl der Wiederholungen, die bei der Erstellung des Berichts angegeben wurde |
| `CallbackUrl` | Rückruf-URL, die in der Anforderung angegeben wurde |
| `Format` | Format der Berichtsdateien |
