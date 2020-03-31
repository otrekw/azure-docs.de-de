---
title: Planen und Verwalten von Kosten für Azure Cosmos DB
description: Es wird beschrieben, wie Sie Kosten für Azure Cosmos DB planen und verwalten, indem Sie die Kostenanalyse im Azure-Portal verwenden.
author: SnehaGunda
ms.author: sngun
ms.custom: subject-cost-optimization
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/02/2020
ms.openlocfilehash: cf259dfa0b00393cf0fee259b199527569ba1792
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152583"
---
# <a name="plan-and-manage-costs-for-azure-cosmos-db"></a>Planen und Verwalten von Kosten für Azure Cosmos DB

In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Cosmos DB planen und verwalten können. Bevor Sie Ressourcen hinzufügen, verwenden Sie zunächst den Azure Cosmos DB Capacity Calculator, um die Kosten zu planen. Beim Hinzufügen der Azure-Ressourcen können Sie dann die geschätzten Kosten überprüfen. Nachdem Sie mit der Nutzung der Azure Cosmos DB-Ressourcen begonnen haben, können Sie die Features für die Kostenverwaltung verwenden, um Budgets festzulegen und die Kosten zu überwachen. Sie können auch die vorhergesagten Kosten überprüfen und Ausgabentrends ermitteln, um die Bereiche zu identifizieren, in denen ggf. Maßnahmen erforderlich sind.

Beachten Sie, dass die Kosten für Azure Cosmos DB nur einen Teil der monatlichen Kosten Ihrer Azure-Rechnung darstellen. Bei Verwendung anderer Azure-Dienste werden Ihnen alle Azure-Dienste und -Ressourcen berechnet, die unter Ihrem Azure-Abonnement genutzt werden, z. B. auch Drittanbieterdienste. In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Cosmos DB planen und verwalten. Nachdem Sie sich mit der Verwaltung der Kosten für Azure Cosmos DB vertraut gemacht haben, können Sie ähnliche Methoden anwenden, um die Kosten für alle Azure-Dienste zu verwalten, die unter Ihrem Abonnement genutzt werden.

## <a name="prerequisites"></a>Voraussetzungen

Die Kostenanalyse unterstützt verschiedene Arten von Azure-Kontotypen. Die vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](../cost-management-billing/costs/understand-cost-mgt-data.md). Um Kostendaten anzeigen zu können, müssen Sie mindestens über Lesezugriff auf Ihr Azure-Konto verfügen. Informationen zum Zuweisen des Zugriffs auf Azure Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Cost Management-Daten](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="review-estimated-costs-with-capacity-calculator"></a>Überprüfen der geschätzten Kosten mit dem Capacity Calculator

Verwenden Sie den [Azure Cosmos DB Capacity Calculator](https://cosmos.azure.com/capacitycalculator/), um die Kosten zu schätzen, bevor Sie die Ressourcen in einem Azure Cosmos-Konto erstellen. Der Capacity Calculator wird genutzt, um eine Schätzung des erforderlichen Durchsatzes und der Kosten Ihrer Workload zu erhalten. Die Konfiguration Ihrer Azure Cosmos-Datenbanken und -Container mit der richtigen Menge an bereitgestelltem Durchsatz bzw. [Anforderungseinheiten (RU/s)](request-units.md) für Ihre Workload ist von entscheidender Bedeutung, um die Kosten und die Leistung zu optimieren. Für eine Kostenschätzung müssen Sie Details wie API-Typ, Anzahl von Regionen, Elementgröße, Lese-/Schreibanforderungen pro Sekunde und insgesamt gespeicherte Daten eingeben. Weitere Informationen zum Capacity Calculator finden Sie im Artikel zum Thema [Schätzung](estimate-ru-with-capacity-planner.md).

Im folgenden Screenshot sind der Durchsatz und die Kostenschätzung bei Verwendung des Capacity Calculators dargestellt:

![Kostenschätzung im Azure Cosmos DB Capacity Calculator](./media/plan-manage-costs/capacity-calculator-cost-estimate.png)

## <a name="review-estimated-costs-from-the-azure-portal"></a>Überprüfen von geschätzten Kosten im Azure-Portal

Beim Erstellen von Azure Cosmos DB-Ressourcen im Azure-Portal werden die geschätzten Kosten angezeigt. Führen Sie die folgenden Schritte aus, um die Kostenschätzung zu überprüfen:

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrem Azure Cosmos-Konto.
1. Navigieren Sie zum **Daten-Explorer**.
1. Erstellen Sie einen neuen Container, z. B. einen Graph-Container.
1. Geben Sie den Durchsatz ein, der für Ihre Workload benötigt wird, z. B. 400 RU/s. Nach dem Eingeben des Durchsatzwerts wird die Kostenschätzung wie im folgenden Screenshot angezeigt:

   ![Kostenschätzung im Azure-Portal](./media/plan-manage-costs/cost-estimate-portal.png)

Wenn Ihr Azure-Abonnement über ein Ausgabenlimit verfügt, wird von Azure verhindert, dass Ihre Ausgaben den Guthabenbetrag übersteigen. Beim Erstellen und Nutzen von Azure-Ressourcen wird Ihr Guthaben verwendet. Wenn Sie Ihr Guthabenlimit erreicht haben, werden die von Ihnen bereitgestellten Ressourcen für den Rest des Abrechnungszeitraums deaktiviert. Sie können Ihr Guthabenlimit nicht ändern, aber Sie können es entfernen. Weitere Informationen zu Ausgabenlimits finden Sie unter [Azure-Ausgabenlimit](../billing/billing-spending-limit.md).

## <a name="use-budgets-and-cost-alerts"></a>Verwenden von Budgets und Kostenwarnungen

Sie können [Budgets](../cost-management/tutorial-acm-create-budgets.md) erstellen, um Kosten zu verwalten, und Warnungen erstellen, die die Beteiligten automatisch über Ausgabenanomalien und Überschreitungsrisiken informieren. Warnungen basieren auf Ausgaben im Vergleich zum Budget und zu Kostenschwellenwerten. Budgets und Warnungen werden für Azure-Abonnements und -Ressourcengruppen erstellt und sind daher im Rahmen einer umfassenden Strategie zur Kostenüberwachung hilfreich. Unter Umständen verfügen sie aber über eine eingeschränkte Funktionalität für die Verwaltung der Kosten einzelner Azure-Dienste, z. B. die Kosten für Azure Cosmos DB, weil diese Funktionen für die Nachverfolgung von Kosten auf höherer Ebene ausgelegt sind.

## <a name="monitor-costs"></a>Überwachen der Kosten

Wenn Sie Ressourcen mit Azure Cosmos DB verwenden, fallen Kosten an. Die Kosten pro Ressourcennutzungseinheit variieren nach dem Zeitintervall (Sekunden, Minuten, Stunden und Tage) oder der Nutzung von Anforderungseinheiten. Nach Beginn der Verwendung von Azure Cosmos DB fallen Kosten an. Sie werden im Azure-Portal im Bereich [Kostenanalyse](../cost-management/quick-acm-cost-analysis.md) angezeigt.

Bei Verwendung der Kostenanalyse können Sie die Azure Cosmos DB-Kosten in Graphen und Tabellen für unterschiedliche Zeitintervalle anzeigen. Beispiele hierfür sind „Tag“, „Aktuell“, „Vorheriger Monat“ und „Jahr“. Außerdem können Sie Kosten nach Budgets und vorhergesagten Kosten anzeigen. Wenn Sie im Laufe der Zeit zu längeren Zeiträumen wechseln, können Sie Ausgabentrends ermitteln und erkennen, wo es ggf. zu hohen Ausgaben gekommen ist. Wenn Sie Budgets erstellt haben, können Sie zudem leicht erkennen, wo diese überschritten wurden. Zeigen Sie die Azure Cosmos DB-Kosten wie folgt in der Kostenanalyse an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Öffnen Sie das Fenster **Kostenverwaltung + Abrechnung**, und wählen Sie im Menü die Option **Kostenverwaltung** und dann **Kostenanalyse** aus. Anschließend können Sie den Bereich für ein bestimmtes Abonnement in der Dropdownliste **Bereich** ändern.

1. Standardmäßig werden die Kosten für alle Dienste im ersten Ringdiagramm angezeigt. Wählen Sie im Diagramm den Bereich „Azure Cosmos DB“ aus.

1. Wählen Sie zum Eingrenzen der Kosten auf einen Dienst, z. B. Azure Cosmos DB, die Option **Filter hinzufügen** und dann **Dienstname** aus. Wählen Sie anschließend in der Liste die Option **Azure Cosmos DB** aus. In diesem Beispiel werden nur die Kosten für Azure Cosmos DB angezeigt:
 
   ![Überwachen der Kosten im Bereich „Kostenanalyse“](./media/plan-manage-costs/cost-analysis-pane.png)

Im obigen Beispiel sind die aktuellen Kosten für Azure Cosmos DB für den Monat Februar angegeben. Darüber hinaus enthalten die Diagramme auch die Azure Cosmos DB-Kosten nach Standort und Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Informationen zu den Preisen in Azure Cosmos DB:

* [Preismodell in Azure Cosmos DB](how-pricing-works.md)
* [Optimieren der Kosten für bereitgestellten Durchsatz in Azure Cosmos DB](optimize-cost-throughput.md)
* [Optimieren der Abfragekosten in Azure Cosmos DB](optimize-cost-queries.md)
* [Optimieren der Speicherkosten in Azure Cosmos DB](optimize-cost-storage.md)