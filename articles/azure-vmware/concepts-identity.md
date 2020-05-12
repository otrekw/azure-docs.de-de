---
title: Konzepte – Identität und Zugriff
description: Informationen zu den Identitäts- und Zugriffskonzepten von Azure VMware Solution (AVS)
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: ce85b60b38db86af14b9e9aa06e568436dc76658
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739966"
---
# <a name="azure-vmware-solution-avs-identity-concepts"></a>Identitätskonzepte von Azure VMware Solution (AVS)

Bei der Bereitstellung einer privaten Cloud werden vCenter Server und NSX-T Manager zur Verfügung gestellt. Sie verwenden vCenter, um VM-Workloads zu verwalten, und NSX-T Manager, um das softwaredefinierte Netzwerk der privaten Cloud zu erweitern.

Bei der Zugriffs- und Identitätsverwaltung werden CloudAdmin-Gruppenberechtigungen für vCenter und eingeschränkte Administratorrechte für NSX-T Manager verwendet. Diese Richtlinie stellt sicher, dass die Plattform Ihrer privaten Cloud automatisch aktualisiert werden kann. So werden regelmäßig die neuesten Features und Patches bereitgestellt. Weitere Informationen zu Upgrades für private Clouds finden Sie im Artikel [Konzepte: Upgrades für private Clouds][concepts-upgrades].

## <a name="vcenter-access-and-identity"></a>vCenter – Zugriff und Identität

Berechtigungen in vCenter werden über die CloudAdmin-Gruppe bereitgestellt. Diese Gruppe kann lokal in vCenter verwaltet werden, oder indem LDAP Single Sign-On von vCenter in Azure Active Directory integriert wird. Sie können diese Integration durchführen, nachdem Sie eine private Cloud bereitgestellt haben.

Die CloudAdmin- und CloudGlobalAdmin-Berechtigungen sind in der folgenden Tabelle aufgeführt.

|  Festgelegte Berechtigung           | CloudAdmin | CloudGlobalAdmin | Comment |
| :---                     |    :---:   |       :---:      |   :--:  |
|  Alarme                  | Ein CloudAdmin-Benutzer verfügt über sämtliche Berechtigungen für Alarme in Compute-ResourcePool und in VMs.     |          --        |  -- |
|  Automatische Bereitstellung             |  --  |        --        |  Hosts werden von Microsoft verwaltet.  |
|  Zertifikate            |  --  |        --       |  Zertifikate werden von Microsoft verwaltet.  |
|  Inhaltsbibliothek         | Ein CloudAdmin-Benutzer verfügt über Berechtigungen zum Erstellen und Verwenden von Dateien in einer Inhaltsbibliothek.    |         Mit SSO aktiviert.         |  Dateien in der Inhaltsbibliothek werden von Microsoft auf ESXi-Hosts verteilt.  |
|  Datacenter              |  --  |        --          |  Sämtliche Vorgänge in Rechenzentren werden von Microsoft ausgeführt.  |
|  Datenspeicher               | Datastore.AllocateSpace, Datastore.Browse, Datastore.Config, Datastore.DeleteFile, Datastore.FileManagement, Datastore.UpdateVirtualMachineMetadata     |    --    |   -- |
|  ESX-Agent-Manager       |  --  |         --       |  Alle Vorgänge werden von Microsoft ausgeführt.  |
|  Ordner                  |  Ein CloudAdmin-Benutzer verfügt über alle Berechtigungen für Ordner.     |  --  |  --  |
|  Global                  |  Global.CancelTask, Global.GlobalTag, Global.Health, Global.LogEvent, Global.ManageCustomFields, Global.ServiceManagers, Global.SetCustomField, Global.SystemTag         |                  |    |
|  Host                    |  Host.Hbr.HbrManagement      |        --          |  Alle anderen Hostvorgänge werden von Microsoft ausgeführt.  |
|  InventoryService        |  InventoryService.Tagging      |        --          |  --  |
|  Netzwerk                 |  Network.Assign    |                  |  Alle anderen Netzwerkvorgänge werden von Microsoft ausgeführt.  |
|  Berechtigungen             |  --  |        --       |  Alle Berechtigungsvorgänge werden von Microsoft ausgeführt.  |
|  Profilgesteuerter Speicher  |  --  |        --       |  Alle Profilvorgänge werden von Microsoft ausgeführt.  |
|  Resource                |  Ein CloudAdmin-Benutzer verfügt über alle Berechtigungen für Ressourcen.        |      --       | --   |
|  Geplante Aufgabe          |  Ein CloudAdmin-Benutzer verfügt über alle ScheduleTask-Berechtigungen.   |   --   | -- |
|  Sitzungen                |  Sessions.GlobalMessage, Sessions.ValidateSession      |   --   |  Alle anderen Sitzungsvorgänge werden von Microsoft ausgeführt.  |
|  Speicheransichten           |  StorageViews.View   |        --          |  Alle anderen Vorgänge für Speicheransichten werden von Microsoft ausgeführt (Dienstkonfiguration).  |
|  Aufgaben                   |  --  |  --   |  Erweiterungen zur Aufgabenverwaltung werden von Microsoft verwaltet.  |
|  vApp                    |  Ein CloudAdmin-Benutzer verfügt über alle vApp-Berechtigungen.  |  --  |  --  |
|  Virtual Machine         |  Ein CloudAdmin-Benutzer verfügt über alle VirtualMachine-Berechtigungen.  |  --  |  --  |
|  vService                |  Ein CloudAdmin-Benutzer verfügt über alle vService-Berechtigungen.  |  --  |  --  |

## <a name="nsx-t-manager-access-and-identity"></a>NSX-T Manager – Zugriff und Identität

Sie greifen mit dem Administratorkonto auf NSX-T Manager zu. Dieses Konto verfügt über vollständige Berechtigungen und ermöglicht es Ihnen, T1-Router, logische Switches und alle Dienste zu erstellen und zu verwalten. Durch die vollständigen Berechtigungen in NSX-T erhalten Sie auch Zugriff auf den NSX-T T0-Router. Eine Änderung am T0-Router kann dazu führen, dass die Netzwerkleistung beeinträchtigt wird oder nicht mehr auf eine private Cloud zugegriffen werden kann. Um die Supportanforderungen zu erfüllen, müssen Sie eine Supportanfrage im Azure-Portal öffnen, um Änderungen an Ihrem NSX-T T0-Router anzufordern.
  
## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt erfahren Sie mehr über [Konzepte für Upgrades von privaten Clouds][concepts-upgrades].

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-upgrades]: ./concepts-upgrades.md