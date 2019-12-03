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
ms.date: 10/22/2019
ms.custom: seodec18
ms.openlocfilehash: cf826c47c61e3714a05dd81fe3eea4e6ee0b03f4
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012495"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Bewährte Methoden für die Auswahl einer Time Series-ID

Dieser Artikel fasst die Wichtigkeit der Time Series-ID für Ihre Azure Time Series Insights Preview-Umgebung zusammen sowie bewährte Methoden für deren Auswahl.

## <a name="choose-a-time-series-id"></a>Auswählen einer Time Series-ID

Das Auswählen einer Time Series-ID ist wie das Auswählen eines Partitionsschlüssels für eine Datenbank. Sie muss ausgewählt werden, während Sie eine Time Series Insights Preview-Umgebung erstellen. Dabei handelt es sich um eine *unveränderliche* Eigenschaft. Das heißt, nachdem Sie eine Time Series Insights Preview-Umgebung mit einer Time Series-ID erstellt haben, können Sie sie für diese Umgebung nicht mehr ändern. 

> [!IMPORTANT]
> Bei der Time Series-ID muss die Groß-/Kleinschreibung beachtet werden.

Die Auswahl einer geeigneten Time Series-ID ist entscheidend. Im Folgenden finden Sie einige der bewährten Methoden, die Sie befolgen können:

* Wählen Sie einen Partitionsschlüssel mit vielen unterschiedlichen Werten (z. B. Hunderten oder Tausenden) aus. In vielen Fällen kann dies die Geräte-ID, die Sensor-ID oder die Tag-ID in Ihrem JSON sein.
* Die Time Series-ID sollte auf Blattknotenebene Ihres [Zeitreihenmodells](./time-series-insights-update-tsm.md) eindeutig sein.
* Wenn es sich bei ihrer Ereignisquelle um einen IoT-Hub handelt, ist Ihre Time Series-ID höchstwahrscheinlich *iothub-connection-device-id*.
* Das Zeichenlimit für die Zeichenfolge des Eigenschaftsnamens der Time Series-ID ist 128. Für den Eigenschaftswert der Time Series-ID ist das Zeichenlimit 1.024.
* Wenn ein eindeutiger Eigenschaftswert für die Time Series-ID fehlt, wird er als Nullwert behandelt und folgt derselben Regel der Eindeutigkeitseinschränkung.
* Darüber hinaus können Sie bis zu *drei* Schlüsseleigenschaften als Ihre Time Series-ID auswählen. Deren Kombination ergibt einen zusammengesetzten Schlüssel, der die Time Series-ID darstellt.  

  > [!NOTE]
  > Ihre drei Schlüsseleigenschaften müssen Zeichenfolgen sein.
  > Sie müssten dann diesen zusammengesetzten Schlüssel abfragen, anstatt jede Eigenschaft einzeln.

Die folgenden Szenarien beschreiben die Auswahl von mehr als einer Schlüsseleigenschaft als Ihre Time Series-ID.  

### <a name="example-1-time-series-id-with-a-unique-key"></a>Beispiel 1: Time Series-ID mit einem eindeutigen Schlüssel

* Sie besitzen Legacygruppen von Ressourcen. Jede besitzt einen eindeutigen Schlüssel.
* Eine Gruppe wird eindeutig durch die Eigenschaft **deviceId** identifiziert. Für eine andere Gruppe ist die eindeutige Eigenschaft **objectId**. Keine der Gruppen enthält die eindeutige Eigenschaft der jeweils anderen Gruppe. In diesem Beispiel würden Sie zwei Schlüssel, **deviceId** und die **objectId**, als eindeutige Schlüssel auswählen.
* Wir akzeptieren Nullwerte, und das Fehlen einer Eigenschaft in der Ereignisnutzlast zählt als ein Nullwert. Dies ist auch die geeignete Methode, um das Senden von Daten an zwei Ereignisquellen zu handhaben, wobei die Daten in jeder Ereignisquelle eine eindeutige Time Series-ID haben.

### <a name="example-2-time-series-id-with-a-composite-key"></a>Beispiel 2: Time Series-ID mit einem zusammengesetzten Schlüssel

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

Im Azure-Portal können Sie diesen zusammengesetzten Schlüssel eingeben als: 

`[{"name":"sensorId","type":"String"},{"name":"flrRm","type":"String"},{"name":"location","type":"string"}]`

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Datenmodellierung](./time-series-insights-update-tsm.md).

* Planen Ihrer [Azure Time Series Insights Preview-Umgebung](./time-series-insights-update-plan.md).