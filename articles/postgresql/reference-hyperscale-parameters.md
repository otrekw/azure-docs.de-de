---
title: Serverparameter – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Parameter in der SQL-API für Hyperscale (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: reference
ms.date: 08/10/2020
ms.openlocfilehash: 07f966c7b0be542f848f1a0a4eaf2b5549735b4b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91336240"
---
# <a name="server-parameters"></a>Serverparameter

Es gibt verschiedene Serverparameter, die sich auf das Verhalten von Hyperscale (Citus) auswirken, und zwar sowohl Parameter aus Standard-PostgreSQL als auch für Hyperscale (Citus) spezifische Parameter.
Diese Parameter können Sie im Azure-Portal für eine Hyperscale (Citus)-Servergruppe festlegen. Wählen Sie hierzu unter der Kategorie **Einstellungen** die Option **Workerknotenparameter** oder die Option **Koordinatorknotenparameter** aus. Auf diesen Seiten können Sie Parameter für alle Workerknoten oder nur für den Koordinatorknoten festlegen.

## <a name="hyperscale-citus-parameters"></a>Hyperscale (Citus)-Parameter

> [!NOTE]
>
> Hyperscale (Citus)-Servergruppen, auf denen ältere Versionen der Citus-Engine ausgeführt werden, bieten möglicherweise nicht alle unten aufgeführten Parameter.

### <a name="general-configuration"></a>Allgemeine Konfiguration

#### <a name="citususe_secondary_nodes-enum"></a>citus.use\_secondary\_nodes (Enumeration)

Legt die Richtlinie fest, die beim Auswählen von Knoten für SELECT-Abfragen verwendet werden soll. Wenn sie auf „always“ festgelegt ist, fragt der Planer nur Knoten ab, die in [pg_dist_node](reference-hyperscale-metadata.md#worker-node-table) als „secondary“ noderole gekennzeichnet sind.

Die unterstützten Werte für diese Enumeration lauten:

-   **never:** (Standardwert) Alle Lesevorgänge erfolgen auf primären Knoten.
-   **always:** Lesevorgänge werden stattdessen für sekundäre Knoten ausgeführt, und insert-/update-Anweisungen sind deaktiviert.

#### <a name="cituscluster_name-text"></a>citus.cluster\_name (Text)

Informiert den Koordinatorknotenplaner, welchen Cluster er koordiniert. Nachdem cluster\_name festgelegt wurde, fragt der Planer die Workerknoten nur in diesem Cluster ab.

#### <a name="citusenable_version_checks-boolean"></a>citus.enable\_version\_checks (boolescher Wert)

Zum Aktualisieren der Version von Hyperscale (Citus) ist ein Neustart des Servers erforderlich (um die neue freigegebene Bibliothek einzubinden), gefolgt vom Befehl ALTER EXTENSION UPDATE.  Wenn nicht beide Schritte ausgeführt werden, kann dies potenziell zu Fehlern oder Abstürzen führen.
Hyperscale (Citus) überprüft daher die Version des Codes und die Übereinstimmung mit der Erweiterung und gibt Fehler aus, wenn dies nicht der Fall ist.

Dieser Wert ist standardmäßig TRUE und für den Koordinator gültig. In seltenen Fällen ist es für komplexe Upgradeprozesse möglicherweise erforderlich, diesen Parameter auf FALSE festzulegen und so die Überprüfung zu deaktivieren.

#### <a name="cituslog_distributed_deadlock_detection-boolean"></a>citus.log\_distributed\_deadlock\_detection (boolescher Wert)

Gibt an, ob die Verarbeitung verteilter Deadlockerkennungen im Serverprotokoll protokolliert werden soll. Der Standardwert ist FALSE.

#### <a name="citusdistributed_deadlock_detection_factor-floating-point"></a>citus.distributed\_deadlock\_detection\_factor (Gleitkommawert)

Legt die Wartezeit bis zur Suche nach verteilten Deadlocks fest. Die Wartezeit besteht aus diesem Wert, multipliziert mit der [deadlock\_timeout](https://www.postgresql.org/docs/current/static/runtime-config-locks.html)-Einstellung von PostgreSQL. Der Standardwert ist `2`. Der Wert `-1` deaktiviert die Erkennung verteilter Deadlocks.

#### <a name="citusnode_connection_timeout-integer"></a>citus.node\_connection\_timeout (Integerwert)

Der `citus.node_connection_timeout`-GUC legt die maximale Dauer (in Millisekunden) für das Warten auf die Verbindungsherstellung fest. Hyperscale (Citus) löst einen Fehler aus, wenn das Timeout abläuft, bevor mindestens eine Workerverbindung hergestellt wurde. Dieser GUC wirkt sich auf Verbindungen zwischen dem Koordinator und Workern sowie Workern untereinander aus.

-   Standardwert: 5 Sekunden
-   Minimum: 10 Millisekunden
-   Maximum: eine Stunde

```postgresql
-- set to 30 seconds
ALTER DATABASE foo
SET citus.node_connection_timeout = 30000;
```

### <a name="query-statistics"></a>Abfragestatistik

#### <a name="citusstat_statements_purge_interval-integer"></a>citus.stat\_statements\_purge\_interval (Integerwert)

Legt die Häufigkeit fest, mit der der Wartungdaemon Datensätze aus [citus_stat_statements](reference-hyperscale-metadata.md#query-statistics-table) entfernt, für die in `pg_stat_statements` keine Übereinstimmung vorliegt. Mit diesem Konfigurationswert wird das Zeitintervall zwischen Bereinigungen in Sekunden festgelegt. Der Standardwert ist 10. Der Wert 0 deaktiviert Bereinigungen.

```psql
SET citus.stat_statements_purge_interval TO 5;
```

Dieser Parameter ist für den Koordinator wirksam und kann zur Laufzeit geändert werden.

### <a name="data-loading"></a>Laden von Daten

#### <a name="citusmulti_shard_commit_protocol-enum"></a>citus.multi\_shard\_commit\_protocol (Enumeration)

Legt das Commitprotokoll fest, das beim Ausführen von COPY für eine Tabelle mit Hashverteilung verwendet werden soll. Bei jeder einzelnen Shardplatzierung wird COPY in einem Transaktionsblock ausgeführt, um sicherzustellen, dass keine Daten erfasst werden, wenn während des Kopiervorgangs ein Fehler auftritt. Es gibt jedoch einen bestimmten Fehlerfall, bei dem COPY bei allen Platzierungsvorgängen erfolgreich ist, aber vor dem Commit aller Transaktionen tritt ein Fehler (Hardwarefehler) auf. Dieser Parameter kann verwendet werden, um in diesem Fall Datenverluste zu verhindern, indem zwischen den folgenden Commitprotokollen ausgewählt wird:

-   **2pc:** (Standardwert) Die Transaktionen, bei denen COPY für die Shardplatzierungen ausgeführt wird, werden zuerst mit einem [Zweiphasencommit](http://www.postgresql.org/docs/current/static/sql-prepare-transaction.html) von PostgreSQL vorbereitet und dann committet. Fehlgeschlagene Commits können mithilfe von COMMIT PREPARED bzw. ROLLBACK PREPARED manuell wiederhergestellt oder abgebrochen werden.
    Bei der Verwendung von 2pc sollte der Wert von [max\_prepared\_transactions](http://www.postgresql.org/docs/current/static/runtime-config-resource.html) für alle Worker erhöht werden (in der Regel auf denselben Wert wie max\_connections).
-   **1pc:** Die Transaktionen, bei denen für die Shardplatzierung COPY ausgeführt wird, werden in einer Runde committet. Daten gehen möglicherweise verloren, wenn ein Commit fehlschlägt, nachdem COPY für alle Platzierungen erfolgreich war (selten).

#### <a name="citusshard_replication_factor-integer"></a>citus.shard\_replication\_factor (Integerwert)

Legt den Replikationsfaktor für Shards fest, d. h. die Anzahl der Knoten, auf denen Shards platziert werden. Der Standardwert ist 1. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig. Der ideale Wert für diesen Parameter hängt von der Größe des Clusters und der Rate der Knotenfehler ab.  Beispielsweise können Sie diesen Replikationsfaktor erhöhen, wenn Sie große Cluster ausführen und Knotenfehler häufiger beobachtet werden.

#### <a name="citusshard_count-integer"></a>citus.shard\_count (Integerwert)

Legt die Anzahl der Shards für Tabellen mit Hashpartitionierung fest. Der Standardwert ist 32.  Dieser Wert wird vom [create_distributed_table](reference-hyperscale-functions.md#create_distributed_table)-UDF verwendet, wenn Tabellen mit Hashpartitionierung erstellt werden. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="citusshard_max_size-integer"></a>citus.shard\_max\_size (Integerwert)

Legt die maximale Größe fest, bis zu der ein Shard vergrößert wird, bevor er geteilt wird. Der Standardwert ist 1 GB. Wenn die Größe der Quelldatei (die für Staging verwendet wird) für einen Shard diesen Konfigurationswert überschreitet, stellt die Datenbank sicher, dass ein neuer Shard erstellt wird. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

### <a name="planner-configuration"></a>Planerkonfiguration

#### <a name="cituslimit_clause_row_fetch_count-integer"></a>citus.limit\_clause\_row\_fetch\_count (Integerwert)

Legt die Anzahl der Zeilen fest, die pro Task für die Optimierung der limit-Klausel abgerufen werden.
In einigen Fällen müssen SELECT-Abfragen mit Limit-Klauseln möglicherweise alle Zeilen aus jedem Task abrufen, um Ergebnisse zu generieren. In diesen Fällen, in denen eine Näherung zu sinnvollen Ergebnissen führen würde, legt dieser Konfigurationswert die Anzahl der Zeilen fest, die aus jedem Shard abgerufen werden sollen. Limit-Näherungen sind standardmäßig deaktiviert, und dieser Parameter ist auf -1 festgelegt. Dieser Wert kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="cituscount_distinct_error_rate-floating-point"></a>citus.count\_distinct\_error\_rate (Gleitkommawert)

Hyperscale (Citus) kann die count(distinct)-Näherung mit der postgresql-hll-Erweiterung berechnen. Mit diesem Konfigurationseintrag wird die gewünschte Fehlerrate beim Berechnen von count(distinct) festgelegt. 0,0 ist die Standardeinstellung und deaktiviert Näherungswerte für count(distinct). 1,0 bietet keine Garantie bezüglich der Genauigkeit der Ergebnisse. Es wird empfohlen, diesen Parameter auf 0,005 festzulegen, um optimale Ergebnisse zu erzielen. Dieser Wert kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="citustask_assignment_policy-enum"></a>citus.task\_assignment\_policy (Enumeration)

> [!NOTE]
> Dieser GUC ist nur anwendbar, wenn [shard_replication_factor](reference-hyperscale-parameters.md#citusshard_replication_factor-integer) größer als 1 ist, oder für Abfragen für [reference_tables](concepts-hyperscale-distributed-data.md#type-2-reference-tables).

Legt die Richtlinie fest, die beim Zuweisen von Tasks an Worker verwendet wird. Der Koordinator weist Workern auf der Grundlage von Shardspeicherorten Tasks zu. Dieser Konfigurationswert gibt die Richtlinie an, die bei diesen Zuweisungen verwendet werden soll.
Derzeit gibt es drei mögliche Taskzuweisungsrichtlinien, die verwendet werden können.

-   **greedy:** Die greedy-Richtlinie ist die Standardeinstellung und zielt darauf ab, Tasks gleichmäßig auf Worker zu verteilen.
-   **round-robin:** Die round-robin-Richtlinie weist Workern Tasks in einem Roundrobinverfahren zu, das ein Rotationsprinzip verwendet. Diese Richtlinie ermöglicht eine bessere Clusterauslastung, wenn die Anzahl der Shards für eine Tabelle im Vergleich zur Anzahl der Worker gering ist.
-   **first-replica:** Die first-replica-Richtlinie weist Tasks auf der Grundlage der Einfügungsreihenfolge der Platzierungen (Replikate) für die Shards zu. Anders ausgedrückt: Die Fragmentabfrage für einen Shard wird dem Worker zugewiesen, der über das erste Replikat dieses Shards verfügt.
    Mit dieser Methode können Sie sicherstellen, welche Shards auf welchen Knoten verwendet werden (d. h. stärkere Speicherresidenzgarantien bereitstellen).

Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

### <a name="intermediate-data-transfer"></a>Übertragung von Zwischendaten

#### <a name="citusbinary_worker_copy_format-boolean"></a>citus.binary\_worker\_copy\_format (boolescher Wert)

Verwenden Sie das binäre Kopierformat, um Zwischendaten zwischen Workern zu übertragen.
Bei großen Tabellenjoins muss Hyperscale (Citus) möglicherweise dynamisch eine Neupartitionierung ausführen und Daten zwischen verschiedenen Workern mischen. Standardmäßig werden diese Daten im Textformat übertragen. Durch Aktivieren dieses Parameters wird die Datenbank angewiesen, das binäre Serialisierungsformat von PostgreSQL zum Übertragen dieser Daten zu verwenden. Dieser Parameter ist für die Worker gültig und muss in der Datei „postgresql.conf“ geändert werden. Nachdem Sie die Konfigurationsdatei bearbeitet haben, können Benutzer ein SIGHUP-Signal senden oder den Server neu starten, damit diese Änderung wirksam wird.

#### <a name="citusbinary_master_copy_format-boolean"></a>citus.binary\_master\_copy\_format (boolescher Wert)

Verwenden Sie das binäre Kopierformat, um Daten zwischen dem Koordinator und Workern zu übertragen. Beim Ausführen verteilter Abfragen übertragen die Worker ihre Zwischenergebnisse für die endgültige Aggregation an den Koordinator. Standardmäßig werden diese Daten im Textformat übertragen. Durch Aktivieren dieses Parameters wird die Datenbank angewiesen, das binäre Serialisierungsformat von PostgreSQL zum Übertragen dieser Daten zu verwenden.
Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="citusmax_intermediate_result_size-integer"></a>citus.max\_intermediate\_result\_size (Integerwert)

Die maximale Größe in KB von Zwischenergebnissen für CTEs, die zur Ausführung nicht auf Workerknoten gepusht werden können, und für komplexe Unterabfragen. Der Standardwert ist 1 GB. Der Wert -1 bedeutet, dass kein Grenzwert gilt.
Abfragen, die den Grenzwert überschreiten, werden abgebrochen und führen zu einer Fehlermeldung.

### <a name="ddl"></a>DDL

#### <a name="citusenable_ddl_propagation-boolean"></a>citus.enable\_ddl\_propagation (boolescher Wert)

Gibt an, ob DDL-Änderungen automatisch vom Koordinator an alle Worker weitergegeben werden sollen. Der Standardwert lautet „true“. Da einige Schemaänderungen eine ausschließliche Zugriffssperre für Tabellen erfordern und die automatische Weitergabe sequenziell für alle Worker gilt, kann ein Hyperscale (Citus)-Cluster vorübergehend weniger reaktionsfähig sein. Sie können diese Einstellung deaktivieren und Änderungen manuell weitergeben.

### <a name="executor-configuration"></a>Executor-Konfiguration

#### <a name="general"></a>Allgemein

##### <a name="citusall_modifications_commutative"></a>citus.all\_modifications\_commutative

Hyperscale (Citus) erzwingt Kommutativitätsregeln und ruft geeignete Sperren für Änderungsvorgänge ab, um die Richtigkeit des Verhaltens zu gewährleisten. Beispielsweise wird davon ausgegangen, dass eine INSERT-Anweisung mit einer anderen INSERT-Anweisung, aber nicht mit einer UPDATE- oder DELETE-Anweisung Kommutativität aufweist. Entsprechend wird auch davon ausgegangen, dass eine UPDATE- oder DELETE-Anweisung keine Kommutativität mit einer anderen UPDATE- oder DELETE-Anweisung verwendet. Diese Vorsichtsmaßnahme bedeutet, dass Hyperscale (Citus) für UPDATE- und DELETE-Vorgänge stärkere Sperren abrufen muss.

Wenn Sie über UPDATE-Anweisungen verfügen, die mit INSERT- oder anderen UPDATE-Vorgängen Kommutativität verwenden, können Sie diese Kommutativitätsannahmen durch Festlegen dieses Parameters auf TRUE lockern. Wenn dieser Parameter auf TRUE festgelegt ist, werden alle Befehle als kommutativ angesehen und beanspruchen eine gemeinsame Sperre, was den Gesamtdurchsatz verbessern kann. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

##### <a name="citusremote_task_check_interval-integer"></a>citus.remote\_task\_check\_interval (Integerwert)

Legt die Häufigkeit fest, mit der Hyperscale (Citus) die Status von Aufträgen überprüft, die vom taskt-racker-Executor verwaltet werden. Der Standardwert lautet 10 ms. Der Koordinator weist Workern Tasks zu und überprüft sie regelmäßig hinsichtlich des Fortschritts der einzelnen Tasks. Mit diesem Konfigurationswert wird das Zeitintervall zwischen zwei aufeinanderfolgenden Überprüfungen festgelegt. Dieser Parameter ist für den Koordinator wirksam und kann zur Laufzeit festgelegt werden.

##### <a name="citustask_executor_type-enum"></a>citus.task\_executor\_type (Enumeration)

Hyperscale (Citus) verfügt über drei Executor-Typen zum Ausführen verteilter SELECT-Abfragen.  Der gewünschte Executor kann durch Festlegen dieses Konfigurationsparameters ausgewählt werden. Die folgenden Werte sind für diesen Parameter zulässig:

-   **adaptive:** Der Standardwert. Er eignet sich optimal für schnelle Antworten auf Abfragen, die Aggregationen und Joins mit Colocation umfassen, die auf mehrere Shards verteilt sind.
-   **task-tracker:** Der task-tracker-Executor eignet sich gut für zeitintensive komplexe Abfragen, bei denen Daten über mehrere Workerknoten hinweg gemischt werden müssen, sowie für effiziente Ressourcenverwaltung.
-   **real-time:** (veraltet) Dient einem ähnlichen Zweck wie der adaptive Executor, ist aber weniger flexibel und kann zu mehr Verbindungsfehlern auf Workerknoten führen.

Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

##### <a name="citusmulti_task_query_log_level-enum-multi_task_logging"></a>citus.multi\_task\_query\_log\_level (Enumeration) {#multi_task_logging}

Legt einen Protokolliergrad für jede Abfrage fest, die mehrere Tasks generiert (d. h., die mehr als einen Shard betrifft). Die Protokollierung ist während der Migration einer mehrinstanzenfähigen Anwendung nützlich, da Sie für solche Abfragen einen Fehler oder eine Warnung anzeigen können, um diese zu ermitteln und ihnen einen tenant\_id-Filter hinzuzufügen. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig. Der Standardwert für diesen Parameter ist \'off\'.

Die unterstützten Werte für diese Enumeration lauten:

-   **off:** Deaktiviert die Protokollierung von Abfragen, die mehrere Tasks generieren (d. h. mehrere Shards umfassen).
-   **debug:** Protokolliert die Anweisung mit dem DEBUG-Schweregrad.
-   **log:** Protokolliert die Anweisung mit dem LOG-Schweregrad. Die Protokollzeile enthält die SQL-Abfrage, die ausgeführt wurde.
-   **notice:** Protokolliert die Anweisung mit dem NOTICE-Schweregrad.
-   **warning:** Protokolliert die Anweisung mit dem WARNING-Schweregrad.
-   **error:** Protokolliert die Anweisung mit dem ERROR-Schweregrad.

Es kann nützlich sein, `error` während der Entwicklungstests und einen niedrigeren Protokolliergrad wie `log` während der tatsächlichen Bereitstellung in der Produktion zu verwenden.
Wenn Sie `log` auswählen, werden Abfragen für mehrere Tasks in den Datenbankprotokollen angezeigt. Dabei wird die Abfrage selbst nach \"STATEMENT\" angezeigt.

```
LOG:  multi-task query about to be executed
HINT:  Queries are split to multiple tasks if they have to be split into several queries on the workers.
STATEMENT:  select * from foo;
```

##### <a name="citusenable_repartition_joins-boolean"></a>citus.enable\_repartition\_joins (boolescher Wert)

Normalerweise schlägt der Versuch, Neupartitionierungsjoins mit dem adaptiven Executor durchzuführen, mit einer Fehlermeldung fehl.  Wenn Sie jedoch `citus.enable_repartition_joins` auf TRUE festlegen, kann Hyperscale (Citus) vorübergehend in den task-tracker-Executor wechseln, um den Join auszuführen.  Der Standardwert ist „FALSE“.

#### <a name="task-tracker-executor-configuration"></a>task-tracker-Executor-Konfiguration

##### <a name="citustask_tracker_delay-integer"></a>citus.task\_tracker\_delay (Integerwert)

Mit diesem Parameter wird der Ruhezustand von task-tracker zwischen Taskverwaltungsrunden festgelegt. Der Standardwert ist 200 ms. Der task-tracker-Prozess wird regelmäßig aktiviert, durchläuft alle ihm zugewiesenen Aufgaben und plant diese Aufgaben und führt sie aus.  Anschließend geht task-tracker für einen bestimmten Zeitraum in den Ruhezustand über, bevor er diese Tasks erneut durchläuft.
Dieser Konfigurationswert bestimmt die Länge dieses Ruhezeitraums. Dieser Parameter ist für die Worker gültig und muss in der Datei „postgresql.conf“ geändert werden. Nachdem Sie die Konfigurationsdatei bearbeitet haben, können Benutzer ein SIGHUP-Signal senden oder den Server neu starten, damit die Änderung wirksam wird.

Der Wert dieses Parameters kann verringert werden, um die Verzögerung zu verkürzen, die durch den task-tracker-Executor verursacht wird, indem die Zeitspanne zwischen den Verwaltungsrunden reduziert wird.
Das Verringern der Verzögerung ist in Fällen hilfreich, in denen die Shardabfragen kurz sind, sodass ihr Status regelmäßig aktualisiert wird.

##### <a name="citusmax_assign_task_batch_size-integer"></a>citus.max\_assign\_task\_batch\_size (Integerwert)

Der task-tracker-Executor für den Koordinator weist dem Daemon für die Worker synchron Tasks in Batches zu. Dieser Parameter legt die maximale Anzahl von Tasks fest, die in einem einzelnen Batch zugewiesen werden sollen. Die Auswahl einer größeren Batchgröße ermöglicht eine schnellere Taskzuweisung. Wenn die Anzahl der Worker jedoch groß ist, kann es länger dauern, bis alle Worker Tasks erhalten.  Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

##### <a name="citusmax_running_tasks_per_node-integer"></a>citus.max\_running\_tasks\_per\_node (Integerwert)

Der task-tracker-Prozess plant und führt die ihm zugewiesenen Tasks nach Bedarf aus. Mit diesem Konfigurationswert wird die maximale Anzahl der gleichzeitig auf einem Knoten auszuführenden Tasks festgelegt. Der Standardwert ist 8.

Durch den Grenzwert wird sichergestellt, dass nicht zu viele Tasks gleichzeitig auf den Datenträger zugreifen, und es werden E/A-Konflikte des Datenträgers vermieden. Wenn Ihre Abfragen aus dem Arbeitsspeicher oder von SSDs bereitgestellt werden, können Sie max\_running\_tasks\_per\_node ohne große Bedenken erhöhen.

##### <a name="cituspartition_buffer_size-integer"></a>citus.partition\_buffer\_size (Integerwert)

Legt die für Partitionsvorgänge zu verwendende Puffergröße fest. Der Standardwert beträgt 8 MB.
Hyperscale (Citus) ermöglicht die Neupartitionierung von Tabellendaten in mehrere Dateien, wenn zwei große Tabellen verknüpft werden. Nachdem dieser Partitionspuffer mit Daten aufgefüllt wurde, werden die neu partitionierten Daten in Dateien auf dem Datenträger geleert.  Dieser Konfigurationseintrag kann zur Laufzeit festgelegt werden und ist für die Worker wirksam.

#### <a name="explain-output"></a>Erläutern der Ausgabe

##### <a name="citusexplain_all_tasks-boolean"></a>citus.explain\_all\_tasks (boolescher Wert)

Standardmäßig zeigt Hyperscale (Citus) die Ausgabe eines einzelnen, beliebigen Tasks an, wenn [EXPLAIN](http://www.postgresql.org/docs/current/static/sql-explain.html) für eine verteilte Abfrage ausgeführt wird. In den meisten Fällen ist die Erläuterungsausgabe für alle Tasks ähnlich. Gelegentlich werden einige der Tasks anders geplant oder weisen eine viel höhere Ausführungszeit auf. In diesen Fällen kann es hilfreich sein, diesen Parameter zu aktivieren. Anschließend umfasst die Ausgabe von EXPLAIN alle Tasks. Wenn Sie alle Tasks erläutern, nimmt EXPLAIN möglicherweise längere Zeit in Anspruch.

## <a name="postgresql-parameters"></a>PostgreSQL-Parameter

* [DateStyle](https://www.postgresql.org/docs/current/datatype-datetime.html#DATATYPE-DATETIME-OUTPUT): Legt das Anzeigeformat für Datums- und Uhrzeitwerte fest
* [IntervalStyle](https://www.postgresql.org/docs/current/datatype-datetime.html#DATATYPE-INTERVAL-OUTPUT): Legt das Anzeigeformat für Intervallwerte fest
* [TimeZone](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-TIMEZONE): Legt die Zeitzone für die Anzeige und Interpretation von Zeitstempeln fest
* [application_name](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-APPLICATION-NAME): Legt den Anwendungsnamen fest, der in Statistiken und Protokollen angegeben werden soll
* [array_nulls](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-ARRAY-NULLS): Ermöglicht die Eingabe von NULL-Elementen in Arrays
* [autovacuum](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM): Startet den Autovacuum-Teilprozess
* [autovacuum_analyze_scale_factor](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-ANALYZE-SCALE-FACTOR): Anzahl der Tupeleinfügungen, -updates oder -löschungen vor der Analyse als Teil von „reltuples“
* [autovacuum_analyze_threshold](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-ANALYZE-THRESHOLD): Mindestanzahl von Tupeleinfügungen, -updates oder -löschungen vor der Analyse
* [autovacuum_naptime](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-NAPTIME): Standbyzeit zwischen Autovacuum-Ausführungen
* [autovacuum_vacuum_cost_delay](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-VACUUM-COST-DELAY): Vacuum-Kostenverzögerung in Millisekunden für Autovacuum
* [autovacuum_vacuum_cost_limit](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-VACUUM-COST-LIMIT): Der vor dem Standby für Autovacuum verfügbare Vacuum-Kostenbetrag
* [autovacuum_vacuum_scale_factor](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-VACUUM-SCALE-FACTOR): Anzahl der Tupelupdates oder -löschungen vor dem Ausführen von Vacuum-Prozessen als Teil von „reltuples“
* [autovacuum_vacuum_threshold](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#GUC-AUTOVACUUM-VACUUM-THRESHOLD): Mindestanzahl von Tupelupdates oder -löschungen vor dem Ausführen von Vacuum-Prozessen
* [autovacuum_work_mem](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-AUTOVACUUM-WORK-MEM): Legt den maximalen Arbeitsspeicher zur Verwendung durch die einzelnen Autovacuum-Workerprozesse fest
* [backend_flush_after](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-BACKEND-FLUSH-AFTER): Anzahl der Seiten, nach denen zuvor durchgeführte Schreibvorgänge auf den Datenträger geleert werden
* [backslash_quote](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-BACKSLASH-QUOTE): Legt fest, ob „\'“ in Zeichenfolgenliteralen zulässig ist
* [bgwriter_delay](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-BGWRITER-DELAY): Standbyzeit für das Schreiben im Hintergrund zwischen zwei Runden
* [bgwriter_flush_after](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-BGWRITER-FLUSH-AFTER): Anzahl der Seiten, nach denen zuvor durchgeführte Schreibvorgänge auf den Datenträger geleert werden
* [bgwriter_lru_maxpages](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-BGWRITER-LRU-MAXPAGES): Maximale Anzahl von Schreibvorgängen im Hintergrund bei LRU-Seiten, die pro Runde geleert werden
* [bgwriter_lru_multiplier](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-BGWRITER-LRU-MULTIPLIER): Vielfaches der Freigabe des durchschnittlich genutzten Puffers pro Runde
* [bytea_output](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-BYTEA-OUTPUT): Legt das Ausgabeformat für bytea fest
* [check_function_bodies](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-CHECK-FUNCTION-BODIES): Prüft Funktionstexte beim Ausführen von CREATE FUNCTION
* [checkpoint_completion_target](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-CHECKPOINT-COMPLETION-TARGET): Zeit, die zum Leeren von leeren modifizierten Puffern während des Prüfpunkts aufgewendet wird, als Teil des Prüfpunktintervalls
* [checkpoint_timeout](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-CHECKPOINT-TIMEOUT): Legt die maximale Zeit zwischen automatischen WAL-Prüfpunkten fest
* [checkpoint_warning](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-CHECKPOINT-WARNING): Aktiviert Warnungen, wenn Prüfpunktsegmente häufiger gefüllt werden
* [client_encoding](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-CLIENT-ENCODING): Legt die Zeichensatzcodierung des Clients fest
* [client_min_messages](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-CLIENT-MIN-MESSAGES): Legt die Meldungsebenen fest, die an den Client gesendet werden
* [commit_delay](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-COMMIT-DELAY): Legt die Verzögerung zwischen einem Transaktionscommit und dem Leeren von WAL auf den Datenträger in Mikrosekunden fest
* [commit_siblings](https://www.postgresql.org/docs/12/runtime-config-wal.html#GUC-COMMIT-SIBLINGS): Legt die Mindestanzahl der gleichzeitig geöffneten Transaktionen vor dem Durchführen von „commit_delay“ fest
* [constraint_exclusion](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CONSTRAINT-EXCLUSION): Ermöglicht dem Planer die Verwendung von Einschränkungen zur Optimierung von Abfragen
* [cpu_index_tuple_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CPU-INDEX-TUPLE-COST): Legt die Kostenschätzung des Planers für die Verarbeitung der einzelnen Indexeinträge bei einem Indexscan fest
* [cpu_operator_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CPU-OPERATOR-COST): Legt die Kostenschätzung des Planers für die Verarbeitung der einzelnen Operatoren oder Funktionsaufrufe fest
* [cpu_tuple_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CPU-TUPLE-COST): Legt die Kostenschätzung des Planers für die Verarbeitung der einzelnen Tupel (Zeilen) fest
* [cursor_tuple_fraction](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CURSOR-TUPLE-FRACTION): Legt die Einschätzung des Planers fest, welcher Teil der Zeilen eines Cursors abgerufen wird
* [deadlock_timeout](https://www.postgresql.org/docs/current/runtime-config-locks.html#GUC-DEADLOCK-TIMEOUT): Legt die Zeitspanne, die bei einer Sperre vor der Prüfung auf einen Deadlock gewartet werden soll, in Millisekunden fest
* [debug_pretty_print](https://www.postgresql.org/docs/current/runtime-config-logging.html#id-1.6.6.11.5.2.3.1.3): Fügt einen Einzug für Analyse- und Planstrukturanzeigen ein
* [debug_print_parse](https://www.postgresql.org/docs/current/runtime-config-logging.html#id-1.6.6.11.5.2.2.1.3): Protokolliert die Analysestruktur der einzelnen Abfragen
* [debug_print_plan](https://www.postgresql.org/docs/current/runtime-config-logging.html#id-1.6.6.11.5.2.2.1.3): Protokolliert den Ausführungsplan der einzelnen Abfragen
* [debug_print_rewritten](https://www.postgresql.org/docs/current/runtime-config-logging.html#id-1.6.6.11.5.2.2.1.3): Protokolliert die umgeschriebene Analysestruktur der einzelnen Abfragen
* [default_statistics_target](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-DEFAULT-STATISTICS-TARGET): Legt das Standardstatistikziel fest
* [default_tablespace](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-DEFAULT-TABLESPACE): Legt den Standardtabellenbereich zum Erstellen von Tabellen und Indizes fest
* [default_text_search_config](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-DEFAULT-TEXT-SEARCH-CONFIG): Legt die Konfiguration für die Standardtextsuche fest
* [default_transaction_deferrable](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-DEFAULT-TRANSACTION-DEFERRABLE): Legt den Standardstatus für die Verzögerbarkeit von neuen Transaktionen fest
* [default_transaction_isolation](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-DEFAULT-TRANSACTION-ISOLATION): Legt die Transaktionsisolationsstufe für jede neue Transaktion fest
* [default_transaction_read_only](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-DEFAULT-TRANSACTION-READ-ONLY): Legt den Standardstatus für den Schreibschutz neuer Transaktionen fest
* default_with_oids: Erstellt neue Tabellen standardmäßig mit OIDs
* [effective_cache_size](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-EFFECTIVE-CACHE-SIZE): Legt die Annahme des Planers in Bezug auf die Größe des Datenträgercaches fest
* [enable_bitmapscan](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-BITMAPSCAN): Ermöglicht dem Planer die Verwendung von Bitmapscanplänen
* [enable_gathermerge](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-GATHERMERGE): Ermöglicht dem Planer die Verwendung von Plänen zum Erfassen und Zusammenführen
* [enable_hashagg](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-HASHAGG): Ermöglicht dem Planer die Verwendung von Plänen für Hashaggregationen
* [enable_hashjoin](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-HASHJOIN): Ermöglicht dem Planer die Verwendung von Hashjoinplänen
* [enable_indexonlyscan](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-INDEXONLYSCAN): Ermöglicht dem Planer die Verwendung von Plänen zum ausschließlichen Scannen von Indizes
* [enable_indexscan](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-INDEXSCAN): Ermöglicht dem Planer die Verwendung von Plänen zum Scannen von Indizes
* [enable_material](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-MATERIAL): Ermöglicht dem Planer die Verwendung der Materialisierung
* [enable_mergejoin](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-MERGEJOIN): Ermöglicht dem Planer die Verwendung von Merge-Join-Plänen
* [enable_nestloop](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-NESTLOOP): Ermöglicht dem Planer die Verwendung von Nested-Loop-Join-Plänen
* [enable_seqscan](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-SEQSCAN): Ermöglicht dem Planer die Verwendung von Plänen für sequenzielle Scans
* [enable_sort](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-SORT): Ermöglicht dem Planer die Verwendung von expliziten Schritten zum Sortieren
* [enable_tidscan](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-ENABLE-TIDSCAN): Ermöglicht dem Planer die Verwendung von Plänen zum Scannen der Steuernummer
* [escape_string_warning](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-ESCAPE-STRING-WARNING): Warnt vor Schrägstrich als Escapezeichen in normalen Zeichenfolgenliteralen
* [exit_on_error](https://www.postgresql.org/docs/current/runtime-config-error-handling.html#GUC-EXIT-ON-ERROR): Beendet eine Sitzung bei einem Fehler
* [extra_float_digits](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-EXTRA-FLOAT-DIGITS):Legt die Anzahl der Stellen fest, die für Gleitkommawerte angezeigt werden
* [force_parallel_mode](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-FORCE-PARALLEL-MODE): Erzwingt die Verwendung von Funktionen zur parallelen Abfrage
* [from_collapse_limit](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-FROM-COLLAPSE-LIMIT): Legt die Größe der FROM-Liste fest, ab der Unterabfragen nicht mehr reduziert werden
* [geqo](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO): Ermöglicht die Optimierung genetischer Abfragen
* [geqo_effort](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO-EFFORT): GEQO: Aufwand zum Festlegen der Standardeinstellung für andere GEQO-Parameter
* [geqo_generations](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO-GENERATIONS): GEQO: Anzahl der Iterationen des Algorithmus
* [geqo_pool_size](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO-POOL-SIZE): GEQO: Anzahl der Individuen in einer Population
* [geqo_seed](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO-SEED): GEQO: Seed für die zufällige Auswahl von Pfaden
* [geqo_selection_bias](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO-SELECTION-BIAS): GEQO: selektiver Druck innerhalb der Population
* [geqo_threshold](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-GEQO-THRESHOLD): Legt den Schwellenwert von FROM-Elementen fest, ab dem GEQO verwendet wird
* [gin_fuzzy_search_limit](https://www.postgresql.org/docs/current/runtime-config-client.html#id-1.6.6.14.5.2.2.1.3): Legt das maximal zulässige Ergebnis für die exakte Suche durch GIN fest
* [gin_pending_list_limit](https://www.postgresql.org/docs/current/runtime-config-client.html#id-1.6.6.14.2.2.23.1.3): Legt die maximale Größe der ausstehenden Liste für den GIN-Index fest
* [idle_in_transaction_session_timeout](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-IDLE-IN-TRANSACTION-SESSION-TIMEOUT): Legt die maximal zulässige Dauer für eine Transaktion im Leerlauf fest
* [join_collapse_limit](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-JOIN-COLLAPSE-LIMIT): Legt die Größe der FROM-Liste fest, ab der JOIN-Konstrukte nicht mehr vereinfacht werden
* [lc_monetary](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-LC-MONETARY): Legt das Gebietsschema für die Formatierung von Geldbeträgen fest
* [lc_numeric](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-LC-NUMERIC): Legt das Gebietsschema für die Formatierung von Zahlen fest
* [lo_compat_privileges](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-LO-COMPAT-PRIVILEGES): Ermöglicht den Abwärtskompatibilitätsmodus für Berechtigungsprüfungen bei großen Objekten
* [lock_timeout](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-LOCK-TIMEOUT): Legt die maximal zulässige Dauer für das Warten auf eine Sperre (in Millisekunden) fest. Mit 0 wird diese Option deaktiviert.
* [log_autovacuum_min_duration](https://www.postgresql.org/docs/current/runtime-config-autovacuum.html#): Legt die mindestens erforderliche Ausführungszeit fest, nach der Autovacuum-Aktionen protokolliert werden
* [log_checkpoints](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-CHECKPOINTS): Protokolliert alle Prüfpunkte
* [log_connections](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-CONNECTIONS): Protokolliert alle erfolgreichen Verbindungen
* [log_destination](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-DESTINATION): Legt das Ziel für die Ausgabe des Serverprotokolls fest
* [log_disconnections](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-DISCONNECTIONS): Protokolliert das Ende einer Sitzung und die Dauer
* [log_duration](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-DURATION): Protokolliert die Dauer aller ausgeführten SQL-Anweisungen
* [log_error_verbosity](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-ERROR-VERBOSITY): Legt die Ausführlichkeit von protokollierten Meldungen fest
* [log_lock_waits](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-LOCK-WAITS): Protokolliert lange Wartezeiten für Sperren
* [log_min_duration_statement](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-MIN-DURATION-STATEMENT): Legt die mindestens erforderliche Ausführungszeit (in Millisekunden) fest, ab der Anweisungen protokolliert werden. Mit -1 wird die Dauer von Protokollierungsanweisungen deaktiviert
* [log_min_error_statement](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-MIN-ERROR-STATEMENT): Bewirkt, dass alle Anweisungen, die auf oder ab dieser Ebene einen Fehler verursachen, protokolliert werden
* [log_min_messages](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-MIN-MESSAGES): Legt die Meldungsebenen fest, die protokolliert werden
* [log_replication_commands](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-REPLICATION-COMMANDS): Protokolliert alle Replikationsbefehle
* [log_statement](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-STATEMENT): Legt den Typ der protokollierten Anweisungen fest
* [log_statement_stats](https://www.postgresql.org/docs/current/runtime-config-statistics.html#id-1.6.6.12.3.2.1.1.3): Schreibt für alle Abfragen eine kumulative Leistungsstatistik in das Serverprotokoll
* [log_temp_files](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-TEMP-FILES): Protokolliert die Verwendung von temporären Dateien, die größer als diese Zahl in Kilobyte sind
* [maintenance_work_mem](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAINTENANCE-WORK-MEM): Legt den maximalen Arbeitsspeicher fest, der für Wartungsvorgänge verwendet werden soll
* [max_parallel_workers](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PARALLEL-WORKERS): Legt die maximale Anzahl von parallelen Worker fest, die gleichzeitig aktiv sein können
* [max_parallel_workers_per_gather](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PARALLEL-WORKERS-PER-GATHER): Legt die maximale Anzahl von parallelen Prozessen pro Executorknoten fest
* [max_pred_locks_per_page](https://www.postgresql.org/docs/current/runtime-config-locks.html#GUC-MAX-PRED-LOCKS-PER-PAGE): Legt die maximale Anzahl von Tupeln mit gesperrtem Prädikat pro Seite fest
* [max_pred_locks_per_relation](https://www.postgresql.org/docs/current/runtime-config-locks.html#GUC-MAX-PRED-LOCKS-PER-RELATION): Legt die maximale Anzahl von Seiten und Tupeln mit gesperrtem Prädikat pro Beziehung fest
* [max_standby_archive_delay](https://www.postgresql.org/docs/current/runtime-config-replication.html#GUC-MAX-STANDBY-ARCHIVE-DELAY): Legt die maximale Verzögerung vor dem Abbruch von Abfragen fest, wenn ein unmittelbar betriebsbereiter Standbyserver archivierte WAL-Daten verarbeitet
* [max_standby_streaming_delay](https://www.postgresql.org/docs/current/runtime-config-replication.html#GUC-MAX-STANDBY-STREAMING-DELAY): Legt die maximale Verzögerung vor dem Abbruch von Abfragen fest, wenn ein unmittelbar betriebsbereiter Standbyserver gestreamte WAL-Daten verarbeitet
* [max_sync_workers_per_subscription](https://www.postgresql.org/docs/current/runtime-config-replication.html#GUC-MAX-SYNC-WORKERS-PER-SUBSCRIPTION): Maximale Anzahl von Tabellensynchronisierungsworkern pro Abonnement
* [max_wal_size](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-MAX-WAL-SIZE): Legt die WAL-Größe fest, bei der ein Prüfpunkt ausgelöst wird
* [min_parallel_index_scan_size](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-MIN-PARALLEL-INDEX-SCAN-SIZE): Legt die mindestens erforderliche Menge an Indexdaten für einen parallelen Scan fest
* [min_wal_size](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-MIN-WAL-SIZE): Legt die mindestens erforderliche Größe fest, auf die das WAL verkleinert werden kann
* [operator_precedence_warning](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-OPERATOR-PRECEDENCE-WARNING): Gibt eine Warnung für Konstrukte aus, deren Bedeutung sich seit PostgreSQL 9.4 geändert hat
* [parallel_setup_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-PARALLEL-SETUP-COST): Legt die Kostenschätzung des Planers für das Starten von Workerprozessen für parallele Abfragen fest
* [parallel_tuple_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-PARALLEL-TUPLE-COST): Legt die Kostenschätzung des Planers für die Übergabe der einzelnen Tupeln (Zeilen) vom Worker an das Master-Back-End fest
* [pg_stat_statements.save](https://www.postgresql.org/docs/current/pgstatstatements.html#id-1.11.7.38.8): Speichert „pg_stat_statements“-Statistiken beim Herunterfahren von Servern
* [pg_stat_statements.track](https://www.postgresql.org/docs/current/pgstatstatements.html#id-1.11.7.38.8): Wählt aus, welche Anweisungen durch „pg_stat_statements“ verfolgt werden
* [pg_stat_statements.track_utility](https://www.postgresql.org/docs/current/pgstatstatements.html#id-1.11.7.38.8): Wählt aus, ob Hilfsprogrammbefehle durch „pg_stat_statements“ verfolgt werden
* [quote_all_identifiers](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-QUOTE-ALL-IDENTIFIERS): Gibt beim Erzeugen von SQL-Fragmenten alle Bezeichner an
* [random_page_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-RANDOM-PAGE-COST): Legt die Kostenschätzung des Planers für eine nicht sequenziell abgerufene Datenträgerseite fest
* [row_security](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-ROW-SECURITY): Aktiviert die Zeilensicherheit
* [search_path](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-SEARCH-PATH): Legt die Schemasuchreihenfolge für Namen fest, die nicht schemaqualifiziert sind
* [seq_page_cost](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-SEQ-PAGE-COST): Legt die Kostenschätzung des Planers für eine sequenziell abgerufene Datenträgerseite fest
* [session_replication_role](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-SESSION-REPLICATION-ROLE): Legt das Verhalten der Sitzung bei Triggern und Umschreibregeln fest
* [standard_conforming_strings](https://www.postgresql.org/docs/current/runtime-config-compatible.html#id-1.6.6.16.2.2.7.1.3): Bewirkt, dass „...“-Zeichenfolgen umgekehrte Schrägstriche wörtlich behandeln
* [statement_timeout](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-STATEMENT-TIMEOUT): Legt die maximal zulässige Dauer einer Anweisung (in Millisekunden) fest. Mit 0 wird diese Option deaktiviert.
* [synchronize_seqscans](https://www.postgresql.org/docs/current/runtime-config-compatible.html#id-1.6.6.16.2.2.8.1.3): Ermöglicht synchronisierte sequenzielle Scans
* [synchronous_commit](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-SYNCHRONOUS-COMMIT): Legt die Synchronisierungsstufe der aktuellen Transaktion fest
* [tcp_keepalives_count](https://www.postgresql.org/docs/current/runtime-config-connection.html#GUC-TCP-KEEPALIVES-COUNT): Maximale Anzahl von TCP-Keep-Alive-Neuübertragungen
* [tcp_keepalives_idle](https://www.postgresql.org/docs/current/runtime-config-connection.html#GUC-TCP-KEEPALIVES-IDLE): Zeit zwischen der Ausgabe von TCP-Keep-Alives
* [tcp_keepalives_interval](https://www.postgresql.org/docs/current/runtime-config-connection.html#GUC-TCP-KEEPALIVES-INTERVAL): Zeit zwischen TCP-Keep-Alive-Neuübertragungen
* [temp_buffers](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-TEMP-BUFFERS):Legt die maximale Anzahl von temporären Puffern fest, die von den einzelnen Datenbanksitzungen verwendet werden
* [temp_tablespaces](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-TEMP-TABLESPACES): Legt die Tabellenbereiche zur Verwendung für temporäre Tabellen und Sortierungsdateien fest
* [track_activities](https://www.postgresql.org/docs/current/runtime-config-statistics.html#GUC-TRACK-ACTIVITIES): Sammelt Informationen über die Ausführung von Befehlen
* [track_counts](https://www.postgresql.org/docs/current/runtime-config-statistics.html#GUC-TRACK-COUNTS): Sammelt Statistiken über Datenbankaktivitäten
* [track_functions](https://www.postgresql.org/docs/current/runtime-config-statistics.html#GUC-TRACK-FUNCTIONS): Sammelt Statistiken über Datenbankaktivitäten auf Funktionsebene
* [track_io_timing](https://www.postgresql.org/docs/current/runtime-config-statistics.html#GUC-TRACK-IO-TIMING): Sammelt Zeitstatistiken für Datenbank-E/A-Aktivitäten
* [transform_null_equals](https://www.postgresql.org/docs/current/runtime-config-compatible.html#GUC-TRANSFORM-NULL-EQUALS): Behandelt „expr=NULL“ wie „expr IS NULL“
* [vacuum_cost_delay](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-VACUUM-COST-DELAY): Vacuum-Kostenverzögerung in Millisekunden
* [vacuum_cost_limit](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-VACUUM-COST-LIMIT): Der vor dem Standby verfügbare Vacuum-Kostenbetrag
* [vacuum_cost_page_dirty](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-VACUUM-COST-PAGE-DIRTY): Vacuum-Kosten für eine durch Vacuum geänderte Seite
* [vacuum_cost_page_hit](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-VACUUM-COST-PAGE-HIT): Vacuum-Kosten für eine im Puffercache gefundene Seite
* [vacuum_cost_page_miss](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-VACUUM-COST-PAGE-MISS): Vacuum-Kosten für eine nicht im Puffercache gefundene Seite
* [vacuum_defer_cleanup_age](https://www.postgresql.org/docs/current/runtime-config-replication.html#GUC-VACUUM-DEFER-CLEANUP-AGE): Anzahl der Transaktionen, nach der eine VACUUM- und HOT-Bereinigung ggf. verzögert werden sollte
* [vacuum_freeze_min_age](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-VACUUM-FREEZE-MIN-AGE): Mindestens erforderliches Alter, ab dem VACUUM eine Tabellenzeile einfrieren sollte
* [vacuum_freeze_table_age](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-VACUUM-FREEZE-TABLE-AGE): Alter, ab dem VACUUM die gesamte Tabelle prüfen sollte, um Tupel einzufrieren
* [vacuum_multixact_freeze_min_age](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-VACUUM-MULTIXACT-FREEZE-MIN-AGE): Mindestens erforderliches Alter, ab dem VACUUM eine MultiXactId in einer Tabellenzeile einfrieren sollte
* [vacuum_multixact_freeze_table_age](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-VACUUM-MULTIXACT-FREEZE-TABLE-AGE): Multixact-Alter, ab dem VACUUM die gesamte Tabelle prüfen sollte, um Tupel einzufrieren
* [wal_receiver_status_interval](https://www.postgresql.org/docs/current/runtime-config-replication.html#GUC-WAL-RECEIVER-STATUS-INTERVAL): Legt das maximale Intervall zwischen WAL-Empfängerstatusberichten auf „primär“ fest
* [wal_writer_delay](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-WAL-WRITER-DELAY): Zeit zwischen im WAL-Schreiber durchgeführten WAL-Leerungen
* [wal_writer_flush_after](https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-WAL-WRITER-FLUSH-AFTER): Anzahl der vom WAL-Schreiber geschriebenen WAL, bei der eine Leerung ausgelöst wird
* [work_mem](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-WORK-MEM): Legt den Arbeitsspeicher fest, der von internen Sortiervorgängen und Hashtabellen vor dem Schreiben in temporäre Datenträgerdateien verwendet werden soll
* [xmlbinary](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-XMLBINARY): Legt fest, wie Binärwerte in XML codiert werden sollen
* [xmloption](https://www.postgresql.org/docs/current/runtime-config-client.html#GUC-XMLOPTION): Legt fest, ob XML-Daten bei impliziten Analyse- und Serialisierungsvorgängen als Dokumente oder Inhaltsfragmente betrachtet werden sollen

## <a name="next-steps"></a>Nächste Schritte

* Eine weitere Form der Konfiguration ist neben den Serverparametern die Ressource [Konfigurationsoptionen](concepts-hyperscale-configuration-options.md) in einer Hyperscale (Citus)-Servergruppe.
* Die zugrunde liegende PostgreSQL-Datenbank verfügt auch über [Konfigurationsparameter](http://www.postgresql.org/docs/current/static/runtime-config.html).
