---
title: Ausschließen von Hyper-V-VM-Datenträgern aus der Notfallwiederherstellung in Azure mit Azure Site Recovery
description: Vorgehensweise zum Ausschließen von Hyper-V-VM-Datenträgern aus der Replikation nach Azure mit Azure Site Recovery
author: mayurigupta13
manager: rochakm
ms.topic: conceptual
ms.author: mayg
ms.date: 11/12/2019
ms.openlocfilehash: 50fb6da2905b2ae27547f25cce3d7a76ca7976b7
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75498138"
---
# <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

In diesem Artikel wird beschrieben, wie Datenträger aus der Replikation von Hyper-V-VMs nach Azure ausgeschlossen werden. Es gibt u. U. mehrere Gründe, um Datenträger aus der Replikation auszuschließen:

- Sicherstellen, dass unwichtige Daten auf dem ausgeschlossenen Datenträger nicht repliziert werden.
- Optimieren der beanspruchten Replikationsbandbreite bzw. der zielseitigen Ressourcen, indem Datenträger ausgeschlossen werden, die nicht repliziert werden müssen.
- Einsparen von Speicher- und Netzwerkressourcen, indem nicht benötigte Daten nicht repliziert werden.

Führen Sie vor dem Ausschließen von Datenträgern aus der Replikation folgende Schritte aus:

- [Weitere Informationen](exclude-disks-replication.md) zum Ausschließen von Datenträgern.
- Überprüfen Sie [typische Ausschlussszenarios](exclude-disks-replication.md#typical-scenarios) und [Beispiele](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk), die zeigen, wie sich das Ausschließen eines Datenträgers auf Replikation, Failover und Failback auswirkt.

## <a name="before-you-start"></a>Vorbereitung

Beachten Sie zunächst Folgendes:

- **Replikation**: Standardmäßig werden alle Datenträger auf einem Computer repliziert.
- **Datenträgertyp**:
    - Sie können Basisdatenträger von der Replikation ausschließen.
    - Sie können keine Betriebssystemdatenträger ausschließen.
    - Es wird empfohlen, keine dynamischen Datenträger auszuschließen. Site Recovery kann nicht feststellen, welche VHD in der Gast-VM eine einfache oder dynamische VHD ist.  Wenn nicht alle abhängigen dynamischen Volumedatenträger ausgeschlossen werden, wird der geschützte dynamische Datenträger auf einer Failover-VM zu einem fehlerhaften Datenträger, sodass die Daten auf diesem Datenträger nicht zugänglich sind.
- **Hinzufügen/Entfernen/Ausschließen von Datenträgern**: Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen, entfernen oder ausschließen. Wenn Sie einen Datenträger hinzufügen/entfernen oder ausschließen möchten, müssen Sie den VM-Schutz deaktivieren und anschließend wieder aktivieren.
- **Failover**: Wenn Apps, für die ein Failover durchgeführt wurde, nach einem Failover ausgeschlossene Datenträger benötigen, damit die App funktioniert, müssen Sie diese Datenträger manuell erstellen. Alternativ können Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.
- **Failback**: Wenn Sie nach einem Failover ein Failback zum lokalen Standort durchführen, wird für Datenträger, die Sie manuell in Azure erstellt haben, kein Failback ausgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger durchführen und zwei Datenträger direkt in einem virtuellen Azure-Computer erstellen, erfolgt nur für die drei Datenträger, für die das Failover durchgeführt wurde, ein Failback. Manuell erstellte Datenträger können nicht in das Failback oder in die umgekehrte Replikation von virtuellen Computern einbezogen werden.

## <a name="exclude-disks"></a>Ausschließen von Datenträgern

1. Um Datenträger auszuschließen, wenn Sie die Replikation für einen virtuellen Hyper-V-Computer [aktivieren](site-recovery-hyper-v-site-to-azure.md), müssen Sie nach Auswahl der zu replizierenden virtuellen Computer auf der Seite **Replikation aktivieren** > **Eigenschaften** > **Eigenschaften konfigurieren** die Spalte **Datenträger für Replikation** überprüfen. Standardmäßig sind alle Datenträger für die Replikation ausgewählt.
2. Wenn Sie einen bestimmten Datenträger nicht replizieren möchten, können Sie in **Datenträger replizieren** die Datenträger abwählen, die Sie ausschließen möchten. 

    ![Ausschließen von Datenträgern von der Replikation](./media/hyper-v-exclude-disk/enable-replication6-with-exclude-disk.png)


## <a name="next-steps"></a>Nächste Schritte
Nachdem die Bereitstellung eingerichtet wurde und ausgeführt wird, können Sie sich über die unterschiedlichen Failoverarten [informieren](failover-failback-overview.md) .
