---
title: Bewährte Methoden für die Auswahl einer Time Series-ID – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie die bewährten Methoden bei der Auswahl einer Time Series-ID in Azure Time Series Insights Preview.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/19/2019
ms.custom: seodec18
ms.openlocfilehash: 6fd8d6187c86306840c33b3aaf334e71086b20a1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75452741"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Bewährte Methoden für die Auswahl einer Time Series-ID

Dieser Artikel fasst die Wichtigkeit der Time Series-ID für Ihre Azure Time Series Insights Preview-Umgebung zusammen sowie bewährte Methoden für deren Auswahl.

## <a name="choose-a-time-series-id"></a>Auswählen einer Time Series-ID

Die Auswahl einer geeigneten Time Series-ID ist entscheidend. Das Auswählen einer Time Series-ID ist wie das Auswählen eines Partitionsschlüssels für eine Datenbank. Dies ist erforderlich, wenn Sie eine Time Series Insights-Umgebung (Vorschauversion) erstellen. 

> [!IMPORTANT]
> Für Zeitreihen-IDs gilt Folgendes:
> * Bei ihnen wird nach *Groß-/Kleinschreibung* unterschieden: Die Groß- und Kleinschreibung von Buchstaben und Zeichen wird bei Suchvorgängen, Vergleichen, Updates und der Partitionierung berücksichtigt.
> * Sie sind *unveränderliche* Eigenschaften: Nach dem Erstellen können sie nicht mehr geändert werden.

> [!TIP]
> Wenn es sich bei Ihrer Ereignisquelle um einen IoT-Hub handelt, lautet Ihre Zeitreihen-ID wahrscheinlich ***iothub-connection-device-id***.

Zu den wichtigsten bewährten Methoden gehören:

* Wählen Sie einen Partitionsschlüssel mit vielen unterschiedlichen Werten (z. B. Hunderten oder Tausenden) aus. In vielen Fällen kann dies die Geräte-ID, die Sensor-ID oder die Tag-ID in Ihrem JSON sein.
* Die Time Series-ID sollte auf Blattknotenebene Ihres [Zeitreihenmodells](./time-series-insights-update-tsm.md) eindeutig sein.
* Das Zeichenlimit für die Zeichenfolge des Eigenschaftsnamens der Time Series-ID ist 128. Für den Eigenschaftswert der Time Series-ID ist das Zeichenlimit 1.024.
* Wenn ein eindeutiger Eigenschaftswert für die Time Series-ID fehlt, wird er als Nullwert behandelt und folgt derselben Regel der Eindeutigkeitseinschränkung.
* Darüber hinaus können Sie bis zu *drei* Schlüsseleigenschaften als Ihre Time Series-ID auswählen. Deren Kombination ergibt einen zusammengesetzten Schlüssel, der die Time Series-ID darstellt.  
  > [!NOTE]
  > Ihre drei Schlüsseleigenschaften müssen Zeichenfolgen sein.
  > Sie müssten dann diesen zusammengesetzten Schlüssel abfragen, anstatt jede Eigenschaft einzeln.

## <a name="select-more-than-one-key-property"></a>Auswählen mehrerer Schlüsseleigenschaften

Die folgenden Szenarien beschreiben die Auswahl von mehr als einer Schlüsseleigenschaft als Ihre Time Series-ID.  

### <a name="example-1-time-series-id-with-a-unique-key"></a>Beispiel 1: Time Series-ID mit einem eindeutigen Schlüssel

* Sie besitzen Legacygruppen von Ressourcen. Jede besitzt einen eindeutigen Schlüssel.
* Eine Gruppe wird eindeutig durch die Eigenschaft **deviceId** identifiziert. Für eine andere Gruppe ist die eindeutige Eigenschaft **objectId**. Keine der Gruppen enthält die eindeutige Eigenschaft der jeweils anderen Gruppe. In diesem Beispiel würden Sie zwei Schlüssel, **deviceId** und die **objectId**, als eindeutige Schlüssel auswählen.
* Wir akzeptieren Nullwerte, und das Fehlen einer Eigenschaft in der Ereignisnutzlast zählt als ein Nullwert. Dies ist auch die geeignete Methode, um das Senden von Daten an zwei Ereignisquellen zu handhaben, wobei die Daten in jeder Ereignisquelle eine eindeutige Time Series-ID haben.

### <a name="example-2-time-series-id-with-a-composite-key"></a>Beispiel 2: Time Series-ID mit einem zusammengesetzten Schlüssel

* Ihre Anforderung ist, dass mehrere Eigenschaften innerhalb derselben Gruppe von Ressourcen eindeutig sind. 
* Sie sind ein Hersteller von intelligenten Gebäuden und stellen in jedem Raum Sensoren bereit. In jedem Raum haben Sie normalerweise dieselben Werte für **sensorId**. Beispiele sind **sensor1**, **sensor2** und **sensor3**.
* Ihr Gebäude besitzt sich überlappende Stockwerk- und Raumnummern zwischen Standorten in der Eigenschaft **flrRm**. Diese Nummern haben Werte wie **1a**, **2b** und **3a**.
* Sie haben eine Eigenschaft, **location** (Ort), die Werte wie **Redmond**, **Barcelona** und **Tokio** enthält. Um Eindeutigkeit herzustellen, legen Sie die folgenden drei Eigenschaften als Ihre Time Series-ID-Schlüssel fest: **sensorId**, **flrRm** und **location**.

Beispielrohereignis:

```JSON
{
  "sensorId": "sensor1",
  "flrRm": "1a",
  "location": "Redmond",
  "temperature": 78
}
```

Im Azure-Portal können Sie diesen zusammengesetzten Schlüssel dann folgendermaßen eingeben: 

```JSON
[{"name":"sensorId","type":"String"},{"name":"flrRm","type":"String"},{"name":"location","type":"string"}]
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Datenmodellierung](./time-series-insights-update-tsm.md).

* Planen Ihrer [Azure Time Series Insights Preview-Umgebung](./time-series-insights-update-plan.md).