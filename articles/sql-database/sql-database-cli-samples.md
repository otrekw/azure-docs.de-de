---
title: Azure CLI-Skriptbeispiele
description: Azure CLI-Skriptbeispiele zum Erstellen und Verwalten von Azure SQL-Datenbank-Servern, Pools für elastische Datenbanken, Datenbanken und Firewalls.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/03/2019
ms.openlocfilehash: 459a5ea69e11a8614c572f68fce039b6cabbb1ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061715"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure CLI-Beispiele für Azure SQL-Datenbank

Azure SQL-Datenbank kann mithilfe der <a href="/cli/azure">Azure CLI</a> konfiguriert werden.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

# <a name="single-database--elastic-pools"></a>[Einzeldatenbank und Pools für elastische Datenbanken](#tab/single-database)

| | |
|---|---|
|**Erstellen einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Create a single SQL database and configure a firewall rule using PowerShell](scripts/sql-database-create-and-configure-database-cli.md) (Erstellen einer einzelnen SQL-Datenbank und Konfigurieren einer Firewallregel mit PowerShell) | Erstellt eine Azure SQL-Datenbank und konfiguriert eine Firewallregel auf Serverebene. |
| [Create elastic pools and move databases between pools and out of a pool using PowerShell](scripts/sql-database-move-database-between-pools-cli.md) (Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool mit PowerShell) | Erstellt Pools für elastische SQL-Datenbanken, verschiebt Azure SQL-Pooldatenbanken und ändert Computegrößen. |
|**Skalieren einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Monitor and scale a single SQL database using the Azure CLI](scripts/sql-database-monitor-and-scale-database-cli.md) (Überwachen und Skalieren einer einzelnen SQL-Datenbank mit Azure CLI) | Skaliert eine Azure SQL-Datenbank nach Abfrage der Größeninformationen für die Datenbank auf eine andere Computegröße. |
| [Scale an elastic pool in Azure SQL Database using the Azure CLI](scripts/sql-database-scale-pool-cli.md) (Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank mit Azure CLI) | Skaliert einen Pool für elastische SQL-Datenbanken auf eine andere Computegröße. |
|**Konfigurieren von Georeplikation und Failover**||
| [Hinzufügen einzelner Datenbanken zur Failovergruppe](scripts/sql-database-add-single-db-to-failover-group-cli.md)| Erstellt eine Datenbank und eine Failovergruppe, fügt die Datenbank zur Failovergruppe hinzu und testet das Failover auf den sekundären Server. |
| [Konfigurieren einer Failovergruppe für einen Pool für elastische Datenbanken](scripts/sql-database-add-elastic-pool-to-failover-group-cli.md) | Erstellt eine Datenbank, fügt die Datenbank zu einem Pool für elastische Datenbanken hinzu, fügt den Pool für elastische Datenbanken zur Failovergruppe hinzu und testet das Failover auf den sekundären Server. |
| [Configure and failover a single database using active geo-replication](scripts/sql-database-setup-geodr-and-failover-database-cli.md) (Konfiguration und Failover einer einzelnen Datenbank mithilfe von aktiver Georeplikation)| Konfiguriert die aktive Georeplikation für eine einzelne Azure SQL-Datenbank und führt ein Failover auf das sekundäre Replikat aus. |
| [Configure and failover a pooled database using active geo-replication](scripts/sql-database-setup-geodr-and-failover-pool-cli.md) (Konfiguration und Failover einer Pooldatenbank mithilfe von aktiver Georeplikation)| Konfiguriert die aktive Georeplikation für eine Azure SQL-Datenbank in einem Pool für elastische SQL-Datenbanken und führt ein Failover auf das sekundäre Replikat aus. |
| **Überwachung und Bedrohungserkennung** |
| [Configure SQL Database auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-cli.md) (Konfigurieren der Überwachung und Bedrohungserkennung für SQL-Datenbank mit PowerShell)| Konfiguriert Richtlinien zur Überwachung und Bedrohungserkennung für eine Azure SQL-Datenbank. |
| **Sichern, Wiederherstellen, Kopieren und Importieren einer Datenbank**||
| [Sichern einer Datenbank](scripts/sql-database-backup-database-cli.md)| Sichert eine Azure SQL-Datenbank in einer Azure-Speichersicherung. |
| [Restore a SQL database using PowerShell (Wiederherstellen einer SQL-Datenbank mit PowerShell)](scripts/sql-database-restore-database-cli.md)| Stellt eine Azure SQL-Datenbank auf Basis einer georedundanten Sicherung und eine gelöschte Azure SQL-Datenbank gemäß der letzten Sicherung wieder her. |
| [Copy a SQL database to a new server using PowerShell](scripts/sql-database-copy-database-to-new-server-cli.md) (Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell) | Erstellt eine Kopie einer vorhandenen Azure SQL-Datenbank auf einem neuen Azure SQL-Server. |
| [Import from a bacpac into a SQL database using PowerShell](scripts/sql-database-import-from-bacpac-cli.md) (Importieren aus einer BACPAC-Datei in eine SQL-Datenbank mit PowerShell)| Importiert eine Datenbank aus einer *BACPAC*-Datei auf einen Azure SQL-Server. |
|||

Erfahren Sie mehr über die [Azure CLI-API für Einzeldatenbanken](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases).

# <a name="managed-instance"></a>[Verwaltete Instanz](#tab/managed-instance)

Die folgende Tabelle enthält Links zu Azure CLI-Skriptbeispielen für verwaltete Azure SQL-Datenbank-Instanzen.

| | |
|---|---|
| **Erstellen einer verwalteten Instanz**||
| [Erstellen einer verwalteten Instanz](scripts/sql-database-create-configure-managed-instance-cli.md)| Erstellt eine verwaltete Instanz. |
| **Konfigurieren von Transparent Data Encryption (TDE)**||
| [Verwalten von Transparent Data Encryption in einer verwalteten Instanz mithilfe einer Azure Key Vault-Instanz](scripts/transparent-data-encryption-byok-sql-managed-instance-cli.md)| Konfiguriert mithilfe von Azure Key Vault und verschiedenen Schlüsselszenarien Transparent Data Encryption (TDE) in einer verwalteten Azure SQL-Datenbank-Instanz. |
|**Konfigurieren einer Failovergruppe**||
| [Konfigurieren einer Failovergruppe für eine verwaltete Instanz](scripts/sql-database-add-managed-instance-to-failover-group-cli.md) | Erstellt zwei verwaltete Instanzen und fügt sie einer Failovergruppe hinzu. Anschließend wird das Failover von der primären verwalteten Instanz auf die sekundäre verwaltete Instanz getestet. |
|||

Weitere Beispiele zu verwalteten Instanzen finden Sie in den Artikeln zum [Erstellen](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/), [Aktualisieren](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/), [Verschieben einer Datenbank](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) und [Verwenden von Skripts](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

Erfahren Sie mehr über die [Azure CLI-API für verwaltete Instanzen](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances).

---
