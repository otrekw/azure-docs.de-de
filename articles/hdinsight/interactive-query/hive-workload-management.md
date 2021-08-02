---
title: Die Hive LLAP Workload-Managementfunktion
titleSuffix: Azure HDInsight
description: Die Hive LLAP Workload-Managementfunktion
ms.service: hdinsight
ms.topic: how-to
author: guptanikhil007
ms.author: guptan
ms.reviewer: jasonh
ms.date: 05/25/2021
ms.openlocfilehash: 8dde65d0547552af90caddd214dd1ceda5a754ee
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482416"
---
# <a name="hive-llap-workload-management-wlm-feature"></a>Die Hive LLAP Workload-Managementfunktion (WLM)
In einem Interactive Query-Cluster ist die Ressourcenverwaltung zwingend erforderlich, insbesondere in einer mehrinstanzenfähigen Umgebung. Die Hive LLAP (analytische Verarbeitung mit geringer Latenz) verwendet das Workload-Management, um den Benutzern zu ermöglichen, bestimmte Workload-Anforderungen zu erfüllen und Konflikte für diese Ressourcen zu vermeiden. <br> Die Workload-Verwaltung implementiert Ressourcenpools (auch als Abfragepools bezeichnet), mit denen Sie die in der Hive/LLAP verfügbaren Ressourcen in Pools unterteilen können, die für bestimmte Workloads verwendet werden sollen.
Außerdem können Sie den Prozentsatz der Ressourcen und der Abfrageparallelität für jeden einzelnen Ressourcenpool konfigurieren.

![„LLAP-Architektur.“](./media/hive-workload-management/llap-architecture.png)

## <a name="enable-hive-llap-workload-management-feature-for-hdinsight-clusters"></a>Aktivieren Sie die Hive LLAP-Workload-Managementfunktion für die HDInsight-Cluster

Aktivieren Sie die Hive LLAP-Workload-Managementfunktion in den HDInsight Interactive Query-Clustern, indem Sie die folgenden Schritte ausführen:
1. Erstellen Sie eine neue Yarn-Warteschlange, die zum Aufrufen der Tez-AMs für die Workload-Verwaltung verwendet werden kann.
2. Ändern Sie die Clusterkonfigurationen über „Ambari“, um die Funktion in Hive zu aktivieren.
3. Erstellen Sie einen Ressourcenplan und aktivieren Sie diesen.

### <a name="create-a-new-yarn-queue-suitable-for-workload-management-feature"></a>Erstellen Sie eine neue Yarn-Warteschlange, die für die Workload-Verwaltungsfunktion geeignet ist
Erstellen Sie eine neun Yarn-Warteschlange namens `wm` mithilfe der folgenden [Anleitung](../hdinsight-troubleshoot-yarn.md).
Konfigurieren Sie die `wm`Warteschlange im Cluster basierend auf den folgenden Konfigurationen:

| QueueName   | Capacity | Maximale Kapazität | Priority | Maximale AM-Ressource |
|------------|---------|--------------|----------|---------------------|
| `default`   | 5 %       | 5 %           | 0        | 33 %                 |
| `llap`      | 85%      | 100%         | 10       | 33 %                 |
| `wm`        | 10 %      | 15 %          | 9        | 100%                |

Überprüfen Sie, ob die `wm` Warteschlangenkonfiguration wie die unten Angezeigte aussieht.
Die :::image type="content" source="./media/hive-workload-management/wm-yarn-queue.png" alt-text="WM-Warteschlangenkonfiguration.":::

### <a name="enable-workload-management-feature-in-hive-configs"></a>Aktivieren Sie die Workload-Verwaltungsfunktion in den Hive-Konfigurationen
Fügen Sie die folgende Eigenschaft zu der benutzerdefinierten hiveserver2-interactive-site hinzu und legen Sie ihren Wert auf den Namen der neu erstellten Yarn-Warteschlange, d. h. `wm`, fest. Starten Sie den interaktiven HiveServer neu, damit die Konfigurationsänderungen übernommen werden.
```
hive.server2.tez.interactive.queue=wm
```

### <a name="create-resource-plan"></a>Erstellen Sie einen Ressourcenplan
Es folgt ein Beispiel zum Erstellen eines Standardressourcenplans.
Ein ![Standard Ressourcenplan.](./media/hive-workload-management/wlm-resourceplan.jpg)

Führen Sie die folgenden Befehle über „BeeLine“ aus, um den oben genannten Ressourcenplan zu erstellen.

#### <a name="commands-to-create-view-and-drop-the-resource-plan"></a>Die Befehle zum Erstellen, Anzeigen und Verwerfen des Ressourcenplans
```hql
# CREATE RESOURCE PLAN
CREATE RESOURCE PLAN demo_plan;

# CREATE POOLS
ALTER POOL demo_plan.default SET ALLOC_FRACTION = 0.65, QUERY_PARALLELISM = 2;
CREATE POOL demo_plan.etl WITH ALLOC_FRACTION = 0.20, QUERY_PARALLELISM = 2;
CREATE POOL demo_plan.sys_accounts WITH ALLOC_FRACTION = 0.15, QUERY_PARALLELISM = 1;

# CREATE MAPPING
CREATE USER MAPPING 'hive' IN demo_plan TO sys_accounts WITH ORDER 1;
 
# CREATE TRIGGERS
CREATE TRIGGER demo_plan.defaultToETL WHEN  ELAPSED_TIME > 20000 DO MOVE TO etl;
ALTER TRIGGER demo_plan.defaultToETL ADD TO POOL default;
CREATE TRIGGER demo_plan.ETLKill  WHEN ELAPSED_TIME > 40000 DO KILL;
ALTER TRIGGER demo_plan.ETLKill ADD TO POOL etl;

# VALIDATE PLAN
ALTER RESOURCE PLAN demo_plan VALIDATE;

# ENABLE PLAN
ALTER RESOURCE PLAN demo_plan ENABLE;

#  ACTIVATE PLAN
ALTER RESOURCE PLAN demo_plan ACTIVATE;

# SHOW RESOURCE PLAN
SHOW RESOURCE PLANS;
SHOW RESOURCE PLAN demo_plan;

# VALIDATE PLAN
ALTER RESOURCE PLAN demo_plan VALIDATE;

# ENABLE PLAN
ALTER RESOURCE PLAN demo_plan ENABLE;

#  ACTIVATE PLAN
ALTER RESOURCE PLAN demo_plan ACTIVATE;

# SHOW RESOURCE PLAN
SHOW RESOURCE PLANS;
SHOW RESOURCE PLAN demo_plan;

# DISABLE PLAN
-- In case plan is in active state first run:
-- DISABLE WORKLOAD MANAGEMENT;
ALTER RESOURCE PLAN demo_plan DISABLE;

# DROP RESOURCE PLAN
DROP RESOURCE PLAN demo_plan;
```

## <a name="understanding-resource-plan"></a>Das Verstehen von den Ressourcenplänen
Um einen optimalen Ressourcenplan zu erhalten, muss man die Workload-Anforderungen umfassend verstehen.

### <a name="number-of-pools"></a>Die Anzahl der Pools
Die Anzahl der Pools wird durch die gesamte Abfrageparallelität (Minimum eine Abfrage pro Pool) beschränkt.
Die meisten Workloads erfordern selten mehr als drei Pools. 
- Die Standardeinstellung für interaktive Abfragen 
- ETL/Batch für zeitintensive Abfragen
- Sys für Systemadministratoren

### <a name="total-query_parallelism"></a>Gesamt QUERY_PARALLELISM
Der gesamte QUERY_PARALLELISM oder die Anzahl der gesamten gleichzeitigen Abfragen kann mit der folgenden Formel ermittelt werden:

```
Number of total concurrent queries(Tez AMs) = Math.floor( (total cluster memory capacity / size of Tez AM container) x percentage of wm queue capacity)
```

Beispiel: <br/>
Angenommen, die Tez AM-Containergröße beträgt 4 GB und die Gesamtspeicherkapazität des Yarn-Clusters beträgt 400 GB, von denen dann 10 % der WM-Warteschlange zugeordnet sind, <br/>
Anzahl der gesamten gleichzeitigen Abfragen = Untergrenze((400/4) x 0,10) = 10

> [!Tip]
> Sie sollten über eine etwas größere Kapazität in der WM-Warteschlange verfügen als erforderlich ist, um zu vermeiden, dass Tez-AMs im akzeptierten Zustand hängen bleiben. Das bedeutet, dass die `wm` Warteschlangenkapazität auf 10,01 % festgelegt werden kann und die `default` Warteschlangenkapazität auf 4,99 % reduziert werden.

### <a name="mappings"></a>Zuordnungen
Die Zuordnungen bieten einen Mechanismus zum weiterleiten von Abfragen an bestimmte Pools. Mit einer zunehmender Anzahl von Zuordnungen können mehrere Regeln für eine bestimmte Abfrage gelten. Beachten Sie Folgendes, um festzulegen welche Regel Vorrang haben soll: Wenn die Reihenfolge mit der optionalen `WITH ORDER` Klausel angegeben wird, hat die Regel der niedrigeren Reihenfolge Priorität. Andernfalls haben `user` Regeln Vorrang vor `application` Regeln und `application` Regeln haben Vorrang vor `group` Regeln. <br/>
Die Reihenfolge der Gruppenregeln mit der gleichen Priorität ist nicht definiert.


> [!Note]
> * Die Tez-AMs in der `llap` Warteschlange bleiben ungenutzt, wenn der WLM-Plan aktiv ist. Diese Tez-AMs in der `llap` Warteschlange sind sofort verfügbar, falls der WLM-Ressourcenplan deaktiviert ist.
> * Das Aktivieren des WLM-Ressourcenplans startet die Anzahl der Tez AMs, die der für den `QUERY_PARALLELISM` angegebenen Ressourcenplan konfigurierten Gesamtzahl entspricht. Die `wm` Warteschlangengröße sollte optimiert werden, um zu vermeiden, dass diese Tez AMs im Zustand AKZEPTIERT hängen bleiben.
> * Wir unterstützen nur die Verwendung der folgenden beiden Indikatoren für die Verwendung in den Ressourcenplänen:
>    * AUSFÜHRUNGS_ZEIT
>    * VERSTRICHENE_ZEIT

## <a name="related-articles"></a>Verwandte Artikel
* Eine [Zusammenfassung der Befehle für das Hive LLAP-Workload-Management](workload-management-commands.md)
* Die [Problembehandlung der Hive LLAP-Workload-Managementprobleme](troubleshoot-workload-management-issues.md)


