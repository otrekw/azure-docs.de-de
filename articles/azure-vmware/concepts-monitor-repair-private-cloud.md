---
title: 'Konzepte: Überwachen und Reparieren von privaten Azure VMware Solution-Clouds'
description: Es wird beschrieben, wie VMware ESXi-Server in Azure VMware Solution in einer privaten Azure VMware Solution-Cloud überwacht und repariert werden.
ms.topic: conceptual
ms.custom: contperf-fy21q2
ms.date: 02/16/2021
ms.openlocfilehash: 59319b5598be9770e82b9676a28444648230a019
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100633137"
---
# <a name="monitor-and-repair-azure-vmware-solution-private-clouds"></a>Überwachen und Reparieren von privaten Azure VMware Solution-Clouds

In Azure VMware Solution werden die VMware ESXi-Server ständig in einer privaten Azure VMware Solution-Cloud überwacht. 

## <a name="what-azure-vmware-solution-monitors"></a>Was wird von Azure VMware Solution überwacht?

In Azure VMware Solution werden die folgenden Bedingungen auf dem Host überwacht:  

- Prozessorstatus 
- Speicherstatus 
- Verbindung und Betriebszustand 
- Zustand des Hardwarelüfters 
- Verlust der Netzwerkkonnektivität 
- Zustand der Hardware-Hauptplatine 
- Aufgetretene Fehler auf den Datenträgern eines vSAN-Hosts 
- Hardwarespannung 
- Hardware-Temperaturstatus 
- Hardware-Betriebszustand 
- Speicherstatus 
- Verbindungsfehler 

> [!NOTE]
> Administratoren von Azure VMware Solution-Mandanten dürfen die obigen definierten VMware vCenter-Alarme nicht bearbeiten oder löschen, da diese über die Azure VMware Solution-Steuerungsebene in vCenter verwaltet werden. Diese Alarme werden von der Azure VMware Solution-Überwachung verwendet, um den Fehlerbehebungsprozess für den Azure VMware Solution-Host auszulösen.

## <a name="azure-vmware-solution-host-remediation"></a>Fehlerbehebung für Azure VMware Solution-Host  

Wenn von Azure VMware Solution ein Leistungsabfall oder ein Fehler auf dem Azure VMware Solution-Knoten erkannt wird, wird der Fehlerbehebungsprozess für den Host ausgelöst. Die Fehlerbehebung für den Host umfasst das Ersetzen des fehlerhaften Knotens durch einen neuen fehlerfreien Knoten.  

Die Fehlerbehebung für den Host beginnt, indem dem Cluster ein neuer fehlerfreier Knoten hinzugefügt wird. Anschließend wird der fehlerhafte Host nach Möglichkeit in den VMware vSphere-Wartungsmodus versetzt. VMware vMotion verschiebt die virtuellen Computer vom fehlerhaften Host auf andere verfügbare Server im Cluster. Hierbei ist es unter Umständen möglich, eine Livemigration von Workloads ohne Ausfallzeiten durchzuführen. Wenn der fehlerhafte Host nicht in den Wartungsmodus versetzt werden kann, wird er aus dem Cluster entfernt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Azure VMware Solution private Clouds überwacht und repariert, möchten Sie vielleicht mehr über Folgendes erfahren:

- [Azure VMware Solution-Upgrades für private Clouds](concepts-upgrades.md)
- [Aktivieren einer Azure VMware Solution-Ressource](enable-azure-vmware-solution.md)
