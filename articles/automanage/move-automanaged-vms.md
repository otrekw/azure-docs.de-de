---
title: Regionsübergreifendes Verschieben einer Azure Automanage-VM
description: Hier erfahren Sie mehr über das regionsübergreifende Verschieben eines automatisch verwalteten virtuellen Computers.
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 02/05/2021
ms.author: alsin
ms.custom: subject-moving-resources
ms.openlocfilehash: 4e9fba5ff9dc10e0691423da689024af3e2b72bc
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114468345"
---
# <a name="move-an-azure-automanage-virtual-machine-to-a-different-region"></a>Verschieben einer Azure Automanage-VM in eine andere Region
In diesem Artikel wird beschrieben, wie Sie Automanage auf einem virtuellen Computer (VM) aktivieren, wenn Sie diesen in eine andere Region verschieben. Sie sollten Ihre VMs aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf Anforderungen der Kapazitätsplanung. Die VMs, die Sie verschieben, werden derzeit möglicherweise automatisch verwaltet. Dies soll auch nach dem Verschieben noch der Fall sein.

## <a name="prerequisites"></a>Voraussetzungen
* Stellen Sie sicher, dass die Zielregion [von Automanage unterstützt wird](./automanage-virtual-machines.md#prerequisites).
* Vergewissern Sie sich, dass Ihre Log Analytics-Arbeitsbereichsregion, Ihre Automation-Kontoregion und die Zielregion alle Regionen sind, die von [diesen Regionszuordnungen](../automation/how-to/region-mappings.md) unterstützt werden.

## <a name="prepare-your-automanaged-vms-for-moving"></a>Vorbereiten der automatisch verwalteten VMs für das Verschieben
Deaktivieren Sie Automanage für Ihre automatisch verwalteten VMs. Wählen Sie hierzu Ihre VMs auf dem Blatt „Automanage“ aus, und klicken Sie dann auf **Disable automanagement** (Automatische Verwaltung deaktivieren).

## <a name="move-your-automanaged-vms-and-re-enable-automanage"></a>Verschieben der automatisch verwalteten VMs und erneutes Aktivieren von Automanage
Ausführliche Informationen zum Verschieben Ihrer VMs finden Sie in [diesem Artikel](../resource-mover/tutorial-move-region-virtual-machines.md).

Nachdem Sie Ihre VMs regionsübergreifend verschoben haben, können Sie Automanage erneut für sie aktivieren. Weitere Informationen finden Sie [hier](./automanage-virtual-machines.md#enabling-automanage-for-vms-in-azure-portal).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Azure Automanage finden Sie [hier](./automanage-virtual-machines.md).
* [Häufig gestellte Fragen zur automatischen Azure-Verwaltung für VMs](./faq.yml)