---
title: API zum Aktualisieren eines Berichts
description: Verwenden Sie diese API, um Parameter für Analyseberichte im kommerziellen Marketplace zu ändern.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 38680eb291417ded4c2f93539e8d1ae091b1d560
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102583483"
---
# <a name="update-report-api"></a>API zum Aktualisieren eines Berichts

Mit dieser API können Sie einen Berichtsparameter ändern.

**Anforderungssyntax**

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| PUT | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport/{Report ID}` |
|||

**Anforderungsheader**

| Header | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |
||||

**Pfadparameter**

Keine

**Query parameter (Abfrageparameter)**

| Parametername | Erforderlich | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- | ------------- |
| `reportId` | Ja | Zeichenfolge | ID des Berichts, der geändert wird |
|||||

**Anforderungsnutzlast**

```json
{
  "ReportName": "string",
  "Description": "string",
  "StartTime": "string",
  "RecurrenceInterval": 0,
  "RecurrenceCount": 0,
  "Format": "string",
  "CallbackUrl": "string"
}
```

**Glossar**

In dieser Tabelle sind die Schlüsseldefinitionen der Elemente in den Anforderungsnutzdaten aufgeführt.

| Parameter | Erforderlich | BESCHREIBUNG | Zulässige Werte |
| ------------ | ------------- | ------------- | ------------- |
| `ReportName` | Ja | Name, der dem Bericht zugewiesen werden soll | Zeichenfolge |
| `Description` | Nein | Beschreibung des erstellten Berichts | Zeichenfolge |
| `StartTime` | Ja | Zeitstempel, nach dem die Berichterstellung beginnt | Zeichenfolge |
| `RecurrenceInterval` | Nein | Häufigkeit in Stunden, mit der der Bericht generiert werden soll. Der Mindestwert ist 4. | integer |
| `RecurrenceCount` | Nein | Anzahl der zu generierenden Berichte. Der Standardwert ist „Unbestimmt“. | integer |
| `Format` | Ja | Dateiformat für die exportierte Datei. Der Standardwert ist „CSV“. | CSV/TSV |
| `CallbackUrl` | Ja | HTTPS-Rückruf-URL, die bei der Berichterstellung aufgerufen werden soll | Zeichenfolge |
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
| `ReportId` | Universell eindeutiger Bezeichner (Universally Unique Identifier, UUID) des aktualisierten Berichts |
| `ReportName` | Name für den Bericht in den Anforderungsnutzdaten |
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
