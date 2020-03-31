---
title: Wiederherstellung einer Data Warehouse-Instanz von einer Geosicherung
description: Leitfaden für die Geowiederherstellung eines SQL-Pools
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/12/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 4390ed39c86e041d3fbd776415f0ffbe71f605bd
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350162"
---
# <a name="geo-restore-for-sql-pool"></a>Geowiederherstellung für SQL-Pool

In diesem Artikel erfahren Sie, wie Sie Ihren SQL-Pool aus einer Geosicherung über das Azure-Portal und PowerShell wiederherstellen.

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**Überprüfen Sie Ihre DTU-Kapazität.** Jeder SQL-Pool wird von einer SQL Server-Instanz gehostet (z. B. „myserver.database.windows.net“), die über ein Standard-DTU-Kontingent verfügt. Vergewissern Sie sich, dass die SQL Server-Instanz über ein ausreichendes DTU-Kontingent für die Datenbankwiederherstellung verfügt. Informationen zum Berechnen des DTU-Bedarfs bzw. zur Anforderung weiterer DTUs finden Sie unter [Anfordern einer DTU-Kontingentänderung](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-from-an-azure-geographical-region-through-powershell"></a>Wiederherstellen von einer geografischen Azure-Region mithilfe von PowerShell

Verwenden Sie für die Wiederherstellung aus einer Geosicherung die Cmdlets [Get-AzSqlDatabaseGeoBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasegeobackup) und [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase).

> [!NOTE]
> Sie können eine Geowiederherstellung nach Gen2 durchführen! Geben Sie zu diesem Zweck als optionalen Parameter einen ServiceObjectiveName-Wert für Gen2 ein (z.B. DW1000**c**).
>

1. Bevor Sie beginnen, müssen Sie [Azure PowerShell installieren](https://docs.microsoft.com/powershell/azure/overview).
2. Öffnen Sie PowerShell.
2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
3. Wählen Sie das Abonnement aus, in dem das wiederherzustellende Data Warehouse enthalten ist.
4. Rufen Sie das Data Warehouse ab, das Sie wiederherstellen möchten.
5. Erstellen Sie die Wiederherstellungsanforderung für das Data Warehouse.
6. Überprüfen Sie den Status des mittels Geowiederherstellung wiederhergestellten das Data Warehouse.
7. Informationen zum Konfigurieren des Data Warehouse nach Abschluss der Wiederherstellung finden Sie unter [Konfigurieren der Datenbank nach der Wiederherstellung]( ../../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$TargetResourceGroupName="<YourTargetResourceGroupName>" # Restore to a different logical server.
$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"
$TargetServiceObjective="<YourTargetServiceObjective-DWXXXc>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName
Get-AzureSqlDatabase -ServerName $ServerName

# Get the data warehouse you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Recover data warehouse
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName $TargetServiceObjective

# Verify that the geo-restored data warehouse is online
$GeoRestoredDatabase.status
```

Für die wiederhergestellte Datenbank ist TDE aktiviert, wenn für die Quelldatenbank TDE aktiviert ist.

## <a name="restore-from-an-azure-geographical-region-through-azure-portal"></a>Wiederherstellen von einer geografischen Azure-Region über das Azure-Portal

Führen Sie die folgenden Schritte aus, um einen SQL-Pool aus einer Geosicherung wiederherzustellen:

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com/)-Konto an.
1. Klicken Sie auf **+ Ressource erstellen**. 

![Neues Data Warehouse](./media/sql-data-warehouse-restore-from-geo-backup/georestore-new.png)

3. Klicken Sie auf **Datenbanken** und dann auf „** Azure Synapse Analytics (vormals SQL DW) **“.

![Neues DW 2](./media/sql-data-warehouse-restore-from-geo-backup/georestore-new-02.png)

4. Geben Sie auf der Registerkarte **Grundlagen** die angeforderten Informationen ein, und klicken Sie auf **Weiter: Zusätzliche Einstellungen**.

![Grundlagen](./media/sql-data-warehouse-restore-from-geo-backup/georestore-dw-1.png)

5. Wählen Sie für den Parameter **Vorhandenen Daten verwenden** die Option **Sicherung** aus, und wählen Sie dann die entsprechende Sicherung aus den Scrolldownoptionen aus. Klicken Sie auf **Überprüfen und erstellen**.
 
![Sicherung](./media/sql-data-warehouse-restore-from-geo-backup/georestore-select.png)

6. Nachdem das Data Warehouse wieder hergestellt wurde, überprüfen Sie, ob der **Status** „Online“ lautet.

## <a name="next-steps"></a>Nächste Schritte
- [Wiederherstellen eines vorhandenen SQL-Pools](sql-data-warehouse-restore-active-paused-dw.md)
- [Wiederherstellen eines gelöschten SQL-Pools](sql-data-warehouse-restore-deleted-dw.md)