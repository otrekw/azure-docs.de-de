---
title: Leitfaden zur Größenanpassung von Interactive Query-Clustern in Azure HDInsight
description: Leitfaden zur Größenanpassung von Interactive Query in Azure HDInsight
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: a7baa9340a1f0a99b94bfcbe535c73d0b502e2a0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98933063"
---
# <a name="interactive-query-cluster-sizing-guide-in-azure-hdinsight"></a>Leitfaden zur Größenanpassung von Interactive Query-Clustern in Azure HDInsight

In diesem Dokument wird die Größenanpassung des HDInsight Interactive Query-Clusters (LLAP) für eine typische Workload beschrieben, wodurch sichergestellt werden soll, dass eine angemessene Leistung erzielt wird. Die Empfehlungen in diesem Dokument sind allgemeine Richtlinien, und für bestimmte Workloads ist ggf. eine spezifische Optimierung erforderlich.

## <a name="default-vm-types-for-interactive-query"></a>Standard-VM-Typen für Interactive Query

| Knotentyp | Instanz | Size |
|---|---|---|
| Head | D13 v2 | 8 vCPUs, 56 GB RAM, SSD mit 400 GB |
| Worker | D14 v2 | 16 vCPUs, 112 GB RAM, SSD mit 800 GB |
| ZooKeeper | A4 v2 | 4 vCPUs, 8 GB RAM, SSD mit 40 GB |

## <a name="recommended-configurations"></a>Empfohlene Konfigurationen

Die empfohlenen Konfigurationswerte basieren auf dem Workerknoten vom Typ „D14 v2“.

| Schlüssel | Wert | BESCHREIBUNG |
|---|---|---|
| yarn.nodemanager.resource.memory-mb | 102.400 (MB) | Gesamter festgelegter Speicher in MB für alle YARN-Container auf einem Knoten |
| yarn.scheduler.maximum-allocation-mb | 102400 (MB) | Maximale Zuordnung für jede Containeranforderung in Resource Manager in MB. Speicheranforderungen, die diesen Wert übersteigen, haben keinerlei Auswirkung. |
| yarn.scheduler.maximum-allocation-vcores | 12 |Maximale Anzahl der CPU-Kerne für jede Containeranforderung in Resource Manager. Anforderungen, die diesen Wert übersteigen, haben keinerlei Auswirkung. |
| yarn.scheduler.capacity.root.llap.capacity | 90% | YARN-Kapazitätszuordnung für LLAP-Warteschlange  |
| hive.server2.tez.sessions.per.default.queue | number_of_worker_nodes |Anzahl von Sitzungen für jede Warteschlange, die in „hive.server2.tez.default.queues“ benannt wird. Diese Anzahl entspricht der Anzahl von Abfragekoordinatoren (Tez-AMs). |
| tez.am.resource.memory.mb | 4\.096 (MB) | Der von Tez-AppMaster zu verwendende Speicher in MB |
| hive.tez.container.size | 4\.096 (MB) | Angegebene Tez-Containergröße in MB |
| hive.llap.daemon.num.executors | 12 | Anzahl von Executors pro LLAP-Daemon |
| hive.llap.io.threadpool.size | 12 | Threadpoolgröße für Executors |
| hive.llap.daemon.yarn.container.mb | 86.016 (MB) | Gesamter Speicher in MB, der von einzelnen LLAP-Daemons verwendet wird (Speicher pro Daemon)|
| hive.llap.io.memory.size | 409.600 (MB) | Cachegröße in MB pro LLAP-Daemon, bereitgestellter SSD-Cache ist aktiviert |
| hive.auto.convert.join.noconditionaltask.size | 2\.048 (MB) | Speichergröße in MB für Map Join |

## <a name="llap-daemon-size-estimations"></a>Größenschätzungen für LLAP-Daemon  

### <a name="yarnnodemanagerresourcememory-mb"></a>yarn.nodemanager.resource.memory-mb

Dieser Wert gibt eine maximale Speichermenge in MB an, die von den YARN-Containern auf jedem Knoten verwendet wird. Hiermit wird die Arbeitsspeichermenge angegeben, die von YARN auf diesem Knoten genutzt werden kann. Dieser Wert sollte also kleiner als die Gesamtmenge des Arbeitsspeichers für den Knoten sein.

Wert = [Gesamter physischer Arbeitsspeicher auf dem Knoten] – [Arbeitsspeicher für Betriebssystem + Andere Dienste].

Es wird empfohlen, diesen Wert auf ca. 90 % des verfügbaren Arbeitsspeichers (RAM) festzulegen. Für D14 v2 lautet der empfohlene Wert **102.400 MB**.

### <a name="yarnschedulermaximum-allocation-mb"></a>yarn.scheduler.maximum-allocation-mb

Dieser Wert gibt die maximale Zuordnung für jede Containeranforderung in Resource Manager in MB an. Speicheranforderungen, die den angegebenen Wert übersteigen, haben keinerlei Auswirkung. Der Resource Manager kann Arbeitsspeicher für Container nur in Inkrementen von `yarn.scheduler.minimum-allocation-mb` bereitstellen und darf die unter `yarn.scheduler.maximum-allocation-mb` angegebene Größe nicht überschreiten. Dieser Wert sollte nicht größer als die angegebene Gesamtmenge des Arbeitsspeicher für den Knoten sein, die unter `yarn.nodemanager.resource.memory-mb` festgelegt ist.

Für D14 v2-Workerknoten wird der Wert **102.400 MB** empfohlen.

### <a name="yarnschedulermaximum-allocation-vcores"></a>yarn.scheduler.maximum-allocation-vcores

Mit dieser Konfiguration wird die maximale Anzahl von virtuellen CPU-Kernen für jede Containeranforderung in Resource Manager angegeben. Die Anforderung eines Werts, der diese Konfiguration übersteigt, hat keinerlei Auswirkung. Diese Konfiguration ist eine globale Eigenschaft des YARN-Schedulers. Für den LLAP-Daemon-Container kann dieser Wert auf 75 % der insgesamt verfügbaren virtuellen Kerne (V-Kerne) festgelegt werden. Die restlichen 25 % müssen für NodeManager, DataNode und andere auf den Workerknoten ausgeführte Dienste reserviert werden.  

Für D14 v2-Workerknoten sind 16 V-Kerne vorhanden, und es können 75 % dieser 16 V-Kerne bereitgestellt werden. Der empfohlene Wert für LLAP-Daemon-Container lautet also **12**.

### <a name="hiveserver2tezsessionsperdefaultqueue"></a>hive.server2.tez.sessions.per.default.queue

Anhand dieses Konfigurationswerts wird die Anzahl von Tez-Sitzungen ermittelt, die für alle Warteschlangen, die mit `hive.server2.tez.default.queues` angegeben werden, parallel gestartet werden sollten. Der Wert entspricht der Anzahl von Tez-AMs (Abfragekoordinatoren). Es ist ratsam, dass dieser Wert der Anzahl von Workerknoten entspricht, damit ein Tez-AM pro Knoten vorhanden ist. Die Anzahl der Tez-AMs kann die Anzahl von LLAP-Daemon-Knoten übersteigen. Ihre Hauptaufgabe besteht darin, die Abfrageausführung zu koordinieren und Abfrageplanfragmente den entsprechenden LLAP-Daemons zur Ausführung zuzuweisen. Wir empfehlen Ihnen, hierfür ein Vielfaches der Anzahl von LLAP-Daemon-Knoten zu verwenden, um einen höheren Durchsatz zu erzielen.  

Da im HDInsight-Standardcluster vier LLAP-Daemons auf vier Workerknoten ausgeführt werden, wird als Wert **4** empfohlen.  

### <a name="tezamresourcememorymb-hivetezcontainersize"></a>tez.am.resource.memory.mb, hive.tez.container.size

Mit `tez.am.resource.memory.mb` wird die Größe für den Tez-Application Master definiert.  
Der empfohlene Wert lautet **4.096 MB**.

Mit `hive.tez.container.size` wird die Arbeitsspeichermenge definiert, die für den Tez-Container angegeben ist. Dieser Wert muss zwischen der Mindestgröße (`yarn.scheduler.minimum-allocation-mb`) und der Maximalgröße für YARN-Container (`yarn.scheduler.maximum-allocation-mb`) liegen.  
Wir empfehlen Ihnen, diesen Wert auf **4.096 MB** festzulegen.  

Eine allgemeine Regel besteht darin, den Wert niedriger als die Arbeitsspeichermenge pro Prozessor festzulegen (ein Prozessor pro Container). Verwenden Sie `Rreserve`, um Arbeitsspeicher für die Anzahl von Tez-AMs auf einem Knoten zu reservieren, bevor Sie den Arbeitsspeicher für den LLAP-Daemon angeben. Wenn Sie beispielsweise zwei Tez-AMs (jeweils 4 GB) pro Knoten nutzen, sollten Sie 82 GB von 90 GB für den LLAP-Daemon festlegen und 8 GB für die beiden Tez-AMs reservieren.

### <a name="yarnschedulercapacityrootllapcapacity"></a>yarn.scheduler.capacity.root.llap.capacity

Dieser Wert gibt einen Prozentsatz der für die LLAP-Warteschlange festgelegten Kapazität an. Der HDInsight Interactive Query-Cluster legt 90 % der Gesamtkapazität für die LLAP-Warteschlange fest, und die verbleibenden 10 % werden für die Standardwarteschlange für andere Containerzuordnungen festgelegt.  
Für D14 v2-Workerknoten wird der Wert **90** für die LLAP-Warteschlange empfohlen.

### <a name="hivellapdaemonyarncontainermb"></a>hive.llap.daemon.yarn.container.mb

Die Gesamtgröße des Arbeitsspeichers für den LLAP-Daemon richtet sich nach den folgenden Komponenten:

* Konfiguration der YARN-Containergröße (`yarn.scheduler.maximum-allocation-mb`, `yarn.scheduler.maximum-allocation-mb`, `yarn.nodemanager.resource.memory-mb`)

* Von Executors verwendeter Heapspeicher (Xmx)

    Die RAM-Menge, die nach Abzug der Größe des Toleranzbereichs verbleibt.  
    Für D14 v2: HDI 4.0 - dieser Wert (86 GB - 6 GB) = 80 GB  
    Für D14 v2: HDI 3.6 - dieser Wert (84 GB - 6 GB) = 78 GB

* Off-Heap-In-Memory-Cache pro Daemon (hive.llap.io.memory.size)

* Toleranzbereich

    Ein Teil des Off-Heap-Arbeitsspeichers, der für den Java-VM-Mehraufwand (Metaspace, Threadstapel, GC-Datenstrukturen usw.) verwendet wird. Dieser Teil sollte bei ca. 6 % der Heapgröße (Xmx) liegen. Sicherheitshalber können hierfür auch 6 % der gesamten LLAP-Daemon-Speichergröße berechnet werden. Weil dies bei aktiviertem SSD-Cache möglich ist, kann für den LLAP-Daemon der gesamte verfügbare In-Memory-Speicher ausschließlich für den Heap genutzt werden.  
    Für D14 v2 wird ein Wert von ca. **6 GB** (ceil(86 GB · 0,06)) empfohlen.  

Arbeitsspeicher pro Daemon = [Größe des In-Memory-Caches] + [Heapgröße] + [Toleranzbereich].

Dieser Wert kann wie folgt berechnet werden:

Tez-AM-Arbeitsspeicher pro Knoten = [(Anzahl von Tez-AMs/Anzahl von LLAP-Daemon-Knoten) · Tez-AM-Größe].
Größe des LLAP-Daemon-Containers = [90 % des max.Arbeitsspeichers des YARN-Containers] – [Tez-AM-Arbeitsspeicher pro Knoten].

Für D14 v2-Workerknoten mit HDI 4.0 wird der Wert (90 - (1/1 · 4 GB)) = **86 GB** empfohlen.
(Für HDI 3.6 wird der Wert **84 GB** empfohlen, da Sie ca. 2 GB für den Slider-AM reservieren sollten.)  

### <a name="hivellapiomemorysize"></a>hive.llap.io.memory.size

Diese Konfiguration gibt die Arbeitsspeichermenge an, die als Cache für den LLAP-Daemon verfügbar ist. Für die LLAP-Daemons kann eine SSD als Cache genutzt werden. Wenn Sie `hive.llap.io.allocator.mmap` auf „true“ festlegen, wird die SSD-Zwischenspeicherung aktiviert. D14 v2 verfügt über ca. 800 GB an SSD-Speicherplatz, und die SSD-Zwischenspeicherung ist für Interactive Query-Cluster (LLAP) standardmäßig aktiviert. In der Konfiguration ist festgelegt, dass 50 % des SSD-Speicherplatzes für den Off-Heap-Cache genutzt wird.

Für D14 v2 lautet der empfohlene Wert **409.600 MB**.  

Bei anderen virtuellen Computern, für die die SSD-Zwischenspeicherung nicht aktiviert ist, sollte ein Teil des verfügbaren Arbeitsspeichers für die LLAP-Zwischenspeicherung bereitgestellt werden, um eine bessere Leistung zu erzielen. Passen Sie die Gesamtgröße des Arbeitsspeichers für den LLAP-Daemon wie folgt an:  

Gesamter Arbeitsspeicher des LLAP-Daemons = [LLAP-Cachegröße] + [Heapgröße] + [Toleranzbereich].

Wir empfehlen Ihnen, die Cache- und Heapgröße so anzupassen, dass sie für Ihre Workload bestmöglich geeignet ist.  

### <a name="hivellapdaemonnumexecutors"></a>hive.llap.daemon.num.executors

Diese Konfiguration steuert die Anzahl von Executors pro LLAP-Daemon, die Tasks parallel ausführen können. In diesem Wert sind die Anzahl verfügbarer V-Kerne, die pro Executor festgelegte Arbeitsspeichermenge und der pro LLAP-Daemon insgesamt verfügbare Arbeitsspeicher zusammengefasst. Normalerweise soll dieser Wert möglichst nah an der Anzahl von Kernen liegen.

Für D14 v2 sind 16 V-Kerne verfügbar, aber es können nicht alle V-Kerne bereitgestellt werden. Auf den Workerknoten werden auch andere Dienste ausgeführt, z. B. NodeManager, DataNode und Metrics Monitor, für die V-Kerne verfügbar sein müssen. Dieser Wert kann auf bis zu 75 % der verfügbaren V-Kerne in einem Knoten festgelegt werden.

Für D14 v2 wird der Wert **12** (0,75 · 16) empfohlen.

Wir empfehlen Ihnen, pro Executor ca. 6 GB an Heapspeicher zu reservieren. Passen Sie Ihre Anzahl von Executors basierend auf der verfügbaren LLAP-Daemon-Größe und der Anzahl verfügbarer V-Kerne pro Knoten an.  

### <a name="hivellapiothreadpoolsize"></a>hive.llap.io.threadpool.size

Dieser Wert gibt die Threadpoolgröße für Executors an. Da Executors wie angegeben festgelegt sind, entspricht er der Anzahl von Executors pro LLAP-Daemon.

Wir empfehlen Ihnen, diesen Wert für D14 v2 auf **12** festzulegen.

Diese Konfiguration darf den Wert `yarn.nodemanager.resource.cpu-vcores` nicht überschreiten.

### <a name="hiveautoconvertjoinnoconditionaltasksize"></a>hive.auto.convert.join.noconditionaltask.size

Stellen Sie sicher, dass Sie `hive.auto.convert.join.noconditionaltask` aktiviert haben, damit dieser Parameter wirksam wird. Mit dieser Konfiguration kann der Benutzer die Größe der Tabellen angeben, die in den Speicher passen, um den Map Join-Vorgang durchzuführen. Wenn die Summe des n-1-Größenwerts von `tables/partitions` für den n-Wege-Join-Vorgang kleiner als der konfigurierte Wert ist, wird der Map Join-Vorgang ausgewählt. Die Größe des LLAP-Executor-Speichers sollte verwendet werden, um den Schwellenwert für die automatische Map Join-Konvertierung zu berechnen.

Wir empfehlen Ihnen, diesen Wert für D14 v2 auf **2.048 MB** festzulegen.

Wir empfehlen Ihnen, diesen Wert so festzulegen, dass er für Ihre Workload möglichst gut geeignet ist. Wenn Sie ihn zu niedrig festlegen, wird die Funktion für die automatische Konvertierung ggf. nicht verwendet. Bei einem zu hohen Wert kann es zu GC-Pausen kommen, und dies kann eine negative Auswirkung auf die Abfrageleistung haben.

## <a name="next-steps"></a>Nächste Schritte

* [Gatewayrichtlinien](gateway-best-practices.md)
* [Entmystifizierung der Optimierung des Apache Tez-Arbeitsspeichers: Schritt für Schritt](https://community.cloudera.com/t5/Community-Articles/Demystify-Apache-Tez-Memory-Tuning-Step-by-Step/ta-p/245279)
* [Anpassung der Größe des Map Join-Arbeitsspeichers für LLAP](https://community.cloudera.com/t5/Community-Articles/Map-Join-Memory-Sizing-For-LLAP/ta-p/247462)
* [LLAP: Kurze Übersicht zur Architektur](https://community.cloudera.com/t5/Community-Articles/LLAP-a-one-page-architecture-overview/ta-p/247439)
* [Ausführliche Betrachtung von Hive LLAP](https://community.cloudera.com/t5/Community-Articles/Hive-LLAP-deep-dive/ta-p/248893)
