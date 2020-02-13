---
title: Datenabfragen in der Vorschau – Azure Time Series Insights | Microsoft-Dokumentation
description: Konzepte für die Datenabfrage und Übersicht über die HTTP-REST-API in Azure Time Series Insights Preview.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 898515f49672a19ed8bf1c62439128b6727afc73
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2020
ms.locfileid: "77087409"
---
# <a name="data-querying-in-azure-time-series-insights-preview"></a>Datenabfragen in Azure Time Series Insights Preview

Azure Time Series Insights Preview ermöglicht das Abfragen von Daten für Ereignisse und Metadaten, die in der Umgebung gespeichert sind, über öffentliche Oberflächen-APIs. Diese APIs werden auch im [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) verwendet.

Drei primäre API-Kategorien sind in Time Series Insights verfügbar:

* **Umgebungs-APIs**: Diese APIs ermöglichen Abfragen der Time Series Insights-Umgebung selbst. Beispiele für Abfragen sind die Liste der Umgebungen, auf die der Aufrufer Zugriff hat, sowie Umgebungsmetadaten.
* **Zeitreihenmodellabfrage (TSM-Q)-APIs**: Ermöglicht das Erstellen, Lesen, Aktualisieren und Löschen (CRUD) von Vorgängen mit Metadaten, die im Umgebungsteil des Zeitreihenmodells gespeichert sind. Beispiele sind Instanzen, Typen und Hierarchien.
* **Zeitreihenabfrage (TSQ)-APIs**: Ermöglicht das Abrufen von Telemetrie- oder Ereignisdaten, wie sie vom Quellanbieter aufgezeichnet werden, oder durch Reduzieren der Daten mithilfe von Skalar- und Aggregatfunktionen, die in einem Teil der Variablen gespeichert sind. Diese APIs können Vorgänge zum Transformieren und Kombinieren sowie Berechnungen auf Zeitreihendaten anwenden.

Time Series Insights verwendet eine umfassende zeichenfolgenbasierte Ausdruckssprache, [Zeitreihenausdrucksprache (Time Series Expression, TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx), um Berechnungen auszudrücken.

## <a name="azure-time-series-insights-preview-core-apis"></a>Kern-APIs von Azure Time Series Insights Preview

Die folgenden Kern-APIs werden unterstützt.

[![Zeitreihenabfrage (TSQ): Übersicht](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Umgebungs-APIs

Die folgenden Umgebungs-APIs sind verfügbar:

* [„Umgebungen abrufen“-API](/rest/api/time-series-insights/management/environments/get): Gibt die Liste der Umgebungen zurück, auf die der Benutzer zum Zugriff autorisiert ist.
* [„Umgebungsverfügbarkeit abrufen“-API](/rest/api/time-series-insights/dataaccess(preview)/query/getavailability): Gibt die Verteilung der Anzahl von Ereignissen über den Ereigniszeitstempel `$ts` zurück. Diese API hilft dabei zu bestimmen, ob Ereignisse in dem Zeitstempel vorhanden sind, indem die Anzahl von Ereignissen, sofern welche vorhanden sind, zurückgegeben wird.
* [„Ereignisschema abrufen“-API](/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema): Gibt die Ereignisschema-Metadaten für einen bestimmten Suchzeitraum zurück. Diese API hilft beim Abrufen aller Metadaten und Eigenschaften, die im Schema für den angegebenen Suchzeitraum zur Verfügung stehen.

## <a name="time-series-model-query-tsm-q-apis"></a>Zeitreihenmodellabfrage (TSM-Q)-APIs

Die folgenden Zeitreihenmodellabfrage-APIs sind verfügbar. Die meisten dieser APIs unterstützen die Batchausführung, um Batch-CRUD-Vorgänge für mehrere Zeitreihenmodellentitäten zu ermöglichen:

* [„Modelleinstellungen“-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): Ermöglicht *GET* und *PATCH* für den Standardtyp und den Modellnamen der Umgebung.
* [Typen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): Ermöglicht CRUD mit Zeitreihentypen und deren zugeordneten Variablen.
* [Hierarchien-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): Ermöglicht CRUD mit Zeitreihenhierarchien und deren zugeordneten Feldpfaden.
* [Instanzen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): Ermöglicht CRUD mit Zeitreiheninstanzen und deren zugeordneten Instanzenfeldern. Zusätzlich unterstützt die Instanzen-API die folgenden Vorgänge:
  * [Suchen](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/search): Ruft eine unvollständige Liste der Treffer bei der Suche nach Zeitreiheninstanzen basierend auf Instanzattributen ab.
  * [Vorschlagen](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/suggest): Sucht eine unvollständige Liste der Treffer bei der Suche nach Zeitreiheninstanzen basierend auf Instanzattributen und schlägt sie vor.

## <a name="time-series-query-tsq-apis"></a>Zeitreihenabfrage (TSQ)-APIs

Die folgenden Zeitreihenabfrage-APIs sind verfügbar. Diese APIs sind in allen unterstützten mehrschichtigen Speichern in Time Series Insights verfügbar. Parameter zum Abfragen der URL werden verwendet, um den [Speichertyp](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters) anzugeben, für den die Abfrage ausgeführt werden soll:

* [„Ereignisse abrufen“-API](/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents): Ermöglicht das Abfragen und Abrufen von Time Series Insights-Daten aus Ereignissen, während sie in Time Series Insights vom Quellenanbieter aufgezeichnet werden. Diese API ermöglicht das Abrufen von Rohereignissen für eine bestimmte Zeitreihen-ID und Suchzeitspanne. Diese API unterstützt die Paginierung, um das komplette Dataset für die ausgewählte Eingabe abzurufen. 

* [„Reihe abrufen“-API](/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries): Ermöglicht das Abfragen und Abrufen von Time Series Insights-Daten aus erfassten Ereignissen, indem bei der Übertragung aufgezeichnete Daten verwendet werden. Die zurückgegebenen Werte, basieren auf den Variablen, die im Modell definiert oder inline angegeben wurden. Diese API unterstützt die Paginierung, um das komplette Dataset für die ausgewählte Eingabe abzurufen. Diese API hilft bei der Definition von berechneten Eigenschaften oder Spalten.

    >[!NOTE]
    > Die Aggregationsklausel wird ignoriert, auch wenn sie in einem Modell oder inline angegeben wurde.

  Die „Reihe abrufen“-API gibt einen Zeitreihenwert für jede Variable in jedem Intervall zurück. Ein Zeitreihenwert ist ein Format, das Time Series Insights für die JSON-Ausgabe aus einer Abfrage verwendet. Die zurückgegebenen Werte, basieren auf der Time Series-ID und dem Satz von Variablen, die bereitgestellt wurden.

* [„Reihe aggregieren“-API](/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregatevariable): Ermöglicht das Abfragen und Abrufen von Time Series Insights-Daten aus erfassten Ereignissen, indem Stichproben aus aufgezeichneten Daten entnommen und diese aggregiert werden. Diese API unterstützt die kontinuierliche Ausführung durch die Verwendung von [Fortsetzungstoken](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#queryresultpage).

  Die „Reihe aggregieren“-API gibt einen Zeitreihenwert für jede Variable in jedem Intervall zurück. Die Werte basieren auf der Time Series-ID und dem Satz von Variablen, die bereitgestellt wurden. Die „Reihe aggregieren“-API erreicht eine Verringerung unter Verwendung der Variablen, die im Zeitreihenmodell gespeichert sind oder inline bereitgestellt werden, um Daten zu aggregieren oder daraus Stichproben zu entnehmen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Datenspeicherung und Dateneingang](./time-series-insights-update-storage-ingress.md) in Azure Time Series Insights Preview.
- Lesen Sie den Artikel über [Datenmodellierung](./time-series-insights-update-tsm.md) in Time Series Insights Preview.
- Untersuchen Sie [Bewährte Methoden für die Auswahl einer Zeitreihen-ID](./time-series-insights-update-how-to-id.md).
