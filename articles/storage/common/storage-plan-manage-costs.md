---
title: Planen und Verwalten von Kosten für Azure Storage
description: Es wird beschrieben, wie Sie Kosten für Azure Storage planen und verwalten, indem Sie die Kostenanalyse im Azure-Portal verwenden.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: normesta
ms.subservice: common
ms.custom: subject-cost-optimization
ms.openlocfilehash: aa0b789b31f50c8b1ccf5450700874a02ad4664c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304361"
---
# <a name="plan-and-manage-costs-for-azure-storage"></a>Planen und Verwalten von Kosten für Azure Storage

In diesem Artikel wird beschrieben, wie Sie die Kosten für Azure Storage planen und verwalten. Bevor Sie Ressourcen hinzufügen, verwenden Sie zunächst den Azure-Preisrechner, um die Speicherkosten zu planen. Nachdem Sie mit der Nutzung der Azure Storage-Ressourcen begonnen haben, können Sie Features für die Kostenverwaltung verwenden, um Budgets festzulegen und die Kosten zu überwachen. Sie können auch vorhergesagte Kosten überprüfen und Ausgabentrends überwachen, um die Bereiche zu identifizieren, in denen ggf. Maßnahmen erforderlich sind.

Beachten Sie, dass Kosten für Azure Storage nur einen Teil der monatlichen Kosten Ihrer Azure-Rechnung darstellen. Zwar wird in diesem Artikel das Planen und Verwalten der Kosten für Azure Storage erläutert, doch werden Ihnen alle Azure-Dienste und -Ressourcen in Rechnung gestellt, die für Ihr Azure-Abonnement verwendet werden, einschließlich der Dienste von Drittanbietern. Nachdem Sie sich mit der Verwaltung der Kosten für Azure Storage vertraut gemacht haben, können Sie ähnliche Methoden anwenden, um die Kosten für alle Azure-Dienste zu verwalten, die unter Ihrem Abonnement genutzt werden.

## <a name="prerequisites"></a>Voraussetzungen

Die Kostenanalyse unterstützt verschiedene Arten von Azure-Kontotypen. Die vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](../../cost-management-billing/costs/understand-cost-mgt-data.md). Um Kostendaten anzeigen zu können, müssen Sie mindestens über Lesezugriff auf Ihr Azure-Konto verfügen. Informationen zum Zuweisen des Zugriffs auf Azure Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Cost Management-Daten](../../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs-before-creating-an-azure-storage-account"></a>Schätzen der Kosten vor dem Erstellen eines Azure Storage Kontos

Verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/), um die Kosten zu schätzen, bevor Sie ein Azure Storage-Konto erstellen und Daten dorthin übertragen.

1. Wählen Sie auf der Seite [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/) die Kachel **Speicherkonten** aus.

2. Scrollen Sie auf der Seite nach unten, und suchen Sie den Abschnitt **Speicherkonten** in Ihrer Schätzung.

3. Wählen Sie Optionen aus den Dropdownlisten aus. 

   Wenn Sie Werte in diesen Dropdownlisten ändern, ändert sich auch die Kostenschätzung. Diese Schätzung wird in der oberen Ecke und am unteren Rand angezeigt. 
    
   ![Überwachen der Kosten im Bereich „Kostenanalyse“](media/storage-plan-manage-costs/price-calculator-storage-type.png)

   Wenn Sie den Wert in der Dropdownliste **Typ** ändern, ändern sich auch andere Optionen, die auf diesem Arbeitsblatt angezeigt werden. Verwenden Sie die Links im Abschnitt **Weitere Informationen**, um mehr darüber zu erfahren, was die einzelnen Optionen bedeuten und wie sich diese Optionen auf den Preis von speicherbezogenen Vorgängen auswirken. 

4. Ändern Sie die übrigen Optionen, um die jeweilige Auswirkung auf die Schätzung anzuzeigen.

## <a name="use-budgets-and-cost-alerts"></a>Verwenden von Budgets und Kostenwarnungen

Sie können [Budgets](../../cost-management-billing/costs/tutorial-acm-create-budgets.md) erstellen, um Kosten zu verwalten, und Warnungen erstellen, die die Beteiligten automatisch über Ausgabenanomalien und Überschreitungsrisiken informieren. Warnungen basieren auf Ausgaben im Vergleich zum Budget und zu Kostenschwellenwerten. Budgets und Warnungen werden für Azure-Abonnements und -Ressourcengruppen erstellt und sind daher im Rahmen einer umfassenden Strategie zur Kostenüberwachung hilfreich. Unter Umständen verfügen sie aber über eine eingeschränkte Funktionalität für die Verwaltung der Kosten einzelner Azure-Dienste, z. B. die Kosten für Azure Storage, weil diese Funktionen für die Nachverfolgung von Kosten auf höherer Ebene ausgelegt sind.

## <a name="monitor-costs"></a>Überwachen der Kosten

Wenn Sie Azure-Ressourcen mit Azure Storage verwenden, fallen Kosten an. Die Kosten pro Ressourcennutzungseinheit variieren nach dem Zeitintervall (Sekunden, Minuten, Stunden und Tage) oder der Einheitennutzung (Bytes, Megabytes usw.) Kosten fallen an, sobald mit der Nutzung von Azure Storage begonnen wird. Die Kosten werden im Bereich [Kostenanalyse](../../cost-management-billing/costs/quick-acm-cost-analysis.md) im Azure-Portal angezeigt.

Bei Verwendung der Kostenanalyse können Sie Azure Storage-Kosten in Diagrammen und Tabellen für unterschiedliche Zeitintervalle anzeigen. Beispiele hierfür sind „Tag“, „Aktueller Monat“, „Vorheriger Monat“ und „Jahr“. Außerdem können Sie Kosten nach Budgets und vorhergesagten Kosten anzeigen. Wenn Sie im Laufe der Zeit zu längeren Zeiträumen wechseln, können Sie Ausgabentrends ermitteln und erkennen, wo es ggf. zu hohen Ausgaben gekommen ist. Wenn Sie Budgets erstellt haben, können Sie auch leicht feststellen, wo diese überschritten wurden.

So zeigen Sie Azure Storage-Kosten in der Kostenanalyse an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Öffnen Sie das Fenster **Kostenverwaltung + Abrechnung**, und wählen Sie im Menü die Option **Kostenverwaltung** und dann **Kostenanalyse** aus. Anschließend können Sie den Bereich für ein bestimmtes Abonnement in der Dropdownliste **Bereich** ändern.

   ![Überwachen der Kosten im Bereich „Kostenanalyse“](./media/storage-plan-manage-costs/cost-analysis-pane.png)

4. Um nur die Kosten für Azure Storage anzuzeigen, wählen Sie **Filter hinzufügen** und dann **Dienstname** aus. Wählen Sie dann **Speicher** aus der Liste aus. 

   In diesem Beispiel werden nur die Kosten für Azure Storage angezeigt:

   ![Überwachen der Speicherkosten im Bereich „Kostenanalyse“](./media/storage-plan-manage-costs/cost-analysis-pane-storage.png)

Im obigen Beispiel sind die aktuellen Kosten für den Dienst angegeben. Die Kosten nach Azure-Regionen (Standorten) und nach Ressourcengruppe werden ebenfalls angezeigt.  

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Verwaltung von Kosten mit der [Kostenanalyse](../../cost-management-billing/costs/quick-acm-cost-analysis.md).

Die folgenden Artikel enthalten weitere Informationen zur Preisgestaltung für Azure Storage:

- [Übersicht über die Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
- [Optimieren der Kosten für Blobspeicher mit reservierter Kapazität](../blobs/storage-blob-reserved-capacity.md)
