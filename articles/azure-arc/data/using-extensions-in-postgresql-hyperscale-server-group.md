---
title: Verwenden von PostgreSQL-Erweiterungen
description: Verwenden von PostgreSQL-Erweiterungen
titleSuffix: Azure Arc enabled data services
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 19b2ec283619df0cc8d3c880cb2df6f53f6fb332
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931292"
---
# <a name="use-postgresql-extensions-in-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Verwenden von PostgreSQL-Erweiterungen in Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen

PostgreSQL eignet sich am besten für die Verwendung mit Erweiterungen. Tatsächlich ist die von Microsoft bereitgestellte und standardmäßig installierte `citus`-Erweiterung ein wichtiges Element der Hyperscalefunktion, da sie Postgres das transparente und knotenübergreifende Konfigurieren von Datenshards ermöglicht.


[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="list-of-extensions"></a>Liste der Erweiterungen
Zusätzlich zu den Erweiterungen in [`contrib`](https://www.postgresql.org/docs/12/contrib.html) umfasst die Liste der in den Containern Ihrer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe verfügbaren Erweiterungen Folgendes:
- `citus`, V: 9,4
- `pg_cron`, V: 1.2
- `plpgsql`, V: 1.0
- `postgis`, V: 3.0.2

Diese Liste wird im Laufe der Zeit weiterentwickelt, und Updates werden in diesem Dokument veröffentlicht. Es ist noch nicht möglich, Erweiterungen hinzuzufügen, die über jene in der oben aufgeführten Liste hinausgehen.

In diesem Leitfaden wird ein Szenario verwendet, in dem zwei dieser Erweiterungen verwendet werden:
- [PostGIS](https://postgis.net/)
- [`pg_cron`](https://github.com/citusdata/pg_cron)


## <a name="manage-extensions"></a>Verwalten von Erweiterungen

### <a name="enable-extensions"></a>Aktivieren von Erweiterungen
Dieser Schritt ist für die Erweiterungen, die Teil von `contrib` sind, nicht erforderlich.
Dies ist das allgemeine Format des Befehls zum Aktivieren von Erweiterungen:

#### <a name="enable-an-extension-at-the-creation-time-of-a-server-group"></a>Aktivieren Sie eine Erweiterung zum Zeitpunkt der Erstellung einer Servergruppe:
```console
azdata arc postgres server create -n <name of your postgresql server group> --extensions <extension names>
```
#### <a name="enable-an-extension-on-an-instance-that-already-exists"></a>Aktivieren Sie eine Erweiterung für eine Instanz, die bereits vorhanden ist:
```console
azdata arc postgres server edit -n <name of your postgresql server group> --extensions <extension names>
```

#### <a name="get-the-list-of-extensions-enabled"></a>Rufen Sie die Liste der aktivierten Erweiterungen ab:
Führen Sie einen der folgenden Befehlen aus.

##### <a name="with-azdata"></a>Verwenden von azdata
```console
azdata arc postgres server show -n <server group name>
```
Scrollen Sie durch die Ausgabe, und beachten Sie die engine\extensions-Abschnitte in den Spezifikationen Ihrer Servergruppe. Zum Beispiel:
```console
"engine": {
      "extensions": [
        {
          "name": "citus"
        },
        {
          "name": "pg_cron"
        }
      ]
    },
```
##### <a name="with-kubectl"></a>Verwenden von kubectl
```console
kubectl describe postgresql-12s/postgres02
```
Scrollen Sie durch die Ausgabe, und beachten Sie die engine\extensions-Abschnitte in den Spezifikationen Ihrer Servergruppe. Zum Beispiel:
```console
Engine:
    Extensions:
      Name:  citus
      Name:  pg_cron
```


### <a name="create-extensions"></a>Erstellen von Erweiterungen:
Stellen Sie mit dem Clienttool Ihrer Wahl eine Verbindung mit Ihrer Servergruppe her, und führen Sie die PostgreSQL-Standardabfrage aus:
```console
CREATE EXTENSION <extension name>;
```

### <a name="get-the-list-of-extension-created-in-your-server-group"></a>Rufen Sie die Liste der in Ihrer Servergruppe erstellten Erweiterungen ab:
Stellen Sie mit dem Clienttool Ihrer Wahl eine Verbindung mit Ihrer Servergruppe her, und führen Sie die PostgreSQL-Standardabfrage aus:
```console
select * from pg_extension;
```

### <a name="drop-an-extension-from-your-server-group"></a>Löschen Sie eine Erweiterung aus der Servergruppe:
Stellen Sie mit dem Clienttool Ihrer Wahl eine Verbindung mit Ihrer Servergruppe her, und führen Sie die PostgreSQL-Standardabfrage aus:
```console
drop extension <extension name>;
```

## <a name="use-the-postgis-and-the-pg_cron-extensions"></a>Verwenden der Erweiterungen PostGIS und Pg_cron

### <a name="the-postgis-extension"></a>PostGIS-Erweiterung

Sie können entweder die PostGIS-Erweiterung für eine vorhandene Servergruppe aktivieren oder eine neue erstellen, bei der die Erweiterung bereits aktiviert ist:

**Aktivieren einer Erweiterung zum Zeitpunkt der Erstellung einer Servergruppe:**
```console
azdata arc postgres server create -n <name of your postgresql server group> --extensions <extension names>

#Example:
azdata arc postgres server create -n pg2 -w 2 --extensions postgis
```

**Aktivieren einer Erweiterung für eine bereits vorhandene Instanz:**
```console
azdata arc postgres server edit -n <name of your postgresql server group> --extensions <extension names>

#Example:
azdata arc postgres server edit --extensions postgis -n pg2
```

Verwenden Sie den folgenden PostgreSQL-Standardbefehl, nachdem Sie mit dem PostgreSQL-Clienttool Ihrer Wahl (z. B. Azure Data Studio) eine Verbindung mit der Instanz hergestellt haben, um zu überprüfen, welche Erweiterungen installiert werden:
```console
select * from pg_extension;
```

Rufen Sie für ein PostGIS-Beispiel zunächst [Beispieldaten](http://duspviz.mit.edu/tutorials/intro-postgis/) vom Department of Urban Studies & Planning des Massachusetts Institute of Technology ab. Möglicherweise müssen Sie zum Installieren und Entpacken `apt-get install unzip` ausführen, wenn Sie die VM zum Testen verwenden.

```console
wget http://duspviz.mit.edu/_assets/data/intro-postgis-datasets.zip
unzip intro-postgis-datasets.zip
```

Stellen Sie eine Verbindung mit der Datenbank her, und erstellen Sie die PostGIS-Erweiterung:

```console
CREATE EXTENSION postgis;
```

> [!NOTE]
> Wenn Sie eine der Erweiterungen im `postgis`-Paket (z. B. `postgis_raster`, `postgis_topology`, `postgis_sfcgal`, `fuzzystrmatch` usw.) verwenden möchten, müssen Sie zuerst die PostGIS-Erweiterung und dann die andere Erweiterung erstellen. Beispiel: `CREATE EXTENSION postgis`; `CREATE EXTENSION postgis_raster`;

Erstellen Sie dann das Schema:

```sql
CREATE TABLE coffee_shops (
  id serial NOT NULL,
  name character varying(50),
  address character varying(50),
  city character varying(50),
  state character varying(50),
  zip character varying(10),
  lat numeric,
  lon numeric,
  geom geometry(POINT,4326)
);
CREATE INDEX coffee_shops_gist ON coffee_shops USING gist (geom);
```

Nun können Sie PostGIS mit der Funktion für horizontales Hochskalieren kombinieren, indem Sie die coffee_shops-Tabelle verteilen:

```sql
SELECT create_distributed_table('coffee_shops', 'id');
```

Laden Sie nun einige Daten:

```console
\copy coffee_shops(id,name,address,city,state,zip,lat,lon) from cambridge_coffee_shops.csv CSV HEADER;
```

Füllen Sie das `geom`-Feld mit den ordnungsgemäß codierten Breiten- und Längengraden des PostGIS-Datentyps `geometry` aus:

```sql
UPDATE coffee_shops SET geom = ST_SetSRID(ST_MakePoint(lon,lat),4326);
```

Nun können Sie die Cafés auflisten, die sich am nächsten des MIT befinden (77 Massachusetts Ave; 42.359055, -71.093500):

```sql
SELECT name, address FROM coffee_shops ORDER BY geom <-> ST_SetSRID(ST_MakePoint(-71.093500,42.359055),4326);
```


### <a name="the-pg_cron-extension"></a>pg_cron-Erweiterung

Aktivieren Sie zusätzlich zu PostGIS auch `pg_cron` für die PostgreSQL-Servergruppe:

```console
azdata postgres server update -n pg2 -ns arc --extensions postgis,pg_cron
```

Beachten Sie, dass dadurch die Knoten neu gestartet und die zusätzlichen Erweiterungen installiert werden, was zwei bis drei Minuten dauern kann.

Nun können Sie noch mal eine Verbindung herstellen und die `pg_cron`-Erweiterung erstellen:

```sql
CREATE EXTENSION pg_cron;
```

Erstellen Sie zu Testzwecken die Tabelle `the_best_coffee_shop`, die einen zufälligen Namen aus der früheren `coffee_shops`-Tabelle annimmt, und legen Sie den Inhalt der Tabelle fest:

```sql
CREATE TABLE the_best_coffee_shop(name text);
```

Sie können `cron.schedule` und einige SQL-Anweisungen verwenden, um einen zufälligen Tabellennamen abzurufen und in `the_best_coffee_shop` zu speichern (dabei temporäre Tabelle zum Speichern eines verteilten Abfrageergebnisses verwenden):

```sql
SELECT cron.schedule('* * * * *', $$
  TRUNCATE the_best_coffee_shop;
  CREATE TEMPORARY TABLE tmp AS SELECT name FROM coffee_shops ORDER BY random() LIMIT 1;
  INSERT INTO the_best_coffee_shop SELECT * FROM tmp;
  DROP TABLE tmp;
$$);
```

Jetzt wird einmal pro Minute ein anderer Name angezeigt:

```sql
SELECT * FROM the_best_coffee_shop;
```

```console
      name
-----------------
 B & B Snack Bar
(1 row)
```

Ausführliche Informationen zur Syntax finden Sie in der [pg_cron-Infodatei](https://github.com/citusdata/pg_cron).

>[!NOTE]
>Das Löschen der `citus`-Erweiterung wird nicht unterstützt. Die `citus`-Erweiterung ist erforderlich, um die Hyperscalefunktion bereitzustellen.

