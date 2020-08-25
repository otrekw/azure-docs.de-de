---
title: Sichern einer SQL Server-VM über den VM-Bereich
description: In diesem Artikel erfahren Sie, wie Sie SQL Server-Datenbanken auf virtuellen Azure-Computern über den VM-Bereich sichern können.
ms.topic: conceptual
ms.date: 08/13/2020
ms.openlocfilehash: dd30ca1fb138d3e3bd44633b9d5e71beae6d96be
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88226846"
---
# <a name="back-up-a-sql-server-from-the-vm-pane"></a>Sichern einer SQL Server-Instanz über den VM-Bereich

Dieser Artikel beschreibt, wie Sie SQL Server auf Azure-VMs mit dem [Azure Backup](backup-overview.md)-Dienst sichern. Sie können SQL Server-VMs auf zwei Arten sichern:

- Einzelne Azure-VM mit SQL Server: Die Anweisungen in diesem Artikel beschreiben, wie Sie eine SQL Server-VM direkt aus der VM-Ansicht sichern können.
- Mehrere Azure-VMs mit SQL Server: Sie können einen Recovery Services-Tresor einrichten und die Sicherung für mehrere VMs konfigurieren. Folgen Sie für dieses Szenario den Anweisungen in [diesem Artikel](backup-sql-server-database-azure-vms.md).

## <a name="before-you-start"></a>Vorbereitung

1. Überprüfen Sie Ihre Umgebung mit der [Unterstützungsmatrix](sql-support-matrix.md).
2. Verschaffen Sie sich einen [Überblick](backup-azure-sql-database.md) über Azure Backup für SQL Server-VM.
3. Stellen Sie sicher, dass der virtuelle Computer über [Netzwerkkonnektivität](backup-sql-server-database-azure-vms.md#establish-network-connectivity) verfügt.

## <a name="configure-backup-on-the-sql-server"></a>Konfigurieren der Sicherung unter SQL Server

Sie können die Sicherung auf Ihrer SQL Server-VM über den Bereich **Sicherung**  der VM aktivieren. Diese Methode bewirkt zwei Dinge:

- Die SQL-VM wird beim Azure Backup-Dienst registriert, um ihr Zugriff zu gewähren.
- Alle innerhalb der VM ausgeführten SQL Server-Instanzen werden automatisch geschützt. Dies bedeutet, dass die Sicherungsrichtlinie auf alle bestehenden Datenbanken sowie auf die Datenbanken, die in Zukunft zu diesen Instanzen hinzugefügt werden, angewendet wird.

1. Wählen Sie das Banner oben auf der Seite aus, um die SQL Server-Sicherungsansicht zu öffnen.

    ![SQL Server-Sicherungsansicht](./media/backup-sql-server-vm-from-vm-pane/sql-server-backup-view.png)

    >[!NOTE]
    >Wird das Banner nicht angezeigt? Das Banner wird nur für die virtuellen SQL Server-Computer angezeigt, die mit Azure Marketplace-Images erstellt werden. Außerdem wird es für die VMs angezeigt, die mit der Azure-VM-Sicherung geschützt sind. Für andere Images können Sie die Sicherung wie [hier](backup-sql-server-database-azure-vms.md) erklärt konfigurieren.

2. Geben Sie den Namen des Recovery Services-Tresors ein. Ein Tresor ist eine logische Entität zum Speichern und Verwalten all Ihrer Sicherungen. Wenn Sie einen neuen Tresor erstellen, gilt Folgendes:

    - Er wird im gleichen Abonnement und in der gleichen Region erstellt, in der sich auch die SQL Server-VM befindet, die Sie schützen.
    - Er wird mit der GRS-Einstellung (georedundanter Speicher) für alle Sicherungen erstellt. Wenn Sie den Redundanztyp ändern möchten, sollten Sie dies vor dem Schutz der VM tun. Weitere Informationen finden Sie in [diesem Artikel](backup-create-rs-vault.md#set-storage-redundancy).

3. Wählen Sie eine **Sicherungsrichtlinie** aus. Sie können eine Standardrichtlinie oder eine der anderen vorhandenen Richtlinien auswählen, die Sie im Tresor erstellt haben. Wenn Sie eine neue Richtlinie erstellen möchten, finden Sie in [diesem Artikel](backup-sql-server-database-azure-vms.md#create-a-backup-policy) eine Schritt-für-Schritt-Anleitung.

    ![Auswählen einer Sicherungsrichtlinie](./media/backup-sql-server-vm-from-vm-pane/backup-policy.png)

4. Wählen Sie **Sicherung aktivieren** aus. Der Vorgang kann einige Minuten dauern.

    ![Auswählen von „Sicherung aktivieren“](./media/backup-sql-server-vm-from-vm-pane/enable-backup.png)

5. Sobald der Vorgang abgeschlossen ist, wird der **Tresorname** im Banner angezeigt.

    ![Tresorname wird im Banner angezeigt](./media/backup-sql-server-vm-from-vm-pane/vault-name.png)

6. Wählen Sie das Banner aus, um zur Tresoransicht zu gelangen, wo alle registrierten VMs und deren Schutzstatus angezeigt werden.

    ![Tresoransicht mit registrierten VMs](./media/backup-sql-server-vm-from-vm-pane/vault-view.png)

7. Für Images, die keine Marketplace-Images sind, kann die Registrierung erfolgreich sein, aber **Sicherung konfigurieren** wird möglicherweise erst dann ausgelöst, wenn die Azure Backup-Erweiterung auf der SQL Server-Instanz die entsprechende Berechtigung erhält. In solchen Fällen enthält die Spalte **Sicherungsbereitschaft** die Angabe **Nicht bereit**. Für Images, die keine Marketplace-Images sind, müssen Sie [die entsprechenden Berechtigungen manuell zuweisen](backup-azure-sql-database.md#set-vm-permissions), damit das „Sicherung konfigurieren ausgelöst werden kann.

    ![„Sicherungsbereitschaft“ ist „Nicht bereit“](./media/backup-sql-server-vm-from-vm-pane/backup-readiness-not-ready.png)

8. Für weitere Vorgänge oder Überwachungen, die Sie auf der gesicherten SQL Server-VM durchführen müssen, wechseln Sie zum entsprechenden Recovery Service-Tresor. Wechseln Sie zu **Sicherungselemente**, um alle in diesem Tresor gesicherten Datenbanken anzuzeigen und Vorgänge wie bedarfsgesteuerte Sicherungen und Wiederherstellungen auszulösen. In ähnlicher Weise wechseln Sie zu **Sicherungsaufträge**, um Aufträge, die Vorgängen wie Konfigurieren von Schutz, Sicherung und Wiederherstellung entsprechen, zu [überwachen](manage-monitor-sql-database-backup.md).

    ![Siehe „Gesicherte Datenbanken“ in „Sicherungselemente“](./media/backup-sql-server-vm-from-vm-pane/backup-items.png)

>[!NOTE]
>Auf den neuen SQL Server-Instanzen, die der geschützten VM möglicherweise später hinzugefügt werden, wird die Sicherung wird nicht automatisch konfiguriert. Um die Sicherung für die neu hinzugefügten Instanzen zu konfigurieren, müssen Sie zum Tresor wechseln, bei dem die VM registriert ist, und die [hier](backup-sql-server-database-azure-vms.md) aufgeführten Schritte ausführen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden folgende Themen erläutert:

- [Wiederherstellen von gesicherten SQL Server-Datenbanken](restore-sql-database-azure-vm.md)
- [Verwalten von gesicherten SQL Server-Datenbanken](manage-monitor-sql-database-backup.md)
