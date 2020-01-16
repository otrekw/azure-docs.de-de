---
title: Bekannte Probleme und Einschränkungen bei Onlinemigrationsvorgängen zu verwalteten Azure SQL-Datenbank-Instanzen
description: Erfahren Sie mehr über bekannte Probleme/Migrationseinschränkungen bei Onlinemigrationsvorgängen zu verwalteten Azure SQL-Datenbank-Instanzen.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/18/2019
ms.openlocfilehash: bc1cbfc1e86db758a4f4d0240f81f5363f817312
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475956"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database-managed-instance"></a>Bekannte Probleme/Migrationseinschränkungen bei Onlinemigrationsvorgängen zu verwalteten Azure SQL-Datenbank-Instanzen

Im Folgenden werden bekannte Probleme und Einschränkungen in Bezug auf Onlinemigrationsvorgängen von SQL Server zu verwalteten Azure SQL-Datenbank-Instanzen beschrieben.

> [!IMPORTANT]
> Bei Onlinemigrationen von SQL Server zu Azure SQL-Datenbank wird die Migration von SQL_variant-Datentypen nicht unterstützt.

## <a name="backup-requirements"></a>Sicherungsanforderungen

- **Sicherungen mit Prüfsumme**

    Azure Database Migration Service verwendet die Sicherungs- und Wiederherstellungsmethode, um Ihre lokalen Datenbanken zur verwalteten SQL-Datenbank-Instanz zu migrieren. Azure Database Migration Service unterstützt nur Sicherungen, die mit einer Prüfsumme erstellt wurden.

    [Aktivieren oder Deaktivieren von Sicherungsprüfsummen während der Sicherung oder Wiederherstellung (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017)

    > [!NOTE]
    > Wenn Sie die Datenbanksicherungen mit Komprimierung durchführen, wird die Prüfsumme standardmäßig ausgeführt, sofern Sie sie nicht explizit deaktiviert haben.

    Wenn Sie bei Offlinemigrationsvorgängen **Azure Database Migration Service soll …** auswählen, verwendet Azure Database Migration Service die Datenbanksicherung mit aktivierter Prüfsumme.

- **Sicherungsmedien**

    Stellen Sie sicher, dass sich jede Sicherung auf einem separaten Sicherungsmedium (Sicherungsdateien) befindet. Azure Database Migration Service unterstützt keine Sicherungen, die an eine einzelne Sicherungsdatei angehängt werden. Erstellen Sie vollständige Sicherungen und Protokollsicherungen in getrennten Sicherungsdateien.

## <a name="data-and-log-file-layout"></a>Daten- und Protokolldateilayout

- **Anzahl der Protokolldateien**

    Azure Database Migration Service unterstützt keine Datenbanken mit mehreren Protokolldateien. Wenn Sie über mehrere Protokolldateien verfügen, müssen Sie diese verkleinern und in einer einzelnen Transaktionsprotokolldatei neu organisieren. Da kein Remotezugriff auf Protokolldateien möglich ist, die nicht leer sind, müssen Sie zunächst die Protokolldatei sichern.

## <a name="sql-server-features"></a>SQL Server-Funktionen

- **Filestream/Dateitabellen**

    Verwaltete SQL-Datenbank-Instanzen unterstützen derzeit weder Filestream noch Dateitabellen. Bei Workloads, für die diese Features erforderlich sind, empfiehlt es sich, SQL-Server, die auf Azure-VMs ausgeführt werden, als Azure-Ziel auszuwählen.

- **In-Memory-Tabellen**

    In-Memory-OLTP ist in den Tarifen „Premium“ und „Unternehmenskritisch“ für verwaltete SQL-Datenbank-Instanzen verfügbar. Beim Tarif „Universell“ wird In-Memory OLTP nicht unterstützt.

## <a name="migration-resets"></a>Migrationszurücksetzungen

- **Bereitstellungen**

    Verwaltete SQL-Datenbank-Instanzen sind ein PaaS-Dienst mit automatischen Patches und Versionsupdates. Während der Migration Ihrer verwalteten SQL-Datenbank-Instanz werden nicht kritische Updates bis zu 36 Stunden zurückgehalten. Danach (und bei kritischen Updates) wird der Prozess bei einer Unterbrechung der Migration auf den vollständigen Wiederherstellungszustand zurückgesetzt.

    Die Migrationsübernahme kann erst nach der Wiederherstellung der vollständigen Sicherung aufgerufen werden, wobei dann auch alle Protokollsicherungen erfasst werden. Wenden Sie sich an den [Azure DMS-Feedbackalias](mailto:dmsfeedback@microsoft.com), wenn Migrationsübernahmen aus der Produktion betroffen sind.
