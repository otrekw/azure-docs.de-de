---
title: 'Migrationsleitfaden: Db2 zu Azure SQL-Datenbank'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre IBM Db2-Datenbanken mithilfe von SQL Server Migration Assistant für Db2 (SSMA für Db2) zu Azure SQL-Datenbank migrieren.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: 429a0d19a0d82ee53c228ad5bfac5ac1d5101fa2
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108136603"
---
# <a name="migration-guide-ibm-db2-to-azure-sql-database"></a>Migrationsleitfaden: IBM Db2 zu Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

In diesem Leitfaden erfahren Sie, wie Sie Ihre IBM Db2-Datenbanken mithilfe von [SQL Server Migration Assistant](https://azure.microsoft.com/en-us/migration/sql-server/) für Db2 [zu Azure SQL-Datenbank migrieren](https://azure.microsoft.com/migration/migration-journey). 

Weitere Migrationsleitfäden finden Sie in den [Leitfäden zur Azure-Datenbankmigration](/data-migration). 

## <a name="prerequisites"></a>Voraussetzungen 

Für die Migration Ihrer Db2-Datenbank zu SQL-Datenbank gelten folgende Voraussetzungen:

- Überprüfen Sie, ob Ihre [Quellumgebung unterstützt wird](/sql/ssma/db2/installing-ssma-for-db2-client-db2tosql#prerequisites).
- Laden Sie [SQL Server Migration Assistant (SSMA) for Db2](https://www.microsoft.com/download/details.aspx?id=54254) herunter.
- Eine Datenbank in [Azure SQL-Datenbank](../../database/single-database-create-quickstart.md).
- Konnektivität und ausreichende Berechtigungen für den Zugriff auf die Quelle und das Ziel. 

## <a name="pre-migration"></a>Vor der Migration

Nachdem Sie diese Voraussetzungen erfüllt haben, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit Ihrer [Azure-Cloudmigration](https://azure.microsoft.com/migration) bewerten.

### <a name="assess-and-convert"></a>Bewerten und Konvertieren

Verwenden Sie SSMA für Db2, um Datenbankobjekte und -daten zu überprüfen und die Datenbanken für die Migration zu bewerten. 

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen:

1. Öffnen Sie [SSMA für Db2](https://www.microsoft.com/download/details.aspx?id=54254). 
1. Wählen Sie **Datei** > **Neues Projekt**. 
1. Geben Sie den Projektnamen und einen Speicherort für Ihr Projekt ein. Wählen Sie dann in der Dropdown Liste die Option Azure SQL-Datenbank als Migrationsziel aus, und klicken Sie auf **OK**.

   :::image type="content" source="media/db2-to-sql-database-guide/new-project.png" alt-text="Screenshot, der die zu spezifizierenden Projektdetails anzeigt.":::


1. Geben Sie unter **Verbinden mit Db2**, Werte für die Db2-Verbindungsdetails ein. 

   :::image type="content" source="media/db2-to-sql-database-guide/connect-to-db2.png" alt-text="Screenshot, der die Optionen zur Verbindung mit Ihrer Db2-Instanz anzeigt.":::


1. Klicken Sie mit der rechten Maustaste auf das Db2-Schema, das Sie migrieren möchten, und wählen Sie dann **Bericht erstellent**. Dadurch wird ein HTML-Bericht generiert. Alternativ können Sie nach dem Auswählen des Schemas in der Navigationsleiste auf **Create report** (Bericht erstellen) klicken.

   :::image type="content" source="media/db2-to-sql-database-guide/create-report.png" alt-text="Screenshot: Erstellung eines Berichts":::

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um den Bestand an Db2-Objekten sowie Informationen zum Aufwand für Schemakonvertierungen zu erhalten. Der Standardstandort für den Bericht ist im Berichtsordner innerhalb von *SSMAProjects*.

   Beispiel: `drive:\<username>\Documents\SSMAProjects\MyDb2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-database-guide/report.png" alt-text="Screenshot des Berichts, den Sie überprüfen, um eventuelle Fehler oder Warnungen zu erkennen.":::


### <a name="validate-data-types"></a>Überprüfen von Datentypen

Validieren Sie die Standard-Datentyp-Zuordnungen und ändern Sie bei Bedarf entsprechend den Anforderungen. Gehen Sie dazu folgendermaßen vor: 

1. Klicken Sie im Menü auf **Tools**. 
1. Klicken auf **Project Settings** (Projekteinstellungen). 
1. Klicken Sie auf die Registerkarte **Type Mappings** (Typzuordnungen).

   :::image type="content" source="media/db2-to-sql-database-guide/type-mapping.png" alt-text="Screenshot, der die Auswahl des Schemas und der Typ-Mapping anzeigt.":::

1. Sie können die Typzuordnung für jede Tabelle ändern, indem Sie die Tabelle im **Db2 Metadata Explorer** auswählen. 

### <a name="convert-schema"></a>Schema konvertieren

Führen Sie die folgenden Schritte aus, um das Schema zu konvertieren:

1. (Optional:) Fügen Sie Anweisungen dynamische Abfragen oder Ad-hoc-Abfragen hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten, und klicken Sie dann auf **Add statements** (Anweisungen hinzufügen). 
1. Klicken Sie auf **Connect to Azure SQL Database** (Verbindung mit Azure SQL-Datenbank herstellen). 
    1. Geben Sie Verbindungsdetails ein, um Ihre Datenbank in Azure SQL-Datenbank zu verbinden.
    1. Wählen Sie die SQL-Zieldatenbank in der Dropdownliste aus, oder geben Sie einen neuen Namen an. In diesem Fall wird eine Datenbank auf dem Zielserver erstellt. 
    1. Geben Sie Details zur Authentifizierung an. 
    1. Wählen Sie **Verbinden** aus.

   :::image type="content" source="media/db2-to-sql-database-guide/connect-to-sql-database.png" alt-text="Screenshot mit den Details, die zum Herstellen einer Verbindung mit dem logischen Server in Azure erforderlich sind.":::


1. Klicken Sie mit der rechten Maustaste auf das Schema, und klicken Sie dann auf **Convert Schema** (Schema konvertieren). Alternativ können Sie nach dem Auswählen des Schemas in der oberen Navigationsleiste auf **Convert Schema** (Schema konvertieren) klicken.

   :::image type="content" source="media/db2-to-sql-database-guide/convert-schema.png" alt-text="Screenshot, der das Auswählen des Schemas und der Konvertierung anzeigt.":::

1. Nachdem die Konvertierung fertiggestellt wurde, vergleichen und überprüfen Sie die Struktur des Schemas, um potenzielle Probleme zu ermitteln. Beheben Sie die Probleme auf Grundlage der Empfehlungen.

   :::image type="content" source="media/db2-to-sql-database-guide/compare-review-schema-structure.png" alt-text="Screenshot, der den Vergleich und die Überprüfung der Struktur des Schemas zeigt, um mögliche Probleme zu identifizieren.":::

1. Wählen Sie im **Fensterbereich** Ausgabe die Option **Ergebnisse überprüfen**. Überprüfen Sie die Fehler im Fenster **Fehlerliste**. 
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie im Menü **Datei** die Option **Projekt speichern** aus. So können Sie das Quell- und Zielschema offline auswerten und Probleme behandeln, bevor Sie das Schema auf SQL-Datenbank veröffentlichen.

## <a name="migrate"></a>Migrate

Wenn Sie die Bewertung Ihrer Datenbanken und die Behandlung etwaiger Abweichungen abgeschlossen haben, besteht der nächste Schritt in der Durchführung des Migrationsprozesses.

Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und Ihre Daten zu migrieren:

1. Veröffentlichen des Schemas. Klicken Sie im **Azure SQL Database Instance Metadata Explorer** im **Datenbank**-Knoten mit der rechten Maustaste auf die Datenbank. Wählen Sie dann **mit Datenbank synchronisieren** aus.

   :::image type="content" source="media/db2-to-sql-database-guide/synchronize-with-database.png" alt-text="Screenshot, der die Option zur Synchronisierung mit der Datenbank zeigt.":::

1. Migrieren der Daten. Klicken Sie im **Db2 Metadata Explorer** mit der rechten Maustaste auf die Datenbank oder das Objekt, das Sie migrieren möchten, und wählen Sie **Daten migrieren**. Alternativ können Sie in der Navigationsleiste **Daten migrieren** auswählen. Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um die Daten einer gesamten Datenbank zu migrieren. Wenn Sie Daten aus einzelnen Tabellen migrieren möchten, erweitern Sie die Datenbank, erweitern Sie die **Tabellen**, und aktivieren Sie dann das Kontrollkästchen neben der Tabelle. Deaktivieren Sie das Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen.

   :::image type="content" source="media/db2-to-sql-database-guide/migrate-data.png" alt-text="Screenshot, der das Auswählen des Schemas und das Auswählen der Datenmigration anzeigt.":::

1. Geben Sie die Verbindungsdetails für die Db2- und die Azure SQL-Datenbank an. 
1. Zeigen Sie nach Abschluss der Migration den **Bericht zur Datenmigration** an.  

   :::image type="content" source="media/db2-to-sql-database-guide/data-migration-report.png" alt-text="Screenshot, der zeigt, wo der Datenmigrationsbericht überprüft werden soll.":::

1. Stellen Sie eine Verbindung mit Ihrer Datenbank in Azure SQL-Datenbank her, indem Sie [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) verwenden. Validieren Sie die Migration, indem Sie die Daten und das Schema überprüfen.

   :::image type="content" source="media/db2-to-sql-database-guide/compare-schema-in-ssms.png" alt-text="Screenshot, der den Vergleich des Schemas in SQL Server Management Studio zeigt.":::

## <a name="post-migration"></a>Nach der Migration 

Nachdem die Migration abgeschlossen ist, müssen Sie eine Reihe von Aufgaben nach der Migration durchführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen 

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testen umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests**: Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.
1. **Einrichten der Testumgebung**: Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.
1. **Ausführen der Validierungstests**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.
1. **Ausführen von Leistungstests:** Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

## <a name="advanced-features"></a>Erweiterte Funktionen 

Sie sollten unbedingt die erweiterten cloudbasierten Features von Azure SQL-Datenbank nutzen, z. B. die [integrierte Hochverfügbarkeit](../../database/high-availability-sla.md), die [Bedrohungserkennung](../../database/azure-defender-for-sql.md) und die [Überwachung und Optimierung Ihrer Workloads](../../database/monitor-tune-overview.md). 

Einige SQL Server-Features sind nur verfügbar, wenn der [Datenbank-Kompatibilitätsgrad](/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database) in den neuesten Kompatibilitätsgrad geändert wurde. 

## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung erhalten Sie in Form der folgenden Ressourcen, die im Rahmen eines Auftrags für ein echtes Migrationsprojekt entwickelt wurden:

|Asset  |BESCHREIBUNG  |
|---------|---------|
|[Data Workload Assessment Model and Tool (Datenarbeitsauslastungs-Bewertungsmodell und -tool)](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Dieses Tool stellt für eine angegebene Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch Bereitstellung eines automatisierten und einheitlichen Zielplattform-Entscheidungsprozesses dabei helfen, umfangreiche Bewertungen zu beschleunigen.|
|[Db2 zOS-Paket zur Erkennung und Bewertung von Datenbeständen](https://github.com/microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|Nach dem Ausführen des SQL-Skripts für eine-Datenbank können Sie die Ergebnisse in eine Datei im Dateisystem exportieren. Es werden verschiedene Dateiformate unterstützt, z. B. *.csv, damit Sie die Ergebnisse in externen Tools wie Tabellen erfassen können. Diese Methode kann nützlich sein, wenn Sie Ergebnisse einfach für Teams freigeben möchten, die die Workbench nicht installiert haben.|
|[IBM Db2 LUW Inventory Scripts and Artifacts](https://github.com/microsoft/DataMigrationTeam/blob/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts) (IBM Db2 LUW-Inventarskripts und -artefakte)|Dieses Asset enthält eine SQL-Abfrage, die auf IBM Db2 LUW Version 11.1-Systemtabellen zugreift und eine Zählung der Objekte nach Schema und Objekttyp, eine grobe Schätzung der "Rohdaten" in jedem Schema und die Größenordnung der Tabellen in jedem Schema liefert, wobei die Ergebnisse in einem CSV-Format gespeichert werden.|
|[Db2 LUW pure scale auf Azure - Einrichtungsanleitunge](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/DB2%20PureScale%20on%20Azure.pdf)|Dieser Leitfaden dient als Startpunkt für einen Db2-Implementierungsplan. Auch wenn Geschäftsanforderungen unterschiedlich sind, gilt dennoch dasselbe grundlegende Muster. Dieses Architekturmuster kann auch für OLAP-Anwendungen auf Azure verwendet werden.|

Das Data SQL Engineering-Team hat diese Ressourcen entwickelt. Die Hauptanwendung dieses Teams besteht darin, die komplexe Modernisierung für Datenplattform-Migrationsprojekte auf der Azure-Datenplattform von Microsoft freizugeben und zu beschleunigen.



## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Weitere Informationen zu Azure SQL-Datenbank:
   - [Übersicht über SQL-Datenbank](../../database/sql-database-paas-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/) 

- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für Kostenermittlung und Größenanpassung von Workloads, die zu Azure migriert werden](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs)
   -  [Ressourcen zur Cloudmigration](https://azure.microsoft.com/migration/resources) 

- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Ausführliche Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Assistent für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).