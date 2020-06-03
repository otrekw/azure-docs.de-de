---
title: Herstellen einer Verbindung mit und Abfragen von Azure SQL Edge (Vorschau)
description: Informationen zum Herstellen einer Verbindung mit und Abfragen von Azure SQL Edge (Vorschau)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e7034c3f664eeba802341510b109ba9cc57845a8
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235137"
---
# <a name="connect-and-query-azure-sql-edge-preview"></a>Herstellen einer Verbindung mit und Abfragen von Azure SQL Edge (Vorschau)

Nach der Bereitstellung des Azure SQL Edge-Containers können Sie folgendermaßen eine Verbindung mit der SQL-Datenbank-Engine herstellen.

- Innerhalb des Containers.
- Aus einem anderen Docker-Container, der auf demselben Host ausgeführt wird.
- Über den Hostcomputer.
- Von einem beliebigen anderen Clientcomputer im Netzwerk.

## <a name="tools-to-connect-to-azure-sql-edge"></a>Tools zum Herstellen einer Verbindung mit Azure SQL Edge

Verbindungen mit einer Azure SQL Edge-Instanz können mithilfe eines der unten genannten gängigen Tools hergestellt werden.

* [sqlcmd](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools): sqlcmd-Clienttools sind bereits im Azure SQL Edge-Containerimage enthalten. Wenn Sie mithilfe einer interaktiven Bash-Shell mit einem ausgeführten Container verbunden sind, können Sie die Tools lokal ausführen.
* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)
* [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio)
* [Visual Studio Code](https://docs.microsoft.com/sql/visual-studio-code/sql-server-develop-use-vscode)

Um von einem Netzwerkcomputer aus eine Verbindung mit einer Azure SQL Edge-Datenbank-Engine herzustellen, benötigen Sie Folgendes:

- *IP-Adresse oder Netzwerkname des Hostcomputers*: Dies ist der Hostcomputer, auf dem der Azure SQL Edge-Container ausgeführt wird.
- *Hostportzuordnung des Azure SQL Edge-Containers*: Dies ist die Portzuordnung für den Post des Docker-Containers zu einem Port auf dem Host. Innerhalb des Containers wird SQL Edge immer Port 1433 zugeordnet. Dies kann im Rahmen der Azure SQL Edge-Bereitstellung geändert werden. Um die Portnummer zu ändern, aktualisieren Sie die Option „Optionen für Containererstellung“ für das SQL Edge-Modul in Azure IoT Edge. Im unten angegebenen Beispiel wird Port 1433 für den Container Port 1600 auf dem Host zugeordnet.

    ```JSON
    {
        "PortBindings": {
          "1433/tcp": [
            {
              "HostPort": "1600"
            }
          ]
        }
    }
    ```

- *SA-Kennwort für die SQL Edge-Instanz*: Dies ist der Wert, der bei der SQL Edge-Bereitstellung für die Umgebungsvariable **SA_PASSWORD** angegeben wurde.

## <a name="connecting-to-the-database-engine-from-within-the-container"></a>Herstellen einer Verbindung mit der Datenbank-Engine aus dem Container

Die [SQL Server-Befehlszeilentools](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) sind im Azure SQL Edge-Containerimage enthalten. Wenn Sie das Anfügen an den Container mit einer interaktiven Befehlszeile durchführen, können Sie die Tools lokal ausführen.

1. Verwenden Sie den Befehl `docker exec -it`, um in Ihrem laufenden Container eine interaktive Bash-Shell zu starten. Im folgenden Beispiel entspricht `e69e056c702d` der Container-ID.

    ```bash
    docker exec -it <Azure SQL Edge container id or name> /bin/bash
    ```

    > [!TIP]
    > Sie müssen nicht immer die gesamte Container-ID angeben. Sie müssen nur ausreichend Zeichen angeben, um diese eindeutig zu identifizieren. In diesem Beispiel reicht es aus, `e6` oder `e69` anstelle der vollständigen ID zu verwenden.

2. Stellen Sie eine lokale Verbindung mit „sqlcmd“ her. „Sqlcmd“ verwendet nicht automatisch den richtigen Pfad. Sie müssen daher selbst den vollständigen Pfand angeben.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Wenn Sie mit sqlcmd fertig sind, geben Sie `exit` ein.

4. Wenn Sie mit der interaktiven Eingabeaufforderung fertig sind, geben Sie `exit` ein. Der Container wird auch nach dem Beenden der interaktiven Bash-Shell weiter ausgeführt.

## <a name="connect-to-sql-edge-from-another-container-on-the-same-host"></a>Herstellen einer Verbindung mit SQL Edge aus einem anderen Container auf demselben Host

Da zwei sich Container, die auf demselben Host ausgeführt werden, im selben Docker-Netzwerk befinden, kann auf sie problemlos über den Containernamen und die Portadresse des Diensts zugegriffen werden. Wenn Sie z. B. eine Verbindung mit der SQL Edge-Instanz aus einem anderen Python-Modul (Container) auf demselben Host herstellen, können Sie eine Verbindungszeichenfolge ähnlich der folgenden verwenden. Im folgenden Beispiel wird davon ausgegangen, dass SQL Edge für das Lauschen am Standardport konfiguriert ist.

```python

import pyodbc
server = 'MySQLEdgeContainer' # Replace this with the actual name of your SQL Edge Docker container
username = 'sa' # SQL Server username
password = 'MyStrongestP@ssword' # Replace this with the actual SA password from your deployment
database = 'MyEdgeDatabase' # Replace this with the actual database name from your deployment. If you do not have a database created, you can use Master database.
db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
conn = pyodbc.connect(db_connection_string, autocommit=True)

```

## <a name="connect-to-sql-edge-from-another-network-machine"></a>Herstellen einer Verbindung mit SQL Edge von einem anderen Netzwerkcomputer

Wenn Sie von einem anderen Computer im Netzwerk aus eine Verbindung mit der SQL Edge-Instanz herstellen möchten, müssen Sie die IP-Adresse des Docker-Hosts und den Hostport verwenden, dem der SQL Edge-Container zugeordnet ist. Wenn die IP-Adresse des Docker-Hosts beispielsweise „xxx.xxx.xxx.xxx“ lautet und der SQL Edge-Container Hostport *1600* zugeordnet ist, wäre die Server Adresse für die SQL Edge-Instanz **xxx.xxx.xxx.xxx,1600**. Das aktualisierte Python-Skript würde wie folgt aussehen:

```python

import pyodbc
server = 'xxx.xxx.xxx.xxx,1600' # Replace this with the actual name of your SQL Edge Docker container
username = 'sa' # SQL Server username
password = 'MyStrongestP@ssword' # Replace this with the actual SA password from your deployment
database = 'MyEdgeDatabase' # Replace this with the actual database name from your deployment. If you do not have a database created, you can use Master database.
db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
conn = pyodbc.connect(db_connection_string, autocommit=True)

```

Weitere Informationen zum Herstellen einer Verbindung mit einer Instanz von SQL Edge mithilfe von SQL Server Management Studio auf einem Windows-Computer finden Sie [SQL Server Management Studio](https://docs.microsoft.com/sql/linux/sql-server-linux-manage-ssms).

Informationen zum Herstellen einer Verbindung mit einer Instanz von SQL Edge mithilfe von Visual Studio Code auf einem Windows-, Mac- oder Linux-Computer finden Sie unter [Visual Studio Code](https://docs.microsoft.com/sql/visual-studio-code/sql-server-develop-use-vscode).

Informationen zum Herstellen einer Verbindung mit einer Instanz von SQL Edge mithilfe von Azure Data Studio auf einem Windows-, Mac- oder Linux-Computer finden Sie unter [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-server).

## <a name="see-also"></a>Weitere Informationen

[Herstellen einer Verbindung und Ausführen von Abfragen](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#connect-and-query)

[Installieren von SQL Server-Tools unter Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools)
