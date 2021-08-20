---
title: 'Tutorial: Onlinemigration von PostgreSQL zu Azure Database for PostgreSQL über die Azure-Befehlszeilenschnittstelle'
titleSuffix: Azure Database Migration Service
description: Hier erfahren Sie, wie Sie mit Azure Database Migration Service über die Befehlszeilenschnittstelle eine Onlinemigration von einer lokalen PostgreSQL-Instanz zu Azure Database for PostgreSQL durchführen.
services: dms
author: arunkumarthiags
ms.author: arthiaga
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019, devx-track-azurecli
ms.topic: tutorial
ms.date: 04/11/2020
ms.openlocfilehash: 52d5c7a500652b0090cf9b21400a9c45f2bf54e7
ms.sourcegitcommit: b044915306a6275c2211f143aa2daf9299d0c574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113033729"
---
# <a name="tutorial-migrate-postgresql-to-azure-db-for-postgresql-online-using-dms-via-the-azure-cli"></a>Tutorial: Onlinemigration von PostgreSQL zu Azure Database for PostgreSQL mit Database Migration Service über die Azure-Befehlszeilenschnittstelle

Mit Azure Database Migration Service können Sie die Datenbanken mit minimaler Ausfallzeit von einer lokalen PostgreSQL-Instanz zu [Azure Database for PostgreSQL](../postgresql/index.yml) migrieren. Somit kommt es bei der Migration nur zu einer geringen Ausfallzeit für die Anwendung. In diesem Tutorial migrieren Sie die Beispieldatenbank **DVD Rental** von einer lokalen Instanz von PostgreSQL 9.6 zu Azure Database for PostgreSQL. Zu diesem Zweck verwenden Sie die Onlinemigrationsaktivität in Azure Database Migration Service.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
>
> * Migrieren des Beispielschemas mit dem Hilfsprogramm pg_dump
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration

> [!NOTE]
> Die Verwendung von Azure Database Migration Service zum Ausführen einer Onlinemigration erfordert das Erstellen einer Instanz auf der Grundlage des Premium-Tarifs. Wir verschlüsseln den Datenträger, um Datendiebstahl während der Migration zu verhindern.

> [!IMPORTANT]
> Für eine optimale Migration empfiehlt Microsoft die Erstellung einer Azure Database Migration Service-Instanz in derselben Azure-Region, in der sich auch die Zieldatenbank befindet. Die Verschiebung von Daten zwischen Regionen oder Geografien kann den Migrationsvorgang verlangsamen und Fehler verursachen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Laden Sie [PostgreSQL Community Edition](https://www.postgresql.org/download/) 9.5, 9.6 oder 10 herunter, und installieren Sie die Edition. Die PostgreSQL-Version des Quellservers muss 9.5.11, 9.6.7, 10 oder höher sein. Weitere Informationen finden Sie im Artikel [Unterstützte PostgreSQL-Datenbankversionen](../postgresql/concepts-supported-versions.md).

    Beachten Sie auch, dass die Azure Database for PostgreSQL-Zielversion höher als die lokale PostgreSQL-Version sein oder dieser entsprechen muss. Beispiel: PostgreSQL 9.6 kann nur zu Azure Database for PostgreSQL 9.6, 10 oder 11 migriert werden, nicht aber zu Azure Database for PostgreSQL 9.5.

* [Erstellen einer Instanz in Azure Database for PostgreSQL-Server](../postgresql/quickstart-create-server-database-portal.md) oder [Erstellen eines Azure Database for PostgreSQL-Servers – Hyperscale (Citus)](../postgresql/quickstart-create-hyperscale-portal.md).
* Erstellen Sie ein virtuelles Microsoft Azure-Netzwerk für Azure Database Migration Service mithilfe des Azure Resource Manager-Bereitstellungsmodells, das Site-to-Site-Konnektivität für Ihre lokalen Quellserver über [ExpressRoute](../expressroute/expressroute-introduction.md) oder über [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) bereitstellt. Weitere Informationen zum Erstellen eines virtuellen Netzwerks finden Sie in der [Dokumentation zu Virtual Network](../virtual-network/index.yml) und insbesondere in den Schnellstartartikeln mit Schritt-für-Schritt-Anleitungen.

    > [!NOTE]
    > Fügen Sie bei Verwendung von ExpressRoute mit Netzwerkpeering zu Microsoft während des Setups des virtuellen Netzwerks die folgenden [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) zu dem Subnetz hinzu, in dem der Dienst bereitgestellt werden soll:
    >
    > * Zieldatenbankendpunkt (z. B. SQL-Endpunkt, Cosmos DB-Endpunkt usw.)
    > * Speicherendpunkt
    > * Service Bus-Endpunkt
    >
    > Diese Konfiguration ist erforderlich, weil Azure Database Migration Service über keine Internetverbindung verfügt.

* Stellen Sie sicher, dass die NSG-Regeln (Netzwerksicherheitsgruppen) des virtuellen Netzwerks nicht den ausgehenden Port 443 von ServiceTag für ServiceBus, Storage und AzureMonitor blockieren. Ausführlichere Informationen zur NSG-Datenverkehrsfilterung in einem virtuellen Netzwerk finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Konfigurieren Sie Ihre [Windows-Firewall für Datenbank-Engine-Zugriff](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die PostgreSQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 5432).
* Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.
* Erstellen Sie für Azure Database for PostgreSQL eine [Firewallregel](../postgresql/concepts-firewall-rules.md) auf Serverebene, um Azure Database Migration Service den Zugriff auf die Zieldatenbanken zu ermöglichen. Geben Sie den Subnetzbereich des für Azure Database Migration Service verwendeten virtuellen Netzwerks an.
* Es gibt zwei Methoden zum Aufrufen der CLI:

  * Wählen Sie im Azure-Portal rechts oben die Cloud Shell-Schaltfläche aus:

       ![Cloud Shell-Schaltfläche im Azure-Portal](media/tutorial-postgresql-to-azure-postgresql-online/cloud-shell-button.png)

  * Installieren Sie die CLI lokal, und führen Sie sie aus. Für die Verwaltung der für diese Migration erforderlichen Azure-Ressourcen ist die Befehlszeilenschnittstelle in der Version CLI 2.18 oder höher erforderlich.

       Um die Befehlszeilenschnittstelle herunterzuladen, befolgen Sie die Anweisungen im Artikel [Installieren von Azure CLI](/cli/azure/install-azure-cli). In dem Artikel werden außerdem die Plattformen aufgelistet, die Azure CLI unterstützen.

       Um das Windows-Subsystem für Linux (WSL) einzurichten, befolgen Sie die Anweisungen im [Windows 10-Installationshandbuch](/windows/wsl/install-win10).

* Aktivieren Sie die logische Replikation auf dem Quellserver, indem Sie die postgresql.config-Datei bearbeiten und die folgenden Parameter festlegen:

  * wal_level = **logical**
  * max_replication_slots = [Anzahl von Slots], empfohlene Einstellung: **5 Slots**
  * max_wal_senders = [Anzahl gleichzeitiger Aufgaben] – Der max_wal_senders-Parameter legt die Anzahl von Aufgaben fest, die gleichzeitig ausgeführt werden können; empfohlene Einstellung: **10 Aufgaben**

## <a name="migrate-the-sample-schema"></a>Migrieren des Beispielschemas

Zum Fertigstellen aller Datenbankobjekte wie Tabellenschemas, Indizes und gespeicherter Prozeduren muss das Schema aus der Quelldatenbank extrahiert und auf die Datenbank angewendet werden.

1. Verwenden Sie den Befehl „pg_dump -s“, um eine Schemasicherungsdatei für eine Datenbank zu erstellen. 

    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```

    Geben Sie beispielsweise Folgendes ein, um eine Schemadatei-dvdrental-Datenbank zu sichern:
    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s  > dvdrentalSchema.sql
    ```

    Weitere Informationen zur Verwendung des Dienstprogramms „pg_dump“ finden Sie in den Beispielen des [pg-dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html#PG-DUMP-EXAMPLES)-Tutorials.

2. Erstellen Sie eine leere Datenbank in der Zielumgebung, Azure Database for PostgreSQL.

    Ausführliche Anleitungen zum Erstellen und Verbinden einer Datenbank finden Sie unter [Erstellen eines Azure Database for PostgreSQL-Servers im Azure-Portal](../postgresql/quickstart-create-server-database-portal.md) und unter [Erstellen eines Azure Database for PostgreSQL-Servers – Hyperscale (Citus) über das Azure-Portal](../postgresql/quickstart-create-hyperscale-portal.md).

3. Importieren Sie das Schema in die Zieldatenbank, die Sie erstellt haben, indem Sie die Schemasicherungsdatei wiederherstellen.

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql 
    ```

    Beispiel:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental < dvdrentalSchema.sql
    ```

  > [!NOTE]
   > Der Migrationsdienst verwaltet intern die Aktivierung/Deaktivierung von Fremdschlüsseln und Triggern, um eine zuverlässige und stabile Datenmigration zu gewährleisten. Daher müssen Sie sich keine Gedanken über Änderungen am Schema der Zieldatenbank machen.

## <a name="provisioning-an-instance-of-dms-using-the-azure-cli"></a>Bereitstellung einer DMS-Instanz mithilfe der Azure CLI

1. Installieren der DMS-Synchronisierungserweiterung:
   * Melden Sie sich mithilfe des folgenden Befehls bei Azure an:
       ```azurecli
       az login
       ```

   * Wenn Sie aufgefordert werden, öffnen Sie einen Webbrowser, und geben Sie einen Code ein, um Ihr Gerät zu authentifizieren. Befolgen Sie die Anweisungen gemäß der Auflistung.

   * Die PostgreSQL-Onlinemigration ist jetzt im regulären CLI-Paket (Version 2.18.0 und höher) verfügbar, ohne dass die `dms-preview`-Erweiterung erforderlich ist. Wenn Sie diese Erweiterung in der Vergangenheit installiert haben, können Sie sie wie folgt entfernen:
        * Führen Sie den folgenden Befehl aus, um zu überprüfen, ob die `dms-preview`-Erweiterung bereits installiert ist:
        
            ```azurecli
            az extension list -o table
            ```

        * Wenn die `dms-preview`-Erweiterung installiert ist, führen Sie den folgenden Befehl aus, um sie zu deinstallieren:
        
            ```azurecli
            az extension remove --name dms-preview
            ```

        * Um zu überprüfen, ob Sie die `dms-preview`-Erweiterung ordnungsgemäß deinstalliert haben, führen Sie den folgenden Befehl aus. Die `dms-preview`-Erweiterung sollte nicht mehr in der Liste angezeigt werden:

            ```azurecli
            az extension list -o table
            ```

      > [!IMPORTANT]
      > Die `dms-preview`-Erweiterung ist möglicherweise weiterhin für andere Migrationspfade erforderlich, die von Azure DMS unterstützt werden. Ob die Erweiterung erforderlich ist, können Sie der Dokumentation des jeweiligen Migrationspfads entnehmen. Diese Dokumentation beinhaltet die Erweiterungsanforderung, die für die Online-Migration von PostgreSQL zu Azure Database for PostgreSQL erforderlich ist.

   * Jederzeit können Sie alle in DMS unterstützten Befehle anzeigen, indem Sie Folgendes ausführen:

       ```azurecli
       az dms -h
       ```

   * Wenn Sie über mehrere Azure-Abonnements verfügen, führen Sie den folgenden Befehl aus, um das Abonnement festzulegen, mit dem Sie eine Instanz des DMS-Diensts bereitstellen möchten.

        ```azurecli
       az account set -s 97181df2-909d-420b-ab93-1bff15acb6b7
        ```

2. Stellen Sie eine Instanz von DMS bereit, indem Sie den folgenden Befehl ausführen:

   ```azurecli
   az dms create -l <location> -n <newServiceName> -g <yourResourceGroupName> --sku-name Premium_4vCores --subnet/subscriptions/{vnet subscription id}/resourceGroups/{vnet resource group}/providers/Microsoft.Network/virtualNetworks/{vnet name}/subnets/{subnet name} –tags tagName1=tagValue1 tagWithNoValue
   ```

   Der folgende Befehl erstellt z.B. einen Dienst in:

   * Standort: USA (Ost 2)
   * Abonnement: 97181df2-909d-420b-ab93-1bff15acb6b7
   * Ressourcengruppenname: PostgresDemo
   * DMS-Dienstname: PostgresCLI

   ```azurecli
   az dms create -l eastus2 -g PostgresDemo -n PostgresCLI --subnet /subscriptions/97181df2-909d-420b-ab93-1bff15acb6b7/resourceGroups/ERNetwork/providers/Microsoft.Network/virtualNetworks/AzureDMS-CORP-USC-VNET-5044/subnets/Subnet-1 --sku-name Premium_4vCores
   ```

   Es dauert ungefähr 10 bis 12 Minuten, um die Instanz des DMS-Diensts zu erstellen.

3. Um die IP-Adresse des DMS-Agent zu identifizieren, damit Sie ihn der Postgres-Datei „pg_hba.conf“ hinzufügen können, führen Sie den folgenden Befehl aus:

    ```azurecli
    az network nic list -g <ResourceGroupName>--query '[].ipConfigurations | [].privateIpAddress'
    ```

    Beispiel:

    ```azurecli
    az network nic list -g PostgresDemo --query '[].ipConfigurations | [].privateIpAddress'
    ```

    Das Ergebnis sollte in etwa der folgenden Adresse entsprechen: 

    ```output
    [
      "172.16.136.18"
    ]
    ```

4. Fügen Sie die IP-Adresse des DMS-Agent der Postgres-Datei „pg_hba.conf“ hinzu.

    * Notieren Sie sich die DMS-IP-Adresse nach Abschluss der Bereitstellung in DMS.
    * Fügen Sie die IP-Adresse der Datei „pg_hba.conf“ auf dem Quellcomputer ähnlich dem folgenden Eintrag hinzu:

        ```
        host     all            all        172.16.136.18/10    md5
        host     replication    postgres   172.16.136.18/10    md5
        ```

5. Erstellen Sie als Nächstes ein PostgreSQL-Migrationsprojekt mithilfe des folgenden Befehls:
    
    ```azurecli
    az dms project create -l <location> -g <ResourceGroupName> --service-name <yourServiceName> --source-platform PostgreSQL --target-platform AzureDbforPostgreSQL -n <newProjectName>
    ```

    Der folgende Befehl erstellt z.B. ein Projekt mit folgenden Parametern:

    * Standort: USA, Westen-Mitte
    * Ressourcengruppenname: PostgresDemo
    * Servicename: PostgresCLI
    * Projektname: PGMigration
    * Quellplattform: PostgreSQL
    * Zielplattform: AzureDbForPostgreSql   

     ```azurecli
     az dms project create -l westcentralus -n PGMigration -g PostgresDemo --service-name PostgresCLI --source-platform PostgreSQL --target-platform AzureDbForPostgreSql
     ```

6. Erstellen Sie mit den folgenden Schritten eine PostgreSQL-Migrationsaufgabe.

    Dieser Schritt umfasst die Quell-IP, Benutzer-ID und Kennwort, Ziel-IP, Benutzer-ID, Kennwort und Aufgabentyp, um eine Verbindung herzustellen.

    * Um eine vollständige Liste der Optionen anzuzeigen, führen Sie diesen Befehl aus:

        ```azurecli
        az dms project task create -h
        ```

        Sowohl für die Quell- als auch Zielverbindung verweist der Eingabeparameter auf eine JSON-Datei, die die Objektliste enthält.

        Das Format des JSON-Verbindungsobjekts für PostgreSQL-Verbindungen.
        
        ```json
        {
            // if this is missing or null, you will be prompted
            "userName": "user name",
            // if this is missing or null (highly recommended) you will  be prompted  
            "password": null,
            "serverName": "server name",
            // if this is missing, it will default to the 'postgres' database
            "databaseName": "database name",
            // if this is missing, it will default to 5432 
            "port": 5432                
        }
        ```

        Es gibt auch eine Datenbankoptions-JSON-Datei, die die JSON-Objekte auflistet. Für PostgreSQL ist das Format des Datenbankoptions-JSON-Objekts unten dargestellt:

        ```json
        [
            {
                "name": "source database",
                "target_database_name": "target database",
                "selectedTables": [
                    "schemaName1.tableName1",
                    ...n
                ]
            },
            ...n
        ]
        ```

    * Um die Quellverbindungs-JSON zu erstellen, öffnen Sie den Editor, kopieren Sie den folgenden JSON-Code, und fügen Sie ihn in die Datei ein. Speichern Sie die Datei unter C:\DMS\source.json, nachdem Sie sie entsprechend Ihres Quellservers angepasst haben.

        ```json
        {
            "userName": "postgres",    
            "password": null,
            "serverName": "13.51.14.222",
            "databaseName": "dvdrental", 
            "port": 5432                
        }
        ```

    * Um die Zielverbindungs-JSON zu erstellen, öffnen Sie den Editor, kopieren Sie den folgenden JSON-Code, und fügen Sie ihn in die Datei ein. Speichern Sie die Datei unter C:\DMS\target.json, nachdem Sie sie entsprechend Ihres Zielservers angepasst haben.
    
        ```json
        {
            "userName": " dms@builddemotarget",    
            "password": null,           
            "serverName": " builddemotarget.postgres.database.azure.com",
            "databaseName": "inventory", 
            "port": 5432                
        }
        ```

    * Erstellen Sie eine Datenbankoptions-JSON-Datei, in der der Bestand und die Zuordnung der zu migrierenden Datenbanken aufgeführt ist:

        * Erstellen Sie eine Liste der zu migrierenden Tabellen, oder verwenden Sie eine SQL-Abfrage, um die Liste aus der Quelldatenbank zu generieren. Eine Beispielabfrage, mit der Sie eine Liste von Tabellen generieren können, wird unten als Beispiel angegeben. Denken Sie beim Verwenden dieser Abfrage daran, das letzte Komma am Ende des letzten Tabellennamens zu entfernen, damit sie zu einem gültigen JSON-Array wird. 
        
            ```sql
            SELECT
                FORMAT('%s,', REPLACE(FORMAT('%I.%I', schemaname, tablename), '"', '\"')) AS SelectedTables
            FROM 
                pg_tables
            WHERE 
                schemaname NOT IN ('pg_catalog', 'information_schema');
            ```

        * Erstellen Sie die Datenbankoptions-JSON-Datei mit einem Eintrag für jede Datenbank, der den Namen der Quell- und Zieldatenbank sowie eine Liste der ausgewählten zu migrierenden Tabellen enthält. Sie können die Ausgabe der obigen SQL-Abfrage verwenden, um das Array *„selectedTables“* (ausgewählte Tabellen) aufzufüllen. **Beachten Sie Folgendes: Wenn die Liste mit den ausgewählten Tabellen leer ist, wird der Dienst alle Tabellen mit passenden Schema- und Tabellennamen migrieren**.
        
            ```json
            [
                {
                    "name": "dvdrental",
                    "target_database_name": "dvdrental",
                    "selectedTables": [
                        "schemaName1.tableName1",
                        "schemaName1.tableName2",                    
                        ...
                        "schemaNameN.tableNameM"
                    ]
                },
                ... n
            ]
            ```

    * Führen Sie den folgenden Befehl aus, der die Quellverbindung, die Zielverbindung und die Datenbankoptions-JSON-Dateien aufnimmt.

        ```azurecli
        az dms project task create -g PostgresDemo --project-name PGMigration --source-connection-json c:\DMS\source.json --database-options-json C:\DMS\option.json --service-name PostgresCLI --target-connection-json c:\DMS\target.json --task-type OnlineMigration -n runnowtask    
        ```

    An diesem Punkt haben Sie erfolgreich eine Migrationsaufgabe gesendet.

7. Um den Fortschritt der Aufgabe anzuzeigen, führen Sie den folgenden Befehl aus:

    * Um den allgemeinen Taskstatus als Kurzversion anzuzeigen
        ```azurecli
        az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask
        ```

    * Um den detaillierten Taskstatus einschließlich der Informationen zum Migrationsfortschritt anzuzeigen

        ```azurecli
        az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask --expand output
        ```

    * Sie können auch das [JMESPATH](https://jmespath.org/)-Abfrageformat verwenden, um ausschließlich den Migrationsstatus (migrationState) aus der erweiterten Ausgabe zu extrahieren:

        ```azurecli
        az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask --expand output --query 'properties.output[].migrationState'
        ```

        In der Ausgabe gibt es mehrere Parameter, die den Status der verschiedenen Migrationsschritte angeben. Betrachten Sie z.B. die folgende Ausgabe:

        ```json
        {
            "output": [
                // Database Level
                {
                    "appliedChanges": 0, // Total incremental sync applied after full load
                    "cdcDeleteCounter": 0, // Total delete operation  applied after full load
                    "cdcInsertCounter": 0, // Total insert operation applied after full load
                    "cdcUpdateCounter": 0, // Total update operation applied after full load
                    "databaseName": "inventory",
                    "endedOn": null,
                    "fullLoadCompletedTables": 2, //Number of tables completed full load
                    "fullLoadErroredTables": 0, //Number of tables that contain migration error
                    "fullLoadLoadingTables": 0, //Number of tables that are in loading status
                    "fullLoadQueuedTables": 0, //Number of tables that are in queued status
                    "id": "db|inventory",
                    "incomingChanges": 0, //Number of changes after full load
                    "initializationCompleted": true,
                    "latency": 0,
                    //Status of migration task
                    "migrationState": "READY_TO_COMPLETE", //READY_TO_COMPLETE => the database is ready for cutover
                    "resultType": "DatabaseLevelOutput",
                    "startedOn": "2018-07-05T23:36:02.27839+00:00"
                }, {
                    "databaseCount": 1,
                    "endedOn": null,
                    "id": "dd27aa3a-ed71-4bff-ab34-77db4261101c",
                    "resultType": "MigrationLevelOutput",
                    "sourceServer": "138.91.123.10",
                    "sourceVersion": "PostgreSQL",
                    "startedOn": "2018-07-05T23:36:02.27839+00:00",
                    "state": "PENDING",
                    "targetServer": "builddemotarget.postgres.database.azure.com",
                    "targetVersion": "Azure Database for PostgreSQL"
                },
                // Table 1
                {
                    "cdcDeleteCounter": 0,
                    "cdcInsertCounter": 0,
                    "cdcUpdateCounter": 0,
                    "dataErrorsCount": 0,
                    "databaseName": "inventory",
                    "fullLoadEndedOn": "2018-07-05T23:36:20.740701+00:00", //Full load completed time
                    "fullLoadEstFinishTime": "1970-01-01T00:00:00+00:00",
                    "fullLoadStartedOn": "2018-07-05T23:36:15.864552+00:00", //Full load started time
                    "fullLoadTotalRows": 10, //Number of rows loaded in full load
                    "fullLoadTotalVolumeBytes": 7056, //Volume in Bytes in full load
                    "id": "or|inventory|public|actor",
                    "lastModifiedTime": "2018-07-05T23:36:16.880174+00:00",
                    "resultType": "TableLevelOutput",
                    "state": "COMPLETED", //State of migration for this table
                    "tableName": "public.catalog", //Table name
                    "totalChangesApplied": 0 //Total sync changes that applied after full load
                },
                //Table 2
                {
                    "cdcDeleteCounter": 0,
                    "cdcInsertCounter": 50,
                    "cdcUpdateCounter": 0,
                    "dataErrorsCount": 0,
                    "databaseName": "inventory",
                    "fullLoadEndedOn": "2018-07-05T23:36:23.963138+00:00",
                    "fullLoadEstFinishTime": "1970-01-01T00:00:00+00:00",
                    "fullLoadStartedOn": "2018-07-05T23:36:19.302013+00:00",
                    "fullLoadTotalRows": 112,
                    "fullLoadTotalVolumeBytes": 46592,
                    "id": "or|inventory|public|address",
                    "lastModifiedTime": "2018-07-05T23:36:20.308646+00:00",
                    "resultType": "TableLevelOutput",
                    "state": "COMPLETED",
                    "tableName": "public.orders",
                    "totalChangesApplied": 0
                }
            ],
            // DMS migration task state
            "state": "Running", //Running => service is still listening to any changes that might come in
            "taskType": null
        }
        ```

## <a name="cutover-migration-task"></a>Übernahmemigrationsaufgabe

Die Datenbank ist zur Übernahme bereit, wenn die Ladung vollständig abgeschlossen ist. Je nach Auslastung des Quellservers mit neuen Transaktionen könnte die DMS-Aufgabe möglicherweise weiterhin nach Abschluss des Ladevorgangs Änderungen anwenden.

Um sicherzustellen, dass alle Daten erfasst sind, vergleichen Sie die Zeilenanzahl in Quell- und Zieldatenbank. Sie können z. B. die folgenden Details anhand der Statusausgabe überprüfen:

```
Database Level
"migrationState": "READY_TO_COMPLETE" => Status of migration task. READY_TO_COMPLETE means database is ready for cutover
"incomingChanges": 0 => Check for a period of 5-10 minutes to ensure no new incoming changes need to be applied to the target server

Table Level (for each table)
"fullLoadTotalRows": 10    => The row count matches the initial row count of the table
"cdcDeleteCounter": 0      => Number of deletes after the full load
"cdcInsertCounter": 50     => Number of inserts after the full load
"cdcUpdateCounter": 0      => Number of updates after the full load
```

1. Führen Sie die Übernahmemigrationsaufgabe mit dem folgenden Befehl durch:

    ```azurecli
    az dms project task cutover -h
    ```

    Mit dem folgenden Befehl wird beispielsweise die Übernahme der Datenbank „Inventory“ initiiert:

    ```azurecli
    az dms project task cutover --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask  --object-name Inventory
    ```

2. Überwachen Sie den Übernahmefortschritt mithilfe des folgenden Befehls :

    ```azurecli
    az dms project task show --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask
    ```
3. Wenn der Status der Datenbankmigration als **Abgeschlossen** angezeigt wird, [erstellen Sie Sequenzen neu](https://wiki.postgresql.org/wiki/Fixing_Sequences) (sofern erforderlich), und stellen Sie eine Verbindung zwischen Ihren Anwendungen und der neuen Azure Database for PostgreSQL-Zielinstanz her.

## <a name="service-project-task-cleanup"></a>Dienst, Projekt, Aufgabenbereinigung

Wenn Sie DMS-Aufgaben, -Projekte oder -Dienste abbrechen oder löschen müssen, führen Sie den Abbruch in der folgenden Reihenfolge durch:

* Abbrechen einer ausgeführten Aufgabe
* Löschen der Aufgabe
* Löschen des Projekts
* Löschen des DMS-Diensts

1. Um eine momentan ausgeführte Aufgabe abzubrechen, verwenden Sie den folgenden Befehl:

    ```azurecli
    az dms project task cancel --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask
     ```

2. Um eine momentan ausgeführte Aufgabe zu löschen, verwenden Sie den folgenden Befehl:
    ```azurecli
    az dms project task delete --service-name PostgresCLI --project-name PGMigration --resource-group PostgresDemo --name runnowtask
    ```

3. Um ein momentan ausgeführtes Projekt abzubrechen, verwenden Sie den folgenden Befehl:
     ```azurecli
    az dms project task cancel -n runnowtask --project-name PGMigration -g PostgresDemo --service-name PostgresCLI
     ```

4. Um ein momentan ausgeführtes Projekt zu löschen, verwenden Sie den folgenden Befehl:
    ```azurecli
    az dms project task delete -n runnowtask --project-name PGMigration -g PostgresDemo --service-name PostgresCLI
    ```

5. Um einen DMS-Dienst zu löschen, verwenden Sie den folgenden Befehl:

     ```azurecli
    az dms delete -g ProgresDemo -n PostgresCLI
     ```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu bekannten Problemen und Einschränkungen beim Ausführen der Onlinemigration zu Azure Database for PostgreSQL finden Sie im Artikel [Bekannte Probleme/Migrationseinschränkungen bei der Onlinemigration zu Azure Database for PostgreSQL](known-issues-azure-postgresql-online.md).
* Informationen zu Azure Database Migration Service finden Sie im Artikel [Was ist Azure Database Migration Service?](./dms-overview.md).
* Weitere Informationen zu Azure Database for PostgreSQL finden Sie im Artikel [Was ist Azure Database for PostgreSQL?](../postgresql/overview.md).