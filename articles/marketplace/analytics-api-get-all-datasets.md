---
title: API zum Abrufen aller Datasets
description: Verwenden Sie diese API, um alle verfügbaren Datasets für Analysen im kommerziellen Marketplace abzurufen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 23aac2c94ffd909ca132cbc481998b9eda317156
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102583486"
---
# <a name="get-all-datasets-api"></a>API zum Abrufen aller Datasets

Mit der API zum Abrufen aller Datasets werden alle verfügbaren Datasets abgerufen. Zu den Datasets gehören Tabellen, Spalten, Metriken und Zeitbereiche.

**Anforderungssyntax**

| **Methode** | **Anforderungs-URI** |
| --- | --- |
| GET | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledDataset?datasetName={Dataset Name}` |

**Anforderungsheader**

| **Header** | **Typ** | **Beschreibung** |
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |

**Pfadparameter**

Keine

**Query parameter (Abfrageparameter)**

| **Parametername** | **Typ** | **Erforderlich** | **Beschreibung** |
| --- | --- | --- | --- |
| `datasetName` | string | Nein | Filter, um nur Details zu einem Dataset abzurufen |

**Anforderungsnutzlast**

Keine

**Glossar**

Keine

**Antwort**

Die Antwortnutzdaten sind wie folgt strukturiert:

Antwortcode: 200, 400, 401, 403, 404, 500

Beispiel einer Antwortnutzlast:

```json
{
   "Value":[
      {
         "DatasetName ":"string",
         "SelectableColumns":[
            "string"
         ],
         "AvailableMetrics":[
            "string"
         ],
         "AvailableDateRanges ":[
            "string"
         ]
      }
   ],
   "TotalCount":int,
   "Message":"<Error Message>",
   "StatusCode": int
}
```

**Glossar**

In dieser Tabelle werden die Schlüsselelemente in der Antwort definiert:

| **Parameter** | **Beschreibung** |
| --- | --- |
| `DatasetName` | Name des Datasets, das von diesem Arrayobjekt definiert wird |
| `SelectableColumns` | Rohspalten, die in den ausgewählten Spalten angegeben werden können |
| `AvailableMetrics` | Spaltennamen von Aggregationen/Metriken, die in den ausgewählten Spalten angegeben werden können |
| `AvailableDateRanges` | Datumsbereich, der in Berichtsabfragen für das Dataset verwendet werden kann |
| `NextLink` | Link zur nächsten Seite, wenn die Daten auf mehrere Seiten verteilt sind |
| `TotalCount` | Anzahl von Datasets im Wertarray |
| `StatusCode` | Ergebniscode. Die möglichen Werte sind 200, 400, 401, 403, 500. |
