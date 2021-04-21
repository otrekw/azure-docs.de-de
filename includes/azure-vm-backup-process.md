---
author: v-amallick
ms.service: Azure Backup
ms.topic: include
ms.date: 04/16/2021
ms.author: v-amallick
ms.openlocfilehash: e7af0819a94661a1008d96b7d0738c30a4a80c18
ms.sourcegitcommit: 79c9c95e8a267abc677c8f3272cb9d7f9673a3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107716743"
---
1. Für Azure-VMs, die zur Sicherung ausgewählt sind, startet Azure Backup einen Sicherungsauftrag gemäß dem von Ihnen angegebenen Sicherungszeitplan.
1. Während der ersten Sicherung wird auf dem virtuellen Computer eine Sicherungserweiterung installiert, wenn die VM ausgeführt wird.
    - Für virtuelle Windows-Computer wird die [Erweiterung VMSnapshot](../articles/virtual-machines/extensions/vmsnapshot-windows.md) installiert.
    - Für virtuelle Linux-Computer wird die [Erweiterung VMSnapshotLinux](../articles/virtual-machines/extensions/vmsnapshot-linux.md) installiert.
1. Bei ausgeführten Windows-VMs erstellt Azure Backup in Koordination mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS) von Windows eine App-konsistente Momentaufnahme des virtuellen Computers.
    - Backup erstellt standardmäßig vollständige VSS-Sicherungen.
    - Wenn Backup keine App-konsistente Momentaufnahme erstellen kann, wird eine dateikonsistente Momentaufnahme des zugrunde liegenden Speichers erstellt (weil keine Schreibvorgänge der Anwendung stattfinden, solange die VM beendet ist).
1. Für virtuelle Linux-Computer erstellt Backup eine dateikonsistente Sicherung. Zur Erstellung App-konsistenter Momentaufnahmen müssen Sie Pre- und Postskripts manuell anpassen.
1. Nachdem Backup die Momentaufnahme erstellt hat, werden die Daten in den Tresor übertragen.
    - Zur Optimierung der Sicherung werden die einzelnen VM-Datenträger parallel gesichert.
    - Für jeden Datenträger, der gesichert wird, liest Azure Backup die Blöcke auf dem Datenträger und identifiziert und überträgt nur die Datenblöcke, die sich seit der vorherigen Sicherung geändert haben (das Delta).
    - Momentaufnahmedaten werden möglicherweise nicht sofort in den Tresor kopiert. Zu Spitzenzeiten vergehen unter Umständen mehrere Stunden. Bei täglichen Sicherungsrichtlinien beträgt die Gesamtdauer der Sicherung eines virtuellen Computers weniger als 24 Stunden.
1. An einem virtuellen Windows-Computer werden folgende Änderungen vorgenommen, nachdem Azure Backup darauf aktiviert wurde:
    - Microsoft Visual C++ 2013 Redistributable(x64) – 12.0.40660 wird auf dem virtuellen Computer installiert.
    - Der Starttyp des Volumeschattenkopie-Diensts (Volume Shadow Copy Service, VSS) wird von „automatisch“ in „manuell“ geändert.
    - Der IaaSVmProvider-Windows-Dienst wird hinzugefügt.

1. Wenn die Datenübertragung abgeschlossen ist, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.

![Architektur der Sicherung von virtuellen Azure-Computern](../articles/backup/media/backup-azure-vms-introduction/vmbackup-architecture.png)
