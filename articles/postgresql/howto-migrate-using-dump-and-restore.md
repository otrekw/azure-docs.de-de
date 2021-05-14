---
title: Sicherung und Wiederherstellung – Azure Database for PostgreSQL – Einzelserver
description: Sie können eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren. Anschließend können Sie eine Wiederherstellung auf Basis einer Datei durchführen, die von pg_dump in Azure Database for PostgreSQL (Einzelserver) erstellt wurde.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.subservice: migration-guide
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 809ff06fe460a06a689d7bbc11cdbd5ee247f585
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450054"
---
# <a name="migrate-your-postgresql-database-by-using-dump-and-restore"></a>Migrieren Ihrer PostgreSQL-Datenbank durch Sichern und Wiederherstellen
[!INCLUDE[applies-to-postgres-single-flexible-server](includes/applies-to-postgres-single-flexible-server.md)]

Sie können mit [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) eine PostgreSQL-Datenbank in eine Sicherungsdatei extrahieren. Stellen Sie dann mit [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) die PostgreSQL-Datenbank aus einer von `pg_dump` erstellten Archivdatei wieder her.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md) mit Firewallregeln, um den Zugriff zu ermöglichen.
- Die Befehlszeilenprogramme [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) müssen installiert sein.

## <a name="create-a-dump-file-that-contains-the-data-to-be-loaded"></a>Erstellen einer Sicherungsdatei, die die zu ladenden Daten enthält

Um eine vorhandene PostgreSQL-Datenbank lokal oder auf einem virtuellen Computer zu sichern, führen Sie folgenden Befehl aus:

```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> -f <database>.dump
```
Wenn Sie z. B. einen lokalen Server mit einer Datenbank namens **testdb** verwenden, führen Sie Folgendes aus:

```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb -f testdb.dump
```

## <a name="restore-the-data-into-the-target-database"></a>Wiederherstellen der Daten in der Zieldatenbank

Nachdem Sie die Zieldatenbank erstellt haben, können Sie den Befehl `pg_restore` und den `--dbname`-Parameter verwenden, um die Daten in der Zieldatenbank anhand der Sicherungsdatei wiederherzustellen.

```bash
pg_restore -v --no-owner --host=<server name> --port=<port> --username=<user-name> --dbname=<target database name> <database>.dump
```

Das Einschließen des Parameters `--no-owner` bewirkt, dass sich alle Objekte, die während der Wiederherstellung erstellt wurden, im Besitz des mit `--username` angegebenen Benutzers befinden. Weitere Informationen finden Sie in der [PostgresSQL-Dokumentation](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> Auf Azure Database for PostgreSQL-Servern sind TLS/SSL-Verbindungen standardmäßig aktiviert. Wenn Ihr PostgreSQL-Server TLS-/SSL-Verbindungen erfordert, Sie jedoch nicht darüber verfügen, legen Sie eine Umgebungsvariable `PGSSLMODE=require` fest, damit das pg_restore-Tool über TLS eine Verbindung herstellt. Ohne TLS könnte folgender Fehler gemeldet werden: „SCHWERWIEGEND: SSL-Verbindung ist erforderlich. Geben Sie SSL-Optionen an, und wiederholen Sie den Vorgang.“ Führen Sie in der Windows-Befehlszeile den Befehl `SET PGSSLMODE=require` aus, bevor Sie den Befehl `pg_restore` ausführen. Führen Sie unter Linux oder Bash den Befehl `export PGSSLMODE=require` aus, bevor Sie den Befehl `pg_restore` ausführen. 
>

In diesem Beispiel stellen Sie die Daten aus der Sicherungsdatei **testdb.dump** in der Datenbank **mypgsqldb** auf dem Zielserver **mydemoserver.postgres.database.azure.com** wieder her.

Hier ist ein Beispiel dafür, wie dieses `pg_restore` für einen Einzelserver verwendet wird:

```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

Hier ist ein Beispiel dafür, wie dieses `pg_restore` für einen flexiblen Server verwendet wird:

```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin --dbname=mypgsqldb testdb.dump
```

## <a name="optimize-the-migration-process"></a>Optimieren des Migrationsprozesses

Eine Möglichkeit zum Migrieren Ihrer vorhandenen PostgreSQL-Datenbank zu Azure Database for PostgreSQL besteht darin, die Datenbank auf dem Quellcomputer zu sichern und in Azure wiederherzustellen. Um die erforderliche Zeit zum Abschließen der Migration zu minimieren, sollten Sie die folgenden Parameter mit den Befehlen zum Sichern und Wiederherstellen verwenden.

> [!NOTE]
> Ausführliche Informationen über die Syntax finden Sie unter [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html).
>

### <a name="for-the-backup"></a>Für die Sicherung

Erstellen Sie die Sicherung mit dem Switch `-Fc`, damit Sie die Wiederherstellung parallel durchführen können, um den Vorgang zu beschleunigen. Zum Beispiel:

```bash
pg_dump -h my-source-server-name -U source-server-username -Fc -d source-databasename -f Z:\Data\Backups\my-database-backup.dump
```

### <a name="for-the-restore"></a>Für die Wiederherstellung

- Verschieben Sie die Sicherungsdatei auf eine Azure-VM in derselben Region, in der sich auch der Azure Database for PostgreSQL-Server befindet, zu dem Sie migrieren. Führen Sie `pg_restore` von dieser VM aus, um die Netzwerklatenz zu reduzieren. Aktivieren Sie zum Erstellen der VM den [beschleunigten Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-powershell.md).

- Öffnen Sie die Sicherungsdatei, um zu überprüfen, ob die CREATE INDEX-Anweisungen auf das Einfügen der Daten folgen. Wenn das nicht der Fall ist, verschieben Sie die Create Index-Anweisungen hinter das Einfügen der Daten. Dies sollte bereits standardmäßig erfolgt sein, aber Sie sollten es trotzdem überprüfen.

- Führen Sie die Wiederherstellung mit den Befehlszeilenschaltern `-Fc` und `-j` (mit einer Zahl) durch, um die Wiederherstellung zu parallelisieren. Die angegebene Zahl ist die Anzahl von Kernen auf dem Zielserver. Sie können auch die doppelte Anzahl von Kernen des Zielservers anzugeben, um die Auswirkungen zu sehen.

    Hier ist ein Beispiel dafür, wie dieses `pg_restore` für einen Einzelserver verwendet wird:

    ```bash
     pg_restore -h my-target-server.postgres.database.azure.com -U azure-postgres-username@my-target-server -Fc -j 4 -d my-target-databasename Z:\Data\Backups\my-database-backup.dump
    ```

    Hier ist ein Beispiel dafür, wie dieses `pg_restore` für einen flexiblen Server verwendet wird:

    ```bash
     pg_restore -h my-target-server.postgres.database.azure.com -U azure-postgres-username@my-target-server -Fc -j 4 -d my-target-databasename Z:\Data\Backups\my-database-backup.dump
    ```

- Sie können auch die Sicherungsdatei bearbeiten, indem Sie den Befehl `set synchronous_commit = off;` am Anfang und den Befehl `set synchronous_commit = on;` am Ende einfügen. Wenn Sie die Aktivierung nicht am Ende einfügen, bevor die Apps die Daten ändern, könnte dies zu Datenverlust führen.

- Erwägen Sie auf dem Azure Database for PostgreSQL-Server vor der Wiederherstellung Folgendes auszuführen:
    
  - Deaktivieren Sie die Nachverfolgung der Abfrageleistung. Diese Statistiken sind während der Migration nicht erforderlich. Hierzu können Sie `pg_stat_statements.track`, `pg_qs.query_capture_mode` und `pgms_wait_sampling.query_capture_mode` auf `NONE` festlegen.

  - Verwenden Sie eine SKU mit hoher Rechenleistung und viel Arbeitsspeicher, wie 32 arbeitsspeicheroptimierten virtuellen Kernen, um die Migration zu beschleunigen. Nachdem die Wiederherstellung abgeschlossen ist, können Sie die SKU problemlos wieder auf die bevorzugte Variante herunterstufen. Je höher die SKU, desto mehr Parallelität können Sie erreichen, indem Sie den entsprechenden Parameter `-j` im Befehl `pg_restore` erhöhen.

  - Mehr IOPS auf dem Zielserver könnten die Leistung bei der Wiederherstellung verbessern. Sie können mehr IOPS bereitstellen, indem Sie die Speichergröße des Servers erhöhen. Diese Einstellung ist nicht umkehrbar, aber bedenken Sie, ob Ihre tatsächliche Workload in Zukunft von einem höheren IOPS-Wert profitieren würde.

Denken Sie daran, diese Befehle in einer Testumgebung zu testen und zu überprüfen, bevor Sie sie in der Produktionsumgebung verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Migrieren einer PostgreSQL-Datenbank mithilfe von Export und Import finden Sie unter [Migrieren der PostgreSQL-Datenbank mit Import und Export](howto-migrate-using-export-and-import.md).
- Weitere Informationen zum Migrieren von Datenbanken zu Azure Database for PostgreSQL finden Sie im [Leitfaden zur Datenbankmigration](https://aka.ms/datamigration).


