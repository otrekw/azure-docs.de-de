---
title: Optimieren für Clusterempfehlungen
titleSuffix: Azure HDInsight
description: Optimieren von Apache HBase für Clusterempfehlungen in Azure HDInsight
author: ramkrish86
ms.author: ramvasu
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/03/2021
ms.openlocfilehash: 36d40215f759190cc9e6c6e3f4918dcbc384f94f
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97893247"
---
# <a name="apache-hbase-advisories-in-azure-hdinsight"></a>Apache HBase-Empfehlungen in Azure HDInsight

Dieser Artikel enthält einige Empfehlungen, die Ihnen bei der Optimierung der Leistung von Apache HBase in Azure HDInsight helfen. 

## <a name="optimize-hbase-to-read-most-recently-written-data"></a>Optimieren von HBase zum Lesen der zuletzt geschriebenen Daten

Wenn Sie Apache HBase in Azure HDInsight verwenden, können Sie die Konfiguration von HBase für das Szenario optimieren, in dem Ihre Anwendung die zuletzt geschriebenen Daten liest. Zum Erzielen einer hohen Leistung sollten HBase-Lesevorgänge nach Möglichkeit über den Memstore statt über den Remotespeicher erfolgen.

Laut Abfrageempfehlung sollten für eine Spaltenfamilie in einer Tabelle mehr als 75 % der Lesevorgänge über den Memstore erfolgen. Gemäß dieser Empfehlung muss auch im Falle einer Leerung im Memstore auf die zuletzt verwendete Datei zugegriffen werden, und diese muss sich im Cache befinden. Die Daten werden zunächst in den Memstore geschrieben, und das System greift dort auf die aktuellen Daten zu. Möglicherweise erkennen die internen HBase-Speicherleerungsthreads, dass ein bestimmter Bereich eine Größe von 128 MB (Standard) erreicht hat, und sie können eine Leerung auslösen. Dieses Szenario gilt auch für die neuesten Daten, die geschrieben wurden, als die Größe des Memstore ungefähr 128 MB betrug. Daher erfordert ein späterer Lesevorgang der aktuellen Datensätze möglicherweise das Lesen aus einer Datei statt aus dem Memstore. Deshalb empfiehlt es sich, die Leistung so zu optimieren, dass auch aktuelle Daten, die kürzlich geleert wurden, im Cache verbleiben können.

Um die aktuellen Daten im Cache zu optimieren, sollten Sie die folgenden Konfigurationseinstellungen beachten:

1. Legen Sie `hbase.rs.cacheblocksonwrite` auf `true` fest. Die Standardkonfiguration in HDInsight HBase lautet `true`. Vergewissern Sie sich daher, dass sie nicht auf `false` festgelegt ist.

2. Erhöhen Sie den Wert für `hbase.hstore.compactionThreshold`, damit keine Komprimierung gestartet wird. Der Standardwert beträgt `3`. Sie können den Wert z. B. auf `10` erhöhen.

3. Wenn Sie Schritt 2 ausführen und „compactionThreshold“ festlegen, ändern Sie `hbase.hstore.compaction.max` in einen höheren Wert, z. B. in `100`, und erhöhen Sie außerdem den Wert für `hbase.hstore.blockingStoreFiles`, z. B. auf `300`.

4. Wenn Sie sicher sind, dass nur in den aktuellen Daten gelesen werden muss, legen Sie `hbase.rs.cachecompactedblocksonwrite` auf **ON** fest. Diese Konfiguration bewirkt, dass die Daten auch im Falle einer Komprimierung im Cache verbleiben. Diese Konfigurationen können auch auf Familienebene festgelegt werden. 

   Führen Sie in der HBase-Shell den folgenden Befehl aus:
   
   ```
   alter '<TableName>', {NAME => '<FamilyName>', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '300'}}
   ```

5. Der Blockcache kann für eine bestimmte Familie in einer Tabelle deaktiviert werden. Stellen Sie sicher, dass er für Familien mit den neuesten Datenlesevorgängen auf **EIN** festgelegt ist. Standardmäßig ist der Blockcache für alle Familien in einer Tabelle aktiviert. Falls Sie den Blockcache für eine Familie deaktiviert haben und ihn aktivieren müssen, verwenden Sie den alter-Befehl in der HBase-Shell.

   Diese Konfigurationen helfen sicherzustellen, dass sich die Daten im Cache befinden und keine Komprimierung der aktuellen Daten erfolgt. Wenn in Ihrem Szenario eine TTL möglich ist, sollten Sie eine nach Datum gestaffelte Komprimierung in Betracht ziehen. Weitere Informationen finden Sie im [Referenzleitfaden zu Apache HBase: Date Tiered Compaction](https://hbase.apache.org/book.html#ops.date.tiered) (in englischer Sprache).  

## <a name="optimize-the-flush-queue"></a>Optimieren der Leerungswarteschlange

Laut der Empfehlung zum Optimieren der Leerungswarteschlange müssen HBase-Leerungen möglicherweise optimiert werden. Der für die Leerungshandler konfigurierte Wert ist möglicherweise nicht hoch genug.

Überprüfen Sie auf der Benutzeroberfläche des Regionsservers, ob die Leerungswarteschlange den Wert 100 überschreitet. Dieser Schwellenwert gibt an, dass die Leerungen langsam erfolgen, und möglicherweise müssen Sie die Konfiguration von `hbase.hstore.flusher.count` optimieren. Der Standardwert lautet 2. Stellen Sie sicher, dass die maximale Anzahl der Leerungsthreads nicht höher als 6 ist.

Überprüfen Sie, ob eine Empfehlung für die Optimierung der Regionsanzahl vorhanden ist. Wenn dies der Fall ist, optimieren Sie die Anzahl der Regionen, um zu überprüfen, ob hierdurch die Leerungen beschleunigt werden. Das Optimieren der Leerungsthreads kann für mehrere Optimierungen hilfreich sein, z. B. die folgenden. 

## <a name="region-count-tuning"></a>Optimierung der Anzahl der Regionen

Die Empfehlung für die Optimierung der Regionsanzahl besagt, dass HBase Aktualisierungen blockiert hat, und die Anzahl der Regionen überschreitet möglicherweise die optimal unterstützte Heapgröße. Sie können die Heapgröße, die Memstore-Größe und die Anzahl der Regionen optimieren.

Beispielszenario:

- Angenommen, die Heapgröße für den Regionsserver beträgt 10 GB. Der Standardwert für `hbase.hregion.memstore.flush.size` ist `128M`. Der Standardwert für `hbase.regionserver.global.memstore.size` ist `0.4`. Dies bedeutet, dass von den 10 GB 4 GB für den Memstore (global) zugeordnet werden.

- Angenommen, die Schreiblast ist gleichmäßig auf alle Regionen verteilt, und die Größe jeder Region erhöht sich auf 128 MB. Nur dann beträgt die maximale Anzahl der Regionen in diesem Setup `32`. Wenn ein bestimmter Regionsserver mit 32 Regionen konfiguriert ist, kann das System das Blockieren von Aktualisierungen besser vermeiden.

- Mit diesen Einstellungen beträgt die Anzahl der Regionen 100. Der globale Memstore von 4 GB ist jetzt auf 100 Regionen verteilt. Tatsächlich erhält jede Region nur 40 MB für den Memstore. Wenn die Schreibvorgänge einheitlich sind, führt das System häufige Leerungen aus, und die Größe ist geringer als 40 MB. Durch eine hohe Anzahl von Leerungsthreads (`hbase.hstore.flusher.count`) kann die Leerung beschleunigt werden.

Die Empfehlung bedeutet, dass es sinnvoll ist, die Anzahl der Regionen pro Server, die Heapgröße und die Konfiguration der Größe des globalen Memstore sowie die Optimierung der Leerungsthreads zu überdenken, damit das Blockieren der Aktualisierungen vermieden werden kann.

## <a name="compaction-queue-tuning"></a>Optimieren der Komprimierungswarteschlange

Wenn die Größe der HBase-Komprimierungswarteschlange regelmäßig 2.000 überschreitet, können Sie die Anzahl der Komprimierungsthreads erhöhen.

Wenn eine übermäßige Anzahl zu komprimierender Dateien vorhanden ist, kann dies zu einer größeren Heapauslastung im Zusammenhang mit der Interaktion der Dateien mit dem Azure-Dateisystem führen. Daher empfiehlt es sich, die Komprimierung so schnell wie möglich abzuschließen. In älteren Clustern kann es vorkommen, dass die Komprimierungskonfigurationen im Zusammenhang mit der Drosselung zu einer langsameren Komprimierung führen.

Überprüfen Sie die Konfigurationen von `hbase.hstore.compaction.throughput.lower.bound` und `hbase.hstore.compaction.throughput.higher.bound`. Wenn sie bereits auf 50 MB und 100 MB festgelegt sind, lassen Sie sie unverändert. Wenn Sie diese Einstellungen jedoch auf einen niedrigeren Wert festgelegt haben (was bei älteren Clustern der Fall war), ändern Sie die Grenzwerte in 50 MB bzw. 100 MB.

Die Konfigurationen lauten `hbase.regionserver.thread.compaction.small` und `hbase.regionserver.thread.compaction.large` (der Standardwert ist jeweils 1).
Begrenzen Sie den maximalen Wert für diese Konfiguration auf einen kleineren Wert als 3.

## <a name="full-table-scan"></a>Suche in der gesamten Tabelle

Die Empfehlung für vollständige Tabellenscans besagt, dass mehr als 75 % der ausgeführten Scans vollständige Tabellen-/Bereichsscans sein sollten. Sie können überprüfen, wie der Code die Scans aufruft, um die Abfrageleistung zu verbessern. Erwägen Sie folgende Verfahren:

* Legen Sie für jeden Scan die richtige Anfangs- und Endzeile fest.

* Verwenden Sie die **MultiRowRangeFilter**-API, damit Sie in einem Scanaufruf verschiedene Bereiche abfragen können. Weitere Informationen finden Sie in der [Dokumentation zur MultiRowRangeFilter-API](https://hbase.apache.org/2.1/apidocs/org/apache/hadoop/hbase/filter/MultiRowRangeFilter.html) (in englischer Sprache).

* Wenn ein vollständiger Tabellen- oder Bereichsscan erforderlich ist, überprüfen Sie, ob sich die Verwendung des Caches für diese Abfragen vermeiden lässt, damit andere Abfragen, die den Cache verwenden, nicht die Blöcke entfernen, die heiß sind. Legen Sie im Code die **scan**-API mit der Option **setCaching(false)** fest, um sicherzustellen, dass die Scans keinen Cache verwenden: 

   ```
   scan#setCaching(false)
   ```
   
## <a name="next-steps"></a>Nächste Schritte

[Optimieren von Apache HBasemti Ambari](../optimize-hbase-ambari.md)
