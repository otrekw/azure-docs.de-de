---
title: Datenabfragen in der Vorschau – Azure Time Series Insights | Microsoft-Dokumentation
description: Konzepte für die Datenabfrage und Übersicht über die HTTP-REST-API in Azure Time Series Insights Preview.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 03/25/2020
ms.custom: seodec18
ms.openlocfilehash: 23094ec71dac5780def10e16b90de0b818ef3c68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284890"
---
# <a name="data-querying-in-azure-time-series-insights-preview"></a>Datenabfragen in Azure Time Series Insights Preview

Azure Time Series Insights ermöglicht das Abfragen von Daten für Ereignisse und Metadaten, die in der Umgebung gespeichert sind, über öffentliche Oberflächen-APIs. Diese APIs werden auch im [Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer) verwendet.

Drei primäre API-Kategorien sind in Time Series Insights verfügbar:

* **Umgebungs-APIs**: Diese APIs ermöglichen Abfragen der Time Series Insights-Umgebung selbst. Sie ermöglichen das Zusammenstellen der Liste der Umgebungen, auf die der Aufrufer Zugriff hat, sowie von Umgebungsmetadaten.
* **Zeitreihenmodellabfrage (TSM-Q)-APIs**: Ermöglicht die Vorgänge Erstellen, Lesen, Aktualisieren und Löschen (Create, Read, Update, Delete, CRUD) mit Metadaten, die im Zeitreihenmodell der Umgebung gespeichert sind. Diese können verwendet werden, um auf die Instanzen, Typen und Hierarchien zuzugreifen und sie zu bearbeiten.
* **Zeitreihenabfrage (TSQ)-APIs**: Ermöglicht das Abrufen von Telemetrie- oder Ereignisdaten, wie sie vom Quellanbieter aufgezeichnet werden, sowie leistungsstarke Berechnungen und Aggregationen der Daten mithilfe von erweiterten Skalar- und Aggregatfunktionen.

Time Series Insights verwendet eine umfassende zeichenfolgenbasierte Ausdruckssprache, [Zeitreihenausdrucksprache (Time Series Expression, TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax), um Berechnungen auszudrücken.

## <a name="azure-time-series-insights-core-apis"></a>Azure Time Series Insights-Kern-APIs

Die folgenden Kern-APIs werden unterstützt.

[![Zeitreihenabfrage (TSQ): Übersicht](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Umgebungs-APIs

* [„Umgebungen abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/management/environments/get): Gibt die Liste der Umgebungen zurück, auf die der Benutzer zum Zugriff autorisiert ist.
* [„Umgebungsverfügbarkeit abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability): Gibt die Verteilung der Anzahl von Ereignissen über den Ereigniszeitstempel `$ts` zurück. Diese API hilft dabei, zu ermitteln, ob Ereignisse in der Umgebung vorhanden sind, indem die Anzahl von Ereignissen (sofern vorhanden) unterteilt in Zeitintervalle zurückgegeben wird.
* [„Ereignisschema abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema): Gibt die Ereignisschema-Metadaten für einen bestimmten Suchzeitraum zurück. Diese API hilft beim Abrufen aller Metadaten und Eigenschaften, die im Schema für den angegebenen Suchzeitraum zur Verfügung stehen.

## <a name="time-series-model-query-tsm-q-apis"></a>Zeitreihenmodellabfrage (TSM-Q)-APIs

Die meisten dieser APIs unterstützen die Batchausführung, um Batch-CRUD-Vorgänge für mehrere Entitäten von Zeitreihenmodellen zu ermöglichen:

* [„Modelleinstellungen“-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#model-settings-api): Ermöglicht *GET* und *PATCH* für den Standardtyp und den Modellnamen der Umgebung.
* [Typen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#types-api): Ermöglicht CRUD mit Zeitreihentypen und deren zugeordneten Variablen.
* [Hierarchien-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#hierarchies-api): Ermöglicht CRUD mit Zeitreihenhierarchien und deren zugeordneten Feldpfaden.
* [Instanzen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#instances-api): Ermöglicht CRUD mit Zeitreiheninstanzen und deren zugeordneten Instanzenfeldern. Zusätzlich unterstützt die Instanzen-API die folgenden Vorgänge:
  * [Suchen](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/search): Ruft eine unvollständige Liste der Treffer bei der Suche nach Zeitreiheninstanzen basierend auf Instanzattributen ab.
  * [Vorschlagen](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/suggest): Sucht eine unvollständige Liste der Treffer bei der Suche nach Zeitreiheninstanzen basierend auf Instanzattributen und schlägt sie vor.

## <a name="time-series-query-tsq-apis"></a>Zeitreihenabfrage (TSQ)-APIs

Diese APIs sind in beiden Speichern unserer mehrschichtigen Speicherlösung in Time Series Insights verfügbar. Parameter zum Abfragen der URL werden verwendet, um den [Speichertyp](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters) anzugeben, für den die Abfrage ausgeführt werden soll:

* [„Ereignisse abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents): Ermöglicht das Abfragen und Abrufen von unformatierten Ereignissen und den zugehörigen Zeitstempeln, wie sie in Time Series Insights vom Quellanbieter aufgezeichnet werden. Diese API ermöglicht das Abrufen von Rohereignissen für eine bestimmte Zeitreihen-ID und Suchzeitspanne. Diese API unterstützt die Paginierung, um das vollständige Antwortdataset für die ausgewählte Eingabe abzurufen. 

* [„Reihe abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries): Ermöglicht das Abfragen und Abrufen berechneter Werte und der zugehörigen Ereigniszeitstempel durch Anwenden von Berechnungen, die durch Variablen in unformatierten Ereignissen definiert werden. Diese Variablen können im Zeitreihenmodell definiert oder in der Abfrage bereitgestellt werden. Diese API unterstützt die Paginierung, um das vollständige Antwortdataset für die ausgewählte Eingabe abzurufen. 

* [„Reihe aggregieren“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries): Ermöglicht das Abfragen und Abrufen aggregierter Werte und der zugehörigen Intervallzeitstempel durch Anwenden von Berechnungen, die durch Variablen in unformatierten Ereignissen definiert werden. Diese Variablen können im Zeitreihenmodell definiert oder in der Abfrage bereitgestellt werden. Diese API unterstützt die Paginierung, um das vollständige Antwortdataset für die ausgewählte Eingabe abzurufen. 
  
  Diese API gibt für eine angegebene Suchzeitspanne und ein angegebenes Intervall eine aggregierte Antwort pro Variable pro Intervall für eine Zeitreihen-ID zurück. Die Anzahl der Intervalle im Antwortdataset wird durch Zählen der Epochenticks (Anzahl der Millisekunden seit Beginn der Unix-Epoche am 1. Januar 1970) und die Division der Ticks durch die in der Abfrage angegebene Intervallzeitspanne berechnet.

  Die im Antwortdataset zurückgegebenen Zeitstempel stellen die linken Intervallgrenzen dar, nicht die Stichprobenereignisse im Intervall. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über verschiedene Variablen, die im [Zeitreihenmodell](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-tsm) definiert werden können.
- Erfahren Sie mehr über das Abfragen von Daten im [Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer).
