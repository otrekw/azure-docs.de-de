---
title: API zum Abrufen von Berichtsabfragen
description: Verwenden Sie diese API, um alle Abfragen abzurufen, die in Analyseberichten zum kommerziellen Marketplace verwendet werden können.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: smannepalle
ms.author: smannepalle
ms.reviewer: sroy
ms.date: 3/08/2021
ms.openlocfilehash: f3a6351f726a5b1bd408764280ad2487cf7ecd95
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355811"
---
# <a name="get-report-queries-api"></a>API zum Abrufen von Berichtsabfragen

Mit der API zum Abrufen von Berichtsabfragen werden alle Abfragen abgerufen, die in Berichten verwendet werden können. Es werden standardmäßig alle Systemabfragen und benutzerdefinierten Abfragen abgerufen.

**Anforderungssyntax**

| **Methode** | **Anforderungs-URI** |
| --- | --- |
| GET | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries?queryId={QueryID}&queryName={QueryName}&includeSystemQueries={include_system_queries}&includeOnlySystemQueries={include_only_system_queries}` |
|||

**Anforderungsheader**

| **Header** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |
||||

**Pfadparameter**

Keine

**Abfrageparameter**

| **Parametername** | **Typ** | **Erforderlich** | **Beschreibung** |
| --- | --- | --- | --- |
| `queryId` | string | Nein | Filter, um nur Details zu Abfragen mit der in diesem Argument angegebenen ID abzurufen |
| `queryName` | Zeichenfolge | Nein | Filter, um nur Details zu Abfragen mit dem in diesem Argument angegebenen Namen abzurufen |
| `IncludeSystemQueries` | boolean | Nein | Vordefinierte Systemabfragen in die Antwort einschließen |
| `IncludeOnlySystemQueries` | boolean | Nein | Nur Systemabfragen in die Antwort einschließen |
|||||

**Anforderungsnutzdaten**

Keine

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
      "QueryId": "string",
      "Name": "string",
      "Description": "string",
      "Query": "string",
      "Type": "string",
      "User": "string",
      "CreatedTime": "string",
      "ModifiedTime": "string"
    }
  ],
  "TotalCount": 0,
  "Message": "string",
  "StatusCode": 0
}
```

**Glossar**

In dieser Tabelle werden die Schlüsseldefinitionen der Elemente in der Antwort beschrieben.

| **Parameter** | **Beschreibung** |
| --- | --- |
| `QueryId` | Eindeutige UUID der Abfrage |
| `Name` | Der Name, der der Abfrage bei ihrer Erstellung gegeben wurde |
| `Description` | Die Beschreibung, die der Abfrage bei ihrer Erstellung hinzugefügt wurde |
| `Query` | Zeichenfolge der Berichtsabfrage |
| `Type` | Für von Benutzern erstellte Abfragen auf `userDefined` und für vordefinierte Systemabfragen auf `system` festlegen |
| `User` | ID des Benutzers, der die Abfrage erstellt hat |
| `CreatedTime` | Zeitpunkt der Erstellung der Abfrage |
| `TotalCount` | Anzahl von Datasets im Wertarray |
| `StatusCode` | Ergebniscode. Die möglichen Werte sind 200, 400, 401, 403, 500. |
|||
