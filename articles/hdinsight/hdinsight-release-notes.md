---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: 0481f92666c8ca547dc427e117f9917afb257629
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511873"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

## <a name="release-date-07132020"></a>Veröffentlichungsdatum: 13.07.2020

Diese Version gilt für HDInsight 3.6 und 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="support-for-customer-lockbox-for-microsoft-azure"></a>Unterstützung der Kunden-Lockbox für Microsoft Azure
Azure HDInsight unterstützt jetzt die Azure Kunden-Lockbox. Sie stellt für Kunden eine Schnittstelle zum Überprüfen und Genehmigen oder zum Ablehnen von Zugriffsanforderungen für Kundendaten bereit. Die Lockbox wird verwendet, wenn ein Microsoft-Techniker während einer Supportanfrage auf Kundendaten zugreifen muss. Weitere Informationen finden Sie unter [Kunden-Lockbox für Microsoft Azure](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-preview).

### <a name="service-endpoint-policies-for-storage"></a>Dienstendpunkt-Richtlinien für Speicher
Kunden können nun im HDInsight-Clustersubnetz Dienstendpunkt-Richtlinien (Service Endpoint Policies, SEP) verwenden. Weitere Informationen finden Sie unter [Azure-Dienstendpunkt-Richtlinie](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview).

## <a name="deprecation"></a>Eingestellte Unterstützung
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>Einstellung der Unterstützung von Spark 2.1 und 2.2 für Spark-Cluster in HDInsight 3.6
Ab dem 1. Juli 2020 können Kunden in HDInsight 3.6 keine neuen Spark-Cluster mit Spark 2.1 und 2.2 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Ziehen Sie in Erwägung, bis 30. Juni 2020 in HDInsight 3.6 zu Spark 2.3 zu wechseln, um eine potenzielle Unterbrechung von System/Support zu vermeiden.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>Einstellung der Unterstützung von Spark 2.3 für Spark-Cluster in HDInsight 4.0
Ab dem 1. Juli 2020 können Kunden in HDInsight 4.0 keine neuen Spark-Cluster mit Spark 2.3 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Ziehen Sie in Erwägung, bis 30. Juni 2020 in HDInsight 4.0 zu Spark 2.4 zu wechseln, um eine potenzielle Unterbrechung von System/Support zu vermeiden.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>Einstellung von Kafka 1.1 für Kafka-Cluster in HDInsight 4.0
Ab dem 1. Juli 2020 können Kunden in HDInsight 4.0 keine neuen Kafka-Cluster mit Kafka 1.1 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Es empfiehlt sich, in HDInsight 4.0 bis zum 30. Juni 2020 auf Kafka 2.1 umzustellen, um potenzielle System-/Supportunterbrechungen zu vermeiden.

## <a name="behavior-changes"></a>Verhaltensänderungen
Keine Verhaltensänderungen, auf die Sie achten müssen.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt. 

### <a name="ability-to-select-different-zookeeper-sku-for-spark-hadoop-and-ml-services"></a>Möglichkeit zum Auswählen einer anderen Zookeeper-SKU für Spark, Hadoop und ML-Dienste
HDInsight unterstützt derzeit keine Änderung der Zookeeper-SKU für die Clustertypen „Spark“, „Hadoop“ und „ML-Dienste“. HDInsight verwendet die SKU „A2_v2/A2“ für Zookeeper-Knoten, die Kunden nicht in Rechnung gestellt werden. In der nächsten Version können Kunden nach Bedarf die Zookeeper-SKU für Spark, Hadoop und ML-Dienste ändern. Zookeeper-Knoten mit einer anderen SKU als A2_v2/A2 sind kostenpflichtig. A2_V2/A2 bleibt weiterhin die Standard-SKU und ist kostenfrei.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 
### <a name="fixed-hive-warehouse-connector-issue"></a>Behobenes Problem mit Hive Warehouse Connector
In der vorherigen Version gab es ein Problem bei der Verwendbarkeit von Hive Warehouse Connector. Dieses Problem wurde behoben. 

### <a name="fixed-zeppelin-notebook-truncates-leading-zeros-issue"></a>Behobenes Problem mit Zeppelin-Notebook und dem Abschneiden führender Nullen
Zeppelin hat in der Tabellenausgabe für das Zeichenfolgenformat fälschlicherweise führende Nullen abgeschnitten. Dieses Problem wurde in dieser Version behoben.

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für dieses Release gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).
