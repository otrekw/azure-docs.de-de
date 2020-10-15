---
title: Ausführen eines Failovers und erneutes Schützen von in einer sekundären Azure-Region replizierten virtuellen Azure-Computern zur Notfallwiederherstellung mit dem Azure Site Recovery-Dienst
description: Hier erfahren Sie, wie Sie mit dem Azure Site Recovery-Dienst zur Notfallwiederherstellung ein Failover für in einer sekundären Azure-Region replizierte virtuelle Azure-Computer ausführen und diese Computer erneut schützen
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 08/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8d38aa513b0829c2626fcd4a92c40faabff1f83e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87502391"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Ausführen eines Failovers und erneutes Schützen von Azure-VMs zwischen Regionen

In diesem Tutorial wird beschrieben, wie ein Failover eines virtuellen Azure-Computers (VM) zu einer sekundären Azure-Region mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts ausgeführt wird. Nach dem Failover schützen Sie den virtuellen Computer erneut. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Ausführen eines Failovers der Azure-VM
> * Erneutes Schützen der sekundären Azure-VM, damit sie in der primären Region repliziert wird

> [!NOTE]
> In diesem Tutorial wird die einfachste Vorgehensweise mit Standardeinstellungen und minimaler Anpassung erläutert. Lesen Sie für komplexere Szenarien die Artikel unter „Anleitungen“ für virtuelle Azure-Computer.


## <a name="prerequisites"></a>Voraussetzungen

- Bevor Sie beginnen, lesen Sie die [häufig gestellten Fragen](site-recovery-faq.md#failover) zu Failover.
- Führen Sie unbedingt ein [Notfallwiederherstellungsverfahren](azure-to-azure-tutorial-dr-drill.md) durch, um zu überprüfen, ob alles wie erwartet funktioniert.
- Überprüfen Sie die VM-Eigenschaften, bevor Sie das Testfailover ausführen. Die VM muss den [Azure-Anforderungen](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) entsprechen.

## <a name="run-a-failover-to-the-secondary-region"></a>Ausführen eines Failovers zur sekundären Region

1. Wählen Sie unter **Replizierten Elemente** die VM, für die ein Failover ausgeführt werden soll, > **Failover** aus.

   ![Screenshot: Failoveroptionen für einen virtuellen Computer](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. Wählen Sie unter **Failover** einen **Wiederherstellungspunkt** für das Failover aus. Sie können eine der folgenden Optionen auswählen:

   * **Neuester** (Standardeinstellung): Bei dieser Option werden alle Daten in Site Recovery verarbeitet, und sie bietet den niedrigsten RPO-Wert (Recovery Point Objective).
   * **Letzte Verarbeitung**: Mit dieser Option wird für den virtuellen Computer der Zustand des letzten Wiederherstellungspunkts wiederhergestellt, der von Site Recovery verarbeitet wurde.
   * **Benutzerdefiniert**: Bei dieser Option wird ein Failover auf einen bestimmten Wiederherstellungspunkt ausgeführt. Diese Option eignet sich für ein Testfailover.

3. Klicken Sie auf **Der Computer wird vor Beginn des Failovers heruntergefahren**, wenn Site Recovery versuchen soll, Quell-VMs herunterzufahren, bevor das Failover ausgelöst wird. Mit dem Herunterfahren können Sie sicherzustellen, dass keine Daten verloren gehen. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Site Recovery bereinigt die Quelle nach dem Failover nicht.

4. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

5. Überprüfen Sie den virtuellen Computer nach Abschluss des Failovers, indem Sie sich am Computer anmelden. Falls Sie einen anderen Wiederherstellungspunkt für den virtuellen Computer verwenden möchten, können Sie die Option **Wiederherstellungspunkt ändern** wählen.

6. Nachdem Sie mit dem virtuellen Computer, für den das Failover durchgeführt wurde, zufrieden sind, können Sie für das Failover die Option **Commit** wählen.
   Dadurch werden alle mit dem Dienst verfügbaren Wiederherstellungspunkte gelöscht. Sie können den Wiederherstellungspunkt nun nicht mehr ändern.

> [!NOTE]
> Beim Failover einer VM, der Sie einen Datenträger hinzugefügt haben, nachdem Sie für die VM Replikation aktiviert haben, zeigen die Replikationspunkte die Datenträger an, die zur Wiederherstellung zur Verfügung stehen. Wenn beispielsweise ein virtueller Computer über einen einzelnen Datenträger verfügt und Sie einen neuen hinzufügen, zeigen Replikationspunkte, die vor dem Hinzufügen des Datenträgers erstellt wurden, an, dass der Replikationspunkt aus „1 von 2 Datenträgern“ besteht.

![Screenshot: Failover mit hinzugefügtem Datenträger](./media/azure-to-azure-tutorial-failover-failback/failover-added.png)

## <a name="reprotect-the-secondary-vm"></a>Erneutes Schützen der sekundären VM

Nach dem Failover der VM müssen Sie sie erneut schützen, damit sie wieder zurück zur primären Region repliziert wird.

1. Stellen Sie sicher, dass sich die VM im Status **Commit für Failover ausgeführt** befindet, dass die primäre Region verfügbar ist und dass Sie neue Ressourcen darin erstellen und darauf zugreifen können.
2. Klicken Sie unter **Tresor** > **Replizierte Elemente** mit der rechten Maustaste auf die VM, für die ein Failover durchgeführt wurde, und dann auf **Erneut schützen**.

   ![Screenshot: Option „Erneut schützen“ für einen virtuellen Computer](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Beachten Sie, dass die Wirkrichtung des Schutzes – von der sekundären zur primären Region – bereits ausgewählt ist.
3. Lesen Sie die Informationen zu **Ressourcengruppe, Netzwerk, Speicher und Verfügbarkeitsgruppen**, und klicken Sie auf „OK“. Alle als neu markierten Ressourcen werden während des Vorgangs zum erneuten Schützen erstellt.
4. Klicken Sie auf **OK**, um einen Auftrag zum erneuten Schützen auszulösen. Dieser Auftrag liefert die neuesten Daten an den Zielstandort. Anschließend werden die Deltas zur primären Region repliziert. Die VM befindet sich jetzt in einem geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie nach dem erneuten Schützen [hier](azure-to-azure-tutorial-failback.md) nach, wie Sie ein Failback zur primären Region ausführen, wenn diese verfügbar ist.
- Erfahren Sie mehr über den [erneuten Schutz](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection).
