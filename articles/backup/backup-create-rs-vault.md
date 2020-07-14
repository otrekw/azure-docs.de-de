---
title: Erstellen und Konfigurieren von Recovery Services-Tresoren
description: In diesem Artikel erfahren Sie, wie Sie Recovery Services-Tresore zum Speichern von Sicherungen und Wiederherstellungspunkten erstellen und konfigurieren.
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 65f7265dccc5fe28d3503e72bdd6e49123871594
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970524"
---
# <a name="create-and-configure-a-recovery-services-vault"></a>Erstellen und Konfigurieren von Recovery Services-Tresoren

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="set-storage-redundancy"></a>Festlegen der Speicherredundanz

Azure Backup übernimmt automatisch die Speicherung für den Tresor. Sie müssen angeben, wie dieser Speicher repliziert wird.

> [!NOTE]
> Bevor Sicherungen im Tresor konfiguriert werden, muss der **Speicherreplikationstyp** (lokal redundant/georedundant) für einen Recovery Services-Tresor geändert werden. Sobald Sie eine Sicherung konfigurieren, wird die Änderungsoption deaktiviert.
>
>- Wenn Sie noch keine Sicherung konfiguriert haben, [führen Sie die folgenden Schritte aus](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy), um die Einstellungen zu überprüfen und zu ändern.
>- Wenn Sie die Sicherung bereits konfiguriert haben und von georedundantem Speicher (GRS) zu lokal redundantem Speicher (LRS) wechseln müssen, [überprüfen Sie diese Problemumgehungen](#how-to-change-from-grs-to-lrs-after-configuring-backup).

1. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf den neuen Tresor. Klicken Sie im Abschnitt **Einstellungen** auf **Eigenschaften**.
1. Klicken Sie in **Eigenschaften** unter **Sicherungskonfiguration** auf **Aktualisieren**.

1. Wählen Sie den Speicherreplikationstyps aus, und klicken Sie auf **Speichern**.

     ![Speicherkonfiguration für neuen Tresor festlegen](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Wir empfehlen, dass Sie, wenn Sie Azure als primären Endpunkt für den Sicherungsspeicher verwenden, weiterhin die Standardeinstellung **Georedundant** verwenden.
   - Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher.
   - Erfahren Sie mehr über [Georedundanz](../storage/common/storage-redundancy-grs.md) and [lokale Redundanz](../storage/common/storage-redundancy-lrs.md).

>[!NOTE]
>Die Speicherreplikationseinstellungen für den Tresor sind nicht relevant für das Sichern von Azure-Dateifreigaben, da die aktuelle Lösung auf Momentaufnahmen basiert und keine Daten in den Tresor übertragen werden. Momentaufnahmen werden im gleichen Speicherkonto gespeichert wie die gesicherte Dateifreigabe.

## <a name="set-cross-region-restore"></a>Festlegen der bereichsübergreifenden Wiederherstellung

Als eine der Wiederherstellungsoptionen ermöglicht die bereichsübergreifende Wiederherstellung (Cross Region Restore, CRR) die Wiederherstellung von virtuellen Azure-Computern in einer sekundären Region, bei der es sich um eine [gepaarte Azure-Region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) handelt. Diese Option ermöglicht Folgendes:

- Durchführen von Übungen für Audit- oder Complianceanforderungen
- Wiederherstellen des virtuellen Computers oder dessen Datenträgers im Falle eines Notfalls in der primären Region

Legen Sie zum Auswählen dieses Features auf dem Blatt **Sicherungskonfiguration** die Option **Bereichsübergreifende Wiederherstellung** auf „Aktiviert“ fest.

Dieser Prozess hat Auswirkungen auf den Preis, da er auf der Speicherebene erfolgt.

>[!NOTE]
>Vorbereitungen
>
>- Machen Sie sich anhand der [Unterstützungsmatrix](backup-support-matrix.md#cross-region-restore) mit der Liste unterstützter verwalteter Typen und Regionen vertraut.
>- Das Feature der bereichsübergreifenden Wiederherstellung (CRR) ist nun in allen öffentlichen Azure-Regionen in der Vorschau verfügbar.
>- CRR ist ein optionales Feature auf Tresorebene für beliebige GRS-Tresore und standardmäßig deaktiviert.
>- Nach der Aktivierung kann es bis zu 48 Stunden dauern, bis die Sicherungselemente in sekundären Regionen verfügbar sind.
>- Momentan wird CRR nur für virtuelle Azure-Computer mit dem Sicherungsverwaltungstyp „ARM“ unterstützt. (Klassische virtuelle Azure-Computer werden nicht unterstützt.)  Wenn CRR von weiteren Verwaltungstypen unterstützt wird, werden diese **automatisch** registriert.
>- Die regionsübergreifende Wiederherstellung (CRR) kann derzeit nicht auf GRS oder LRS zurückgesetzt werden, sobald der Schutz zum ersten Mal initiiert wurde.

### <a name="configure-cross-region-restore"></a>Konfigurieren der bereichsübergreifenden Wiederherstellung

Bei einem mit GRS-Redundanz erstellten Tresor kann die bereichsübergreifende Wiederherstellung konfiguriert werden. Jeder GRS-Tresor verfügt über ein Banner, das mit der Dokumentation verknüpft ist. Wenn Sie CRR für den Tresor konfigurieren möchten, navigieren Sie zum Blatt mit der Sicherungskonfiguration. Dort befindet sich die Option zum Aktivieren dieses Features.

 ![Banner für die Sicherungskonfiguration](./media/backup-azure-arm-restore-vms/banner.png)

1. Navigieren Sie im Portal zu „Recovery Services-Tresor“ > „Einstellungen“ > „Eigenschaften“.
2. Klicken Sie unter **Bereichsübergreifende Wiederherstellung** auf „Aktivieren“, um die Funktion zu aktivieren.

   ![Vor Aktivierung der bereichsübergreifenden Wiederherstellung in diesem Tresor](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![Nach Aktivierung der bereichsübergreifenden Wiederherstellung in diesem Tresor](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

Informationen zum Anzeigen von Sicherungselementen in der sekundären Region finden Sie [hier](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region).

Informationen zur Wiederherstellung in der sekundären Region finden Sie [hier](backup-azure-arm-restore-vms.md#restore-in-secondary-region).

Informationen zum Überwachen von Wiederherstellungsaufträgen für die sekundäre Regionen finden Sie [hier](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs).

## <a name="modifying-default-settings"></a>Ändern der Standardeinstellungen

Es wird dringend empfohlen, vor dem Konfigurieren von Sicherungen im Tresor die Standardeinstellungen für **Speicherreplikationstyp** und **Sicherheitseinstellungen** zu überprüfen.

- Der **Speicherreplikationstyp** ist standardmäßig auf **Georedundant** (GRS) festgelegt. Nachdem Sie die Sicherung konfiguriert haben, ist die Option zum Ändern deaktiviert.
  - Wenn Sie noch keine Sicherung konfiguriert haben, [führen Sie die folgenden Schritte aus](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy), um die Einstellungen zu überprüfen und zu ändern.
  - Wenn Sie die Sicherung bereits konfiguriert haben und von georedundantem Speicher (GRS) zu lokal redundantem Speicher (LRS) wechseln müssen, [überprüfen Sie diese Problemumgehungen](#how-to-change-from-grs-to-lrs-after-configuring-backup).

- **Vorläufiges Löschen** ist für neu erstellte Tresore standardmäßig auf **Aktiviert** festgelegt, um das versehentliche oder bösartige Löschen von Sicherungsdaten zu verhindern. [Führen Sie diese Schritte aus](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#enabling-and-disabling-soft-delete), um die Einstellungen zu überprüfen und zu ändern.

### <a name="how-to-change-from-grs-to-lrs-after-configuring-backup"></a>Wechseln von GRS zu LRS nach dem Konfigurieren einer Sicherung

Bevor Sie sich für den Wechsel von GRS zu LRS (lokal redundanter Speicher) entscheiden, wägen Sie Kosten und höhere Dauerhaftigkeit der Daten gegeneinander ab und entscheiden Sie, was für Ihr Szenario am besten geeignet ist. Wenn Sie von GRS zu LRS wechseln müssen, haben Sie zwei Möglichkeiten. Die Möglichkeiten hängen von Ihren geschäftlichen Anforderungen an die Aufbewahrung der Sicherungsdaten ab:

- [Zuvor gesicherte Daten müssen nicht aufbewahrt werden](#dont-need-to-preserve-previous-backed-up-data)
- [Zuvor gesicherte Daten müssen aufbewahrt werden](#must-preserve-previous-backed-up-data)

#### <a name="dont-need-to-preserve-previous-backed-up-data"></a>Zuvor gesicherte Daten müssen nicht aufbewahrt werden

Zum Schutz von Workloads in einem neuen LRS-Tresor müssen der aktuelle Schutz und die Daten im GRS-Tresor gelöscht und die Sicherungen erneut konfiguriert werden.

>[!WARNING]
>Der folgende Vorgang ist destruktiv und kann nicht rückgängig gemacht werden. Alle Sicherungsdaten und Sicherungselemente, die dem geschützten Server zugeordnet sind, werden dauerhaft gelöscht. Gehen Sie daher mit Bedacht vor.

Beenden und Löschen des aktuellen Schutzes für den GRS-Tresor:

1. Deaktivieren Sie in den Eigenschaften des GRS-Tresors das vorläufige Löschen. Führen Sie [die folgenden Schritte](backup-azure-security-feature-cloud.md#disabling-soft-delete-using-azure-portal) aus, um das vorläufige Löschen zu deaktivieren.

1. Beenden Sie den Schutz, und löschen Sie die Sicherungen aus dem vorhandenen GRS-Tresor. Wählen Sie im Dashbordmenü des Tresors die Option **Sicherungselemente** aus. Die hier aufgelisteten Elemente, die in den LRS-Tresor verschoben werden müssen, müssen zusammen mit den zugehörigen Sicherungsdaten entfernt werden. Weitere Informationen finden Sie unter [Löschen von geschützten Elementen in der Cloud](backup-azure-delete-vault.md#delete-protected-items-in-the-cloud) und [Löschen von geschützten lokalen Elementen](backup-azure-delete-vault.md#delete-protected-items-on-premises).

1. Wenn Sie planen, Azure-Dateifreigaben (AFS), SQL-Server oder SAP HANA-Server zu verschieben, müssen Sie auch deren Registrierung aufheben. Wählen Sie im Dashbordmenü des Tresors die Option **Sicherungsinfrastruktur** aus. Weitere Informationen finden Sie unter [Aufheben der Registrierung von SQL-Servern](manage-monitor-sql-database-backup.md#unregister-a-sql-server-instance), [Aufheben der Registrierung von Speicherkonten, die mit Azure-Dateifreigaben verknüpft sind](manage-afs-backup.md#unregister-a-storage-account) und [Aufheben der Registrierung einer SAP HANA-Instanz](sap-hana-db-manage.md#unregister-an-sap-hana-instance).

1. Nachdem Sie diese aus dem GRS-Tresor entfernt haben, fahren Sie mit dem Konfigurieren der Sicherungen für Ihre Workload im neuen LRS-Tresor fort.

#### <a name="must-preserve-previous-backed-up-data"></a>Zuvor gesicherte Daten müssen aufbewahrt werden

Wenn Sie die aktuell geschützten Daten im GRS-Tresor aufbewahren und den Schutz in einem neuen LRS-Tresor fortsetzen müssen, gibt es für einige der Workloads nur eingeschränkte Optionen:

- Bei MARS können Sie [Beendigung des Schutzes mit Beibehaltung der Daten](backup-azure-manage-mars.md#stop-protecting-files-and-folder-backup) auswählen und den Agent im neuen LRS-Tresor registrieren.

  - Der Azure Backup-Dienst behält weiterhin alle vorhandenen Wiederherstellungspunkte des GRS-Tresors bei.
  - Die Beihaltung der Wiederherstellungspunkte im GRS-Tresor ist kostenpflichtig.
  - Für die Wiederherstellung gesicherter Daten können Sie nur nicht abgelaufene Wiederherstellungspunkte im GRS-Tresor auswählen.
  - Im LRS-Tresor muss ein neues Erstreplikat der Daten erstellt werden.

- Bei einem virtuellen Azure-Computer können Sie [Beendigung des Schutzes mit Beibehaltung der Daten](backup-azure-manage-vms.md#stop-protecting-a-vm) für den virtuellen Computer im GRS-Tresor auswählen, den virtuellen Computer in eine andere Ressourcengruppe verschieben und dann den virtuellen Computer im LRS-Tresor schützen. Weitere Informationen zum Verschieben eines virtuellen Computers in eine andere Ressourcengruppe finden Sie unter [Anleitungen und Einschränkungen](https://docs.microsoft.com/azure/azure-resource-manager/management/move-limitations/virtual-machines-move-limitations).

  Ein virtueller Computer kann jeweils nur in einem Tresor geschützt werden. Der virtuelle Computer in der neuen Ressourcengruppe kann jedoch im LRS-Tresor geschützt werden, da er als anderer virtueller Computer betrachtet wird.

  - Der Azure Backup-Dienst behält die Wiederherstellungspunkte bei, die im GRS-Tresor gesichert wurden.
  - Die Beibehaltung der Wiederherstellungspunkte im GRS-Tresor ist kostenpflichtig (Einzelheiten finden Sie unter [Azure Backup – Preise](azure-backup-pricing.md)).
  - Sie können den virtuellen Computer bei Bedarf aus dem GRS-Tresor wiederherstellen.
  - Die erste Sicherung des virtuellen Computers in der neuen Ressourcengruppe im LRS-Tresor erfolgt als Erstreplikat.


## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)
Informieren Sie sich über das [Löschen von Recovery Services-Tresoren](backup-azure-delete-vault.md).
