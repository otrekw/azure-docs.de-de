---
title: 'Zugreifen auf Azure SQL-Datenbank: Migrationsleitfaden'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre Microsoft Access-Datenbanken mit SQL Server Migration Assistant für Access (SSMA für Access) zur Azure SQL-Datenbank migrieren.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 73ee4d4be16284880b10df4a52b422a08c04c6a0
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107284186"
---
# <a name="migration-guide-access-to-azure-sql-database"></a>Migrationsleitfaden: Zugreifen auf Azure SQL-Datenbank

In diesem Leitfaden erfahren Sie, wie Sie Ihre Microsoft Access-Datenbank mithilfe von [SQL Server Migration](https://azure.microsoft.com/en-us/migration/sql-server/) Assistant für Access (SSMA für Access) zu einer Azure SQL-Datenbank [migrieren](https://azure.microsoft.com/migration/migration-journey).

Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Migration der Access-Datenbank zu einer SQL-Datenbank beginnen, gehen Sie folgendermaßen vor:

- Überprüfen Sie, ob Ihre Quellumgebung unterstützt wird. 
- Herunterladen und installieren [von SQL Server Migration Assistant für Access](https://www.microsoft.com/download/details.aspx?id=54255).
- Stellen Sie sicher, dass Sie über Konnektivität und ausreichende Berechtigungen für den Zugriff auf Quelle und Ziel verfügen.

## <a name="pre-migration"></a>Vor der Migration

Nachdem Sie diese Voraussetzungen erfüllt haben, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit Ihrer [Azure-Cloudmigration](https://azure.microsoft.com/migration) bewerten.


### <a name="assess"></a>Bewerten 

Verwenden Sie SSMA für Access, um Datenbankobjekte und -daten zu überprüfen und Datenbanken zur Migration zu bewerten. 

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen: 

1. Öffnen Sie [SSMA für Access](https://www.microsoft.com/download/details.aspx?id=54255). 
1. Wählen Sie **Datei**, und wählen Sie dann **Neues Projekt**. 
1. Geben Sie einen Projektnamen und einen Speicherort für das Projekt an, und wählen Sie dann **Azure SQL-Datenbank** in der Dropdownliste als Migrationsziel aus. 
1. Wählen Sie **OK** aus. 

   ![Screenshot des Bereichs „Neues Projekt“ zur Eingabe des Namens und des Speicherorts des Migrationsprojekts.](./media/access-to-sql-database-guide/new-project.png)

1. Klicken Sie auf **Datenbanken hinzufügen**, und wählen Sie Datenbanken aus, die dem neuen Projekt hinzugefügt werden sollen. 

   ![Screenshot der Registerkarte „Datenbanken hinzufügen“ in SSMA für Access.](./media/access-to-sql-database-guide/add-databases.png)

1. Klicken Sie im Bereich **Access Metadata Explorer** mit der rechten Maustaste auf eine Datenbank, und wählen Sie dann **Bericht erstellen** aus. Alternativ dazu können Sie die Registerkarte **Bericht erstellen** in der oberen rechten Ecke auswählen.

   ![Screenshot des Befehls „Bericht erstellen“ im Access Metadata Explorer.](./media/access-to-sql-database-guide/create-report.png)

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um ein Inventar der Access-Objekte zu erhalten sowie Informationen zum Aufwand für Schemakonvertierungen zu verstehen. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects. Beispiel:

   `drive:\<username>\Documents\SSMAProjects\MyAccessMigration\report\report_<date>`

   ![Screenshot eines Beispiels für eine Bewertung des Datenbank-Berichts in SSMA.](./media/access-to-sql-database-guide/sample-assessment.png)

### <a name="validate-the-data-types"></a>Überprüfen von Datentypen

Überprüfen Sie die standardmäßig festgelegten Datentypzuordnungen, und ändern Sie diese bei Bedarf basierend auf den Anforderungen. Dazu gehen Sie wie folgt vor:

1. Wählen Sie in SSMA für Access die Option **Tools** aus, und klicken Sie dann auf **Projekteinstellungen**. 
1. Klicken Sie auf die Registerkarte **Type Mappings** (Typzuordnungen). 

   ![Screenshot des Bereichs „Typzuordnung“ in SSMA für Access.](./media/access-to-sql-database-guide/type-mappings.png)

1. Sie können die Typzuordnung für jede Tabelle ändern, indem Sie den Tabellennamen in **Access Metadata Explorer** auswählen.


### <a name="convert-the-schema"></a>Schema konvertieren

Gehen Sie folgendermaßen vor, um Datenbankobjekte zu konvertieren: 

1. Wählen Sie die Registerkarte **mit Azure SQL-Datenbank verbinden** aus, und gehen Sie dann wie folgt vor:

   a. Geben Sie die Details zum Herstellen einer Verbindung mit Ihrer SQL-Datenbank ein.  
   b. Wählen Sie in der Dropdownliste die Zielinstanz in SQL-Datenbank aus. Oder Sie können einen neuen Namen eingeben. In diesem Fall wird eine Datenbank auf dem Zielserver erstellt.  
   c. Angeben von Authentifizierungsdetails.   
   d. Wählen Sie **Verbinden** aus.

   ![Screenshot des Bereichs „Verbinden mit Azure SQL-Datenbank“ für die Eingabe von Verbindungsdetails.](./media/access-to-sql-database-guide/connect-to-sqldb.png)

1. Klicken Sie im Bereich **Access Metadata Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie dann **Schema konvertieren** aus. Alternativ können Sie Ihre Datenbank auswählen und dann die Registerkarte **Schema konvertieren** auswählen.

   ![Screenshot des Befehls „Schema konvertieren“ im Bereich „Access Metadata Explorer“.](./media/access-to-sql-database-guide/convert-schema.png)

1. Vergleichen und überprüfen Sie nach Abschluss der Konvertierung die konvertierten Objekte mit den Ursprungsobjekten, um potenzielle Probleme zu identifizieren und diese Probleme anhand der Empfehlungen zu lösen.

   ![Screenshot, der einen Vergleich der konvertierten Objekte mit den Quellobjekten anzeigt.](./media/access-to-sql-database-guide/table-comparison.png)

    Vergleichen Sie den konvertierten Transact-SQL-Text mit dem ursprünglichen Code und prüfen Sie die Empfehlungen.

   ![Screenshot, der einen Vergleich der konvertierten Abfragen mit dem Quellcode anzeigt.](./media/access-to-sql-database-guide/query-comparison.png) 

1. (Optional) Zum Konvertieren eines einzelnen Objekts klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Schema konvertieren** aus. Konvertierte Objekte werden in einem fett formatierten Text im **Access Metadata Explorer** angezeigt: 

   ![Ein Screenshot, der anzeigt, dass die Objekte in Access Metadata Explorer konvertiert werden.](./media/access-to-sql-database-guide/converted-items.png)
 
1. Wählen Sie im Bereich **Ausgabe** das Symbol **Ergebnisse überprüfen** aus, und überprüfen Sie die Fehler im Bereich **Fehlerliste**. 
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie hierzu **Datei**  >  **Projekt speichern** aus. So können Sie das Quell- und Zielschema offline auswerten und warten, bevor Sie das Schema auf der SQL-Datenbank veröffentlichen.

## <a name="migrate-the-databases"></a>Migrieren der Datenbanken

Nachdem Sie Ihre Datenbanken ausgewertet und Abweichungen gelöst haben, können Sie den Migrationsprozess ausführen. Die Migration von Daten ist ein Massenladevorgang, bei dem Datenzeilen in Transaktionen in eine Azure SQL-Datenbank verschoben werden. Die Anzahl der Zeilen, die in jeder Transaktion in Ihre SQL-Datenbank geladen werden sollen, wird in den Projekteinstellungen konfiguriert.

Gehen Sie folgendermaßen vor, um Ihr Schema zu veröffentlichen und die Daten mithilfe von SSMA für Access zu migrieren: 

1. Sofern nicht bereits geschehen, wählen Sie **Connect to Azure SQL Database** (Verbindung mit Azure SQL-Datenbank herstellen) aus, und geben Sie die Verbindungsdetails an. 

1. Veröffentlichen des Schemas. Klicken Sie im Bereich **Metadaten-Explorer von Azure SQL-Datenbank** mit der rechten Maustaste auf die Datenbank, mit der Sie arbeiten, und wählen Sie dann **mit Datenbank synchronisieren** aus. Mit dieser Aktion wird das MySQL-Schema in der SQL-Datenbank veröffentlicht.

1. Überprüfen Sie im Bereich **Synchronisierung mit der Datenbank** die Zuordnung zwischen dem Quellprojekt und Ihrem Ziel:

   ![Screenshot des Bereichs „Synchronisierung mit der Datenbank“ zum Überprüfen der Synchronisierung mit der Datenbank.](./media/access-to-sql-database-guide/synchronize-with-database-review.png)

1. Verwenden Sie **Access Metadata Explorer**, um die Kontrollkästchen neben den Elementen zu aktivieren, die Sie migrieren möchten. Aktivieren Sie das Kontrollkästchen neben der Datenbank, um die gesamte Datenbank zu migrieren. 

1. Migrieren der Daten. Klicken Sie mit der rechten Maustaste auf die zu migrierende Datenbank oder das zu migrierende Objekt, und wählen Sie **Daten migrieren** aus. Alternativ können Sie oben rechts die Registerkarte **Daten migrieren** auswählen.  

   Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um die Daten einer gesamten Datenbank zu migrieren. Wenn Sie Daten aus einzelnen Tabellen migrieren möchten, erweitern Sie die Datenbank, erweitern Sie **Tabellen**, und aktivieren Sie dann das Kontrollkästchen neben der gewünschten Tabelle. Deaktivieren Sie das Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen.

    ![Screenshot des Befehls „Daten migrieren“ im Bereich „Access Metadata Explorer“.](./media/access-to-sql-database-guide/migrate-data.png)

1. Zeigen Sie nach Abschluss der Migration den **Bericht zur Datenmigration** an.  

    ![Screenshot des Bereichs „Bericht zur Datenmigration“ mit einem Beispielbericht zur Überprüfung.](./media/access-to-sql-database-guide/migrate-data-review.png)

1. Stellen Sie über [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) eine Verbindung mit Ihrer Instanz von Azure SQL-Datenbank her, und überprüfen Sie die Migration anhand der Daten und des Schemas.

   ![Screenshot von SQL Server Management Studio Object Explorer zum Überprüfen der Migration in SSMA.](./media/access-to-sql-database-guide/validate-data.png)

## <a name="post-migration"></a>Nach der Migration 

Nach erfolgreichem Abschluss der *Migrationsphase* müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests**: Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.

1. **Einrichten einer Testumgebung**: Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.

1. **Ausführen der Validierungstests**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.

1. **Ausführen von Leistungstests**: Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.


### <a name="optimize"></a>Optimieren

Die Postmigrationsphase ist unerlässlich, um Datengenauigkeitsprobleme abzustimmen, die Vollständigkeit zu bestätigen und Leistungsprobleme der Workload zu beheben.

Weitere Informationen zu diesen Problemen sowie die erforderlichen Schritte zur Problembehandlung finden Sie im [Leitfaden zur Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).

## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung bei der Durchführung dieses Migrationsszenarios finden Sie in der folgenden Ressource. Es wurde für die Unterstützung eines echten Migrationsprojekts entwickelt.

| Titel | BESCHREIBUNG |
| --- | --- |
| [Data Workload Assessment Model and Tool (Datenarbeitsauslastungs-Bewertungsmodell und -tool)](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Stellt für bestimmte Workloads Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch einen automatisierten und einheitlichen Entscheidungsprozess für die Zielplattform dabei helfen, Bewertungen von umfangreichen Datenbeständen zu beschleunigen. |

Das Data SQL Engineering-Team hat diese Ressourcen entwickelt. Die Hauptanwendung dieses Teams besteht darin, die komplexe Modernisierung für Datenplattform-Migrationsprojekte auf der Azure-Datenplattform von Microsoft freizugeben und zu beschleunigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios sowie Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Weitere Informationen zu Azure SQL-Datenbank:
   - [Übersicht über SQL-Datenbank](../../database/sql-database-paas-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/) 


- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 
   -  [Ressourcen zur Cloudmigration](https://azure.microsoft.com/migration/resources)


- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit (Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Überblick über den Assistenten für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).
