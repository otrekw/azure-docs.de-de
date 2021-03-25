---
title: 'Übersicht: Wartungssteuerung für Betriebssystemimage-Upgrades in Azure-VM-Skalierungsgruppen'
description: Es wird beschrieben, wie Sie steuern können, wann automatische Betriebssystemimage-Upgrades in Ihren Azure-VM-Skalierungsgruppen per Wartungssteuerung per Rollout bereitgestellt werden.
author: ju-shim
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/11/2020
ms.author: jushiman
ms.openlocfilehash: 55cbc5db534dd1b05f91a24e0104b1f2dc110547
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90532945"
---
# <a name="preview-maintenance-control-for-azure-virtual-machine-scale-sets"></a>Vorschau: Wartungssteuerung für Azure-VM-Skalierungsgruppen 

Verwalten Sie [automatische Betriebssystemimage-Upgrades](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md) für Ihre VM-Skalierungsgruppen per Wartungssteuerung.

Bei der Wartungssteuerung können Sie in einer einfacheren und besser vorhersagbaren Umgebung entscheiden, wann Updates auf Betriebssystemdatenträger in Ihren VM-Skalierungsgruppen angewendet werden sollen. 

Wartungskonfigurationen funktionieren abonnement- und ressourcengruppenübergreifend.

Der gesamte Workflow besteht aus den folgenden Schritten: 
- Erstellen einer Wartungskonfiguration
- Zuordnen einer VM-Skalierungsgruppe zu einer Wartungskonfiguration
- Aktivieren von automatischen Betriebssystemupgrades

> [!IMPORTANT]
> Die Wartungssteuerung für Betriebssystemimage-Upgrades in Azure-VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschauphase (Public Preview).
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="limitations"></a>Einschränkungen

- Die VMs müssen in einer Skalierungsgruppe enthalten sein.
- Der Benutzer muss über einen **Ressourcenmitwirkender**-Zugriff verfügen.
- Die Wartungsdauer in der Wartungskonfiguration muss mindestens fünf Stunden betragen.
- Die Wartungswiederholung muss in der Wartungskonfiguration auf „Day“ (Tag) festgelegt werden.


## <a name="management-options"></a>Verwaltungsoptionen

Wartungskonfigurationen können mithilfe einer der folgenden Optionen erstellt und verwaltet werden:

- [Azure PowerShell](virtual-machine-scale-sets-maintenance-control-powershell.md)


## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Sie per Wartungssteuerung und PowerShell steuern, wann die Wartung auf Ihre virtuellen Azure-VM-Skalierungsgruppen angewendet wird.

> [!div class="nextstepaction"]
> [Steuerung der Wartung von VM-Skalierungsgruppen mit PowerShell](virtual-machine-scale-sets-maintenance-control-powershell.md)
