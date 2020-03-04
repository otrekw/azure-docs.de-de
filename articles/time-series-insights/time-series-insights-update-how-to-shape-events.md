---
title: Strukturieren von Ereignissen – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie mehr über bewährte Methoden und das Formen von Ereignissen für Abfragen in der Vorschauversion von Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/24/2020
ms.custom: seodec18
ms.openlocfilehash: 99a2f32c3f76d7fec475c9b299f7208b4db29cfe
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650922"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Strukturieren von Ereignissen in Azure Time Series Insights Preview

In diesem Artikel werden bewährte Methoden beim Strukturieren Ihrer JSON-Nutzlasten für die Erfassung in Azure Time Series Insights und zum Maximieren der Effizienz Ihrer Preview-Abfragen vorgestellt.

## <a name="best-practices"></a>Bewährte Methoden

Sie sollten sorgfältig abwägen, wie Sie Ereignisse an Ihre Time Series Insights Preview-Umgebung senden. 

Allgemeine bewährte Methoden:

* Senden Sie Daten so effizient wie möglich über das Netzwerk.
* Speichern Sie Ihre Daten in einer Weise, die Ihnen dabei hilft, sie für Ihr Szenario besser geeignet zu aggregieren.

Für die beste Abfrageleistung befolgen Sie die folgenden Faustregeln:

* Senden Sie keine unnötigen Eigenschaften. Die Abrechnung von Time Series Insights Preview erfolgt nach Nutzung. Es wird empfohlen, nur die Daten, die Sie abfragen möchten, zu speichern und zu verarbeiten.
* Verwenden Sie Instanzenfelder für statische Daten. Diese Vorgehensweise hilft dabei, das Senden von statischen Daten über das Netzwerk zu vermeiden. Instanzenfelder, eine Komponente das Zeitreihenmodells, funktionieren wie Verweisdaten im Time Series Insights-Dienst, der allgemein verfügbar ist. Weitere Informationen zu Instanzenfeldern finden Sie unter [Zeitreihenmodell](./time-series-insights-update-tsm.md).
* Teilen Sie Dimensionseigenschaften zwischen zwei oder mehr Ereignissen. Diese Vorgehensweise hilft Ihnen dabei, Daten effizienter über das Netzwerk zu senden.
* Verwenden Sie keine tiefe Arrayschachtelung. Time Series Insights Preview unterstützt bis zu zwei Ebenen für geschachtelte Arrays, die Objekte enthalten. Time Series Insights Preview sorgt für flache Arrays in Nachrichten, indem eine Aufteilung in mehrere Ereignisse mit Eigenschaft-Wert-Paaren durchgeführt wird.
* Wenn nur wenige Messwerte für viele oder alle Ereignisse vorhanden sind, ist es besser, diese Messwerte als separate Eigenschaften innerhalb desselben Objekts zu senden. Durch das separate Senden wird die Anzahl von Ereignissen verringert und die Abfrageleistung möglicherweise gesteigert, weil weniger Ereignisse verarbeitet werden müssen.

## <a name="column-flattening"></a>Spaltenvereinfachung

Während der Erfassung werden Nutzlasten, die geschachtelte Objekte enthalten, vereinfacht, sodass der Spaltenname ein Einzelwert mit einer Abgrenzung ist.

* Nehmen Sie als Beispiel den folgenden geschachtelten JSON-Code:

   ```JSON
   "data": {
        "flow": 1.0172575712203979,
   },
   ```

   Wird zu: `data_flow` bei Vereinfachung.

> [!IMPORTANT]
> * Azure Time Series Insights Preview verwendet Unterstriche (`_`) zur Spaltenabgrenzung.
> * Beachten Sie den Unterschied zur allgemeinen Verfügbarkeit, bei der stattdessen Punkte (`.`) verwendet werden.

Im Folgenden finden Sie komplexere Szenarien.

#### <a name="example-1"></a>Beispiel 1:

Das folgende Szenario verfügt über zwei (oder mehr) Geräte, die die Messungen (Signale) senden: *Flow Rate* (Flussrate), *Engine Oil Pressure* (Öldruck), *Temperature* und *Humidity* (Feuchtigkeit).

Es wird eine einzelne Azure IoT Hub-Nachricht gesendet, bei der das äußere Array einen gemeinsam verwendeten Abschnitt für allgemeine Dimensionswerte enthält (beachten Sie die beiden Geräteeinträge in der Nachricht).

```JSON
[
  {
    "deviceId":"FXXX",
    "timestamp":"2018-01-17T01:17:00Z",
    "series":[
      {
        "Flow Rate ft3/s":1.0172575712203979,
        "Engine Oil Pressure psi ":34.7
      },
      {
        "Flow Rate ft3/s":2.445906400680542,
        "Engine Oil Pressure psi ":49.2
      }
    ]
  },
  {
    "deviceId":"FYYY",
    "timestamp":"2018-01-17T01:18:00Z",
    "series":[
      {
        "Flow Rate ft3/s":0.58015072345733643,
        "Engine Oil Pressure psi ":22.2
      }
    ]
  }
]
```

**Wesentliche Punkte:**

* Der JSON-Beispielcode enthält ein äußeres Array, das [Zeitreiheninstanzdaten](./time-series-insights-update-tsm.md#time-series-model-instances) verwendet, um die Effizienz der Nachricht zu erhöhen. Obwohl sich die Gerätemetadaten der Zeitreiheninstanzen wahrscheinlich nicht ändern, bieten sie häufig nützliche Eigenschaften für die Datenanalyse.

* Der JSON-Code kombiniert zwei oder mehr Nachrichten (eine von jedem Gerät) zu einer einzelnen Nutzlast und spart so im Lauf der Zeit Bandbreite.

* Einzelne Datenreihenpunkte werden für jedes Gerät in einem einzelnen **Reihenattribut** zusammengefasst, um die Notwendigkeit fortlaufender Streamingupdates für jedes Gerät zu verringern.

> [!TIP]
> Um die Anzahl der Nachrichten zu verringern, die zum Senden von Daten erforderlich sind, und die Telemetriedatenerfassung effizienter zu machen, sollten Sie das Zusammenfassen allgemeiner Dimensionswerte und Metadaten von Zeitreiheninstanzen in einer einzigen JSON-Nutzlast in Erwägung ziehen.

#### <a name="time-series-instance"></a>Zeitreiheninstanz 

Im Folgenden finden Sie ausführlichere Informationen zum Verwenden von [Zeitreiheninstanzen](./time-series-insights-update-tsm.md#time-series-model-instances) zum optimalen Strukturieren Ihrer JSON-Nutzlasten. 

> [!NOTE]
> Die [Zeitreihen-IDs](./time-series-insights-update-how-to-id.md) unten sind *Geräte-IDs*.

```JSON
[
  {
    "timeSeriesId":[
      "FXXX"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"REVOLT SIMULATOR",
      "L2":"Battery System"
    }
  },
  {
    "timeSeriesId":[
      "FYYY"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"COMMON SIMULATOR",
      "L2":"Battery System"
    }
  }
]
```

Time Series Insights Preview verknüpft eine Tabelle (nach deren Vereinfachung) während der Abfragezeit. Die Tabelle enthält zusätzliche Spalten, z. B. **Typ**.

| deviceId  | type | L1 | L2 | timestamp | series_Flow Rate ft3/s | series_Engine Oil Pressure psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMULATOR | Battery System | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34.7 |
| `FXXX` | Default_Type | SIMULATOR |   Battery System |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA    COMMON | SIMULATOR |    Battery System |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

> [!NOTE]
>  Die obige Tabelle stellt die Abfrageansicht im [Time Series Preview-Explorer](./time-series-insights-update-explorer.md) dar.

**Wesentliche Punkte:**

* Im vorherigen Beispiel werden statische Eigenschaften in Time Series Insights Preview gespeichert, um über das Netzwerk gesendete Daten zu optimieren.
* Time Series Insights Preview-Daten werden zur Abfragezeit verknüpft, indem die in der Instanz definierte Time Series-ID verwendet wird.
* Es werden zwei Ebenen der Schachtelung verwendet. Diese Anzahl ist die höchste, die von Time Series Insights Preview unterstützt wird. Tief verschachtelte Arrays müssen vermieden werden.
* Da nur wenige Messwerte vorliegen, werden sie als separate Eigenschaften innerhalb desselben Objekts gesendet. In dem Beispiel sind **series_Flow Rate psi**, **series_Engine Oil Pressure psi** und **series_Flow Rate ft3/s** eindeutige Spalten.

>[!IMPORTANT]
> Instanzenfelder werden nicht mit Telemetriedaten gespeichert. Sie werden mit Metadaten im Zeitreihenmodell gespeichert.

#### <a name="example-2"></a>Beispiel 2:

Betrachten Sie das folgende JSON-Beispiel:

```JSON
{
  "deviceId": "FXXX",
  "timestamp": "2019-01-18T01:17:00Z",
  "data": {
        "flow": 1.0172575712203979,
    },
  "data_flow" : 1.76435072345733643
}
```

Im obigen Beispiel würde die vereinfachte `data["flow"]`-Eigenschaft einen Namenskonflikt mit der `data_flow`-Eigenschaft darstellen.

In diesem Fall würde der *aktuellste* Eigenschaftswert den vorherigen überschreiben. 

> [!TIP]
> Wenden Sie sich für weitere Unterstützung an das Time Series Insights-Team.

> [!WARNING] 
> * In Fällen, in denen doppelte Eigenschaften in derselben Ereignisnutzlast (singulär) aufgrund der Vereinfachung oder eines anderen Mechanismus vorhanden sind, wird der neueste Eigenschaftswert gespeichert, der alle vorherigen Werte überschreibt.
> * Reihen von kombinierten Ereignissen setzen einander nicht außer Kraft.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Umsetzen dieser Richtlinien finden Sie unter [Azure Time Series Insights Preview-Abfragesyntax](./time-series-insights-query-data-csharp.md). Dort erfahren Sie mehr über die Abfragesyntax der [REST-API](https://docs.microsoft.com/rest/api/time-series-insights/preview) in Time Series Insights Preview für den Datenzugriff.

* Nutzen Sie die bewährten Methoden für JSON mit den Informationen unter [Datenmodellierung in Azure Time Series Insights Preview](./time-series-insights-update-how-to-tsm.md).
