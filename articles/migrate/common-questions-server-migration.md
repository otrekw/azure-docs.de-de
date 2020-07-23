---
title: Häufig gestellte Fragen zur Azure Migrate-Servermigration
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Verwendung der Azure Migrate-Servermigration für das Migrieren von Computern.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 282f7ab27eead59fc87a95ea7d397268177f4f2c
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86224127"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure Migrate-Servermigration: Häufig gestellte Fragen

Dieser Artikel beantwortet häufige Fragen zu Azure Migrate: Servermigration“ kommunizieren kann. Wenn Sie weitere Fragen haben, sehen Sie sich die folgenden Ressourcen an:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate
- Fragen zur [Azure Migrate-Appliance](common-questions-appliance.md)
- Fragen zur [Ermittlung, Bewertung und Abhängigkeitsvisualisierung](common-questions-discovery-assessment.md)
- Antworten auf Fragen im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum)

## <a name="what-geographies-are-supported-for-migration-with-azure-migrate"></a>Welche geografischen Regionen werden für die Migration mit Azure Migrate unterstützt?

In der Unterstützungsmatrix für Azure Migrate finden Sie die unterstützten geografischen Regionen für die [öffentliche Cloud](migrate-support-matrix.md#supported-geographies-public-cloud) und für [Azure Government](migrate-support-matrix.md#supported-geographies-azure-government).

## <a name="how-does-agentless-vmware-replication-work"></a>Wie funktioniert die VMware-Replikation ohne Agent?

Die Replikationsmethode ohne Agent für VMware verwendet VMware-Momentaufnahmen und die Nachverfolgung geänderter Blöcke von VMware (Changed Block Tracking, CBT).

Der Prozess sieht wie folgt aus:

1. Wenn Sie die Replikation starten, wird ein erster Replikationszyklus geplant. Im ersten Zyklus wird eine Momentaufnahme des virtuellen Computers erstellt. Die Momentaufnahme wird verwendet, um die VMDKs (Datenträger) der virtuellen Computer zu replizieren. 
2. Nach Abschluss des anfänglichen Replikationszyklus werden regelmäßige Deltareplikationszyklen geplant.
    - Während der Deltareplikation wird eine Momentaufnahme erstellt, und diejenigen Datenblöcke, die sich seit dem letzten Replikationszyklus geändert haben, werden repliziert.
    - Mithilfe der VMware-Nachverfolgung geänderter Blöcke (CBT) werden Blöcke ermittelt, die sich seit dem letzten Zyklus verändert haben.
    - Die Häufigkeit der regelmäßigen Replikation wird automatisch durch Azure Migrate verwaltet und richtet sich danach, wie viele andere VMs und Datenträger gleichzeitig vom selben Datenspeicher aus repliziert werden. Unter idealen Bedingungen erreicht die Replikation schließlich einen Zyklus pro Stunde für jede VM.

Bei der Migration ist ein bedarfsbasierter Replikationszyklus eingeplant, sodass der Computer alle verbleibenden Daten erfassen kann. Um Datenverluste zu vermeiden und Anwendungskonsistenz sicherzustellen, können Sie den Computer während der Migration herunterfahren.

## <a name="why-isnt-resynchronization-exposed"></a>Warum wird die erneute Synchronisierung nicht offengelegt?

Bei der Migration ohne Agent wird in jedem Deltazyklus die Differenz zwischen der aktuellen Momentaufnahme und der zuvor erstellten Momentaufnahme geschrieben. Daten werden immer nach der Differenz zwischen Momentaufnahmen zusammengelegt. Wenn ein bestimmter Sektor zwischen den Momentaufnahmen *N*-mal geschrieben wird, muss nur der letzte Schreibvorgang übertragen werden, da wir nur an der letzten Synchronisierung interessiert sind. Das Verfahren unterscheidet sich von der Agent-basierten Replikation, bei der wir jeden Schreibvorgang nachverfolgen und anwenden. Bei diesem Verfahren ist jeder Deltazyklus eine Neusynchronisierung. Daher wird keine Option zur Neusynchronisierung verfügbar gemacht. Wenn die Datenträger aufgrund eines Fehlers einmal nicht synchronisiert werden, wird dies im nächsten Zyklus behoben. 

## <a name="how-does-churn-rate-affect-agentless-replication"></a>Wie wirkt sich die Änderungsrate auf die Replikation ohne Agent aus?

Da bei der Replikation ohne Agent Daten zusammengelegt werden, ist das *Änderungsmuster* wichtiger als die *Änderungsrate*. Wenn eine Datei immer wieder geschrieben wird, hat die Rate keine große Auswirkung. Dagegen verursacht ein Muster, bei dem in jeden zweiten Sektor geschrieben wird, im nächsten Zyklus viele Änderungen. Da die zu übertragende Datenmenge minimiert werden soll, sollen die Daten so weit wie möglich zusammengelegt werden, bevor der nächste Zyklus geplant wird.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Wie häufig wird ein Replikationszyklus geplant?

Die Formel zur Planung des nächsten Replikationszyklus ist (Vorherige Zyklusdauer / 2) oder eine Stunde, je nachdem, welcher Wert höher ist.

Beispiel: Wenn ein Deltazyklus für eine VM vier Stunden dauert, wird der nächste Zyklus in zwei Stunden geplant, nicht in der nächsten Stunde. Das Verfahren unterscheidet sich unmittelbar nach der ersten Replikation, wenn sofort der erste Deltazyklus geplant wird.

## <a name="how-does-agentless-replication-affect-vmware-servers"></a>Welche Auswirkungen hat die Replikation ohne Agent auf VMware-Server?

Die Replikation ohne Agent hat einige Auswirkungen auf die Leistung von VMware vCenter Server- und VMware ESXi-Hosts. Da die Replikation ohne Agent Momentaufnahmen verwendet, verbraucht sie IOPS im Speicher, sodass eine gewisse IOPS-Speicherbandbreite erforderlich ist. Wir raten davon ab, die Replikation ohne Agent zu verwenden, wenn es in Ihrer Umgebung Einschränkungen hinsichtlich Speicher oder IOPS gibt.

## <a name="can-i-do-agentless-migration-of-uefi-vms-to-azure-gen-2"></a>Kann ich eine Migration ohne Agents von UEFI-VMs zu Azure Gen 2 durchführen?

Nein. Verwenden Sie Azure Site Recovery, um diese VMs zu Azure Gen 2-VMs zu migrieren. 

## <a name="can-i-pin-vms-to-azure-availability-zones-when-i-migrate"></a>Kann ich VMs beim Migrieren an Azure-Verfügbarkeitszonen anheften?

Nein. Azure-Verfügbarkeitszonen werden für die Azure Migrate-Migration nicht unterstützt.

## <a name="what-transport-protocol-does-azure-migrate-use-during-replication"></a>Welches Transportprotokoll verwendet Azure Migrate bei der Replikation?

Azure Migrate verwendet das NBD-Protokoll (Network Block Device) mit TLS-Verschlüsselung.

## <a name="how-is-the-data-transmitted-from-on-prem-environment-to-azure-is-it-encrypted-before-transmission"></a>Wie werden die Daten von der lokalen Umgebung zu Azure übertragen? Werden sie vor der Übertragung verschlüsselt? 
Die Azure Migrate-Appliance komprimiert Daten bei der Replikation ohne Agent und verschlüsselt sie vor dem Hochladen. Daten werden über einen sicheren Kommunikationskanal über HTTPS übertragen und verwenden TLS 1.2 oder höher. Zudem werden Ihre Daten von Azure Storage beim Speichern in der Cloud automatisch verschlüsselt (Verschlüsselung ruhender Daten).  

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Welche vCenter Server-Version ist für die Migration mindestens erforderlich?

Sie müssen mindestens über vCenter Server 5.5 und die vSphere ESXi-Hostversion 5.5 verfügen.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Können Kunden ihre VMs zu nicht verwalteten Datenträgern migrieren?

Nein. Azure Migrate unterstützt nur die Migration zu verwalteten Datenträgern (HDD Standard, SSD Premium).

## <a name="how-many-vms-can-i-replicate-at-one-time-by-using-agentless-migration"></a>Wie viele VMs kann ich gleichzeitig mithilfe der Migration ohne Agent replizieren?

Zurzeit können Sie pro vCenter Server-Instanz 100 VMs gleichzeitig migrieren. Migrieren Sie in Batches von 10 VMs.

## <a name="how-do-i-throttle-replication-in-using-azure-migrate-appliance-for-agentless-vmware-replication"></a>Wie kann ich die Replikation mithilfe der Azure Migrate-Appliance für die VMware-Replikation ohne Agent drosseln?  

Sie können die Replikation mithilfe von NetQosPolicy drosseln. Beispiel:

Das in der NetQosPolicy zu verwendende AppNamePrefix ist „GatewayWindowsService“. Sie können eine Richtlinie auf der Azure Migrate-Appliance erstellen, um den Replikationsdatenverkehr von der Appliance zu drosseln, indem Sie eine Richtlinie wie die folgende erstellen:
 
New-NetQosPolicy -Name "ThrottleReplication" -AppPathNameMatchCondition "GatewayWindowsService.exe" -ThrottleRateActionBitsPerSecond 1MB

## <a name="can-i-migrate-vms-that-are-already-being-replicated-to-azure"></a>Kann ich virtuelle Computer, die bereits repliziert werden, nach Azure migrieren? 

Wenn virtuelle Computer bereits auf andere Weise nach Azure repliziert werden, können Sie diese Computer nicht als virtuelle Computer mit Azure Migrate-Servermigration migrieren. Als Problemumgehung können Sie die virtuellen Computer wie physische Server behandeln und sie gemäß [unterstützter physischer Servermigration](migrate-support-matrix-physical-migration.md) migrieren.

## <a name="when-do-i-migrate-machines-as-physical-servers"></a>Wann migriere ich Computer als physische Server?

Die Behandlung von Computern als physische Server für die Migration ist in verschiedenen Szenarien hilfreich:

- Wenn Sie lokale physische Server migrieren.
- Wenn Sie VMs migrieren, die über eine Plattform, z. B. Xen oder KVM, virtualisiert wurden.
- Zum Migrieren von Hyper-V- oder VMware-VMs, falls Sie aus irgendeinem Grund den Standardmigrationsprozess für [Hyper-V](tutorial-migrate-hyper-v.md) oder die [VMware](server-migrate-overview.md)-Migration nicht nutzen können. Wenn Sie beispielsweise VMware vCenter nicht ausführen und nur ESXi-Hosts verwenden.
- Zum Migrieren von VMs, die zurzeit in privaten Clouds ausgeführt werden, zu Azure.
- Wenn Sie VMs, die in öffentlichen Clouds ausgeführt werden, z. B. Amazon Web Services (AWS) oder Google Cloud Platform (GCP), zu Azure migrieren möchten.

## <a name="i-deployed-two-or-more-appliances-to-discover-vms-in-my-vcenter-server-however-when-i-try-to-migrate-the-vms-i-only-see-vms-corresponding-to-one-of-the-appliance"></a>Ich habe zwei (oder mehr) Appliances zum Entdecken von VMs auf meiner vCenter Server-Instanz bereitgestellt. Wenn ich versuche, die VMs zu migrieren, werden mir aber nur die VMs für eine der Appliances angezeigt.

Dies ist möglicherweise ein guter Anwendungsfall, wird aber derzeit nicht unterstützt. Die Bereitstellung von zwei (oder mehr) Appliances zum Entdecken der gleichen Gruppe aus VMs führ zu einem Dienstproblem, bei dem der VM-Besitz ständig zwischen den Appliances wechselt. Dies ist der Grund dafür, dass VMs scheinbar auftauchen und wieder verschwinden. Um dieses Problem zu lösen, müssen Sie eine Appliance löschen und eine Aktualisierung mit Leerung des Caches durchführen.

## <a name="do-i-need-vmware-vcenter-to-migrate-vmware-vms"></a>Benötige ich VMware vCenter, um VMware-VMs zu migrieren?
Zum [Migrieren von VMware-VMs](server-migrate-overview.md) mithilfe der VMware-Migration mit oder ohne Agent müssen ESXi-Hosts, auf denen sich VMs befinden, von vCenter Server verwaltet werden. Wenn Sie nicht über vCenter Server verfügen, können Sie VMware-VMs migrieren, indem Sie sie als physische Server migrieren. [Weitere Informationen](migrate-support-matrix-physical-migration.md)
 
## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.
