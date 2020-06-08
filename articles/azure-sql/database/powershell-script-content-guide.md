---
title: Azure PowerShell-Skriptbeispiele
description: Azure PowerShell-Skriptbeispiele zum Erstellen und Verwalten von Azure SQL-Datenbank-Ressourcen und Ressourcen verwalteter Azure SQL-Instanzen
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: 93fac2e3ae8d83f0fa49f94a3ba7880a2ecc2b4c
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193902"
---
# <a name="azure-powershell-samples-for-azure-sql-database-and-azure-sql-managed-instance"></a>Azure PowerShell-Beispiele für Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen ermöglichen Ihnen das Konfigurieren Ihrer Datenbanken, Instanzen und Pools mithilfe von Azure PowerShell.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 1.4.0 von Azure PowerShell verwenden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="azure-sql-database"></a>[Azure SQL-Datenbank](#tab/single-database)

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure SQL-Datenbank.

| |  |
|---|---|
|**Erstellen und Konfigurieren von Einzeldatenbanken und Pools für elastische Datenbanken**||
| [Erstellen einer einzelnen Datenbank und Konfigurieren einer Firewallregel auf Serverebene](scripts/create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript erstellt eine einzelne Datenbank und konfiguriert eine IP-Firewallregel auf Serverebene. |
| [Create elastic pools and move databases between pools and out of a pool using PowerShell](scripts/move-database-between-elastic-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool mit PowerShell) | Dieses PowerShell-Skript erstellt Pools für elastische Datenbanken, verschiebt Pooldatenbanken und ändert Computegrößen.|
|**Konfigurieren von Georeplikation und Failover**||
| [Configure and failover a single database using active geo-replication](scripts/setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Konfiguration und Failover einer einzelnen Datenbank mithilfe von aktiver Georeplikation)| Dieses PowerShell-Skript konfiguriert die aktive Georeplikation für eine einzelne Datenbank und führt ein Failover zum sekundären Replikat aus. |
| [Configure and failover a pooled database using active geo-replication](scripts/setup-geodr-and-failover-elastic-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Konfiguration und Failover einer Pooldatenbank mithilfe von aktiver Georeplikation)| Dieses PowerShell-Skript konfiguriert die aktive Georeplikation für eine in einem Pool für elastische Datenbanken enthaltene Datenbank und führt ein Failover zum sekundären Replikat aus. |
|**Konfigurieren einer Failovergruppe**||
| [Konfigurieren einer Failovergruppe für eine einzelne Datenbank](scripts/add-database-to-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript erstellt eine Datenbank und eine Failovergruppe, fügt die Datenbank zur Failovergruppe hinzu und testet das Failover auf dem sekundären Server. |
| [Konfigurieren einer Failovergruppe für einen Pool für elastische Datenbanken](scripts/add-elastic-pool-to-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript erstellt eine Datenbank, fügt die Datenbank zu einem Pool für elastische Datenbanken hinzu, fügt den Pool für elastische Datenbanken zur Failovergruppe hinzu und testet das Failover auf dem sekundären Server. |
|**Skalieren einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Monitor and scale a single SQL database using the Azure CLI](scripts/monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Überwachen und Skalieren einer einzelnen SQL-Datenbank mit Azure CLI) | Dieses PowerShell-Skript überwacht die Leistungsmetriken einer einzelnen Datenbank, skaliert sie auf eine höhere Computegröße und erstellt eine Warnungsregel zu einer der Leistungsmetriken. |
| [Scale an elastic pool in Azure SQL Database using the Azure CLI](scripts/monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank mit Azure CLI) | Dieses PowerShell-Skript überwacht die Leistungsmetriken eines Pools für elastische Datenbanken, skaliert ihn auf eine höhere Computegröße und erstellt eine Warnungsregel auf einer der Leistungsmetriken. |
| **Überwachung und Bedrohungserkennung** |
| [Configure SQL Database auditing and threat detection using PowerShell](scripts/auditing-threat-detection-powershell-configure.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Konfigurieren der Überwachung und Bedrohungserkennung für SQL-Datenbank mit PowerShell)| Dieses PowerShell-Skript konfiguriert Richtlinien zur Überwachung und Bedrohungserkennung für eine Datenbank. |
| **Wiederherstellen, Kopieren und Importieren einer Datenbank**||
| [Restore a SQL database using PowerShell (Wiederherstellen einer SQL-Datenbank mit PowerShell)](scripts/restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dieses PowerShell-Skript stellt eine Datenbank auf Basis einer georedundanten Sicherung und eine gelöschte Datenbank gemäß der letzten Sicherung wieder her. |
| [Copy a SQL database to a new server using PowerShell](scripts/copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell)| Dieses PowerShell-Skript erstellt eine Kopie einer vorhandenen Datenbank auf einem neuen Server. |
| [Import from a bacpac into a SQL database using PowerShell](scripts/import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Importieren aus einer BACPAC-Datei in eine SQL-Datenbank mit PowerShell)| In diesem PowerShell-Skript wird eine Datenbank aus einer BACPAC-Datei in eine Azure SQL-Datenbank-Instanz importiert. |
| **Synchronisieren von Daten zwischen Datenbanken**||
| [Synchronisieren von Daten zwischen SQL-Datenbanken](scripts/sql-data-sync-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | In diesem PowerShell-Skript wird die Datensynchronisierung für die Synchronisierung zwischen mehreren Datenbanken in Azure SQL-Datenbank konfiguriert. |
| [Synchronisieren von Daten zwischen Azure SQL-Datenbank und SQL Server](scripts/sql-data-sync-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript konfiguriert die Azure SQL-Datensynchronisierung für die Synchronisierung von Datenbanken in Azure SQL-Datenbank und SQL Server. |
| [Aktualisieren des Synchronisierungsschemas der SQL-Datensynchronisierung](scripts/update-sync-schema-in-sync-group.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript fügt dem Synchronisierungsschema der Datensynchronisierung Elemente hinzu oder entfernt sie daraus. |
|||

Erfahren Sie mehr über die [Azure PowerShell-API für Einzeldatenbanken](single-database-manage.md#powershell). 

## <a name="azure-sql-managed-instance"></a>[Verwaltete Azure SQL-Datenbank-Instanz](#tab/managed-instance)

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für verwaltete Azure SQL-Instanzen.

| |  |
|---|---|
|**Erstellen und Konfigurieren von verwalteten Instanzen**||
| [Erstellen und Verwalten einer verwalteten Instanz](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | Dieses PowerShell-Skript zeigt, wie Sie mithilfe von Azure PowerShell eine verwaltete Instanz erstellen und verwalten. |
| [Verwenden von PowerShell mit einer Azure Resource Manager-Vorlage zum Erstellen einer verwalteten Instanz in Azure SQL-Datenbank](../managed-instance/scripts/create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dieses PowerShell-Skript zeigt, wie Sie mithilfe von Azure PowerShell und einer Azure Resource Manager-Vorlage eine verwaltete Instanz erstellen und verwalten.|
| [Wiederherstellen einer Datenbank in einer verwalteten Instanz in einer anderen geografischen Region](../managed-instance/scripts/restore-geo-backup.md) | Dieses PowerShell-Skript stellt eine Datenbanksicherung in einer anderen Region wieder her. Dies wird als Notfallwiederherstellungsszenario mit Geowiederherstellung bezeichnet. |
| **Konfigurieren von Transparent Data Encryption (TDE)**||
| [Verwalten von Transparent Data Encryption in einer verwalteten Instanz mithilfe eines eigenen Azure Key Vault-Schlüssels](../managed-instance/scripts/transparent-data-encryption-byok-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| In diesem PowerShell-Skript wird Transparent Data Encryption (TDE) in einem Bring Your Own Key-Szenario (BYOK) für eine verwaltete Azure SQL-Datenbank-Instanz unter Verwendung eines Schlüssels aus Azure Key Vault konfiguriert.|
|**Konfigurieren einer Failovergruppe**||
| [Konfigurieren einer Failovergruppe für eine verwaltete Instanz](../managed-instance/scripts/add-to-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Mit diesem PowerShell-Skript werden zwei verwaltete Instanzen erstellt und einer Failovergruppe hinzugefügt. Anschließend wird das Failover von der primären verwalteten Instanz zur sekundären verwalteten Instanz getestet. |
|||

Erfahren Sie mehr über [PowerShell-Cmdlets für verwaltete Azure SQL-Instanzen](../managed-instance/api-references-create-manage-instance.md#powershell-create-and-manage-managed-instances).

---

## <a name="additional-resources"></a>Zusätzliche Ressourcen

In den Beispielen auf dieser Seite werden zum Erstellen und Verwalten von Azure SQL-Ressourcen die [PowerShell-Cmdlets](/powershell/module/az.sql/) verwendet. Weitere Cmdlets zum Ausführen von Abfragen und vielen anderen Datenbankaufgaben befinden sich im Modul [sqlserver](/powershell/module/sqlserver/). Weitere Informationen finden Sie unter [SQL Server-PowerShell](/sql/powershell/sql-server-powershell/).
