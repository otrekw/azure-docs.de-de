---
title: 'Azure Synapse Analytics: Migrationsleitfaden'
description: Befolgen Sie diesen Leitfaden, um Ihre Datenbanken zu dedizierten SQL-Pools in Azure Synapse Analytics zu migrieren.
ms.service: synapse-analytics
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: julieMSFT
ms.author: jrasnick
ms.reviewer: jrasnick
ms.date: 03/10/2021
ms.openlocfilehash: aa32083d48c868435ce77114f3a036b8bec47201
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608562"
---
# <a name="migrating-a-data-warehouse-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Migrieren eines Data Warehouse zu einem dedizierten SQL-Pool in Azure Synapse Analytics 
In den folgenden Abschnitten erhalten Sie einen Überblick über die Migration einer bestehenden Data-Warehouse-Lösung zu einem dedizierten SQL-Pool in Azure Synapse Analytics.

## <a name="overview"></a>Übersicht
Vor der Migration sollten Sie sicherstellen, dass Azure Synapse Analytics die beste Lösung für Ihre Workload ist. Azure Synapse Analytics ist ein verteiltes System, das für die Durchführung von Analysen großer Datenmengen konzipiert wurde. Die Migration zu Azure Synapse Analytics erfordert einige Entwurfsänderungen, die zwar nicht komplex sind, aber etwas Zeit in Anspruch nehmen. Wenn Ihr Unternehmen ein professionelles Data Warehouse benötigt, sind die Vorteile den Aufwand wert. Wenn Sie jedoch nicht die Leistung von Azure Synapse Analytics benötigen, ist es kostengünstiger, [SQL Server](https://docs.microsoft.com/sql/sql-server/) oder [Azure SQL-Datenbank](https://docs.microsoft.com/azure/azure-sql/) zu verwenden.

Ziehen Sie Azure Synapse Analytics in Betracht, wenn Folgendes gegeben ist:
- Sie verfügen über mindestens ein Terabyte an Daten.
- Sie planen Analysen großer Mengen von Daten.
- Sie müssen Compute- und Speicherressourcen skalieren können.
- Sie möchten Kosten sparen, indem Sie Computeressourcen anhalten, wenn Sie sie nicht benötigen.

Ziehen Sie für Betriebsworkloads (OLTP) mit folgenden Eigenschaften andere Optionen als Azure Synapse Analytics in Betracht:
- Sehr häufige Lese- und Schreibvorgänge
- Große Anzahl an Singleton Select-Anweisungen
- Große Mengen einzelner Zeileneinfügungen
- Zeilenweise Verarbeitung ist erforderlich
- Inkompatible Formate (JSON, XML)

## <a name="azure-synapse-pathway"></a>Azure Synapse Pathway
Eine der größten Hürden für Kunden ist die Übersetzung ihrer Datenbankobjekte bei der Migration von einem System zu einem anderen. [Azure Synapse Pathway](https://docs.microsoft.com/sql/tools/synapse-pathway/azure-synapse-pathway-overview) unterstützt Sie beim Upgrade auf eine moderne Data-Warehouse-Plattform durch Automatisierung der Objektübersetzung für Ihr bestehendes Data Warehouse. Es handelt sich dabei um ein kostenloses, intuitives und leicht zu verwendendes Tool, das die Codeübersetzung automatisiert und dadurch eine schnellere Migration zu Azure Synapse Analytics ermöglicht.

## <a name="prerequisites"></a>Voraussetzungen
# <a name="migrate-from-sql-server"></a>[Migrieren von SQL Server](#tab/migratefromSQLServer)
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, um Ihr Data Warehouse in SQL Server zu Azure Synapse Analytics zu migrieren: 

- Sie verfügen über eine Data-Warehouse- oder Analytics-Workload. 
- Laden Sie die neueste Version des Tools [Azure Synapse Pathway](https://www.microsoft.com/en-us/download/details.aspx?id=102787) herunter, um SQL Server-Objekte zu Azure Synapse-Objekten zu migrieren.
- Sie verfügen über einen [dedizierten SQL-Pool](../get-started-create-workspace.md) im Azure Synapse-Arbeitsbereich. 

# <a name="migrate-from-netezza"></a>[Migration von Netezza](#tab/migratefromNetezza)
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, um Ihr Netezza-Data-Warehouse zu Azure Synapse Analytics zu migrieren: 

- Laden Sie die neueste Version des Tools [Azure Synapse Pathway](https://www.microsoft.com/en-us/download/details.aspx?id=102787) herunter, um SQL Server-Objekte zu Azure Synapse-Objekten zu migrieren.
- Sie verfügen über einen [dedizierten SQL-Pool](../get-started-create-workspace.md) im Azure Synapse-Arbeitsbereich.

Weitere Informationen finden Sie unter [Azure Synapse Analytics-Lösungen und Migration von Netezza](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/analytics/analytics-solutions-netezza).

# <a name="migrate-from-snowflake"></a>[Migration von Snowflake](#tab/migratefromSnowflake)
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, um Ihr Snowflake-Data-Warehouse zu Azure Synapse Analytics zu migrieren: 

- Laden Sie die neueste Version des Tools [Azure Synapse Pathway](https://www.microsoft.com/en-us/download/details.aspx?id=102787) herunter, um Snowflake-Objekte zu Azure Synapse-Objekten zu migrieren.
- Sie verfügen über einen [dedizierten SQL-Pool](../get-started-create-workspace.md) im Azure Synapse-Arbeitsbereich. 

# <a name="migrate-from-oracle"></a>[Migration von Oracle](#tab/migratefromOracle)
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, um Ihr Oracle-Data-Warehouse zu Azure Synapse Analytics zu migrieren: 

- Sie verfügen über eine Data-Warehouse- oder Analytics-Workload. 
- Laden Sie SSMA für Oracle herunter, um Oracle- in SQL Server-Objekte zu konvertieren. Weitere Informationen finden Sie unter [Migrieren von Oracle-Datenbanken zu SQL Server (OracleToSQL)](https://docs.microsoft.com/sql/ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql). 
- Laden Sie die neueste Version des Tools [Azure Synapse Pathway](https://www.microsoft.com/download/details.aspx?id=102787), um SQL Server-Objekte zu Azure Synapse-Objekten zu migrieren.
- Sie verfügen über einen [dedizierten SQL-Pool](../get-started-create-workspace.md) im Azure Synapse-Arbeitsbereich. 

Weitere Informationen finden Sie unter [Azure Synapse Analytics-Lösungen und Migration eines Oracle-Data-Warehouse](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/analytics/analytics-solutions-exadata).

---

## <a name="pre-migration"></a>Vor der Migration
Nachdem Sie die Entscheidung getroffen haben, eine vorhandene Lösung zu Azure Synapse Analytics zu migrieren, ist es wichtig, die Migration zu planen, bevor Sie beginnen. Ein Hauptziel der Planung besteht darin, sicherzustellen, dass Ihre Daten, Ihre Tabellenschemas und Ihr Code mit Azure Synapse Analytics kompatibel sind. Zwischen Ihrem aktuellen System und Azure Synapse Analytics bestehen gewisse Kompatibilitätsunterschiede, die Sie umgehen müssen. Darüber hinaus ist die Migration großer Datenmengen zu Azure zeitintensiv. Mithilfe sorgfältiger Planung können Sie die Datenverschiebung zu Azure beschleunigen. Ein weiteres wichtiges Ziel der Planung besteht darin, Ihren Entwurf anzupassen, um sicherzustellen, dass Ihre Lösung die hohe Abfrageleistung von Azure Synapse Analytics optimal nutzt. Das Entwerfen von Data Warehouses für die Skalierung umfasst einzigartige Entwurfsmuster, weshalb herkömmliche Ansätze nicht immer optimal sind. Zwar können einige Anpassungen am Entwurf auch nach der Migration vorgenommen werden, allerdings spart es Ihnen Zeit, wenn Sie Änderungen frühzeitig vornehmen.

## <a name="migrate"></a>Migrieren
Eine erfolgreiche Migration erfordert, dass Sie Ihre Tabellenschemas, Ihren Code und Ihre Daten migrieren. Ausführlichere Anleitungen zu diesen Themen finden Sie in den folgenden Artikeln:
- [Entwerfen von Tabellen mithilfe eines dedizierten SQL-Pools in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-overview).
- [Entwurfsentscheidungen und Programmiertechniken für einen dedizierten SQL-Pool (früher SQL Data Warehouse) in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop#development-recommendations-and-coding-techniques).
- [Migrieren Ihrer Daten](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
- [Worum geht es bei der Workloadverwaltung?](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-workload-management).

## <a name="additional-resources"></a>Zusätzliche Ressourcen 
- Das Customer Advisory Team hat einige hervorragende Anleitungen für Azure Synapse Analytics (zuvor SQL DW) in Form von Blogbeiträgen veröffentlicht. Weitere Informationen zur Migration finden Sie im Artikel [Migrieren von Daten zu Azure SQL Data Warehouse in der Praxis](https://docs.microsoft.com/archive/blogs/sqlcat/migrating-data-to-azure-sql-data-warehouse-in-practice).

## <a name="migration-assets-from-real-world-engagements"></a>Migrationsressourcen aus echten Projekten
Weitere Unterstützung zu diesem Migrationsszenario erhalten Sie in den folgenden Ressourcen, die im Rahmen eines echten Migrationsprojekts entwickelt wurden.

| Titel/Link                              | BESCHREIBUNG                                                                                                                       |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Data Workload Assessment Model and Tool](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Dieses Tool stellt für eine bestimmte Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Seine One-Click-Features für eine einfache Berechnung und Berichterstellung helfen Ihnen, umfangreiche Datenbestände schneller zu bewerten, indem ein automatisierter und einheitlicher Entscheidungsprozess für die Findung einer Zielplattform bereitgestellt wird. |
| [Behandeln von Datencodierungsproblemen beim Laden von Daten in Azure Synapse Analytics](https://azure.microsoft.com/en-us/blog/handling-data-encoding-issues-while-loading-data-to-sql-data-warehouse/) | Dieser Blogbeitrag bietet Einblicke in einige der Datencodierungsprobleme, die möglicherweise auftreten, wenn Sie PolyBase zum Laden von Daten in SQL Data Warehouse verwenden. Außerdem werden in diesem Artikel einige Möglichkeiten genannt, wie Sie solche Probleme lösen und die Daten erfolgreich laden können. |
| [Abrufen von Tabellengrößen in einem dedizierten SQL-Pool in Azure Synapse Analytics](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Getting%20table%20sizes%20in%20SQL%20DW.pdf) | Eine wichtige Aufgabe eines Lösungsarchitekten besteht darin, Metriken über eine neue Umgebung nach der Migration abzurufen. Beispielsweise erfasst dieser die Ladezeiten zwischen der lokalen Umgebung und der Cloud, PolyBase-Ladezeiten und mehr. Eine der wichtigsten dieser Aufgaben besteht darin, die Speichergröße in SQL Data Warehouse im Vergleich zur aktuellen Plattform des Kunden zu ermitteln. |
| [Hilfsprogramm zum Verschieben lokaler SQL Server-Anmeldungen zu Azure Synapse Analytics](https://github.com/Microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) | Ein PowerShell-Skript, das ein T-SQL-Befehlsskript erstellt, um Anmeldungen und ausgewählte Datenbankbenutzer aus einer lokalen SQL Server-Instanz in einem Azure SQL-PaaS-Dienst neu zu erstellen. Das Tool ermöglicht die automatische Zuordnung von Windows AD-Konten zu Azure AD-Konten. Alternativ können UPN-Lookups für jede Anmeldung in der lokalen Microsoft Active Directory-Instanz durchgeführt werden. Das Tool kann optional auch native SQL Server-Anmeldeinformationen verschieben. Benutzerdefinierte Server- und Datenbankrollen sowie Rollenmitgliedschaften, Datenbankrollen und Benutzerberechtigungen werden erstellt. Eigenständige Datenbanken werden noch nicht unterstützt, und nur für einen Teil der möglichen SQL Server-Berechtigungen wurden Skripts erstellt. Das bedeutet, dass die Erteilung von Berechtigungen mit WITH GRANT-Anweisungen nicht unterstützt wird (komplexe Berechtigungsstrukturen). Weitere Details finden Sie im Hilfsdokument. Das Skript enthält außerdem Kommentare mit Erläuterungen. |

> [!NOTE]
> Die oben aufgeführten Ressourcen wurden im Rahmen des Data Migration Jumpstart Program (DM Jumpstart) entworfen, das vom Azure Data Group-Engineeringteam gesponsert wird. Der Hauptzweck von DM Jumpstart besteht darin, Hindernisse für komplexe Modernisierungen zu beseitigen, diese zu beschleunigen sowie die Möglichkeiten zur Migration von Datenplattformen zu Microsoft Azure zu vervollständigen. Wenn Ihre Organisation an einer Teilnahme am DM Jumpstart-Programm interessiert sein könnte, wenden Sie sich an Ihr Kundenteam, und bitten Sie es, eine Nominierung einzureichen.

## <a name="videos"></a>Videos
- Hier erfahren Sie, wie [Walgreens sein Einzelhandelsinventursystem](https://www.youtube.com/watch?v=86dhd8N1lH4), das Daten im Umfang von ungefähr 100 TB umfasst, in Rekordzeit von Netezza zu Azure Synapse Analytics (früher SQL DW) migriert hat. 