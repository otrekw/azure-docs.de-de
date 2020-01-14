---
title: Einrichten des Failovers auf Azure für Hyper-V-VMs in Azure Site Recovery
description: Hier erfahren Sie, wie Sie mit Azure Site Recovery das Failover auf Azure für Hyper-V-VMs einrichten.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/16/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 03826abf6da94859c510f4c127dfce035aa79370
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75498160"
---
# <a name="fail-over-hyper-v-vms-to-azure"></a>Ausführen eines Failovers auf Azure für virtuelle Hyper-V-Computer

In diesem Tutorial erfahren Sie, wie Sie mit [Azure Site Recovery](site-recovery-overview.md) das Failover auf Azure für Hyper-V-VMs einrichten. Nach dem Failover erfolgt ein Failback zum lokalen Standort, wenn er verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Hyper-V-VM-Eigenschaften zum Feststellen der Konformität mit Azure-Anforderungen
> * Ausführen eines Failovers auf Azure für bestimmte VMs


Dies ist das fünfte Tutorial in einer Reihe. Es wird davon ausgegangen, dass Sie bereits die Aufgaben in den vorherigen Tutorials durchgearbeitet haben.    

1. [Vorbereiten von Azure](tutorial-prepare-azure.md)
2. [Vorbereiten lokaler Hyper-V-Instanzen](tutorial-prepare-on-premises-hyper-v.md)
3. Einrichten der Notfallwiederherstellung für [Hyper-V-VMs](tutorial-hyper-v-to-azure.md) oder [in System Center VMM-Clouds verwaltete Hyper-V-VMs](tutorial-hyper-v-vmm-to-azure.md)
4. [Durchführen eines Notfallwiederherstellungsverfahrens](tutorial-dr-drill-azure.md)

[Erfahren Sie mehr](failover-failback-overview.md#types-of-failover) über verschiedene Failovertypen. Wenn Sie ein Failover für mehrere VMs in einem Wiederherstellungsplan ausführen möchten, lesen Sie [diesen Artikel](site-recovery-failover.md).

## <a name="prepare-for-failover"></a>Vorbereiten des Failovers 
Stellen Sie sicher, dass auf dem virtuellen Computer keine Momentaufnahmen vorhanden sind und dass die lokalen virtuellen Computer während des Failbacks deaktiviert sind. Dies hilft, die Konsistenz der Daten während der Replikation sicherzustellen. Aktivieren Sie während des Failbacks keine lokalen virtuellen Computer. 

Failover und Failback weisen drei Phasen auf:

1. **Failover zu Azure**: Failover von Hyper-V-VMs vom lokalen Standort zu Azure.
2. **Lokales Failover**: Failover von Azure-VMs an Ihren lokalen Standort, wenn dieser verfügbar ist. Hierbei wird mit dem Synchronisieren der Daten aus Azure am lokalen Speicherort begonnen, und nach Abschluss des Vorgangs werden die VMs lokal gestartet.  
3. **Umgekehrtes Replizieren von lokalen VMs**: Nach dem Failback am lokalen Standort werden die lokalen virtuellen Computer umgekehrt repliziert, um mit der Replikation in Azure zu beginnen.

## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Überprüfen Sie vor dem Ausführen des Failovers die VM-Eigenschaften, und stellen Sie sicher, dass der virtuelle Computer die [Azure-Anforderungen](hyper-v-azure-support-matrix.md#replicated-vms) erfüllt.

Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > VM.

1. Im Bereich **Repliziertes Element** finden Sie eine Zusammenfassung der Informationen zu virtuellen Computern, den Integritätsstatus sowie die neuesten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.

1. In **Compute und Netzwerk** können Sie den Azure-Namen, die Ressourcengruppe, die Zielgröße, die [Verfügbarkeitsgruppe](../virtual-machines/windows/tutorial-availability-sets.md) und die Einstellungen verwalteter Datenträger ändern.

1. Sie können Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, anzeigen und ändern.

1. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="fail-over-to-azure"></a>Failover in Azure

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf VM > **Failover**.
2. Wählen Sie unter **Failover** den **letzten** Wiederherstellungspunkt aus. 
3. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, die Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
4. Klicken Sie auf **Commit ausführen**, nachdem Sie das Failover überprüft haben. Daraufhin werden alle verfügbaren Wiederherstellungspunkte gelöscht.

> [!WARNING]
> **Brechen Sie ein aktuell ausgeführtes Failover nicht ab**: Wenn Sie den Fortschritt abbrechen, wird das Failover beendet, die Replikation der VM wird jedoch nicht erneut durchgeführt.

## <a name="connect-to-failed-over-vm"></a>Herstellen einer Verbindung mit einer VM nach dem Failover

1. Wenn Sie Verbindungen mit Azure-VMs nach dem Failover mithilfe von RDP (Remote Desktop Protocol) und SSH (Secure Shell) herstellen möchten, [überprüfen Sie, ob die Anforderungen erfüllt wurden](failover-failback-overview.md#connect-to-azure-after-failover).
2. Navigieren Sie nach einem Failover zu dem virtuellen Computer, und stellen Sie zur Überprüfung eine [Verbindung](../virtual-machines/windows/connect-logon.md) her.
3. Verwenden Sie **Wiederherstellungspunkt ändern**, falls Sie nach dem Failover einen anderen Wiederherstellungspunkt verwenden möchten. Wenn Sie das Failover im nächsten Schritt committen, steht diese Option nicht mehr zur Verfügung.
4. Wählen Sie nach der Überprüfung **Committen** aus, um den Wiederherstellungspunkt des virtuellen Computers nach dem Failover fertig zu stellen.
5. Nach dem Committen werden alle anderen verfügbaren Wiederherstellungspunkte gelöscht. Mit diesem Schritt wird das Failover abgeschlossen.

>[!TIP]
> Sollten nach dem Failover Verbindungsprobleme auftreten, lesen Sie das [Handbuch zur Problembehandlung](site-recovery-failover-to-azure-troubleshoot.md).


## <a name="next-steps"></a>Nächste Schritte

Schützen Sie die Azure-VMs nach dem Failover erneut, sodass sie die Replikation von Azure zu lokalen Ressourcen durchführen. Sobald die virtuellen Computer am lokalen Standort erneut geschützt und repliziert werden, können Sie ein Failback von Azure durchführen, wenn Sie bereit sind.
