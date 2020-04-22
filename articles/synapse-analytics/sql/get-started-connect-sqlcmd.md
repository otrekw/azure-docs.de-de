---
title: Herstellen einer Verbindung mit Synapse SQL mithilfe von sqlcmd
description: Verwenden Sie das Befehlszeilenprogramm sqlcmd, um eine Verbindung mit SQL On-Demand (Vorschauversion) und einem SQL-Pool herzustellen und Abfragen durchzuführen.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 8ff9034e6c31c8d95e862570e3962990dfec8442
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419654"
---
# <a name="connect-to-synapse-sql-with-sqlcmd"></a>Herstellen einer Verbindung mit Synapse SQL mithilfe von sqlcmd

> [!div class="op_single_selector"]
>
> * [Azure Data Studio (Vorschauversion)](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

Sie können das Befehlszeilenprogramm [sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) verwenden, um eine Verbindung mit SQL On-Demand (Vorschauversion) und einem SQL-Pool in Synapse SQL herzustellen und Abfragen durchzuführen.  

## <a name="1-connect"></a>1. Verbinden
Öffnen Sie zur Verwendung von [sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) die Eingabeaufforderung, und geben Sie **sqlcmd** gefolgt von der Verbindungszeichenfolge für Ihre Synapse SQL-Datenbank ein. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

* **Server (-S):** Server in Form von `<`Servername`>`.database.windows.net
* **Datenbank (-d):** Datenbankname
* **Bezeichner in Anführungszeichen aktivieren (-I):** Bezeichner in Anführungszeichen müssen aktiviert sein, um eine Verbindung mit einer Synapse SQL-Instanz herzustellen.

Zur Verwendung der SQL Server-Authentifizierung müssen Sie die Parameter für Benutzername und Kennwort hinzufügen:

* **Benutzer (-U):** Serverbenutzer im Format `<`Benutzer`>`
* **Kennwort (-P):** Das Kennwort des Benutzers

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

**SQL On-Demand**

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
```

**SQL-Pool**

```
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Um die in Azure Active Directory integrierte Authentifizierung verwenden zu können, müssen Sie die Azure Active Directory-Parameter hinzufügen:

* **Azure Active Directory-Authentifizierung (-G):** Azure Active Directory für die Authentifizierung verwenden

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

**SQL On-Demand**

```
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -G -I
```

**SQL-Pool**

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Sie müssen [Azure Active Directory-Authentifizierung aktivieren](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) , um die Authentifizierung mithilfe von Active Directory zu ermöglichen.

## <a name="2-query"></a>2. Abfrage

### <a name="use-sql-pool"></a>Verwenden eines SQL-Pools

Nach dem Herstellen der Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen für die Instanz ausgeben.  In diesem Beispiel werden Abfragen im interaktiven Modus gesendet:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Bei einem SQL-Pool zeigen die folgenden Beispiele, wie Sie Abfragen im Batchmodus ausführen, indem Sie die Option „-Q“ verwenden oder Ihren SQL-Code an „sqlcmd“ übergeben:

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

### <a name="use-sql-on-demand"></a>Verwenden von SQL On-Demand

Nach dem Herstellen der Verbindung können Sie alle unterstützten [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Anweisungen (T-SQL) für die Instanz ausgeben.  Im folgenden Beispiel werden Abfragen im interaktiven Modus gesendet:

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
1> SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')
2> GO
3> QUIT
```

Bei SQL On-Demand zeigen die folgenden Beispiele, wie Sie Abfragen im Batchmodus ausführen, indem Sie die Option „-Q“ verwenden oder Ihren SQL-Code an „sqlcmd“ übergeben:

```sql
sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I -Q "SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')"
```

```sql
"SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')" | sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I > ./tables.out
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu sqlcmd-Optionen finden Sie in der [Dokumentation zu sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).
