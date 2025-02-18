---
title: 'Oracle zu Azure SQL Managed Instance: Migrationsleitfaden'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre Oracle-Schemas über SQL Server Migration Assistant für Oracle zu Azure SQL Managed Instance migrieren.
ms.service: sql-managed-instance
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: cawrites
ms.date: 11/06/2020
ms.openlocfilehash: 74060212d5cf140dcd7a3ee099954123e52b82ae
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111961971"
---
# <a name="migration-guide-oracle-to-azure-sql-managed-instance"></a>Migrationsleitfaden: Oracle zu Azure SQL Managed Instance

[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqlmi.md)]

In diesem Leitfaden erfahren Sie, wie Sie Ihre Oracle-Schemas über SQL Server Migration Assistant für Oracle (SSMA für Oracle) zu Azure SQL Managed Instance migrieren.

Weitere Migrationsleitfäden finden Sie in den [Leitfäden zur Azure-Datenbankmigration](/data-migration).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Migration Ihres Oracle-Schemas zu SQL Managed Instance beginnen:

- Überprüfen, ob Ihre Quellumgebung unterstützt wird.
- Laden Sie [SSMA für Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258) herunter.
- Legen Sie ein [SQL Managed Instance](../../managed-instance/instance-create-quickstart.md)-Ziel fest.
- Fordern Sie die [erforderlichen Berechtigungen für SSMA für Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) und den [Anbieter](/sql/ssma/oracle/connect-to-oracle-oracletosql) an.
 
## <a name="pre-migration"></a>Vor der Migration

Wenn diese Voraussetzungen erfüllt sind, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit der Migration bewerten. Diese Schritte umfassen eine Bestandsaufnahme der Datenbanken, die migriert werden müssen, eine Untersuchung dieser Datenbanken auf potenzielle Migrationsprobleme oder Hindernisse sowie das Behandeln eventuell entdeckter Probleme.

### <a name="assess"></a>Bewerten

Mithilfe von SSMA für Oracle können Sie Datenbankobjekte und Daten überprüfen, Datenbanken für die Migration bewerten, Datenbankobjekte zu SQL Managed Instance migrieren und schließlich die Daten zur Datenbank migrieren.

So erstellen Sie eine Bewertung

1. Öffnen Sie [SSMA für Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258).
1. Wählen Sie **Datei** und dann **Neues Projekt** aus.
1. Geben Sie den Projektnamen und einen Speicherort für Ihr Projekt ein. Wählen Sie dann in der Dropdown Liste die Option **Azure SQL Managed Instance** als Migrationsziel aus, und klicken Sie auf **OK**.

   ![Screenshot von „Neues Projekt“](./media/oracle-to-managed-instance-guide/new-project.png)

1. Wählen Sie **Verbindung mit Oracle herstellen** aus. Geben Sie im Dialogfeld **Verbindung mit Oracle herstellen** Werte für die Oracle-Verbindungsdetails ein.

   ![Screenshot von „Verbindung mit Oracle herstellen“](./media/oracle-to-managed-instance-guide/connect-to-oracle.png)

1. Wählen Sie die Oracle-Schemas aus, die migriert werden sollen.

   ![Screenshot von der Auswahl des Oracle-Schemas](./media/oracle-to-managed-instance-guide/select-schema.png)

1. Klicken Sie im **Metadaten-Explorer von Oracle** mit der rechten Maustaste auf das zu migrierende Oracle-Schema, und wählen Sie dann **Bericht erstellen** aus, um einen HTML-Bericht zu generieren. Alternativ können Sie eine Datenbank auswählen und dann auf die Registerkarte **Bericht erstellen** klicken.

   ![Screenshot von „Bericht erstellen“](./media/oracle-to-managed-instance-guide/create-report.png)

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um ein Inventar der Oracle-Objekte sowie Informationen zum Aufwand für Schemakonvertierungen zu erhalten. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects.

   Ein Beispiel finden Sie unter `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2020_11_12T02_47_55\`.

   ![Screenshot von Bewertungsbericht](./media/oracle-to-managed-instance-guide/assessment-report.png)

### <a name="validate-the-data-types"></a>Überprüfen von Datentypen

Überprüfen Sie die standardmäßig festgelegten Datentypzuordnungen, und ändern Sie sie bei Bedarf basierend auf den Anforderungen. Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie in SSMA für Oracle die Option **Extras** aus, und klicken Sie dann auf **Projekteinstellungen**.
1. Wählen Sie die Registerkarte **Typzuordnung** aus.

   ![Screenshot von „Typzuordnungen“](./media/oracle-to-managed-instance-guide/type-mappings.png)

1. Sie können die Typzuordnung für jede Tabelle ändern, indem Sie die Tabelle im **Metadaten-Explorer von Oracle** auswählen.

### <a name="convert-the-schema"></a>Konvertieren des Schemas

So konvertieren Sie das Schema

1. (Optional:) Fügen Sie Anweisungen dynamische Abfragen oder Ad-hoc-Abfragen hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten, und klicken Sie dann auf **Anweisungen hinzufügen**.
1. Wählen Sie die Registerkarte **Verbindung mit Azure SQL Managed Instance herstellen** aus.
    1. Geben Sie die Verbindungsdetails ein, um eine Verbindung mit Ihrer Datenbank in der **verwalteten SQL-Datenbank-Instanz** herzustellen.
    1. Wählen Sie die Zieldatenbank in der Dropdownliste aus, oder geben Sie einen neuen Namen ein. In diesem Fall wird eine Datenbank auf dem Zielserver erstellt.
    1. Geben Sie die Authentifizierungsdetails ein, und klicken Sie auf **Verbinden**.

    ![Screenshot von „Verbindung mit Azure SQL Managed Instance herstellen“](./media/oracle-to-managed-instance-guide/connect-to-sql-managed-instance.png)

1. Klicken Sie im **Metadaten-Explorer von Oracle** mit der rechten Maustaste auf das Oracle-Schema, und klicken Sie dann auf **Schema konvertieren**. Alternativ können Sie Ihr Schema auswählen und dann auf die Registerkarte **Schema konvertieren** klicken.

   ![Screenshot von „Schema konvertieren“](./media/oracle-to-managed-instance-guide/convert-schema.png)

1. Vergleichen und überprüfen Sie nach Abschluss der Konvertierung die konvertierten Objekte mit den Ursprungsobjekten, um potenzielle Probleme zu identifizieren, und behandeln Sie diese anhand der Empfehlungen.

   ![Screenshot vom Vergleich von Tabellenempfehlungen](./media/oracle-to-managed-instance-guide/table-comparison.png)

1. Vergleichen Sie den konvertierten Transact-SQL-Text mit dem ursprünglichen Code und prüfen Sie die Empfehlungen.

   ![Screenshot vom Vergleich von Prozedurempfehlungen](./media/oracle-to-managed-instance-guide/procedure-comparison.png)

1. Wählen Sie im Ausgabebereich **Ergebnisse überprüfen** aus, und überprüfen Sie die Fehler im Bereich **Fehlerliste**.
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie im Menü **Datei** die Option **Projekt speichern** aus. So können Sie das Quell- und Zielschema offline auswerten und Probleme behandeln, bevor Sie das Schema auf SQL Managed Instance veröffentlichen.

## <a name="migrate"></a>Migrieren

Wenn Sie die Bewertung Ihrer Datenbanken und die Behandlung etwaiger Abweichungen abgeschlossen haben, besteht der nächste Schritt in der Durchführung des Migrationsprozesses. Die Migration umfasst zwei Schritte: das Veröffentlichen des Schemas und das Migrieren der Daten.

So veröffentlichen Sie das Schema und migrieren Ihre Daten
1. Veröffentlichen Sie das Schema, indem Sie im **Metadaten-Explorer von Azure SQL Managed Instance** im Knoten **Datenbanken** mit der rechten Maustaste auf die Datenbank klicken und **Mit Datenbank synchronisieren** auswählen.

   ![Screenshot von „Mit Datenbank synchronisieren“](./media/oracle-to-managed-instance-guide/synchronize-with-database.png)
   

1. Überprüfen Sie die Zuordnung zwischen dem Quellprojekt und dem Ziel.

   ![Screenshot von „Mit Datenbank synchronisieren“ zur Überprüfung](./media/oracle-to-managed-instance-guide/synchronize-with-database-review.png)

1. Migrieren Sie die Daten, indem Sie im **Metadaten-Explorer für Oracle** mit der rechten Maustaste auf das zu migrierende Schema oder Objekt klicken und **Daten migrieren** auswählen. Alternativ können Sie die Registerkarte **Daten migrieren** auswählen. Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um Daten für eine gesamte Datenbank zu migrieren. Wenn Sie Daten aus einzelnen Tabellen migrieren möchten, erweitern Sie die Datenbank, erweitern Sie **Tabellen**, und aktivieren Sie dann das Kontrollkästchen neben der gewünschten Tabelle. Deaktivieren Sie die Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen.

   ![Screenshot von „Daten migrieren“](./media/oracle-to-managed-instance-guide/migrate-data.png)

1. Geben Sie die Verbindungsdetails für Oracle und SQL Managed Instance an.
1. Überprüfen Sie nach Abschluss der Migration den **Bericht zur Datenmigration**.

   ![Screenshot von Bericht zur Datenmigration](./media/oracle-to-managed-instance-guide/data-migration-report.png)

1. Stellen Sie über [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) eine Verbindung mit Ihrer Instanz von SQL Managed Instance her, und validieren Sie die Migration, indem Sie die Daten und das Schema überprüfen.

   ![Screenshot der Validierung in SSMA für Oracle](./media/oracle-to-managed-instance-guide/validate-data.png)

Stattdessen können Sie die Migration auch über SQL Server Integration Services durchführen. Weitere Informationen finden Sie unter:

- [Erste Schritte mit SQL Server Integration Services](/sql/integration-services/sql-server-integration-services)
- [SQL Server Integration Services für Azure und die Hybriddatenverschiebung](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx)

## <a name="post-migration"></a>Nach der Migration

Nach erfolgreichem Abschluss der *Migrationsphase* müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

Das [Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) ist eine Erweiterung für Visual Studio Code, mit der Sie Ihren Java-Quellcode analysieren und API-Aufrufe und Abfragen mit Datenzugriff erkennen können. Das Toolkit zeigt auf einen Blick, was für die Unterstützung des neuen Datenbank-Back-Ends erforderlich ist. Weitere Informationen finden Sie im Blogbeitrag [Migrieren einer Java-Anwendung von Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727).

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests:** Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.
2. **Einrichten der Testumgebung:** Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.
3. **Führen Sie die Validierungstests aus**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.
4. **Ausführen von Leistungstests:** Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

### <a name="optimize"></a>Optimieren

Die Phase nach der Migration ist entscheidend für den Abgleich von Problemen mit der Datengenauigkeit, die Überprüfung der Vollständigkeit und das Beheben von Leistungsproblemen der Workload.

> [!NOTE]
> Weitere Informationen zu diesen Problemen sowie die erforderlichen Schritte zur Problembehandlung finden Sie im [Leitfaden zur Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).

## <a name="migration-assets"></a>Migrationsressourcen

Weitere Unterstützung bei der Durchführung dieses Migrationsszenarios finden Sie in den folgenden Ressourcen. Sie wurden für die Unterstützung eines echten Migrationsprojekts entwickelt.

| **Titel/Link**                                                                                                                                          | **Beschreibung**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Data Workload Assessment Model and Tool](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Dieses Tool stellt für eine bestimmte Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen oder Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch einen automatisierten und einheitlichen Entscheidungsprozess für die Zielplattform dabei helfen, Bewertungen von umfangreichen Datenbeständen zu beschleunigen.                                                          |
| [Oracle Inventory Script Artifacts](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Diese Ressource umfasst eine PL/SQL-Abfrage für Oracle-Systemtabellen, die die Anzahl von Objekten nach Schematyp, Objekttyp und Status bereitstellt. Darüber hinaus erhalten Sie eine grobe Schätzung zu den „Rohdaten“ sowie zur Größe der Tabellen in jedem Schema. Die Ergebnisse werden im CSV-Format gespeichert.                                                                                                               |
| [Automate SSMA Oracle Assessment Collection & Consolidation](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Diese Ressourcensammlung verwendet eine CSV-Datei als Eingabe („sources.csv“ in den Projektordnern), um die XML-Dateien zu erstellen, die zum Ausführen einer SSMA-Bewertung im Konsolenmodus benötigt werden. Die Datei „source.csv“ wird vom Kunden bereitgestellt und basiert auf einer Inventarisierung vorhandener Oracle-Instanzen. Die Ausgabedateien sind „AssessmentReportGeneration_source_1.xml“, „ServersConnectionFile.xml“ und „VariableValueFile.xml“.|
| [SSMA für Oracle: häufige Probleme und deren Behebung](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | In Oracle kann eine nicht skalare Bedingung in der WHERE-Klausel zugewiesen werden. Dieser Bedingungstyp wird von SQL Server jedoch nicht unterstützt. SSMA für Oracle konvertiert daher keine Abfragen, die eine nicht skalare Bedingung in der WHERE-Klausel aufweisen. Stattdessen wird der Fehler O2SS0001 generiert. In diesem Whitepaper finden Sie weitere Einzelheiten zu diesem Problem sowie die erforderlichen Schritte zur Problembehandlung.          |
| [Handbuch für die Migration von Oracle zu SQL Server](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | In diesem Whitepaper sind die Aufgaben bei der Migration eines Oracle-Schemas zur neuesten Version von SQL Server-Datenbank beschrieben. Wenn für die Migration Änderungen an Features oder Funktionen erforderlich sind, müssen die Auswirkungen dieser Änderungen auf die Anwendungen, die die Datenbank verwenden, sorgfältig abgewogen werden.                                                     |
|[Oracle zu SQL MI – Hilfsprogramm „Database Compare“](https://www.microsoft.com/download/details.aspx?id=103016)|SSMA für Oracle Tester ist das empfohlene Tool, um automatisch die Umwandlung von Datenbankobjekten und Datenmigration zu überprüfen. Außerdem enthält es eine Obermenge der Database Compare-Funktionen.<br /><br />Wenn Sie nach einer alternativen Datenüberprüfungsoption suchen, können Sie Daten mithilfe des Hilfsprogramms „Database Compare“ in ausgewählten oder allen Tabellen, Zeilen und Spalten bis auf Zeilen- oder Spaltenebene vergleichen.|

Das Data SQL Engineering-Team hat diese Ressourcen entwickelt. Die Hauptanwendung dieses Teams besteht darin, die komplexe Modernisierung für Datenplattform-Migrationsprojekte auf der Azure-Datenplattform von Microsoft freizugeben und zu beschleunigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarien sowie Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Informationen zu SQL Managed Instance finden Sie hier:
  - [Überblick über Azure SQL Managed Instance](../../managed-instance/sql-managed-instance-paas-overview.md)
  - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/en-us/pricing/tco/calculator/)

- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Arbeitsauslastungen](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs)

- Videoinhalte finden Sie unter:
    - [Übersicht über den Migrationsprozess und die Tools und Dienste, die für die Durchführung von Bewertung und Migration empfohlen werden](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
