---
title: API zum Löschen von Berichtsabfragen
description: Verwenden Sie diese API, um benutzerdefinierte Abfragen für Analysen für den kommerziellen Marketplace zu löschen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 4fc3479f1e35970a97684396a7a2e0c0c2582128
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102583657"
---
# <a name="delete-report-queries-api"></a>API zum Löschen von Berichtsabfragen

Mit dieser API werden benutzerdefinierte Abfragen gelöscht.

**Anforderungssyntax**

| **Methode** | **Anforderungs-URI** |
| --- | --- |
| Delete | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries/{queryId}` |

**Anforderungsheader**

| **Header** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |

**Pfadparameter**

| **Parametername** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| `queryId` | string | Filter, um nur Details zu Abfragen mit der in diesem Argument angegebenen ID zu erhalten. |

**Abfrageparameter**

Keine

**Anforderungsnutzdaten**

Keine

**Glossar**

Keine

**Antwort**

Die Antwortnutzlast ist wie folgt im JSON-Format strukturiert.

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

In dieser Tabelle sind die Schlüsseldefinitionen der Elemente in der Antwort aufgeführt.

| **Parameter** | **Beschreibung** |
| --- | --- |
| `QueryId` | Eindeutige UUID der gelöschten Abfrage |
| `Name` | Name der gelöschten Abfrage |
| `Description` | Beschreibung der gelöschten Abfrage |
| `Query` | Berichtsabfrage-Zeichenfolge der gelöschten Abfrage |
| `Type` | userDefined |
| `User` | ID des Benutzers, der die Abfrage erstellt hat |
| `CreatedTime` | Zeitpunkt, zu dem die Abfrage erstellt wurde |
| `ModifiedTime` | Null |
| `TotalCount` | Anzahl von Datasets im Wertarray |
| `StatusCode` | Ergebniscode. Die möglichen Werte sind 200, 400, 401, 403, 500. |
