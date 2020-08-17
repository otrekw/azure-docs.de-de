---
title: Serverparameter – Hyperscale (Hyperscale (Citus)) – Azure Database for PostgreSQL
description: Parameter in der SQL-API für Hyperscale (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: reference
ms.date: 08/10/2020
ms.openlocfilehash: 57258540f3cd7b4c47b662b0885453cedd188e5b
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136619"
---
# <a name="server-parameters"></a>Serverparameter

Es gibt verschiedene Serverparameter, die sich auf das Verhalten von Hyperscale (Citus) auswirken, und zwar sowohl Parameter aus Standard-PostgreSQL als auch für Hyperscale (Citus) spezifische Parameter. Weitere Informationen zu PostgreSQL-Konfigurationsparametern finden Sie im Abschnitt [Laufzeitkonfiguration](http://www.postgresql.org/docs/current/static/runtime-config.html) in der PostgreSQL-Dokumentation.

Im Rest dieser Referenz werden für Hyperscale (Citus) spezifische Konfigurationsparameter beschrieben. Diese Parameter können im Azure-Portal unter **Workerknotenparameter** unter **Einstellungen** für eine Hyperscale (Citus)-Servergruppe festgelegt werden.

> [!NOTE]
>
> Hyperscale-Servergruppen, auf denen ältere Versionen der Citus-Engine ausgeführt werden, bieten möglicherweise nicht alle unten aufgeführten Parameter.

## <a name="general-configuration"></a>Allgemeine Konfiguration

### <a name="citususe_secondary_nodes-enum"></a>citus.use\_secondary\_nodes (Enumeration)

Legt die Richtlinie fest, die beim Auswählen von Knoten für SELECT-Abfragen verwendet werden soll. Wenn sie auf „always“ festgelegt ist, fragt der Planer nur Knoten ab, die in [pg_dist_node](reference-hyperscale-metadata.md#worker-node-table) als „secondary“ noderole gekennzeichnet sind.

Die unterstützten Werte für diese Enumeration lauten:

-   **never:** (Standardwert) Alle Lesevorgänge erfolgen auf primären Knoten.
-   **always:** Lesevorgänge werden stattdessen für sekundäre Knoten ausgeführt, und insert-/update-Anweisungen sind deaktiviert.

### <a name="cituscluster_name-text"></a>citus.cluster\_name (Text)

Informiert den Koordinatorknotenplaner, welchen Cluster er koordiniert. Nachdem cluster\_name festgelegt wurde, fragt der Planer die Workerknoten nur in diesem Cluster ab.

### <a name="citusenable_version_checks-boolean"></a>citus.enable\_version\_checks (boolescher Wert)

Zum Aktualisieren der Version von Hyperscale (Citus) ist ein Neustart des Servers erforderlich (um die neue freigegebene Bibliothek einzubinden), gefolgt vom Befehl ALTER EXTENSION UPDATE.  Wenn nicht beide Schritte ausgeführt werden, kann dies potenziell zu Fehlern oder Abstürzen führen.
Hyperscale (Citus) überprüft daher die Version des Codes und die Übereinstimmung mit der Erweiterung und gibt Fehler aus, wenn dies nicht der Fall ist.

Dieser Wert ist standardmäßig TRUE und für den Koordinator gültig. In seltenen Fällen ist es für komplexe Upgradeprozesse möglicherweise erforderlich, diesen Parameter auf FALSE festzulegen und so die Überprüfung zu deaktivieren.

### <a name="cituslog_distributed_deadlock_detection-boolean"></a>citus.log\_distributed\_deadlock\_detection (boolescher Wert)

Gibt an, ob die Verarbeitung verteilter Deadlockerkennungen im Serverprotokoll protokolliert werden soll. Der Standardwert ist FALSE.

### <a name="citusdistributed_deadlock_detection_factor-floating-point"></a>citus.distributed\_deadlock\_detection\_factor (Gleitkommawert)

Legt die Wartezeit bis zur Suche nach verteilten Deadlocks fest. Die Wartezeit besteht aus diesem Wert, multipliziert mit der [deadlock\_timeout](https://www.postgresql.org/docs/current/static/runtime-config-locks.html)-Einstellung von PostgreSQL. Der Standardwert ist `2`. Der Wert `-1` deaktiviert die Erkennung verteilter Deadlocks.

### <a name="citusnode_connection_timeout-integer"></a>citus.node\_connection\_timeout (Integerwert)

Der `citus.node_connection_timeout`-GUC legt die maximale Dauer (in Millisekunden) für das Warten auf die Verbindungsherstellung fest. Hyperscale (Citus) löst einen Fehler aus, wenn das Timeout abläuft, bevor mindestens eine Workerverbindung hergestellt wurde. Dieser GUC wirkt sich auf Verbindungen zwischen dem Koordinator und Workern sowie Workern untereinander aus.

-   Standardwert: 5 Sekunden
-   Minimum: 10 Millisekunden
-   Maximum: eine Stunde

```postgresql
-- set to 30 seconds
ALTER DATABASE foo
SET citus.node_connection_timeout = 30000;
```

## <a name="query-statistics"></a>Abfragestatistik

### <a name="citusstat_statements_purge_interval-integer"></a>citus.stat\_statements\_purge\_interval (Integerwert)

Legt die Häufigkeit fest, mit der der Wartungdaemon Datensätze aus [citus_stat_statements](reference-hyperscale-metadata.md#query-statistics-table) entfernt, für die in `pg_stat_statements` keine Übereinstimmung vorliegt. Mit diesem Konfigurationswert wird das Zeitintervall zwischen Bereinigungen in Sekunden festgelegt. Der Standardwert ist 10. Der Wert 0 deaktiviert Bereinigungen.

```psql
SET citus.stat_statements_purge_interval TO 5;
```

Dieser Parameter ist für den Koordinator wirksam und kann zur Laufzeit geändert werden.

## <a name="data-loading"></a>Laden von Daten

### <a name="citusmulti_shard_commit_protocol-enum"></a>citus.multi\_shard\_commit\_protocol (Enumeration)

Legt das Commitprotokoll fest, das beim Ausführen von COPY für eine Tabelle mit Hashverteilung verwendet werden soll. Bei jeder einzelnen Shardplatzierung wird COPY in einem Transaktionsblock ausgeführt, um sicherzustellen, dass keine Daten erfasst werden, wenn während des Kopiervorgangs ein Fehler auftritt. Es gibt jedoch einen bestimmten Fehlerfall, bei dem COPY bei allen Platzierungsvorgängen erfolgreich ist, aber vor dem Commit aller Transaktionen tritt ein Fehler (Hardwarefehler) auf. Dieser Parameter kann verwendet werden, um in diesem Fall Datenverluste zu verhindern, indem zwischen den folgenden Commitprotokollen ausgewählt wird:

-   **2pc:** (Standardwert) Die Transaktionen, bei denen COPY für die Shardplatzierungen ausgeführt wird, werden zuerst mit einem [Zweiphasencommit](http://www.postgresql.org/docs/current/static/sql-prepare-transaction.html) von PostgreSQL vorbereitet und dann committet. Fehlgeschlagene Commits können mithilfe von COMMIT PREPARED bzw. ROLLBACK PREPARED manuell wiederhergestellt oder abgebrochen werden.
    Bei der Verwendung von 2pc sollte der Wert von [max\_prepared\_transactions](http://www.postgresql.org/docs/current/static/runtime-config-resource.html) für alle Worker erhöht werden (in der Regel auf denselben Wert wie max\_connections).
-   **1pc:** Die Transaktionen, bei denen für die Shardplatzierung COPY ausgeführt wird, werden in einer Runde committet. Daten gehen möglicherweise verloren, wenn ein Commit fehlschlägt, nachdem COPY für alle Platzierungen erfolgreich war (selten).

### <a name="citusshard_replication_factor-integer"></a>citus.shard\_replication\_factor (Integerwert)

Legt den Replikationsfaktor für Shards fest, d. h. die Anzahl der Knoten, auf denen Shards platziert werden. Der Standardwert ist 1. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig. Der ideale Wert für diesen Parameter hängt von der Größe des Clusters und der Rate der Knotenfehler ab.  Beispielsweise können Sie diesen Replikationsfaktor erhöhen, wenn Sie große Cluster ausführen und Knotenfehler häufiger beobachtet werden.

### <a name="citusshard_count-integer"></a>citus.shard\_count (Integerwert)

Legt die Anzahl der Shards für Tabellen mit Hashpartitionierung fest. Der Standardwert ist 32.  Dieser Wert wird vom [create_distributed_table](reference-hyperscale-functions.md#create_distributed_table)-UDF verwendet, wenn Tabellen mit Hashpartitionierung erstellt werden. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

### <a name="citusshard_max_size-integer"></a>citus.shard\_max\_size (Integerwert)

Legt die maximale Größe fest, bis zu der ein Shard vergrößert wird, bevor er geteilt wird. Der Standardwert ist 1 GB. Wenn die Größe der Quelldatei (die für Staging verwendet wird) für einen Shard diesen Konfigurationswert überschreitet, stellt die Datenbank sicher, dass ein neuer Shard erstellt wird. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

## <a name="planner-configuration"></a>Planerkonfiguration

### <a name="cituslimit_clause_row_fetch_count-integer"></a>citus.limit\_clause\_row\_fetch\_count (Integerwert)

Legt die Anzahl der Zeilen fest, die pro Task für die Optimierung der limit-Klausel abgerufen werden.
In einigen Fällen müssen SELECT-Abfragen mit Limit-Klauseln möglicherweise alle Zeilen aus jedem Task abrufen, um Ergebnisse zu generieren. In diesen Fällen, in denen eine Näherung zu sinnvollen Ergebnissen führen würde, legt dieser Konfigurationswert die Anzahl der Zeilen fest, die aus jedem Shard abgerufen werden sollen. Limit-Näherungen sind standardmäßig deaktiviert, und dieser Parameter ist auf -1 festgelegt. Dieser Wert kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

### <a name="cituscount_distinct_error_rate-floating-point"></a>citus.count\_distinct\_error\_rate (Gleitkommawert)

Hyperscale (Citus) kann die count(distinct)-Näherung mit der postgresql-hll-Erweiterung berechnen. Mit diesem Konfigurationseintrag wird die gewünschte Fehlerrate beim Berechnen von count(distinct) festgelegt. 0,0 ist die Standardeinstellung und deaktiviert Näherungswerte für count(distinct). 1,0 bietet keine Garantie bezüglich der Genauigkeit der Ergebnisse. Es wird empfohlen, diesen Parameter auf 0,005 festzulegen, um optimale Ergebnisse zu erzielen. Dieser Wert kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

### <a name="citustask_assignment_policy-enum"></a>citus.task\_assignment\_policy (Enumeration)

> [!NOTE]
> Dieser GUC ist nur anwendbar, wenn [shard_replication_factor](reference-hyperscale-parameters.md#citusshard_replication_factor-integer) größer als 1 ist, oder für Abfragen für [reference_tables](concepts-hyperscale-distributed-data.md#type-2-reference-tables).

Legt die Richtlinie fest, die beim Zuweisen von Tasks an Worker verwendet wird. Der Koordinator weist Workern auf der Grundlage von Shardspeicherorten Tasks zu. Dieser Konfigurationswert gibt die Richtlinie an, die bei diesen Zuweisungen verwendet werden soll.
Derzeit gibt es drei mögliche Taskzuweisungsrichtlinien, die verwendet werden können.

-   **greedy:** Die greedy-Richtlinie ist die Standardeinstellung und zielt darauf ab, Tasks gleichmäßig auf Worker zu verteilen.
-   **round-robin:** Die round-robin-Richtlinie weist Workern Tasks in einem Roundrobinverfahren zu, das ein Rotationsprinzip verwendet. Diese Richtlinie ermöglicht eine bessere Clusterauslastung, wenn die Anzahl der Shards für eine Tabelle im Vergleich zur Anzahl der Worker gering ist.
-   **first-replica:** Die first-replica-Richtlinie weist Tasks auf der Grundlage der Einfügungsreihenfolge der Platzierungen (Replikate) für die Shards zu. Anders ausgedrückt: Die Fragmentabfrage für einen Shard wird dem Worker zugewiesen, der über das erste Replikat dieses Shards verfügt.
    Mit dieser Methode können Sie sicherstellen, welche Shards auf welchen Knoten verwendet werden (d. h. stärkere Speicherresidenzgarantien bereitstellen).

Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

## <a name="intermediate-data-transfer"></a>Übertragung von Zwischendaten

### <a name="citusbinary_worker_copy_format-boolean"></a>citus.binary\_worker\_copy\_format (boolescher Wert)

Verwenden Sie das binäre Kopierformat, um Zwischendaten zwischen Workern zu übertragen.
Bei großen Tabellenjoins muss Hyperscale (Citus) möglicherweise dynamisch eine Neupartitionierung ausführen und Daten zwischen verschiedenen Workern mischen. Standardmäßig werden diese Daten im Textformat übertragen. Durch Aktivieren dieses Parameters wird die Datenbank angewiesen, das binäre Serialisierungsformat von PostgreSQL zum Übertragen dieser Daten zu verwenden. Dieser Parameter ist für die Worker gültig und muss in der Datei „postgresql.conf“ geändert werden. Nachdem Sie die Konfigurationsdatei bearbeitet haben, können Benutzer ein SIGHUP-Signal senden oder den Server neu starten, damit diese Änderung wirksam wird.

### <a name="citusbinary_master_copy_format-boolean"></a>citus.binary\_master\_copy\_format (boolescher Wert)

Verwenden Sie das binäre Kopierformat, um Daten zwischen dem Koordinator und Workern zu übertragen. Beim Ausführen verteilter Abfragen übertragen die Worker ihre Zwischenergebnisse für die endgültige Aggregation an den Koordinator. Standardmäßig werden diese Daten im Textformat übertragen. Durch Aktivieren dieses Parameters wird die Datenbank angewiesen, das binäre Serialisierungsformat von PostgreSQL zum Übertragen dieser Daten zu verwenden.
Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

### <a name="citusmax_intermediate_result_size-integer"></a>citus.max\_intermediate\_result\_size (Integerwert)

Die maximale Größe in KB von Zwischenergebnissen für CTEs, die zur Ausführung nicht auf Workerknoten gepusht werden können, und für komplexe Unterabfragen. Der Standardwert ist 1 GB. Der Wert -1 bedeutet, dass kein Grenzwert gilt.
Abfragen, die den Grenzwert überschreiten, werden abgebrochen und führen zu einer Fehlermeldung.

## <a name="ddl"></a>DDL

### <a name="citusenable_ddl_propagation-boolean"></a>citus.enable\_ddl\_propagation (boolescher Wert)

Gibt an, ob DDL-Änderungen automatisch vom Koordinator an alle Worker weitergegeben werden sollen. Der Standardwert lautet „true“. Da einige Schemaänderungen eine ausschließliche Zugriffssperre für Tabellen erfordern und die automatische Weitergabe sequenziell für alle Worker gilt, kann ein Hyperscale (Citus)-Cluster vorübergehend weniger reaktionsfähig sein. Sie können diese Einstellung deaktivieren und Änderungen manuell weitergeben.

## <a name="executor-configuration"></a>Executor-Konfiguration

### <a name="general"></a>Allgemein

#### <a name="citusall_modifications_commutative"></a>citus.all\_modifications\_commutative

Hyperscale (Citus) erzwingt Kommutativitätsregeln und ruft geeignete Sperren für Änderungsvorgänge ab, um die Richtigkeit des Verhaltens zu gewährleisten. Beispielsweise wird davon ausgegangen, dass eine INSERT-Anweisung mit einer anderen INSERT-Anweisung, aber nicht mit einer UPDATE- oder DELETE-Anweisung Kommutativität aufweist. Entsprechend wird auch davon ausgegangen, dass eine UPDATE- oder DELETE-Anweisung keine Kommutativität mit einer anderen UPDATE- oder DELETE-Anweisung verwendet. Diese Vorsichtsmaßnahme bedeutet, dass Hyperscale (Citus) für UPDATE- und DELETE-Vorgänge stärkere Sperren abrufen muss.

Wenn Sie über UPDATE-Anweisungen verfügen, die mit INSERT- oder anderen UPDATE-Vorgängen Kommutativität verwenden, können Sie diese Kommutativitätsannahmen durch Festlegen dieses Parameters auf TRUE lockern. Wenn dieser Parameter auf TRUE festgelegt ist, werden alle Befehle als kommutativ angesehen und beanspruchen eine gemeinsame Sperre, was den Gesamtdurchsatz verbessern kann. Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="citusremote_task_check_interval-integer"></a>citus.remote\_task\_check\_interval (Integerwert)

Legt die Häufigkeit fest, mit der Hyperscale (Citus) die Status von Aufträgen überprüft, die vom taskt-racker-Executor verwaltet werden. Der Standardwert lautet 10 ms. Der Koordinator weist Workern Tasks zu und überprüft sie regelmäßig hinsichtlich des Fortschritts der einzelnen Tasks. Mit diesem Konfigurationswert wird das Zeitintervall zwischen zwei aufeinanderfolgenden Überprüfungen festgelegt. Dieser Parameter ist für den Koordinator wirksam und kann zur Laufzeit festgelegt werden.

#### <a name="citustask_executor_type-enum"></a>citus.task\_executor\_type (Enumeration)

Hyperscale (Citus) verfügt über drei Executor-Typen zum Ausführen verteilter SELECT-Abfragen.  Der gewünschte Executor kann durch Festlegen dieses Konfigurationsparameters ausgewählt werden. Die folgenden Werte sind für diesen Parameter zulässig:

-   **adaptive:** Der Standardwert. Er eignet sich optimal für schnelle Antworten auf Abfragen, die Aggregationen und Joins mit Colocation umfassen, die auf mehrere Shards verteilt sind.
-   **task-tracker:** Der task-tracker-Executor eignet sich gut für zeitintensive komplexe Abfragen, bei denen Daten über mehrere Workerknoten hinweg gemischt werden müssen, sowie für effiziente Ressourcenverwaltung.
-   **real-time:** (veraltet) Dient einem ähnlichen Zweck wie der adaptive Executor, ist aber weniger flexibel und kann zu mehr Verbindungsfehlern auf Workerknoten führen.

Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="citusmulti_task_query_log_level-enum-multi_task_logging"></a>citus.multi\_task\_query\_log\_level (Enumeration) {#multi_task_logging}

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

#### <a name="citusenable_repartition_joins-boolean"></a>citus.enable\_repartition\_joins (boolescher Wert)

Normalerweise schlägt der Versuch, Neupartitionierungsjoins mit dem adaptiven Executor durchzuführen, mit einer Fehlermeldung fehl.  Wenn Sie jedoch `citus.enable_repartition_joins` auf TRUE festlegen, kann Hyperscale (Citus) vorübergehend in den task-tracker-Executor wechseln, um den Join auszuführen.  Der Standardwert ist „FALSE“.

### <a name="task-tracker-executor-configuration"></a>task-tracker-Executor-Konfiguration

#### <a name="citustask_tracker_delay-integer"></a>citus.task\_tracker\_delay (Integerwert)

Mit diesem Parameter wird der Ruhezustand von task-tracker zwischen Taskverwaltungsrunden festgelegt. Der Standardwert ist 200 ms. Der task-tracker-Prozess wird regelmäßig aktiviert, durchläuft alle ihm zugewiesenen Aufgaben und plant diese Aufgaben und führt sie aus.  Anschließend geht task-tracker für einen bestimmten Zeitraum in den Ruhezustand über, bevor er diese Tasks erneut durchläuft.
Dieser Konfigurationswert bestimmt die Länge dieses Ruhezeitraums. Dieser Parameter ist für die Worker gültig und muss in der Datei „postgresql.conf“ geändert werden. Nachdem Sie die Konfigurationsdatei bearbeitet haben, können Benutzer ein SIGHUP-Signal senden oder den Server neu starten, damit die Änderung wirksam wird.

Der Wert dieses Parameters kann verringert werden, um die Verzögerung zu verkürzen, die durch den task-tracker-Executor verursacht wird, indem die Zeitspanne zwischen den Verwaltungsrunden reduziert wird.
Das Verringern der Verzögerung ist in Fällen hilfreich, in denen die Shardabfragen kurz sind, sodass ihr Status regelmäßig aktualisiert wird.

#### <a name="citusmax_assign_task_batch_size-integer"></a>citus.max\_assign\_task\_batch\_size (Integerwert)

Der task-tracker-Executor für den Koordinator weist dem Daemon für die Worker synchron Tasks in Batches zu. Dieser Parameter legt die maximale Anzahl von Tasks fest, die in einem einzelnen Batch zugewiesen werden sollen. Die Auswahl einer größeren Batchgröße ermöglicht eine schnellere Taskzuweisung. Wenn die Anzahl der Worker jedoch groß ist, kann es länger dauern, bis alle Worker Tasks erhalten.  Dieser Parameter kann zur Laufzeit festgelegt werden und ist für den Koordinator gültig.

#### <a name="citusmax_running_tasks_per_node-integer"></a>citus.max\_running\_tasks\_per\_node (Integerwert)

Der task-tracker-Prozess plant und führt die ihm zugewiesenen Tasks nach Bedarf aus. Mit diesem Konfigurationswert wird die maximale Anzahl der gleichzeitig auf einem Knoten auszuführenden Tasks festgelegt. Der Standardwert ist 8.

Durch den Grenzwert wird sichergestellt, dass nicht zu viele Tasks gleichzeitig auf den Datenträger zugreifen, und es werden E/A-Konflikte des Datenträgers vermieden. Wenn Ihre Abfragen aus dem Arbeitsspeicher oder von SSDs bereitgestellt werden, können Sie max\_running\_tasks\_per\_node ohne große Bedenken erhöhen.

#### <a name="cituspartition_buffer_size-integer"></a>citus.partition\_buffer\_size (Integerwert)

Legt die für Partitionsvorgänge zu verwendende Puffergröße fest. Der Standardwert beträgt 8 MB.
Hyperscale (Citus) ermöglicht die Neupartitionierung von Tabellendaten in mehrere Dateien, wenn zwei große Tabellen verknüpft werden. Nachdem dieser Partitionspuffer mit Daten aufgefüllt wurde, werden die neu partitionierten Daten in Dateien auf dem Datenträger geleert.  Dieser Konfigurationseintrag kann zur Laufzeit festgelegt werden und ist für die Worker wirksam.

### <a name="explain-output"></a>Erläutern der Ausgabe

#### <a name="citusexplain_all_tasks-boolean"></a>citus.explain\_all\_tasks (boolescher Wert)

Standardmäßig zeigt Hyperscale (Citus) die Ausgabe eines einzelnen, beliebigen Tasks an, wenn [EXPLAIN](http://www.postgresql.org/docs/current/static/sql-explain.html) für eine verteilte Abfrage ausgeführt wird. In den meisten Fällen ist die Erläuterungsausgabe für alle Tasks ähnlich. Gelegentlich werden einige der Tasks anders geplant oder weisen eine viel höhere Ausführungszeit auf. In diesen Fällen kann es hilfreich sein, diesen Parameter zu aktivieren. Anschließend umfasst die Ausgabe von EXPLAIN alle Tasks. Wenn Sie alle Tasks erläutern, nimmt EXPLAIN möglicherweise längere Zeit in Anspruch.

## <a name="next-steps"></a>Nächste Schritte

* Eine weitere Form der Konfiguration ist neben den Serverparametern die Ressource [Konfigurationsoptionen](concepts-hyperscale-configuration-options.md) in einer Hyperscale (Citus)-Servergruppe.
* Die zugrunde liegende PostgreSQL-Datenbank verfügt auch über [Konfigurationsparameter](http://www.postgresql.org/docs/current/static/runtime-config.html).
