---
title: Abfragen einer Datenbank mithilfe von Java
description: Hier erfahren Sie, wie Sie unter Verwendung von Java ein Programm erstellen, das eine Verbindung mit einer Azure SQL-Datenbank herstellt und sie mithilfe von T-SQL-Anweisungen abfragt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
ms.date: 03/25/2019
ms.custom: seo-java-july2019. seo-java-august2019
ms.openlocfilehash: 034f92ca3b7552373ae69148d09d58d3a5dd166a
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76768646"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von Java

In dieser Schnellstartanleitung stellen Sie mithilfe von Java eine Verbindung mit einer Azure SQL-Datenbank her und fragen Daten mithilfe von T-SQL-Anweisungen ab.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine [Azure SQL-Datenbank](sql-database-single-database-get-started.md)
- [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)-bezogene Software

  # <a name="macostabmacos"></a>[macOS](#tab/macos)

  Installieren Sie Homebrew und Java sowie Maven mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Java-Apps mit SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  # <a name="ubuntutabubuntu"></a>[Ubuntu](#tab/ubuntu)

  Installieren Sie Java und das Java Development Kit sowie Maven mithilfe der Schritte **1.2**, **1.3** und **1.4** des Artikels [Erstellen von Java-Apps mit SQL Server unter Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  # <a name="windowstabwindows"></a>[Windows](#tab/windows)

  Installieren Sie Java und Maven mithilfe der Schritte **1.2** und **1.3** des Artikels [Erstellen von Java-Apps mit SQL Server unter Windows](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

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

2. Wählen Sie **SQL-Datenbanken** aus, oder öffnen Sie die Seite **Verwaltete SQL-Instanzen**.

3. Sehen Sie sich auf der Seite **Übersicht** bei einer Einzeldatenbank den vollqualifizierten Servernamen neben **Servername** und bei einer verwalteten Instanz den vollqualifizierten Servernamen neben **Host** an. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus. 

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

## <a name="add-code-to-query-database"></a>Hinzufügen von Code zum Abfragen der Datenbank

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
   > Im Codebeispiel wird die Beispieldatenbank **AdventureWorksLT** für Azure SQL verwendet.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die App an der Eingabeaufforderung aus.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. Vergewissern Sie sich, dass die ersten 20 Zeilen zurückgegeben werden, und schließen Sie das App-Fenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)  

- [Microsoft JDBC-Treiber für SQL Server](https://github.com/microsoft/mssql-jdbc)  

- [Melden von Problemen/Stellen von Fragen](https://github.com/microsoft/mssql-jdbc/issues)  
