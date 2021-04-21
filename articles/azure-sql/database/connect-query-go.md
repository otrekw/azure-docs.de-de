---
title: Verwenden von Go für Abfragen
description: Erstellen Sie mithilfe von Go ein Programm, das eine Verbindung mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance herstellt und Abfragen ausführt.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: go
ms.topic: quickstart
author: David-Engel
ms.author: sstein
ms.reviewer: MightyPen
ms.date: 04/14/2021
ms.openlocfilehash: 1a479572ba8dbd68ccc072fce32446abcc9b873c
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107517779"
---
# <a name="quickstart-use-golang-to-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Schnellstart: Abfragen einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance mithilfe von Golang
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In dieser Schnellstartanleitung verwenden Sie die Programmiersprache [Golang](https://godoc.org/github.com/denisenkom/go-mssqldb), um eine Verbindung mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance herzustellen. Anschließend führen Sie Transact-SQL-Anweisungen aus, um Daten abzufragen und zu ändern. [Golang](https://golang.org/) ist eine Open-Source-Programmiersprache zur einfachen Erstellung von unkomplizierter, zuverlässiger und effizienter Software.  

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance Sie können eine der folgenden Schnellstartanleitungen verwenden, um eine Datenbank zu erstellen:

  || SQL-Datenbank | Verwaltete SQL-Instanz | SQL Server auf Azure-VMs |
  |:--- |:--- |:---|:---|
  | **Erstellen**| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  | **Erstellen** | [BEFEHLSZEILENSCHNITTSTELLE (CLI)](scripts/create-and-configure-database-cli.md) | [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  | **Erstellen** | [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | **Konfigurieren** | [IP-Firewallregel auf Serverebene](firewall-create-server-level-portal-quickstart.md)| [Verbindung von einem virtuellen Computer](../managed-instance/connect-vm-instance-configure.md)|
  | **Konfigurieren** ||[Verbindungen von lokalen Computern](../managed-instance/point-to-site-p2s-configure.md) | [Herstellen einer Verbindung mit einer SQL Server-Instanz](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |**Laden von Daten**|Laden von Adventure Works gemäß Schnellstartanleitung|[Wiederherstellen von Wide World Importers](../managed-instance/restore-sample-database-quickstart.md) | [Wiederherstellen von Wide World Importers](../managed-instance/restore-sample-database-quickstart.md) |
  | **Laden von Daten** ||Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)| Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|

  > [!IMPORTANT]
  > Die Skripts in diesem Artikel sind für die Adventure Works-Datenbank geschrieben. Bei einer verwalteten SQL-Instanz müssen Sie entweder die Adventure Works-Datenbank in eine Instanzdatenbank importieren oder die Skripts in diesem Artikel zur Verwendung der Wide World Importers-Datenbank anpassen.

- Installation von Golang und der dazugehörigen Software für Ihr Betriebssystem:

  - **macOS**: Installieren Sie Homebrew und Golang. Weitere Informationen finden Sie unter [Schritt 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/mac/).
  - **Ubuntu**:  Installieren Sie Golang. Weitere Informationen finden Sie unter [Schritt 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/ubuntu/).
  - **Windows**: Installieren Sie Golang. Weitere Informationen finden Sie unter [Schritt 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/windows/).

## <a name="get-server-connection-information"></a>Ermitteln von Serververbindungsinformationen

Ermitteln Sie die Verbindungsinformationen, die Sie zum Herstellen einer Verbindung mit der Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Navigieren Sie zur Seite **SQL-Datenbanken** oder **Verwaltete SQL-Instanzen**.

3. Auf der Seite **Übersicht** finden Sie den vollqualifizierten Servernamen für eine Datenbank in Azure SQL-Datenbank neben **Servername** oder den vollqualifizierten Servernamen (oder die IP-Adresse) für Azure SQL Managed Instance bzw. für SQL Server auf einem virtuellen Azure-Computer neben **Host**. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus.

> [!NOTE]
> Verbindungsinformationen für SQL Server auf einem virtuellen Azure-Computer finden Sie unter [Herstellen einer Verbindung mit einer SQL Server-Instanz](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="create-golang-project-and-dependencies"></a>Erstellen des Golang-Projekts und der Abhängigkeiten

1. Erstellen Sie über das Terminal einen neuen Projektordner mit dem Namen **SqlServerSample**. 

   ```bash
   mkdir SqlServerSample
   ```

2. Navigieren Sie zum Verzeichnis **SqlServerSample**, und installieren Sie den SQL Server-Treiber für Go.

   ```bash
   cd SqlServerSample
   go get github.com/denisenkom/go-mssqldb
   ```

## <a name="create-sample-data"></a>Erstellen von Beispieldaten

1. Erstellen Sie in einem Text-Editor eine Datei mit dem Namen **CreateTestData.sql** im Ordner **SqlServerSample**. Fügen Sie diesen T-SQL-Code in die Datei ein. Hiermit werden ein Schema und eine Tabelle erstellt und einige Zeilen eingefügt.

   ```sql
   CREATE SCHEMA TestSchema;
   GO

   CREATE TABLE TestSchema.Employees (
     Id       INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
     Name     NVARCHAR(50),
     Location NVARCHAR(50)
   );
   GO

   INSERT INTO TestSchema.Employees (Name, Location) VALUES
     (N'Jared',  N'Australia'),
     (N'Nikita', N'India'),
     (N'Tom',    N'Germany');
   GO

   SELECT * FROM TestSchema.Employees;
   GO
   ```

2. Verwenden Sie `sqlcmd`, um eine Verbindung mit der Datenbank herzustellen und Ihr neu erstelltes Azure SQL-Skript auszuführen. Setzen Sie dabei die entsprechenden Werte für Server, Datenbank, Benutzernamen und Kennwort ein.

   ```bash
   sqlcmd -S <your_server>.database.windows.net -U <your_username> -P <your_password> -d <your_database> -i ./CreateTestData.sql
   ```

## <a name="insert-code-to-query-the-database"></a>Einfügen von Code zum Abfragen der Datenbank

1. Erstellen Sie eine Datei mit dem Namen **sample.go** im Ordner **SqlServerSample**.

2. Fügen Sie diesen Code in der Datei ein. Fügen Sie die Werte für Server, Datenbank, Benutzernamen und Kennwort hinzu. In diesem Beispiel werden die Golang-[Kontextmethoden](https://golang.org/pkg/context/) verwendet, um sicherzustellen, dass eine aktive Verbindung besteht.

   ```go
   package main

   import (
       _ "github.com/denisenkom/go-mssqldb"
       "database/sql"
       "context"
       "log"
       "fmt"
       "errors"
   )

   var db *sql.DB

   var server = "<your_server.database.windows.net>"
   var port = 1433
   var user = "<your_username>"
   var password = "<your_password>"
   var database = "<your_database>"

   func main() {
       // Build connection string
       connString := fmt.Sprintf("server=%s;user id=%s;password=%s;port=%d;database=%s;",
           server, user, password, port, database)

       var err error

       // Create connection pool
       db, err = sql.Open("sqlserver", connString)
       if err != nil {
           log.Fatal("Error creating connection pool: ", err.Error())
       }
       ctx := context.Background()
       err = db.PingContext(ctx)
       if err != nil {
           log.Fatal(err.Error())
       }
       fmt.Printf("Connected!\n")

       // Create employee
       createID, err := CreateEmployee("Jake", "United States")
       if err != nil {
           log.Fatal("Error creating Employee: ", err.Error())
       }
       fmt.Printf("Inserted ID: %d successfully.\n", createID)

       // Read employees
       count, err := ReadEmployees()
       if err != nil {
           log.Fatal("Error reading Employees: ", err.Error())
       }
       fmt.Printf("Read %d row(s) successfully.\n", count)

       // Update from database
       updatedRows, err := UpdateEmployee("Jake", "Poland")
       if err != nil {
           log.Fatal("Error updating Employee: ", err.Error())
       }
       fmt.Printf("Updated %d row(s) successfully.\n", updatedRows)

       // Delete from database
       deletedRows, err := DeleteEmployee("Jake")
       if err != nil {
           log.Fatal("Error deleting Employee: ", err.Error())
       }
       fmt.Printf("Deleted %d row(s) successfully.\n", deletedRows)
   }

   // CreateEmployee inserts an employee record
   func CreateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()
       var err error

       if db == nil {
           err = errors.New("CreateEmployee: db is null")
           return -1, err
       }

       // Check if database is alive.
       err = db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := `
         INSERT INTO TestSchema.Employees (Name, Location) VALUES (@Name, @Location);
         select isNull(SCOPE_IDENTITY(), -1);
       `

       stmt, err := db.Prepare(tsql)
       if err != nil {
          return -1, err
       }
       defer stmt.Close()

       row := stmt.QueryRowContext(
           ctx,
           sql.Named("Name", name),
           sql.Named("Location", location))
       var newID int64
       err = row.Scan(&newID)
       if err != nil {
           return -1, err
       }

       return newID, nil
   }

   // ReadEmployees reads all employee records
   func ReadEmployees() (int, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := fmt.Sprintf("SELECT Id, Name, Location FROM TestSchema.Employees;")

       // Execute query
       rows, err := db.QueryContext(ctx, tsql)
       if err != nil {
           return -1, err
       }

       defer rows.Close()

       var count int

       // Iterate through the result set.
       for rows.Next() {
           var name, location string
           var id int

           // Get values from row.
           err := rows.Scan(&id, &name, &location)
           if err != nil {
               return -1, err
           }

           fmt.Printf("ID: %d, Name: %s, Location: %s\n", id, name, location)
           count++
       }

       return count, nil
   }

   // UpdateEmployee updates an employee's information
   func UpdateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := fmt.Sprintf("UPDATE TestSchema.Employees SET Location = @Location WHERE Name = @Name")

       // Execute non-query with named parameters
       result, err := db.ExecContext(
           ctx,
           tsql,
           sql.Named("Location", location),
           sql.Named("Name", name))
       if err != nil {
           return -1, err
       }

       return result.RowsAffected()
   }

   // DeleteEmployee deletes an employee from the database
   func DeleteEmployee(name string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           return -1, err
       }

       tsql := fmt.Sprintf("DELETE FROM TestSchema.Employees WHERE Name = @Name;")

       // Execute non-query with named parameters
       result, err := db.ExecContext(ctx, tsql, sql.Named("Name", name))
       if err != nil {
           return -1, err
       }

       return result.RowsAffected()
   }
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus.

   ```bash
   go run sample.go
   ```

2. Überprüfen Sie die Ausgabe.

   ```text
   Connected!
   Inserted ID: 4 successfully.
   ID: 1, Name: Jared, Location: Australia
   ID: 2, Name: Nikita, Location: India
   ID: 3, Name: Tom, Location: Germany
   ID: 4, Name: Jake, Location: United States
   Read 4 row(s) successfully.
   Updated 1 row(s) successfully.
   Deleted 1 row(s) successfully.
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Entwerfen Ihrer ersten Datenbank in Azure SQL-Datenbank](design-first-database-tutorial.md)
- [Golang-Treiber für SQL Server](https://github.com/denisenkom/go-mssqldb)
- [Seite für Problemmeldungen und Fragen](https://github.com/denisenkom/go-mssqldb/issues)

