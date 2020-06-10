---
title: Verwenden von Ruby zum Abfragen
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Ruby ein Programm erstellen, das eine Verbindung mit einer Datenbank herstellt und sie mithilfe von Transact-SQL-Anweisungen abfragt.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2 
ms.devlang: ruby
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/29/2020
ms.openlocfilehash: c297a40a5452938d8e706bf2e0791e4a7d03210c
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267289"
---
# <a name="quickstart-use-ruby-to-query-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Schnellstart: Abfragen einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance mithilfe von Ruby
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von [Ruby](https://www.ruby-lang.org) eine Verbindung mit einer Datenbank herstellen und Daten mit Transact-SQL-Anweisungen abfragen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- Eine Datenbank. In den folgenden Schnellstartanleitungen erfahren Sie jeweils, wie Sie die Datenbank erstellen und anschließend konfigurieren:

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

  > [!IMPORTANT]
  > Die Skripts in diesem Artikel sind für die Adventure Works-Datenbank geschrieben. Bei einer verwalteten SQL-Instanz müssen Sie entweder die Adventure Works-Datenbank in eine Instanzdatenbank importieren oder die Skripts in diesem Artikel zur Verwendung der Wide World Importers-Datenbank anpassen.
  
- Ruby und verwandte Software für Ihr Betriebssystem:
  
  - **macOS**: Installieren Sie Homebrew, rbenv und ruby-build, Ruby, FreeTDS und TinyTDS. Siehe Schritte 1.2, 1.3, 1.4, 1.5 und 2.1 in [Erstellen von Ruby-Apps mit SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Installieren Sie die Voraussetzungen für Ruby, rbenv und ruby-build, Ruby, FreeTDS und TinyTDS. Siehe Schritte 1.2, 1.3, 1.4, 1.5 und 2.1 in [Erstellen von Ruby-Apps mit SQL Server unter Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows**: Installieren Sie Ruby, Ruby Devkit und TinyTDS. Siehe [Konfigurieren der Entwicklungsumgebung für die Ruby-Entwicklung](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-server-connection-information"></a>Ermitteln von Serververbindungsinformationen

Ermitteln Sie die Verbindungsinformationen, die Sie zum Herstellen einer Verbindung mit einer Datenbank in Azure SQL-Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Navigieren Sie zur Seite **SQL-Datenbanken** oder **Verwaltete SQL-Instanzen**.

3. Auf der Seite **Übersicht** finden Sie den vollqualifizierten Servernamen für eine Datenbank in Azure SQL-Datenbank neben **Servername** oder den vollqualifizierten Servernamen (oder die IP-Adresse) für Azure SQL Managed Instance bzw. für SQL Server auf einem virtuellen Azure-Computer neben **Host**. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus.

> [!NOTE]
> Verbindungsinformationen für SQL Server auf einem virtuellen Azure-Computer finden Sie unter [Herstellen einer Verbindung mit einer SQL Server-Instanz](../virtual-machines/windows/sql-vm-create-portal-quickstart.md#connect-to-sql-server).

## <a name="create-code-to-query-your-database-in-azure-sql-database"></a>Schreiben von Code zum Abfragen einer Datenbank in Azure SQL-Datenbank

1. Erstellen Sie in einem Text- oder Code-Editor eine neue Datei namens *sqltest.rb*.

1. Fügen Sie den folgenden Code hinzu. Ersetzen Sie `<server>`, `<database>`, `<username>` und `<password>` durch die Werte aus Ihrer Datenbank in Azure SQL-Datenbank.

   >[!IMPORTANT]
   >Der Code in diesem Beispiel verwendet die AdventureWorksLT-Beispieldaten, die Sie beim Erstellen Ihrer Datenbank als Datenquelle auswählen können. Wenn Ihre Datenbank unterschiedliche Daten aufweist, verwenden Sie Tabellen aus Ihrer eigenen Datenbank in der SELECT-Abfrage. 

   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung folgenden Befehl aus:

   ```bash
   ruby sqltest.rb
   ```
   
1. Stellen Sie sicher, dass die obersten 20 Kategorie-/Produktzeilen aus Ihrer Datenbank zurückgegeben werden. 

## <a name="next-steps"></a>Nächste Schritte
- [Entwerfen Ihrer ersten Datenbank in Azure SQL-Datenbank](design-first-database-tutorial.md)
- [GitHub-Repository für TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Report issues or ask questions about TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues) (Melden von Problemen oder Stellen von Fragen zu TinyTDS)
- [Ruby-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
