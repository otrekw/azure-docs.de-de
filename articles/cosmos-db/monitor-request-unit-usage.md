---
title: Überwachen der Durchsatznutzung eines Vorgangs in Azure Cosmos DB
description: Es wird beschrieben, wie Sie für einen Vorgang die Nutzung des Durchsatzes oder der Anforderungseinheiten in Azure Cosmos DB überwachen. Besitzer eines Azure Cosmos DB-Kontos können so ermitteln, für welche Vorgänge mehr Anforderungseinheiten verbraucht werden.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 04/09/2020
ms.openlocfilehash: 4ea195dfbf9eddbf10680196c9c75646b45380ae
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83119002"
---
# <a name="how-to-monitor-throughput-or-request-unit-usage-of-an-operation-in-azure-cosmos-db"></a>Überwachen der Nutzung des Durchsatzes oder der Anforderungseinheiten für einen Vorgang in Azure Cosmos DB

Azure Monitor für Azure Cosmos DB verfügt über eine Metrikansicht zum Überwachen Ihres Kontos und zum Erstellen von Dashboards. Weil die Azure Cosmos DB-Metriken standardmäßig erfasst werden, müssen Sie bei diesem Feature nichts explizit aktivieren oder konfigurieren. Die Metrik **Anforderungseinheiten gesamt** wird verwendet, um die Nutzung von Anforderungseinheiten für unterschiedliche Arten von Vorgängen abzurufen. Später können Sie analysieren, für welche Vorgänge die größte Durchsatzmenge genutzt wurde. Standardmäßig werden die Durchsatzdaten mit einem Intervall von einer Minute aggregiert. Sie können die Aggregationseinheit aber ändern, indem Sie die Option für die Zeitgranularität anpassen.

Es gibt zwei Möglichkeiten, die Daten zur Nutzung von Anforderungseinheiten zu analysieren:

* Überprüfen, für welche Vorgänge innerhalb des angegebenen Zeitintervalls mehr Anforderungseinheiten verbraucht werden
* Überprüfen, welche Vorgänge generell für Ihre Workload vorherrschend sind, indem sie mehr Anforderungseinheiten verbrauchen
Bei dieser Analyse können Sie Funktionen für „Einfügen“ und „Upsert“ nutzen und sich die Indizierung ansehen. Sie können ermitteln, ob die Indizierung für bestimmte Felder zu hoch oder zu niedrig ist, und die [Indizierungsrichtlinie](index-policy.md#include-exclude-paths) ändern, um Pfade ein- oder auszuschließen.

Falls Sie merken, dass für bestimmte Abfragen mehr Anforderungseinheiten verbraucht werden, stehen Ihnen folgende Aktionen zur Verfügung:

* Überprüfen, ob Sie die richtige Menge an Daten anfordern
* Umstellen der Abfrage auf die Nutzung des Index mit Filterklausel
* Durchführen von kostengünstigeren UDF-Funktionsaufrufen
* Definieren von Partitionsschlüsseln zum Minimieren der Verteilung einer Abfrage auf unterschiedliche Partitionen
* Sie können auch die in der Aufrufantwort zurückgegebenen Metriken und die Details des Diagnoseprotokolls verwenden und den Artikel zur [Optimierung der Abfrageleistung](sql-api-query-metrics.md) lesen, um mehr zur Ausführung von Abfragen zu erfahren.
* Sie können mit der Summe beginnen und sich dann die durchschnittliche Nutzung ansehen, indem Sie die entsprechende Dimension verwenden.

## <a name="view-the-total-request-unit-usage-metric"></a>Anzeigen der Metrik für die Gesamtnutzung von Anforderungseinheiten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie in der Navigationsleiste auf der linken Seite die Option **Monitor** und dann **Metrik** aus.

   ![Bereich „Metriken“ in Azure Monitor](./media/monitor-request-unit-usage/monitor-metrics-blade.png)

1. Klicken Sie im Bereich **Metriken** auf **Ressource auswählen**. Wählen Sie dann das erforderliche **Abonnement** und die **Ressourcengruppe** aus. Wählen Sie unter **Ressourcentyp** die Option **Azure Cosmos DB-Konten** aus. Wählen Sie dann eins der vorhandenen Azure Cosmos-Konten und anschließend **Anwenden** aus.

   ![Auswählen des Azure Cosmos DB-Kontos zum Anzeigen von Metriken](./media/monitor-request-unit-usage/select-cosmos-db-account.png)

1. Wählen Sie als Nächstes in der Liste mit den verfügbaren Metriken die Metrik **Anforderungseinheiten gesamt** aus. Ausführliche Informationen zu allen verfügbaren Metriken in dieser Liste finden Sie im Artikel [Metriken nach Kategorie](monitor-cosmos-db-reference.md). In diesem Beispiel wählen Sie **Anforderungseinheiten gesamt** und als Aggregationswert **Mittelw.** aus. Zusätzlich zu diesen Angaben können Sie auch **Zeitbereich** und **Zeitgranularität** für die Metriken auswählen. Sie können Metriken maximal für die letzten 30 Tage anzeigen.  Nach Anwendung des Filters wird ein darauf basierendes Diagramm angezeigt. Sie sehen die durchschnittliche Anzahl der pro Minute verbrauchten Anforderungseinheiten für den ausgewählten Zeitraum.  

   ![Auswählen einer Metrik über das Azure-Portal](./media/monitor-request-unit-usage/request-unit-usage-metric.png)

## <a name="filters-for-request-unit-usage"></a>Filter für die Nutzung von Anforderungseinheiten

Sie können Metriken und die Diagrammanzeige auch nach bestimmten Werten für **CollectionName**, **DatabaseName**, **OperationType**, **Region**, **Status** und **StatusCode** filtern. Mit den Optionen **Filter hinzufügen** und **Teilung anwenden** können Sie die Nutzung von Anforderungseinheiten filtern und die Metriken gruppieren.

Wählen Sie zum Abrufen der Nutzung von Anforderungseinheiten für die einzelnen Vorgänge („total(sum)“ oder „average“) die Option **Teilung anwenden** und dann **Vorgangsart** und den Filterwert aus. Dies ist in der folgenden Abbildung dargestellt:

   ![Cosmos DB-Anforderungseinheiten für Vorgänge in Azure Monitor](./media/monitor-request-unit-usage/request-unit-usage-operations.png)

Wenn Sie die Nutzung der Anforderungseinheiten nach Sammlung anzeigen möchten, wählen Sie die Option **Teilung anwenden** und den Namen der Sammlung als Filter aus. Im Dashboard werden ein Chat und einige Sammlungen angezeigt (siehe Abbildung unten). Sie können dann den Namen einer Sammlung auswählen, um weitere Details dazu anzuzeigen:

   ![Cosmos DB-Anforderungseinheiten für alle Vorgänge nach Sammlung in Azure Monitor](./media/monitor-request-unit-usage/request-unit-usage-collection.png)

## <a name="next-steps"></a>Nächste Schritte

* Überwachen der Azure Cosmos DB-Daten anhand der [Diagnoseeinstellungen](cosmosdb-monitor-resource-logs.md) in Azure
* [Überwachen von Azure Cosmos DB-Vorgängen auf Steuerungsebene](audit-control-plane-logs.md)