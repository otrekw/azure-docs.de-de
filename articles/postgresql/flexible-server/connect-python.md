---
title: 'Schnellstart: Herstellen einer Verbindung unter Verwendung von Python: Azure Database for PostgreSQL Flexible Server'
description: Diese Schnellstartanleitung enthält mehrere Python-Codebeispiele, mit deren Hilfe Sie eine Verbindung von Azure Database for PostgreSQL Flexible Server herstellen und Daten abfragen können.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.custom: mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 09/22/2020
ms.openlocfilehash: 89dc36a9b1b1fee9ad10d55945c7fc17bf72f476
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90945028"
---
# <a name="quickstart-use-python-to-connect-and-query-data-in-azure-database-for-postgresql---flexible-server"></a>Schnellstart: Verwenden von Python zum Herstellen einer Verbindung mit Azure Database for PostgreSQL Flexible Server sowie zum Abfragen von Daten

> [!IMPORTANT]
> Azure Database for PostgreSQL Flexible Server befindet sich in der Vorschau.

In dieser Schnellstartanleitung stellen Sie unter Verwendung von Python eine Verbindung mit einer Azure Database for PostgreSQL Flexible Server-Instanz her. Anschließend verwenden Sie SQL-Anweisungen, um Daten in der Datenbank über Mac-, Ubuntu Linux- und Windows-Plattformen abzufragen, einzufügen, zu aktualisieren und zu löschen. 

In diesem Artikel wird davon ausgegangen, dass Sie mit der Python-Entwicklung vertraut sind, aber noch keine Erfahrung mit Azure Database for PostgreSQL Flexible Server haben.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Eine Instanz von Azure Database for PostgreSQL Flexible Server. Wie Sie einen flexiblen Server erstellen, erfahren Sie unter [Erstellen einer flexiblen Azure Database for PostgreSQL Flexible Server-Instanz im Azure-Portal](./quickstart-create-server-portal.md).
* [Python](https://www.python.org/downloads/) 2.7.9+ oder 3.4+.
* Aktuelles [pip](https://pip.pypa.io/en/stable/installing/)-Paketinstallationsprogramm.

## <a name="preparing-your-client-workstation"></a>Vorbereiten der Clientarbeitsstation
- Wenn Sie Ihre Flexible Server-Instanz mit *Privater Zugriff (VNET-Integration)* erstellt haben, müssen Sie eine Verbindung mit Ihrem Server über eine Ressource innerhalb desselben virtuellen Netzwerks herstellen. Sie können einen virtuellen Computer erstellen und dem VNET hinzufügen, das mit Ihrer Flexible Server-Instanz erstellt wurde. Weitere Informationen finden Sie unter [Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server mit der Azure CLI](./how-to-manage-virtual-network-cli.md).
- Wenn Sie Ihre Flexible Server-Instanz mit *Öffentlicher Zugriff (zulässige IP-Adressen)* erstellt haben, können Sie die lokale IP-Adresse der Liste der Firewallregeln auf Ihrem Server hinzufügen. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL Flexible Server mithilfe der Azure CLI](./how-to-manage-firewall-cli.md).

## <a name="install-the-python-libraries-for-postgresql"></a>Installieren der Python-Bibliotheken für PostgreSQL
Das Modul [psycopg2](https://pypi.python.org/pypi/psycopg2/) ermöglicht das Herstellen einer Verbindung mit einer PostgreSQL-Datenbank sowie das Abfragen von Daten aus dieser Datenbank und ist als [wheel-Paket](https://pythonwheels.com/) für Linux, macOS oder Windows verfügbar. Installieren Sie die Binärversion des Moduls mit allen Abhängigkeiten. Weitere Informationen zur Installation von `psycopg2` sowie zu den Anforderungen finden Sie unter [Installation](http://initd.org/psycopg/docs/install.html). 

Öffnen Sie zum Installieren von `psycopg2` ein Terminal oder eine Eingabeaufforderung, und führen Sie den Befehl `pip install psycopg2` aus.

## <a name="get-database-connection-information"></a>Abrufen von Datenbankverbindungsinformationen
Für die Verbindungsherstellung mit einer Azure Database for PostgreSQL Flexible Server-Instanz sind der vollqualifizierte Servername und Anmeldeinformationen erforderlich. Diese Informationen finden Sie im Azure-Portal.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach dem Namen der Flexible Server-Instanz und wählen Sie ihn aus. 
2. Kopieren Sie auf der **Übersichtsseite** des Servers unter **Servername** den vollqualifizierten Servernamen und unter **Administratorbenutzername** den Administratorbenutzernamen. Der vollqualifizierte **Servername** hat immer das Format *\<my-server-name>.postgres.database.azure.com*.

   Darüber hinaus benötigen Sie Ihr Administratorkennwort. Sollten Sie es vergessen, können Sie es auf der Übersichtsseite zurücksetzen. 

   <!--![Azure Database for PostgreSQL server name](./media/connect-python/1-connection-string.png)-->

## <a name="how-to-run-the-python-examples"></a>Ausführen der Python-Beispiele

Führen Sie für jedes Codebeispiel in diesem Artikel die folgenden Schritte aus:

1. Erstellen Sie eine neue Datei in einem Text-Editor. 

1. Fügen Sie der Datei das Codebeispiel hinzu. Ersetzen Sie im Code Folgendes:
   - `<server-name>` und `<admin-username>` durch die Werte, die Sie aus dem Azure-Portal kopiert haben.
   - `<admin-password>` durch Ihr Serverkennwort.
   - `<database-name>` durch den Namen Ihrer Azure Database for PostgreSQL Flexible Server-Datenbankinstanz. Bei der Servererstellung wurde automatisch eine Standarddatenbank namens *postgres* erstellt. Sie können diese Datenbank umbenennen oder mithilfe von SQL-Befehlen eine neue Datenbank erstellen. 

1. Speichern Sie die Datei mit der Erweiterung *.py* in Ihrem Projektordner (Beispiel: *postgres-insert.py*). Achten Sie unter Windows darauf, dass beim Speichern die UTF-8-Codierung ausgewählt ist. 

1. Wechseln Sie zum Ausführen der Datei mithilfe einer Befehlszeilenschnittstelle zu Ihrem Projektordner, und geben Sie `python` sowie den Dateinamen ein (Beispiel: `python postgres-insert.py`).

## <a name="create-a-table-and-insert-data"></a>Erstellen einer Tabelle und Einfügen von Daten
Im folgenden Codebeispiel wird mithilfe der Funktion [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) eine Verbindung mit Ihrer Azure Database for PostgreSQL Flexible Server-Datenbankinstanz hergestellt, und es werden Daten mithilfe einer **INSERT**-SQL-Anweisung geladen. Die Funktion [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) führt die SQL-Abfrage für die Datenbank aus. 

```Python
import psycopg2
# Update connection string information 
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print("Finished dropping table (if existed)")
# Create a table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print("Finished creating table")
# Insert some data into the table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print("Inserted 3 rows of data")
# Clean up
conn.commit()
cursor.close()
conn.close()
```

Nach erfolgreicher Ausführung generiert der Code die folgende Ausgabe:

![Befehlszeilenausgabe](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Lesen von Daten
Im folgenden Codebeispiel wird eine Verbindung mit Ihrer Azure Database for PostgreSQL Flexible Server-Datenbankinstanz hergestellt und [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) mit der SQL-Anweisung **SELECT** verwendet, um Daten zu lesen. Diese Funktion akzeptiert eine Abfrage und gibt ein Resultset zurück, das mithilfe von [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall) durchlaufen werden kann. 

```Python
import psycopg2
# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()
# Print all rows
for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))
# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Aktualisieren von Daten
Im folgenden Codebeispiel wird eine Verbindung mit Ihrer Azure Database for PostgreSQL Flexible Server-Datenbankinstanz hergestellt und [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) mit der SQL-Anweisung **UPDATE** verwendet, um Daten zu aktualisieren. 

```Python
import psycopg2
# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print("Updated 1 row of data")
# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Löschen von Daten
Im folgenden Codebeispiel wird eine Verbindung mit Ihrer Azure Database for PostgreSQL Flexible Server-Datenbankinstanz hergestellt und [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) mit der SQL-Anweisung **DELETE** verwendet, um ein zuvor eingefügtes Bestandselement zu löschen. 

```Python
import psycopg2
# Update connection string information
host = "<server-name>"
dbname = "<database-name>"
user = "<admin-username>"
password = "<admin-password>"
sslmode = "require"
# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print("Connection established")
cursor = conn.cursor()
# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print("Deleted 1 row of data")
# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren einer Datenbank durch Sichern und Wiederherstellen](../howto-migrate-using-dump-and-restore.md)