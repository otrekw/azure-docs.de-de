---
title: Failback von virtuellen Hyper-V-Computern aus Azure mit Azure Site Recovery
description: Erfahren Sie, wie Sie mit Azure Site Recovery ein Failback für Hyper-V-VMs aus Azure zum lokalen Standort durchführen.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 09/12/2019
ms.author: rajanaki
ms.openlocfilehash: 4b005ae308576db6fd26fcf079161430b266ec3f
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75498183"
---
# <a name="run-a-failback-for-hyper-v-vms"></a>Durchführen eines Failbacks für virtuelle Hyper-V-Computer

In diesem Artikel wird beschrieben, wie Sie mit [Azure Site Recovery](site-recovery-overview.md) ein Failback für virtuelle Azure-Computer durchführen, die nach einem Failover von Hyper-V-VMs von einem lokalen Standort zu Azure erstellt wurden.

- Sie führen ein Failback für Hyper-V-VMs von Azure aus, indem Sie ein geplantes Failover von Azure zum lokalen Standort ausführen. Wenn das Failover in der Richtung von Azure zu lokal stattfindet, handelt es sich um ein Failback.
- Da Azure eine hoch verfügbare Umgebung ist und VMs immer verfügbar sind, stellt das Failback von Azure eine geplante Aktivität dar. Sie können eine kleine Ausfallzeit einplanen, damit Workloads lokal neu gestartet werden können. 
- Durch ein geplantes Failback werden die virtuellen Computer in Azure ausgeschaltet und die neuesten Änderungen heruntergeladen. Dabei wird kein Datenverlust erwartet.

## <a name="before-you-start"></a>Vorbereitung

1. [Überprüfen Sie die Typen der Failbacks](failover-failback-overview.md#hyper-v-reprotectionfailback), die Sie verwenden können: Wiederherstellung am ursprünglichen Standort und Wiederherstellung an einem anderen Standort.
2. Stellen Sie sicher, dass die Azure-VMs ein Speicherkonto und keine verwalteten Datenträger verwenden. Ein Failback von virtuellen Hyper-V-Computern, die über verwaltete Datenträger repliziert werden, wird nicht unterstützt.
3. Überprüfen Sie, ob der lokale Hyper-V-Host (oder System Center VMM-Server bei Verwendung von Site Recovery) ausgeführt wird und mit Azure verbunden ist. 
4. Stellen Sie sicher, dass für die VMs alle Failover und Commits abgeschlossen wurden. Sie müssen keine spezifischen Site Recovery-Komponenten für das Failback von Hyper-V-VMs aus Azure einrichten.
5. Die Zeit zum Durchführen der Datensynchronisierung und zum Starten der lokalen VM ist von einer Reihe von Faktoren abhängig. Um das Herunterladen von Daten zu beschleunigen, können Sie den Microsoft Azure Recovery Services-Agent konfigurieren, sodass mehr Threads verwendet und der Download parallelisiert wird. [Weitere Informationen](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage)


## <a name="fail-back-to-the-original-location"></a>Failback zum ursprünglichen Speicherort

Führen Sie wie folgt ein geplantes Failover von Azure zum lokalen Standort aus, um ein Failback für virtuelle Hyper-V-Computer in Azure zur ursprünglichen lokalen VM durchzuführen:

1. Wählen Sie im Tresor unter **Replizierte Elemente** die VM aus. Klicken Sie mit der rechten Maustaste auf den virtuellen Computer und dann auf **Geplantes Failover**. Wenn Sie ein Failback für einen Wiederherstellungsplan ausführen, wählen Sie den Plannamen aus, und klicken Sie auf **Failover** > **Geplantes Failover**.
2. Wählen Sie unter **Geplantes Failover bestätigen** den Quell- und Zielort aus. Beachten Sie die Failover-Richtung. Wenn das Failover vom primären Standort erwartungsgemäß funktioniert hat und sich alle virtuellen Computer am sekundären Standort befinden, dient diese Angabe nur zu Informationszwecken.
3. Wählen Sie in **Datensynchronisierung** eine Option aus:
    - **Daten vor dem Failover synchronisieren (nur Deltaänderungen synchronisieren):** Diese Option minimiert die Ausfallzeiten der virtuellen Computer, da diese für die Synchronisierung nicht heruntergefahren werden.
        - **Phase 1:** Eine Momentaufnahme der Azure-VM wird erstellt und auf den lokalen Hyper-V-Host kopiert. Der Computer wird weiterhin in Azure ausgeführt.
        - **Phase 2:** Die Azure-VM wird heruntergefahren, sodass dort keine neuen Änderungen mehr auftreten können. Die letzten Deltaänderungen werden an den lokalen Server übertragen, und der lokale virtuelle Computer wird gestartet.
    - **Daten nur während Failover synchronisieren (vollständiger Download):** Diese Option ist schneller, da davon ausgegangen wird, dass sich der größte Teil des Datenträgers geändert hat und keine Zeit für die Berechnung von Prüfsummen aufgewendet werden soll. Mit dieser Option werden keine Prüfsummenberechnungen durchgeführt.
        - Mit der Option wird ein Download des Datenträgers durchgeführt. 
        - Es wird empfohlen, diese Option zu verwenden, wenn Sie Azure bereits seit einer Weile nutzen (mindestens seit einem Monat) oder wenn der lokale virtuelle Computer gelöscht wurde.

4. Für VMM gilt: Wenn die Datenverschlüsselung für die Cloud aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen der Anbieterinstallation auf dem VMM-Server ausgestellt wurde.
5. Initiieren Sie das Failover. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt.
6. Falls Sie die Option ausgewählt haben, mit der die Daten vor dem Failover synchronisiert werden, klicken Sie unter **Aufträge** auf „Auftragsname“ und wählen dann die Option **Failover abschließen** aus, sobald die erste Datensynchronisierung abgeschlossen ist und Sie zum Herunterfahren der virtuellen Computer in Azure bereit sind. Die folgenden Schritte werden ausgeführt:
    - Der Azure-Computer wird heruntergefahren.
    - Die neuesten Änderungen werden an die lokale VM übertragen.
    - Die lokale VM wird gestartet.
7. Sie können sich jetzt auf der lokalen VM anmelden, um zu überprüfen, ob diese wie erwartet verfügbar ist.
8. Der Status des virtuellen Computers weist darauf hin, dass ein Commit aussteht. Klicken Sie auf **Commit**, um ein Commit für das Failover auszuführen.
9. Klicken Sie zum Abschließen des Failbacks und zum Starten der erneuten Replikation des lokalen virtuellen Computers in Azure auf **Umgekehrt replizieren**.



## <a name="fail-back-to-an-alternate-location"></a>Failback zu einem alternativen Speicherort 

Ein Failback zu einem alternativen Speicherort wird wie folgt ausgeführt:

1. Wenn Sie neue Hardware einrichten möchten, installieren Sie eine [unterstützte Windows-Version](hyper-v-azure-support-matrix.md#replicated-vms) und die Hyper-V-Rolle auf dem Computer.
2. Erstellen Sie einen virtuellen Netzwerkswitch mit dem gleichen Namen wie auf dem ursprünglichen Server.
3. Wählen Sie unter **Geschützte Elemente** > **Schutzgruppe** > \<NameDerSchutzgruppe> den \<NamenDesVirtuellenComputers> und dann die Option **Geplantes Failover** aus.
4. Wählen Sie unter **Geplantes Failover bestätigen** die Option **Lokalen virtuellen Computer erstellen, wenn nicht vorhanden** aus.
5. Wählen Sie unter **Hostname** den neuen Hyper-V-Hostserver aus, auf dem sich der virtuelle Computer befinden soll.
6. Für die **Datensynchronisierung** empfiehlt sich die Option zum Synchronisieren der Daten vor dem Failover. Diese Option minimiert die Ausfallzeiten der virtuellen Computer, da diese für die Synchronisierung nicht heruntergefahren werden. Sie führt die folgenden Aktionen aus:
    - **Phase 1:** Eine Momentaufnahme der Azure-VM wird erstellt und auf den lokalen Hyper-V-Host kopiert. Der Computer wird weiterhin in Azure ausgeführt.
    - **Phase 2:** Die Azure-VM wird heruntergefahren, sodass dort keine neuen Änderungen mehr auftreten können. Die letzten Änderungen werden an den lokalen Server übertragen, und der lokale virtuelle Computer wird gestartet.
    
7. Klicken Sie auf das Häkchen, um das Failover (Failback) zu starten.
8. Klicken Sie auf **Aufträge** > \<Geplanter Failoverauftrag> > **Failover abschließen**, wenn die anfängliche Synchronisierung beendet ist und Sie die Azure-VM herunterfahren können. Dadurch wird der Azure-Computer heruntergefahren, die neuesten Änderungen werden an den lokalen virtuellen Computer übertragen, und der lokale virtuelle Computer wird gestartet.
9. Sie können sich auf dem lokalen virtuellen Computer anmelden und sich vergewissern, dass alles wie erwartet funktioniert.
10. Klicken Sie auf **Commit**, um das Failover abzuschließen. Durch den Commit werden die Azure-VM und ihre Datenträger gelöscht und die lokale VM für den erneuten Schutz vorbereitet.
10. Klicken Sie zum Starten der Replikation des lokalen virtuellen Computers in Azure auf **Umgekehrt replizieren**. Nur die Deltaänderungen seit dem Ausschalten der VM in Azure werden repliziert.

    > [!NOTE]
    > Wenn Sie den Failbackauftrag während der Datensynchronisierung abbrechen, wird der lokale virtuelle Computer als beschädigt angezeigt. Das liegt daran, dass bei der Datensynchronisierung die aktuellen Daten von den Datenträgern der Azure-VM auf die lokalen Datenträger kopiert werden. Bis zum Abschluss der Synchronisierung befinden sich die Datenträgerdaten unter Umständen nicht in einem konsistenten Zustand. Wenn die lokale VM nach dem Abbruch der Datensynchronisierung gestartet wird, ist das Starten unter Umständen nicht möglich. Führen Sie in diesem Fall das Failover erneut aus, um die Datensynchronisierung abzuschließen.


## <a name="next-steps"></a>Nächste Schritte
Wenn die lokale VM in Azure repliziert wird, können Sie bei Bedarf [ein weiteres Failover](site-recovery-failover.md) in Azure ausführen.
