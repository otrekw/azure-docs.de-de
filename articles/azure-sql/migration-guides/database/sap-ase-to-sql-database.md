---
title: 'Migrationsleitfaden: SAP ASE zu Azure SQL-Datenbank'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre SAP ASE-Datenbanken mit SQL Server Migration Assistant für SAP Adapter Server Enterprise zu Azure SQL-Datenbank migrieren.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 81956a16142f314f54afd9d5a1b9055a559e906c
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103564648"
---
# <a name="migration-guide-sap-ase-to-azure-sql-database"></a>Migrationsleitfaden: SAP ASE zu Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

In diesem Leitfaden erfahren Sie, wie Sie Ihre SAP ASE-Datenbanken mit SQL Server Migration Assistant für SAP Adapter Server Enterprise zu Azure SQL-Datenbank migrieren.

Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Voraussetzungen 

Für die Migration Ihrer SAP ASE-Datenbanken zu Azure SQL-Datenbank müssen Sie folgende Schritte ausführen:

- Überprüfen Sie, ob Ihre Quellumgebung unterstützt wird. 
- Installieren Sie [SQL Server Migration Assistant für SAP Adaptive Server Enterprise (früher SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256). 

## <a name="pre-migration"></a>Vor der Migration

Wenn diese Voraussetzungen erfüllt sind, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit der Migration bewerten.

### <a name="assess"></a>Bewerten

Verwenden Sie [SQL Server Migration Assistant (SSMA) für SAP Adaptive Server Enterprise (früher SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256), um Datenbankobjekte und Daten zu überprüfen, die Datenbanken für die Migration zu bewerten und um Sybase-Datenbankobjekte und anschließend Daten zu Azure SQL-Datenbank zu migrieren. Weitere Informationen finden Sie unter [SQL Server Migration Assistant für Sybase (SybaseToSQL)](/sql/ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql).

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen: 

1. Öffnen Sie **SSMA für Sybase**. 
1. Klicken Sie auf **File** (Datei) und dann auf **New Project** (Neues Projekt). 
1. Geben Sie einen Projektnamen und einen Speicherort für das Projekt an, und wählen Sie dann Azure SQL-Datenbank in der Dropdownliste als Migrationsziel aus. Klicken Sie auf **OK**.
1. Geben Sie in das Dialogfeld **Connect to Sybase** (Verbindung mit Sybase herstellen) die Werte der SAP-Verbindungsdetails ein. 
1. Klicken Sie mit der rechten Maustaste auf die SAP-Datenbank, die Sie migrieren möchten, und wählen Sie **Bericht erstellen** aus. Dadurch wird ein HTML-Bericht generiert.
1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um ein Inventar der DB2-Objekte sowie Informationen zum für die Durchführung von Schemakonvertierungen erforderlichen Aufwand zu erhalten. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects.

   Beispiel: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 


### <a name="validate-type-mappings"></a>Überprüfen der Typzuordnungen

Überprüfen Sie vor der Schemakonvertierung die Standardzuordnungen der Datentypen, oder ändern Sie sie nach Bedarf. Hierzu können Sie zum Menü **Extras** wechseln und **Project Settings** (Projekteinstellungen) auswählen oder die Typzuordnung für die einzelnen Tabellen ändern, indem Sie die Tabelle im **Metadaten-Explorer von SAP ASE** auswählen.


### <a name="convert-schema"></a>Schema konvertieren

Führen Sie die folgenden Schritte aus, um das Schema zu konvertieren:

1. Optional: Zum Konvertieren von dynamischen Abfragen oder Ad-hoc-Abfragen klicken Sie mit der rechten Maustaste auf den Knoten, und wählen Sie **Anweisung hinzufügen** aus. 
1. Wählen Sie in der oberen Navigationsleiste **Connect to Azure SQL Database** (Verbindung mit Azure SQL-Datenbank herstellen) aus, und geben Sie Details zu Azure SQL-Datenbank an. Sie können eine Verbindung mit einer vorhandenen Datenbank herstellen oder einen neuen Namen angeben. In diesem Fall wird auf dem Zielserver eine Datenbank erstellt.
1. Klicken Sie im **Metadaten-Explorer von Sybase** mit der rechten Maustaste auf das SAP ASE-Schema, und wählen Sie **Convert schema** (Schema konvertieren) aus. Sie können stattdessen auch auf der obersten Navigationsleiste die Option **Schema konvertieren** auswählen. 
1. Vergleichen und überprüfen Sie die Struktur des Schemas, um potenzielle Probleme zu ermitteln. 

   Nach der Schemakonvertierung können Sie dieses Projekt lokal speichern, um das Schema offline zu warten. Klicken Sie im Menü **File** (Datei) auf **Save Project** (Projekt speichern). So können Sie das Quell- und Zielschema offline auswerten und warten, bevor Sie das Schema auf Azure SQL-Datenbank veröffentlichen.

Weitere Informationen finden Sie unter [Konvertieren von Schemas](/sql/ssma/sybase/converting-sybase-ase-database-objects-sybasetosql).


## <a name="migrate"></a>Migrate 

Wenn die Voraussetzungen erfüllt sind und Sie die Aufgaben **vor der Migration** ausgeführt haben, können Sie die Schema- und Datenmigration durchführen.

Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und die Daten zu migrieren: 

1. Klicken Sie im **Metadaten-Explorer von Azure SQL-Datenbank** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Synchronize with Database** (Mit Datenbank synchronisieren) aus.  Mit dieser Aktion wird das SAP ASE-Schema in der Instanz von Azure SQL-Datenbank veröffentlicht.
1. Klicken Sie im **Metadaten-Explorer von SAP ASE** mit der rechten Maustaste auf das SAP ASE-Schema, und wählen Sie **Daten migrieren** aus.  Sie können stattdessen auch in der oberen Navigationsleiste auf **Daten migrieren** klicken.  
1. Zeigen Sie nach Abschluss der Migration den **Bericht zur Datenmigration** an: 
1. Überprüfen Sie die Migration, indem Sie die Daten und das Schema in der Azure SQL-Datenbank-Instanz mit Azure SQL-Datenbank Management Studio (SSMS) überprüfen.


## <a name="post-migration"></a>Nach der Migration 

Nach erfolgreichem Abschluss der **Migrationsphase** müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln der Validierungstests:** Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.

2. **Einrichten der Testumgebung:** Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.

3. **Ausführen der Validierungstests:** Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.

4. **Ausführen der Leistungstests:** Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

### <a name="optimize"></a>Optimieren

Die Postmigrationsphase ist unerlässlich, um Datengenauigkeitsprobleme abzustimmen, die Vollständigkeit zu bestätigen und Leistungsprobleme der Workload zu beheben.

> [!NOTE]
> Weitere Einzelheiten zu diesen Problemen sowie die erforderlichen Schritte zur Problembehandlung finden Sie im [Leitfaden zur Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios und Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Weitere Informationen zu Azure SQL-Datenbank:
   - [Übersicht über SQL-Datenbank](../../database/sql-database-paas-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/) 


- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit (Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Ausführliche Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Assistent für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).
