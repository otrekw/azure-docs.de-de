---
title: 'Modellvariablen: Azure Time Series Insights Gen2 | Microsoft-Dokumentation'
description: Modellvariablen
author: shreyasharmamsft
ms.author: shresha
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 01/22/2021
ms.openlocfilehash: 01184a4eb2aac81bbcabcebf89ef10afeabddbe8
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107872963"
---
# <a name="time-series-model-variables"></a>Zeitreihenmodellvariablen

In diesem Artikel werden die Zeitreihenmodellvariablen beschrieben, die Formel- und Berechnungsregeln bei Ereignissen angeben.

Jede Variable kann eine der drei folgenden Arten aufweisen: *numerisch*, *kategorisch* und *aggregiert*.

* **Numerische** Variablen arbeiten mit kontinuierlichen numerischen Werten.
* **Kategorische** Variablen arbeiten mit einem definierten Satz diskreter Werte.
* Bei **aggregierten** Werten werden mehrere Variablen einer einzigen Art kombiniert (entweder alle numerischen oder alle kategorischen Variablen).

Die folgende Tabelle zeigt die Eigenschaften, die für die jeweilige Variablenart relevant sind.

[![Tabelle für Zeitreihenmodellvariable](media/v2-update-tsm/time-series-model-variable-table.png)](media/v2-update-tsm/time-series-model-variable-table.png#lightbox)

## <a name="numeric-variables"></a>Numerische Variablen

| Variableneigenschaft | BESCHREIBUNG |
| --- | ---|
| Variablenfilter | Filter sind optionale Bedingungsklauseln, um die Anzahl von Zeilen zu beschränken, die bei der Berechnung berücksichtigt werden. |
| Variablenwert | Für Berechnungen verwendete Telemetriewerte, die aus Geräten oder Sensoren stammen oder mithilfe von Zeitreihenausdrücken transformiert werden. Numerische Typ-Variablen müssen entweder `Double` oder `Long` sein, um dem Datentyp der eingehenden Daten zu entsprechen.|
| Variableninterpolation | Die Interpolation gibt an, wie ein Signal anhand von vorhandenen Daten rekonstruiert wird. Für numerische Variablen sind die Interpolationsoptionen *Schritt* und *linear* verfügbar. |
| Variablenaggregation | Führen Sie Berechnungen über die unterstützten [Aggregationsfunktionen für numerische Variablen](/rest/api/time-series-insights/reference-time-series-expression-syntax#numeric-variable-kind) durch. |

Variablen entsprechen dem folgenden JSON-Beispiel:

```JSON
"Interpolated Speed": {
  "kind": "numeric",
  "value": {
    "tsx": "$event['Speed-Sensor'].Double"
  },
  "filter": null,
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span": "P1D"
    }
  },
  "aggregation": {
    "tsx": "right($value)"
  }
}
```

## <a name="categorical-variables"></a>Kategorische Variablen

| Variableneigenschaft | BESCHREIBUNG |
| --- | ---|
| Variablenfilter | Filter sind optionale Bedingungsklauseln, um die Anzahl von Zeilen zu beschränken, die bei der Berechnung berücksichtigt werden. |
| Variablenwert | Für Berechnungen verwendete Telemetriewerte, die aus Geräten oder Sensoren stammen. Kategorische Typ-Variablen müssen entweder `Long` oder `String` sein, um dem Datentyp der eingehenden Daten zu entsprechen. |
| Variableninterpolation | Die Interpolation gibt an, wie ein Signal anhand von vorhandenen Daten rekonstruiert wird. Die Option *Schrittinterpolation* ist für kategorische Variablen verfügbar. |
| Variablenkategorien | Kategorien sorgen für die Zuordnung zwischen den aus einem Gerät oder Sensor stammenden Werten und einer Bezeichnung. |
| Standardkategorie für Variablen | Die Standardkategorie steht für alle Werte zur Verfügung, die nicht in der Eigenschaft „Kategorien“ zugeordnet werden. |

Variablen entsprechen dem folgenden JSON-Beispiel:

```JSON
"Status": {
  "kind": "categorical",
  "value": {
     "tsx": "$event.Status.Long"
},
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span" : "PT1M"
    }
  },
  "categories": [
    {
      "values": [0, 1, 2, 3],
      "label": "Good"
    },
    {
      "values": [4],
      "label": "Bad"
    }
  ],
  "defaultCategory": {
    "label": "Not Applicable"
  }
}
```

## <a name="aggregate-variables"></a>Aggregieren von Variablen

| Variableneigenschaft | BESCHREIBUNG |
| --- | ---|
| Variablenfilter | Filter sind optionale Bedingungsklauseln, um die Anzahl von Zeilen zu beschränken, die bei der Berechnung berücksichtigt werden. |
| Variablenaggregation | Führen Sie Berechnungen über die unterstützten [Aggregationsfunktionen für aggregierte Variablen](/rest/api/time-series-insights/reference-time-series-expression-syntax#aggregate-variable-kind) durch. |

Variablen entsprechen dem folgenden JSON-Beispiel:

```JSON
"Speed Range": {
  "kind": "aggregate",
  "filter": null,
  "aggregation": {
    "tsx": "max($event.Speed.Double) - min($event.Speed.Double)"
  }
}
```

Variablen werden in der Typdefinition eines Zeitreihenmodells gespeichert und können inline über APIs angegeben werden, um die gespeicherte Definition zu überschreiben.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Zeitreihenmodelle](./concepts-model-overview.md).

* Erfahren Sie mehr darüber, wie Sie mithilfe von [Abfrage-APIs](./concepts-query-overview.md) Variablen inline definieren.
