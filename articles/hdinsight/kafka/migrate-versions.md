---
title: Migrieren von Apache Kafka-Workloads zu Azure HDInsight 4.0
description: Erfahren Sie, wie Sie Apache Kafka-Workloads in HDInsight 3.6 zu HDInsight 4.0 migrieren.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: e15ebb13aee0e5dd814688ae77edaded667d54ac
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104864124"
---
# <a name="migrate-apache-kafka-workloads-to-azure-hdinsight-40"></a>Migrieren von Apache Kafka-Workloads zu Azure HDInsight 4.0

Azure HDInsight 4.0 enthält die neuesten Open-Source-Komponenten mit erheblichen Verbesserungen in Bezug auf Leistung, Konnektivität und Sicherheit. In diesem Artikel wird erläutert, wie Sie Apache Kafka-Workloads in HDInsight 3.6 zu HDInsight 4.0 migrieren. Nach dem Migrieren der Workloads zu HDInsight 4.0 können Sie viele der neuen Funktionen nutzen, die in HDInsight 3.6 nicht verfügbar sind.

## <a name="hdinsight-36-kafka-migration-paths"></a>HDInsight 3.6-Migrationspfade für Kafka

HDInsight 3.6 unterstützt zwei Versionen von Kafka: 1.0.0 und 1.1.0. HDInsight 4.0 unterstützt die Versionen 1.1.0 und 2.1.0. Abhängig davon, welche Version von Kafka und welche Version von HDInsight Sie ausführen möchten, werden verschiedene Migrationspfade unterstützt. Diese Pfade werden unten erläutert und sind in der folgenden Abbildung dargestellt.

* **Ausführen der neuesten Versionen von Kafka und HDInsight (empfohlen):** Migrieren Sie eine HDInsight 3.6- und eine Kafka 1.0.0- oder 1.1.0-Anwendung zu HDInsight 4.0 mit Kafka 2.1.0 (Pfade D und E unten).
* **Ausführen der neuesten Version von HDInsight, jedoch nur einer neueren Version von Kafka:** Migrieren Sie eine HDInsight 3.6- und eine Kafka 1.0.0-Anwendung zu HDInsight 4.0 mit Kafka 1.1.0 (Pfad B unten).
* **Ausführen der neuesten Version von HDInsight und Beibehalten der Kafka-Version:** Migrieren Sie eine HDInsight 3.6- und eine Kafka 1.1.0-Anwendung zu HDInsight 4.0 mit Kafka 1.1.0 (Pfad C unten).
* **Ausführen einer neueren Version von Kafka und Beibehalten der HDInsight-Version:** Migrieren Sie eine Kafka 1.0.0-Anwendung zu Kafka 1.1.0, und behalten Sie HDInsight 3.6 bei (Pfad A unten). Beachten Sie, dass auch für diese Option ein neuer Cluster bereitgestellt werden muss. Die Aktualisierung der Kafka-Version auf einem vorhandenen Cluster ist nicht möglich. Nachdem Sie einen Cluster mit der gewünschten Version erstellt haben, migrieren Sie die Kafka-Clients zur Verwendung des neuen Clusters.

:::image type="content" source="./media/upgrade-threesix-to-four/apache-kafka-upgrade-path.png" alt-text="Upgradepfade für Apache Kafka in HDInsight 3.6" border="false":::

## <a name="apache-kafka-versions"></a>Apache Kafka-Versionen

### <a name="kafka-110"></a>Kafka 1.1.0
  
Wenn Sie von Kafka 1.0.0 zu 1.1.0 migrieren, können Sie die folgenden neuen Funktionen nutzen:

* Verbesserungen am Kafka-Controller beschleunigen das kontrollierte Herunterfahren, sodass Broker schneller neu gestartet und Probleme schneller behoben werden können. 
* Verbesserungen an der [FetchRequests-Logik](https://issues.apache.org/jira/browse/KAFKA-6254), die eine größere Anzahl von Partitionen (und damit auch mehr Themen) im Cluster ermöglichen. 
* Kafka Connect unterstützt [Datensatzheader](https://issues.apache.org/jira/browse/KAFKA-5142) und [reguläre Ausdrücke](https://issues.apache.org/jira/browse/KAFKA-3073) für Themen. 

Eine umfassende Liste der Aktualisierungen finden Sie in den [Versionshinweisen zu Apache Kafka 1.1](https://archive.apache.org/dist/kafka/1.1.0/RELEASE_NOTES.html).

### <a name="apache-kafka-210"></a>Apache Kafka 2.1.0

Wenn Sie zu Kafka 2.1 migrieren, können Sie die folgenden Funktionen nutzen:

* Optimierte Brokerresilienz durch ein verbessertes Replikationsprotokoll
* Neue Funktionen in der KafkaAdminClient-API
* Konfigurierbare Kontingentverwaltung
* Unterstützung für die Zstandard-Komprimierung

Eine umfassende Liste der Aktualisierungen finden Sie in den [Versionshinweisen zu Apache Kafka 2.0](https://archive.apache.org/dist/kafka/2.0.0/RELEASE_NOTES.html) und den [Versionshinweisen zu Apache Kafka 2.1](https://archive.apache.org/dist/kafka/2.1.0/RELEASE_NOTES.html).

## <a name="kafka-client-compatibility"></a>Kafka-Clientkompatibilität

Neue Kafka-Broker unterstützen ältere Clients. Mit [KIP-35: Retrieving protocol version](https://cwiki.apache.org/confluence/display/KAFKA/KIP-35+-+Retrieving+protocol+version) (Abrufen der Protokollversion) wurde ein Mechanismus zur dynamischen Bestimmung der Funktionalität eines Kafka-Brokers eingeführt. Mit [KIP-97: Improved Kafka Client RPC Compatibility Policy](https://cwiki.apache.org/confluence/display/KAFKA/KIP-97%3A+Improved+Kafka+Client+RPC+Compatibility+Policy) (Kompatibilitätsrichtlinie für verbesserten Kafka-Client-RPC) wurden eine neue Kompatibilitätsrichtlinie und Garantien für den Java-Client eingeführt. Zuvor musste die Interaktion zwischen einem Kafka-Client und einem Broker derselben oder einer höheren Version erfolgen. Nun können neuere Versionen der Java-Clients und anderer Clients, die KIP-35 unterstützen, z. B. `librdkafka`, auf ältere Anforderungstypen zurückgreifen oder entsprechende Fehler auslösen, wenn die Funktionalität nicht verfügbar ist.

:::image type="content" source="./media/upgrade-threesix-to-four/apache-kafka-client-compatibility.png" alt-text="Aktualisieren der Kafka-Clientkompatibilität" border="false":::

Beachten Sie, dass das nicht bedeutet, dass der Client ältere Broker unterstützt.  Weitere Informationen finden Sie in der [Kompatibilitätsmatrix](https://cwiki.apache.org/confluence/display/KAFKA/Compatibility+Matrix).

## <a name="general-migration-process"></a>Allgemeiner Migrationsvorgang

In der folgenden Migrationsanleitung wird davon ausgegangen, dass ein Apache Kafka 1.0.0- oder 1.1.0-Cluster in HDInsight 3.6 in einem einzelnen virtuellen Netzwerk bereitgestellt wird. Der vorhandene Broker enthält einige Themen und wird von Producern und Consumern aktiv verwendet.

:::image type="content" source="./media/upgrade-threesix-to-four/apache-kafka-presumed-environment.png" alt-text="Aktuelle angenommene Kafka-Umgebung" border="false":::

Führen Sie zur Migration die folgenden Schritte aus:

1. **Stellen Sie einen neuen HDInsight 4.0-Cluster und entsprechende Clients zum Testen bereit:** Stellen Sie einen neuen HDInsight 4.0-Kafka-Cluster bereit. Wenn mehrere Kafka-Clusterversionen ausgewählt werden können, empfiehlt sich die Auswahl der neuesten Version. Legen Sie nach der Bereitstellung einige Parameter fest, und erstellen Sie ein Thema mit dem gleichen Namen wie die vorhandene Umgebung. Legen Sie außerdem die TLS- und die Bring-Your-Own-Key-Verschlüsselung (BYOK) nach Bedarf fest. Überprüfen Sie dann, ob alles mit dem neuen Cluster ordnungsgemäß ausgeführt wird.

    :::image type="content" source="./media/upgrade-threesix-to-four/deploy-new-hdinsight-clusters.png" alt-text="Bereitstellen neuer HDInsight 4.0-Cluster" border="false":::

1. **Ändern Sie den Cluster für die Produceranwendung, und warten Sie, bis alle Warteschlangendaten von den aktuellen Consumern verarbeitet wurden:** Wenn der neue Kafka-Cluster in HDInsight 4.0 verwendet werden kann, ändern Sie das vorhandene Producerziel in den neuen Cluster. Ändern Sie das Ziel erst, wenn die vorhandene Consumeranwendung alle Daten vom vorhandenen Cluster verarbeitet hat.

    :::image type="content" source="./media/upgrade-threesix-to-four/switch-cluster-producer-app.png" alt-text="Ändern des Clusters für die Produceranwendung" border="false":::

1. **Ändern Sie den Cluster für die Consumeranwendung:** Nachdem Sie überprüft haben, ob die vorhandene Consumeranwendung alle Daten des vorhandenen Clusters genutzt hat, ändern Sie die Verbindung in den neuen Cluster.

    :::image type="content" source="./media/upgrade-threesix-to-four/switch-cluster-consumer-app.png" alt-text="Ändern des Clusters für die Consumeranwendung" border="false":::

1. **Entfernen Sie den alten Cluster, und testen Sie die Anwendungen nach Bedarf:** Wenn der Wechsel abgeschlossen ist und der Cluster ordnungsgemäß ausgeführt wird, entfernen Sie den alten HDInsight 3.6-Kafka-Cluster und die beim Testen verwendeten Producer und Consumer.

## <a name="next-steps"></a>Nächste Schritte

* [Leistungsoptimierung für Apache Kafka HDInsight-Cluster](apache-kafka-performance-tuning.md)
* [Schnellstart: Erstellen eines Apache Kafka-Clusters in Azure HDInsight im Azure-Portal](apache-kafka-get-started.md)
