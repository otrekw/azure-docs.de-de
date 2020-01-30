---
title: Benutzerdefinierte Wiederherstellungspunkte
description: Vorgehensweise beim Erstellen eines Wiederherstellungspunkts für Azure SQL Data Warehouse.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/03/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: bf092b5b6c6eb88b565a940de56d614426e34d8e
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759600"
---
# <a name="user-defined-restore-points"></a>Benutzerdefinierte Wiederherstellungspunkte

In diesem Artikel erfahren Sie, wie Sie mithilfe von PowerShell und Azure-Portal einen neuen benutzerdefinierten Wiederherstellungspunkt für Azure SQL Data Warehouse erstellen.

## <a name="create-user-defined-restore-points-through-powershell"></a>Erstellen benutzerdefinierter Wiederherstellungspunkte mit PowerShell

Verwenden Sie zum Erstellen eines benutzerdefinierten Wiederherstellungspunkts das PowerShell-Cmdlet [New-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint?view=azps-2.4.0).

1. Bevor Sie beginnen, müssen Sie [Azure PowerShell installieren](https://docs.microsoft.com/powershell/azure/overview).
2. Öffnen Sie PowerShell.
3. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.
4. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.
5. Erstellen Sie einen Wiederherstellungspunkt für eine sofortige Kopie Ihrer Data Warehouse-Instanz.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$Label = "<YourRestorePointLabel>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

```

6. Sehen Sie sich die Liste aller vorhandenen Wiederherstellungspunkte an.

```Powershell
# List all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName
```

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Erstellen benutzerdefinierter Wiederherstellungspunkte mit dem Azure-Portal

Benutzerdefinierte Wiederherstellungspunkte können auch mit dem Azure-Portal erstellt werden.

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com/)-Konto an.

2. Navigieren Sie zu der SQL Data Warehouse-Instanz, für die Sie einen Wiederherstellungspunkt erstellen möchten.

3. Wählen Sie im linken Bereich **Übersicht** aus, und wählen Sie **+ Neuer Wiederherstellungspunkt** aus. Wenn die Schaltfläche „Neuer Wiederherstellungspunkt“ nicht aktiviert ist, stellen Sie sicher, dass Data Warehouse nicht angehalten ist.

    ![Neuer Wiederherstellungspunkt](./media/sql-data-warehouse-restore-points/creating-restore-point-01.png)

4. Geben Sie einen Namen für den benutzerdefinierten Wiederherstellungspunkt ein, und klicken Sie auf **Übernehmen**. Für benutzerdefinierte Wiederherstellungspunkte gilt eine standardmäßige Aufbewahrungsdauer von sieben Tagen.

    ![Name für den Wiederherstellungspunkt](./media/sql-data-warehouse-restore-points/creating-restore-point-11.png)

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen einer vorhandenen Azure SQL Data Warehouse-Instanz](sql-data-warehouse-restore-active-paused-dw.md)
- [Wiederherstellen einer gelöschten Azure SQL Data Warehouse-Instanz](sql-data-warehouse-restore-deleted-dw.md)
- [Geowiederherstellung einer Azure SQL Data Warehouse-Instanz](sql-data-warehouse-restore-from-geo-backup.md)

