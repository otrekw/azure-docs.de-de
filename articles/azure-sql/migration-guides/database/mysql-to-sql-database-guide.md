---
title: 'Migrationsleitfaden: MySQL zu Azure SQL-Datenbank'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre MySQL-Datenbanken mithilfe von SQL Server Migration Assistant für MySQL (SSMA für MySQL) zu einer Azure SQL-Datenbank migrieren.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 6aa6537cefb110662f9fbc535832d8c9423a6f68
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108136587"
---
# <a name="migration-guide-mysql-to-azure-sql-database"></a>Migrationsleitfaden: MySQL zu Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

In diesem Leitfaden erfahren Sie, wie Sie Ihre MySQL-Datenbank mithilfe von [SQL Server Migration](https://azure.microsoft.com/en-us/migration/sql-server/) Assistant für MySQL (SSMA für MySQL) zu einer Azure SQL-Datenbank [migrieren](https://azure.microsoft.com/migration/migration-journey). 

Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](/data-migration). 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Migration der MySQL-Datenbank zu einer SQL-Datenbank beginnen, gehen Sie folgendermaßen vor:

- Überprüfen Sie, ob Ihre Quellumgebung unterstützt wird. Derzeit werden MySQL 5.6 und 5.7 unterstützt. 
- Laden Sie das Tool [SQL Server Migration Assistant für MySQL](https://www.microsoft.com/download/details.aspx?id=54257) herunter, und installieren Sie es.
- Stellen Sie sicher, dass Sie über Konnektivität und ausreichende Berechtigungen für den Zugriff auf Quelle und Ziel verfügen.

## <a name="pre-migration"></a>Vor der Migration 

Nachdem Sie diese Voraussetzungen erfüllt haben, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit Ihrer [Azure-Cloudmigration](https://azure.microsoft.com/migration) bewerten.

### <a name="assess"></a>Bewerten 

Mit SQL Server Migration Assistant (SSMA) für MySQL können Sie Datenbankobjekte und Daten überprüfen sowie Datenbanken für die Migration bewerten. 

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen:

1. Öffnen Sie [SSMA für MySQL](https://www.microsoft.com/download/details.aspx?id=54257). 
1. Wählen Sie **Datei** und dann **Neues Projekt** aus. 
1. Geben Sie im Bereich **Neues Projekt** einen Namen und einen Speicherort für das Projekt ein, und wählen Sie dann in der Dropdown Liste **Migrieren zu** die Option **Azure SQL-Datenbank** aus. 
1. Wählen Sie **OK** aus.

   ![Screenshot des Bereichs „Neues Projekt“ zur Eingabe des Namens, des Speicherorts und des Ziels für das Migrationsprojekt](./media/mysql-to-sql-database-guide/new-project.png)

1. Wählen Sie die Registerkarte **Verbindung mit MySQL herstellen** aus, und geben Sie dann Details zum Verbinden Ihres MySQL-Servers an. 

   ![Screenshot des Bereichs „Verbindung mit MySQL herstellen“ zur Angabe von Verbindungen mit der Quelle](./media/mysql-to-sql-database-guide/connect-to-mysql.png)

1. Klicken Sie im Bereich **Metadaten-Explorer für MySQL** mit der rechten Maustaste auf das MySQL-Schema, und wählen Sie dann **Bericht erstellen** aus. Alternativ dazu können Sie die Registerkarte **Bericht erstellen** in der oberen rechten Ecke auswählen.

   ![Screenshot der Links „Bericht erstellen“ in SSMA für MySQL](./media/mysql-to-sql-database-guide/create-report.png)

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken, Fehler und Warnungen zu verstehen. Analysieren Sie diese Informationen, um Konvertierungsprobleme und mögliche Lösungen zu ermitteln. 
   Sie können den Bericht auch in Excel öffnen, um ein Inventar der MySQL-Objekte zu erhalten sowie den Aufwand für Schemakonvertierungen nachzuvollziehen. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects. Beispiel: 
   
   `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`
 
   ![Screenshot eines Beispiels für einen Konvertierungsbericht in SSMA](./media/mysql-to-sql-database-guide/conversion-report.png)

### <a name="validate-the-data-types"></a>Überprüfen von Datentypen

Überprüfen Sie die standardmäßig festgelegten Datentypzuordnungen, und ändern Sie sie bei Bedarf basierend auf den Anforderungen. Gehen Sie folgendermaßen vor: 

1. Wählen Sie **Extras** und dann **Projekteigenschaften** aus.  
1. Klicken Sie auf die Registerkarte **Typzuordnungen**. 

   ![Screenshot des Bereichs „Typzuordnung“ in SSMA für MySQL](./media/mysql-to-sql-database-guide/type-mappings.png)

1. Sie können die Typzuordnung für die einzelnen Tabellen ändern, indem Sie den Tabellennamen im Bereich **Metadaten-Explorer für MySQL** auswählen. 

### <a name="convert-the-schema"></a>Konvertieren des Schemas 

Gehen Sie folgendermaßen vor, um das Schema zu konvertieren: 

1. (Fakultativ) Zum Konvertieren von dynamischen oder spezialisierten Abfragen klicken Sie mit der rechten Maustaste auf den Knoten, und wählen Sie dann **Anweisung hinzufügen** aus. 

1. Wählen Sie die Registerkarte **mit Azure SQL-Datenbank verbinden** aus, und gehen Sie dann wie folgt vor:

   a. Geben Sie die Details zum Herstellen einer Verbindung mit Ihrer SQL-Datenbank ein.  
   b. Wählen Sie in der Dropdownliste die Zielinstanz in SQL-Datenbank aus. Alternativ können Sie einen neuen Namen angeben. In diesem Fall wird eine Datenbank auf dem Zielserver erstellt.  
   c. Angeben von Authentifizierungsdetails.  
   d. Wählen Sie **Verbinden** aus.

   ![Screenshot des Bereichs „Verbindung mit Azure SQL-Datenbank herstellen“ in SSMA für MySQL](./media/mysql-to-sql-database-guide/connect-to-sqldb.png)
 
1. Klicken Sie mit der rechten Maustaste auf das Schema, mit dem Sie arbeiten, und wählen Sie dann **Schema konvertieren** aus. Alternativ können Sie die Registerkarte **Schema konvertieren** in der rechten oberen Ecke auswählen.

   ![Screenshot des Befehls „Schema konvertieren“ im Bereich „Metadaten-Explorer für MySQL“](./media/mysql-to-sql-database-guide/convert-schema.png)

1. Nach Abschluss der Konvertierung überprüfen und vergleichen Sie die konvertierten Objekte mit den Ursprungsobjekten, um potenzielle Probleme zu identifizieren und diese anhand der Empfehlungen zu behandeln. 

   ![Screenshot eines Vergleichs der konvertierten Objekte mit den ursprünglichen Objekten](./media/mysql-to-sql-database-guide/table-comparison.png)

   Vergleichen Sie den konvertierten Transact-SQL-Text mit dem ursprünglichen Code und prüfen Sie die Empfehlungen.

   ![Screenshot, der einen Vergleich der konvertierten Abfragen mit dem Quellcode anzeigt.](./media/mysql-to-sql-database-guide/procedure-comparison.png)

1. Klicken Sie im Bereich **Ausgabe** auf **Ergebnisse überprüfen**, und überprüfen Sie die Fehler im Bereich **Fehlerliste**. 
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie hierzu **Datei**  >  **Projekt speichern** aus. So können Sie das Quell- und Zielschema offline auswerten und warten, bevor Sie das Schema auf Ihrer SQL-Datenbank veröffentlichen.

   Vergleichen Sie die konvertierten Prozeduren mit den ursprünglichen Prozeduren, wie hier gezeigt: 

   ![Screenshot eines Vergleichs der konvertierten Prozeduren mit den ursprünglichen Prozeduren](./media/mysql-to-sql-database-guide/procedure-comparison.png)


## <a name="migrate-the-databases"></a>Migrieren der Datenbanken 

Nachdem Sie Ihre Datenbanken ausgewertet und Abweichungen gelöst haben, können Sie den Migrationsprozess ausführen. Die Migration umfasst zwei Schritte: das Veröffentlichen des Schemas und das Migrieren der Daten. 

Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und die Daten zu migrieren: 

1. Veröffentlichen Sie das Schema. Klicken Sie in der Anzeige des **Azure SQL Database Metadata Explorers** mit der rechten Maustaste auf die Datenbank, mit der Sie arbeiten, und wählen Sie dann **mit Datenbank synchronisieren** aus. Mit dieser Aktion wird das MySQL-Schema in Ihrer SQL-Datenbank veröffentlicht.

   ![Screenshot des Bereichs für die Synchronisierung mit der Datenbank zum Überprüfen der Datenbankzuordnung](./media/mysql-to-sql-database-guide/synchronize-database-review.png)

1. Migrieren der Daten: Klicken Sie im **Metadaten-Explorer für MySQL** mit der rechten Maustaste auf das zu migrierende MySQL-Schema, und wählen Sie dann **Daten migrieren** aus. Alternativ können Sie rechts oben die Registerkarte **Daten migrieren** auswählen.

   Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um die Daten einer gesamten Datenbank zu migrieren. Wenn Sie Daten aus einzelnen Tabellen migrieren möchten, erweitern Sie die Datenbank, erweitern Sie die **Tabellen**, und aktivieren Sie dann das Kontrollkästchen neben der Tabelle. Deaktivieren Sie das Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen.

   ![Screenshot des Befehls „Daten migrieren“ im Bereich „Metadaten-Explorer für MySQL“](./media/mysql-to-sql-database-guide/migrate-data.png)

1. Überprüfen Sie nach Abschluss der Migration den **Bericht zur Datenmigration**.
   
   ![Screenshot des Berichts zur Datenmigration](./media/mysql-to-sql-database-guide/data-migration-report.png)

1. Stellen Sie über [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) eine Verbindung mit Ihrer SQL-Datenbank her, und überprüfen Sie die Migration anhand der Daten und des Schemas.

   ![Screenshot von SQL Server Management Studio](./media/mysql-to-sql-database-guide/validate-in-ssms.png)

## <a name="post-migration"></a>Nach der Migration 

Nach erfolgreichem Abschluss der *Migrationsphase* müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests:** Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die sowohl gegen die Quell- als auch gegen die Zieldatenbank ausgeführt werden. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.

1. **Einrichten der Testumgebung:** Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.

1. **Führen Sie die Validierungstests aus**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.

1. **Führen Sie Leistungstests aus**: Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

### <a name="optimize"></a>Optimieren

Die Phase nach der Migration ist entscheidend für den Abgleich von Problemen mit der Datengenauigkeit, die Überprüfung der Vollständigkeit und das Beheben von Leistungsproblemen der Workload.

Weitere Informationen zu diesen Problemen sowie die erforderlichen Schritte zur Problembehandlung finden Sie im [Leitfaden zur Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).

## <a name="migration-assets"></a>Migrationsressourcen

Weitere Unterstützung bei der Durchführung dieses Migrationsszenarios finden Sie in der folgenden Ressource. Es wurde für die Unterstützung eines echten Migrationsprojekts entwickelt.

| Titel | BESCHREIBUNG |
| --- | --- |
| [Data Workload Assessment Model and Tool (Datenarbeitsauslastungs-Bewertungsmodell und -tool)](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Stellt für bestimmte Workloads Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch einen automatisierten und einheitlichen Entscheidungsprozess für die Zielplattform dabei helfen, Bewertungen von umfangreichen Datenbeständen zu beschleunigen. |

Das Data SQL Engineering-Team hat diese Ressourcen entwickelt. Die Hauptanwendung dieses Teams besteht darin, die komplexe Modernisierung für Datenplattform-Migrationsprojekte auf der Azure-Datenplattform von Microsoft freizugeben und zu beschleunigen.

## <a name="next-steps"></a>Nächste Schritte 

- Um die Kostenersparnisse einschätzen können, die Sie durch eine Migration Ihrer Workloads zu Azure realisieren würden, sehen Sie sich den [Azure-Gesamtkostenrechner](https://aka.ms/azure-tco) an.

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios sowie Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](https://datamigration.microsoft.com/). 

- Videos zur Migration finden Sie unter [Übersicht über den Migrationsprozess sowie empfohlene Tools und Dienste für die Migration und Bewertung](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).

- Weitere [Cloudmigrationsressourcen](https://azure.microsoft.com/migration/resources/)finden Sie unter [Cloudmigrationslösungen](https://azure.microsoft.com/migration).