---
title: Plattformupdates für Azure VMware Solution
description: Erfahren Sie mehr über die Plattformupdates für Azure VMware Solution.
ms.topic: reference
ms.date: 03/24/2021
ms.openlocfilehash: da6317d49edd3f40e1a8f2518f91fe353bbae285
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105045211"
---
# <a name="platform-updates-for-azure-vmware-solution"></a>Plattformupdates für Azure VMware Solution

Die Azure VMware Solution wird ab März 2021 wichtige Aktualisierungen anwenden. Sie erhalten eine Benachrichtigung über Azure Service Health, die den Zeitplan der Wartung enthält. Weitere Informationen finden Sie unter [Aktualisierungen und Upgrades](concepts-upgrades.md) der Azure VMware Solution Private Cloud.

## <a name="march-24-2021"></a>24. März 2021
Alle neuen Azure VMware Solution Private Clouds werden mit VMware vCenter Version 6.7U3l und NSX-T Version 3.1.1 implementiert. Alle vorhandenen privaten Clouds werden aktualisiert und bis **Juni 2021** auf die oben erwähnten Freigaben aktualisiert.

Sie erhalten eine E-Mail mit dem Datum und der Uhrzeit der geplanten Wartung. Sie können ein Upgrade erneut planen. Die E-Mail enthält auch Details über die aktualisierte Komponente, ihre Auswirkungen auf Arbeitslasten, den Zugriff auf die Private Cloud und andere Azure-Dienste.  Eine Stunde vor dem Upgrade erhalten Sie eine Benachrichtigung und dann erneut, sobald Sie abgeschlossen ist.

## <a name="march-15-2021"></a>15. März 2021 

- Der Azure VMware Solution Service wird **bis zum 19. März 2021** Wartungsarbeiten durchführen, um den vCenter-Server in Ihrer privaten Cloud auf die Version vCenter Server 6.7 Update 3l zu aktualisieren.

- Das VMware vCenter ist während dieses Zeitraums nicht verfügbar.  Sie werden also nicht in der Lage sein, Ihre VMs zu verwalten (stoppen, starten, erstellen, löschen) oder die private Cloud zu skalieren (Hinzufügen/Entfernen von Servern und Clustern). Die VMware High Availability (HA) wird jedoch weiterhin betrieben, um bestehende VMs zu schützen. 
 
Weitere Informationen zu dieser vCenter-Version finden Sie in den [Versionshinweisen zu VMware vCenter Server 6.7 Update 3l](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/vsphere-vcenter-server-67u3l-release-notes.html).

## <a name="march-4-2021"></a>4\. März 2021

- Die Azure VMware Solution wendet das [VMware ESXi 6.7, Patch Release ESXi670-202011002](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/esxi670-202011002.html) **bis zum 15. März 2021** auf bestehende Privates an.

- Dokumentierte Workarounds für den vSphere-Stapel gemäß [VMSA-2021-0002](https://www.vmware.com/security/advisories/VMSA-2021-0002.html) werden ebenfalls bis zum **15. März 2021 angewendet**.

>[!NOTE]
>Diese Updates werden unterbrechungsfrei durchgeführt und sollten sich nicht auf Dienste oder Workloads in Azure VMware auswirken. Während der Wartung werden verschiedene VMware-Warnungen, z. B. _Lost network connectivity on DVPorts_ (Netzwerkverbindung für DVPorts getrennt) und _Lost uplink redundancy on DVPorts_ (Uplinkredundanz für DVPorts nicht mehr vorhanden), in vCenter angezeigt und mit Fortschreiten der Wartung automatisch gelöscht.

## <a name="post-update"></a>Nach dem Update
Nach Abschluss der Updates werden neuere Versionen der VMware-Komponenten angezeigt. Wenn Probleme auftreten oder Sie Fragen haben, erstellen Sie ein Supportticket, um unser Supportteam zu kontaktieren.