---
title: Migrieren von Oracle
titleSuffix: Azure Database for PostgreSQL
description: In dieser Anleitung erfahren Sie, wie Sie Ihr Oracle-Schema zu Azure Database for PostgreSQL migrieren.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.subservice: migration-guide
ms.topic: how-to
ms.date: 03/18/2021
ms.openlocfilehash: ec6cf87b3fd326c905b4843dc30ae6ce15379305
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608541"
---
# <a name="migrate-oracle-to-azure-database-for-postgresql"></a>Onlinemigration von Oracle zu Azure Database for PostgreSQL

In dieser Anleitung erfahren Sie, wie Sie Ihr Oracle-Schema zu Azure Database for PostgreSQL migrieren. 

Ausführliche und umfassende Anleitungen für die Migration finden Sie in den [Ressourcen des Migrationshandbuchs](https://github.com/microsoft/OrcasNinjaTeam/blob/master/Oracle%20to%20PostgreSQL%20Migration%20Guide/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Guide.pdf). 

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihr Oracle-Schema zu Azure Database for PostgreSQL migrieren möchten, müssen Sie folgende Schritte ausführen: 

- Überprüfen, ob Ihre Quellumgebung unterstützt wird. 
- Laden Sie die neueste Version von [ora2pg](https://ora2pg.darold.net/) herunter. 
- Die aktuellste Version des [DBD Moduls](https://www.cpan.org/modules/by-module/DBD/). 


## <a name="overview"></a>Übersicht

PostgreSQL ist eine der fortschrittlichsten Open Source-Datenbanken der Welt. In diesem Artikel wird beschrieben, wie Sie mit dem kostenlosen ora2pg-Hilfsprogramm eine Oracle-Datenbank zu PostgreSQL migrieren. Sie können ora2pg, ein kostenloses Tool, verwenden, um eine Oracle- oder MySQL-Datenbank zu einem PostgreSQL-kompatiblen Schema zu migrieren. Das Hilfsprogramm stellt eine Verbindung mit Ihrer Oracle-Datenbank her, scannt sie automatisch und extrahiert ihre Struktur bzw. Ihre Daten. Anschließend generiert ora2pg SQL-Skripts, die Sie in Ihre PostgreSQL-Datenbank laden können. ora2pg kann für Aufgaben verwendet werden, die vom Reverse Engineering einer Oracle-Datenbank, über die Migration einer großen Unternehmensdatenbank bis hin zur einfachen Replikation einiger Oracle-Daten in eine PostgreSQL-Datenbank reichen. Es ist einfach zu verwenden und erfordert keine Oracle-Datenbankkenntnisse, sondern lediglich die Bereitstellung der für die Verbindung zur Oracle-Datenbank erforderlichen Parameter.

> [!NOTE]
> Weitere Informationen zur Verwendung der neuesten Version von ora2pg finden Sie in der [ora2pg-Dokumentation](https://ora2pg.darold.net/documentation.html).

### <a name="typical-ora2pg-migration-architecture"></a>Typische ora2pg-Migrationsarchitektur

![Screenshot der or2pg-Migrationsarchitektur](media/howto-migrate-from-oracle/ora2pg-migration-architecture.png)

Nach der Bereitstellung der VM und der Azure Database for PostgreSQL sind zwei Konfigurationen erforderlich, um die Konnektivität zwischen beiden zu aktivieren: **Zulassen von Azure Services**  und **Erzwingen der SSL-Verbindung**, die wie folgt dargestellt werden:

- **Connection Security**  Blade -> **Zugriff auf Azure Services zulassen**  -> EIN

- **Connection Security**  Blade -> **SSL-Einstellungen** ->  **SSL-Verbindung erzwingen** -> DEAKTIVIERT

### <a name="recommendations"></a>Empfehlungen

- Sammeln Sie Statistiken wie folgt, um die Leistung der Bewertungs-oder Export Vorgänge auf dem Oracle-Server zu verbessern:

   ```
   BEGIN
   
     DBMS_STATS.GATHER_SCHEMA_STATS
     DBMS_STATS.GATHER_DATABASE_STATS
     DBMS_STATS.GATHER_DICTIONARY_STATS
     END;
   ```

- Exportieren Sie Daten mit dem Kopierbefehl anstelle von EINFÜGEN.

- Vermeiden Sie es, Tabellen mit ihren FKs, Constraints und Indexen zu exportieren – dadurch wird der Import der Daten in PostgreSQL langsamer.

- Erstellen Sie materialisierte Sichten mithilfe der **keine Daten Klausel** und aktualisieren sie Sie später.

- Implementieren Sie, wenn möglich, eindeutige Indizes in materialisierten Sichten. Dadurch wird die Aktualisierung mit der Syntax beschleunigt `REFRESH MATERIALIZED VIEW CONCURRENTLY` .



## <a name="pre-migration"></a>Vor der Migration 

Nachdem Sie sich vergewissert haben, dass Ihre Quellumgebung unterstützt wird, und sichergestellt haben, dass Sie alle Voraussetzungen erfüllen, können Sie die Prä-Migrationsphase starten. Diese Schritte umfassen eine Bestandsaufnahme der Datenbanken, die migriert werden müssen, eine Untersuchung dieser Datenbanken auf potenzielle Migrationsprobleme oder Hindernisse sowie das Behandeln eventuell entdeckter Probleme. Bei heterogenen Migrationen, wie z. B. Oracle zu Azure Database for PostgreSQL, umfasst diese Phase auch das umrechnen der Schema(s) in den Quelldatenbanken, damit diese mit der Zielumgebung kompatibel sind.

### <a name="discover"></a>Entdecken

Ziel der Entdeckungsphase ist es, bestehende Datenquellen und Details zu den verwendeten Funktionen zu identifizieren, um ein besseres Verständnis für die Migration zu erhalten und diese zu planen. Bei diesem Vorgang ermitteln Sie u. a. alle Oracle-Instanzen Ihrer Organisation, die innerhalb des Netzwerks ausgeführt werden, sowie die jeweilige Version und die verwendeten Features.

Die Microsoft Oracle Pre-Assessment-Skripte werden gegen die Oracle-Datenbank ausgeführt. Bei den Pre-Assessment-Skripten handelt es sich um eine Reihe von Abfragen, die auf die Oracle-Metadaten zugreifen und Folgendes liefern:

- Datenbank Inventur, einschließlich der Anzahl von Objekten nach Schema, Typ und Status.

- Eine grobe Schätzung von Rohdaten in jedem Schema (basierend auf Statistiken).

- Größenanpassung von Tabellen in jedem Schema.

- Die Anzahl der Codezeilen pro Paket, Funktion, Prozedur usw.

Laden Sie die zugehörigen Skripts von der [ora2pg-Website](http://ora2pg.darold.net/)herunter.

### <a name="assess"></a>Bewerten

Nachdem Sie die Inventur der Oracle-Datenbank(en) abgeschlossen haben, um eine Vorstellung von der Größe der Datenbank und den Herausforderungen zu bekommen, folgt als nächster Schritt die Durchführung des Assessments.

Das Einschätzen der Kosten für einen Migrationsprozess von Oracle auf PostgreSQL ist nicht einfach. Um eine gute Einschätzung dieser Migrationskosten zu erhalten, überprüft ora2pg alle Datenbankobjekte, alle Funktionen und gespeicherten Prozeduren, um festzustellen, ob es noch Objekte und PL/SQL-Codes gibt, die nicht automatisch von ora2pg konvertiert werden können.

ora2pg verfügt über einen Inhaltsanalysemodus, der die Oracle-Datenbank überprüft und einen Textbericht darüber erstellt, was  in der Oracle-Datenbank enthalten ist und was nicht exportiert werden kann.

Um den **Analyse-und Berichtsmodus** zu aktivieren, verwenden Sie den exportierten Typ, `SHOW_REPORT` der im folgenden Befehl gezeigt wird:

```
ora2pg -t SHOW_REPORT
```

Nachdem die Datenbank analysiert wurde, kann ora2pg mit seiner Fähigkeit, den SQL- und PL/SQL-Code von der Oracle-Syntax nach PostgreSQL zu konvertieren, einen Schritt weiter gehen, indem es die Code-Schwierigkeiten und die Zeit, die für eine vollständige Datenbankmigration erforderlich ist, abschätzt.

Um die Migrationskosten in Arbeitstagen abzuschätzen, bietet ora2pg die Möglichkeit der Verwendung einer Konfigurationsrichtlinie mit der Bezeichnung ESTIMATE_COST, die auch über die Befehlszeile aktiviert werden kann:

```
ora2pg -t SHOW_REPORT --estimate_cost
```

Die Standard-Migrationseinheit entspricht etwa fünf Minuten für einen PostgreSQL-Experten. Wenn dies Ihre erste Migration ist, können Sie den Wert mit der Konfigurationsanweisung COST_UNIT_VALUE oder der Befehlszeilenoption cost_unit_value erhöhen.

In der letzten Zeile des Berichts wird der gesamte geschätzte Migrationscode in Arbeitstagen angezeigt, gefolgt von der Anzahl der geschätzten Migrationseinheiten für jedes Objekt.

Diese Migrationseinheit entspricht etwa fünf Minuten für einen PostgreSQL-Experten. Wenn dies Ihre erste Migration ist, können Sie den Wert mit der Konfigurationsanweisung COST_UNIT_VALUE oder der Befehlszeilenoption cost_unit_value erhöhen. Im Folgenden finden Sie einige Varianten der Bewertung a) Tabellenbewertung; b) Spaltenbewertung c) Schema-Bewertung mit Standard-Kosteneinheit (5 min) d) Schema-Bewertung mit 10 min als Kosteneinheit.

```
ora2pg -t SHOW_TABLE -c c:\ora2pg\ora2pg_hr.conf > c:\ts303\hr_migration\reports\tables.txt ora2pg -t SHOW_COLUMN -c c:\ora2pg\ora2pg_hr.conf > c:\ts303\hr_migration\reports\columns.txt
ora2pg -t SHOW_REPORT -c c:\ora2pg\ora2pg_hr.conf --dump_as_html --estimate_cost > c:\ts303\hr
_migration\reports\report.html
ora2pg -t SHOW_REPORT -c c:\ora2pg\ora2pg_hr.conf –-cost_unit_value 10 --dump_as_html --estimate_cost > c:\ts303\hr_migration\reports\report2.html
```

Die Ausgabe der Schemabewertung ist wie folgt dargestellt:

**Migrations Ebene: B-5**

Migrationebenen:

A-Migration, die möglicherweise automatisch ausgeführt wird

B - Migration mit Code-Rewrite und einem Zeitaufwand von bis zu 5 Tagen

C - Migration mit Code-Rewrite und einem Zeitaufwand von über 5 Tagen

Technische Ebenen:

1 = trivial: keine gespeicherten Funktionen und keine Trigger

2 = easy: keine gespeicherten Funktionen, jedoch mit Triggern, kein manuelles Umschreiben

3 = einfach: gespeicherte Funktionen und/oder Trigger, kein manuelles Umschreiben

4 = manuell: keine gespeicherten Funktionen, aber mit Triggern oder Ansichten mit Codeumschreibung

5 = schwierig: gespeicherte Funktionen und/oder Trigger mit Codeumschreibung

Die Bewertung besteht aus einem Buchstaben (A oder B), um anzugeben, ob die Migration manuell neu geschrieben werden muss oder nicht, und einer Zahl von 1 bis 5, um den technischen Schwierigkeitsgrad anzugeben. Mit der zusätzlichen Option -human_days_limit können Sie die Anzahl der Personentage festlegen, bei denen die Migrationsstufe auf C gesetzt werden soll, um anzuzeigen, dass ein großer Arbeitsaufwand und ein vollständiges Projektmanagement mit Migrationsunterstützung erforderlich sind. Der Standardwert beträgt 10 Personentage. Sie können die Konfigurationsdirektive HUMAN_DAYS_LIMIT verwenden, um diesen Standardwert dauerhaft zu ändern.

Diese Funktion wurde entwickelt, um die Entscheidung zu erleichtern, welche Datenbank zuerst migriert werden kann und welches Team mobilisiert werden muss.

### <a name="convert"></a>Convert

Bei Migrationen mit minimaler Ausfallzeit ändert sich die Quelle, die Sie migrieren, auch nach der einmaligen Migration weiter und weicht in Bezug auf Daten und Schema vom Ziel ab. Während der **Datensynchronisierungsphase** müssen Sie sicherstellen, dass alle Änderungen in der Quelle erfasst und nahezu in Echtzeit auf das Ziel angewendet werden. Nachdem Sie überprüft haben, dass alle Änderungen in der Quelle auf das Ziel übertragen wurden, können Sie von der Quell- auf die Zielumgebung umschalten.

In diesem Schritt der Migration erfolgt die Konvertierung bzw. Übersetzung des Oracle Codes + DDLS nach PostgreSQL. Das ora2pg Tool exportiert die Oracle-Objekte automatisch in ein PostgreSQL-Format. Bei diesen generierten Objekten werden einige nicht ohne manuelle Änderungen in der PostgreSQL-Datenbank kompiliert.  
Der Prozess, herauszufinden, welche Elemente einen manuellen Eingriff erfordern, besteht darin, die von ora2pg generierten Dateien gegen die PostgreSQL-Datenbank zu vergleichen, das Protokoll zu überprüfen und die notwendigen Änderungen vorzunehmen, bis die gesamte Schemastruktur mit der PostgreSQL-Syntax kompatibel ist.


#### <a name="create-migration-template"></a>Migrationsvorlage erstellen 

Zunächst empfiehlt es sich, die Migrationsvorlage zu erstellen, die mit ora2pg mitgeliefert wird. Die beiden Optionen --project_base und --init_project geben ora2pg an, dass es eine Projektvorlage mit einem Arbeitsbaum, einer Konfigurationsdatei und einem Skript zum Exportieren aller Objekte aus der Oracle-Datenbank erstellen soll. Weitere Informationen finden Sie in der [ora2pg Dokumentation](https://ora2pg.darold.net/documentation.html).

   Verwenden Sie den folgenden Befehl: 

   ```
   ora2pg --project_base /app/migration/ --init_project test_project
   
   ora2pg --project_base /app/migration/ --init_project test_project
   ```

Beispielausgabe: 
   
   ```
   Creating project test_project. /app/migration/test_project/ schema/ dblinks/ directories/ functions/ grants/ mviews/ packages/ partitions/ procedures/ sequences/ synonyms/    tables/ tablespaces/ triggers/ types/ views/ sources/ functions/ mviews/ packages/ partitions/ procedures/ triggers/ types/ views/ data/ config/ reports/
   
   Generating generic configuration file
   
   Creating script export_schema.sh to automate all exports.
   
   Creating script import_all.sh to automate all imports.
   ```

Das Verzeichnis sources/ enthält den Oracle-Code, das Verzeichnis schema/ enthält den auf PostgreSQL portierten Code. Das Verzeichnis reports/ enthält die html-Berichte mit der Bewertung der Migrationskosten.


Nachdem die Projektstruktur erstellt wurde, wird eine generische Konfigurationsdatei erstellt. Definieren Sie die Oracle-Datenbankverbindung sowie die entsprechenden Konfigurationsparameter in der Config.  In der Dokumentation zu ora2pg erfahren Sie, was und wie in der Konfigurationsdatei konfiguriert werden kann.


#### <a name="export-oracle-objects"></a>Konvertieren von Oracle-Objekten

Als nächstes exportieren Sie die Oracle-Objekte als PostgreSQL-Objekte, indem Sie die Datei export_schema.sh ausführen.

   ```
   cd /app/migration/mig_project
   ./export_schema.sh
   
   Run the following command manually:
   
   SET namespace="/app/migration/mig_project"
   
   ora2pg -t DBLINK -p -o dblink.sql -b %namespace%/schema/dblinks -c
   %namespace%/config/ora2pg.conf
   ora2pg -t DIRECTORY -p -o directory.sql -b %namespace%/schema/directories -c
   %namespace%/config/ora2pg.conf
   ora2pg -p -t FUNCTION -o functions2.sql -b %namespace%/schema/functions -c
   %namespace%/config/ora2pg.conf ora2pg -t GRANT -o grants.sql -b %namespace%/schema/grants -c %namespace%/config/ora2pg.conf ora2pg -t MVIEW -o mview.sql -b %namespace%/schema/   mviews -c %namespace%/config/ora2pg.conf
   ora2pg -p -t PACKAGE -o packages.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/packages -c
   ora2pg -p -t PARTITION -o partitions.sql %namespace%/config/ora2pg.conf -b %namespace%/schema/partitions -c
   ora2pg -p -t PROCEDURE -o procs.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/procedures -c
   ora2pg -t SEQUENCE -o sequences.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/sequences -c
   ora2pg -p -t SYNONYM -o synonym.sql -b %namespace%/schema/synonyms -c
   %namespace%/config/ora2pg.conf
   ora2pg -t TABLE -o table.sql -b %namespace%/schema/tables -c %namespace%/config/ora2pg.conf ora2pg -t TABLESPACE -o tablespaces.sql -b %namespace%/schema/tablespaces -c
   %namespace%/config/ora2pg.conf
   ora2pg -p -t TRIGGER -o triggers.sql -b %namespace%/schema/triggers -c
   %namespace%/config/ora2pg.conf ora2pg -p -t TYPE -o types.sql -b %namespace%/schema/types -c %namespace%/config/ora2pg.conf ora2pg -p -t VIEW -o views.sql -b %namespace%/   schema/views -c %namespace%/config/ora2pg.conf
   ```

   Um die Daten zu extrahieren, verwenden Sie den folgenden Befehl:

   ```
   ora2pg -t COPY -o data.sql -b %namespace/data -c %namespace/config/ora2pg.conf
   ```

#### <a name="compile-files"></a>Dateien kompilieren

Zum Schluss kompilieren Sie alle Dateien gegen Azure Database for PostgreSQL Server. Sie können nun wählen, ob Sie die generierten DDL-Dateien manuell laden oder das zweite Skript import_all.sh verwenden, um diese Dateien interaktiv zu importieren.

   ```
   psql -f %namespace%\schema\sequences\sequence.sql -h server1-
   
   server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l
   
   %namespace%\ schema\sequences\create_sequences.log
   
   psql -f %namespace%\schema\tables\table.sql -h server1-server.postgres.database.azure.com p 5432 -U username@server1-server -d database -l    %namespace%\schema\tables\create_table.log
   ```

   Befehl zum Datenimport:

   ```
   psql -f %namespace%\data\table1.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l %namespace%\data\table1.log
   
   psql -f %namespace%\data\table2.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l %namespace%\data\table2.log
   ```

Prüfen Sie während der Kompilierung der Dateien die Protokolle und korrigieren Sie die notwendigen Syntaxen, die ora2pg nicht auf Anhieb konvertieren konnte.

Im Whitepaper [Oracle to Azure Database for PostgreSQL Migration Workarounds](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Workarounds.pdf) finden Sie Unterstützung bei der Behebung von Problemen.

## <a name="migrate"></a>Migrieren 

Sobald die Voraussetzungen erfüllt sind, und Sie die Aufgaben **vor der Migration** ausgeführt haben, können Sie die Schema- und Datenmigration durchführen.

### <a name="migrate-schema-and-data"></a>Migrieren von Schemas und Daten

Nachdem die Fixes durchgeführt wurden, steht ein stabiler Build der Datenbank für die Bereitstellung bereit.

An diesem Punkt müssen nur noch die *psql* Import-Befehle ausgeführt werden, die auf die Dateien verweisen, die den geänderten Code enthalten, um die Datenbankobjekte gegen die PostgreSQL-Datenbank zu kompilieren und die Daten zu importieren.

Bei diesem Schritt kann ein gewisses Maß an Parallelität beim Importieren der Daten implementiert werden.

### <a name="data-sync-and-cutover"></a>Datensynchronisierung und Übernahme

Bei Migrationen mit minimaler Ausfallzeit ändert sich die Quelle, die Sie migrieren, auch nach der einmaligen Migration weiter und weicht in Bezug auf Daten und Schema vom Ziel ab. Während der **Datensynchronisierungsphase** müssen Sie sicherstellen, dass alle Änderungen in der Quelle erfasst und nahezu in Echtzeit auf das Ziel angewendet werden. Nachdem Sie überprüft haben, dass alle Änderungen in der Quelle auf das Ziel übertragen wurden, können Sie von der Quell- auf die Zielumgebung umschalten.

Wenn Sie ab März 2019 eine Online-Migration durchführen möchten, sollten Sie Attunity Replicate for Microsoft Migrations oder Striim verwenden.

Bei der *delta/incremental* Migration mit ora2pg besteht die Technik darin, für jede Tabelle eine Abfrage anzuwenden, die einen Filter (Schnitt) nach Datum oder Zeit usw. anwendet, und danach die Migration mit einer zweiten Abfrage abzuschließen, die den Rest der Daten (Reste) migriert.

Migrieren Sie in der Quelldatentabelle zunächst alle historischen Daten. Ein Beispiel hierfür ist:

```
select * from table1 where filter_data < 01/01/2019
```

Sie können die seit der ersten Migration vorgenommenen Änderungen abfragen, indem Sie einen Befehl ähnlich dem folgenden ausführen:

```
select * from table1 where filter_data >= 01/01/2019
```

In diesem Fall wird empfohlen, die Validierung durch die Überprüfung der Datenparität auf beiden Seiten, also Quelle und Ziel, zu erweitern.

## <a name="post-migration"></a>Nach der Migration 

Nach dem erfolgreichen Abschluss der **Migrationsphase** müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Nachdem die Daten in das Zielsystem migriert wurden, führen Sie Tests mit den Datenbanken durch, um zu überprüfen, ob die Anwendungen nach der Migration gut mit dem Zielsystem funktionieren.

Um sicherzustellen, dass die Quelle und das Ziel ordnungsgemäß migriert werden, führen Sie die manuellen Datenvalidierungsskripte gegen die Oracle-Quell- und PostgreSQL-Zieldatenbank aus.

Wenn die Quell- und Zieldatenbank einen Netzwerkpfad haben, sollte idealerweise ora2pg für die Datenvalidierung verwendet werden. Mit der Aktion TEST können Sie überprüfen, ob alle Objekte aus der Oracle-Datenbank unter PostgreSQL angelegt wurden. Führen Sie den Befehl wie gezeigt aus:

```
ora2pg -t TEST -c config/ora2pg.conf > migration_diff.txt
```

### <a name="optimize"></a>Optimieren

Die Postmigrationsphase ist unerlässlich, um Datengenauigkeitsprobleme abzustimmen, die Vollständigkeit zu bestätigen und Leistungsprobleme der Workload zu beheben.

## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung zu diesem Migrationsszenario erhalten Sie in Form der folgenden Ressourcen, die im Rahmen eines Auftrags für ein echtes Migrationsprojekt entwickelt wurden.

| **Titellink** | **Beschreibung**    |
| -------------- | ------------------ |
| [Cookbook für die Migration von Oracle zu Azure PostgreSQL](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf) | Dieses Dokument soll Architects, Consultants, DBAs und ähnlichen Berufsgruppen einen Leitfaden für die schnelle Migration von Workloads von Oracle zu Azure Database for PostgreSQL mit dem ora2pg-Tool an die Hand geben. |
| [Workarounds für die Migration von Oracle zu Azure PostgreSQL](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Workarounds.pdf) | Dieses Dokument soll Architects, Consultants, DBAs und ähnlichen Berufsgruppen einen Leitfaden zur schnellen Behebung / Umgehung von Problemen bei der Migration von Workloads von Oracle zu Azure Database for PostgreSQL an die Hand geben. |
| [Schritte zum Installieren von ora2pg unter Windows oder Linux](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows%20and%20Linux.pdf)                       | Dieses Dokument ist als Schnellinstallationsanleitung für die Migration von Schema und Daten von Oracle zu Azure Database for PostgreSQL mit dem Tool ora2pg unter Windows oder Linux gedacht. Ausführliche Informationen zum Tool finden Sie unter http://ora2pg.darold.net/documentation.html . |

Diese Ressourcen wurden im Rahmen des Data SQL Ninja-Programms entwickelt, das vom Azure Data Group-Entwicklungsteam gesponsert wird. Der Hauptzweck des Data SQL Ninja-Programms besteht darin, Hindernisse für komplexe Modernisierung zu beseitigen und Letztere zu beschleunigen sowie den Wettbewerb in Bezug auf Möglichkeiten zur Migration von Datenplattformen zur Azure-Datenplattform von Microsoft zu fördern. Wenn Sie denken, dass Ihre Organisation an einer Teilnahme am Data SQL Ninja-Programm interessiert wäre, wenden Sie sich an Ihr Kundenteam, und bitten Sie es, eine Nominierung einzureichen.


### <a name="contact-support"></a>Support kontaktieren

Wenn Sie Unterstützung bei Ihren Migrationen über das ora2pg-Tooling hinaus benötigen, wenden Sie sich an den [@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) Alias, um Informationen zu anderen Migrationsoptionen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios (und Spezialaufgaben) ausführen können, finden Sie im Artikel [Dienste und Tools für die Datenmigration](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

Weitere Informationen finden Sie unter: 
- [Dokumentation zu Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/)
- [ora2pg-Dokumentation](https://ora2pg.darold.net/documentation.html)
- [PostgreSQL-Website](https://www.postgresql.org/)
- [Unterstützung für autonome Transaktionen in PostgreSQL](http://blog.dalibo.com/2016/08/19/Autonoumous_transactions_support_in_PostgreSQL.html) 

Für Videoinhalte: 
- [Übersicht über die Migrations Journey und die Tools/Dienste, die für die Durchführung von Bewertung und Migration empfohlen werden.](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)