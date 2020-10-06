---
title: Ausführen von Aktionen mit Backup Center
description: In diesem Artikel erfahren Sie, wie Sie Aktionen mithilfe von Backup Center ausführen.
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 81c81f0e9d955e0a5243485baaedff4e6f0fc10d
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993463"
---
# <a name="perform-actions-using-backup-center"></a>Ausführen von Aktionen mit Backup Center

Backup Center ermöglicht das Ausführen wichtiger sicherungsbezogener Aktionen über eine zentrale Oberfläche, ohne zu einem einzelnen Tresor navigieren zu müssen. Mit Backup Center können unter anderem folgende Aktionen ausgeführt werden:

* Konfigurieren der Sicherung für Ihre Datenquellen
* Wiederherstellen einer Sicherungsinstanz
* Erstellen eines neuen Tresors
* Erstellen einer neuen Sicherungsrichtlinie
* Auslösen einer bedarfsgesteuerten Sicherung für eine Sicherungsinstanz
* Beenden der Sicherung für eine Sicherungsinstanz

## <a name="supported-scenarios"></a>Unterstützte Szenarien

* Backup Center wird derzeit für Sicherungen von virtuellen Azure-Computern und Azure Database for PostgreSQL-Servern unterstützt.
* Eine ausführliche Liste der unterstützten und nicht unterstützten Szenarien finden Sie in der [Supportmatrix](backup-center-support-matrix.md).

## <a name="configure-backup"></a>Konfigurieren der Sicherung

Führen Sie die entsprechenden Schritte für die Art der zu sichernden Datenquelle aus:

### <a name="configure-backup-for-azure-virtual-machines"></a>Konfigurieren der Sicherung für virtuelle Azure-Computer

1. Navigieren Sie zu Backup Center, und wählen Sie oben auf der Registerkarte **Übersicht** die Option **+ Sichern** aus.

    ![Backup Center-Übersicht](./media/backup-center-actions/backup-center-overview-configure-backup.png)

2. Wählen Sie die Art der zu sichernden Datenquelle aus (in diesem Fall „Virtueller Azure-Computer“).

    ![Auswählen der Datenquelle zum Konfigurieren einer VM-Sicherung](./media/backup-center-actions/backup-select-datasource-vm.png)

3. Wählen Sie einen Recovery Services-Tresor und anschließend **Fortfahren** aus. Daraufhin gelangen Sie zur gleichen Sicherungskonfiguration wie über einen Recovery Services-Tresor. Weitere Informationen zum Konfigurieren einer Sicherung für virtuelle Azure-Computer mit einem Recovery Services-Tresor finden Sie [hier](tutorial-backup-vm-at-scale.md).

### <a name="configure-backup-for-azure-database-for-postgresql-server"></a>Konfigurieren der Sicherung für einen Azure Database for PostgreSQL-Server

1. Navigieren Sie zu Backup Center, und wählen Sie oben auf der Registerkarte **Übersicht** die Option **+ Sichern** aus.
2. Wählen Sie die Art der zu sichernden Datenquelle aus (in diesem Fall „Azure Database for PostgreSQL-Server“).

    ![Auswählen der Datenquelle zum Konfigurieren einer Sicherung für Azure Database for PostgreSQL-Server](./media/backup-center-actions/backup-select-datasource-type-postgresql.png)

3. Wählen Sie **Proceed** (Fortfahren) aus. Daraufhin gelangen Sie zur gleichen Sicherungskonfiguration wie über einen Backup-Tresor. Weitere Informationen zum Konfigurieren einer Sicherung für Azure Database for PostgreSQL-Server mit einem Backup-Tresor finden Sie [hier](backup-azure-database-postgresql.md#configure-backup-on-azure-postgresql-databases).

## <a name="restore-a-backup-instance"></a>Wiederherstellen einer Sicherungsinstanz

Führen Sie die entsprechenden Schritte für die Art der wiederherzustellenden Datenquelle aus:

### <a name="if-youre-restoring-an-azure-virtual-machine"></a>Wiederherstellen eines virtuellen Azure-Computers

1. Navigieren Sie zu Backup Center, und wählen Sie oben auf der Registerkarte **Übersicht** die Option **Wiederherstellen** aus.

    ![Backup Center-Übersicht für die VM-Wiederherstellung](./media/backup-center-actions/backup-center-overview-restore.png)

2. Wählen Sie die Art der wiederherzustellenden Datenquelle aus (in diesem Fall „Virtueller Azure-Computer“).

    ![Auswählen der Datenquelle für die VM-Wiederherstellung](./media/backup-center-actions/restore-select-datasource-vm.png)

3. Wählen Sie eine Sicherungsinstanz und anschließend **Fortsetzen** aus. Daraufhin gelangen Sie zu den gleichen Wiederherstellungseinstellungen wie über einen Recovery Services-Tresor. Weitere Informationen zum Wiederherstellen eines virtuellen Azure-Computers mit einem Recovery Services-Tresor finden Sie [hier](backup-azure-arm-restore-vms.md#before-you-start).

### <a name="if-youre-restoring-an-azure-database-for-postgresql-server"></a>Wiederherstellen eines Azure Database for PostgreSQL-Servers

1. Navigieren Sie zu Backup Center, und wählen Sie oben auf der Registerkarte **Übersicht** die Option **Wiederherstellen** aus.
2. Wählen Sie die Art der wiederherzustellenden Datenquelle aus (in diesem Fall „Azure Database for PostgreSQL-Server“).

    ![Auswählen der Datenquelle für die Wiederherstellung eines Azure Database for PostgreSQL-Servers](./media/backup-center-actions/restore-select-datasource-postgresql.png)

3. Wählen Sie eine Sicherungsinstanz und anschließend **Fortsetzen** aus. Daraufhin gelangen Sie zu den gleichen Wiederherstellungseinstellungen wie über einen Recovery Services-Tresor. Weitere Informationen zum Wiederherstellen eines Azure Database for PostgreSQL-Servers mit einem Backup-Tresor finden Sie [hier](backup-azure-database-postgresql.md#restore).

## <a name="create-a-new-vault"></a>Erstellen eines neuen Tresors

Sie können einen neuen Tresor erstellen, indem Sie zu Backup Center navigieren und oben auf der Registerkarte **Übersicht** die Option **+ Tresor** auswählen.

![Tresor erstellen](./media/backup-center-actions/backup-center-create-vault.png)

* Weitere Informationen zum Erstellen eines Recovery Services-Tresors finden Sie [hier](backup-create-rs-vault.md).
* Weitere Informationen zum Erstellen eines Backup-Tresors finden Sie [hier](backup-vault-overview.md).

## <a name="create-a-new-backup-policy"></a>Erstellen einer neuen Sicherungsrichtlinie

Führen Sie die entsprechenden Schritte für die Art der zu sichernden Datenquelle aus:

### <a name="if-youre-backing-up-an-azure-virtual-machine"></a>Sichern eines virtuellen Azure-Computers

1. Navigieren Sie zu Backup Center, und wählen Sie oben auf der Registerkarte **Übersicht** die Option **+ Richtlinie** aus.

    ![Backup Center-Übersicht für die Sicherungsrichtlinie](./media/backup-center-actions/backup-center-overview-policy.png)

2. Wählen Sie die Art der zu sichernden Datenquelle aus (in diesem Fall „Virtueller Azure-Computer“).

    ![Auswählen der Datenquelle für die Richtlinie für eine VM-Sicherung](./media/backup-center-actions/policy-select-datasource-vm.png)

3. Wählen Sie einen Recovery Services-Tresor und anschließend **Fortfahren** aus. Daraufhin gelangen Sie zur gleichen Richtlinienerstellung wie über einen Recovery Services-Tresor. Weitere Informationen zum Erstellen einer neuen Sicherungsrichtlinie für einen virtuellen Azure-Computer mit einem Recovery Services-Tresor finden Sie [hier](backup-azure-arm-vms-prepare.md#create-a-custom-policy).

### <a name="if-youre-backing-up-an-azure-database-for-postgresql-server"></a>Sichern eines Azure Database for PostgreSQL-Servers

1. Navigieren Sie zu Backup Center, und wählen Sie oben auf der Registerkarte **Übersicht** die Option **+ Richtlinie** aus.
2. Wählen Sie die Art der zu sichernden Datenquelle aus (in diesem Fall „Azure Database for PostgreSQL-Server“).

    ![Auswählen der Datenquelle für die Richtlinie für eine Sicherung eines Azure Database for PostgreSQL-Servers](./media/backup-center-actions/policy-select-datasource-postgresql.png)

3. Wählen Sie **Proceed** (Fortfahren) aus. Daraufhin gelangen Sie zur gleichen Richtlinienerstellung wie über einen Backup-Tresor. Weitere Informationen zum Erstellen einer neuen Sicherungsrichtlinie mit einem Backup-Tresor finden Sie [hier](backup-azure-database-postgresql.md#create-backup-policy).

## <a name="execute-an-on-demand-backup-for-a-backup-instance"></a>Ausführen einer bedarfsgesteuerten Sicherung für eine Sicherungsinstanz

Mit Backup Center können Sie nach Sicherungsinstanzen in Ihrem gesamten Sicherungsbestand suchen und Sicherungsvorgänge nach Bedarf ausführen.

Navigieren Sie zum Auslösen einer bedarfsgesteuerten Sicherung zu Backup Center, und wählen Sie das Menüelement **Sicherungsinstanzen** aus. Dadurch können Sie Details zu allen Sicherungsinstanzen anzeigen, auf die Sie Zugriff haben. Sie können nach der Sicherungsinstanz suchen, die Sie sichern möchten. Wenn Sie im Raster mit der rechten Maustaste auf ein Element klicken, wird eine Liste mit verfügbaren Aktionen geöffnet. Wählen Sie die Option **Jetzt sichern** aus, um eine bedarfsgesteuerte Sicherung auszuführen.

![Bedarfsgesteuerte Sicherung](./media/backup-center-actions/backup-center-on-demand-backup.png)

Weitere Informationen zum Ausführen bedarfsgesteuerter Sicherungen für virtuelle Azure-Computer finden Sie [hier](backup-azure-manage-vms.md#run-an-on-demand-backup).

Weitere Informationen zum Ausführen bedarfsgesteuerter Sicherungen für Azure Database for PostgreSQL-Server finden Sie [hier](backup-azure-database-postgresql.md#on-demand-backup).

## <a name="stop-backup-for-a-backup-instance"></a>Beenden der Sicherung für eine Sicherungsinstanz

Manchmal möchten Sie möglicherweise die Sicherung für eine Sicherungsinstanz beenden – etwa, wenn die zugrunde liegende Ressource nicht mehr vorhanden ist.

Navigieren Sie zum Auslösen einer bedarfsgesteuerten Sicherung zu Backup Center, und wählen Sie das Menüelement **Sicherungsinstanzen** aus. Dadurch können Sie Details zu allen Sicherungsinstanzen anzeigen, auf die Sie Zugriff haben. Sie können nach der Sicherungsinstanz suchen, die Sie sichern möchten. Wenn Sie im Raster mit der rechten Maustaste auf ein Element klicken, wird eine Liste mit verfügbaren Aktionen geöffnet. Wählen Sie die Option **Sicherung beenden** aus, um die Sicherung für die Sicherungsinstanz zu beenden.

![Schutz beenden](./media/backup-center-actions/backup-center-stop-protection.png)

Weitere Informationen zum Beenden der Sicherung für virtuelle Azure-Computer finden Sie [hier](backup-azure-manage-vms.md#stop-protecting-a-vm).

Weitere Informationen zum Beenden der Sicherung für Azure Database for PostgreSQL-Server finden Sie [hier](backup-azure-database-postgresql.md#stop-protection).

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Betreiben von Sicherungen mit Backup Center](backup-center-monitor-operate.md)
* [Steuern Ihres Sicherungsbestands mit Backup Center](backup-center-govern-environment.md)
* [Gewinnen von Erkenntnissen mit Backup Center](backup-center-obtain-insights.md)
