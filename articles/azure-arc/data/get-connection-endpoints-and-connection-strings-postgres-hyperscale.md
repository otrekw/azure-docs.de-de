---
title: Abrufen von Verbindungsendpunkten und Erstellen von Verbindungszeichenfolgen für eine Arc-fähige PostgreSQL Hyperscale-Servergruppe
titleSuffix: Azure Arc enabled data services
description: Abrufen von Verbindungsendpunkten und Erstellen von Verbindungszeichenfolgen für eine Arc-fähige PostgreSQL Hyperscale-Servergruppe
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 93b0547ba25f6534593a0a016ebfa5cbe4d2be2e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931800"
---
# <a name="get-connection-endpoints-and-form-connection-strings-for-your-arc-enabled-postgresql-hyperscale-server-group"></a>Abrufen von Verbindungsendpunkten und Erstellen von Verbindungszeichenfolgen für eine Arc-fähige PostgreSQL Hyperscale-Servergruppe

In diesem Artikel wird erläutert, wie Sie die Verbindungsendpunkte für Ihre Servergruppe abrufen und Verbindungszeichenfolgen erstellen, die Sie mit Ihren Anwendungen und/oder Tools verwenden.


[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="get-connection-end-points"></a>Abrufen von Verbindungsendpunkten:

### <a name="from-cli-with-azdata"></a>Über die CLI mit azdata
#### <a name="1-connect-to-your-arc-data-controller"></a>1. Herstellen einer Verbindung mit dem Arc-Datencontroller:
- Wenn Sie bereits eine Sitzung auf dem Arc-Datencontroller-Host geöffnet haben: Führen Sie den folgenden Befehl aus:
```console
azdata login
```

- Wenn auf dem Arc-Datencontroller-Host keine Sitzungen geöffnet sind, führen Sie den folgenden Befehl aus: 
```console
azdata login --endpoint https://<external IP address of host/data controller>:30080
```

#### <a name="2-show-the-connection-endpoints"></a>2. Anzeigen der Verbindungsendpunkte
Führen Sie den folgenden Befehl aus:
```console
azdata arc postgres endpoint list -n <server group name>
```
Es wird eine ähnliche Ausgabe wie die folgende zurückgegeben:
```console
[
  {
    "Description": "PostgreSQL Instance",
    "Endpoint": "postgresql://postgres:<replace with password>@12.345.123.456:1234"
  },
  {
    "Description": "Log Search Dashboard",
    "Endpoint": "https://12.345.123.456:12345/kibana/app/kibana#/discover?_a=(query:(language:kuery,query:'custom_resource_name:\"postgres01\"'))"
  },
  {
    "Description": "Metrics Dashboard",
    "Endpoint": "https://12.345.123.456:12345/grafana/d/postgres-metrics?var-Namespace=arc3&var-Name=postgres01"
  }
]
```
Verwenden Sie diese Endpunkte für die folgenden Zwecke:
- Erstellen von Verbindungszeichenfolgen und Herstellen einer Verbindung mit Ihren Clienttools oder -anwendungen
- Zugriff auf die Grafana- und Kibana-Dashboards über Ihren Browser

Beispielsweise können Sie den Endpunkt _PostgreSQL-Instanz_ verwenden, um über psql eine Verbindung mit Ihrer Servergruppe herzustellen. Zum Beispiel:
```console
psql postgresql://postgres:MyPassworkd@12.345.123.456:1234
psql (10.14 (Ubuntu 10.14-0ubuntu0.18.04.1), server 12.4 (Ubuntu 12.4-1.pgdg16.04+1))
WARNING: psql major version 10, server major version 12.
         Some psql features might not work.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=#
```
> [!NOTE]
>
> - Das Kennwort des am Endpunkt namens _PostgreSQL-Instanz_ angegebenen _postgresql_-Benutzers ist das Kennwort, das Sie beim Erstellen der Servergruppe angegeben haben.
> - Hinweis zu azdata: Die Leasedauer für die Verbindung beträgt ca. 10 Stunden. Danach müssen Sie die Verbindung erneut herstellen. Wenn die Leasedauer abgelaufen ist, erhalten Sie die folgende Fehlermeldung, wenn Sie versuchen, einen Befehl mit azdata auszuführen (außer bei azdata login): _FEHLER: (401)_ 
> _Reason: Unauthorized_
> _HTTP response headers: HTTPHeaderDict({'Date': 'Sun, 06 Sep 2020 16:58:38 GMT', 'Content-Length': '0', 'WWW-Authenticate': '_ 
> _Basic realm="Login_ credentials required", Bearer error="invalid_token", error_description="The token is expired"'})_ (Fehlermeldung 401, dass das Token abgelaufen ist) Wenn dies passiert, müssen Sie wie weiter oben erläutert erneut eine Verbindung mit azdata herstellen.

## <a name="from-cli-with-kubectl"></a>Über die CLI mit kubectl
- Wenn die Servergruppe Version 12 (Standard) von Postgres verwendet, führen Sie den folgenden Befehl aus:
```console
kubectl get postgresql-12/<server group name>
```
- Wenn die Servergruppe Version 11 von Postgres verwendet, führen Sie den folgenden Befehl aus:
```console
kubectl get postgresql-11/<server group name>
```

Mit diesen Befehlen wird eine Ausgabe ähnlich der folgenden erzeugt. Diese Informationen können Sie verwenden, um Ihre Verbindungszeichenfolgen zu erstellen:
```console
NAME         STATE   READY-PODS   EXTERNAL-ENDPOINT   AGE
postgres01   Ready   3/3          123.456.789.4:31066      5d20h
``` 


## <a name="form-connection-strings"></a>Erstellen von Verbindungszeichenfolgen:
Verwenden Sie die folgende Tabelle mit Vorlagen für Verbindungszeichenfolgen für Ihre Servergruppe. Sie können sie kopieren und einfügen sowie nach Bedarf anpassen:

### <a name="adonet"></a>ADO.NET

```ado.net
Server=192.168.1.121;Database=postgres;Port=24276;User Id=postgres;Password={your_password_here};Ssl Mode=Require;`
```

### <a name="c-libpq"></a>C++ (libpq)

```cpp
host=192.168.1.121 port=24276 dbname=postgres user=postgres password={your_password_here} sslmode=require
```

### <a name="jdbc"></a>JDBC

```jdbc
jdbc:postgresql://192.168.1.121:24276/postgres?user=postgres&password={your_password_here}&sslmode=require
```

### <a name="nodejs"></a>Node.js

```node.js
host=192.168.1.121 port=24276 dbname=postgres user=postgres password={your_password_here} sslmode=require
```

### <a name="php"></a>PHP

```php
host=192.168.1.121 port=24276 dbname=postgres user=postgres password={your_password_here} sslmode=require
```

### <a name="psql"></a>psql

```psql
psql "host=192.168.1.121 port=24276 dbname=postgres user=postgres password={your_password_here} sslmode=require"
```

### <a name="python"></a>Python

```python
dbname='postgres' user='postgres' host='192.168.1.121' password='{your_password_here}' port='24276' sslmode='true'
```

### <a name="ruby"></a>Ruby

```ruby
host=192.168.1.121; dbname=postgres user=postgres password={your_password_here} port=24276 sslmode=require
```

### <a name="web-app"></a>Webanwendung

```webapp
Database=postgres; Data Source=192.168.1.121; User Id=postgres; Password={your_password_here}
```

## <a name="next-steps"></a>Nächste Schritte
- Hier erfahren Sie mehr über das [horizontale Skalieren (Hinzufügen von Workerknoten)](scale-out-postgresql-hyperscale-server-group.md) Ihrer Servergruppe.
- Hier erfahren Sie mehr über das [zentrale Hoch- oder Herunterskalieren (Erhöhen oder Verringern von Arbeitsspeicher/virtuellen Kernen)](scale-up-down-postgresql-hyperscale-server-group-using-cli.md) Ihrer Servergruppe.


