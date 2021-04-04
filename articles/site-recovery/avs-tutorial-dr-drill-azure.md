---
title: Durchführen eines Notfallwiederherstellungsverfahrens von Azure VMware Solution in Azure mit Azure Site Recovery
description: Hier wird beschrieben, wie Sie mit Azure Site Recovery ein Notfallwiederherstellungsverfahren von Azure VMware Solution in Azure durchführen.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: 46d5e1cf773a24b032874ee54021c780e1f361af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91814237"
---
# <a name="run-a-disaster-recovery-drill-from-azure-vmware-solution-to-azure"></a>Durchführen eines Notfallwiederherstellungsverfahren von Azure VMware Solution in Azure

In diesem Artikel erfahren Sie, wie Sie für eine Azure VMware Solution-VM unter Verwendung des [Azure Site Recovery](site-recovery-overview.md)-Diensts ein Notfallwiederherstellungsverfahren in Azure durchführen. Eine Übung dient der Überprüfung Ihrer Replikationsstrategie ohne Datenverlust.


Dies ist das vierte in einer Reihe von Tutorials zur Einrichtung der Notfallwiederherstellung in Azure für Azure VMware Solution-VMs.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Einrichten eines isolierten Netzwerks für das Testfailover
> * Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover
> * Ausführen eines Testfailovers für einen einzelnen Computer

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführlichere Informationen zu den Schritten des Notfallwiederherstellungsverfahrens finden Sie in [diesem Artikel](site-recovery-test-failover-to-azure.md).

## <a name="before-you-start"></a>Vorbereitung

Absolvieren Sie die vorherigen Tutorials:

1. Vergewissern Sie sich, dass Sie die Schritte zum [Einrichten von Azure](avs-tutorial-prepare-azure.md) für die Notfallwiederherstellung in Azure ausgeführt haben.
2. Führen Sie [diese Schritte](avs-tutorial-prepare-avs.md) aus, um Ihre Azure VMware Solution-Bereitstellung für die Notfallwiederherstellung in Azure vorzubereiten.
3. [Richten Sie die Notfallwiederherstellung für Azure VMware Solution-VMs ein](avs-tutorial-replication.md).
 

## <a name="verify-vm-properties"></a>Überprüfen von VM-Eigenschaften

Bevor Sie ein Testfailover ausführen, überprüfen Sie die VM-Eigenschaften, und stellen Sie sicher, dass die [VMware-VM](vmware-physical-azure-support-matrix.md#replicated-machines) die Azure-Anforderungen erfüllt.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** und dann auf den virtuellen Computer.
2. Im Bereich **Repliziertes Element** finden Sie eine Zusammenfassung der Informationen zu virtuellen Computern, den Integritätsstatus sowie die neuesten verfügbaren Wiederherstellungspunkte. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
3. In **Compute und Netzwerk** können Sie den Azure-Namen, die Ressourcengruppe, die Zielgröße, die Verfügbarkeitsgruppe und die Einstellungen verwalteter Datenträger ändern.
4. Sie können Netzwerkeinstellungen einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie der IP-Adresse, die ihm zugewiesen wird, anzeigen und ändern.
5. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="create-a-network-for-test-failover"></a>Erstellen eines Netzwerks für Testfailover

Für Testfailover sollten Sie ein Netzwerk wählen, das von dem in den Einstellungen **Compute und Netzwerk** für jede VM angegebenen Produktionswiederherstellungsstandort-Netzwerk isoliert ist. Standardmäßig ist ein virtuelles Azure-Netzwerk beim Erstellen von anderen Netzwerken isoliert. Das Testnetzwerk sollte Ihr Produktionsnetzwerk imitieren:

- Das Testnetzwerk muss dieselbe Anzahl von Subnetzen wie das Produktionsnetzwerk besitzen. Subnetze müssen die gleichen Namen aufweisen.
- Das Testnetzwerk muss die gleiche IP-Adressklasse und den gleichen Subnetzbereich verwenden.
- Aktualisieren Sie das DNS des Testnetzwerks mit der IP-Adresse, die in den Einstellungen **Compute und Netzwerk** für die DNS-VM angegeben ist. Unter [Überlegungen zum Test-Failover](site-recovery-active-directory.md#test-failover-considerations) finden Sie weitere Details.

## <a name="run-a-test-failover-for-a-single-vm"></a>Ausführen eines Testfailovers für eine einzelne VM

Beim Durchführen eines Testfailovers geschieht Folgendes:

1. Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für ein Failover erfüllt sind.
2. Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
3. Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

Führen Sie das Testfailover wie folgt aus:

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf die VM > **+Testfailover**.
2. Wählen Sie für dieses Tutorial **Letzte Verarbeitung** für den Wiederherstellungspunkt aus. Bei Auswahl dieser Option wird für die VM ein Failover auf den letzten verfügbaren Zeitpunkt durchgeführt. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten aufgewendet, und der RTO-Wert (Recovery Time Objective) wird niedrig gehalten.
3. Wählen Sie unter **Testfailover** das Azure-Zielnetzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt des Vorgangs verfolgen, indem Sie auf die VM klicken, um ihre Eigenschaften zu öffnen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** >
   **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Vergewissern Sie sich, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Sie sollten nun eine Verbindung mit der replizierten VM in Azure herstellen können.
7. Klicken Sie zum Löschen der während des Testfailovers erstellten Azure-VMs auf **Testfailover bereinigen** für die VM. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

In einigen Szenarien erfordert ein Failover zusätzliche Verarbeitungsschritte, die etwa 8 bis 10 Minuten dauern können. Bei VMware-Linux-Computern, VMware-VMs ohne aktivierten DHCP-Dienst und VMware-VMs ohne die Starttreiber storvsc, vmbus, storflt, intelide und atapi kann das Testfailover länger dauern.

## <a name="connect-after-failover"></a>Herstellen einer Verbindung nach einem Failover

Wenn Sie nach einem Failover eine RDP-/SSH-Verbindung mit virtuellen Azure-Computern herstellen möchten, [bereiten Sie die Verbindungsherstellung vor](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover). Sollten nach dem Failover Verbindungsprobleme auftreten, lesen Sie das [Handbuch zur Problembehandlung](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen eines Failovers](avs-tutorial-failover.md)

