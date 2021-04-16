---
title: Migrieren durch Sicherungen und Wiederherstellungen – Azure Database for MariaDB
description: In diesem Artikel werden zwei Möglichkeiten zum Sichern und Wiederherstellen von Datenbanken in Ihrer Instanz von Azure Database for MariaDB beschrieben, indem Tools wie mysqldump, MySQL Workbench und PHPMyAdmin genutzt werden.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.subservice: migration-guide
ms.topic: how-to
ms.date: 2/27/2020
ms.openlocfilehash: 35b1e84bdf74afd9577c7c98f023179dd769cd66
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105628219"
---
# <a name="migrate-your-mariadb-database-to-an-azure-database-for-mariadb-by-using-dump-and-restore"></a>Migrieren der MariaDB-Datenbank auf Azure Database for MariaDB durch Sicherungen und Wiederherstellungen

In diesem Artikel werden zwei allgemeine Verfahren zum Sichern und Wiederherstellen von Datenbanken in Ihrer Instanz von Azure Database for MariaDB beschrieben:
- Sichern und Wiederherstellen mithilfe eines Befehlszeilentools (mithilfe von mysqldump) 
- Sichern und Wiederherstellen mithilfe von phpMyAdmin

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die folgenden Schritte aus, bevor Sie mit der Migration Ihrer Datenbank beginnen:
- Erstellen eines [Azure Database for MariaDB-Servers – Azure-Portal](quickstart-create-mariadb-server-database-using-azure-portal.md).
- Installieren Sie das Befehlszeilenprogramm [mysqldump](https://mariadb.com/kb/en/library/mysqldump/).
- Herunterladen und Installieren von [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) oder eines anderen MySQL-Tools von Drittanbietern zum Ausführen von Sicherungs- und Wiederherstellungsbefehlen.

## <a name="use-common-tools"></a>Verwenden von gängigen Tools
Mithilfe von gängigen Hilfsprogrammen und Tools wie MySQL Workbench oder mysqldump können Sie eine Remoteverbindung zu Azure Database for MariaDB herstellen und Daten darin wiederherstellen. Verwenden Sie diese Tools auf Ihrem Clientcomputer mit Internetverbindung, um eine Verbindung zu Azure Database for MariaDB herzustellen. Verwenden Sie als bewährte Sicherheitsmaßnahme eine SSL-verschlüsselte Verbindung. Weitere Informationen finden Sie unter [Konnektivitätssicherheit in Azure Database for MariaDB](concepts-ssl-connection-security.md). Bei der Migration zu Azure Database for MariaDB müssen Sie Ihre Sicherungsdateien nicht in einen bestimmten Cloudspeicherort verschieben. 

## <a name="common-uses-for-dump-and-restore"></a>Gängige Nutzungsszenarien für die Sicherung von Abbildern und die Wiederherstellung
Sie können MySQL-Hilfsprogramme wie mysqldump und mysqlpump verwenden, um Datenbanken in mehreren häufigen Szenarien zu sichern und in einen Azure Database for MariaDB-Server zu laden. 

- Verwenden Sie Sicherungen von Datenbankabbildern, wenn Sie die gesamte Datenbank migrieren. Diese Empfehlung gilt, wenn Sie eine große Menge von Daten verschieben oder die Dienstunterbrechung für Live-Websites oder -Anwendungen minimieren möchten. 
-  Stellen Sie sicher, dass für alle Tabellen in der Datenbank die InnoDB-Speicher-Engine verwendet wird, wenn Sie Daten in Azure Database for MySQL laden. Azure Database for MariaDB unterstützt nur die InnoDB-Speicher-Engine und keine anderen Speicher-Engines. Wenn Ihre Tabellen mit anderen Speicher-Engines konfiguriert werden, sollten Sie diese in das InnoDB-Engine-Format konvertieren, bevor Sie die Migration zu Azure Database for MariaDB durchführen.
   
   Wenn Sie beispielsweise über eine Wordpress-App oder eine Web-App verfügen, die MyISAM-Tabellen verwendet, konvertieren Sie zunächst diese Tabellen, indem Sie sie in das InnoDB-Format migrieren, bevor Sie Sie in ihrer Azure-Datenbank für MariaDB wiederherstellen. Verwenden Sie die `ENGINE=InnoDB`Klausel, um die Engine festzulegen, mit der eine Tabelle erstellt wird, und übertragen Sie die Daten vor der Migration in die kompatible Tabelle. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Stellen Sie zur Verhinderung von Kompatibilitätsproblemen sicher, dass beim Sichern von Datenbankabbildern auf den Quell- und Zielsystemen die gleiche Version von MariaDB verwendet wird. Wenn Ihr vorhandener MariaDB-Server beispielsweise Version 10.2 hat, sollten Sie zu einer Azure Database for MariaDB-Instanz migrieren, die für die Ausführung von Version 10.2 konfiguriert ist. Der Befehl `mysql_upgrade` funktioniert auf einem Azure Database for MariaDB-Server nicht und wird nicht unterstützt. Wenn Sie ein übergreifendes Upgrade für MariaDB-Versionen durchführen müssen, sollten Sie die niedrigere Version Ihrer Datenbank in Ihrer eigenen Umgebung zuerst in einer höheren Version von MariaDB sichern bzw. dahin exportieren. Sie können `mysql_upgrade` dann ausführen, bevor Sie versuchen, zu ihrer Azure-Datenbank für MariaDB zu migrieren.

## <a name="performance-considerations"></a>Überlegungen zur Leistung
Beachten Sie die folgenden Überlegungen, um die Leistung beim Sichern von großen Datenbanken zu optimieren:
-   Verwenden Sie die `exclude-triggers`-Option in mysqldump. Schließen Sie Trigger aus Abbilddateien aus, um zu verhindern, dass die Triggerbefehle während der Wiederherstellung der Daten ausgelöst werden. 
-   Verwenden Sie die Option `single-transaction`, um den Transaktionsisolationsmodus auf REPEATABLE READ festzulegen und um die SQL-Anweisung START TRANSACTION an den Server zu senden, bevor die Daten gesichert werden. Durch das Sichern vieler Tabellen in einer Transaktion wird bei der Wiederherstellung zusätzlicher Speicher benötigt. Diese Option `single-transaction` und die Option `lock-tables` schließen sich gegenseitig aus. Das liegt daran, dass LOCK TABLES dazu führt, dass alle ausstehenden Transaktionen implizit committet werden. Um große Tabellen zu sichern, kombinieren Sie die Optionen `single-transaction` und `quick`. 
-   Verwenden Sie die Syntax für mehrere Zeilen `extended-insert`, die mehrere VALUE-Listen enthält. Dies führt zu einer kleineren Speicherabbilddatei und beschleunigt Einfügevorgänge beim erneuten Laden der Datei.
-  Verwenden Sie in mysqldump die Option `order-by-primary`, wenn Sie Datenbanken sichern, damit die Skripterstellung für die Daten in der Primärschlüssel-Reihenfolge erfolgt.
-   Verwenden Sie in mysqldump beim Erstellen von Sicherungsabbildern für Daten die Option `disable-keys`, um vor dem Laden Fremdschlüsseleinschränkungen zu deaktivieren. Durch das Deaktivieren von Fremdschlüsselüberprüfungen wird die Leistung verbessert. Aktivieren Sie die Einschränkungen, und überprüfen Sie die Daten nach dem Laden, um die referentielle Integrität sicherzustellen.
-   Verwenden Sie ggf. partitionierte Tabellen.
-   Laden Sie Daten parallel. Vermeiden Sie zu viel Parallelität, da dies zum Erreichen eines Ressourcenlimits führen kann, und überwachen Sie die Ressourcen mit den Metriken, die im Azure-Portal verfügbar sind. 
-   Verwenden Sie in mysqlpump die Option `defer-table-indexes`, wenn Sie Sicherungsabbilder von Datenbanken erstellen, damit die Indexerstellung nach dem Laden der Tabellendaten erfolgt.
-   Kopieren Sie die Sicherungsdateien in einen Azure-BLOB-Speicher, und führen Sie die Wiederherstellung von dort aus. Diese Vorgehensweise sollte viel schneller sein als die Wiederherstellung über das Internet.

## <a name="create-a-backup-file"></a>Erstellen einer Sicherungsdatei

Führen Sie den Befehl „mysqldump“ aus, um eine vorhandene MariaDB-Datenbank auf dem lokalen Server oder einem virtuellen Computer zu sichern: 

```bash
$ mysqldump --opt -u <uname> -p<pass> <dbname> > <backupfile.sql>
```

Es müssen folgende Parameter bereitgestellt werden:
- *\<uname>* : Ihr Datenbank-Benutzername 
- *\<pass>* : Das Kennwort für Ihre Datenbank (beachten Sie, dass kein Leerzeichen zwischen „-p“ und dem Kennwort verwendet wird) 
- *\<dbname>* : Der Name Ihrer Datenbank. 
- *\<backupfile.sql>* : Der Dateiname der Datenbanksicherung. 
- *\<--opt>* : Die mysqldump-Option 

Um eine Datenbank namens *„testdb“* mit dem Benutzernamen *„testuser“* auf Ihrem MariaDB-Server ohne Kennwort als Datei mit dem Namen „testdb_backup.sql“ zu sichern, verwenden Sie den folgenden Befehl. Mit dem Befehl wird die Datenbank `testdb` in einer Datei mit dem Namen `testdb_backup.sql` gesichert, die alle SQL-Anweisungen enthält, die zum erneuten Erstellen der Datenbank erforderlich sind. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Um zur Sicherung bestimmte Tabellen in der Datenbank auszuwählen, listen Sie die Tabellennamen durch Leerzeichen getrennt auf. Um beispielsweise nur die Tabellen „table1“ und „table2“ von *„testdb“* zu sichern, führen Sie folgendes Beispiel durch: 

```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

Um mehrere Datenbanken gleichzeitig zu sichern, verwenden Sie den Schalter „--database“ und listen Sie die Datenbanknamen durch Leerzeichen getrennt auf. 

```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```

## <a name="create-a-database-on-the-target-server"></a>Erstellen einer Datenbank auf dem Zielserver
Erstellen Sie eine leere Datenbank auf dem Azure Database for MariaDB-Zielserver, zu dem Sie die Daten migrieren möchten. Verwenden Sie ein Tool wie MySQL Workbench zum Erstellen der Datenbank. Die Datenbank kann denselben Namen aufweisen wie die Datenbank, die die Sicherungsdaten enthält, Sie können aber auch eine Datenbank mit einem anderen Namen erstellen.

Ermitteln Sie zum Herstellen der Verbindung die Verbindungsinformationen in der **Übersicht** Ihrer Azure Database for MariaDB-Instanz.

![Screenshot des Übersichtsbereichs für einen Azure Database for MariaDB-Server in der Azure-Portal.](./media/howto-migrate-dump-restore/1_server-overview-name-login.png)

Fügen Sie die Verbindungsinformationen in MySQL Workbench hinzu.

![Screenshot des Bereichs „MySQL-Verbindungen“ in MySQL Workbench.](./media/howto-migrate-dump-restore/2_setup-new-connection.png)

## <a name="restore-your-mariadb-database"></a>Wiederherstellen der MariaDB-Datenbank
Nachdem Sie die Zieldatenbank erstellt haben, können Sie mithilfe des Befehls „mysql“ oder von MySQL Workbench Daten in der jeweiligen neu erstellten Datenbank anhand der Sicherungsdatei wiederherstellen.

```bash
mysql -h <hostname> -u <uname> -p<pass> <db_to_restore> < <backupfile.sql>
```

In diesem Beispiel stellen Sie die Daten in der neu erstellten Datenbank auf dem Azure Database for MariaDB-Server wieder her.

```bash
$ mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-your-mariadb-database-by-using-phpmyadmin"></a>So exportieren Sie Ihre MariaDB-Datenbank mithilfe von phpMyAdmin

Für den Export können Sie das allgemeine Tool phpMyAdmin verwenden, das Sie eventuell bereits lokal in Ihrer Umgebung installiert haben. So exportieren Sie Ihre MariaDB-Datenbank mithilfe von phpMyAdmin:
1. Öffnen Sie phpMyAdmin.
1. Wählen Sie im linken Bereich Ihre Datenbank aus, und wählen Sie dann den Link **Export** aus. Eine neue Seite mit dem Speicherabbild der Datenbank wird angezeigt.
1. Klicken Sie im Bereich **Export** auf den Link **Alle auswählen**, um die Tabellen in Ihrer Datenbank auszuwählen. 
1. Klicken Sie im Bereich **SQL-Optionen** auf die gewünschten Optionen. 
1. Klicken Sie auf die Option **Als Datei speichern** und die entsprechende Komprimierungsoption. Klicken Sie dann auf die Schaltfläche **Gehe zu**. Speichern Sie die Datei an der Eingabeaufforderung lokal.

## <a name="import-your-database-by-using-phpmyadmin"></a>Importieren der Datenbank mithilfe von phpMyAdmin

Der Importvorgang ähnelt dem Exportprozess. Führen Sie folgende Schritte aus:
1. Öffnen Sie phpMyAdmin. 
1. Klicken Sie auf der phpMyAdmin-Setupseite auf **Hinzufügen**, um Ihre Azure Database for MariaDB-Serverinstanz hinzuzufügen. 
1. Geben Sie die Verbindungsdetails und Anmeldeinformationen an.
1. Erstellen Sie eine passend benannte Datenbank, und wählen Sie sie links auf dem Bildschirm aus. Klicken Sie zum erneuten Generieren der vorhandenen Datenbank auf den Datenbanknamen, aktivieren Sie alle Kontrollkästchen neben den Tabellennamen, und wählen Sie **Drop** (Verwerfen), um die vorhandenen Tabellen zu löschen. 
1. Klicken Sie auf den Link **SQL**, um die Seite anzuzeigen, auf der Sie SQL-Befehle eingeben oder Ihre SQL-Datei hochladen können. 
1. Verwenden Sie die Schaltfläche **Durchsuchen**, um nach der Datenbankdatei zu suchen. 
1. Klicken Sie auf die Schaltfläche **Go** (Los), um die Sicherung zu exportieren, die SQL-Befehle auszuführen und die Datenbank neu zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
- [Herstellen einer Verbindung zwischen Anwendungen und Azure Database for MariaDB](./howto-connection-string.md).
