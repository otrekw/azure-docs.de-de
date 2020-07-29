---
title: Bevorstehende Änderungen an den Erfassungs- und Vereinfachungsregeln in Azure Time Series Insights Gen2 | Microsoft-Dokumentation
description: Änderungen an den Erfassungsregeln
ms.service: time-series-insights
services: time-series-insights
author: lyrana
ms.author: lyhughes
manager: dpalled
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: lyhughes
ms.openlocfilehash: f667ca5ad82182fcf40d5c1fbb325f2ea99a7e08
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86495107"
---
# <a name="upcoming-changes-to-the-json-flattening-and-escaping-rules-for-new-environments"></a>Bevorstehende Änderungen an den JSON-Vereinfachungs- und -Escaperegeln für neue Umgebungen

**Diese Änderungen werden nur auf *neu erstellte* Azure Time Series Insights Gen2-Umgebungen angewandt. Sie gelten nicht für Gen1-Umgebungen.**

Ihre Azure Time Series Insights Gen2-Umgebung erstellt die Speicherspalten dynamisch unter Berücksichtigung bestimmter Namenskonventionen. Wenn ein Ereignis erfasst wird, werden bestimmte Regeln auf die Namen der JSON-Nutzlast und JSON-Eigenschaften angewendet. Ab Juli 2020 gelten für neue Azure Time Series Insights Gen2-Instanzen Änderungen an der Art und Weise, wie JSON-Daten vereinfacht und gespeichert werden. Dies betrifft Sie in den folgenden Fällen:

* Wenn Ihre JSON-Nutzlast geschachtelte Objekte enthält
*  Wenn Ihre JSON-Nutzlast Arrays enthält
*  Wenn Sie eines der folgenden vier Sonderzeichen in einem JSON-Eigenschaftsnamen verwenden: „[“, „\“, „.“ oder '
*  Wenn eine oder mehrere Ihrer TS-ID-Eigenschaften in einem geschachtelten Objekt enthalten sind

Wenn Sie eine neue Umgebung erstellen und mindestens einer der oben genannten Fälle auf Ihre Ereignisnutzlast zutrifft, werden Ihre Daten anders vereinfacht und gespeichert. Zusammenfassung der Änderungen:

| Aktuelle Regel | Neue Regel | JSON-Beispiel | Vorheriger Spaltenname | Neuer Spaltenname
|---|---| ---| ---|  ---|
| Geschachtelter JSON-Code wird mithilfe eines Unterstrichs als Abgrenzung vereinfacht. |Geschachtelter JSON-Code wird mithilfe eines Punkts als Abgrenzung vereinfacht.  | ``{"series" : { "value" : 19.338 }}`` | `series_value_double` |`series.value_double` |
| Keine Escapezeichen bei Sonderzeichen | Für JSON-Eigenschaftsnamen, die die Sonderzeichen „.“, „[“, „\“ oder „'“ enthalten, werden „['“ und „']“ als Escapezeichen verwendet. Zwischen „['“ und „']“ werden bei einfachen Anführungszeichen und umgekehrten Schrägstrichen ebenfalls Escapezeichen verwendet. Ein einfaches Anführungszeichen wird als „\'“ geschrieben und ein umgekehrter Schrägstrich als „\\\“.  | ```"Foo's Law Value": "17.139999389648"``` | `Foo's Law Value_double` | `['Foo\'s Law Value']_double` | 
| Arrays von primitiven Typen werden als Zeichenfolgen gespeichert. | Arrays von primitiven Typen werden als dynamische Typen gespeichert.  | `"values": [154, 149, 147]` | `values_string`  | `values_dynamic` |
Arrays von Objekten werden immer vereinfacht und mehrere Ereignisse generiert. | Wenn die Objekte in einem Array weder über die TS-ID noch über die Zeitstempeleigenschaft verfügen, wird das gesamte Array von Objekten als dynamischer Typ gespeichert. | `"values": [{"foo" : 140}, {"bar" : 149}]` | `values_foo_long | values_bar_long` | `values_dynamic` |

## <a name="recommended-changes-for-new-environments"></a>Empfohlene Änderungen für neue Umgebungen

#### <a name="if-your-ts-id-andor-timestamp-property-is-nested-within-an-object"></a>Wenn die TS-ID und/oder die Zeitstempeleigenschaft innerhalb eines Objekts geschachtelt ist:

*  Alle neuen Bereitstellungen müssen den neuen Erfassungsregeln entsprechen. Wenn Ihre TS-ID z. B. `telemetry_tagId` ist, müssen Sie alle Resource Manager-Vorlagen oder automatisierten Bereitstellungsskripts so aktualisieren, dass `telemetry.tagId` als TS-ID für die Umgebung konfiguriert ist. Diese Änderung ist auch bei Zeitstempeln für Ereignisquellen in geschachteltem JSON-Code erforderlich.

 #### <a name="if-your-payload-contains-nested-json-or-special-characters-and-you-automate-authoring-time-series-model-variable-expressions"></a>Wenn Ihre Nutzlast geschachtelten JSON-Code oder Sonderzeichen enthält, und Sie die Erstellung von variablen Ausdrücken für das [Zeitreihenmodell](.\time-series-insights-update-tsm.md) automatisieren

*  Aktualisieren Sie den Clientcode, indem Sie [TypesBatchPut](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch#typesbatchput) ausführen, um ihn an die neuen Erfassungsregeln anzupassen. Beispielsweise sollte ein bisheriger [Zeitreihenausdruck](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) von `"value": {"tsx": "$event.series_value.Double"}` auf eine der folgenden Optionen aktualisiert werden:
    * `"value": {"tsx": "$event.series.value.Double"}`
    * `"value": {"tsx": "$event['series']['value'].Double"}`


## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den Artikel [Datenspeicherung und -eingang in Azure Time Series Insights Gen2](./time-series-insights-update-storage-ingress.md).

- Erfahren Sie mehr über das Abfragen von Daten mit [Time Series-Abfrage-APIs](./concepts-query-overview.md).

- Erfahren Sie mehr über die [neue Time Series-Ausdruckssyntax](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax).

