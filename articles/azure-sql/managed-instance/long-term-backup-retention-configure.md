---
title: 'Azure SQL Managed Instance: Langfristige Sicherungsaufbewahrung'
description: Erfahren Sie, wie Sie mithilfe von PowerShell automatisierte Sicherungen in separaten Azure Blob Storage-Containern für Azure SQL Managed Instance speichern und wiederherstellen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: shkale-msft
ms.author: shkale
ms.reviewer: mathoma, sstein
ms.date: 02/25/2021
ms.openlocfilehash: f298f0f9d76750be932db79b5a08b6385e984f88
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052025"
---
# <a name="manage-azure-sql-managed-instance-long-term-backup-retention-powershell"></a>Verwalten der langfristigen Sicherungsaufbewahrung für Azure SQL Managed Instance (PowerShell)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In Azure SQL Managed Instance können Sie eine Richtlinie für die [langfristige Sicherungsaufbewahrung](../database/long-term-retention-overview.md) (Long-Term Retention, LTR) als öffentliche Previewfunktion konfigurieren. Dies ermöglicht es Ihnen, Datenbanksicherungen automatisch bis zu zehn Jahre in separaten Azure Blob Storage-Containern beizubehalten. Sie können dann mit PowerShell eine Datenbank anhand dieser Sicherungen wiederherstellen.

   > [!IMPORTANT]
   > LTR für verwaltete Instanzen ist zurzeit in der öffentlichen Vorschau in öffentlichen Azure-Regionen verfügbar. 

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe von PowerShell die langfristige Sicherungsaufbewahrung konfigurieren, Sicherungen in Azure SQL-Speicher anzeigen und eine Sicherung in Azure SQL-Speicher wiederherstellen.


## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

In den folgenden Abschnitten erfahren Sie, wie Sie das Azure-Portal verwenden, um Richtlinien für die Langzeitaufbewahrung festzulegen, verfügbare Sicherungen für die Langzeitaufbewahrung verwalten und eine Wiederherstellung aus einer verfügbaren Sicherung durchführen.

### <a name="configure-long-term-retention-policies"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung

Sie können SQL Managed Instance zur [Aufbewahrung von automatisierten Sicherungen](../database/long-term-retention-overview.md) für einen längeren Zeitraum konfigurieren, als gemäß der Aufbewahrungsdauer für Ihre Dienstebene vorgesehen ist.

1. Wählen Sie im Azure-Portal Ihre verwaltete Instanz aus, und klicken Sie dann auf **Sicherungen**. Wählen Sie auf der Registerkarte **Aufbewahrungsrichtlinien** die Datenbank(en) aus, für die Sie Richtlinien für die langfristige Sicherungsaufbewahrung festlegen oder ändern möchten. Änderungen gelten nicht für Datenbanken, die nicht ausgewählt wurden. 

   ![Link zum Verwalten von Sicherungen](./media/long-term-backup-retention-configure/ltr-configure-ltr.png)

2. Geben Sie im Bereich **Richtlinien konfigurieren** den gewünschten Aufbewahrungszeitraum für wöchentliche, monatliche oder jährliche Sicherungen an. Wählen Sie den Aufbewahrungszeitraum „0“ aus, um anzugeben, dass keine Langzeitaufbewahrung von Sicherungen festgelegt werden soll.

   ![Konfigurieren von Richtlinien](./media/long-term-backup-retention-configure/ltr-configure-policies.png)

3. Klicken Sie auf **Anwenden**, wenn Sie fertig sind.

> [!IMPORTANT]
> Wenn Sie eine Richtlinie für die Langzeitaufbewahrung von Sicherungen aktivieren, kann es bis zu 7 Tage dauern, bevor die erste Sicherung angezeigt wird und wiederhergestellt werden kann. Weitere Informationen dazu, in welchen Intervallen Sicherungskopien für die Langzeitaufbewahrung erstellt werden, finden Sie unter [Langzeitaufbewahrung von Sicherungen](../database/long-term-retention-overview.md).

### <a name="view-backups-and-restore-from-a-backup"></a>Anzeigen von Sicherungen und Wiederherstellen einer Sicherung

Zeigen Sie die Sicherungen an, die für eine bestimmte Datenbank mit einer LTR-Richtlinie aufbewahrt werden, und führen Sie die Wiederherstellung aus diesen Sicherungen aus.

1. Wählen Sie im Azure-Portal Ihre verwaltete Instanz aus, und klicken Sie dann auf **Sicherungen**. Wählen Sie auf der Registerkarte **Verfügbare Sicherungen** die Datenbank aus, für die Sie verfügbare Sicherungen anzeigen möchten. Klicken Sie auf **Manage**.

   ![Auswählen der Datenbank](./media/long-term-backup-retention-configure/ltr-available-backups-select-database.png)

1. Überprüfen Sie im Bereich **Sicherungen verwalten** die verfügbaren Sicherungen.

   ![Anzeigen von Sicherungen](./media/long-term-backup-retention-configure/ltr-available-backups.png)

1. Wählen Sie die Sicherung aus, die Sie wiederherstellen möchten, klicken Sie auf **Wiederherstellen**, geben Sie dann auf der Seite „Wiederherstellen“ den neuen Datenbanknamen an. Die Sicherung und die Quelle werden auf dieser Seite vorab ausgefüllt. 

   ![Auswählen der Sicherung für die Wiederherstellung](./media/long-term-backup-retention-configure/ltr-available-backups-restore.png)
   
   ![Wiederherstellen](./media/long-term-backup-retention-configure/ltr-restore.png)

1. Klicken Sie auf **Überprüfen + erstellen**, um die Wiederherstellungsdetails zu überprüfen. Klicken Sie dann auf **Erstellen**, um die Datenbank aus der ausgewählten Sicherung wiederherzustellen.

1. Klicken Sie auf der Symbolleiste auf das Benachrichtigungssymbol, um den Status des Wiederherstellungsauftrags anzuzeigen.

   ![Status des Wiederherstellungsauftrags](./media/long-term-backup-retention-configure/restore-job-progress-long-term.png)

1. Öffnen Sie nach Abschluss des Wiederherstellungsauftrags die Seite **Übersicht über verwaltete Instanzen**, um die neu wiederhergestellte Datenbank anzuzeigen.

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen](../database/recovery-using-backups.md#point-in-time-restore).


## <a name="using-powershell"></a>PowerShell
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, jedoch erfolgen zukünftige Entwicklungen im Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch.

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe von PowerShell die langfristige Sicherungsaufbewahrung konfigurieren, Sicherungen in Azure Storage anzeigen und eine Wiederherstellung aus einer Sicherung in Azure Storage ausführen.

### <a name="azure-rbac-roles-to-manage-long-term-retention"></a>Azure RBAC-Rollen zum Verwalten der langfristigen Aufbewahrung

Für **Get-AzSqlInstanceDatabaseLongTermRetentionBackup** und **Restore-AzSqlInstanceDatabase** müssen Sie über eine der folgenden Rollen verfügen:

- Rolle „Besitzer des Abonnements“ oder
- Rolle „Mitwirkender“ der verwalteten Instanz oder
- Benutzerdefinierte Rolle mit den folgenden Berechtigungen:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/read`

Für **Remove-AzSqlInstanceDatabaseLongTermRetentionBackup** müssen Sie über eine der folgenden Rollen verfügen:

- Rolle „Besitzer des Abonnements“ oder
- Benutzerdefinierte Rolle mit der folgenden Berechtigung:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

> [!NOTE]
> Die Rolle „Mitwirkender“ für die verwaltete Instanz besitzt keine Berechtigung zum Löschen von LTR-Sicherungen.

Azure RBAC-Berechtigungen können im Bereich *Abonnement* oder *Ressourcengruppe* erteilt werden. Für den Zugriff auf LTR-Sicherungen, die zu einer gelöschten Instanz gehören, muss die Berechtigung jedoch im Bereich *Abonnement* dieser Instanz erteilt werden.

- `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

### <a name="create-an-ltr-policy"></a>Erstellen einer LTR-Richtlinie

```powershell
# get the Managed Instance
$subId = "<subscriptionId>"
$instanceName = "<instanceName>"
$resourceGroup = "<resourceGroupName>"
$dbName = "<databaseName>"

Connect-AzAccount

Select-AzSubscription -SubscriptionId $subId

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    WeeklyRetention = 'P12W'
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    WeeklyRetention = 'P12W' 
    YearlyRetention = 'P5Y' 
    WeekOfYear = '16'
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy
```

### <a name="view-ltr-policies"></a>Anzeigen von LTR-Richtlinien

In diesem Beispiel wird gezeigt, wie Sie die LTR-Richtlinien in einer Instanz für eine Einzeldatenbank auflisten.

```powershell
# gets the current version of LTR policy for a database
$LTRPolicies = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup
}
Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy 
```

In diesem Beispiel wird gezeigt, wie Sie die LTR-Richtlinien in einer Instanz für alle Datenbanken auflisten.

```powershell
# gets the current version of LTR policy for all of the databases on an instance

$Databases = Get-AzSqlInstanceDatabase -ResourceGroupName $resourceGroup -InstanceName $instanceName

$LTRParams = @{
    InstanceName = $instanceName
    ResourceGroupName = $resourceGroup
}

foreach($database in $Databases.Name){
    Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRParams  -DatabaseName $database
 }
```

### <a name="clear-an-ltr-policy"></a>Löschen einer LTR-Richtlinie

In diesem Beispiel wird gezeigt, wie Sie eine LTR-Richtlinie aus einer Datenbank löschen.

```powershell
# remove the LTR policy from a database
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    RemovePolicy = $true
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy
```

### <a name="view-ltr-backups"></a>Anzeigen von LTR-Sicherungen

In diesem Beispiel wird gezeigt, wie Sie die LTR-Sicherungen in einer Instanz auflisten.

```powershell

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# get the list of all LTR backups in a specific Azure region
# backups are grouped by the logical database id, within each group they are ordered by the timestamp, the earliest backup first
Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location

# get the list of LTR backups from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# get the LTR backups for a specific database from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbName
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# list LTR backups only from live databases (you have option to choose All/Live/Deleted)
$LTRBackupParam = @{
    Location = $instance.Location 
    DatabaseState = 'Live'
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# only list the latest LTR backup for each database
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    OnlyLatestPerDatabase = $true
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam 
```

### <a name="delete-ltr-backups"></a>Löschen von LTR-Sicherungen

Dieses Beispiel zeigt, wie Sie eine LTR-Sicherung aus der Liste der Sicherungen löschen.

```powershell
# remove the earliest backup
# get the LTR backups for a specific database from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbName
}
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam
$ltrBackup = $ltrBackups[0]
Remove-AzSqlInstanceDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> Das Löschen der LTR-Sicherung kann nicht rückgängig gemacht werden. Wenn Sie eine LTR-Sicherung löschen möchten, nachdem die Instanz gelöscht wurde, müssen Sie über die Berechtigung im Bereich „Abonnement“ verfügen. Sie können Benachrichtigungen zu jedem Löschvorgang in Azure Monitor einrichten, indem Sie nach dem Vorgang „Löscht eine Sicherung zur langfristigen Aufbewahrung“ filtern. Das Aktivitätsprotokoll enthält Informationen dazu, wer die Anforderung vorgenommen hat und wann sie erfolgt ist. Ausführliche Anweisungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../../azure-monitor/alerts/alerts-activity-log.md).

### <a name="restore-from-ltr-backups"></a>Wiederherstellen von LTR-Sicherungen

Dieses Beispiel zeigt, wie Sie eine LTR-Sicherung wiederherstellen. Beachten Sie, dass sich diese Schnittstelle nicht geändert hat, der Parameter für die Ressourcen-ID jetzt jedoch die Ressourcen-ID der LTR-Sicherung erfordert.

```powershell
# restore a specific LTR backup as an P1 database on the instance $instanceName of the resource group $resourceGroup
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbname
    OnlyLatestPerDatabase = $true
}
$ltrBackup = Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam 

$RestoreLTRParam = @{
    TargetInstanceName          = $instanceName 
    TargetResourceGroupName     = $resourceGroup 
    TargetInstanceDatabaseName  = $dbName
    FromLongTermRetentionBackup = $true
    ResourceId                  = $ltrBackup.ResourceId 
}
Restore-AzSqlInstanceDatabase @RestoreLTRParam
```

> [!IMPORTANT]
> Um die Wiederherstellung aus einer LTR-Sicherung nach dem Löschen der Instanz durchführen zu können, müssen Sie über Berechtigungen im Bereich des Abonnements der Instanz verfügen, und dieses Abonnement muss aktiv sein. Sie müssen außerdem den optionalen Parameter -ResourceGroupName auslassen.

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen. Siehe [Point-in-Time-Wiederherstellung](../database/recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](../database/automated-backups-overview.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](../database/long-term-retention-overview.md).
