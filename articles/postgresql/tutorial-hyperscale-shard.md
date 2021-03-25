---
title: 'Tutorial: Sharddaten auf Workerknoten – Hyperscale (Citus) – Azure Database for PostgreSQL'
description: In diesem Tutorial erfahren Sie, wie Sie in Azure Database for PostgreSQL – Hyperscale (Citus) verteilte Tabellen erstellen und die Datenverteilung visualisieren.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 12/16/2020
ms.openlocfilehash: 7d93002af866aa653972182a13ea37d37e912ce8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97630308"
---
# <a name="tutorial-shard-data-on-worker-nodes-in-azure-database-for-postgresql--hyperscale-citus"></a>Tutorial: Sharddaten auf Workerknoten in Azure Database for PostgreSQL – Hyperscale (Citus)

In diesem Tutorial verwenden Sie Azure Database for PostgreSQL – Hyperscale (Citus), um Folgendes zu lernen:

> [!div class="checklist"]
> * Erstellen von Shards mit Hashverteilung
> * Anzeigen der Platzierung von Tabellenshards
> * Identifizieren einer uneinheitlichen Verteilung (Datenschiefe)
> * Erstellen von Einschränkungen für verteilte Tabellen
> * Ausführen von Abfragen für verteilte Daten

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial ist die Ausführung einer Hyperscale (Citus)-Servergruppe mit zwei Workerknoten erforderlich. Wenn Sie keine Servergruppe haben, die ausgeführt wird, arbeiten Sie das Tutorial [Erstellen einer Servergruppe](tutorial-hyperscale-server-group.md) durch, und kehren Sie dann zu diesem Schritt zurück.

## <a name="hash-distributed-data"></a>Daten mit Hashverteilung

Das Verteilen von Tabellenzeilen über mehrere PostgreSQL-Server ist ein wichtiges Verfahren für skalierbare Abfragen in Hyperscale (Citus). Im Zusammenspiel können mehrere Knoten mehr Daten als eine herkömmliche Datenbank enthalten. In vielen Fällen können Sie Worker-CPUs zudem parallel zum Ausführen von Abfragen verwenden.

Als Voraussetzung haben Sie eine Hyperscale (Citus)-Servergruppe mit zwei Workerknoten erstellt.

![Ein Koordinator- und zwei Workerknoten](tutorial-hyperscale-shard/nodes.png)

Mit den Metadatentabellen des Koordinatorknotens werden Workerknoten und verteilte Daten nachverfolgt. In der Tabelle [pg_dist_node](reference-hyperscale-metadata.md#worker-node-table) können Sie die aktiven Workerknoten überprüfen.

```sql
select nodeid from pg_dist_node where isactive;
```
```
 nodeid | nodename
--------+-----------
      1 | 10.0.0.21
      2 | 10.0.0.23
```

> [!NOTE]
> Knotennamen in Hyperscale (Citus) sind interne IP-Adressen in einem virtuellen Netzwerk. Die tatsächlichen Adressen, die angezeigt werden, lauten möglicherweise anders.

### <a name="rows-shards-and-placements"></a>Zeilen, Shards und Platzierungen

Um die CPU- und Speicherressourcen von Workerknoten verwenden zu können, müssen Sie die Tabellendaten über die Servergruppe verteilen.  Beim Verteilen einer Tabelle wird jede Zeile einer logischen Gruppe zugeordnet, die als *Shard* bezeichnet wird. Sie erstellen nun eine Tabelle und verteilen sie:

```sql
-- create a table on the coordinator
create table users ( email text primary key, bday date not null );

-- distribute it into shards on workers
select create_distributed_table('users', 'email');
```

Hyperscale (Citus) weist anhand des Werts der *Verteilungsspalte* (in unserem Fall auf `email` festgelegt) jede Zeile einem Shard zu. Jede Zeile befindet sich in genau einem Shard, und jeder Shard kann mehrere Zeilen enthalten.

![Tabelle „Users“ mit Zeilen, die auf Shards verweisen](tutorial-hyperscale-shard/table.png)

Mit dem Befehl `create_distributed_table()` werden standardmäßig 32 Shards erstellt, wie aus der Metadatentabelle [pg_dist_shard](reference-hyperscale-metadata.md#shard-table) hervorgeht:

```sql
select logicalrelid, count(shardid)
  from pg_dist_shard
 group by logicalrelid;
```
```
 logicalrelid | count
--------------+-------
 users        |    32
```

Hyperscale (Citus) verwendet die Tabelle `pg_dist_shard` zum Zuweisen von Zeilen zu Shards, und zwar anhand eines Hashs des Werts in der Verteilungsspalte. Die Details der Hashfunktion sind in diesem Tutorial nicht wichtig. Wichtig ist jedoch, dass Sie mithilfe von Abfragen anzeigen können, welche Werte welchen Shard-IDs zugeordnet sind:

```sql
-- Where would a row containing hi@test.com be stored?
-- (The value doesn't have to actually be present in users, the mapping
-- is a mathematical operation consulting pg_dist_shard.)
select get_shard_id_for_distribution_column('users', 'hi@test.com');
```
```
 get_shard_id_for_distribution_column
--------------------------------------
                               102008
```

Das Zuordnen von Zeilen zu Shards ist ein rein logischer Vorgang. Shards müssen zum Speichern bestimmten Workerknoten zugewiesen werden. Dies wird in Hyperscale (Citus) als *Shardplatzierung* bezeichnet.

![Workerknoten zugewiesene Shards](tutorial-hyperscale-shard/shard-placement.png)

In der Tabelle [pg_dist_placement](reference-hyperscale-metadata.md#shard-placement-table) können Sie die Shardplatzierung erkennen.
In Verbindung mit den anderen Metadatentabellen, die Sie gesehen haben, ist erkennbar, wo sich jeder Shard befindet.

```sql
-- limit the output to the first five placements

select
    shard.logicalrelid as table,
    placement.shardid as shard,
    node.nodename as host
from
    pg_dist_placement placement,
    pg_dist_node node,
    pg_dist_shard shard
where placement.groupid = node.groupid
  and shard.shardid = placement.shardid
order by shard
limit 5;
```
```
 table | shard  |    host
-------+--------+------------
 users | 102008 | 10.0.0.21
 users | 102009 | 10.0.0.23
 users | 102010 | 10.0.0.21
 users | 102011 | 10.0.0.23
 users | 102012 | 10.0.0.21
```

### <a name="data-skew"></a>Datenschiefe

Eine Servergruppe wird am effizientesten ausgeführt, wenn Sie Daten gleichmäßig auf Workerknoten verteilen und verwandte Daten auf denselben Workerknoten platzieren. In diesem Abschnitt konzentrieren wir uns auf den ersten Teil, die Gleichmäßigkeit der Platzierung.

Um dies zu veranschaulichen, erstellen Sie Beispieldaten für die Tabelle `users`:

```sql
-- load sample data
insert into users
select
    md5(random()::text) || '@test.com',
    date_trunc('day', now() - random()*'100 years'::interval)
from generate_series(1, 1000);
```

Zum Anzeigen der Shardgrößen können Sie für die Shards [Funktionen zur Ermittlung der Tabellengröße](https://www.postgresql.org/docs/current/functions-admin.html#FUNCTIONS-ADMIN-DBSIZE) ausführen.

```sql
-- sizes of the first five shards
select *
from
    run_command_on_shards('users', $cmd$
      select pg_size_pretty(pg_table_size('%1$s'));
    $cmd$)
order by shardid
limit 5;
```
```
 shardid | success | result
---------+---------+--------
  102008 | t       | 16 kB
  102009 | t       | 16 kB
  102010 | t       | 16 kB
  102011 | t       | 16 kB
  102012 | t       | 16 kB
```

Sie sehen, dass die Shards die gleiche Größe haben. Sie haben bereits festgestellt, dass die Platzierungen gleichmäßig auf die Workerknoten verteilt sind. Davon können Sie ableiten, dass die Workerknoten ungefähr gleich viele Zeilen enthalten.

Die Zeilen in der Beispieltabelle `users` sind aufgrund der Eigenschaften der Verteilungsspalte (`email`) gleichmäßig verteilt.

1. Die Anzahl von E-Mail-Adressen war größer oder gleich der Anzahl von Shards.
2. Die Anzahl von Zeilen pro E-Mail-Adresse war vergleichbar (in diesem Fall genau eine Zeile pro Adresse, da die E-Mail-Adresse als Schlüssel deklariert wurde).

Jede Auswahl von Tabelle und Verteilungsspalte, die bei einer Eigenschaft Fehler verursachen, hat eine uneinheitliche Datengröße auf den Workerknoten zur Folge, die sogenannte *Datenschiefe*.

### <a name="add-constraints-to-distributed-data"></a>Hinzufügen von Einschränkungen zu verteilten Daten

Mit Hyperscale (Citus) können Sie weiterhin von der Sicherheit einer relationalen Datenbank (einschließlich [Datenbankeinschränkungen](https://www.postgresql.org/docs/current/ddl-constraints.html)) profitieren.
Es gibt jedoch eine Beschränkung. Aufgrund der Natur verteilter Systeme erstellt Hyperscale (Citus) keine Querverweise für die Eindeutigkeit von Einschränkungen oder die referenzielle Integrität zwischen Workerknoten.

Betrachten Sie die Beispieltabelle `users` zusammen mit einer verwandten Tabelle.

```sql
-- books that users own
create table books (
    owner_email text references users (email),
    isbn text not null,
    title text not null
);

-- distribute it
select create_distributed_table('books', 'owner_email');
```

Aus Effizienzgründen verteilen Sie die Tabelle `books` auf die gleiche Weise wie die Tabelle `users`: anhand der E-Mail-Adresse des Besitzers. Die Verteilung anhand ähnlicher Spaltenwerte wird als [Tabellenzusammenstellung oder auch Colocation](concepts-hyperscale-colocation.md) bezeichnet.

Es gab kein Problem beim Verteilen der Tabelle „Books“ mit einem Fremdschlüssel an die Tabelle „Users“, da sich der Schlüssel in einer Verteilungsspalte befand. Es gäbe jedoch Probleme, wenn Sie `isbn` als Schlüssel verwenden würden:

```sql
-- will not work
alter table books add constraint books_isbn unique (isbn);
```
```
ERROR:  cannot create constraint on "books"
DETAIL: Distributed relations cannot have UNIQUE, EXCLUDE, or
        PRIMARY KEY constraints that do not include the partition column
        (with an equality operator if EXCLUDE).
```

In einer verteilten Tabelle ist es am besten, Spalten zum eindeutigen Modulo der Verteilungsspalte zu machen:

```sql
-- a weaker constraint is allowed
alter table books add constraint books_isbn unique (owner_email, isbn);
```

Aufgrund der oben genannten Beschränkung ist nur „isbn“ eindeutig pro Benutzer. Eine andere Möglichkeit besteht darin, „books“ als [Referenztabelle](howto-hyperscale-modify-distributed-tables.md#reference-tables) und nicht als verteilte Tabelle zu definieren und eine separate verteilte Tabelle zu erstellen, die „books“ mit „users“ assoziiert.

## <a name="query-distributed-tables"></a>Abfragen von verteilten Tabellen

In den vorherigen Abschnitten wurde erläutert, wie verteilte Tabellenzeilen in Shards auf Workerknoten platziert werden. In den meisten Fällen brauchen Sie nicht zu wissen, wie oder wo die Daten in einer Servergruppe gespeichert werden. Hyperscale (Citus) verfügt über ein Ausführungsmodul für verteilte Abfragen, das reguläre SQL-Abfragen automatisch aufteilt. Die Abfragen werden parallel auf den Workerknoten in Datennähe ausgeführt.

Sie können beispielsweise eine Abfrage ausführen, um das Durchschnittsalter der Benutzer zu ermitteln. Dabei wird die verteilte Tabelle `users` auf dem Koordinatorknoten wie eine normale Tabelle behandelt.

```sql
select avg(current_date - bday) as avg_days_old from users;
```
```
    avg_days_old
--------------------
 17926.348000000000
```

![Abfrage über den Koordinatorknoten an die Shards](tutorial-hyperscale-shard/query-fragments.png)

Im Hintergrund erstellt das Hyperscale (Citus)-Ausführungsmodul eine separate Abfrage für jeden Shard, führt sie auf den Workerknoten aus und kombiniert die Ergebnisse. Diesen Vorgang können Sie mit dem PostgreSQL-Befehl „EXPLAIN“ anzeigen:

```sql
explain select avg(current_date - bday) from users;
```
```
                                  QUERY PLAN
----------------------------------------------------------------------------------
 Aggregate  (cost=500.00..500.02 rows=1 width=32)
   ->  Custom Scan (Citus Adaptive)  (cost=0.00..0.00 rows=100000 width=16)
     Task Count: 32
     Tasks Shown: One of 32
     ->  Task
       Node: host=10.0.0.21 port=5432 dbname=citus
       ->  Aggregate  (cost=41.75..41.76 rows=1 width=16)
         ->  Seq Scan on users_102040 users  (cost=0.00..22.70 rows=1270 width=4)
```

Die Ausgabe enthält ein Beispiel für einen Ausführungsplan für ein *Abfragefragment*, das auf Shard 102040 ausgeführt wird (Tabelle `users_102040` auf Worker 10.0.0.21). Die anderen Fragmente werden nicht angezeigt, da sie ähnlich sind. Sie sehen, dass der Workerknoten die Shardtabellen scannt und die Gesamtsumme anwendet. Der Koordinatorknoten kombiniert die Gesamtsummen und ermittelt so das Endergebnis.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine verteilte Tabelle erstellt und Informationen zu Shards und Platzierungen erhalten. Sie haben die Herausforderung bei der Verwendung von Beschränkungen im Zusammenhang mit Eindeutigkeit und Fremdschlüsseln kennengelernt und schließlich erfahren, wie verteilte Abfragen auf hoher Ebene funktionieren.

* Weitere Informationen finden Sie unter [Hyperscale (Citus)-Tabellentypen](concepts-hyperscale-nodes.md).
* Weitere Tipps finden Sie unter [Auswählen einer Verteilungsspalte](concepts-hyperscale-choose-distribution-column.md).
* Informieren Sie sich über die Vorteile der [Tabellenzusammenstellung (Colocation)](concepts-hyperscale-colocation.md).
