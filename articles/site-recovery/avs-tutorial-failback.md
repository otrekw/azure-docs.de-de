---
title: Failback für Azure VMware Solution über Azure mit Azure Site Recovery
description: Hier erfahren Sie, wie Sie nach einem Failover zu Azure während der Notfallwiederherstellung einen Failback auf die private Azure VMware Solution-Cloud durchführen.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: fb14e647d3444f2f0d0cb86901f93582a18848f5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91814230"
---
# <a name="fail-back-vms-to-azure-vmware-solution-private-cloud"></a>Failback für VMs auf die private Azure VMware Solution-Cloud

In diesem Artikel wird beschrieben, wie Sie Failbacks für Azure-VMs (virtuelle Computer) auf eine private Azure VMware Solution-Cloud durchführen, nachdem ein [Failover](avs-tutorial-failover.md) für Azure VMware Solution-VMs auf Azure mithilfe von [Azure Site Recovery](site-recovery-overview.md) durchgeführt wurde. Nach einem Failback aktivieren Sie die Replikation, damit die Azure VMware Solution-VMs in Azure repliziert werden.

## <a name="before-you-start"></a>Vorbereitung

1. Informieren Sie sich über das [VMware-Failback](failover-failback-overview.md#vmwarephysical-reprotectionfailback). 
2. Vergewissern Sie sich, dass Sie die Schritte zum [Vorbereiten des Failbacks](vmware-azure-prepare-failback.md) durchgelesen und durchgeführt haben sowie alle erforderlichen Komponenten bereitgestellt wurden. Zu den Komponenten des Failbacks gehören ein Prozessserver in Azure, ein Masterzielserver und eine Site-to-Site-VPN-Verbindung (oder privates ExpressRoute-Peering).
3. Vergewissern Sie sich, dass die [Anforderungen](avs-tutorial-reprotect.md#before-you-begin) für den erneuten Schutz und das Failback erfüllt sind und Sie das [erneute Schützen von virtuellen Azure-Computern aktiviert](avs-tutorial-reprotect.md#enable-reprotection) haben, sodass diese von Azure in der privaten Azure VMware Solution-Cloud repliziert werden. Virtuelle Computer müssen sich in einem replizierten Zustand befinden, damit ein Failback möglich ist.




## <a name="run-a-failover-to-fail-back"></a>Ausführen eines Failovers für ein Failback

1. Stellen Sie sicher, dass die Azure-VMs wieder geschützt und in die private Azure VMware Solution-Cloud repliziert werden.
    - Ein virtueller Computer benötigt mindestens einen Wiederherstellungspunkt, damit ein Failback möglich ist.
    - Wenn Sie ein Failback für einen Wiederherstellungsplan durchführen, müssen alle Computer im Plan über mindestens einen Wiederherstellungspunkt verfügen.
2. Wählen Sie im Tresor **Replizierte Elemente** und dann die VM aus. Klicken Sie mit der rechten Maustaste auf die VM und wählen Sie **Nicht geplantes Failover** aus.
3. Überprüfen Sie in **Failover bestätigen** die Failoverrichtung (von Azure).
4. Wählen Sie den Wiederherstellungspunkt aus, den Sie für das Failover verwenden möchten.
    - Es wird empfohlen, den **neuesten** Wiederherstellungspunkt zu verwenden. Der App-konsistente Punkt liegt vor dem aktuellen Zeitpunkt und verursacht einige Datenverluste.
    - Der **neueste** Wiederherstellungspunkt ist ein absturzkonsistenter Wiederherstellungspunkt.
    - Bei Auswahl des **neuesten** Wiederherstellungspunkts wird ein Failover der VM zum letzten verfügbaren Zeitpunkt durchgeführt. Falls in einem Wiederherstellungsplan eine Replikationsgruppe für Multi-VM-Konsistenz enthalten ist, wird für jede VM in der Gruppe ein Failover zum letzten unabhängigen Zeitpunkt durchgeführt.
    - Wenn Sie einen App-konsistenten Wiederherstellungspunkt verwenden, wird für jede VM ein Failover zum letzten verfügbaren Punkt durchgeführt. Wenn ein Wiederherstellungsplan eine Replikationsgruppe aufweist, erfolgt die Wiederherstellung für jede Replikationsgruppe zum allgemein verfügbaren Wiederherstellungspunkt.
5. Das Failover beginnt. Site Recovery fährt die virtuellen Azure-Computer herunter.
6. Überprüfen Sie nach Abschluss des Failovers, ob alles wie erwartet funktioniert. Vergewissern Sie sich, dass die Azure VMs heruntergefahren wurden. 
7. Klicken Sie nach der Überprüfung mit der rechten Maustaste auf die VM und wählen Sie **Commit** aus, um den Failover-Prozess fertig zu stellen. Durch das Committen wird die Azure-VM, für die das Failover durchgeführt wurde, entfernt. 

> [!NOTE]
> Bei Windows-VMs deaktiviert Site Recovery die VMware-Tools während des Failovers. Beim Failback der Windows-VM werden die VMware-Tools wieder aktiviert. 




## <a name="reprotect-from-azure-vmware-solution-to-azure"></a>Erneutes Schützen von Azure VMware Solution in Azure

Nach dem Committen des Failbacks werden die Azure VMs gelöscht. Die VM befindet sich in der privaten Azure VMware Solution-Cloud, aber sie wird nicht geschützt. Gehen Sie folgendermaßen vor, um die Replikation der VMs nach Azure erneut zu starten:

1. Wählen Sie im Tresor **Replizierte Elemente**, dann die VMs, für die das Failback durchgeführt wurde, und anschließend **Erneut schützen** aus.
2. Geben Sie den Prozessserver an, der zum Senden von Daten zurück an Azure verwendet wird.
3. Wählen Sie **OK** aus, um den Auftrag zum erneuten Schützen zu starten.

> [!NOTE]
> Nachdem eine Azure VMware Solution-VM gestartet wurde, dauert es 15 Minuten, bis der Agent wieder beim Konfigurationsserver registriert wurde. Während dieses Zeitraums schlägt das erneute Schützen fehl, und eine zurückgegebene Fehlermeldung gibt an, dass der Agent nicht installiert ist. Warten Sie in diesem Fall einige Minuten, und führen Sie dann das erneute Schützen durch.

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss des Auftrags zum erneuten Schützen wird die Azure VMware Solution-VM in Azure repliziert. Bei Bedarf können Sie [ein weiteres Failover](avs-tutorial-failover.md) zu Azure ausführen.

