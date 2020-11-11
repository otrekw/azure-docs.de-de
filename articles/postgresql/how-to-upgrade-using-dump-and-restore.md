---
title: Upgrade mithilfe von Sicherung und Wiederherstellung – Azure Database for PostgreSQL – Single Server
description: Erläuterung verschiedener Methoden zum Sichern und Wiederherstellen von Datenbanken, um zu einer höheren Version von Azure Database for PostgreSQL – Single Server zu migrieren.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 11/03/2020
ms.openlocfilehash: 26154f4501daba373f1f8b108f1ee7105b1b194f
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93294214"
---
# <a name="upgrade-your-postgresql-database-using-dump-and-restore"></a>Upgrade einer PostgreSQL-Datenbank durch Sichern und Wiederherstellen

In Azure Database for PostgreSQL – Single Server empfiehlt es sich, die PostgreSQL-Datenbank-Engine mit einer der folgenden Methoden auf eine höhere Hauptversion zu aktualisieren:
* Offlinemethode unter Verwendung der PostgreSQL-Befehle [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html). Bei dieser Methode führen Sie zunächst die Sicherung auf dem Quellserver aus und stellen die Sicherung dann auf dem Zielserver wieder her.
* Onlinemethode mit [**Database Migration Service**](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal) (DMS). Bei dieser Methode bleibt die Zieldatenbank mit der Quelldatenbank synchron, und Sie können wählen, wann der Übergang erfolgen soll. Allerdings müssen einige Voraussetzungen und Einschränkungen beachtet werden. 

Die folgende Empfehlung kann Ihnen bei der Entscheidung zwischen der Online- und Offlinemethode für Upgrades von Hauptversionen helfen.

| **Datenbank** | **Sichern/Wiederherstellen (offline)** | **DMS (online)** |
| ------ | :------: | :-----: |
| Sie haben eine kleine Datenbank und können von einem Upgrade verursachte Ausfallzeiten verkraften  | X | |
| Kleine Datenbanken (< 10 GB)  | X | X | 
| Kleine bis mittelgroße Datenbanken (10 GB bis 100 GB) | X | X |
| Große Datenbanken (> 100 GB) |  | X |
| Können von einem Upgrade verursachte Ausfallzeiten (unabhängig von der Datenbankgröße) verkraftet werden? | X |  |
| Können [Voraussetzungen](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal#prerequisites) für DMS einschließlich Neustart erfüllt werden? |  | X |
| Können DDLs und nicht protokollierte Tabellen während des Upgradevorgangs vermieden werden? | |  X |

Diese Anleitung bietet zwei Beispielmethoden, wie Sie Ihre Datenbanken mit den PostgreSQL-Befehlen pg_dump und pg_restore aktualisieren können. Der Prozess wird in diesem Dokument als **Upgrade** bezeichnet, obwohl die Datenbank vom Quell- auf den Zielserver **migriert** wird. 

> [!NOTE]
> Die PostgreSQL-Sicherung und -Wiederherstellung kann auf verschiedene Weisen erfolgen. Sie haben die Möglichkeit, andere Methoden als die in diesem Dokument genannten zu wählen. Wenn Sie beispielsweise eine Sicherung und anschließende Wiederherstellung auf einem PostgreSQL-Client ausführen möchten, finden Sie in der [Dokumentation](./howto-migrate-using-dump-and-restore.md) Einzelheiten zu den einzelnen Schritten und bewährten Methoden. Die detaillierte Syntax für Sicherung und Wiederherstellung mit zusätzlichen Parametern finden Sie in den Artikeln [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html). 


## <a name="prerequisites-for-using-dump-and-restore-with-azure-postgresql"></a>Voraussetzungen für die Sicherung und Wiederherstellung mit Azure PostgreSQL
 
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Quelldatenbank der Version 9.5, 9.6 oder 10 (Azure Database for PostgreSQL – Single Server)
- Zieldatenbankserver mit der gewünschten PostgreSQL-Hauptversion von [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md). 
- Clientsystem (Linux), auf dem PostgreSQL sowie die Befehlszeilenprogramme [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) und [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) installiert sind. 
- Alternativ können Sie [Azure Cloud Shell](https://shell.azure.com) verwenden, indem Sie auf der Menüleiste rechts oben im [Azure-Portal](https://portal.azure.com) auf Azure Cloud Shell klicken. Sie müssen sich bei Ihrem Konto `az login` anmelden, ehe Sie die Befehle zur Sicherung und Wiederherstellung ausführen können.
- Den Standort Ihres PostgreSQL-Clients, z. B. eine VM, die vorzugsweise in der gleichen Region wie der Quell- und Zielserver läuft. 

## <a name="additional-details-and-considerations"></a>Weitere Details und Überlegungen
- Sie können die Verbindungszeichenfolge für die Quell- und Zieldatenbank finden, indem Sie im Portal auf „Verbindungszeichenfolgen“ klicken. 
- Möglicherweise führen Sie mehr als eine Datenbank auf dem Server aus. Sie finden die Liste der Datenbanken, indem Sie eine Verbindung mit dem Quellserver herstellen und `\l` ausführen.
- Erstellen Sie entsprechende Datenbanken auf dem Zieldatenbankserver.
- Sie können das Upgrade von `azure_maintenance` -oder Vorlagendatenbanken überspringen.
- Anhand der obigen Tabellen können Sie feststellen, ob die Datenbank für diesen Migrationsmodus geeignet ist.
- Wenn Sie Azure Cloud Shell verwenden möchten, erfolgt für die Sitzung nach 20 Minuten ein Timeout. Wenn die Datenbankgröße kleiner als 10 GB ist, können Sie das Upgrade ohne Timeout abschließen. Andernfalls müssen Sie die Sitzung möglicherweise durch andere Mittel geöffnet halten, z. B. durch einmaliges Drücken der <Enter>-Taste alle 10-15 Minuten. 

> [!TIP] 
> - Wenn Sie für Quell- und Zieldatenbank dasselbe Kennwort verwenden, können Sie die Umgebungsvariable `PGPASSWORD=yourPassword` festlegen.  Dann müssen Sie nicht jedes Mal ein Kennwort angeben, wenn Sie Befehle wie psql, pg_dump und pg_restore ausführen.  Ebenso können Sie zusätzliche Variablen wie `PGUSER`, `PGSSLMODE` usw. einrichten. Weitere Informationen finden Sie unter [PostgreSQL-Umgebungsvariablen](https://www.postgresql.org/docs/11/libpq-envars.html).
>  
> - Wenn Ihr PostgreSQL-Server TLS-/SSL-Verbindungen erfordert (auf Azure Database for PostgreSQL-Servern standardmäßig aktiviert), legen Sie eine Umgebungsvariable `PGSSLMODE=require` fest, damit das pg_restore-Tool über TLS eine Verbindung herstellt. Erfolgt die Verbindung nicht über TLS, kann der Fehler möglicherweise wie folgt lauten: `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> - Führen Sie in der Windows-Befehlszeile den Befehl `SET PGSSLMODE=require` aus, bevor Sie den Befehl „pg_restore“ ausführen. Führen Sie unter Linux oder Bash den Befehl `export PGSSLMODE=require` aus, bevor Sie den Befehl „pg_restore“ ausführen.

## <a name="example-database-used-in-this-guide"></a>In dieser Anleitung verwendete Beispieldatenbank

 | **Beschreibung** | **Wert** |
 | ------- | ------- |
 | Quellserver (Version 9.5) | pg-95.postgres.database.azure.com |
 | Quelldatenbank | bench5gb |
 | Größe der Quelldatenbank | 5 GB |
 | Quellbenutzername | pg@pg-95 |
 | Zielserver (Version 11) | pg-11.postgres.database.azure.com |
 | Zieldatenbank | bench5gb |
 | Zielbenutzername | pg@pg-11 |

## <a name="method-1-upgrade-with-streaming-backups-to-the-target"></a>Methode 1: Upgrade durch Streaming der Sicherungen in das Ziel 

Bei dieser Methode wird die gesamte Datenbanksicherung direkt zum Zieldatenbankserver gestreamt, ohne dass die Sicherung im Client gespeichert wird. Folglich kann diese Methode bei einem Client mit begrenztem Speicherplatz verwendet und sogar über Azure Cloud Shell ausgeführt werden. 

1. Vergewissern Sie sich mit dem Befehl `\l`, dass die Datenbank auf dem Zielserver vorhanden ist. Wenn die Datenbank nicht vorhanden ist, erstellen Sie sie.
   ```azurecli-interactive
    psql "host=myTargetServer port=5432 dbname=postgres user=myUser password=###### sslmode=mySSLmode"
    ```
    ```bash
    postgres> \l   
    postgres> create database myTargetDB;
   ```

2. Führen Sie die Sicherung und Wiederherstellung als einzelne Befehlszeile mithilfe einer Pipe aus. 
    ```azurecli-interactive
    pg_dump -Fc -v --mySourceServer --port=5432 --username=myUser --dbname=mySourceDB | pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB
    ```

    Beispiel:

    ```azurecli-interactive
    pg_dump -Fc -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb | pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb
    ```  
3. Nach Abschluss des Prozesses zum Upgrade (Migration) können Sie Ihre Anwendung auf dem Zielserver testen. 
4. Wiederholen Sie diesen Vorgang für alle Datenbanken auf dem Server.

 In der folgenden Tabelle finden Sie eine Angabe zur Dauer des Upgrades mit dieser Methode. Die Daten werden mithilfe von [pgbench](https://www.postgresql.org/docs/10/pgbench.html) aufgefüllt. Da Ihre Datenbank eine andere Anzahl von Objekten mit unterschiedlichen Größen als die von pgbench generierten Tabellen und Indizes enthalten kann, wird dringend empfohlen, die Befehle zur Sicherung und Wiederherstellung Ihrer Datenbank zu testen, um die tatsächliche Dauer der Aktualisierung Ihrer Datenbank zu ermitteln. 

| **Datenbankgröße** | **Ca. benötigte Zeit** | 
| ----- | ------ |
| 1 GB  | 1-2 Minuten |
| 5 GB | 8-10 Minuten |
| 10 GB | 15-20 Minuten |
| 50 GB | 1-1,5 Stunden |
| 100 GB | 2,5-3 Stunden|
   
## <a name="method-2-upgrade-with-parallel-dump-and-restore"></a>Methode 2: Upgrade mit paralleler Sicherung und Wiederherstellung 

Diese Methode eignet sich, wenn die Datenbank nur wenige größere Tabellen enthält und Sie den Sicherungs- und Wiederherstellungsprozess für diese Datenbank parallelisieren möchten. Sie benötigen genügend lokalen Datenträgerspeicher für die Sicherungen Ihrer Datenbanken. Dieser parallele Sicherungs- und Wiederherstellungsprozess verringert den Zeitaufwand für die gesamte Migration bzw. das gesamte Upgrade. Beispielsweise wurde der Vorgang für die 50 GB große pgbench-Datenbank, deren Migration 1-1,5 Stunden dauerte, in weniger als 30 Minuten abgeschlossen.

1. Erstellen Sie für jede Datenbank auf dem Quellserver eine entsprechende Datenbank auf dem Zielserver.

   ```bash
    psql "host=myTargetServer port=5432 dbname=postgres user=myuser password=###### sslmode=mySSLmode"
    postgresl> create database myDB;
   ```
   Beispiel:
    ```bash
    psql "host=pg-11.postgres.database.azure.com port=5432 dbname=postgres user=pg@pg-11 password=###### sslmode=require"

    postgres> create database bench5gb;
    postgres> \q
    ```

2. Führen Sie den Befehl pg_dump in einem Verzeichnisformat mit „Anzahl der Aufträge = 4 (Anzahl der Tabellen in der Datenbank)“ aus. Bei einem höheren Computetarif und mehr Tabellen können Sie diesen Wert steigern. Mit diesem Befehl pg_dump wird ein Verzeichnis erstellt, in dem komprimierte Dateien für jeden Auftrag gespeichert werden.

    ```bash
    pg_dump -Fd -v --host=sourceServer --port=5432 --username=myUser --dbname=mySourceDB -j 4 -f myDumpDirectory
    ```
    Beispiel:
    ```bash
    pg_dump -Fd -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb -j 4 -f dump.dir
    ```

3. Stellen Sie anschließend die Sicherung auf dem Zielserver wieder her.
    ```bash
    $ pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB -j 4 myDumpDir
    ```
    Beispiel:
    ```bash
    $ pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb -j 4 dump.dir
    ```

> [!TIP]
> Der in diesem Dokument erwähnte Prozess kann auch zum Einsatz kommen, um Ihre Instanz von Azure Database for PostgreSQL – Flexibler Server (in der Vorschau) zu aktualisieren. Der Hauptunterschied besteht darin, dass die Verbindungszeichenfolge für das Ziel auf dem flexiblen Server `@dbName` nicht enthält.  Wenn der Benutzername beispielsweise `pg` ist, lautet der Benutzername des einzelnen Servers in der Verbindungszeichenfolge `pg@pg-95`, während Sie bei einem flexiblen Server einfach `pg` verwenden können.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie mit der Funktionsweise der Zieldatenbank zufrieden sind, können Sie Ihren alten Datenbankserver löschen. 
- Wenn Sie den gleichen Datenbankendpunkt wie der Quellserver verwenden möchten, können Sie, nachdem Sie Ihren alten Quelldatenbankserver gelöscht haben, ein Lesereplikat mit dem Namen des alten Datenbankservers erstellen. Sobald der Zustand stabil ist, können Sie das Replikat beenden, wodurch der Replikatserver zu einem unabhängigen Server höher gestuft wird. Weitere Informationen finden Sie unter [Replikation](./concepts-read-replicas.md).
- Denken Sie daran, diese Befehle in einer Testumgebung zu testen und zu überprüfen, bevor Sie sie in der Produktionsumgebung verwenden.
