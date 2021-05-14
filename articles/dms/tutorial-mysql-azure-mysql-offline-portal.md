---
title: 'Tutorial: Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mithilfe von DMS'
titleSuffix: Azure Database Migration Service
description: Hier erfahren Sie, wie Sie mit Azure Database Migration Service eine Offlinemigration von einer lokalen MySQL-Instanz zu Azure Database for MySQL durchführen.
services: dms
author: sumitgaurin
ms.author: sgaur
manager: arthiaga
ms.reviewer: arthiaga
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 04/11/2021
ms.openlocfilehash: d1a8cc9a615474685222d0339ec948401fb8cdff
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108128008"
---
# <a name="tutorial-migrate-mysql-to-azure-database-for-mysql-offline-using-dms"></a>Tutorial: Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mithilfe von DMS

Sie können Azure Database Migration Service verwenden, um per Hochgeschwindigkeits-Datenmigrationsfunktion eine einmalige vollständige Datenbankmigration einer lokalen MySQL-Instanz zu [Azure Database for MySQL](../mysql/index.yml) durchzuführen. In diesem Tutorial wird eine Beispieldatenbank unter Verwendung der Offlinemigrationsaktivität in Azure Database Migration Service von einer lokalen Instanz von MySQL 5.7 zu Azure Database for MySQL (v5.7) migriert. In dem Artikel wird zwar davon ausgegangen, dass die Quelle eine MySQL-Datenbankinstanz ist und es sich beim Ziel um Azure Database for MySQL handelt, er kann jedoch auch für die Migration zwischen Azure Database for MySQL-Instanzen verwendet werden. Hierzu müssen lediglich der Name und die Anmeldeinformationen des Quellservers geändert werden. Darüber hinaus wird auch die Migration von MySQL-Servern mit niedrigerer Version (ab v5.6) zu höheren Versionen unterstützt.

> [!IMPORTANT]
> Für Onlinemigrationen können Sie Open-Source-Tools wie [MyDumper/MyLoader](https://centminmod.com/mydumper.html) mit [Datenreplikation](/azure/mysql/concepts-data-in-replication) verwenden. 

[!INCLUDE [preview features callout](../../includes/dms-boilerplate-preview.md)]

> [!NOTE]
> Eine PowerShell-basierte skriptfähige Version dieser Migration finden Sie unter [Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mit PowerShell und Azure Database Migration Service](https://docs.microsoft.com/azure/dms/migrate-mysql-to-azure-mysql-powershell).

> [!NOTE]
> Amazon Relational Database Service (RDS) für MySQL und Amazon Aurora (MySQL-basiert) werden ebenfalls als Quellen für die Migration unterstützt.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
>
> * Migrieren des Datenbankschemas mit dem Hilfsprogramm „mysqldump“
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen eines Migrationsprojekts mithilfe von Azure Database Migration Service
> * Ausführen der Migration
> * Überwachen der Migration

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free).
* Eine lokale MySQL-Datenbank mit der Version 5.7. Laden Sie bei Bedarf die Version 5.7 von [MySQL Community Edition](https://dev.mysql.com/downloads/mysql/) herunter, und installieren Sie sie.
* [Erstellen einer Instanz in Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-portal.md) Im Artikel [Azure-Datenbank für MySQL: Verwenden von MySQL Workbench zum Verbinden und Abfragen von Daten](../mysql/connect-workbench.md) finden Sie ausführliche Informationen zur Verbindungsherstellung sowie zum Erstellen einer Datenbank mithilfe der Workbench-Anwendung. Die Version von Azure Database for MySQL muss mindestens der lokalen MySQL-Version entsprechen. MySQL 5.7 kann beispielsweise zu Azure Database for MySQL 5.7 migriert oder auf die Version 8 aktualisiert werden. 
* Erstellen Sie ein Microsoft Azure Virtual Network für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das Site-to-Site-Konnektivität für Ihre lokalen Quellserver entweder über [ExpressRoute](../expressroute/expressroute-introduction.md) oder über [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) bereitstellt. Weitere Informationen zum Erstellen eines virtuellen Netzwerks finden Sie in der [Dokumentation zu Virtual Network](../virtual-network/index.yml) und insbesondere in den Schnellstartartikeln mit Schritt-für-Schritt-Anleitungen.

    > [!NOTE]
    > Fügen Sie bei Verwendung von ExpressRoute mit Netzwerkpeering zu Microsoft während des Setups des virtuellen Netzwerks die folgenden [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) zu dem Subnetz hinzu, in dem der Dienst bereitgestellt werden soll:
    >
    > * Zieldatenbankendpunkt (z. B. SQL-Endpunkt, Cosmos DB-Endpunkt usw.)
    > * Speicherendpunkt
    > * Service Bus-Endpunkt
    >
    > Diese Konfiguration ist erforderlich, weil Azure Database Migration Service über keine Internetverbindung verfügt.

* Stellen Sie sicher, dass die NSG-Regeln (Netzwerksicherheitsgruppen) des virtuellen Netzwerks nicht den ausgehenden Port 443 von ServiceTag für ServiceBus, Storage und AzureMonitor blockieren. Ausführlichere Informationen zur NSG-Datenverkehrsfilterung in einem virtuellen Netzwerk finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Öffnen Sie Ihre Windows-Firewall, um Verbindungen aus dem virtuellen Netzwerk für Azure Database Migration Service zuzulassen und den Zugriff auf den MySQL-Quellserver zu ermöglichen (standardmäßig TCP-Port 3306).
* Wenn Sie eine Firewallappliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, um Verbindungen aus dem virtuellen Netzwerk für Azure Database Migration Service zuzulassen und den Zugriff auf die Quelldatenbanken für die Migration zu ermöglichen.
* Erstellen Sie für die Azure Database for MySQL-Zielinstanz eine [Firewallregel](../azure-sql/database/firewall-configure.md) auf Serverebene, oder [konfigurieren Sie VNet-Dienstendpunkte](../mysql/howto-manage-vnet-using-portal.md), um den Zugriff auf die Zieldatenbanken durch das virtuelle Netzwerk für Azure Database Migration Service zu ermöglichen.
* Die MySQL-Quellinstanz muss unter einer unterstützten Version von MySQL Community Edition ausgeführt werden. Führen Sie zum Ermitteln der Version der MySQL-Instanz im MySQL-Hilfsprogramm oder in MySQL Workbench den folgenden Befehl aus:

    ```
    SELECT @@version;
    ```

* Azure Database for MySQL unterstützt nur InnoDB-Tabellen. Informationen zum Konvertieren von MyISAM-Tabellen in InnoDB finden Sie im Artikel [Converting Tables from MyISAM to InnoDB](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) (Konvertieren von Tabellen von MyISAM zu InnoDB).
* Der Benutzer muss über Berechtigungen zum Lesen von Daten aus der Quelldatenbank verfügen.

## <a name="migrate-database-schema"></a>Migrieren des Datenbankschemas

Um alle Datenbankobjekte wie Tabellenschemas, Indizes und gespeicherte Prozeduren zu übertragen, muss das Schema aus der Quelldatenbank extrahiert und auf die Zieldatenbank angewendet werden. Zum Extrahieren des Schemas können Sie „mysqldump“ mit dem Parameter `--no-data` verwenden. Hierzu benötigen Sie einen Computer, der sowohl eine Verbindung mit der MySQL-Quelldatenbank als auch eine Verbindung mit der Azure Database for MySQL-Zieldatenbank herstellen kann.

Führen Sie den folgenden Befehl aus, um das Schema mit „mysqldump“ zu exportieren:

```
mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
```

Beispiel:

```
mysqldump -h 10.10.123.123 -u root -p --databases migtestdb --no-data > d:\migtestdb.sql
```

Führen Sie den folgenden Befehl aus, um das Schema in die Azure Database for MySQL-Zielinstanz zu importieren:

```
mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
 ```

Beispiel:

```
mysql.exe -h mysqlsstrgt.mysql.database.azure.com -u docadmin@mysqlsstrgt -p migtestdb < d:\migtestdb.sql
 ```

Wenn Ihr Schema Fremdschlüssel enthält, wird das parallele Laden von Daten während der Migration von der Migrationsaufgabe übernommen. Fremdschlüssel müssen bei der Schemamigration nicht entfernt werden.

Falls die Datenbank Trigger enthält, wird vor der vollständigen Migration der Daten aus der Quelldatenbank die Datenintegrität in der Zieldatenbank erzwungen. Es wird empfohlen, während der Migration die Trigger in allen Tabellen der Zieldatenbank zu deaktivieren und die Trigger nach der Migration wieder zu aktivieren.

Führen Sie in MySQL Workbench das folgende Skript für die Zieldatenbank aus, um das Skript zum Entfernen von Triggern und das Skript zum Hinzufügen von Triggern zu extrahieren:

```sql
SELECT
    SchemaName,
    GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery,
    Concat('DELIMITER $$ \n\n', GROUP_CONCAT(AddQuery SEPARATOR '$$\n'), '$$\n\nDELIMITER ;') as AddQuery
FROM
(
SELECT 
    TRIGGER_SCHEMA as SchemaName,
    Concat('DROP TRIGGER `', TRIGGER_NAME, "`") as DropQuery,
    Concat('CREATE TRIGGER `', TRIGGER_NAME, '` ', ACTION_TIMING, ' ', EVENT_MANIPULATION, 
            '\nON `', EVENT_OBJECT_TABLE, '`\n' , 'FOR EACH ', ACTION_ORIENTATION, ' ',
            ACTION_STATEMENT) as AddQuery
FROM  
    INFORMATION_SCHEMA.TRIGGERS
ORDER BY EVENT_OBJECT_SCHEMA, EVENT_OBJECT_TABLE, ACTION_TIMING, EVENT_MANIPULATION, ACTION_ORDER ASC
) AS Queries
GROUP BY SchemaName
```

Führen Sie die generierte Abfrage zum Entfernen von Triggern (Spalte „DropQuery“) im Ergebnis aus, um Trigger in der Zieldatenbank zu entfernen. Die Abfrage zum Hinzufügen von Triggern kann gespeichert und nach Abschluss der Datenmigration verwendet werden.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

Die Registrierung des Ressourcenanbieters muss für jedes Azure-Abonnement lediglich einmal durchgeführt werden. Ohne die Registrierung kann keine Instanz von **Azure Database Migration Service** erstellt werden.

1. Melden Sie sich beim Azure-Portal an, und klicken Sie auf **Alle Dienste** und anschließend auf **Abonnements**.

   ![Abonnements im Portal anzeigen](media/tutorial-mysql-to-azure-mysql-offline-portal/01-dms-portal-select-subscription.png)

2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie dann auf **Ressourcenanbieter**.

3. Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.

    ![Registrieren des Ressourcenanbieters](media/tutorial-mysql-to-azure-mysql-offline-portal/02-dms-portal-register-rp.png)

## <a name="create-a-database-migration-service-instance"></a>Erstellen einer Database Migration Service-Instanz

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.

    ![Azure Marketplace](media/tutorial-mysql-to-azure-mysql-offline-portal/03-dms-portal-marketplace.png)

2. Wählen Sie auf dem Bildschirm **Azure Database Migration Service** die Schaltfläche **Erstellen** aus.

    ![Erstellen einer Instanz von Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/04-dms-portal-marketplace-create.png)
  
3. Geben Sie auf dem Bildschirm **Migrationsdienst erstellen** einen Namen für den Dienst, das Abonnement und eine neue oder vorhandene Ressourcengruppe an.

4. Wählen Sie einen Tarif aus, und wechseln Sie zum Bildschirm „Netzwerk“. Die Offlinemigrationsfunktion steht sowohl im Standard- als auch im Premium-Tarif zur Verfügung.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).

    ![Konfigurieren der Grundeinstellungen von Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/05-dms-portal-create-basic.png)

5. Wählen Sie ein bereits vorhandenes virtuelles Netzwerk aus der Liste aus, oder geben Sie einen Namen für ein neu zu erstellendes virtuelles Netzwerk an. Wechseln Sie zum Bildschirm „Überprüfen und erstellen“. Auf dem Bildschirm „Tags“ können Sie dem Dienst optional Tags hinzufügen.

    Das virtuelle Netzwerk erteilt Azure Database Migration Service Zugriff auf die SQL Server-Quellinstanz und die Azure SQL-Datenbank-Zielinstanz.

    ![Konfigurieren der Netzwerkeinstellungen von Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/06-dms-portal-create-networking.png)

    Weitere Informationen zum Erstellen eines virtuellen Netzwerks im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure Portal](../virtual-network/quick-create-portal.md).

6. Überprüfen Sie die Konfiguration, und wählen Sie **Erstellen** aus, um den Dienst zu erstellen.
    
    ![Azure Database Migration Service: Erstellen](media/tutorial-mysql-to-azure-mysql-offline-portal/07-dms-portal-create-submit.png)

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts

Nachdem der Dienst erstellt wurde, suchen Sie diesen im Azure-Portal, öffnen Sie ihn, und erstellen Sie anschließend ein neues Migrationsprojekt.  

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.

    ![Suchen aller Instanzen von Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/08-01-dms-portal-search-service.png)

2. Wählen Sie in den Suchergebnissen Ihre Migrationsdienstinstanz und anschließend **+ Neues Migrationsprojekt** aus.
    
    ![Erstellen eines neuen Migrationsprojekts](media/tutorial-mysql-to-azure-mysql-offline-portal/08-02-dms-portal-new-project.png)

3. Geben Sie auf dem Bildschirm **Neues Migrationsprojekt** einen Namen für das Projekt an, und wählen Sie im Auswahlfeld **Typ des Quellservers** die Option **MySQL**, im Auswahlfeld **Typ des Zielservers** die Option **Azure Database for MySQL** und im Auswahlfeld für die **Art der Migrationsaktivität** die Option **\[Datenmigration \]Vorschau** aus. Wählen Sie **Aktivität erstellen und ausführen** aus.

    ![Erstellen eines Database Migration Service-Projekts](media/tutorial-mysql-to-azure-mysql-offline-portal/09-dms-portal-project-mysql-create.png)

    > [!NOTE]
    > Alternativ können Sie **Nur Projekt erstellen** auswählen, um das Migrationsprojekt jetzt zu erstellen und die Migration später durchzuführen.

## <a name="configure-migration-project"></a>Konfigurieren des Migrationsprojekts

1. Geben Sie auf dem Bildschirm **Quelle auswählen** die Verbindungsdetails für die MySQL-Quellinstanz an, und wählen Sie anschließend **Weiter: Ziel auswählen>>** aus.

    ![Bildschirm zum Hinzufügen der Quelldetails](media/tutorial-mysql-to-azure-mysql-offline-portal/10-dms-portal-project-mysql-source.png)

2. Geben Sie auf dem Bildschirm **Ziel auswählen** die Verbindungsdetails für die Azure Database for MySQL-Zielinstanz an, und wählen Sie anschließend **Weiter: Datenbanken auswählen>>** aus.

    ![Bildschirm zum Hinzufügen der Zieldetails](media/tutorial-mysql-to-azure-mysql-offline-portal/11-dms-portal-project-mysql-target.png)

3. Ordnen Sie auf dem Bildschirm **Datenbanken auswählen** die Quell- und die Zieldatenbank für die Migration zu, und wählen Sie anschließend **Weiter: Migrationseinstellungen konfigurieren>>** aus. Sie können die Option **Make Source Server Read Only** (Quellserver als schreibgeschützt festlegen) auswählen, um die Quelle als schreibgeschützt festzulegen. Beachten Sie jedoch, dass es sich hierbei um eine Einstellung auf Serverebene handelt. Wenn diese Option ausgewählt ist, wird der gesamte Server schreibgeschützt, nicht nur die ausgewählten Datenbanken.
    
    Wenn die Zieldatenbank denselben Datenbanknamen wie die Quelldatenbank enthält, wählt Azure Database Migration Service die Zieldatenbank standardmäßig aus.
    ![Bildschirm zum Auswählen der Datenbankdetails](media/tutorial-mysql-to-azure-mysql-offline-portal/12-dms-portal-project-mysql-select-db.png)
    
    > [!NOTE] 
    > Sie können in diesem Schritt zwar mehrere Datenbanken auswählen, es gibt jedoch Einschränkungen bei der Anzahl von Datenbanken, die auf diese Weise migriert werden können, sowie bei der Geschwindigkeit, mit der diese Datenbanken migriert werden, da die Compute-Ressourcen von den Datenbanken gemeinsam genutzt werden. In der Standardkonfiguration der Premium-SKU wird bei jeder Migrationsaufgabe versucht, zwei Tabellen parallel zu migrieren. Diese Tabellen können aus beliebigen der ausgewählten Datenbanken stammen. Sollte dieses Verfahren nicht schnell genug sein, können Sie Datenbankmigrationsaktivitäten auf verschiedene Migrationsaufgaben aufteilen und über mehrere Dienste hinweg skalieren. Darüber hinaus besteht eine Beschränkung auf zehn Instanzen von Azure Database Migration Service pro Abonnement und Region.
    > Informationen zur präziseren Steuerung des Migrationsdurchsatzes und der Parallelisierung finden Sie im Artikel [Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mit PowerShell und Azure Database Migration Service](./migrate-mysql-to-azure-mysql-powershell.md).

4. Wählen Sie auf dem Bildschirm **Migrationseinstellungen konfigurieren** die Tabellen aus, die in die Migration einbezogen werden sollen, und wählen Sie **Weiter: Zusammenfassung>>** aus. Wenn die Zieltabellen Daten enthalten, werden sie nicht standardmäßig ausgewählt. Sie können aber explizit ausgewählt werden. In diesem Fall werden sie dann vor Beginn der Migration abgeschnitten.

    ![Bildschirm zum Auswählen der Tabellen](media/tutorial-mysql-to-azure-mysql-offline-portal/13-dms-portal-project-mysql-select-tbl.png)

5. Geben Sie auf dem Bildschirm **Zusammenfassung** im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität ein, und überprüfen Sie anschließend die Zusammenfassung, um sicherzustellen, dass die Ziel- und Quelldetails Ihren vorherigen Angaben entsprechen.

    ![Zusammenfassung des Migrationsprojekts](media/tutorial-mysql-to-azure-mysql-offline-portal/14-dms-portal-project-mysql-activity-summary.png)

6. Wählen Sie **Migration starten** aus. Das Fenster „Migrationsaktivität“ wird angezeigt, und der **Status** der Aktivität lautet **Initialisierung**. Der **Status** ändert sich in **Wird ausgeführt**, wenn die Tabellenmigrationen beginnen.
 
     ![Aktive Migration](media/tutorial-mysql-to-azure-mysql-offline-portal/15-dms-portal-project-mysql-running.png)

## <a name="monitor-the-migration"></a>Überwachen der Migration

1. Wählen Sie auf dem Bildschirm für die Migrationsaktivität die Option **Aktualisieren** aus, um die Anzeige zu aktualisieren und den Fortschritt in Form der Anzahl abgeschlossener Tabellen anzuzeigen. 

2. Auf dem Aktivitätsbildschirm können Sie auf den Datenbanknamen klicken, um den Status der einzelnen Tabellen anzuzeigen, während diese migriert werden. Wählen Sie zum Aktualisieren der Anzeige die Option **Aktualisieren** aus. 

     ![Migrationsüberwachung](media/tutorial-mysql-to-azure-mysql-offline-portal/16-dms-portal-project-mysql-monitor.png)

## <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Klicken Sie auf dem Bildschirm „Migrationsaktivität“ auf **Aktualisieren**, um die Anzeige zu aktualisieren, bis der **Status** der Migration **Abgeschlossen** lautet.

    ![Abschließen der Migration](media/tutorial-mysql-to-azure-mysql-offline-portal/17-dms-portal-project-mysql-complete.png)

## <a name="post-migration-activities"></a>Aktivitäten nach der Migration

Die Migrationsübernahme in einer Offlinemigration ist ein anwendungsabhängiger Prozess und wird in diesem Dokument nicht behandelt. Nach der Migration sind jedoch folgende Aktivitäten erforderlich:

1. Erstellen Sie Anmeldungen, Rollen und Berechtigungen gemäß den Anwendungsanforderungen.
2. Erstellen Sie alle Trigger in der Zieldatenbank neu, die im Rahmen des Schritts vor der Migration extrahiert wurden.
3. Testen Sie zur Zertifizierung der Migration die Integrität der Anwendung mit der Zieldatenbank. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Database Migration Service nicht mehr benötigen, können Sie den Dienst wie folgt löschen:

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.

    ![Suchen aller DMS-Instanzen](media/tutorial-mysql-to-azure-mysql-offline-portal/08-01-dms-portal-search-service.png)

2. Wählen Sie in den Suchergebnissen Ihre Migrationsdienstinstanz und anschließend **Dienst löschen** aus.
    
    ![Löschen des Migrationsdiensts](media/tutorial-mysql-to-azure-mysql-offline-portal/18-dms-portal-delete.png)

3. Geben Sie im Bestätigungsdialogfeld den Namen des Diensts in das Textfeld **TYPE THE DATABASE MIGRATION SERVICE NAME** (NAME DER DATABASE MIGRATION SERVICE-INSTANZ EINGEBEN) ein, und wählen Sie **Löschen** aus.

    ![Bestätigen des Löschens des Migrationsdiensts](media/tutorial-mysql-to-azure-mysql-offline-portal/19-dms-portal-deleteconfirm.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu bekannten Problemen und Einschränkungen bei Migrationen mit DMS finden Sie im Artikel [Troubleshooting von häufigen Problemen und Fehlern bei Azure Database Migration Service](./known-issues-troubleshooting-dms.md).
* Informationen zur Behandlung von Problemen mit der Quelldatenbankkonnektivität bei der Verwendung von DMS finden Sie im Artikel [Problembehandlung von DMS-Fehlern beim Herstellen einer Verbindung mit Quelldatenbanken](./known-issues-troubleshooting-dms-source-connectivity.md).
* Informationen zu Azure Database Migration Service finden Sie im Artikel [Was ist Azure Database Migration Service?](./dms-overview.md).
* Informationen zu Azure Database for MySQL finden Sie im Artikel [Was ist Azure-Datenbank für MySQL?](../mysql/overview.md).
* Eine Anleitung zur Verwendung von DMS über PowerShell finden Sie im Artikel [Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mit PowerShell und Azure Database Migration Service](./migrate-mysql-to-azure-mysql-powershell.md).
