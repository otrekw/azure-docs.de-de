---
title: Status des Datenbankmigrationsszenarios
titleSuffix: Azure Database Migration Service
description: Informationen zum Status von Migrationsszenarien, die in Azure Database Migration Service unterstützt werden.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: troubleshooting
ms.date: 07/08/2020
ms.openlocfilehash: 6c1a0853dc59b2e2ceabfd47d81aac364a2b5716
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107589430"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Status von Migrationsszenarien, die in Azure Database Migration Service unterstützt werden

Azure Database Migration Service wurde zur Unterstützung verschiedener Migrationsszenarien (Quelle-Ziel-Paare) und sowohl für die Offline- (einmalig) als auch die Onlinemigration (fortlaufende Synchronisierung) konzipiert. Der in Azure Database Migration Service bereitgestellte Szenarioumfang wird im Lauf der Zeit erweitert. In regelmäßigen Abständen werden neue Szenarios hinzugefügt. In diesem Artikel werden die Migrationsszenarien, die derzeit in Azure Database Migration Service unterstützt werden, und der Status der einzelnen Szenarien (private Vorschau, öffentliche Vorschau oder allgemein verfügbar) definiert.

## <a name="offline-versus-online-migrations"></a>Offline- und Onlinemigrationen

Mit Azure Database Migration Service können Sie eine Offline- oder eine Onlinemigration durchführen. Bei Migrationen des Typs *Offline* beginnt die Ausfallzeit der Anwendung mit dem Start der Migration. Führen Sie eine Migration des Typs *Online* durch, um die Ausfallzeit auf die Zeit zu begrenzen, die bei Abschluss der Migration für die Umstellung auf die neue Umgebung erforderlich ist. Es empfiehlt sich, eine Offlinemigration zu testen, um zu ermitteln, ob die Ausfallzeit akzeptabel ist. Wenn dies nicht der Fall ist, sollten Sie eine Onlinemigration durchführen.

## <a name="migration-scenario-support"></a>Unterstützung von Migrationsszenarios

In den folgenden Tabellen sind die Migrationsszenarien aufgeführt, die bei Verwendung von Azure Database Migration Service unterstützt werden.

> [!NOTE]
> Wenn ein Szenario, das im Folgenden als unterstützt aufgeführt wird, auf der Benutzeroberfläche aber nicht angezeigt wird, wenden Sie sich an den Alias für [Azure-Datenbankmigrationen](mailto:AskAzureDatabaseMigrations@service.microsoft.com), um weitere Informationen zu erhalten.

> [!IMPORTANT]
> Alle derzeit in Azure Database Migration Service in der privaten Vorschau unterstützten Szenarien finden Sie auf der [DMS Preview-Website](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Unterstützung der Offlinemigration (einmalig)

Die folgende Tabelle enthält die Azure Database Migration Service-Unterstützung für Offlinemigrationen.

| Ziel  | `Source` | Support | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL-Datenbank** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Azure SQL-Datenbank MI** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL | X |  |
|   | Oracle | X |   |
| **Virtueller Azure SQL-Computer** | SQL Server | ✔ | Allgemein verfügbar |
|   | Oracle | X |   |
| **Azure Cosmos DB** | MongoDB | ✔ | Allgemein verfügbar |
| **Azure-Datenbank für MySQL** | MySQL | X |   |
|   | RDS MySQL | X |   |
| **Azure DB for PostgreSQL – Einzelserver** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |
| **Azure DB for PostgreSQL – Hyperscale (Citus)** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |

### <a name="online-continuous-sync-migration-support"></a>Unterstützung der Onlinemigration (fortlaufende Synchronisierung)

Die folgende Tabelle enthält die Azure Database Migration Service-Unterstützung für Onlinemigrationen.

| Ziel  | `Source` | Support | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL-Datenbank** | SQL Server | X |  |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Azure SQL-Datenbank MI** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Virtueller Azure SQL-Computer** | SQL Server | X |   |
|   | Oracle  | X |  |
| **Azure Cosmos DB** | MongoDB | ✔ | Allgemein verfügbar |
| **Azure-Datenbank für MySQL** | MySQL | ✔ | Allgemein verfügbar |
|   | RDS MySQL | ✔ | Allgemein verfügbar |
| **Azure DB for PostgreSQL – Einzelserver** | PostgreSQL | ✔ | Allgemein verfügbar |
|   | Azure DB for PostgreSQL – Einzelserver | ✔ | Allgemein verfügbar |
|   | RDS PostgreSQL | ✔ | Allgemein verfügbar |
|   | Oracle | ✔ | Öffentliche Vorschau (wird nach dem 1. Mai 2021 als veraltet markiert) |
| **Azure DB for PostgreSQL – Hyperscale (Citus)** | PostgreSQL | ✔ | Allgemein verfügbar |
|   | RDS PostgreSQL | ✔ | Allgemein verfügbar |

> [!IMPORTANT]
> Das Migrationsszenario „Oracle zu Azure Database for PostgreSQL" (derzeit in der Vorschauphase) steht nach dem 1. Mai 2021 nicht mehr zur Verfügung. Wir bieten weiterhin Unterstützung über alternative Tools (wie Ora2pg) und bieten die beste Migrationserfahrung für Migrationen von Oracle zu PostgreSQL. Bewährte Methoden für die Migration finden Sie unter [Handbuch für die Migration von Oracle zu Azure Database for PostgreSQL](https://aka.ms/OracletoPGguide).


## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure Database Migration Service und Informationen zur regionalen Verfügbarkeit finden Sie im Artikel [Was ist Azure Database Migration Service?](dms-overview.md).
