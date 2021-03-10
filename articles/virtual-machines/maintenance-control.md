---
title: Übersicht über die Wartungssteuerung für virtuelle Azure-Computer unter Verwendung des Azure-Portals
description: Es wird beschrieben, wie Sie die Wartung steuern, wenn diese mithilfe der Wartungssteuerung auf Ihre virtuellen Azure-Computer angewendet wird.
author: cynthn
ms.service: virtual-machines
ms.subservice: maintenance-control
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 11/19/2020
ms.author: cynthn
ms.openlocfilehash: 290a1e8da4e9b3e8eff171ab2d5837bfc9c381b9
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102552421"
---
# <a name="managing-platform-updates-with-maintenance-control"></a>Verwalten von Plattformupdates mit der Wartungssteuerung 

Verwalten Sie mit der Wartungssteuerung Plattformupdates, die keinen Neustart erfordern. Azure aktualisiert die eigene Infrastruktur häufig, um die Zuverlässigkeit, Leistung und Sicherheit zu verbessern oder neue Features zu integrieren. Die meisten Updates sind für Benutzer transparent. Einige empfindliche Workloads wie Gaming, Medienstreaming und Finanztransaktionen können nicht einmal wenige Sekunden tolerieren, in denen eine VM zur Wartung eingefroren oder getrennt wird. Die Wartungssteuerung bietet Ihnen die Möglichkeit, auf Plattformupdates zu warten und diese innerhalb eines 35-tägigen rollierenden Zeitfensters anzuwenden. 

Mit der Wartungssteuerung können Sie entscheiden, wann Sie Updates auf Ihre isolierten VMs und dedizierten Azure-Hosts anwenden.

Mit der Wartungssteuerung können Sie folgende Aktionen ausführen:
- Batchupdates in ein Updatepaket
- Warten von bis zu 35 Tagen bis zur Anwendung der Updates 
- Automatisieren Sie Plattformupdates durch Konfigurieren eines Wartungszeitplans oder mithilfe von [Azure Functions](https://github.com/Azure/azure-docs-powershell-samples/tree/master/maintenance-auto-scheduler).
- Wartungskonfigurationen funktionieren abonnement- und ressourcengruppenübergreifend. 

## <a name="limitations"></a>Einschränkungen

- Virtuelle Computer müssen sich auf einem [dedizierten Host](./dedicated-hosts.md) befinden oder mithilfe einer [isolierten VM-Größe](isolation.md) erstellt werden.
- Wenn ein Wartungszeitplan festgelegt wird, muss er mindestens zwei Stunden betragen.
- Nach 35 Tagen wird automatisch ein Update angewendet.
- Der Benutzer muss über einen **Ressourcenmitwirkender**-Zugriff verfügen.

## <a name="management-options"></a>Verwaltungsoptionen

Wartungskonfigurationen können mithilfe einer der folgenden Optionen erstellt und verwaltet werden:

- [Azure-Befehlszeilenschnittstelle](maintenance-control-cli.md)
- [Azure PowerShell](maintenance-control-powershell.md)
- [Azure portal](maintenance-control-portal.md)

Ein Azure Functions-Beispiel finden Sie unter [Planen von Wartungsupdates mit der Wartungssteuerung und Azure Functions](https://github.com/Azure/azure-docs-powershell-samples/tree/master/maintenance-auto-scheduler).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Wartung und Updates](maintenance-and-updates.md).
