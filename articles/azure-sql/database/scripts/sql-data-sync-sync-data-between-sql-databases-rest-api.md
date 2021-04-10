---
title: 'REST-API: Synchronisieren zwischen mehreren Datenbanken'
description: Verwenden Sie ein REST-API-Beispielskript, um Daten zwischen mehreren Datenbanken zu synchronisieren.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: sqldbrb=1
ms.devlang: REST API
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: d3ff8114c11b224a0bdbb0bd2d0e5686a7e57b55
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105565902"
---
# <a name="use-rest-api-to-sync-data-between-multiple-databases"></a>Verwenden der REST-API zum Synchronisieren von Daten zwischen mehreren Datenbanken 

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

In diesem REST-API-Beispiel wird die SQL-Datensynchronisierung für die Synchronisierung von Daten zwischen mehreren Datenbanken konfiguriert.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Was ist SQL-Datensynchronisierung für Azure?](../sql-data-sync-data-sql-server-sql-database.md).

> [!IMPORTANT]
> SQL-Datensynchronisierung unterstützt Azure SQL Managed Instance derzeit nicht.

## <a name="create-sync-group"></a>Erstellen der Synchronisierungsgruppe

Verwenden Sie die [Vorlage zum Erstellen oder Aktualisieren](/rest/api/sql/syncgroups/createorupdate), um eine Synchronisierungsgruppe zu erstellen.
 
Übergeben Sie beim Erstellen einer Synchronisierungsgruppe weder das Synchronisierungsschema (Tabelle\Spalte) noch „masterSyncMemberName“, da die Synchronisierungsgruppe zu diesem Zeitpunkt noch nicht über Tabellen-\Spalteninformationen verfügt.

Beispielanforderung zum Erstellen einer Synchronisierungsgruppe: 

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187?api-version=2015-05-01-preview
```

```json
{
  "properties": {
    "interval": -1,
    "lastSyncTime": "0001-01-01T08:00:00Z",
    "conflictResolutionPolicy": "HubWin",
    "syncDatabaseId": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328",
    "hubDatabaseUserName": "hubUser"
  }
}
```

Beispielantwort beim Erstellen einer Synchronisierungsgruppe:

Statuscode: 200
```json
{
  "properties": {
    "interval": -1,
    "lastSyncTime": "0001-01-01T08:00:00Z",
    "conflictResolutionPolicy": "HubWin",
    "syncDatabaseId": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328",
    "hubDatabaseUserName": "hubUser",
    "syncState": "NotReady"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187",
  "name": "syncgroupcrud-3187",
  "type": "Microsoft.Sql/servers/databases/syncGroups"
}
```

Statuscode: 201
```json
{
  "properties": {
    "interval": -1,
    "lastSyncTime": "0001-01-01T08:00:00Z",
    "conflictResolutionPolicy": "HubWin",
    "syncDatabaseId": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328",
    "hubDatabaseUserName": "hubUser",
    "syncState": "NotReady"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187",
  "name": "syncgroupcrud-3187",
  "type": "Microsoft.Sql/servers/databases/syncGroups"
}
```

## <a name="create-sync-member"></a>Erstellen eines Synchronisierungsmitglieds

Verwenden Sie die [Vorlage zum Erstellen oder Aktualisieren](/rest/api/sql/syncmembers/createorupdate), um ein Synchronisierungsmitglied zu erstellen.

Beispielanforderung zum Erstellen eines Synchronisierungsmitglieds:

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/syncMembers/syncgroupcrud-4879?api-version=2015-05-01-preview
```

```json
{
  "properties": {
    "databaseType": "AzureSqlDatabase",
    "serverName": "syncgroupcrud-3379.database.windows.net",
    "databaseName": "syncgroupcrud-7421",
    "userName": "myUser",
    "syncDirection": "Bidirectional",
    "syncState": "UnProvisioned"
  }
}
```
Beispielantwort beim Erstellen eines Synchronisierungsmitglieds:

Statuscode: 200
```json
{
  "properties": {
    "databaseType": "AzureSqlDatabase",
    "serverName": "syncgroupcrud-3379.database.windows.net",
    "databaseName": "syncgroupcrud-7421",
    "userName": "myUser",
    "syncDirection": "Bidirectional",
    "syncState": "UnProvisioned"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/syncMembers/syncgroupcrud-4879",
  "name": "syncgroupcrud-4879",
  "type": "Microsoft.Sql/servers/databases/syncGroups/syncMembers"
}
```

Statuscode: 201
```json
{
  "properties": {
    "databaseType": "AzureSqlDatabase",
    "serverName": "syncgroupcrud-3379.database.windows.net",
    "databaseName": "syncgroupcrud-7421",
    "userName": "myUser",
    "syncDirection": "Bidirectional",
    "syncState": "UnProvisioned"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/syncMembers/syncgroupcrud-4879",
  "name": "syncgroupcrud-4879",
  "type": "Microsoft.Sql/servers/databases/syncGroups/syncMembers"
}
```

## <a name="refresh-schema"></a>Aktualisieren des Schemas

Aktualisieren Sie nach erfolgreicher Erstellung der Synchronisierungsgruppe das Schema mithilfe der folgenden Vorlagen.

Verwenden Sie die [Vorlage zum Aktualisieren des Hub-Schemas](/rest/api/sql/syncgroups/refreshhubschema), um das Schema für die Hub-Datenbank zu aktualisieren. 

Beispielanforderung zum Aktualisieren eines Hub-Datenbankschemas: 

```http
POST https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/refreshHubSchema?api-version=2015-05-01-preview
```

Beispielantwort beim Aktualisieren eines Hub-Datenbankschemas: 

Statuscode: 200

Statuscode: 202

Verwenden Sie die [Vorlage zum Auflisten von Hub-Schemas](/rest/api/sql/syncgroups/listhubschemas), um das Hub-Datenbankschema aufzulisten. 

Verwenden Sie die [Vorlage zum Aktualisieren des Mitgliederschemas](/rest/api/sql/syncmembers/refreshmemberschema), um das Mitgliederdatenbankschema zu aktualisieren. 

Verwenden Sie die [Vorlage zum Auflisten des Mitgliederschemas](/rest/api/sql/syncmembers/listmemberschemas), um das Mitgliederdatenbankschema zu aufzulisten. 

Fahren Sie erst mit dem nächsten Schritt fort, wenn das Schema erfolgreich aktualisiert wurde. 

## <a name="update-sync-group"></a>Aktualisieren der Synchronisierungsgruppe 

Verwenden Sie die [Vorlage zum Erstellen oder Aktualisieren](/rest/api/sql/syncgroups/createorupdate), um Ihre Synchronisierungsgruppe zu aktualisieren.

Aktualisieren Sie die Synchronisierungsgruppe durch Angeben des Synchronisierungsschemas. Schließen Sie Ihr Schema sowie „masterSyncMemberName“ ein. Hierbei handelt es sich um den Namen, der das zu verwendende Schema enthält. 

Beispielanforderung zum Aktualisieren der Synchronisierungsgruppe: 

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187?api-version=2015-05-01-preview
```

```json
{
  "properties": {
    "interval": -1,
    "lastSyncTime": "0001-01-01T08:00:00Z",
    "conflictResolutionPolicy": "HubWin",
    "syncDatabaseId": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328",
    "hubDatabaseUserName": "hubUser"
  }
}
```

Beispielantwort beim Aktualisieren der Synchronisierungsgruppe: 

```json
{
  "properties": {
    "interval": -1,
    "lastSyncTime": "0001-01-01T08:00:00Z",
    "conflictResolutionPolicy": "HubWin",
    "syncDatabaseId": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328",
    "hubDatabaseUserName": "hubUser",
    "syncState": "NotReady"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187",
  "name": "syncgroupcrud-3187",
  "type": "Microsoft.Sql/servers/databases/syncGroups"
}
```

```json
{
  "properties": {
    "interval": -1,
    "lastSyncTime": "0001-01-01T08:00:00Z",
    "conflictResolutionPolicy": "HubWin",
    "syncDatabaseId": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328",
    "hubDatabaseUserName": "hubUser",
    "syncState": "NotReady"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-3521/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187",
  "name": "syncgroupcrud-3187",
  "type": "Microsoft.Sql/servers/databases/syncGroups"
}
```
## <a name="update-sync-member"></a>Aktualisieren des Synchronisierungsmitglieds

Verwenden Sie die [Vorlage zum Erstellen oder Aktualisieren](/rest/api/sql/syncmembers/createorupdate), um Ihr Synchronisierungsmitglied zu aktualisieren.

Beispielanforderung zum Aktualisieren eines Synchronisierungsmitglieds: 

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/syncMembers/syncgroupcrud-4879?api-version=2015-05-01-preview
```

```json
{
  "properties": {
    "databaseType": "AzureSqlDatabase",
    "serverName": "syncgroupcrud-3379.database.windows.net",
    "databaseName": "syncgroupcrud-7421",
    "userName": "myUser",
    "syncDirection": "Bidirectional",
    "syncState": "UnProvisioned"
  }
}
```

Beispielantwort beim Aktualisieren eines Synchronisierungsmitglieds: 

Statuscode: 200
```json
{
  "properties": {
    "databaseType": "AzureSqlDatabase",
    "serverName": "syncgroupcrud-3379.database.windows.net",
    "databaseName": "syncgroupcrud-7421",
    "userName": "myUser",
    "syncDirection": "Bidirectional",
    "syncState": "UnProvisioned"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/syncMembers/syncgroupcrud-4879",
  "name": "syncgroupcrud-4879",
  "type": "Microsoft.Sql/servers/databases/syncGroups/syncMembers"
}
```

Statuscode: 201
```json
{
  "properties": {
    "databaseType": "AzureSqlDatabase",
    "serverName": "syncgroupcrud-3379.database.windows.net",
    "databaseName": "syncgroupcrud-7421",
    "userName": "myUser",
    "syncDirection": "Bidirectional",
    "syncState": "UnProvisioned"
  },
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/syncMembers/syncgroupcrud-4879",
  "name": "syncgroupcrud-4879",
  "type": "Microsoft.Sql/servers/databases/syncGroups/syncMembers"
}
```

## <a name="trigger-sync"></a>Auslösen einer Synchronisierung

Verwenden Sie die [Vorlage zum Auslösen der Synchronisierung](/rest/api/sql/syncgroups/triggersync), um einen Synchronisierungsvorgang auszulösen.

Beispielanforderung zum Auslösen des Synchronisierungsvorgangs: 

```http
POST https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/syncgroupcrud-65440/providers/Microsoft.Sql/servers/syncgroupcrud-8475/databases/syncgroupcrud-4328/syncGroups/syncgroupcrud-3187/triggerSync?api-version=2015-05-01-preview
```

Beispielantwort beim Auslösen des Synchronisierungsvorgangs: 

Statuscode: 200

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](../powershell-script-content-guide.md).

Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

- Übersicht: [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung in Azure](../sql-data-sync-data-sql-server-sql-database.md)
- Einrichten der Datensynchronisierung
    - Verwenden des Azure-Portals: [Tutorial: Einrichten der SQL-Datensynchronisierung zum Synchronisieren von Daten zwischen Azure SQL-Datenbank und SQL Server](../sql-data-sync-sql-server-configure.md)
    - Verwenden von PowerShell: [Verwenden von PowerShell zum Synchronisieren von Daten zwischen Azure SQL-Datenbank und SQL Server](sql-data-sync-sync-data-between-azure-onprem.md)
- Datensynchronisierungs-Agent: [Datensynchronisierungs-Agent für die SQL-Datensynchronisierung in Azure](../sql-data-sync-agent-overview.md)
- Bewährte Methoden: [Bewährte Methoden für die SQL-Datensynchronisierung in Azure](../sql-data-sync-best-practices.md)
- Überwachung: [Überwachen der SQL-Datensynchronisierung mit Azure Monitor-Protokollen](../monitor-tune-overview.md)
- Problembehandlung: [Behandeln von Problemen mit der SQL-Datensynchronisierung in Azure](../sql-data-sync-troubleshoot.md)
- Aktualisieren des Synchronisierungsschemas
    - Verwenden von Transact-SQL: [Automatisieren der Replikation von Schemaänderungen in der SQL-Datensynchronisierung in Azure](../sql-data-sync-update-sync-schema.md)
    - Verwenden von PowerShell: [Verwenden von PowerShell zum Aktualisieren des Synchronisierungsschemas in einer bestehenden Synchronisierungsgruppe](update-sync-schema-in-sync-group.md)

Weitere Informationen zu SQL-Datenbank finden Sie hier:

- [Übersicht über die SQL-Datenbank](../sql-database-paas-overview.md)
- [Datenbank-Lebenszyklusverwaltung](/previous-versions/sql/sql-server-guides/jj907294(v=sql.110))