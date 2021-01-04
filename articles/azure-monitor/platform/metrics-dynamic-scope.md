---
title: Anzeigen mehrerer Ressourcen in Metrik-Explorer
description: Hier erfahren Sie, wie Sie mehrere Ressourcen in Metrik-Explorer (Azure Monitor) visualisieren.
author: ritaroloff
services: azure-monitor
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: riroloff
ms.subservice: metrics
ms.openlocfilehash: 724809dbce3ca1b5a36f4da0ba5c03d0f78897f5
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577560"
---
# <a name="viewing-multiple-resources-in-metrics-explorer"></a>Anzeigen mehrerer Ressourcen in Metrik-Explorer

Mit der Ressourcenbereichsauswahl können Sie Metriken für mehrere Ressourcen anzeigen, die sich in demselben Abonnement und in derselben Region befinden. Im Folgenden finden Sie Anweisungen zum Anzeigen mehrerer Ressourcen in Metrik-Explorer (Azure Monitor). 

## <a name="selecting-a-resource"></a>Auswählen einer Ressource 

Wählen Sie im Menü **Azure Monitor** oder im Abschnitt **Überwachung** des Menüs einer Ressource die Option **Metriken** aus. Klicken Sie auf die Schaltfläche „Bereich auswählen“, um die Ressourcenbereichsauswahl zu öffnen, in der Sie die Ressource(n) auswählen können, für die Sie Metriken anzeigen möchten. Wenn Sie Metrik-Explorer über das Menü einer Ressource geöffnet haben, sollte dieser Bereich bereits angegeben sein. 

![Screenshot der rot hervorgehobenen Ressourcenbereichsauswahl](./media/metrics-charts/019.png)

## <a name="selecting-multiple-resources"></a>Auswählen mehrerer Ressourcen 

Bei einigen Ressourcentypen ist die Möglichkeit aktiviert, Metriken für mehrere Ressourcen abzufragen, sofern sie sich im selben Abonnement und am selben Standort befinden. Diese Ressourcentypen finden Sie oben in der Dropdownliste „Ressourcentypen“. 

![Screenshot einer Dropdownliste von Ressourcen, die mit mehreren Ressourcen kompatibel sind ](./media/metrics-charts/020.png)

> [!WARNING] 
> Sie müssen über die Berechtigung „Überwachungsleser“ auf der Abonnementebene verfügen, um Metriken für mehrere Ressourcen, Ressourcengruppen oder ein Abonnement visuell darstellen zu können. Folgen Sie hierzu den Anweisungen in [diesem Dokument](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Um Metriken für mehrere Ressourcen zu visualisieren, müssen Sie zunächst in der Ressourcenbereichsauswahl mehrere Ressourcen auswählen. 

![Screenshot: Auswählen mehrerer Ressourcen](./media/metrics-charts/021.png)

> [!NOTE]
> Sie können nur mehrere Ressourcen desselben Ressourcentyps, Standorts und Abonnements auswählen. Ressourcen, die diese Kriterien nicht erfüllen, können nicht ausgewählt werden. 

Klicken Sie nach dem Auswählen der gewünschten Ressourcen auf die Schaltfläche „Übernehmen“, um Ihre Auswahl zu speichern. 

## <a name="selecting-a-resource-group-or-subscription"></a>Auswählen einer Ressourcengruppe oder eines Abonnements 

> [!WARNING]
> Sie müssen über die Berechtigung „Überwachungsleser“ auf der Abonnementebene verfügen, um Metriken für mehrere Ressourcen, Ressourcengruppen oder ein Abonnement visuell darstellen zu können. 

Bei Typen, die mit mehreren Ressourcen kompatibel sind, können Sie auch Metriken für ein Abonnement oder mehrere Ressourcengruppen abfragen. Wählen Sie zunächst ein Abonnement oder eine oder mehrere Ressourcengruppe aus: 

![Screenshot: Abfragen von Metriken für mehrere Ressourcengruppen ](./media/metrics-charts/022.png)

Dann müssen Sie einen Ressourcentyp und einen Standort auswählen, bevor Sie den neuen Bereich anwenden können. 

![Screenshot der ausgewählten Ressourcengruppen ](./media/metrics-charts/023.png)

Sie können die ausgewählten Bereiche auch erweitern, um zu überprüfen, für welche Ressourcen dies gilt.

![Screenshot der ausgewählten Ressourcen innerhalb der Gruppen ](./media/metrics-charts/024.png)

Klicken Sie nach dem Auswählen Ihrer Bereiche auf „Übernehmen“, um Ihre Auswahl zu speichern. 

## <a name="splitting-and-filtering-by-resource-group-or-resources"></a>Aufteilen und Filtern nach Ressourcengruppe oder Ressourcen

Nachdem Sie Ihre Ressourcen festgelegt haben, können Sie das Tool zum Aufteilen und Filtern verwenden, um mehr Einblicke in Ihre Daten zu gewinnen. 

Durch Aufteilen können Sie visualisieren, wie verschiedene Segmente der Metrik miteinander verglichen werden. Wenn Sie beispielsweise eine Metrik für mehrere Ressourcen darstellen, können Sie das Tool zum Anwenden der Aufteilung verwenden, um nach Ressourcen-ID oder Ressourcengruppe aufzuteilen. Auf diese Weise können Sie eine einzelne Metrik problemlos über mehrere Ressourcen oder Ressourcengruppen vergleichen.  

Nachfolgend finden Sie beispielsweise ein Diagramm der CPU-Auslastung in Prozent für 9 VMs. Durch Aufteilen nach Ressourcen-ID können Sie leicht erkennen, wie sich die prozentuale CPU-Auslastung pro VM unterscheidet. 

![Screenshot: Verwenden der Aufteilung zum Anzeigen der CPU-Auslastung in Prozent pro VM](./media/metrics-charts/026.png)

Zusätzlich zur Aufteilung können Sie die Filterfunktion verwenden, um nur die gewünschten Ressourcengruppen anzuzeigen.  Wenn Sie beispielsweise die prozentuale CPU-Auslastung für VMs für eine bestimmte Ressourcengruppe anzeigen möchten, können Sie das Tool „Filter hinzufügen“ verwenden, um nach der Ressourcengruppe zu filtern. In diesem Beispiel filtern wir nach „TailspinToys“. Dadurch werden die mit den Ressourcen in TailspinToysDemo verknüpften Metriken entfernt. 

![Screenshot: Filtern nach Ressourcengruppe](./media/metrics-charts/027.png)

## <a name="pinning-your-multi-resource-charts"></a>Anheften Ihrer Diagramme mit mehreren Ressourcen 

> [!WARNING] 
> Sie müssen über die Berechtigung „Überwachungsleser“ auf der Abonnementebene verfügen, um Metriken für mehrere Ressourcen, Ressourcengruppen oder ein Abonnement visuell darstellen zu können. Folgen Sie hierzu den Anweisungen in [diesem Dokument](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). 

Wenn Sie Ihr Diagramm mit mehreren Ressourcen anheften möchten, folgen Sie den [hier](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts#create-alert-rules) angegebenen Anweisungen. 

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für den Metrik-Explorer](metrics-troubleshoot.md)
* [Unterstützte Metriken von Azure Monitor](metrics-supported.md)
* [Beispiele für Metrikdiagramme](metric-chart-samples.md)

