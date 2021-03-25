---
title: Herstellen einer Verbindung mit sqlcmd
description: Verwenden Sie das Befehlszeilenprogramm sqlcmd, um in Azure Synapse Analytics eine Verbindung mit einem dedizierten SQL-Pool herzustellen und Abfragen durchzuführen.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f8b4d54585bc70c3ee5f24846e216f75e985cf84
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101675735"
---
# <a name="connect-to-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sqlcmd"></a>Herstellen einer Verbindung mit einem dedizierten SQL-Pool in Azure Synapse Analytics mit sqlcmd

> [!div class="op_single_selector"]
>
> * [Power BI](/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

Verwenden Sie das Befehlszeilenprogramm [sqlcmd][sqlcmd], um eine Verbindung mit einem dedizierten SQL-Pool herzustellen und Abfragen durchzuführen.  

## <a name="1-connect"></a>1. Verbinden

Öffnen Sie zur Verwendung von [sqlcmd][sqlcmd] die Eingabeaufforderung, und geben Sie **sqlcmd** gefolgt von der Verbindungszeichenfolge für Ihren dedizierten SQL-Pool ein. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

* **Server (-S):** Server in Form von `<`Servername`>`.database.windows.net
* **Datenbank (-d):** Name des dedizierten SQL-Pools.
* **Bezeichner in Anführungszeichen aktivieren (-I):** Bezeichner in Anführungszeichen müssen aktiviert sein, um eine Verbindung mit einer dedizierten SQL-Pool-Instanz herzustellen.

Zur Verwendung der SQL Server-Authentifizierung müssen Sie die Parameter für Benutzername und Kennwort hinzufügen:

* **Benutzer (-U):** Serverbenutzer im Format `<`Benutzer`>`
* **Kennwort (-P):** Das Kennwort des Benutzers.

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Um die in Azure Active Directory integrierte Authentifizierung verwenden zu können, müssen Sie die Azure Active Directory-Parameter hinzufügen:

* **Azure Active Directory-Authentifizierung (-G):** Azure Active Directory für die Authentifizierung verwenden

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Sie müssen [Azure Active Directory-Authentifizierung aktivieren](sql-data-warehouse-authentication.md) , um die Authentifizierung mithilfe von Active Directory zu ermöglichen.

## <a name="2-query"></a>2. Abfrage

Nach dem Herstellen der Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen für die Instanz ausgeben.  In diesem Beispiel werden Abfragen im interaktiven Modus gesendet.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Die nächsten Beispiele zeigen, wie Sie Ihre Abfragen im Batchmodus ausführen können, indem Sie die Option „-Q“ oder für den SQL-Code ein Pipe-Zeichen zur Verknüpfung mit „sqlcmd“ verwenden.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Nächste Schritte

Ausführlichere Informationen zu den in sqlcmd verfügbaren Optionen finden Sie in der [Dokumentation zu sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true).