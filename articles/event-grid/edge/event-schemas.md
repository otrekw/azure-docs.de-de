---
title: Ereignisschemas – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Ereignisschemas in Event Grid in IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: ea36c40f2038d016afb0c45944a98d4d90df6240
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171566"
---
# <a name="event-schemas"></a>Ereignisschemas

Das Event Grid-Modul akzeptiert und übermittelt Ereignisse im JSON-Format. Derzeit werden drei Schemas von Event Grid unterstützt:

* **EventGridSchema**
* **CustomSchema**
* **CloudEventSchema**

Sie können das Schema, das ein Herausgeber einhalten muss, während der Themaerstellung konfigurieren. Wenn keine Angabe erfolgt, wird standardmäßig **EventGridSchema** verwendet. Ereignisse, die nicht dem erwarteten Schema entsprechen, werden zurückgewiesen.

Abonnenten können auch das Schema konfigurieren, in dem Sie die Ereignisse übermittel haben möchten. Wenn keine Angabe erfolgt, wird standardmäßig das Schema des Themas verwendet.
Derzeit muss das Abonnentenübermittlungsschema dem Eingabeschema seines Themas entsprechen. 

## <a name="eventgrid-schema"></a>EventGrid-Schema

Das EventGrid-Schema besteht aus einem Satz erforderlicher Eigenschaften, die eine Veröffentlichungsentität einhalten muss. Jeder Herausgeber muss die Felder der obersten Ebene auffüllen.

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

### <a name="eventgrid-schema-properties"></a>Eigenschaften des EventGrid-Schemas

Alle Ereignisse weisen die folgenden Daten auf oberster Ebene auf:

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
| -------- | ---- | ----------- |-----------
| topic | string | Nein | Sollte dem Thema entsprechen, für das es veröffentlicht wurde. Event Grid füllt dies mit dem Namen des Themas auf, für das es veröffentlicht wird, wenn nichts angegeben ist. |
| subject | string | Ja | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | string | Ja | Der Ereignistyp für diese Ereignisquelle, z. B. BlobCreated. |
| eventTime | string | Ja | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | string | Nein | Eindeutiger Bezeichner für das Ereignis. |
| data | Objekt (object) | Nein | Wird zum Erfassen von Ereignisdaten verwendet, die für die Veröffentlichungsentität spezifisch sind. |
| dataVersion | string | Ja | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | string | Nein | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

### <a name="example--eventgrid-schema-event"></a>Beispiel – EventGrid-Schemaereignis

```json
[
  {
    "id": "1807",
    "eventType": "recordInserted",
    "subject": "myapp/vehicles/motorcycles",
    "eventTime": "2017-08-10T21:03:07+00:00",
    "data": {
      "make": "Ducati",
      "model": "Monster"
    },
    "dataVersion": "1.0"
  }
]
```

## <a name="customevent-schema"></a>CustomEvent-Schema

Im benutzerdefinierten Schema gibt es keine obligatorischen Eigenschaften, die wie das EventGrid-Schema erzwungen werden. Die Veröffentlichungsentität kann das Ereignisschema vollständig kontrollieren. Sie bietet maximale Flexibilität und ermöglicht Szenarien, in denen ein ereignisbasiertes System bereits vorhanden ist und Sie vorhandene Ereignisse wiederverwenden und/oder nicht an ein bestimmtes Schema gebunden sein möchten.

### <a name="custom-schema-properties"></a>Eigenschaften des benutzerdefinierten Schemas

Keine obliagtorischen Eigenschaften. Es liegt an der Veröffentlichungsentität, die Nutzlast zu ermitteln.

### <a name="example--custom-schema-event"></a>Beispiel – benutzerdefiniertes Schemaereignis

```json
[
  {
    "eventdata": {
      "make": "Ducati",
      "model": "Monster"
    }
  }
]
```

## <a name="cloudevent-schema"></a>CloudEvents-Schema

Zusätzlich zu den vorgenannten Schemas unterstützt Event Grid nativ Ereignisse im [CloudEvents-JSON-Schema](https://github.com/cloudevents/spec/blob/master/json-format.md). CloudEvents ist eine offene Spezifikation zur Beschreibung von Ereignisdaten. Sie vereinfacht die Interoperabilität, indem ein allgemeines Ereignisschema für die Veröffentlichung bereitgestellt wird und Ereignisse genutzt werden. Sie ist Teil von [CNCF](https://www.cncf.io/), und die derzeit verfügbare Version lautet 1.0-rc1.

### <a name="cloudevent-schema-properties"></a>CloudEvent-Schemaeigenschaften

Informationen zu den obligatorischen Umschlageigenschaften finden Sie in der [CloudEvents-Spezifikation](https://github.com/cloudevents/spec/blob/master/json-format.md#3-envelope).

### <a name="example--cloud-event"></a>Beispiel – Cloudereignis
```json
[{
       "id": "1807",
       "type": "recordInserted",
       "source": "myapp/vehicles/motorcycles",
       "time": "2017-08-10T21:03:07+00:00",
       "datacontenttype": "application/json",
       "data": {
            "make": "Ducati",
            "model": "Monster"
        },
        "dataVersion": "1.0",
        "specVersion": "1.0-rc1"
}]
```
