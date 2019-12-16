---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: 5d9ca8d0df3eb0186add5c40765c87a4409a5660
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926421"
---
# <a name="release-notes"></a>Versionshinweise

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist einer der beliebtesten Dienste unter Enterprise-Kunden für die Open Source-Analyseframeworks Apache Hadoop und Apache Spark in Azure.

## <a name="release-date-11072019"></a>Veröffentlichungsdatum: 07.11.2019

Diese Version gilt für HDInsight 3.6 und 4.0.

> [!IMPORTANT]  
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie im Artikel zur [HDInsight-Versionsverwaltung](hdinsight-component-versioning.md).


## <a name="new-features"></a>Neue Funktionen

### <a name="hdinsight-identity-broker-hib-preview"></a>HDInsight Identity Broker (HIB) (Vorschauversion)

HDInsight Identity Broker (HIB) ermöglicht es Benutzern, sich über Multi-Factor Authentication (MFA) bei Apache Ambari anzumelden und die erforderlichen Kerberos-Tickets zu erhalten, ohne dass Kennworthashes in Azure Active Directory Domain Services (AAD-DS) erforderlich sind. Derzeit ist HIB nur für Cluster verfügbar, die über eine ARM-Vorlage bereitgestellt werden.

### <a name="kafka-rest-api-proxy-preview"></a>Kafka-REST-API-Proxy (Vorschauversion)

Der Kafka-REST-API-Proxy bietet die Bereitstellung eines REST-Proxys mit Kafka-Cluster mit Hochverfügbarkeit über eine gesicherte AAD-Autorisierung und ein OAuth-Protokoll mit nur einem Klick. 

### <a name="auto-scale"></a>Automatische Skalierung

Die Autoskalierung für Azure HDInsight ist jetzt in allen Regionen für Apache Spark- und Hadoop-Clustertypen allgemein verfügbar. Dieses Feature ermöglicht es, Big Data-Workloads kostengünstiger und auf produktivere Weise zu verwalten. Jetzt können Sie die Verwendung Ihrer HDInsight-Cluster optimieren und dabei die bedarfsabhängige Bezahlung nutzen.

Abhängig von Ihren Anforderungen können Sie zwischen der lastbasierten und der zeitplanbasierten Autoskalierung wählen. Mit der lastbasierten Autoskalierung kann die Clustergröße basierend auf den aktuellen Ressourcenanforderungen hoch- und herunterskaliert werden, während bei der zeitplanbasierten Autoskalierung die Clustergröße basierend auf einem vordefinierten Zeitplan geändert werden kann. 

Die Unterstützung für die Autoskalierung von HBase- und LLAP-Workloads befindet sich ebenfalls in der öffentlichen Vorschauversion. Weitere Informationen finden Sie unter [Automatische Skalierung von Azure HDInsight-Clustern](https://docs.microsoft.com/azure/hdinsight/hdinsight-autoscale-clusters).

### <a name="hdinsight-accelerated-writes-for-apache-hbase"></a>Beschleunigte HDInsight-Schreibvorgänge für Apache HBase 

Bei „Beschleunigte Schreibvorgänge“ werden verwaltete Azure-Premium-SSD-Datenträger zur Verbesserung der Leistung des Apache HBase-Write-Ahead-Protokolls (Write Ahead Log, WAL) verwendet. Weitere Informationen finden Sie unter [Beschleunigte Azure HDInsight-Schreibvorgänge für Apache HBase](https://docs.microsoft.com/azure/hdinsight/hbase/apache-hbase-accelerated-writes).

### <a name="custom-ambari-db"></a>Benutzerdefinierte Ambari-Datenbank

HDInsight bietet jetzt eine neue Kapazität, mit der Kunden ihre eigene SQL-Datenbank für Ambari verwenden können. Kunden können nun die richtige SQL-Datenbank für Ambari auswählen und sie auf einfache Weise basierend auf ihren eigenen geschäftlichen Wachstumsanforderungen aktualisieren. Die Bereitstellung erfolgt über eine Azure Resource Manager-Vorlage. Weitere Informationen finden Sie unter [Einrichten von HDInsight-Clustern mit einer benutzerdefinierten Ambari-Datenbank](https://docs.microsoft.com/azure/hdinsight/hdinsight-custom-ambari-db).

### <a name="f-series-virtual-machines-are-now-available-with-hdinsight"></a>Virtuelle Computer der F-Serie sind jetzt mit HDInsight verfügbar.

Virtuelle Computer (VMs) der F-Serie sind eine gute Wahl für die ersten Schritte mit HDInsight bei geringen Verarbeitungsanforderungen. Die F-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Compute-Einheit (Azure Compute Unit, ACU) das beste Preis-Leistungs-Verhältnis pro vCPU im Azure-Portfolio. Weitere Informationen finden Sie unter [Auswählen der richtigen VM-Größe für Ihren Azure HDInsight-Cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-selecting-vm-size).

## <a name="deprecation"></a>Eingestellte Unterstützung

### <a name="g-series-virtual-machine-deprecation"></a>Eingestellte Unterstützung der virtuellen Computer der G-Serie
Ab dieser Version werden VMs der G-Serie in HDInsight nicht mehr angeboten.

### <a name="dv1-virtual-machine-deprecation"></a>Eingestellte Unterstützung der virtuellen Dv1-Computer
Ab dieser Version wird die Verwendung von Dv1-VMs mit HDInsight nicht mehr unterstützt. Jegliche Kundenanforderungen für Dv1 werden automatisch mit Dv2 bedient. Es gibt keinen Preisunterschied zwischen Dv1- und Dv2-VMs.

## <a name="behavior-changes"></a>Verhaltensänderungen

### <a name="cluster-managed-disk-size-change"></a>Änderung der Größe des verwalteten Clusterdatenträgers
HDInsight stellt verwalteten Speicherplatz auf dem Datenträger mit dem Cluster bereit. Ab dieser Version wird die Größe des verwalteten Datenträgers der einzelnen Knoten im neu erstellten Cluster in 128 GB geändert.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in den kommenden Versionen durchgeführt. 

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Ab Dezember wird HDInsight stattdessen Azure-VM-Skalierungsgruppen verwenden. Weitere Informationen zu [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).

### <a name="hbase-20-to-21"></a>HBase 2.0 zu 2.1
Im bevorstehenden Release von HDInsight 4.0 erfolgt ein Upgrade der HBase-Version von Version 2.0 auf 2.1.

### <a name="a-series-virtual-machine-deprecation-for-esp-cluster"></a>Eingestellte Unterstützung der virtuellen Computer der A-Serie für ESP-Cluster
VMs der A-Serie können aufgrund der relativ geringen CPU- und Speicherkapazität zu ESP-Clusterproblemen führen. In der kommenden Version wird die Unterstützung von VMs der A-Serie zum Erstellen von neuen ESP-Clustern eingestellt.

## <a name="bug-fixes"></a>Fehlerbehebungen
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für diese Version gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
