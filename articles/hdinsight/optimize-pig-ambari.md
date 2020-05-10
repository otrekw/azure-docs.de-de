---
title: Optimieren von Apache Pig mit Apache Ambari in Azure HDInsight
description: Verwenden Sie die Apache Ambari-Webbenutzeroberfläche zum Konfigurieren und Optimieren von Apache Pig.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 469019eb1e90654d1953156337593d5de99b46c0
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793866"
---
# <a name="optimize-apache-pig-with-apache-ambari-in-azure-hdinsight"></a>Optimieren von Apache Pig mit Apache Ambari in Azure HDInsight

Apache Ambari ist eine Webbenutzeroberfläche zum Verwalten und Überwachen von HDInsight-Clustern. Eine Einführung in die Ambari-Webbenutzeroberfläche finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

Apache Pig-Eigenschaften können über die Ambari-Webbenutzeroberfläche geändert werden, um Pig-Abfragen zu optimieren. Beim Ändern von Pig-Eigenschaften über Ambari werden die Pig-Eigenschaften direkt in der Datei `/etc/pig/2.4.2.0-258.0/pig.properties` geändert.

1. Navigieren Sie zum Ändern von Pig-Eigenschaften zur Pig-Registerkarte **Configs** (Konfigurationen), und erweitern Sie den Bereich **Advanced pig-properties** (Erweiterte Pig-Eigenschaften).

1. Suchen Sie nach dem Wert der Eigenschaft, den Sie ändern möchten, kommentieren Sie ihn aus, und ändern Sie ihn.

1. Wählen Sie oben rechts im Fenster die Option **Speichern**, um den neuen Wert zu speichern. Für einige Eigenschaften ist ggf. ein Neustart des Diensts erforderlich.

    ![Erweiterte Apache Pig-Eigenschaften](./media/optimize-pig-ambari/advanced-pig-properties.png)

> [!NOTE]  
> Alle Einstellungen auf Sitzungsebene setzen die Eigenschaftswerte in der Datei `pig.properties` außer Kraft.

## <a name="tune-execution-engine"></a>Optimieren des Ausführungsmoduls

Zwei Ausführungsmodule sind für die Ausführung von Pig-Skripts verfügbar: MapReduce und Tez. Tez ist ein optimiertes Modul, das deutlich schneller als MapReduce ist.

1. Suchen Sie im Bereich **Advanced pig-properties** (Erweiterte Pig-Eigenschaften) nach der Eigenschaft `exectype`, um das Ausführungsmodul zu ändern.

1. Der Standardwert lautet **MapReduce**. Ändern Sie ihn in **Tez**.

## <a name="enable-local-mode"></a>Aktivieren des lokalen Modus

Ähnlich wie in Hive wird der lokale Modus verwendet, um Aufträge mit relativ kleiner Datenmenge zu beschleunigen.

1. Legen Sie zum Aktivieren des lokalen Modus `pig.auto.local.enabled` auf **true** fest. Der Standardwert ist „FALSE“.

1. Aufträge mit einer Eingabedatengröße unter dem Wert der Eigenschaft `pig.auto.local.input.maxbytes` werden als kleine Aufträge angesehen. Der Standardwert ist 1 GB.

## <a name="copy-user-jar-cache"></a>Kopieren von JARs in den Cache für Benutzer

Pig kopiert die JAR-Dateien, die für UDFs erforderlich sind, in einen verteilten Cache, um sie für Taskknoten verfügbar zu machen. Diese JARs ändern sich nicht häufig. Bei einer Aktivierung können JARs mit der Einstellung `pig.user.cache.enabled` in einem Cache platziert werden, damit sie für Aufträge wiederverwendet werden können, die von demselben Benutzer ausgeführt werden. Diese Einstellung führt zu einer leichten Verbesserung der Auftragsleistung.

1. Legen Sie `pig.user.cache.enabled` auf „true“ fest, um dies zu aktivieren. Die Standardeinstellung ist „false“.

1. Legen Sie `pig.user.cache.location` auf den Basispfad fest, um den Basispfad für die JARs im Cache festzulegen. Der Standardwert lautet `/tmp`.

## <a name="optimize-performance-with-memory-settings"></a>Optimieren der Leistung über Arbeitsspeichereinstellungen

Die folgenden Einstellungen für den Arbeitsspeicher können zur Optimierung der Pig-Skriptleistung beitragen.

* `pig.cachedbag.memusage`: Die Menge an Arbeitsspeicher, die einem Behälter zugewiesen wird. Ein Behälter (Englisch: bag) ist eine Sammlung mit Tupeln. Ein Tupel ist eine geordnete Menge von Feldern, und ein Feld ist ein Datenelement. Wenn der zugeteilte Arbeitsspeicher für die Daten in einem Behälter nicht mehr ausreicht, werden sie auf dem Datenträger gespeichert. Der Standardwert ist „0,2“. Dies sind 20 Prozent des verfügbaren Arbeitsspeichers. Dieser Arbeitsspeicher wird für alle Behälter einer Anwendung gemeinsam genutzt.

* `pig.spill.size.threshold`: Die Daten von Behältern, die diesen Überlauf-Schwellenwert (in Byte) überschreiten, werden auf dem Datenträger gespeichert. Der Standardwert ist 5 MB.

## <a name="compress-temporary-files"></a>Komprimieren von temporären Dateien

Pig generiert während der Auftragsausführung temporäre Dateien. Das Komprimieren der temporären Dateien führt zu einer Leistungsverbesserung beim Lesen oder Schreiben von Dateien auf Datenträger. Die folgenden Einstellungen können verwendet werden, um temporäre Dateien zu komprimieren.

* `pig.tmpfilecompression`: Bei der Einstellung „true“ wird die Komprimierung von temporären Dateien aktiviert. Der Standardwert ist „FALSE“.

* `pig.tmpfilecompression.codec`: Der Komprimierungscodec zum Komprimieren der temporären Dateien. Die empfohlenen Komprimierungscodecs bei geringer CPU-Auslastung sind LZO und Snappy.

## <a name="enable-split-combining"></a>Aktivieren von kombinierten Teilungen

Wenn diese Einstellung aktiviert ist, werden kleinere Dateien kombiniert, um weniger Zuordnungsaufgaben zu erhalten. Diese Einstellung verbessert die Effizienz von Aufträgen mit vielen kleinen Dateien. Legen Sie `pig.noSplitCombination` auf „true“ fest, um dies zu aktivieren. Der Standardwert ist „FALSE“.

## <a name="tune-mappers"></a>Optimieren von Zuordnungen

Die Anzahl von Zuordnungen (Mappern) wird gesteuert, indem die Eigenschaft `pig.maxCombinedSplitSize` geändert wird. Hiermit wird die Größe der Daten angegeben, die von einem einzelnen Zuordnungstask verarbeitet werden sollen. Der Standardwert ist die Standardblockgröße des Dateisystems. Eine Erhöhung dieses Werts führt zu einer geringeren Anzahl von Mapper-Tasks.

## <a name="tune-reducers"></a>Optimieren von Reducern

Die Anzahl von Reducern wird basierend auf dem Parameter `pig.exec.reducers.bytes.per.reducer` berechnet. Mit dem Parameter wird die Anzahl von Bytes angegeben, die pro Reducer verarbeitet werden. Der Standardwert ist 1 GB. Wenn Sie die maximale Anzahl von Reducern angeben möchten, legen Sie einen Wert für die Eigenschaft `pig.exec.reducers.max` fest. Der Standardwert ist „999“.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Optimieren von Clustern](./hdinsight-changing-configs-via-ambari.md)
* [Optimieren von Apache HBase](./optimize-hbase-ambari.md)
* [Optimieren von Apache Hive](./optimize-hive-ambari.md)
