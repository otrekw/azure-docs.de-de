---
title: Abfragen einer Datenbank mithilfe von Node.js
description: Hier erfahren Sie, wie Sie unter Verwendung von Node.js ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von T-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: c0da38a41bf613237ea3b164d70e4729a7284ca7
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76768605"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von Node.js

In dieser Schnellstartanleitung stellen Sie mithilfe von Node.js eine Verbindung mit einer Azure SQL-Datenbank her und fragen Daten mithilfe von T-SQL-Anweisungen ab.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine [Azure SQL-Datenbank](sql-database-single-database-get-started.md)
- [Node.js](https://nodejs.org)-bezogene Software

  # <a name="macostabmacos"></a>[macOS](#tab/macos)

  Installieren Sie Homebrew und Node.js sowie den ODBC-Treiber und SQLCMD mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Node.js-Apps mit SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).

  # <a name="ubuntutabubuntu"></a>[Ubuntu](#tab/ubuntu)

  Installieren Sie Node.js sowie den ODBC-Treiber und SQLCMD mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Node.js-Apps mit SQL Server unter Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).

  # <a name="windowstabwindows"></a>[Windows](#tab/windows)

  Installieren Sie Chocolatey und Node.js sowie den ODBC-Treiber und SQLCMD mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Node.js-Apps mit SQL Server unter Windows](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

  ---

> [!IMPORTANT]
> Die Skripts in diesem Artikel wurden für die Datenbank **Adventure Works** geschrieben.

> [!NOTE]
> Optional kann eine verwaltete Azure SQL-Instanz verwendet werden.
>
> Verwenden Sie zum Erstellen und Konfigurieren das [Azure-Portal](sql-database-managed-instance-get-started.md), [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) oder die [Befehlszeilenschnittstelle](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44), und richten Sie anschließend die Konnektivität ([lokal](sql-database-managed-instance-configure-p2s.md) oder [VM](sql-database-managed-instance-configure-vm.md)) ein.
>
> Informationen zum Laden von Daten finden Sie unter [Schnellstart: Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank](sql-database-import.md) (mit der Datei für [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)) bzw. unter [Schnellstart: Wiederherstellen einer Datenbank in einer verwalteten Instanz](sql-database-managed-instance-get-started-restore.md).

## <a name="get-sql-server-connection-information"></a>Abrufen von SQL Server-Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die Sie zum Herstellen einer Verbindung mit der Azure SQL-Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wechseln Sie zur Seite **SQL-Datenbanken** oder **Verwaltete SQL-Instanzen**.

3. Sehen Sie sich auf der Seite **Übersicht** bei einer Einzeldatenbank den vollqualifizierten Servernamen neben **Servername** und bei einer verwalteten Instanz den vollqualifizierten Servernamen neben **Host** an. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus. 

## <a name="create-the-project"></a>Erstellen des Projekts

Öffnen Sie eine Eingabeaufforderung, und erstellen Sie einen Ordner namens *sqltest*. Öffnen Sie den erstellten Ordner, und führen Sie den folgenden Befehl aus:

  ```bash
  npm init -y
  npm install tedious
  ```

## <a name="add-code-to-query-database"></a>Hinzufügen von Code zum Abfragen der Datenbank

1. Erstellen Sie in Ihrem bevorzugten Text-Editor eine neue Datei namens *sqltest.js*.

1. Ersetzen Sie ihren Inhalt durch den folgenden Code. Fügen Sie anschließend die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

    ```js
    const { Connection, Request } = require("tedious");

    // Create connection to database
    const config = {
      authentication: {
        options: {
          userName: "username", // update me
          password: "password" // update me
        },
        type: "default"
      },
      server: "your_server.database.windows.net", // update me
      options: {
        database: "your_database", //update me
        encrypt: true
      }
    };

    const connection = new Connection(config);

    // Attempt to connect and execute queries if connection goes through
    connection.on("connect", err => {
      if (err) {
        console.error(err.message);
      } else {
        queryDatabase();
      }
    });

    function queryDatabase() {
      console.log("Reading rows from the Table...");

      // Read all rows from table
      const request = new Request(
        `SELECT TOP 20 pc.Name as CategoryName,
                       p.name as ProductName
         FROM [SalesLT].[ProductCategory] pc
         JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid`,
        (err, rowCount) => {
          if (err) {
            console.error(err.message);
          } else {
            console.log(`${rowCount} row(s) returned`);
          }
        }
      );

      request.on("row", columns => {
        columns.forEach(column => {
          console.log("%s\t%s", column.metadata.colName, column.value);
        });
      });
      
      connection.execSql(request);
    }
    ```

> [!NOTE]
> Im Codebeispiel wird die Beispieldatenbank **AdventureWorksLT** für Azure SQL verwendet.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie das Programm an der Eingabeaufforderung aus.

    ```bash
    node sqltest.js
    ```

1. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte

- [Microsoft Node.js Driver for SQL Server](/sql/connect/node-js/node-js-driver-for-sql-server) (Microsoft Node.js-Treiber für SQL Server)

- Herstellen einer Verbindung und Abfragen unter Windows/Linux/macOS mit [.NET Core](sql-database-connect-query-dotnet-core.md), [Visual Studio Code](sql-database-connect-query-vscode.md) oder [SSMS](sql-database-connect-query-ssms.md) (nur Windows)

- [Erste Schritte mit .NET Core unter Windows/Linux/Mac OS unter Verwendung der Befehlszeile](/dotnet/core/tutorials/using-with-xplat-cli)

- Entwerfen Ihrer ersten Azure SQL-Datenbank mit [.NET](sql-database-design-first-database-csharp.md) oder [SSMS](sql-database-design-first-database.md)
