---
title: Abfragen einer Datenbank mithilfe von Java
description: Erfahren Sie, wie Sie unter Verwendung von Java ein Programm erstellen, das eine Verbindung mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance herstellt und diese mithilfe von T-SQL-Anweisungen abfragt.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
ms.date: 05/29/2020
ms.custom: seo-java-july2019. seo-java-august2019, sqldbrb=2 
ms.openlocfilehash: 6be52d2d3472888607bbd6276b4794184bb11273
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267391"
---
# <a name="quickstart-use-java-to-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Schnellstart: Abfragen einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance mithilfe von Java
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In dieser Schnellstartanleitung stellen Sie mithilfe von Java eine Verbindung mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance her und fragen Daten mithilfe von T-SQL-Anweisungen ab.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

  || SQL-Datenbank | Verwaltete SQL-Instanz | SQL Server auf Azure-VMs |
  |:--- |:--- |:---|:---|
  | Erstellen| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  || [BEFEHLSZEILENSCHNITTSTELLE (CLI)](scripts/create-and-configure-database-cli.md) | [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | Konfigurieren | [IP-Firewallregel auf Serverebene](firewall-create-server-level-portal-quickstart.md)| [Verbindung von einem virtuellen Computer](../managed-instance/connect-vm-instance-configure.md)|
  |||[Verbindungen von lokalen Computern](../managed-instance/point-to-site-p2s-configure.md) | [Herstellen einer Verbindung mit einer SQL Server-Instanz](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |Laden der Daten|Laden von Adventure Works gemäß Schnellstartanleitung|[Wiederherstellen von Wide World Importers](../managed-instance/restore-sample-database-quickstart.md) | [Wiederherstellen von Wide World Importers](../managed-instance/restore-sample-database-quickstart.md) |
  |||Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)| Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

- [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)-bezogene Software

  # <a name="macos"></a>[macOS](#tab/macos)

  Installieren Sie Homebrew und Java sowie Maven mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Java-Apps mit SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Installieren Sie Java, das Java Development Kit und Maven mithilfe der Schritte **1.2**, **1.3** und **1.4** des Artikels [Erstellen von Java-Apps mit SQL Server unter Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  # <a name="windows"></a>[Windows](#tab/windows)

  Installieren Sie Java und Maven mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Java-Apps mit SQL Server unter Windows](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

  ---

> [!IMPORTANT]
> Die Skripts in diesem Artikel wurden für die Datenbank **Adventure Works** geschrieben.

> [!NOTE]
> Optional kann Azure SQL Managed Instance verwendet werden.
>
> Verwenden Sie zum Erstellen und Konfigurieren das [Azure-Portal](../managed-instance/instance-create-quickstart.md), [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) oder die [Befehlszeilenschnittstelle](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44), und richten Sie anschließend die Konnektivität ([lokal](../managed-instance/point-to-site-p2s-configure.md) oder [VM](../managed-instance/connect-vm-instance-configure.md)) ein.
>
> Informationen zum Laden von Daten finden Sie unter [Schnellstart: Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank](database-import.md) (mit der Datei für [Adventure Works](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)) bzw. unter [Schnellstart: Wiederherstellen einer Datenbank in einer verwalteten Instanz](../managed-instance/restore-sample-database-quickstart.md).

## <a name="get-server-connection-information"></a>Ermitteln von Serververbindungsinformationen

Ermitteln Sie die Verbindungsinformationen, die Sie zum Herstellen einer Verbindung mit der Datenbank in Azure SQL-Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **SQL-Datenbanken** aus, oder öffnen Sie die Seite **Verwaltete SQL-Instanzen**.

3. Auf der Seite **Übersicht** finden Sie den vollqualifizierten Servernamen für eine Datenbank in Azure SQL-Datenbank neben **Servername** oder den vollqualifizierten Servernamen (oder die IP-Adresse) für Azure SQL Managed Instance bzw. für SQL Server auf einem virtuellen Azure-Computer neben **Host**. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus.

> [!NOTE]
> Verbindungsinformationen für SQL Server auf einem virtuellen Azure-Computer finden Sie unter [Herstellen einer Verbindung mit SQL Server](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="create-the-project"></a>Erstellen des Projekts

1. Erstellen Sie über die Eingabeaufforderung ein neues Maven-Projekt namens *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Wechseln Sie zum Ordner *sqltest*, und öffnen Sie *pom.xml* in Ihrem bevorzugten Text-Editor. Fügen Sie den Abhängigkeiten Ihres Projekts den **Microsoft JDBC-Treiber für SQL Server** hinzu. Verwenden Sie dazu den folgenden Code:

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. Fügen Sie Ihrem Projekt in *pom.xml* auch die folgenden Eigenschaften hinzu. Sollte kein Abschnitt für Eigenschaften vorhanden sein, können Sie im Anschluss an die Abhängigkeiten einen entsprechenden Abschnitt hinzufügen.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Speichern und schließen Sie *pom.xml*.

## <a name="add-code-to-query-the-database"></a>Hinzufügen von Code zum Abfragen der Datenbank

1. Ihr Maven-Projekt müsste bereits eine Datei namens *App.java* enthalten (unter

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*).

1. Öffnen Sie die Datei, und ersetzen Sie ihren Inhalt durch den folgenden Code. Fügen Sie anschließend die entsprechenden Werte für Server, Datenbank, Benutzer und Kennwort hinzu.

    ```java
    package com.sqldbsamples;

    import java.sql.Connection;
    import java.sql.Statement;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.DriverManager;

    public class App {

        public static void main(String[] args) {

            // Connect to database
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
            Connection connection = null;

            try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName "
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";

                try (Statement statement = connection.createStatement();
                ResultSet resultSet = statement.executeQuery(selectSql)) {

                    // Print results from select statement
                    System.out.println("Top 20 categories:");
                    while (resultSet.next())
                    {
                        System.out.println(resultSet.getString(1) + " "
                            + resultSet.getString(2));
                    }
                    connection.close();
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```

   > [!NOTE]
   > Im Codebeispiel wird die Beispieldatenbank **AdventureWorksLT** in Azure SQL-Datenbank verwendet.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die App an der Eingabeaufforderung aus.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie das App-Fenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwerfen Ihrer ersten Datenbank in Azure SQL-Datenbank](design-first-database-tutorial.md)  
- [Microsoft JDBC-Treiber für SQL Server](https://github.com/microsoft/mssql-jdbc)  
- [Melden von Problemen/Stellen von Fragen](https://github.com/microsoft/mssql-jdbc/issues)  
