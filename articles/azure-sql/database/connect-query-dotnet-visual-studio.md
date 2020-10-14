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
ms.openlocfilehash: 30cf90f6020ecd4eb560ac0fe2886ff03c552d13
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91825574"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Schnellstart: Verwenden Sie .NET und C# in Visual Studio, um eine Verbindung mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance herzustellen und die Datenbank abzufragen.
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit [.NET Framework](https://www.microsoft.com/net/) und C#-Code in Visual Studio eine Datenbank in Azure SQL-Datenbank mit Transact-SQL-Anweisungen abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine Datenbank in Azure SQL-Datenbank In den folgenden Schnellstartanleitungen erfahren Sie jeweils, wie Sie eine Datenbank in Azure SQL-Datenbank erstellen und anschließend konfigurieren:

  | Aktion | SQL-Datenbank | Verwaltete SQL-Instanz | SQL Server auf Azure-VMs |
  |:--- |:--- |:---|:---|
  | Erstellen| [Portal](single-database-create-quickstart.md) | [Portal](../managed-instance/instance-create-quickstart.md) | [Portal](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  || [BEFEHLSZEILENSCHNITTSTELLE (CLI)](scripts/create-and-configure-database-cli.md) | [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/create-and-configure-database-powershell.md) | [PowerShell](../managed-instance/scripts/create-configure-managed-instance-powershell.md) | [PowerShell](../virtual-machines/windows/sql-vm-create-powershell-quickstart.md)
  | Konfigurieren | [IP-Firewallregel auf Serverebene](firewall-create-server-level-portal-quickstart.md)| [Verbindung von einem virtuellen Computer](../managed-instance/connect-vm-instance-configure.md)|
  |||[Verbindungen von lokalen Computern](../managed-instance/point-to-site-p2s-configure.md) | [Connect to SQL Server (Herstellen einer Verbindung mit SQL Server)](../virtual-machines/windows/sql-vm-create-portal-quickstart.md)
  |Laden der Daten|Laden von Adventure Works gemäß Schnellstartanleitung|[Wiederherstellen von Wide World Importers](../managed-instance/restore-sample-database-quickstart.md) | [Wiederherstellen von Wide World Importers](../managed-instance/restore-sample-database-quickstart.md) |
  |||Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)| Wiederherstellen oder Importieren von Adventure Works über eine [BACPAC-Datei](database-import.md) von [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Die Skripts in diesem Artikel sind für die Adventure Works-Datenbank geschrieben. Bei einer verwalteten SQL-Instanz müssen Sie entweder die Adventure Works-Datenbank in eine Instanzdatenbank importieren oder die Skripts in diesem Artikel zur Verwendung der Wide World Importers-Datenbank anpassen.

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) – Community, Professional oder Enterprise Edition.

## <a name="get-server-connection-information"></a>Ermitteln von Serververbindungsinformationen

Ermitteln Sie die Verbindungsinformationen, die Sie zum Herstellen einer Verbindung mit der Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Navigieren Sie zur Seite **SQL-Datenbanken** oder **Verwaltete SQL-Instanzen**.

3. Auf der Seite **Übersicht** finden Sie den vollqualifizierten Servernamen für eine Datenbank in Azure SQL-Datenbank neben **Servername** oder den vollqualifizierten Servernamen (oder die IP-Adresse) für Azure SQL Managed Instance bzw. für SQL Server auf einem virtuellen Azure-Computer neben **Host**. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus.

> [!NOTE]
> Verbindungsinformationen für SQL Server auf einem virtuellen Azure-Computer finden Sie unter [Herstellen einer Verbindung mit einer SQL Server-Instanz](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

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
   
   >[!IMPORTANT]
   >Der Code in diesem Beispiel verwendet die AdventureWorksLT-Beispieldaten, die Sie beim Erstellen Ihrer Datenbank als Datenquelle auswählen können. Wenn Ihre Datenbank unterschiedliche Daten aufweist, verwenden Sie Tabellen aus Ihrer eigenen Datenbank in der SELECT-Abfrage. 
   
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
                       
                       StringBuilder sb = new StringBuilder();
                       sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                       sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                       sb.Append("JOIN [SalesLT].[Product] p ");
                       sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                       String sql = sb.ToString();
   
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
1. Stellen Sie sicher, dass die obersten 20 Kategorie-/Produktzeilen aus Ihrer Datenbank zurückgegeben werden, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie unter Windows/Linux/macOS [unter Verwendung von .NET Core eine Verbindung mit einer Datenbank in Azure SQL-Datenbank herstellen und die Datenbank abfragen](connect-query-dotnet-core.md).  
- Informieren Sie sich über [die ersten Schritte mit .NET Core unter Windows/Linux/macOS bei Verwendung der Befehlszeile](/dotnet/core/tutorials/using-with-xplat-cli).
- Erfahren Sie, wie Sie [Ihre erste Datenbank in Azure SQL-Datenbank mithilfe von SSMS entwerfen](design-first-database-tutorial.md) oder wie Sie [Ihre erste Datenbank in Azure SQL-Datenbank mithilfe von .NET entwerfen](design-first-database-csharp-tutorial.md).
- Weitere Informationen zu .NET finden Sie in der [.NET-Dokumentation](https://docs.microsoft.com/dotnet/).
- Beispiel für eine Wiederholungslogik: [Herstellen resilienter Azure SQL-Verbindungen mit ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

