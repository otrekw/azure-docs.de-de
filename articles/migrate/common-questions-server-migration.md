---
title: Häufig gestellte Fragen zur Azure Migrate-Servermigration
description: Erhalten von Antworten auf häufig gestellte Fragen zum Migrieren von Computern mithilfe der Azure Migrate-Servermigration
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 0c967027457b925b45ea19d994cfadfdbd0b8ab3
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425832"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure Migrate-Servermigration: Häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zum Azure Migrate-Servermigrationstool beantwortet. Wenn Sie nach dem Lesen dieses Artikels weitere Fragen haben, lesen Sie folgende Artikel:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate.
- [Fragen](common-questions-appliance.md) zur Azure Migrate-Appliance.
- [Fragen](common-questions-discovery-assessment.md) zur Ermittlung, Bewertung und Abhängigkeitsvisualisierung.
- Veröffentlichen von Fragen im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum).


## <a name="how-does-agentless-vmware-replication-work"></a>Wie funktioniert die VMware-Replikation ohne Agent?

Die Replikationsmethode ohne Agent für VMware verwendet VMware-Momentaufnahmen und die Nachverfolgung geänderter Blöcke von VMware (Changed Block Tracking, CBT).

1. Wenn Sie die Replikation starten, wird ein erster Replikationszyklus geplant. Im ersten Zyklus wird eine Momentaufnahme des virtuellen Computers erstellt. Diese Momentaufnahme wird verwendet, um die VMDKs (Datenträger) der virtuellen Computer zu replizieren. 
2. Nach Abschluss des anfänglichen Replikationszyklus werden regelmäßige Deltareplikationszyklen geplant.
    - Während der Deltareplikation wird eine Momentaufnahme erstellt, und diejenigen Datenblöcke, die sich seit dem letzten Replikationszyklus geändert haben, werden repliziert.
    - Mithilfe der VMware-Nachverfolgung geänderter Blöcke (CBT) werden Blöcke ermittelt, die sich seit dem letzten Zyklus verändert haben.
    - Die Häufigkeit der regelmäßigen Replikation wird automatisch durch Azure Migrate verwaltet und richtet sich danach, wie viele andere VMs oder Datenträger gleichzeitig vom selben Datenspeicher aus repliziert werden. Unter idealen Bedingungen erreicht die Replikation schließlich einen Zyklus pro Stunde für jede VM.

Bei der Migration ist ein bedarfsbasierter Replikationszyklus eingeplant, sodass der Computer alle verbleibenden Daten erfassen kann. Sie können auswählen, den Computer während der Migration herunterzufahren, um Datenverluste zu vermeiden und die Anwendungskonsistenz sicherzustellen.

## <a name="why-isnt-resynchronization-exposed"></a>Warum wird die erneute Synchronisierung nicht offengelegt?

Bei der Migration ohne Agent wird in jedem Deltazyklus die Differenz zwischen der aktuellen Momentaufnahme und der zuvor erstellten Momentaufnahme geschrieben. Da es sich immer um die Differenz zwischen Momentaufnahmen handelt, werden Daten zusammengelegt. Wenn also ein bestimmter Sektor zwischen den Momentaufnahmen N-mal geschrieben wird, muss nur der letzte Schreibvorgang übertragen werden, da wir nur an der letzten Synchronisierung interessiert sind. Dies unterscheidet sich von der Agent-basierten Replikation, bei der wir jeden Schreibvorgang nachverfolgen und anwenden. Das bedeutet, dass jeder Deltazyklus eine Neusynchronisierung ist. Daher wird keine Option zur Neusynchronisierung verfügbar gemacht. Wenn die Datenträger aufgrund eines Fehlers nicht synchronisiert werden, wird dies im nächsten Zyklus behoben. 

## <a name="how-does-churn-rate-impact-agentless-replication"></a>Wie wirkt sich die Änderungsrate auf die Replikation ohne Agents aus?

Da die Replikation ohne Agent das Datum zusammenlegt, ist das Änderungsmuster wichtiger als die Änderungsrate. Wenn eine Datei immer wieder geschrieben wird, hat die Rate keine große Auswirkung. Dagegen verursacht ein Muster, bei dem in jeden zweiten Sektor geschrieben wird, im nächsten Zyklus viele Änderungen. Da die zu übertragende Datenmenge minimiert werden soll, sollen die Daten so weit wie möglich gefaltet werden, bevor der nächste Zyklus geplant wird.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Wie häufig wird ein Replikationszyklus geplant?

Replikationszyklen werden gemäß folgender Formel geplant: (Vorherige Zyklusdauer / 2) oder 1 Stunde, je nachdem, welcher Wert höher ist.

Beispiel: Wenn ein Deltazyklus für eine VM vier Stunden dauert, wird der nächste Zyklus in zwei Stunden geplant, nicht in der nächsten Stunde. Dies unterscheidet sich unmittelbar nach der ersten Replikation, bei der der erste Deltazyklus sofort geplant wird.

## <a name="how-does-agentless-replication-impact-vmware-servers"></a>Welche Auswirkungen hat die Replikation ohne Agents auf VMware-Server?

Es gibt einige Auswirkungen auf die Leistung von vCenter Server-/ESXi-Hosts. Da die Replikation ohne Agents Momentaufnahmen verwendet, verbraucht sie IOPS im Speicher, und es wird eine gewisse IOPS-Speicherbandbreite benötigt. Wir raten davon ab, die Replikation ohne Agents zu verwenden, wenn es in Ihrer Umgebung Einschränkungen hinsichtlich Speicher/IOPS gibt.

## <a name="can-i-do-agentless-migration-of-uefi-vms-to-azure-gen-2"></a>Kann ich eine Migration ohne Agents von UEFI-VMs zu Azure Gen 2 durchführen?

Nein. Verwenden Sie Azure Site Recovery, um diese VMs zu Azure Gen 2-VMs zu migrieren. 

## <a name="can-i-pin-vms-to-azure-availability-zones-when-i-migrate"></a>Kann ich VMs beim Migrieren an Azure-Verfügbarkeitszonen anheften?

Nein, Azure-Verfügbarkeitszonen werden nicht unterstützt.

## <a name="which-transport-protocol-is-used-by-azure-migrate-during-replication"></a>Welches Transportprotokoll wird von Azure Migrate bei der Replikation verwendet?

Azure Migrate verwendet das NBD-Protokoll (Network Block Device) mit SSL-Verschlüsselung.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Welche vCenter Server-Version ist für die Migration mindestens erforderlich?

Sie müssen mindestens über vCenter Server 5.5 und die VMware vSphere ESXi-Hostversion 5.5 verfügen.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Können Kunden ihre VMs zu nicht verwalteten Datenträgern migrieren?

Nein. Azure Migrate unterstützt nur die Migration zu verwalteten Datenträgern (HDD Standard, SSD Premium).

## <a name="how-many-vms-can-i-replicate-together-with-agentless-migration"></a>Wie viele VMs kann ich zusammen mit der Migration ohne Agents replizieren?

Zurzeit können Sie pro vCenter Server 100 VMs gleichzeitig migrieren. Migrieren Sie in Batches von 10 VMs.
 



