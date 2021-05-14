---
title: 'Spaltenbasierter Tabellenspeicher (Vorschau): Hyperscale (Citus): Azure Database for PostgreSQL'
description: Komprimieren von Daten mithilfe von spaltenbasiertem Speicher (Vorschau)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: c60c398d49a802dbe051ca37c4aea2092ab5144b
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107023855"
---
# <a name="columnar-table-storage-preview"></a>Spaltenbasierter Tabellenspeicher (Vorschau)

> [!IMPORTANT]
> Spaltenbasierter Tabellenspeicher in Hyperscale (Citus) befindet sich derzeit in der Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Previewfunktionen für PostgreSQL – Hyperscale (Citus)](hyperscale-preview-features.md).

Azure Database for PostgreSQL: Spaltenbasierter Tabellenspeicher wird von Hyperscale (Citus) nur für Analyse- und Data Warehousing-Workloads unterstützt. Wenn Spalten (anstelle von Zeilen) zusammenhängend auf einem Datenträger gespeichert werden, lassen sich die Daten besser komprimieren, und bei Abfragen kann schneller eine Teilmenge von Spalten angefordert werden.

Geben Sie beim Erstellen einer Tabelle `USING columnar` an, um spaltenbasierten Speicher zu verwenden:

```postgresql
CREATE TABLE contestant (
    handle TEXT,
    birthdate DATE,
    rating INT,
    percentile FLOAT,
    country CHAR(3),
    achievements TEXT[]
) USING columnar;
```

Zeilen werden beim Einfügen von Hyperscale (Citus) in Form von „Stripes“ in spaltenbasierten Speicher konvertiert. Jeder Stripe enthält Daten einer einzelnen Transaktion oder 150.000 Zeilen, je nachdem, was geringer ist.  (Die Stripegröße und andere Parameter einer spaltenbasierten Tabelle können mithilfe der Funktion [alter_columnar_table_set](reference-hyperscale-functions.md#alter_columnar_table_set) geändert werden.)

Durch die folgende Anweisung werden beispielsweise alle fünf Zeilen in den gleichen Stripe eingefügt, da alle Werte in einer einzelnen Transaktion eingefügt werden:

```postgresql
-- insert these values into a single columnar stripe

INSERT INTO contestant VALUES
  ('a','1990-01-10',2090,97.1,'XA','{a}'),
  ('b','1990-11-01',2203,98.1,'XA','{a,b}'),
  ('c','1988-11-01',2907,99.4,'XB','{w,y}'),
  ('d','1985-05-05',2314,98.3,'XB','{}'),
  ('e','1995-05-05',2236,98.2,'XC','{a}');
```

Verwenden Sie nach Möglichkeit große Stripes, da spaltenbasierte Daten von Hyperscale (Citus) jeweils separat pro Stripe komprimiert werden. Informationen zu einer spaltenbasierten Tabelle (beispielsweise die Komprimierungsrate, die Anzahl von Stripes und die durchschnittliche Anzahl von Zeilen pro Stripe) können mithilfe von `VACUUM VERBOSE` angezeigt werden:

```postgresql
VACUUM VERBOSE contestant;
```
```
INFO:  statistics for "contestant":
storage id: 10000000000
total file size: 24576, total data size: 248
compression rate: 1.31x
total row count: 5, stripe count: 1, average rows per stripe: 5
chunk count: 6, containing data for dropped columns: 0, zstd compressed: 6
```

Der Ausgabe können Sie entnehmen, dass von Hyperscale (Citus) der zstd-Komprimierungsalgorithmus verwendet wurde, um eine 1,31-fache Datenkomprimierung zu erreichen. Die Komprimierungsrate gibt das Verhältnis zwischen der Größe der eingefügten, im Arbeitsspeicher gestageten Daten und der Größe der gleichen Daten nach der Komprimierung im späteren Stripe an.

Aufgrund der Art der Messung entspricht die Komprimierungsrate möglicherweise nicht dem Größenunterschied zwischen zeilen- und spaltenbasiertem Speicher für eine Tabelle. Zur Ermittlung des tatsächlichen Unterschieds müssen Sie eine zeilen- und eine spaltenbasierte Tabelle mit den gleichen Daten erstellen und sie vergleichen:

```postgresql
CREATE TABLE contestant_row AS
    SELECT * FROM contestant;

SELECT pg_total_relation_size('contestant_row') as row_size,
       pg_total_relation_size('contestant') as columnar_size;
```
```
 row_size | columnar_size
----------+---------------
    16384 |         24576
```

Bei unserer kleinen Tabelle benötigt der spaltenbasierte Speicher sogar mehr Platz. Mit zunehmender Datenmenge kommt jedoch die Komprimierung stärker zum Tragen.

## <a name="example"></a>Beispiel

Spaltenbasierter Speicher funktioniert gut mit Tabellenpartitionierung. Ein Beispiel finden Sie in der Communitydokumentation zur Citus-Engine: [Archivieren mit spaltenbasiertem Speicher](https://docs.citusdata.com/en/stable/use_cases/timeseries.html#archiving-with-columnar-storage).

## <a name="gotchas"></a>Häufige Fehler

* Bei spaltenbasiertem Speicher werden die Daten pro Stripe komprimiert. Stripes werden pro Transaktion erstellt. Wenn Sie also eine einzelne Zeile pro Transaktion einfügen, werden die einzelnen Zeilen jeweils in eigenen Stripes platziert. Komprimierung und Leistung einzeiliger Stripes sind schlechter als eine zeilenbasierte Tabelle. Fügen Sie Daten immer per Massenvorgang in eine spaltenbasierte Tabelle ein.
* Wenn Sie den Fehler machen und eine Reihe kleiner Stripes in Spalten konvertieren, kommen Sie nicht mehr weiter.
  In diesem Fall können Sie nur noch eine neue spaltenbasierte Tabelle erstellen und die Originaldaten in einer einzelnen Transaktion kopieren:
  ```postgresql
  BEGIN;
  CREATE TABLE foo_compacted (LIKE foo) USING columnar;
  INSERT INTO foo_compacted SELECT * FROM foo;
  DROP TABLE foo;
  ALTER TABLE foo_compacted RENAME TO foo;
  COMMIT;
  ```
* Grundsätzlich nicht komprimierbare Daten können zwar ein Problem darstellen, spaltenbasierter Speicher ist jedoch auch nützlich, wenn spezifische Spalten ausgewählt werden. Die anderen Spalten müssen nicht in den Arbeitsspeicher geladen werden.
* Bei partitionierten Tabellen mit einer Mischung aus zeilen- und spaltenbasierten Partitionen müssen Aktualisierungen sorgfältig ausgerichtet werden. Verwenden Sie Filter, um den Vorgang auf die Zeilenpartitionen zu beschränken.
   * Wenn der Vorgang auf eine bestimmte Zeilenpartition (beispielsweise `UPDATE p2 SET i = i + 1`) ausgerichtet wird, ist er erfolgreich. Wird er dagegen auf eine bestimmte Spaltenpartition (beispielsweise `UPDATE p1 SET i = i + 1`) ausgerichtet, tritt ein Fehler auf.
   * Wenn der Vorgang auf die partitionierte Tabelle ausgerichtet wird und über eine WHERE-Klausel verfügt, durch die alle Spaltenpartitionen ausgeschlossen werden (beispielsweise `UPDATE parent SET i = i + 1 WHERE timestamp = '2020-03-15'`), ist er erfolgreich.
   * Wird der Vorgang auf die partitionierte Tabelle ausgerichtet, ohne jedoch nach den Partitionsschlüsselspalten zu filtern, tritt ein Fehler auf. Selbst wenn WHERE-Klauseln vorhanden sind, die dafür sorgen, dass Zeilen nur in Spaltenpartitionen abgeglichen werden, reicht das nicht aus. Der Partitionsschlüssel muss ebenfalls gefiltert werden.

## <a name="limitations"></a>Einschränkungen

Dieses Feature hat weiterhin eine Reihe von erheblichen Einschränkungen. Weitere Informationen finden Sie unter [Spaltenbasierter Speicher](concepts-hyperscale-limits.md#columnar-storage).

## <a name="next-steps"></a>Nächste Schritte

* Ein Beispiel für spaltenbasierten Speicher finden Sie in einem [Zeitreihentutorial](https://docs.citusdata.com/en/stable/use_cases/timeseries.html#archiving-with-columnar-storage) (externer Link) von Citus.
