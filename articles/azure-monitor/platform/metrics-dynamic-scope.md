---
title: Anzeigen mehrerer Ressourcen im Azure-Metrik-Explorer
description: Hier erfahren Sie, wie Sie mehrere Ressourcen mit dem Azure-Metrik-Explorer visualisieren.
author: ritaroloff
services: azure-monitor
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: riroloff
ms.subservice: metrics
ms.openlocfilehash: 5f5f75e63a0b201144329a6e2b7c0d503de54692
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736472"
---
# <a name="view-multiple-resources-in-the-azure-metrics-explorer"></a>Anzeigen mehrerer Ressourcen im Azure-Metrik-Explorer

Mit der Ressourcenbereichsauswahl können Sie Metriken für mehrere Ressourcen anzeigen, die sich in demselben Abonnement und in derselben Region befinden. In diesem Artikel wird erläutert, wie Sie mehrere Ressourcen mithilfe der Azure-Metrik-Explorer-Funktion von Azure Monitor anzeigen. 

## <a name="select-a-resource"></a>Auswählen einer Ressource 

Wählen Sie im Menü **Azure Monitor** oder im Abschnitt **Überwachung** des Menüs einer Ressource die Option **Metriken** aus. Wählen Sie dann **Bereich auswählen** aus, um die Bereichsauswahl zu öffnen. 

Wählen Sie mithilfe der Bereichsauswahl die Ressourcen aus, deren Metriken Sie anzeigen möchten. Wenn Sie den Metrik-Explorer über das Menü einer Ressource geöffnet haben, sollte der Bereich angegeben sein. 

![Screenshot zum Öffnen der Auswahl des Ressourcenbereichs.](./media/metrics-charts/019.png)

## <a name="select-multiple-resources"></a>Auswählen mehrerer Ressourcen 

Einige Ressourcentypen können Metriken über mehrere Ressourcen abfragen. Die Metriken müssen sich innerhalb desselben Abonnements und Speicherorts befinden. Diese Ressourcentypen finden Sie oben im Menü **Ressourcentypen**.

![Screenshot eines Menüs mit Ressourcen, die mit mehreren Ressourcen kompatibel sind.](./media/metrics-charts/020.png)

> [!WARNING] 
> Sie müssen über die Berechtigung „Überwachungsleser“ auf der Abonnementebene verfügen, um Metriken für mehrere Ressourcen, Ressourcengruppen oder ein Abonnement visuell darstellen zu können. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

Um Metriken für mehrere Ressourcen zu visualisieren, müssen Sie zunächst in der Ressourcenbereichsauswahl mehrere Ressourcen auswählen. 

![Screenshot: Auswählen mehrerer Ressourcen.](./media/metrics-charts/021.png)

> [!NOTE]
> Bei den Ressourcen, die Sie auswählen, müssen Ressourcentyp, Speicherort und Abonnement identisch sein. Ressourcen, die diese Kriterien nicht erfüllen, sind nicht wählbar. 

Wählen Sie nach Abschluss des Vorgangs **Übernehmen** aus, um Ihre Auswahl zu speichern. 

## <a name="select-a-resource-group-or-subscription"></a>Auswählen einer Ressourcengruppe oder eines Abonnements 

> [!WARNING]
> Sie müssen über die Berechtigung „Überwachungsleser“ auf der Abonnementebene verfügen, um Metriken für mehrere Ressourcen, Ressourcengruppen oder ein Abonnement visuell darstellen zu können. 

Bei Typen, die mit mehreren Ressourcen kompatibel sind, können Sie Metriken auch ein Abonnement oder mehrere Ressourcengruppen übergreifend abfragen. Wählen Sie zunächst ein Abonnement oder eine oder mehrere Ressourcengruppe aus: 

![Screenshot: Abfragen von Metriken für mehrere Ressourcengruppen.](./media/metrics-charts/022.png)

Wählen Sie einen Ressourcentyp und einen Speicherort aus. 

![Screenshot der ausgewählten Ressourcengruppen.](./media/metrics-charts/023.png)

Sie können die ausgewählten Bereiche erweitern, um die Ressourcen zu überprüfen, für die die Auswahl gilt.

![Screenshot der ausgewählten Ressourcen innerhalb der Gruppen.](./media/metrics-charts/024.png)

Wählen Sie **Anwenden** aus, wenn Sie die Auswahl der Bereiche abgeschlossen haben. 

## <a name="split-and-filter-by-resource-group-or-resources"></a>Aufteilen und Filtern nach Ressourcengruppe oder Ressourcen

Nachdem Sie Ihre Ressourcen festgelegt haben, können Sie mit Aufteilen und Filtern mehr Einblicke in Ihre Daten gewinnen. 

Durch Aufteilen können Sie visualisieren, wie verschiedene Segmente der Metrik miteinander verglichen werden. Wenn Sie beispielsweise eine Metrik für mehrere Ressourcen darstellen, können Sie **Teilung anwenden** verwenden, um nach Ressourcen-ID oder Ressourcengruppe aufzuteilen. Mit der Teilung können Sie eine einzelne Metrik über mehrere Ressourcen oder Ressourcengruppen vergleichen.  

Das folgende Diagramm zeigt z. B. den Prozentsatz der CPU-Auslastung auf neun VMs. Wenn Sie nach Ressourcen-ID aufteilen, sehen Sie, wie sich der Prozentsatz der CPU-Auslastung von VM zu VM unterscheidet. 

![Screenshot: Verwenden der Aufteilung zum Anzeigen der CPU-Auslastung in Prozent auf VMs.](./media/metrics-charts/026.png)

Zusammen mit der Aufteilung können Sie die Filterfunktion verwenden, um nur die gewünschten Ressourcengruppen anzuzeigen.  Um beispielsweise die prozentuale CPU-Auslastung für VMs für eine bestimmte Ressourcengruppe anzuzeigen, können Sie mit dem Tool **Filter hinzufügen** nach der Ressourcengruppe filtern. 

In diesem Beispiel filtern wir nach TailspinToysDemo. Hier entfernt der Filter Metriken, die Ressourcen in TailspinToys zugeordnet sind. 

![Screenshot: Filtern nach Ressourcengruppe.](./media/metrics-charts/027.png)

## <a name="pin-multiple-resource-charts"></a>Anheften mehrerer Ressourcendiagramme 

Diagramme über mehrere Ressourcen, die Metriken Ressourcengruppen und Abonnements übergreifend visualisieren, setzen beim Benutzer die Berechtigung *Überwachungsleser* auf der Abonnementebene voraus. Stellen Sie sicher, dass alle Benutzer der Dashboards, denen Sie mehrere Ressourcendiagramme anheften, über ausreichende Berechtigungen verfügen. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

Informationen zum Anheften des Diagramms über mehrere Ressourcen an ein Dashboard finden Sie unter [Anheften von Diagrammen an Dashboards](./metrics-charts.md#pinning-to-dashboards). 

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für Metrikdiagramme](metrics-troubleshoot.md)
* [Unterstützte Metriken von Azure Monitor](metrics-supported.md)
* [Beispiele für Metrikdiagramme](metric-chart-samples.md)