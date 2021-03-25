---
title: 'Schnellstart: Herstellen einer Verbindung unter Verwendung der Azure CLI: Azure Database for PostgreSQL Flexible Server'
description: In dieser Schnellstartanleitung erhalten Sie Informationen zu verschiedenen Möglichkeiten, mithilfe der Azure CLI eine Verbindung mit Azure Database for PostgreSQL Flexible Server herzustellen.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 03/06/2021
ms.openlocfilehash: f4eec89aadee1966271286b9280916af973e4b1c
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102614342"
---
# <a name="quickstart-connect-and-query-with-azure-cli--with-azure-database-for-postgresql---flexible-server"></a>Schnellstart: Verbinden und Abfragen mit der Azure CLI mit Azure Database for PostgreSQL Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

In diesem Schnellstart wird gezeigt, wie Sie mithilfe der Azure CLI und des Befehls ```az postgres flexible-server connect``` eine Verbindung mit einer Azure Database for PostgreSQL Flexible Server-Instanz herstellen. Mit diesem Befehl können Sie die Konnektivität mit dem Datenbankserver testen und Abfragen ausführen. Sie können mithilfe des interaktiven Modus auch mehrere Abfragen ausführen. 

## <a name="prerequisites"></a>Voraussetzungen
- Ein Azure-Konto. Falls Sie noch kein Konto haben, können Sie eine [kostenlose Testversion](https://azure.microsoft.com/free/) verwenden.
- Installieren der aktuellen Version der [Azure CLI](/cli/azure/install-azure-cli) (mindestens 2.20.0)
- Anmelden unter Verwendung der Azure CLI mit dem Befehl ```az login``` 
- Aktivieren der Parameterpersistenz mit ```az config param-persist on```. Mithilfe der Parameterpersistenz können Sie den lokalen Kontext verwenden, ohne zahlreiche Argumente wie Ressourcengruppe oder Speicherort wiederholen zu müssen.

## <a name="create-an-postgresql-flexible-server"></a>Erstellen einer PostgreSQL Flexible Server-Instanz

Sie erstellen zuerst einen verwalteten PostgreSQL-Server. Führen Sie in [Azure Cloud Shell](https://shell.azure.com/) das folgende Skript aus, und notieren Sie sich den **Servernamen**, den **Benutzernamen** und das **Kennwort**, die mit diesem Befehl generiert werden.

```azurecli
az postgres flexible-server create --public-access <your-ip-address>
```
Sie können zur Anpassung zusätzliche Argumente für diesen Befehl angeben. Sehen Sie sich alle Argumente für [az postgres flexible-server create](/cli/azure/postgres/flexible-server#az_postgres_flexible_server_create) an.

## <a name="view-all-the-arguments"></a>Anzeigen aller Argumente
Sie können alle Argumente für diesen Befehl mit dem Argument ```--help``` anzeigen. 

```azurecli
az postgresql flexible-server connect --help
```

## <a name="test-database-server-connection"></a>Testen der Verbindung des Datenbankservers
Mit dem folgenden Befehl können Sie die Verbindung mit der Datenbank aus Ihrer Entwicklungsumgebung testen und überprüfen:

```azurecli
az postgres flexible-server connect -n <servername> -u <username> -p "<password>" -d <databasename>
```
**Beispiel:** 
```azurecli
az postgres flexible-server connect -n postgresdemoserver -u dbuser -p "dbpassword" -d postgres
```
Die Ausgabe wird angezeigt, wenn die Verbindung erfolgreich hergestellt wurde.
```output
Command group 'postgres flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Successfully connected to postgresdemoserver.
Local context is turned on. Its information is saved in working directory C:\mydir. You can run `az local-context off` to turn it off.
Your preference of  are now saved to local context. To learn more, type in `az local-context --help`
```

Konnte die Verbindung nicht hergestellt werden, versuchen Sie Folgendes:
- Überprüfen Sie, ob Port 5432 auf dem Clientcomputer geöffnet ist.
- Überprüfen Sie, ob Benutzername und Kennwort des Serveradministrators korrekt sind.
- Überprüfen Sie, ob die Firewallregel für Ihren Clientcomputer konfiguriert wurde.
- Wenn Sie Ihren Server mit privatem Zugriff in einem virtuellen Netzwerk konfiguriert haben, vergewissern Sie sich, dass sich Ihr Clientcomputer im gleichen virtuellen Netzwerk befindet.

## <a name="run-single-query"></a>Ausführen einer einzelnen Abfrage
Sie können mit dem Befehl eine einzelne Abfrage unter Verwendung des ```--querytext```-Arguments ```-q``` ausführen.

```azurecli
az postgres flexible-server connect -n <server-name> -u <username> -p "<password>" -d <database-name> -q "<query-text>"
```

**Beispiel:** 
```azurecli
az postgresql flexible-server connect -n postgresdemoserver -u dbuser -p "dbpassword" -d flexibleserverdb -q "select * from table1;" --output table
```

Die folgende Ausgabe sollte angezeigt werden:

```output
Command group 'postgres flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Successfully connected to postgresdemoserver.
Ran Database Query: 'select * from table1;'
Retrieving first 30 rows of query output, if applicable.
Closed the connection postgresdemoserver.
Local context is turned on. Its information is saved in working directory C:\mydir. You can run `az local-context off` to turn it off.
Your preference of  are now saved to local context. To learn more, type in `az local-context --help`
Txt    Val
-----  -----
test   200
test   200
test   200
test   200
test   200
test   200
test   200
```

## <a name="run-multiple-queries-using-interactive-mode"></a>Ausführen mehrerer Abfragen mithilfe des interaktiven Modus
Sie können mehrere Abfragen mithilfe des **interaktiven** Modus ausführen. Führen Sie zum Aktivieren des interaktiven Modus den folgenden Befehl aus:

```azurecli
az postgres flexible-server connect -n <servername> -u <username> -p "<password>" -d <databasename>
```

**Beispiel:**

```azurecli
az postgresql flexible-server connect -n postgresdemoserver -u dbuser -p "dbpassword" -d flexibleserverdb --interactive
```

Die **psql**-Shelloberfläche wird wie folgt angezeigt:

```bash
Command group 'postgres flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Password for earthyTurtle7:
Server: PostgreSQL 12.5
Version: 3.0.0
Chat: https://gitter.im/dbcli/pgcli
Home: http://pgcli.com
postgres> create database pollsdb;
CREATE DATABASE
Time: 0.308s
postgres> exit
Goodbye!
Local context is turned on. Its information is saved in working directory C:\sunitha. You can run `az local-context off` to turn it off.
Your preference of  are now saved to local context. To learn more, type in `az local-context --help`
```


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten des Servers](./how-to-manage-server-cli.md)
