---
title: Häufig gestellte Fragen zur Azure Migrate-Servermigration
description: Erhalten Sie Antworten auf häufig gestellte Fragen zur Azure Migrate-Servermigration.
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: bae3447f0fada18de5473e1ef1a1c1d431535f63
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77065997"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure Migrate-Servermigration: Häufig gestellte Fragen

Dieser Artikel beantwortet häufige Fragen zu Azure Migrate: Servermigration. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum). Wenn Sie weitere Fragen haben, lesen Sie die folgenden Artikel:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate.
- [Fragen](common-questions-appliance.md) zur Azure Migrate-Appliance.
- [Fragen](common-questions-discovery-assessment.md) zur Ermittlung, Bewertung und Abhängigkeitsvisualisierung.


## <a name="how-does-agentless-vmware-replication-work"></a>Wie funktioniert die VMware-Replikation ohne Agent?

Die Replikationsmethode ohne Agent für VMware verwendet VMware-Momentaufnahmen und die Nachverfolgung geänderter Blöcke von VMware (Changed Block Tracking, CBT). Ein anfänglicher Replikationszyklus wird geplant, wenn der Benutzer die Replikation startet. Im anfänglichen Replikationszyklus wird eine Momentaufnahme der VM erstellt. Diese wird zum Replizieren der VMDKs (Datenträger) der VM verwendet. Nach Abschluss des anfänglichen Replikationszyklus werden regelmäßige Deltareplikationszyklen geplant. In einem Deltareplikationszyklus wird eine Momentaufnahme erstellt, und diejenigen Datenblöcke, die sich seit dem letzten Replikationszyklus geändert haben, werden repliziert. Mithilfe der VMware-Nachverfolgung geänderter Blöcke werden Blöcke ermittelt, die sich seit dem letzten Zyklus verändert haben.
Die Häufigkeit der regelmäßigen Replikation wird automatisch durch den Dienst verwaltet und richtet sich danach, wie viele andere VMs oder Datenträger gleichzeitig Replikationen aus demselben Datenspeicher durchführen. Unter idealen Bedingungen erfolgt die Replikation schließlich mit einem Zyklus pro Stunde und VM.

Bei der Migration ist ein bedarfsbasierter Replikationszyklus eingeplant, sodass die VM alle verbleibenden Daten erfassen kann. Sie können auswählen, dass die VM im Rahmen der Migration heruntergefahren wird, um jeglichen Datenverlust zu vermeiden und Anwendungskonsistenz sicherzustellen.

## <a name="why-is-the-resynchronization-option-not-exposed-in-agentless-stack"></a>Warum wird die Option zur Neusynchronisierung nicht in einem Stapel ohne Agent verfügbar gemacht?

In einem Stapel ohne Agent wird in jedem Deltazyklus der Unterschied zwischen der aktuellen und der vorherigen Momentaufnahme übertragen. Da immer ein Unterschied zwischen den Momentaufnahmen besteht, bietet dies den Vorteil der Datenfaltung (wenn also ein bestimmter Sektor zwischen den Momentaufnahmen n-mal geschrieben wurde, muss nur der letzte Schreibvorgang übertragen werden, da nur die letzte Synchronisierung von Interesse ist). Dies unterscheidet sich von Agent-basierten Stapeln, in denen jeder Schreibvorgang nachverfolgt und angewendet wird. Das bedeutet, dass jeder Deltazyklus eine Neusynchronisierung ist. Daher wird keine Option zur Neusynchronisierung verfügbar gemacht. 

Wenn Datenträger einmal aufgrund eines Fehlers nicht synchron sind, wird dies im nächsten Zyklus behoben. 

## <a name="what-is-the-impact-of-churn-rate-if-i-use-agentless-replication"></a>Wie wirkt sich die Replikation ohne Agent auf die Änderungsrate aus?

Da der Stapel eher von der Datenfaltung als von der Änderungsrate abhängig ist, ist in diesem Stapel das Änderungsmuster von großer Bedeutung. Ein Muster, bei dem eine Datei immer wieder geschrieben wird, hat keine großen Auswirkungen. Dagegen verursacht ein Muster, bei dem in jeden zweiten Sektor geschrieben wird, im nächsten Zyklus viele Änderungen. Da die zu übertragende Datenmenge minimiert werden soll, sollen die Daten so weit wie möglich gefaltet werden, bevor der nächste Zyklus geplant wird.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Wie häufig wird ein Replikationszyklus geplant?

Replikationszyklen werden gemäß folgender Formel geplant: (Vorherige Zyklusdauer / 2) oder 1 Stunde, je nachdem, welcher Wert höher ist. Beispiel: Wenn ein Deltazyklus für eine VM vier Stunden gedauert hat, wird der nächste Zyklus in zwei Stunden geplant, nicht in der nächsten Stunde. Anders liegt der Fall bei einem Zyklus direkt nach der anfänglichen Replikation – hier wird der erste Deltazyklus direkt geplant.

## <a name="what-is-the-impact-on-performance-of-vcenter-server-or-esxi-host-while-using-agentless-replication"></a>Wie wirkt sich die Replikation ohne Agent auf die Leistung von vCenter Server oder eines ESXi-Hosts aus?

Da bei der Replikation ohne Agent Momentaufnahmen verwendet werden, führt dies zu einer höheren IOPS-Belegung im Speicher, und Kunden benötigen einen gewissen IOPS-Puffer im Speicher. Die Verwendung dieses Stapels wird in Umgebungen mit Einschränkungen in Bezug auf Speicher bzw. IOPS-Leistung nicht empfohlen.

## <a name="does-agentless-migration-stack-support-migration-of-uefi-vms-to-azure-gen-2-vms"></a>Unterstützt der Stapel für die Migration ohne Agent die Migration von UEFI-VMs zu Azure Gen 2-VMs?

Nein, Sie müssen Azure Site Recovery verwenden, um diese VMs zu Azure Gen 2-VMs zu migrieren. 

## <a name="can-i-pin-my-vms-to-azure-availability-zones-when-i-migrate"></a>Kann ich meine VMs beim Migrieren an Azure-Verfügbarkeitszonen anheften?

Nein, Azure-Verfügbarkeitszonen werden nicht unterstützt.

## <a name="which-transport-protocol-is-used-by-azure-migrate-during-replication"></a>Welches Transportprotokoll wird von Azure Migrate bei der Replikation verwendet?

Azure Migrate verwendet das NBD-Protokoll (Network Block Device) mit SSL-Verschlüsselung.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Welche vCenter Server-Version ist für die Migration mindestens erforderlich?

Sie müssen mindestens über vCenter Server 5.5 und die VMware vSphere ESXi-Hostversion 5.5 verfügen.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Können Kunden ihre VMs zu nicht verwalteten Datenträgern migrieren?

Nein. Azure Migrate unterstützt nur die Migration zu verwalteten Datenträgern (HDD Standard, SSD Premium).

## <a name="how-many-vms-can-replicate-simultaneously-using-agentless-vmware-stack"></a>Wie viele VMs können mit dem VMware-Stapel ohne Agent simultan repliziert werden?

Zurzeit können Kunden pro vCenter Server 100 VMs gleichzeitig migrieren. Dies kann in Batches von jeweils 10 VMs erfolgen.




