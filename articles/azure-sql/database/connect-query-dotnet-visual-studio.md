---
title: Verwenden von Visual Studio mit .NET und C# zum Abfragen
description: Erstellen Sie mithilfe von Visual Studio eine C#-App, die eine Verbindung mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance herstellt und Abfragen ausführt.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: devx-track-csharp, sqldbrb=2
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 08/10/2020
ms.openlocfilehash: 1fe39e0ff9acc0c092199ab2dd199cf396f67d01
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107874763"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-a-database"></a>Schnellstart: Herstellen einer Verbindung mit einer Datenbank und Abfragen der Datenbank mit .NET und C# in Visual Studio
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi-asa.md)]

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit [.NET Framework](https://dotnet.microsoft.com) und C#-Code in Visual Studio eine Datenbank in Azure SQL oder Synapse SQL mit Transact-SQL-Anweisungen abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Visual Studio 2019](https://www.visualstudio.com/downloads/) – Community, Professional oder Enterprise Edition.
- Eine Datenbank, in der Sie eine Abfrage ausführen können.

  [!INCLUDE[create-configure-database](../includes/create-configure-database.md)]

## <a name="create-code-to-query-the-database-in-azure-sql-database"></a>Schreiben von Code zum Abfragen der Datenbank in Azure SQL-Datenbank

1. Erstellen Sie in Visual Studio ein neues Projekt. 
   
1. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Visual C#** und anschließend **Konsolen-App (.NET Framework)** aus.
   
1. Geben Sie als Projektname *sqltest* ein, und wählen Sie **OK** aus. Das neue Projekt wird erstellt. 
   
1. Wählen Sie **Projekt** >  **NuGet-Pakete verwalten** aus. 
   
1. Wählen Sie in **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, suchen Sie nach **Microsoft.Data.SqlClient**, und wählen Sie die Option aus.
   
1. Wählen Sie auf der Seite **Microsoft.Data.SqlClient** die Option **Installieren** aus. 
   - Wählen Sie bei Aufforderung **OK** aus, um die Installation fortzusetzen. 
   - Sollte das Fenster **Zustimmung zur Lizenz** angezeigt werden, wählen Sie **Ich stimme zu** aus.
   
1. Wenn die Installation abgeschlossen ist, können Sie den **NuGet-Paket-Manager** schließen. 
   
1. Ersetzen Sie den Inhalt von **Program.cs** im Code-Editor durch den folgenden Code. Ersetzen Sie Ihre Werte für `<your_server>`, `<your_username>`, `<your_password>` und `<your_database>`.
   
   ```csharp
   using System;
   using Microsoft.Data.SqlClient;
   using System.Text;
   
   namespace sqltest
   {
       class Program
       {
           static void Main(string[] args)
           {
               try 
               { 
                   SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                   builder.DataSource = "<your_server>.database.windows.net"; 
                   builder.UserID = "<your_username>";            
                   builder.Password = "<your_password>";     
                   builder.InitialCatalog = "<your_database>";
   
                   using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                   {
                       Console.WriteLine("\nQuery data example:");
                       Console.WriteLine("=========================================\n");
                       
                       String sql = "SELECT name, collation_name FROM sys.databases";
   
                       using (SqlCommand command = new SqlCommand(sql, connection))
                       {
                           connection.Open();
                           using (SqlDataReader reader = command.ExecuteReader())
                           {
                               while (reader.Read())
                               {
                                   Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                               }
                           }
                       }                    
                   }
               }
               catch (SqlException e)
               {
                   Console.WriteLine(e.ToString());
               }
               Console.ReadLine();
           }
       }
   }
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Um die App auszuführen, wählen Sie **Debuggen** > **Debuggen starten**, oder wählen Sie **Start** in der Symbolleiste, oder drücken Sie **F5**.
1. Überprüfen Sie, ob die Datenbanknamen und Sortierungen zurückgegeben werden, und schließen Sie dann das App-Fenster.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie unter Windows/Linux/macOS [unter Verwendung von .NET Core eine Verbindung mit einer Datenbank in Azure SQL-Datenbank herstellen und die Datenbank abfragen](connect-query-dotnet-core.md).  
- Informieren Sie sich über [die ersten Schritte mit .NET Core unter Windows/Linux/macOS bei Verwendung der Befehlszeile](/dotnet/core/tutorials/using-with-xplat-cli).
- Erfahren Sie, wie Sie [Ihre erste Datenbank in Azure SQL-Datenbank mithilfe von SSMS entwerfen](design-first-database-tutorial.md) oder wie Sie [Ihre erste Datenbank in Azure SQL-Datenbank mithilfe von .NET entwerfen](design-first-database-csharp-tutorial.md).
- Weitere Informationen zu .NET finden Sie in der [.NET-Dokumentation](/dotnet/).
- Beispiel für eine Wiederholungslogik: [Herstellen resilienter Azure SQL-Verbindungen mit ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: /sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net
