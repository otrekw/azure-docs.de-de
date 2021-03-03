---
title: Ändern der Analyse in Azure Monitor für VMs
description: Die Integration von Azure Monitor für VMs in die Anwendungsänderungsanalyse	ermöglicht Ihnen die Anzeige beliebiger Änderungen an einem virtuellen Computer, die sich möglicherweise auf seine Leistung ausgewirkt haben.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/23/2020
ms.openlocfilehash: 59799a09436d5968a441f6f17655d3138a2d84d8
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601853"
---
# <a name="change-analysis-in-azure-monitor-for-vms"></a>Ändern der Analyse in Azure Monitor für VMs
Die Integration von Azure Monitor für VMs in die [Anwendungsänderungsanalyse](../app/change-analysis.md) ermöglicht Ihnen die Anzeige beliebiger Änderungen an einem virtuellen Computer, die sich möglicherweise auf seine Leistung ausgewirkt haben.

## <a name="overview"></a>Übersicht
Angenommen, Sie verfügen über einen virtuellen Computer, dessen Ausführung sich verlangsamt, und möchten untersuchen, ob die aktuellen Änderungen an der Konfiguration die Leistung beeinträchtigt haben. Sie zeigen die Leistung der VM mit Azure Monitor für VMs an und stellen fest, dass die Speicherauslastung in der letzten Stunde zugenommen hat. Mit der Änderungsanalyse können Sie feststellen, ob Konfigurationsänderungen, die zu diesem Zeitpunkt durchgeführt wurden, die Ursache dieser Zunahme sind.

Der Anwendungsänderungsanalyse-Dienst aggregiert Änderungen aus [Azure Resource Graph](../../governance/resource-graph/how-to/get-resource-changes.md) sowie Änderungen geschachtelter Eigenschaften wie Netzwerksicherheitsregeln von Azure Resource Manager. 

## <a name="enabling-change-analysis"></a>Aktivieren der Änderungsanalyse
Wenn Sie die Änderungsanalyse in Azure Monitor für VMs integrieren möchten, müssen Sie den *Microsoft.ChangeAnalysis*-Ressourcenanbieter registrieren. Wenn Sie Azure Monitor für VMs oder die Anwendungsänderungsanalyse zum ersten Mal im Azure-Portal starten, wird dieser Ressourcenanbieter automatisch für Sie registriert. Bei der Anwendungsänderungsanalyse handelt es sich um einen kostenlosen Dienst, der keinen Leistungsmehraufwand von Ressourcen erfordert.

## <a name="view-change-analysis"></a>Anzeigen der Änderungsanalyse
Die Änderungsanalyse steht auf der Registerkarte **Leistung** oder **Zuordnen** von Azure Monitor für VMs zur Verfügung, indem Sie die Option **Ändern** auswählen. 

[![Änderungen untersuchen](media/vminsights-change-analysis/investigate-changes-screenshot.png)](media/vminsights-change-analysis/investigate-changes-screenshot-zoom.png#lightbox)


Klicken Sie auf die Schaltfläche **Änderungen untersuchen**, um die für die VM gefilterte Seite „Anwendungsänderungsanalyse“ zu starten. Sie können die aufgeführten Änderungen überprüfen, um festzustellen, ob das Problem durch sie verursacht worden sein könnte. Wenn Sie sich bei einer bestimmten Änderung nicht sicher sind, erfahren Sie in der Spalte **Geändert von**, welche Person die Änderung vorgenommen hat.

[![Änderungsdetails](media/vminsights-change-analysis/change-details-screenshot.png)](media/vminsights-change-analysis/change-details-screenshot.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Anwendungsänderungsanalyse](../app/change-analysis.md).
- Erfahren Sie mehr über [Azure Resource Graph](../../governance/resource-graph/how-to/get-resource-changes.md). 

