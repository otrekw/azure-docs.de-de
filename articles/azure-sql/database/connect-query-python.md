---
title: Abfragen einer Datenbank mithilfe von Python
description: In diesem Thema erfahren Sie, wie Sie unter Verwendung von Python ein Programm erstellen, das eine Verbindung mit einer Datenbank in Azure SQL-Datenbank herstellt und diese mithilfe von Transact-SQL-Anweisungen abfragt.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019, sqldbrb=2, devx-track-python
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/19/2020
ms.openlocfilehash: 8fb6d319cacf85630b2c400cd18d14487725f925
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2020
ms.locfileid: "97703965"
---
# <a name="quickstart-use-python-to-query-a-database"></a>Schnellstart: Abfragen einer Datenbank mithilfe von Python
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi-asa.md)]

In dieser Schnellstartanleitung stellen Sie mithilfe von Python eine Verbindung mit Azure SQL-Datenbank, Azure SQL Managed Instance oder einer Synapse SQL-Datenbank her und fragen Daten mithilfe von T-SQL-Anweisungen ab.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- Eine Datenbank, in der Sie eine Abfrage ausführen können.

  [!INCLUDE[create-configure-database](../includes/create-configure-database.md)]

- [Python](https://python.org/downloads) 3 und zugehörige Software

  # <a name="macos"></a>[macOS](#tab/macos)

  Führen Sie die Schritte **1.2**, **1.3** und **2.1** des Artikels [Erstellen von Python-Apps mithilfe von SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/) aus, um Homebrew und Python, den ODBC-Treiber und SQLCMD sowie den Python-Treiber für SQL Server zu installieren.

  Weitere Informationen finden Sie unter [Installation von Microsoft ODBC Driver for SQL Server (macOS)](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Verwenden Sie `sudo apt-get install python python-pip gcc g++ build-essential`, um Python und andere erforderliche Pakete zu installieren.

  Informationen zum Installieren von ODBC-Treiber, SQLCMD und Python-Treiber für SQL Server finden Sie unter [Schritt 1: Konfigurieren der Entwicklungsumgebung für die pyODBC-Python-Entwicklung](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  Weitere Informationen finden Sie unter [Installation von Microsoft ODBC Driver for SQL Server (Linux)](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="windows"></a>[Windows](#tab/windows)

  Informationen zum Installieren von Python, ODBC-Treiber, SQLCMD und Python-Treiber für SQL Server finden Sie unter [Schritt 1: Konfigurieren der Entwicklungsumgebung für die pyODBC-Python-Entwicklung](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

  Weitere Informationen finden Sie unter [Microsoft ODBC Driver for SQL Server](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server).

---
Unter [Azure SQL-Datenbank-Bibliotheken für Python](/python/api/overview/azure/sql), im [pyodbc-Repository](https://github.com/mkleehammer/pyodbc/wiki/) sowie anhand eines [pyodbc-Beispiels](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) können Sie sich näher mit Python und der Datenbank in Azure SQL-Datenbank vertraut machen.

## <a name="create-code-to-query-your-database"></a>Erstellen von Code zum Abfragen der Datenbank 

1. Erstellen Sie in einem Text-Editor eine neue Datei namens *sqltest.py*.  
   
1. Fügen Sie den folgenden Code hinzu. Verwenden Sie die Verbindungsinformationen aus dem Abschnitt mit den Voraussetzungen, und ersetzen Sie \<server>, \<database>, \<username> und \<password> durch Ihre eigenen Werte.
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'   
   driver= '{ODBC Driver 17 for SQL Server}'
   
   with pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password) as conn:
       with conn.cursor() as cursor:
           cursor.execute("SELECT TOP 3 name, collation_name FROM sys.databases")
           row = cursor.fetchone()
           while row:
               print (str(row[0]) + " " + str(row[1]))
               row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie an der Eingabeaufforderung folgenden Befehl aus:

   ```cmd
   python sqltest.py
   ```

1. Vergewissern Sie sich, dass die Datenbanken und die zugehörigen Sortierungen zurückgegeben werden, und schließen Sie dann das Befehlsfenster.

## <a name="next-steps"></a>Nächste Schritte

- [Entwerfen Ihrer ersten Datenbank in Azure SQL-Datenbank](design-first-database-tutorial.md)
- [Microsoft-Python-Treiber für SQL Server](/sql/connect/python/python-driver-for-sql-server/)
- [Python Developer Center](https://azure.microsoft.com/develop/python/?v=17.23h)
