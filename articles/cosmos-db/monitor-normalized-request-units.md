---
title: Überwachen von normalisierten RU/s für einen Azure Cosmos-Container oder ein -Konto
description: Es wird beschrieben, wie Sie für einen Vorgang die Nutzung der normalisierten Anforderungseinheiten in Azure Cosmos DB überwachen. Besitzer eines Azure Cosmos DB-Kontos können so ermitteln, für welche Vorgänge mehr Anforderungseinheiten verbraucht werden.
ms.service: cosmos-db
ms.topic: how-to
author: kanshiG
ms.author: govindk
ms.date: 06/25/2020
ms.openlocfilehash: 8709389208ba1320685b1834b20893f08ef33ed7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482903"
---
# <a name="how-to-monitor-normalized-rus-for-an-azure-cosmos-container-or-an-account"></a>Überwachen von normalisierten RU/s für einen Azure Cosmos-Container oder ein -Konto

Azure Monitor für Azure Cosmos DB verfügt über eine Metrikansicht zum Überwachen Ihres Kontos und zum Erstellen von Dashboards. Weil die Azure Cosmos DB-Metriken standardmäßig erfasst werden, müssen Sie bei diesem Feature nichts explizit aktivieren oder konfigurieren.

Die Metrik **Normalisierter RU-Verbrauch** wird verwendet, um zu erfassen, wie gut ausgelastet die Replikate Anforderungseinheiten über die Partitionsschlüsselbereiche hinweg nutzen. Azure Cosmos DB verteilt den Durchsatz gleichmäßig auf alle physischen Partitionen. Diese Metrik stellt eine Ansicht des maximalen Durchsatzverbrauchs innerhalb einer Replikatgruppe pro Sekunde bereit. Verwenden Sie diese Metrik, um die Nutzung von Anforderungseinheiten pro Sekunde für alle Partitionen eines gegebenen Containers zu berechnen. Wenn Sie diese Metrik verwenden und ein hoher Prozentsatz der Auslastung von Anforderungseinheiten angezeigt wird, sollten Sie den Durchsatz erhöhen, um die Anforderungen Ihrer Workload zu erfüllen.

## <a name="what-to-expect-and-do-when-normalized-rus-is-higher"></a>Was zu erwarten und zu tun ist, wenn der Wert für normalisierte RU/s höher ist

Wenn der normalisierte RU/s-Verbrauch 100 % erreicht, empfängt der Client Rateneinschränkungsfehler. Der Client sollte die Wartezeit einhalten und den Vorgang dann wiederholen. Wenn in einer kurzen Spitze eine Auslastung von 100 % erreicht wird, bedeutet dies, dass der Durchsatz für das Replikat sein maximales Leistungslimit erreicht hat. Beispielsweise führt ein einzelner Vorgang, z. B. eine gespeicherte Prozedur, die alle RU/s für ein Replikat verbraucht, zu einer kurzen Spitze im normalisierten RU/s-Verbrauch. In solchen Fällen gibt es keine unmittelbaren Rateneinschränkungsfehler, wenn die Anforderungsrate niedrig ist. Dies liegt daran, dass Azure Cosmos DB Anforderungen erlaubt, mehr als die bereitgestellten RU/s für die jeweilige Anforderung abzurechnen, und für andere Anforderungen innerhalb dieses Zeitraums wird die Rate eingeschränkt.

Die Azure Monitor-Metriken helfen Ihnen, die Vorgänge über den Statuscode mithilfe der Metrik **Anforderungen gesamt** zu ermitteln. Später können Sie diese Anforderungen nach dem Statuscode 429 filtern und nach **Vorgangstyp** aufteilen.

Um die Anforderungen zu ermitteln, bei denen die Rate begrenzt ist, empfiehlt es sich, diese Informationen über Diagnoseprotokolle zu erfassen.

Wenn ein kontinuierlicher Spitzenwert von 100 % normalisierter RU/s-Verbrauch vorliegt oder der Wert fast 100 % beträgt, wird empfohlen, den Durchsatz zu erhöhen. Mithilfe der Azure Monitor-Metriken und der Azure Monitor-Protokolle können Sie herausfinden, welche Vorgänge einen hohen Verbrauch haben und wie hoch deren Spitzenauslastung ist.

Die Metrik **Normalisierter RU-Verbrauch** wird auch verwendet, um zu erkennen, welcher Partitionsschlüsselbereich höher ausgelastet ist. Auf diese Weise erhalten Sie die Verzerrung des Durchsatzes auf einen Partitionsschlüsselbereich. Später können Sie die Auslastung im **PartitionKeyRUConsumption**-Protokoll in den Monitor-Protokollen nachverfolgen, um Informationen darüber zu erhalten, welche logischen Partitionsschlüssel eine hohe Auslastung aufweisen.

## <a name="view-the-normalized-request-unit-consumption-metric"></a>Anzeigen der Metrik für normalisierten Anforderungseinheitenverbrauch

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie auf der Navigationsleiste auf der linken Seite die Option **Monitor** und anschließend **Metrik** aus.

   :::image type="content" source="./media/monitor-normalized-request-units/monitor-metrics-blade.png" alt-text="Bereich „Metriken“ in Azure Monitor":::

3. Klicken Sie im Bereich **Metriken** auf **Ressource auswählen**. Wählen Sie dann das erforderliche **Abonnement** und die **Ressourcengruppe** aus. Wählen Sie unter **Ressourcentyp** die Option **Azure Cosmos DB-Konten** aus. Wählen Sie dann eins der vorhandenen Azure Cosmos-Konten und anschließend **Anwenden** aus.

   :::image type="content" source="./media/monitor-normalized-request-units/select-cosmos-db-account.png" alt-text="Auswählen eines Azure Cosmos-Kontos zum Anzeigen von Metriken":::

4. Als Nächstes können Sie eine Metrik aus der Liste der verfügbaren Metriken auswählen. Sie können spezifische Metriken für Anforderungseinheiten, Speicher, Wartezeit, Verfügbarkeit, Cassandra usw. auswählen. Ausführliche Informationen zu allen verfügbaren Metriken in dieser Liste finden Sie im Artikel [Metriken nach Kategorie](monitor-cosmos-db-reference.md). In diesem Beispiel wählen wir die Metrik **Normalisierter RU-Verbrauch** und **Max** als Aggregationswert aus.

   Zusätzlich zu diesen Angaben können Sie auch **Zeitbereich** und **Zeitgranularität** für die Metriken auswählen. Sie können Metriken maximal für die letzten 30 Tage anzeigen.  Nach Anwendung des Filters wird ein darauf basierendes Diagramm angezeigt.

   :::image type="content" source="./media/monitor-normalized-request-units/normalized-request-unit-usage-metric.png" alt-text="Auswählen einer Metrik über das Azure-Portal":::

### <a name="filters-for-normalized-request-unit-consumption"></a>Filter für normalisierten Anforderungseinheitenverbrauch

Sie können Metriken und das angezeigte Diagramm auch nach bestimmten Werten für **CollectionName**, **DatabaseName**, **PartitionKeyRangeID** und **Region** filtern. Wählen Sie zum Filtern der Metriken **Filter hinzufügen** aus, und wählen Sie die erforderliche Eigenschaft (z. B. **CollectionName**) und den entsprechenden Wert aus, der Sie interessiert. Im Diagramm werden die normalisierten RU-Verbrauchseinheiten angezeigt, die für den Container im ausgewählten Zeitraum verbraucht wurden.  

Sie können Metriken mit der Option **Apply splitting** (Aufteilung anwenden) gruppieren.  

Die Metrik für den Verbrauch von normalisierten Anforderungseinheiten für jeden Container wird wie in der folgenden Abbildung dargestellt angezeigt:

:::image type="content" source="./media/monitor-normalized-request-units/normalized-request-unit-usage-filters.png" alt-text="Anwenden von Filtern auf die Metrik für normalisierten Anforderungseinheitenverbrauch":::

## <a name="next-steps"></a>Nächste Schritte

* Überwachen der Azure Cosmos DB-Daten anhand der [Diagnoseeinstellungen](cosmosdb-monitor-resource-logs.md) in Azure
* [Überwachen von Azure Cosmos DB-Vorgängen auf Steuerungsebene](audit-control-plane-logs.md)
