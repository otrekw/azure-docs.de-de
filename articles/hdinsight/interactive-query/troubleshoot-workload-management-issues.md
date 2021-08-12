---
title: Problembehandlung bei den Hive LLAP-Workload-Managementproblemen
titleSuffix: Azure HDInsight
description: Problembehandlung bei den Hive LLAP-Workload-Managementproblemen
ms.service: hdinsight
ms.topic: troubleshooting
author: guptanikhil007
ms.author: guptan
ms.reviewer: jasonh
ms.date: 05/25/2021
ms.openlocfilehash: 6a06a67b5039bc3a7e25e8300128c85ee008be3a
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482534"
---
# <a name="troubleshoot-hive-llap-workload-management-issues"></a>Problembehandlung bei den Hive LLAP-Workload-Managementproblemen

Das Workload-Management (WLM) ist für die Kunden verfügbar, die HDInsight 4.0-Cluster starten. Verwenden Sie die unten angegebenen Ressourcen, um Probleme im Zusammenhang mit der WLM-Funktion zu debuggen.

## <a name="get-wlm-resource-plan-and-plan-entities"></a>Das Abrufen von einem WLM-Ressourcenplan und von WLM-Planentitäten
#### <a name="to-get-all-resource-plans-on-the-cluster"></a>So rufen Sie alle Ressourcenpläne im Cluster ab:
```hql
SHOW RESOURCE PLANS;
```

#### <a name="to-get-definition-of-a-given-resource-plan"></a>So rufen Sie die Definition eines bestimmten Ressourcenplans ab
```hql
SHOW RESOURCE PLAN <plan_name>;
```

## <a name="get-wlm-entities-information-from-metastore-database"></a>Das Abrufen von WLM-Entitätsinformationen aus der Metastore-Datenbank
> [!NOTE]
> Das gilt nur für eine benutzerdefinierte Hive-Metastore-Datenbank

Die WLM-Entitätsinformationen können auch in den folgenden Tabellen in der Hive-Metastore-Datenbank angezeigt werden 

* **WM_RESOURCEPLÄNE** (NAME-Zeichenfolge, STATUS-Zeichenfolge, QUERY_PARALLELISM-Integer, DEFAULT_POOL_PATH-Zeichenfolge)
* **WM_POOLS** (RP_NAME-Zeichenfolge, PATH-Zeichenfolge, ALLOC_FRACTION-Doppelt, QUERY_PARALLELISM-Integer, SCHEDULING_POLICY-Zeichenfolge)
* **WM_ZUORDNUNGEN** (RP_NAME-Zeichenfolge, ENTITY_TYPE-Zeichenfolge, ENTITY_NAME-Zeichenfolge, POOL_PATH-Zeichenfolge, ORDERING-Integer)
* **WM_AUSLÖSER** (RP_NAME-Zeichenfolge, NAME-Zeichenfolge, TRIGGER_EXPRESSION-Zeichenfolge, ACTION_EXPRESSION-Zeichenfolge)
* **WM_POOLS_ZU_AUSLÖSERN** (RP_NAME-Zeichenfolge, POOL_PATH-Zeichenfolge, TRIGGER_NAME-Zeichenfolge)

## <a name="wlm-metrics"></a>WLM-Metriken

Auf die WLM-Metriken kann direkt über die HS2-Interaktive Benutzeroberfläche auf der Registerkarte „Metrik-Sicherungsdatei“ zugegriffen werden. <br>
:::image type="content" source="./media/hive-workload-management/hs2-interactive-wlm.jpg" alt-text="HS2 Interactive UI." lightbox="./media/hive-workload-management/hs2-interactive-wlm.jpg":::

Die Beispielmetriken, die von dem WLM für einen bestimmten Pool in einem Ressourcenplan veröffentlicht wurden.
```
    "name" : "Hadoop:service=hiveserver2,name=WmPoolMetrics.etl",
    "modelerType" : "WmPoolMetrics.etl",
    "tag.Context" : "HS2",
    "tag.SessionId" : "etl",
    "tag.Hostname" : "hn0-c2b-ll.cu1cgjaim53urggr4psrgczloa.cx.internal.cloudapp.net",
    "NumExecutors" : 10,
    "NumRunningQueries" : 2,
    "NumParallelQueries" : 3,
    "NumQueuedQueries" : 0,
    "NumExecutorsMax" : 10
```

HS2 Interactive UI funktioniert möglicherweise nicht für ESP (Enterprise-Sicherheitspaket)-fähige Cluster, die vor April 2021 veröffentlicht wurden. In solchen Fällen können die WLM-bezogene Metriken von den benutzerdefinierten Grafana-Dashboards bezogen werden.
<br>
Der Metrikname folgt den nachfolgenden Mustern:
```
default.General.WM_<pool>_numExecutors
default.General.WM_<pool>_numExecutorsMax
default.General.WM_<pool>_numRunningQueries
default.General.WM_<pool>_numParallelQueries
default.General.WM_<pool>_numQueuedQueries
```
Ersetzen Sie `<pool>` durch den entsprechenden Poolnamen, um die Metriken in Grafana zu abzurufen.

Die :::image type="content" source="./media/hive-workload-management/grafana-wlm.jpg" alt-text="Grafana WLM-Metriken." lightbox="./media/hive-workload-management/grafana-wlm.jpg":::

> Hinweis: Stellen Sie sicher, dass in den obigen Filtern und dem Komponentennamen die Komponente HiveServer2 ausgewählt ist.

<br>

## <a name="wlm-feature-characteristics"></a>Die Merkmale von den WLM-Funktionseigenschaften
### <a name="lifecycle-of-tez-ams-in-wlm-enabled-clusters"></a>Der **Lebenszyklus von Tez-AMs in den WLM-fähigen Clustern**
Im Gegensatz zu den LLAP-Standardclustern verfügen die WLM-fähigen Cluster über einen weiteren Satz von Tez-AMs. Die Ausführung dieser Tez-AMs erfolgt planmäßig in der `wm`-Warteschlange, wenn *hive.server2.tez.interactive.queue=wm* in den Hive-Konfigurationen festgelegt ist. <br>
Diese Tez-AMs entstehen, wenn das WLM, basierend auf der Summe von dem QUERY_PARALLELISM aller im Ressourcenplan definierten Pools, aktiviert ist. <br>
Wenn wir das Workload-Management im Cluster deaktivieren, werden diese Tez-AMs automatisch beendet.
`{ DISABLE WORKLOAD MANAGEMENT; }`

### <a name="resource-contention"></a>**Ressourcenkonflikte**
In einem WLM-fähigen LLAP-Cluster werden die Ressourcen basierend auf der Ressourcenplankonfiguration von den Abfragen gemeinsam genutzt. Die gemeinsame Nutzung der Ressourcen führt manchmal zu einer langsamen Abfrage.
Es können einige Optimierungen am Ressourcenplan vorgenommen werden, um den Ressourcenkonflikt, innerhalb eines Pools zu reduzieren. Beispielsweise kann `scheduling_policy` als `fair` definiert werden, was für jede Abfrage, die dem Pool zugewiesen ist, einen gleichen Anteil an Ressourcen im Cluster garantiert. Alternative kann es als `fifo` definiert werden, wodurch alle Ressourcen für die erste Abfrage garantiert werden, die zum Pool gehört.<br>
Das folgende Beispiel zeigt, wie Sie die Planungsrichtlinie für einen Pool mit dem Namen `etl` im Ressourcenplan `wlm_basic` festlegen:
```hql
ALTER POOL wlm_basic.etl SET SCHEDULING_POLICY = fair;
```
Die Planungsrichtlinie kann auch beim Erstellen des Pools festgelegt werden:
```hql
CREATE POOL wlm_basic.default WITH ALLOC_FRACTION = 0.5, QUERY_PARALLELISM = 2, SCHEDULING_POLICY = fifo;
```

### <a name="query-failures-for-some-specific-use-cases"></a>Die **Abfragefehler für bestimmte Anwendungsfälle**
Das Ausführen von Abfragen im WLM kann in folgenden Fällen automatisch beendet werden:
1. Wenn die Option „Auslöser verschieben“ auf eine Abfrage und einen Zielpool angewendet wird, für die keine Tez-AMs verfügbar sind, wird die Abfrage stattdessen beendet. <br>
Die obige Funktion ist eine Entwurfseinschränkung des WLM. Sie können diese Funktion umgehen, indem Sie die Eigenschaft `QUERY_PARALLELISM` für den Zielpool erhöhen. So können selbst die an den Cluster gesendeten Abfragen für das Szenario mit maximaler Auslastung von diesem Pool unterstützt werden. Optimieren Sie außerdem die `wm`-Warteschlangengröße, um diese Änderung zu berücksichtigen. <br>
2. Wenn das WLM deaktiviert ist, werden alle In-Flight-Abfragen mit dem folgenden Ausnahmemuster nicht ausgeführt:
   ```
   FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.tez.TezTask. Dag received [DAG_TERMINATE, DAG_KILL] in RUNNING state.
   ```
3. Wenn ein WLM-Tez-AM manuell beendet wird, sorgt das möglicherweise bei einigen der Abfragen für Fehler mit dem folgendem Muster. <br/>Diese Abfragen sollten ohne Probleme bei einer erneuten Übermittlung ausgeführt werden.
```
java.util.concurrent.CancellationException: Task was cancelled.
    at com.google.common.util.concurrent.AbstractFuture.cancellationExceptionWithCause(AbstractFuture.java:1349) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture.getDoneValue(AbstractFuture.java:550) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture.get(AbstractFuture.java:513) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture$TrustedFuture.get(AbstractFuture.java:90) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.Uninterruptibles.getUninterruptibly(Uninterruptibles.java:237) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.Futures.getDone(Futures.java:1064) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.Futures$CallbackListener.run(Futures.java:1013) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.DirectExecutor.execute(DirectExecutor.java:30) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture.executeListener(AbstractFuture.java:1137) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture.complete(AbstractFuture.java:957) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture.cancel(AbstractFuture.java:611) ~[guava-28.0-jre.jar:?]
    at com.google.common.util.concurrent.AbstractFuture$TrustedFuture.cancel(AbstractFuture.java:118) ~[guava-28.0-jre.jar:?]
    at org.apache.hadoop.hive.ql.exec.tez.WmTezSession$TimeoutRunnable.run(WmTezSession.java:264) ~[hive-exec-3.1.3.4.1.3.6.jar:3.1.3.4.1.3.6]
    at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511) [?:1.8.0_275]
    at java.util.concurrent.FutureTask.run(FutureTask.java:266) [?:1.8.0_275]
    at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$201(ScheduledThreadPoolExecutor.java:180) ~[?:1.8.0_275]
    at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:293) ~[?:1.8.0_275]
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [?:1.8.0_275]
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [?:1.8.0_275]
    at java.lang.Thread.run(Thread.java:748) [?:1.8.0_275]
```

## <a name="known-issues"></a>Bekannte Probleme
* Bei Spark-Aufträgen, die über den[Hive Warehouse Connector (HWC)](apache-hive-warehouse-connector.md) übermittelt werden, kann es zu zeitweilige Fehlern kommen, wenn für den LLAP-Zielcluster die WLM-Funktion aktiviert ist. <br>
  Um die oben genannten Probleme zu vermeiden, kann der Kunde über zwei LLAP-Cluster verfügen – eins mit einem aktivierten WLM und ein anderes ohne das WLM.
  Der Kunde kann dann HWC verwenden, um seinen Spark-Cluster ohne das WLM mit dem LLAP-Cluster zu verbinden.

* Der Befehl `DISABLE WORKLOAD MANAGEMENT;` bleibt manchmal eine lange Zeit hängen. <br>
Brechen Sie den Befehl ab und überprüfen Sie den Zustand der Ressourcenpläne mit dem folgenden Befehl: `SHOW RESOURCE PLANS;`
Überprüfen Sie, ob ein aktiver Ressourcenplan verfügbar ist, bevor Sie den `DISABLE WORKLOAD MANAGEMENT` Befehl erneut ausführen; <br>

* Einige der Tez-AMs können weiterhin ausgeführt werden und werden nicht mit dem `DISABLE WORKLOAD MANAGEMENT` Befehl oder einem HS2-Neustart entfernt. <br>
Beenden Sie diese Tez-AMs über `yarn UI` oder `yarn console application` nach dem Deaktivieren des Workload-Managements.

## <a name="related-articles"></a>Verwandte Artikel
* [Hive LLAP Workload-Management](hive-workload-management.md)
* [Zusammenfassung der Befehle für das Hive LLAP Workload-Management](workload-management-commands.md)

