---
title: Ausschließen von VMware-VM-Datenträgern von der Notfallwiederherstellung in Azure mit Azure Site Recovery
description: Vorgehensweise zum Ausschließen von VMware-VM-Datenträgern aus der Replikation nach Azure mit Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.date: 12/10/2019
ms.author: mayg
ms.topic: conceptual
ms.openlocfilehash: c4842172ff181b5cdbe7f6fecf69da8755ae43fa
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86129877"
---
# <a name="exclude-disks-from-vmware-vm-replication-to-azure"></a>Ausschließen von Datenträgern von der Replikation virtueller VMware-Computer zu Azure

In diesem Artikel wird beschrieben, wie Datenträger aus der Replikation von VMware-VMs nach Azure für die Notfallwiederherstellung ausgeschlossen werden. Es gibt u. U. mehrere Gründe, um Datenträger aus der Replikation auszuschließen:

- Sicherstellen, dass unwichtige Daten auf dem ausgeschlossenen Datenträger nicht repliziert werden.
- Optimieren der beanspruchten Replikationsbandbreite bzw. der zielseitigen Ressourcen, indem Datenträger ausgeschlossen werden, die nicht repliziert werden müssen.
- Einsparen von Speicher- und Netzwerkressourcen, indem nicht benötigte Daten nicht repliziert werden.

Führen Sie vor dem Ausschließen von Datenträgern aus der Replikation folgende Schritte aus:

- [Weitere Informationen](exclude-disks-replication.md) zum Ausschließen von Datenträgern.
- Überprüfen Sie [typische Ausschlussszenarios](exclude-disks-replication.md#typical-scenarios) und [Beispiele](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk), die zeigen, wie sich das Ausschließen eines Datenträgers auf Replikation, Failover und Failback auswirkt.

## <a name="before-you-start"></a>Vorbereitung

 Beachten Sie zunächst Folgendes:

- **Replikation**: Standardmäßig werden alle Datenträger auf einem Computer repliziert.
- **Datenträgertyp**: Nur Basisdatenträger können von der Replikation ausgeschlossen werden. Sie können kein Betriebssystem und keine dynamischen Datenträger ausschließen.
- **Mobilitätsdienst**: Um einen Datenträger aus der Replikation auszuschließen, muss der Mobilitätsdienst vor der Aktivierung der Replikation manuell auf dem Computer installiert werden. Die Pushinstallation kann nicht verwendet werden, da diese Methode den Mobilitätsdienst nur auf einem virtuellen Computer installiert, nachdem die Replikation aktiviert wurde.  
- **Hinzufügen/Entfernen/Ausschließen von Datenträgern**: Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen, entfernen oder ausschließen. Wenn Sie Datenträger hinzufügen/entfernen möchten, müssen Sie den Schutz für den Computer deaktivieren und anschließend wieder aktivieren.
- **Failover**: Wenn Apps, für die ein Failover durchgeführt wurde, nach einem Failover ausgeschlossene Datenträger benötigen, damit die App funktioniert, müssen Sie diese Datenträger manuell erstellen. Alternativ können Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.
- **Failback – Windows**: Wenn Sie nach einem Failover ein Failback zum lokalen Standort durchführen, wird für Windows-Datenträger, die Sie manuell in Azure erstellt haben, kein Failback ausgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger durchführen und zwei Datenträger direkt auf Azure-VMs erstellen, erfolgt nur für die drei Datenträger, für die das Failover ausgeführt wurde, ein Failback.
- **Failback – Linux**: Bei Linux-Computern wird für Datenträger, die Sie manuell in Azure erstellen, ein Failback durchgeführt. Wenn Sie beispielsweise ein Failover für drei Datenträger ausführen und zwei Datenträger direkt auf Azure-VMs erstellen, wird für alle fünf ein Failback ausgeführt. Manuell erstellte Datenträger können nicht aus dem Failback oder dem erneuten Schützen von VMs ausgeschlossen werden.



## <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

1. Überprüfen Sie beim [Aktivieren der Replikation](./hyper-v-azure-tutorial.md) für eine VMware-VM nach Auswahl der zu replizierenden virtuellen Computer auf der Seite **Replikation aktivieren** > **Eigenschaften** > **Eigenschaften konfigurieren** die Spalte **Datenträger für Replikation**. Standardmäßig sind alle Datenträger für die Replikation ausgewählt.
2. Wenn Sie einen bestimmten Datenträger nicht replizieren möchten, können Sie in **Datenträger replizieren** die Datenträger abwählen, die Sie ausschließen möchten. 

    ![Ausschließen von Datenträgern von der Replikation](./media/vmware-azure-exclude-disk/enable-replication-exclude-disk1.png)



## <a name="next-steps"></a>Nächste Schritte
Nachdem die Bereitstellung eingerichtet wurde und ausgeführt wird, können Sie sich über die unterschiedlichen Failoverarten [informieren](failover-failback-overview.md) .
