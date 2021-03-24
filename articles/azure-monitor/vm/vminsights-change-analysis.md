---
title: Änderungsanalyse in VM Insights
description: Die Integration von VM Insights in die Anwendungsänderungsanalyse ermöglicht Ihnen die Anzeige von Änderungen an einer VM, die sich möglicherweise auf die Leistung ausgewirkt haben.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/23/2020
ms.openlocfilehash: 375812813d704eb9b48d0ed8fbbc65dd5e47da49
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102046770"
---
# <a name="change-analysis-in-vm-insights"></a>Änderungsanalyse in VM Insights
Die Integration von VM Insights in die [Anwendungsänderungsanalyse](../app/change-analysis.md) ermöglicht Ihnen die Anzeige beliebiger Änderungen an einer VM, die sich möglicherweise auf die Leistung ausgewirkt haben.

## <a name="overview"></a>Übersicht
Angenommen, Sie verfügen über einen virtuellen Computer, dessen Ausführung sich verlangsamt, und möchten untersuchen, ob die aktuellen Änderungen an der Konfiguration die Leistung beeinträchtigt haben. Sie zeigen die Leistung der VM mit VM Insights an und stellen fest, dass die Speicherauslastung in der letzten Stunde zugenommen hat. Mit der Änderungsanalyse können Sie feststellen, ob Konfigurationsänderungen, die zu diesem Zeitpunkt durchgeführt wurden, die Ursache dieser Zunahme sind.

Der Anwendungsänderungsanalyse-Dienst aggregiert Änderungen aus [Azure Resource Graph](../../governance/resource-graph/how-to/get-resource-changes.md) sowie Änderungen geschachtelter Eigenschaften wie Netzwerksicherheitsregeln von Azure Resource Manager. 

## <a name="enabling-change-analysis"></a>Aktivieren der Änderungsanalyse
Wenn Sie die Änderungsanalyse in VM Insights integrieren möchten, müssen Sie den *Microsoft.ChangeAnalysis*-Ressourcenanbieter registrieren. Wenn Sie VM Insights oder die Anwendungsänderungsanalyse zum ersten Mal im Azure-Portal starten, wird dieser Ressourcenanbieter automatisch für Sie registriert. Bei der Anwendungsänderungsanalyse handelt es sich um einen kostenlosen Dienst, der keinen Leistungsmehraufwand von Ressourcen erfordert.

## <a name="view-change-analysis"></a>Anzeigen der Änderungsanalyse
Die Änderungsanalyse steht auf der Registerkarte **Leistung** oder **Zuordnen** von VM Insights zur Verfügung, wenn Sie die Option **Ändern** auswählen. 

[![Änderungen untersuchen](media/vminsights-change-analysis/investigate-changes-screenshot.png)](media/vminsights-change-analysis/investigate-changes-screenshot-zoom.png#lightbox)


Klicken Sie auf die Schaltfläche **Änderungen untersuchen**, um die für die VM gefilterte Seite „Anwendungsänderungsanalyse“ zu starten. Sie können die aufgeführten Änderungen überprüfen, um festzustellen, ob das Problem durch sie verursacht worden sein könnte. Wenn Sie sich bei einer bestimmten Änderung nicht sicher sind, erfahren Sie in der Spalte **Geändert von**, welche Person die Änderung vorgenommen hat.

[![Änderungsdetails](media/vminsights-change-analysis/change-details-screenshot.png)](media/vminsights-change-analysis/change-details-screenshot.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Anwendungsänderungsanalyse](../app/change-analysis.md).
- Erfahren Sie mehr über [Azure Resource Graph](../../governance/resource-graph/how-to/get-resource-changes.md). 

