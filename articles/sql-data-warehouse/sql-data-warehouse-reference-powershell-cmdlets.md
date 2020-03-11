---
title: PowerShell und REST-APIs
description: Hier finden Sie die wichtigsten PowerShell-Cmdlets für den Azure Synapse Analytics-SQL-Pool, einschließlich der Cmdlets zum Anhalten und Fortsetzen einer Datenbank.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c0c8b1e9b7526bd45d037f053715613b53ec163f
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198455"
---
# <a name="powershell--rest-apis-for-azure-synapse-analytics-sql-pool"></a>PowerShell und REST-APIs für Azure Synapse Analytics-SQL-Pool

Viele Verwaltungsaufgaben für den Azure Synapse Analytics-SQL-Pool können entweder mithilfe von Azure PowerShell-Cmdlets oder REST-APIs erfolgen.  Die folgenden Beispiele zeigen, wie PowerShell-Befehle verwendet werden, um häufige Aufgaben in Ihrem SQL-Pool zu automatisieren.  Einige gute REST-Beispiele finden Sie im Artikel [Verwalten von Computeleistung in Azure SQL Data Warehouse (REST)](sql-data-warehouse-manage-compute-rest-api.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-Cmdlets

1. Öffnen Sie Windows PowerShell.
2. Zum Anmelden bei Azure Resource Manager und Auswählen Ihres Abonnements führen Sie bei der PowerShell-Eingabeaufforderung die folgenden Befehle aus:
   
    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-data-warehouse-example"></a>Beispiel für das Anhalten eines Data Warehouse
Halten Sie eine Datenbank mit dem Namen „Database02“ an, die auf einem Server mit dem Namen „Server01“ gehostet wird.  Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```

Variation: Dieses Beispiel leitet das abgerufene Objekt an [Suspend-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase) weiter.  Dadurch wird die Datenbank angehalten. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-data-warehouse-example"></a>Beispiel für das Starten eines Data Warehouse

Setzen Sie den Betrieb der auf dem Server „Server01“ gehosteten Datenbank „Database02“ fort. Der Server befindet sich in einer Ressourcengruppe namens „ResourceGroup1“.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Abweichung: Eine Datenbank „Database02“ von einem Server „Server01“, der sich in einer Ressourcengruppe „ResourceGroup1“ befindet, wird abgerufen. Das abgerufene Objekt wird an [Resume-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase) weitergeleitet.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Hinweis: Wenn Ihr Server „foo.database.windows.net“ heißt, verwenden Sie in den PowerShell-Cmdlets für „-ServerName“ den Namen „foo“.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Andere unterstützte PowerShell-Cmdlets
Diese PowerShell-Cmdlets werden für ein Azure Synapse Analytics Data Warehouse unterstützt.

* [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase)
* [Get-AzSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup)
* [Get-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserestorepoint)
* [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
* [Remove-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabase)
* [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)
* [Resume-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase)
* [Select-AzSubscription](https://msdn.microsoft.com/library/dn722499.aspx)
* [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
* [Suspend-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase)

## <a name="next-steps"></a>Nächste Schritte
Weitere PowerShell-Beispiele finden Sie unter:

* [Erstellen eines Data Warehouse mit PowerShell](create-data-warehouse-powershell.md)
* [Datenbankwiederherstellung](sql-data-warehouse-restore-database-powershell.md)

Weitere Aufgaben, die mit PowerShell automatisiert werden können, finden Sie unter [Azure SQL-Datenbank-Cmdlets](https://docs.microsoft.com/powershell/module/az.sql). Nicht alle Azure SQL-Datenbank-Cmdlets werden für in Azure Synapse Analytics Data Warehouse unterstützt.  Eine Liste der Aufgaben, die mit REST automatisiert werden können, finden Sie unter [Vorgänge für Azure SQL-Datenbank](/rest/api/sql/).
