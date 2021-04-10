---
title: 'Oracle zu SQL Server auf Azure-VMs: Migrationsleitfaden'
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre Oracle-Schemata mithilfe von SQL Server Migration Assistant für Oracle zu SQL Server auf Azure-VMs migrieren.
ms.service: virtual-machines-sql
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: 1767f1f990326e513393b8ce47e1ed8485f73849
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656475"
---
# <a name="migration-guide-oracle-to-sql-server-on-azure-vm"></a>Migrationsleitfaden: Oracle zu SQL Server auf Azure-VMs
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

In diesem Leitfaden erfahren Sie, wie Sie Ihre Oracle-Schemata mithilfe von SQL Server Migration Assistant für Oracle zu SQL Server auf Azure-VMs migrieren. 

Informationen zu anderen Szenarios finden Sie im [Leitfaden zur Datenbankmigration](https://datamigration.microsoft.com/).

## <a name="prerequisites"></a>Voraussetzungen 

Für die Migration Ihres Oracle-Schemas zu SQL Server auf Azure-VMs sind folgende Schritte bzw. Voraussetzungen erforderlich:

- Überprüfen, ob Ihre Quellumgebung unterstützt wird.
- Herunterladen von [SQL Server Migration Assistant (SSMA) für Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258).
- Eine [SQL Server-Ziel-VM](../../virtual-machines/windows/sql-vm-create-portal-quickstart.md).
- Die [erforderlichen Berechtigungen für SSMA für Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) und [Anbieter](/sql/ssma/oracle/connect-to-oracle-oracletosql).

## <a name="pre-migration"></a>Vor der Migration

Überprüfen Sie bei der Vorbereitung Ihrer Migration zur Cloud, ob Ihre Quellumgebung unterstützt wird und alle Voraussetzungen erfüllt sind. Durch diese Maßnahmen wird eine effiziente und erfolgreiche Migration sichergestellt.

Diese Schritte umfassen eine Bestandsaufnahme der Datenbanken, die migriert werden müssen, eine Untersuchung dieser Datenbanken auf potenzielle Migrationsprobleme oder Hindernisse sowie das Behandeln eventuell entdeckter Probleme. 

### <a name="discover"></a>Entdecken

Ermitteln Sie mithilfe des [MAP Toolkits](https://go.microsoft.com/fwlink/?LinkID=316883) vorhandene Datenquellen und Details zu den Features, die von Ihrem Unternehmen verwendet werden. Dadurch erhalten Sie einen besseren Überblick und können die Migration besser planen. Bei diesem Vorgang ermitteln Sie u. a. alle Oracle-Instanzen Ihrer Organisation, die innerhalb des Netzwerks ausgeführt werden, sowie die jeweilige Version und die verwendeten Features.

Führen Sie für eine Inventarüberprüfung mithilfe des MAP Toolkits die folgenden Schritte aus: 

1. Öffnen Sie das [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883).
1. Wählen Sie **Datenbank erstellen/auswählen** aus.

   ![Datenbank auswählen](./media/oracle-to-sql-on-azure-vm-guide/select-database.png)

1. Wählen Sie **Inventardatenbank erstellen** aus, geben Sie einen Namen und eine kurze Beschreibung für die neue Datenbank ein, und wählen Sie dann **OK** aus. 

   :::image type="content" source="media/oracle-to-sql-on-azure-vm-guide/create-inventory-database.png" alt-text="Bestandsdatenbank erstellen":::

1. Wählen Sie **Inventardaten sammeln** aus, um den **Assistenten für Inventar und Bewertung** zu öffnen. 

   :::image type="content" source="media/oracle-to-sql-on-azure-vm-guide/collect-inventory-data.png" alt-text="Sammeln von Bestandsdaten":::

1. Wählen Sie im **Assistenten für Inventar und Bewertung** die Option **Oracle** und dann **Weiter** aus. 

   ![„Oracle“ auswählen](./media/oracle-to-sql-on-azure-vm-guide/choose-oracle.png)

1. Wählen Sie die Option für die Computersuche aus, die sich am besten für Ihre Geschäftsanforderungen und Umgebung eignet, und wählen Sie dann **Weiter** aus: 

   ![Wählen Sie die Option für die Computersuche aus, die Ihren Geschäftsanforderungen am besten entspricht](./media/oracle-to-sql-on-azure-vm-guide/choose-search-option.png)

1. Geben Sie Anmeldeinformationen für die Systeme ein, die untersucht werden sollen, oder erstellen Sie neue Anmeldeinformationen, und wählen Sie dann **Weiter** aus.

    ![Eingeben von Anmeldeinformationen](./media/oracle-to-sql-on-azure-vm-guide/choose-credentials.png)

1. Legen Sie die Reihenfolge der Anmeldeinformationen fest, und wählen Sie dann **Weiter** aus. 

   ![Festlegen der Reihenfolge der Anmeldeinformationen](./media/oracle-to-sql-on-azure-vm-guide/set-credential-order.png)  

1. Geben Sie die Anmeldeinformationen für die einzelnen Computer ein, die entdeckt werden sollen. Sie können individuelle Anmeldeinformationen für jeden Computer bzw. jede VM verwenden oder **Liste aller Computeranmeldeinformationen verwenden** auswählen.  


   ![Geben Sie die Anmeldeinformationen für die einzelnen Computer ein, die ermittelt werden sollen.](./media/oracle-to-sql-on-azure-vm-guide/specify-credentials-for-each-computer.png)


1. Überprüfen Sie Ihre Auswahl in der Zusammenfassung, und wählen Sie dann **Fertig stellen** aus.

   ![Überprüfen der Zusammenfassung](./media/oracle-to-sql-on-azure-vm-guide/review-summary.png)

1. Zeigen Sie nach Abschluss der Überprüfung die Zusammenfassung der **Datensammlung** an. Die Überprüfung kann einige Minuten dauern (abhängig von der Anzahl von Datenbanken). Wenn Sie fertig sind, wählen Sie **Schließen** aus. 

   ![Zusammenfassungsbericht für die Sammlung](./media/oracle-to-sql-on-azure-vm-guide/collection-summary-report.png)


1. Wählen Sie **Optionen** aus, um einen Bericht zur Oracle-Bewertung und den Datenbankdetails zu generieren. Wählen Sie beide Optionen (einzeln) aus, um den Bericht zu generieren.


### <a name="assess"></a>Bewerten

Nachdem Sie die Datenquellen ermittelt haben, verwenden Sie [SQL Server Migration Assistant (SSMA) für Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258), um die Oracle-Instanzen zu bewerten, die zur SQL Server-VM migriert werden sollen. Bei diesem Schritt lassen sich mögliche Lücken oder Probleme ermitteln. Mithilfe des Migrations-Assistenten können Sie Datenbankobjekte und Daten überprüfen, Datenbanken für die Migration bewerten und Datenbankobjekte bzw. anschließend Daten zu SQL Server migrieren.

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen: 

1. Öffnen Sie [SQL Server Migration Assistant (SSMA) für Oracle](https://www.microsoft.com/en-us/download/details.aspx?id=54258). 
1. Klicken Sie auf **File** (Datei) und dann auf **New Project** (Neues Projekt). 
1. Geben Sie einen Projektnamen und einen Speicherort für das Projekt an, und wählen Sie dann in der Dropdownliste ein SQL Server-Migrationsziel aus. Klicken Sie auf **OK**. 

   ![Neues Projekt](./media/oracle-to-sql-on-azure-vm-guide/new-project.png)

1. Wählen Sie **Verbindung mit Oracle herstellen** aus. Geben Sie im Dialogfeld **Mit Oracle verbinden** Werte für die Oracle-Verbindungsdetails ein.

   ![Herstellen einer Verbindung mit Oracle](./media/oracle-to-sql-on-azure-vm-guide/connect-to-oracle.png)

   Wählen Sie die Oracle-Schemas aus, die migriert werden sollen: 

   ![Wählen Sie das Oracle-Schema aus](./media/oracle-to-sql-on-azure-vm-guide/select-schema.png)

1. Klicken Sie im **Oracle-Metadaten-Explorer** mit der rechten Maustaste auf das Oracle-Schema, das migriert werden soll, und wählen Sie dann **Create report** (Bericht erstellen) aus. Dadurch wird ein HTML-Bericht generiert. Alternativ können Sie nach der Auswahl der Datenbank in der Navigationsleiste auf **Create report** (Bericht erstellen) klicken.

   ![Erstellen von Berichten](./media/oracle-to-sql-on-azure-vm-guide/create-report.png)

1. Wählen Sie im **Oracle-Metadaten-Explorer** das Oracle-Schema und dann **Create report** (Bericht erstellen) aus, um einen HTML-Bericht mit Konvertierungsstatistiken und Fehlern/Warnungen (sofern vorhanden) zu generieren.
1. Überprüfen Sie den HTML-Bericht auf Konvertierungsstatistiken sowie auf Fehler und Warnungen. Analysieren Sie diese Informationen, um Konvertierungsprobleme und mögliche Lösungen zu bestimmen.

   Auf diesen Bericht kann auch über den SSMA-Projektordner zugegriffen werden (wie im ersten Bildschirm gezeigt). Suchen Sie im obigen Beispiel nach der Datei „report.xml“: 

   `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`

    Sie können den Bericht in Excel öffnen, um ein Inventar der Oracle-Objekte sowie Informationen zum für die Durchführung von Schemakonvertierungen erforderlichen Aufwand zu erhalten.

   ![Konvertierungsbericht](./media/oracle-to-sql-on-azure-vm-guide/conversion-report.png)



### <a name="validate-data-types"></a>Überprüfen von Datentypen

Überprüfen Sie die standardmäßig festgelegten Datentypzuordnungen, und ändern Sie sie bei Bedarf basierend auf den Anforderungen. Gehen Sie dazu folgendermaßen vor: 

1. Klicken Sie im Menü auf **Tools**. 
1. Klicken auf **Project Settings** (Projekteinstellungen). 
1. Klicken Sie auf die Registerkarte **Type Mappings** (Typzuordnungen). 

   ![Typzuordnungen](./media/oracle-to-sql-on-azure-vm-guide/type-mappings.png)

1. Zum Ändern der Typzuordnung für jede Tabelle wählen Sie die Tabelle im **Oracle-Metadaten-Explorer** aus. 



### <a name="convert-schema"></a>Schema konvertieren

Führen Sie die folgenden Schritte aus, um das Schema zu konvertieren: 

1. (Optional) Zum Konvertieren von dynamischen oder Ad-hoc-Abfragen klicken Sie mit der rechten Maustaste auf den Knoten und wählen **Anweisung hinzufügen** aus.
1. Wählen Sie in der oberen Navigationsleiste **Mit SQL Server verbinden** aus, und geben Sie die Verbindungsdetails für Ihre SQL Server-Instanz auf der Azure-VM an. Sie können eine Verbindung mit einer vorhandenen Datenbank herstellen oder einen neuen Namen angeben. In diesem Fall wird auf dem Zielserver eine Datenbank erstellt.

   ![Herstellen einer Verbindung mit SQL](./media/oracle-to-sql-on-azure-vm-guide/connect-to-sql-vm.png)

1. Klicken Sie im **Oracle Metadata Explorer** mit der rechten Maustaste auf das Oracle-Schema, und klicken Sie dann auf **Schema konvertieren**.

   ![Schema konvertieren](./media/oracle-to-sql-on-azure-vm-guide/convert-schema.png)

1. Nachdem das Schema konvertiert wurde, vergleichen und überprüfen Sie die Struktur des Schemas, um potenzielle Probleme zu ermitteln.

   ![Überprüfen der Empfehlungen](./media/oracle-to-sql-on-azure-vm-guide/table-mapping.png)

   Vergleichen Sie den konvertierten Transact-SQL-Text mit den ursprünglichen gespeicherten Prozeduren, und prüfen Sie die Empfehlungen: 

   ![Code zum Überprüfen der Empfehlungen](./media/oracle-to-sql-on-azure-vm-guide/procedure-comparison.png)

   Sie können das Projekt lokal speichern, um offline eine Problembehandlung für das Schema durchzuführen. Wählen Sie dazu im Menü **Datei** die Option **Projekt speichern** aus. So können Sie das Quell- und das Zielschema offline auswerten und Probleme behandeln, bevor Sie das Schema für SQL Server veröffentlichen.


## <a name="migrate"></a>Migrieren

Sobald die Voraussetzungen erfüllt sind, und Sie die Aufgaben **vor der Migration** ausgeführt haben, können Sie die Schema- und Datenmigration durchführen. Die Migration umfasst zwei Schritte: Veröffentlichen des Schemas und Migrieren der Daten. 


Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und die Daten zu migrieren: 

1. Klicken Sie im **SQL Server-Metadaten-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Mit Datenbank synchronisieren** aus. Mit dieser Aktion wird das Oracle-Schema für SQL Server auf Azure-VMs veröffentlicht. 

   ![Synchronisieren mit der Datenbank](./media/oracle-to-sql-on-azure-vm-guide/synchronize-database.png)

   Überprüfen Sie den Synchronisierungsstatus: 

   ![Synchronisierungsstatus überprüfen](./media/oracle-to-sql-on-azure-vm-guide/synchronize-database-review.png)


1. Klicken Sie im **Oracle-Metadaten-Explorer** mit der rechten Maustaste auf das Oracle-Schema, und klicken Sie dann auf **Migrate Data** (Daten migrieren). Alternativ können Sie in der oberen Navigationsleiste die Option zur Datenmigration auswählen.

   ![Migrieren von Daten](./media/oracle-to-sql-on-azure-vm-guide/migrate-data.png)

1. Geben Sie im Dialogfeld die Verbindungsdetails für Oracle und SQL Server auf Azure-VMs ein.
1. Zeigen Sie nach Abschluss der Migration den Bericht zur Datenmigration an:

    ![Bericht zur Datenmigration](./media/oracle-to-sql-on-azure-vm-guide/data-migration-report.png)

1. Stellen Sie über [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) eine Verbindung mit SQL Server auf Azure-VMs her, um die Daten und das Schema auf Ihrer SQL Server-Instanz zu überprüfen. 

   ![Überprüfung in SSMA](./media/oracle-to-sql-on-azure-vm-guide/validate-in-ssms.png)




Zusätzlich zu SSMA können Sie auch SQL Server Integration Services (SSIS) für die Migration der Daten verwenden. Weitere Informationen finden Sie unter: 
- [Erste Schritte mit SQL Server Integration Services](https://docs.microsoft.com//sql/integration-services/sql-server-integration-services).
- [SQL Server Integration Services: SSIS für Azure und Hybriddatenverschiebung](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx).



## <a name="post-migration"></a>Nach der Migration 

Nach dem erfolgreichen Abschluss der **Migrationsphase** müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

Das [Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) ist eine Erweiterung für Visual Studio Code, mit dem Sie Ihren Java-Quellcode analysieren und API-Aufrufe und Abfragen für den Datenzugriff ermitteln können. Mit diesem Toolkit profitieren Sie von einer zentralen Übersicht über Themen oder Probleme, die für das neue Datenbank-Back-End behandelt werden müssen. Weitere Informationen finden Sie im Blog [Migrate our Java application from Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727). 

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln der Validierungstests**. Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.

2. **Einrichten der Testumgebung**. Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.

3. **Ausführen der Validierungstests**. Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.

4. **Ausführen der Leistungstests**. Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

### <a name="optimize"></a>Optimieren

Die Postmigrationsphase ist unerlässlich, um Datengenauigkeitsprobleme abzustimmen, die Vollständigkeit zu bestätigen und Leistungsprobleme der Workload zu beheben.

> [!Note]
> Weitere Einzelheiten zu diesen Problemen sowie die erforderlichen Schritte zur Problembehandlung finden Sie im [Handbuch für die Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung zu diesem Migrationsszenario erhalten Sie in Form der folgenden Ressourcen, die im Rahmen eines Auftrags für ein echtes Migrationsprojekt entwickelt wurden.

| **Titel/Link**                                                                                                                                          | **Beschreibung**                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Data Workload Assessment Model and Tool](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Dieses Tool stellt für eine bestimmte Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch die Bereitstellung eines automatisierten und einheitlichen Zielplattform-Entscheidungsprozesses dabei helfen, umfangreiche Bewertungen zu beschleunigen.                                                          |
| [Oracle Inventory Script Artifacts](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Diese Ressource umfasst eine PL/SQL-Abfrage für Oracle-Systemtabellen, die die Anzahl von Objekten nach Schematyp, Objekttyp und Status bereitstellt. Darüber hinaus erhalten Sie eine grobe Schätzung zu den „Rohdaten“ sowie zur Größe der Tabellen in jedem Schema. Die Ergebnisse werden im CSV-Format gespeichert.                                                                                                               |
| [Automate SSMA Oracle Assessment Collection & Consolidation](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Diese Ressource verwendet eine CSV-Datei als Eingabe („sources.csv“ in den Projektordnern), um die XML-Dateien zu erstellen, die zum Ausführen der SSMA-Bewertung im Konsolenmodus benötigt werden. Die Datei „source.csv“ wird vom Kunden bereitgestellt und basiert auf einer Inventarisierung vorhandener Oracle-Instanzen. Die Ausgabedateien sind „AssessmentReportGeneration_source_1.xml“, „ServersConnectionFile.xml“ und „VariableValueFile.xml“.|
| [SSMA: Probleme und Problemumgehungen bei der Migration von Oracle-Datenbanken](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | Bei Oracle kann eine nicht skalare Bedingung in der WHERE-Klausel zugewiesen werden. Dieser Bedingungstyp wird von SQL Server jedoch nicht unterstützt. Folglich konvertiert SQL Server Migration Assistant (SSMA) für Oracle Abfragen mit nicht skalarer Bedingung in der WHERE-Klausel nicht. Stattdessen wird der Fehler O2SS0001 ausgegeben. In diesem Whitepaper finden Sie weitere Einzelheiten zu diesem Problem sowie die erforderlichen Schritte zur Problembehandlung.          |
| [Handbuch für die Migration von Oracle zu SQL Server](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | In diesem Whitepaper sind die Aufgaben bei der Migration eines Oracle-Schemas zur neuesten Version von SQL Server beschrieben. Wenn für die Migration Änderungen an Features/Funktionen erforderlich sind, müssen Sie die Auswirkungen dieser Änderungen auf die Anwendungen, die die Datenbank verwenden, sorgfältig untersuchen.                                                     |

Diese Ressourcen wurden im Rahmen des Data SQL Ninja-Programms entwickelt, das vom Azure Data Group-Entwicklungsteam gesponsert wird. Der Hauptzweck des Data SQL Ninja-Programms besteht darin, Hindernisse für komplexe Modernisierung zu beseitigen und Letztere zu beschleunigen sowie den Wettbewerb in Bezug auf Möglichkeiten zur Migration von Datenplattformen zur Azure-Datenplattform von Microsoft zu fördern. Wenn Sie denken, dass Ihre Organisation an einer Teilnahme am Data SQL Ninja-Programm interessiert wäre, wenden Sie sich an Ihr Kundenteam, und bitten Sie es, eine Nominierung einzureichen.

## <a name="next-steps"></a>Nächste Schritte

- Die Verfügbarkeit der mit SQL Server kompatiblen Dienste finden Sie auf der Seite für die [globale Azure-Infrastruktur](https://azure.microsoft.com/global-infrastructure/services/?regions=all&amp;products=synapse-analytics,virtual-machines,sql-database).

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios und Spezialaufgaben ausführen können, finden Sie im Artikel [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Hier finden Sie weitere Informationen zu Azure SQL:
   - [Bereitstellungsoptionen](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [SQL Server auf Azure-VMs](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/) 


- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Hier finden Sie weitere Informationen zur Lizenzierung:
   - [Bring-Your-Own-License mit dem Azure-Hybridvorteil](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [Erweiterter Support für SQL Server 2008 und SQL Server 2008 R2](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)


- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit (Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Ausführliche Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Assistent für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).

