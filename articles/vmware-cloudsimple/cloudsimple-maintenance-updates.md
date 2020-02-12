---
title: 'Azure VMware Solutions (AVS): AVS-Wartung und -Updates'
description: Hier wird der AVS-Dienstprozess für planmäßige Wartung und Updates beschrieben.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bf5937183fc20579ecd21aca8543a0a78d4b9ff3
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025026"
---
# <a name="avs-maintenance-and-updates"></a>AVS-Wartung und -Updates

Die private AVS-Cloudumgebung ist so konzipiert, dass sie keinen Single Point of Failure aufweist.

* ESXi-Cluster sind mit vSphere-Hochverfügbarkeit (High-Availability, HA) konfiguriert. Die Cluster sind so bemessen, dass sie mindestens einen Ersatzknoten für die Ausfallsicherheit aufweisen.
* Redundanter Primärspeicher wird von vSAN bereitgestellt. Dafür werden mindestens drei Knoten benötigt, um Schutz vor einem einzigen Ausfall zu bieten. vSAN kann konfiguriert werden, um eine höhere Resilienz für größere Cluster zu gewährleisten.
* vCenter-, PSC- und NSX Manager-VMs sind mit einem RAID-10-Speicher konfiguriert, um sie vor Speicherausfällen zu schützen. Die VMs sind durch vSphere-Hochverfügbarkeit gegen Knoten-/Netzausfälle geschützt.
* ESXi-Hosts haben redundante Lüfter und NICs.
* Tor- und Spine-Switches sind paarweise für Hochverfügbarkeit konfiguriert, um Resilienz zu gewährleisten.

AVS überwacht kontinuierlich die folgenden virtuellen Computer auf Betriebszeit und Verfügbarkeit und bietet Verfügbarkeits-SLAs:

* ESXi-Hosts
* vCenter
* PSC
* NSX Manager

AVS überwacht auch die folgenden Komponenten ständig auf Fehler:

* Festplatten
* Physische NIC-Ports
* Server
* Lüfter
* Power
* Switches
* Switchports

Wenn ein Datenträger oder Knoten ausfällt, wird automatisch ein neuer Knoten zu dem betroffenen VMware-Cluster hinzugefügt, um ihn sofort wieder in den ordnungsgemäßen Zustand zu versetzen.

AVS sichert, wartet und aktualisiert die folgenden VMware-Elemente in den privaten AVS-Clouds:

* ESXi
* VCenter-Plattformdienste
* Controller
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Sichern und Wiederherstellen

Die AVS-Sicherung umfasst Folgendes:

* Nächtliche inkrementelle Sicherungen von vCenter-, PSC- und DVS-Regeln.
* Native vCenter-APIs zur Sicherung von Komponenten auf Anwendungsebene.
* Automatische Sicherung vor Updates oder Upgrades der VMware-Verwaltungssoftware.
* vCenter-Datenverschlüsselung an der Quelle vor der Datenübertragung über einen mit TLS1.2 verschlüsselten Kanal an Azure. Die Daten werden in einem Azure-Blob gespeichert, wo sie über Regionen hinweg repliziert werden.

Sie können eine Wiederherstellung anfordern, indem Sie eine [Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) stellen.

## <a name="maintenance"></a>Wartung

AVS führt mehrere Arten von geplanten Wartungsmaßnahmen durch.

### <a name="backendinternal-maintenance"></a>Wartung von Back-End-/internen Komponenten

Diese Wartung beinhaltet in der Regel die Rekonfiguration physischer Objekte oder die Installation von Softwarepatches. Es hat keinen Einfluss auf die normale Nutzung der Objekte. Da sich redundante NICs in jedem physischen Rack befinden, sind der normale Netzwerkverkehr und der Betrieb der privaten AVS-Cloud nicht beeinträchtigt. Möglicherweise treten Leistungseinbußen nur dann auf, wenn Ihre Organisation davon ausgeht, die gesamte redundante Bandbreite während des Wartungsintervalls zu nutzen.

### <a name="avs-portal-maintenance"></a>AVS-Portalwartung

Wenn die Steuerungsebene oder Infrastruktur von AVS aktualisiert wird, sind begrenzte Ausfallzeiten erforderlich. Derzeit können die Wartungsintervalle einmal pro Monat stattfinden. Es wird erwartet, dass die Frequenz mit der Zeit abnimmt. AVS benachrichtigt Sie, wenn eine Portalwartung stattfindet, und hält das Intervall so kurz wie möglich. Während eines Portal-Wartungsintervalls funktionieren die folgenden Dienste weiterhin ohne Beeinträchtigung:

* VMware-Verwaltungsebene und Anwendungen
* vCenter-Zugriff
* Alle Netzwerk und Speicher
* Der gesamte Azure-Datenverkehr

### <a name="vmware-infrastructure-maintenance"></a>VMware-Infrastrukturverwaltung

Gelegentlich ist es notwendig, Änderungen an der Konfiguration der VMware-Infrastruktur vorzunehmen. Derzeit können diese Intervalle alle 1-2 Monate auftreten, aber es wird erwartet, dass die Häufigkeit mit der Zeit abnimmt. Diese Art der Wartung kann in der Regel ohne Unterbrechung der normalen Nutzung der AVS-Dienste durchgeführt werden. Während eines VMware-Wartungsintervalls funktionieren die folgenden Dienste weiterhin ohne Beeinträchtigung:

* VMware-Verwaltungsebene und Anwendungen
* vCenter-Zugriff
* Alle Netzwerk und Speicher
* Der gesamte Azure-Datenverkehr

## <a name="updates-and-upgrades"></a>Updates und Upgrades

AVS ist für die Lebenszyklusverwaltung der VMware-Software (ESXi, vCenter, PSC und NSX) in der privaten AVS-Cloud zuständig.

Softwareupdates umfassen:

* **Patches**. Sicherheitspatches oder Fehlerbehebungen, die von VMware freigegeben werden.
* **Updates**. Ändern der Nebenversion einer VMware-Stack-Komponente.
* **Upgrades**. Ändern der Hauptversion einer VMware-Stack-Komponente.

AVS testet ein wichtiges Sicherheitsupdate, sobald er von VMware zur Verfügung gestellt wird. Gemäß SLA führt AVS den Rollout des Sicherheitsupdates an private AVS-Cloudumgebungen innerhalb einer Woche aus.

AVS bietet vierteljährliche Wartungsupdates für VMware-Softwarekomponenten. Wenn eine neue Hauptversion der VMware-Software verfügbar ist, koordiniert AVS in Zusammenarbeit mit den Kunden ein geeignetes Wartungsfenster für das Upgrade.

## <a name="next-steps"></a>Nächste Schritte

[Sichern von Workload-VMs mit Veeam](backup-workloads-veeam.md)
