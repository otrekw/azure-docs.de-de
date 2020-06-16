---
title: SSIS-Migration mit Azure SQL Managed Instance als Datenbankworkloadziel
description: SSIS-Migration mit Azure SQL Managed Instance als Datenbankworkloadziel
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: c91731d70d5db59ecf5daa726fd42ee42c58f51e
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84116309"
---
# <a name="ssis-migration-with-azure-sql-managed-instance-as-the-database-workload-destination"></a>SSIS-Migration mit Azure SQL Managed Instance als Datenbankworkloadziel

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Wenn Sie Datenbankworkloads von einer SQL Server-Instanz zu Azure SQL Managed Instance migrieren, sollten Sie mit [Azure Data Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) (DMS) und den [Netzwerktopologien für SQL Managed Instance-Migrationsvorgänge mithilfe von DMS](https://docs.microsoft.com/azure/dms/resource-network-topologies) vertraut sein.

Dieser Artikel konzentriert sich auf die Migration von SSIS-Paketen (SQL Server Integration Service), die im SSIS-Katalog (SSISDB) gespeichert sind, und auf SQL Server-Agentaufträge, die die Ausführung von SSIS-Paketen planen.

## <a name="migrate-ssis-catalog-ssisdb"></a>Migrieren des SSIS-Katalogs (SSISDB)

Die SSISDB-Migration kann mithilfe von DMS erfolgen, wie im folgenden Artikel beschrieben: [Migrieren von SSIS-Paketen zu SQL Managed Instance](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="ssis-jobs-to-sql-managed-instance-agent"></a>SSIS-Aufträge für den SQL Managed Instance-Agent

SQL Managed Instance verfügt wie der lokale SQL Server-Agent über einen nativen, erstklassigen Planer.  Da noch kein Migrationstool für SSIS-Aufträge verfügbar ist, müssen sie vom lokalen SQL Server-Agent mithilfe von Skripts bzw. durch manuelles Kopieren zum SQL Managed Instance-Agent migriert werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
- [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview)
- [Netzwerktopologien für SQL Managed Instance-Migrationsvorgänge](https://docs.microsoft.com/azure/dms/resource-network-topologies)
- [Migrieren von SSIS-Paketen zu SQL Managed Instance](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>Nächste Schritte

- [Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Ausführen von in Azure bereitgestellten SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
