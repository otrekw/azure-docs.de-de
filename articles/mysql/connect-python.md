---
title: 'Schnellstart: Herstellen einer Verbindung unter Verwendung von Python: Azure Database for MySQL'
description: Diese Schnellstartanleitung enthält mehrere Python-Codebeispiele, mit deren Hilfe Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen und Daten daraus abfragen können.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom:
- mvc
- seo-python-october2019
- devx-track-python
ms.devlang: python
ms.topic: quickstart
ms.date: 5/26/2020
ms.openlocfilehash: aad160f8a4ec96a1e0e627684c598062079b57e0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90896316"
---
# <a name="quickstart-use-python-to-connect-and-query-data-in-azure-database-for-mysql"></a>Schnellstart: Verwenden von Python zum Herstellen einer Verbindung und zum Abfragen von Daten in Azure Database for MySQL

In dieser Schnellstartanleitung stellen Sie unter Verwendung von Python eine Verbindung mit einer Azure Database for MySQL-Instanz her. Anschließend verwenden Sie SQL-Anweisungen, um Daten in der Datenbank über Mac-, Ubuntu Linux- und Windows-Plattformen abzufragen, einzufügen, zu aktualisieren und zu löschen. 

In diesem Thema wird davon ausgegangen, dass Sie mit der Python-Entwicklung vertraut sind, aber noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Ein Azure Database for MySQL-Server. [Erstellen Sie einen Azure Database for MySQL-Server mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md), oder [erstellen Sie einen Azure Database for MySQL-Server mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

> [!IMPORTANT] 
> Sicherstellen, dass der IP-Adresse, über die Sie eine Verbindung herstellen, die Firewallregeln des Servers über das [Azure-Portal](./howto-manage-firewall-using-portal.md) oder die [Azure CLI](./howto-manage-firewall-using-cli.md) hinzugefügt wurden

## <a name="install-python-and-the-mysql-connector"></a>Installieren von Python und dem MySQL-Connector

Führen Sie die folgenden Schritte aus, um Python und den MySQL-Connector für Python auf Ihrem Computer zu installieren: 

> [!NOTE]
> In dieser Schnellstartanleitung wird eine unformatierte SQL-Abfrage verwendet, um eine Verbindung mit MySQL herzustellen. Verwenden Sie bei Nutzung eines Webframeworks den empfohlenen Connector für das Framework, z. B. [mysqlclient](https://pypi.org/project/mysqlclient/) für Django.

1. Laden Sie [Python 3.7 oder höher](https://www.python.org/downloads/) für Ihr Betriebssystem herunter, und führen Sie die Installation durch. Stellen Sie sicher, dass Sie Python unter `PATH` hinzufügen, da dies für den MySQL-Connector erforderlich ist.
   
1. Öffnen Sie eine Eingabeaufforderung oder eine `bash`-Shell, und überprüfen Sie Ihre Python-Version, indem Sie `python -V` ausführen (Switch „V“ als Großbuchstabe).
   
1. Das Installationsprogramm für das `pip`-Paket ist in den aktuellen Versionen von Python enthalten. Führen Sie für `pip` das Update auf die aktuelle Version durch, indem Sie `pip install -U pip` ausführen. 
   
   Wenn `pip` nicht installiert ist, können Sie `get-pip.py` zum Herunterladen und Installieren verwenden. Weitere Informationen finden Sie unter [Installation](https://pip.pypa.io/en/stable/installing/). 
   
1. Verwenden Sie `pip`, um den MySQL-Connector für Python und die Abhängigkeiten zu installieren:
   
   ```bash
   pip install mysql-connector-python
   ```
   
   Sie können den Python-Connector für MySQL auch über [mysql.com](https://dev.mysql.com/downloads/connector/python/) installieren. Weitere Informationen zum MySQL-Connector für Python finden Sie unter [Entwicklerhandbuch für MySQL-Connector/Python](https://dev.mysql.com/doc/connector-python/en/). 

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Sie können die Verbindungsinformationen, die Sie zum Herstellen einer Verbindung mit Azure Database for MySQL benötigen, über das Azure-Portal ermitteln. Sie benötigen den Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
   
1. Suchen Sie im Portal in der Suchleiste nach dem von Ihnen erstellten Azure Database for MySQL-Server, z. B. **mydemoserver**, und wählen Sie ihn aus.
   
   :::image type="content" source="./media/connect-python/1_server-overview-name-login.png" alt-text="Servername für Azure-Datenbank für MySQL":::
   
1. Notieren Sie sich auf der Seite **Übersicht** des Servers den **Servernamen** und die **Anmelde-ID des Serveradministrators**. Falls Sie Ihr Kennwort vergessen haben, können Sie es auf dieser Seite auch zurücksetzen.
   
   :::image type="content" source="./media/connect-python/azure-database-for-mysql-server-overview-name-login.png" alt-text="Servername für Azure-Datenbank für MySQL":::

## <a name="run-the-python-examples"></a>Ausführen der Python-Beispiele

Führen Sie für jedes Codebeispiel in diesem Artikel die folgenden Schritte aus:

1. Erstellen Sie eine neue Datei in einem Text-Editor.
1. Fügen Sie der Datei das Codebeispiel hinzu. Ersetzen Sie im Code die Platzhalter `<mydemoserver>`, `<myadmin>`, `<mypassword>` und `<mydatabase>` durch die Werte für Ihren MySQL-Server und die zugehörige Datenbank.
1. Speichern Sie die Datei in einem Projektordner mit der Erweiterung *.py*, z. B. *C:\pythonmysql\createtable.py* oder */home/username/pythonmysql/createtable.py*.
1. Öffnen Sie zum Ausführen des Codes eine Eingabeaufforderung oder `bash`-Shell, und wechseln Sie in Ihren Projektordner, indem Sie beispielsweise `cd pythonmysql` ausführen. Geben Sie den Befehl `python` gefolgt vom Dateinamen ein (z. B. `python createtable.py`), und drücken Sie die EINGABETASTE. 
   
   > [!NOTE]
   > Falls *python.exe* unter Windows nicht gefunden wird, müssen Sie Ihrer PATH-Umgebungsvariablen ggf. den Python-Pfad hinzufügen oder den vollständigen Pfad zu *python.exe* angeben, z. B. `C:\python27\python.exe createtable.py`.

## <a name="create-a-table-and-insert-data"></a>Erstellen einer Tabelle und Einfügen von Daten

Verwenden Sie den folgenden Code, um eine Verbindung mit dem Server und der Datenbank herzustellen, eine Tabelle zu erstellen und die Daten mit einer SQL-Anweisung vom Typ **INSERT** zu laden. 

Mit dem Code wird die Bibliothek „mysql.connector“ importiert und die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) verwendet, um eine Verbindung mit Azure Database for MySQL herzustellen. Hierfür werden die [Argumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung genutzt. Im Code wird ein Cursor für die Verbindung verwendet, und mit der [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wird die SQL-Abfrage für die MySQL-Datenbank ausgeführt. 

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a>Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **SELECT** zu verbinden und zu lesen. 

Mit dem Code wird die Bibliothek „mysql.connector“ importiert und die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) verwendet, um eine Verbindung mit Azure Database for MySQL herzustellen. Hierfür werden die [Argumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung genutzt. Im Code wird ein Cursor für die Verbindung verwendet, und mit der [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wird die SQL-Abfrage für die MySQL-Datenbank ausgeführt. 

Der Code liest die Datenzeilen mit der [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html)-Methode, legt die Ergebnisse in einer Zeile der Auflistung ab und verwendet einen `for`-Iterator, um einen Schleifenvorgang für die Zeilen durchzuführen.

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **UPDATE** zu verbinden und zu aktualisieren. 

Mit dem Code wird die Bibliothek „mysql.connector“ importiert und die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) verwendet, um eine Verbindung mit Azure Database for MySQL herzustellen. Hierfür werden die [Argumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung genutzt. Im Code wird ein Cursor für die Verbindung verwendet, und mit der [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wird die SQL-Abfrage für die MySQL-Datenbank ausgeführt. 

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in the table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten mithilfe einer SQL-Anweisung des Typs **DELETE** zu entfernen. 

Mit dem Code wird die Bibliothek „mysql.connector“ importiert und die Funktion [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) verwendet, um eine Verbindung mit Azure Database for MySQL herzustellen. Hierfür werden die [Argumente](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in der config-Auflistung genutzt. Im Code wird ein Cursor für die Verbindung verwendet, und mit der [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html)-Methode wird die SQL-Abfrage für die MySQL-Datenbank ausgeführt. 

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in the table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./concepts-migrate-import-export.md)
