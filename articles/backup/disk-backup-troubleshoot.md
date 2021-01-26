---
title: Problembehandlung bei Sicherungsfehlern in Azure Disk Backup
description: Informationen zur Problembehandlung bei Sicherungsfehlern in Azure Disk Backup
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 0a2ef1ea20ee8d6b7a3f32e244d3e00f3add80a2
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2021
ms.locfileid: "98557096"
---
# <a name="troubleshooting-backup-failures-in-azure-disk-backup-in-preview"></a>Problembehandlung bei Sicherungsfehlern in Azure Disk Backup (Vorschau)

>[!IMPORTANT]
>Die Vorschauversion von Azure Disk Backup wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Informationen zur regionalen Verfügbarkeit finden Sie in der [Supportmatrix](disk-backup-support-matrix.md).
>
>[Füllen Sie dieses Formular aus](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u), wenn Sie sich für die Vorschau registrieren möchten.

Dieser Artikel enthält Informationen zur Problembehandlung bei Sicherungs- und Wiederherstellungsproblemen mit Azure Disk. Weitere Informationen zur regionalen Verfügbarkeit von [Azure Disk Backup](disk-backup-overview.md), zu unterstützten Szenarien und zu Einschränkungen finden Sie in der [Supportmatrix](disk-backup-support-matrix.md).

## <a name="common-issues-faced-with-azure-disk-backup"></a>Häufige Probleme bei Azure Disk Backup

### <a name="error-code-usererrorsnapshotrgsubscriptionmismatch"></a>Fehlercode: UserErrorSnapshotRGSubscriptionMismatch

Fehlermeldung: Ungültiges Abonnement ausgewählt für Momentaufnahme-Datenspeicher.

Empfohlene Maßnahme: Datenträger und Datenträger-Momentaufnahmen werden im selben Abonnement gespeichert. Sie können eine beliebige Ressourcengruppe auswählen, um die Datenträger-Momentaufnahmen im Abonnement zu speichern. Wählen Sie das gleiche Abonnement wie für den Quelldatenträger aus. Weitere Informationen finden Sie in der [Supportmatrix](disk-backup-support-matrix.md).

### <a name="error-code-usererrorsnapshotrgnotfound"></a>Fehlercode: UserErrorSnapshotRGNotFound

Fehlermeldung: Der Vorgang konnte nicht ausgeführt werden, weil die Ressourcengruppe des Momentaufnahme-Datenspeichers nicht vorhanden ist.

Empfohlene Maßnahme: Erstellen Sie die Ressourcengruppe, und geben Sie die erforderlichen Berechtigungen dafür an. Weitere Informationen finden Sie unter [Konfigurieren der Sicherung](backup-managed-disks.md#configure-backup).

### <a name="error-code-usererrormanageddisknotfound"></a>Fehlercode: UserErrorManagedDiskNotFound

Fehlermeldung: Der Vorgang konnte nicht ausgeführt werden, weil der verwaltete Datenträger nicht mehr vorhanden ist.

Empfohlene Maßnahme: Die Sicherungen werden weiterhin nicht ausgeführt, weil der Quelldatenträger möglicherweise gelöscht oder an einen anderen Speicherort verschoben wurde. Verwenden Sie den vorhandenen Wiederherstellungspunkt, um den Datenträger wiederherzustellen, falls er versehentlich gelöscht wurde. Falls der Datenträger an einen anderen Speicherort verschoben wurde, konfigurieren Sie die Sicherung für den Datenträger.

### <a name="error-code-usererrornotenoughpermissionondisk"></a>Fehlercode: UserErrorNotEnoughPermissionOnDisk

Fehlermeldung: Der Azure Backup-Dienst benötigt zusätzliche Berechtigungen auf dem Datenträger, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Erteilen Sie der verwalteten Identität des Sicherungstresors die entsprechenden Berechtigungen auf dem Datenträger. In der [Dokumentation](backup-managed-disks.md) erfahren Sie, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrornotenoughpermissiononsnapshotrg"></a>Fehlercode: UserErrorNotEnoughPermissionOnSnapshotRG

Fehlermeldung: Der Azure Backup-Dienst benötigt zusätzliche Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Erteilen Sie der verwalteten Identität des Sicherungstresors geeignete Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers. Die Ressourcengruppe des Momentaufnahme-Datenspeichers ist der Speicherort, an dem die Momentaufnahmen der Datenträger gespeichert werden. In der [Dokumentation](backup-managed-disks.md) erfahren Sie, welche Ressourcengruppe verwendet wird, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrordiskbackupdiskormsipermissionsnotpresent"></a>Fehlercode: UserErrorDiskBackupDiskOrMSIPermissionsNotPresent

Fehlermeldung: Der Datenträger ist ungültig, oder der Azure Backup-Dienst benötigt zusätzliche Berechtigungen auf dem Datenträger, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Die Sicherungen werden weiterhin nicht ausgeführt, weil der Quelldatenträger möglicherweise gelöscht oder an einen anderen Speicherort verschoben wurde. Verwenden Sie den vorhandenen Wiederherstellungspunkt, um den Datenträger wiederherzustellen, falls er versehentlich gelöscht wurde. Falls der Datenträger an einen anderen Speicherort verschoben wurde, konfigurieren Sie die Sicherung für den Datenträger. Wenn der Datenträger weder gelöscht noch verschoben wurde, erteilen Sie der verwalteten Identität des Sicherungstresors die entsprechenden Berechtigungen auf dem Datenträger. In der [Dokumentation](backup-managed-disks.md) erfahren Sie, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrordiskbackupsnapshotrgormsipermissionsnotpresent"></a>Fehlercode: UserErrorDiskBackupSnapshotRGOrMSIPermissionsNotPresent

Fehlermeldung: Der Vorgang konnte nicht ausgeführt werden, weil die Ressourcengruppe des Momentaufnahme-Datenspeichers nicht mehr vorhanden ist oder der Azure Backup-Dienst zusätzliche Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers benötigt, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Erstellen Sie eine Ressourcengruppe, und erteilen Sie der verwalteten Identität des Sicherungstresors geeignete Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers. Die Ressourcengruppe des Momentaufnahme-Datenspeichers ist der Speicherort, an dem die Momentaufnahmen der Datenträger gespeichert werden. In der [Dokumentation](backup-managed-disks.md) erfahren Sie, welche Ressourcengruppe verwendet wird, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrordiskbackupauthorizationfailed"></a>Fehlercode: UserErrorDiskBackupAuthorizationFailed

Fehlermeldung: Der verwalteten Identität des Sicherungstresors fehlen die erforderlichen Berechtigungen, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Erteilen Sie der verwalteten Identität des Sicherungstresors geeignete Berechtigungen für den zu sichernden Datenträger und für die Ressourcengruppe des Momentaufnahme-Datenspeichers, in der die Momentaufnahmen gespeichert werden. In der [Dokumentation](backup-managed-disks.md) erfahren Sie, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrorsnapshotrgormsipermissionsnotpresent"></a>Fehlercode: UserErrorSnapshotRGOrMSIPermissionsNotPresent

Fehlermeldung: Der Vorgang konnte nicht ausgeführt werden, weil die Ressourcengruppe des Momentaufnahme-Datenspeichers nicht mehr vorhanden ist oder der Azure Backup-Dienst zusätzliche Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers benötigt, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Erstellen Sie die Ressourcengruppe, und erteilen Sie der verwalteten Identität des Sicherungstresors geeignete Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers. Die Ressourcengruppe des Momentaufnahme-Datenspeichers ist der Speicherort, an dem die Momentaufnahmen gespeichert werden. In der [Dokumentation](backup-managed-disks.md) erfahren Sie, welche Ressourcengruppe verwendet wird, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererroroperationalstoreparametersnotprovided"></a>Fehlercode: UserErrorOperationalStoreParametersNotProvided

Fehlermeldung: Der Vorgang konnte nicht ausgeführt werden, weil der Parameter für die Ressourcengruppe des Momentaufnahme-Datenspeichers nicht angegeben wurde.

Empfohlene Maßnahme: Geben Sie den Parameter für die Ressourcengruppe des Momentaufnahme-Datenspeichers an. Die Ressourcengruppe des Momentaufnahme-Datenspeichers ist der Speicherort, an dem die Momentaufnahmen der Datenträger gespeichert werden. Weitere Informationen finden Sie in der [Dokumentation](backup-managed-disks.md).

### <a name="error-code-usererrorinvalidoperationalstoreresourcegroup"></a>Fehlercode: UserErrorInvalidOperationalStoreResourceGroup

Fehlermeldung: Die bereitgestellte Ressourcengruppe des Momentaufnahme-Datenspeichers ist ungültig.

Empfohlene Maßnahme: Geben Sie eine gültige Ressourcengruppe für den Momentaufnahme-Datenspeicher an. Die Ressourcengruppe des Momentaufnahme-Datenspeichers ist der Speicherort, an dem die Momentaufnahmen der Datenträger gespeichert werden. Weitere Informationen finden Sie in der [Dokumentation](backup-managed-disks.md).

### <a name="error-code-usererrordiskbackupdisktypenotsupported"></a>Fehlercode: UserErrorDiskBackupDiskTypeNotSupported

Fehlermeldung: Nicht unterstützter Datenträgertyp

Empfohlene Maßnahme: Informationen zu nicht unterstützten Szenarien und zu Einschränkungen finden Sie in der [Supportmatrix](disk-backup-support-matrix.md).

### <a name="error-code-usererrorsamenamediskalreadyexists"></a>Fehlercode: UserErrorSameNameDiskAlreadyExists

Fehlermeldung: Ein Datenträger mit demselben Namen ist in der ausgewählten Zielressourcengruppe bereits vorhanden.

Empfohlene Maßnahme: Geben Sie einen anderen Datenträgernamen für die Wiederherstellung an. Weitere Informationen finden Sie unter [Wiederherstellen von Azure Managed Disks](restore-managed-disks.md).

### <a name="error-code-usererrorrestoretargetrgnotfound"></a>Fehlercode: UserErrorRestoreTargetRGNotFound

Fehlermeldung: Fehler beim Vorgang, weil die Zielressourcengruppe nicht vorhanden ist.

Empfohlene Maßnahme: Geben Sie eine gültige Ressourcengruppe für die Wiederherstellung an. Weitere Informationen finden Sie unter [Wiederherstellen von Azure Managed Disks](restore-managed-disks.md).

### <a name="error-code-usererrornotenoughpermissionontargetrg"></a>Fehlercode: UserErrorNotEnoughPermissionOnTargetRG

Fehlermeldung: Der Azure Backup-Dienst benötigt zusätzliche Berechtigungen für die Zielressourcengruppe, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Erteilen Sie der verwalteten Identität des Sicherungstresors geeignete Berechtigungen für die Zielressourcengruppe. Die Zielressourcengruppe ist der ausgewählte Speicherort, an dem der Datenträger wiederhergestellt werden soll. In der [Dokumentation zur Wiederherstellung](restore-managed-disks.md) erfahren Sie, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrorsubscriptiondiskquotalimitreached"></a>Fehlercode: UserErrorSubscriptionDiskQuotaLimitReached

Fehlermeldung: Fehler beim Vorgang, weil die Obergrenze des Datenträgerkontingents für das Abonnement erreicht wurde.

Empfohlene Maßnahme: Weitere Informationen finden Sie in der [Dokumentation zu Grenzwerten und Kontingenten für Azure-Abonnements und -Dienste](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits), oder wenden Sie sich an den Microsoft-Support.

### <a name="error-code-usererrordiskbackuprestorergormsipermissionsnotpresent"></a>Fehlercode: UserErrorDiskBackupRestoreRGOrMSIPermissionsNotPresent

Fehlermeldung: Fehler beim Vorgang, weil die Zielressourcengruppe nicht vorhanden ist oder der Azure Backup-Dienst zusätzliche Berechtigungen für die Ressourcengruppe des Momentaufnahme-Datenspeichers benötigt, um diesen Vorgang auszuführen.

Empfohlene Maßnahme: Geben Sie eine gültige Ressourcengruppe für die Wiederherstellung an, und erteilen Sie der verwalteten Identität des Sicherungstresors geeignete Berechtigungen für die Zielressourcengruppe. Die Zielressourcengruppe ist der ausgewählte Speicherort, an dem der Datenträger wiederhergestellt werden soll. In der [Dokumentation zur Wiederherstellung](restore-managed-disks.md) erfahren Sie, welche Berechtigungen für die verwaltete Identität des Sicherungstresors erforderlich sind und wie diese bereitgestellt werden.

### <a name="error-code-usererrordeskeyvaultkeydisabled"></a>Fehlercode: UserErrorDESKeyVaultKeyDisabled

Fehlermeldung: Der für den Datenträgerverschlüsselungssatz verwendete Schlüsseltresorschlüssel muss aktiviert sein.

Empfohlene Maßnahme: Aktivieren Sie den Schlüsseltresorschlüssel, der für den Datenträgerverschlüsselungssatz verwendet wird. Beachten Sie die Einschränkungen in der [Supportmatrix](disk-backup-support-matrix.md).

### <a name="error-code-usererrormsipermissionsnotpresentondes"></a>Fehlercode: UserErrorMSIPermissionsNotPresentOnDES

Fehlermeldung: Der Azure Backup-Dienst benötigt eine Berechtigung für den Zugriff auf den Datenträgerverschlüsselungssatz, der für den Datenträger verwendet wird.

Empfohlene Maßnahme: Gewähren Sie Lesezugriff auf die verwaltete Identität des Sicherungstresors für den Datenträgerverschlüsselungssatz (DES).

### <a name="error-code-usererrordeskeyvaultkeynotavailable"></a>Fehlercode: UserErrorDESKeyVaultKeyNotAvailable

Fehlermeldung: Der für den Datenträgerverschlüsselungssatz verwendete Schlüsseltresorschlüssel ist nicht verfügbar.

Empfohlene Maßnahme: Stellen Sie sicher, dass der für den Datenträgerverschlüsselungssatz verwendete Schlüsseltresorschlüssel nicht deaktiviert oder gelöscht wurde.

### <a name="error-code-usererrordisksnapshotnotfound"></a>Fehlercode: UserErrorDiskSnapshotNotFound

Fehlermeldung: Die Datenträger-Momentaufnahme für diesen Wiederherstellungspunkt wurde gelöscht.

Empfohlene Maßnahme: Momentaufnahmen werden in der Ressourcengruppe des Momentaufnahme-Datenspeichers innerhalb Ihres Abonnements gespeichert. Möglicherweise wurde die Momentaufnahme für den ausgewählten Wiederherstellungspunkt gelöscht oder aus dieser Ressourcengruppe verschoben. Wählen Sie ggf. einen anderen Wiederherstellungspunkt für die Wiederherstellung aus. Befolgen Sie außerdem die in der [Dokumentation zur Wiederherstellung](restore-managed-disks.md) empfohlenen Richtlinien für die Auswahl der Ressourcengruppe für die Momentaufnahme.

### <a name="error-code-usererrorsnapshotmetadatanotfound"></a>Fehlercode: UserErrorSnapshotMetadataNotFound

Fehlermeldung: Die Metadaten der Datenträger-Momentaufnahme für diesen Wiederherstellungspunkt wurden gelöscht.

Empfohlene Maßnahme: Wählen Sie ggf. einen anderen Wiederherstellungspunkt für die Wiederherstellung aus. Weitere Informationen finden Sie in der [Dokumentation zur Wiederherstellung](restore-managed-disks.md).

### <a name="error-code-usererrormaxconcurrentoperationlimitreached"></a>Fehlercode: UserErrorMaxConcurrentOperationLimitReached

Fehlermeldung: Der Vorgang kann nicht gestartet werden, weil die maximale Anzahl zulässiger gleichzeitiger Vorgänge für diesen Vorgangstyp erreicht wurde.

Empfohlene Maßnahme: Warten Sie, bis die vorherigen Vorgänge abgeschlossen sind.

## <a name="next-steps"></a>Nächste Schritte

- [Supportmatrix für die Azure Disk-Sicherung](disk-backup-support-matrix.md)
