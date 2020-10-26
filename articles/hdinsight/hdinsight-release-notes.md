---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/07/2020
ms.openlocfilehash: 616e3e6c37faa3c085b8531173b557973e09fbf8
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974565"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

## <a name="release-date-10082020"></a>Veröffentlichungsdatum: 08.10.2020

Diese Version gilt sowohl für HDInsight 3.6 als auch für HDInsight 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="hdinsight-private-clusters-with-no-public-ip-and-private-link-preview"></a>Private HDInsight-Cluster ohne öffentliche IP-Adresse und private Verbindung (Vorschau)
HDInsight unterstützt jetzt die Erstellung von Clustern ohne öffentliche IP-Adresse und den Zugriff auf Cluster über private Verbindungen in der Vorschau. Kunden können die neuen erweiterten Netzwerkeinstellungen verwenden, um einen vollständig isolierten Cluster ohne öffentliche IP-Adresse zu erstellen und ihre eigenen privaten Endpunkte für den Zugriff auf den Cluster verwenden. 

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Ab dieser Version wird der Dienst schrittweise zu [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) migriert. Der gesamte Prozess kann Monate dauern. Nachdem Ihre Regionen und Abonnements migriert wurden, werden neu erstellte HDInsight-Cluster ohne Kundenaktionen in VM-Skalierungsgruppen ausgeführt. Es wird kein Breaking Change erwartet.

## <a name="deprecation"></a>Eingestellte Unterstützung
#### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>Eingestellte Unterstützung des HDInsight 3.6 ML Services-Clusters
Der Support für den Clustertyp „HDInsight 3.6 ML Services“ wird am 31. Dezember 2020 auslaufen. Kunden werden danach keine neuen 3.6 ML Services-Cluster mehr erstellen können. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Prüfen Sie [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#available-versions) das Auslaufen des Supports für HDInsight-Versionen und Clustertypen.

## <a name="behavior-changes"></a>Verhaltensänderungen
Keine Verhaltensänderung für diese Version.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt.

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Möglichkeit zum Auswählen anderer Größen virtueller Zookeeper-Computer für Spark, Hadoop und ML Services
HDInsight unterstützt derzeit keine Anpassung der Zookeeper-Knotengröße für die Clustertypen „Spark“, „Hadoop“ und „ML Services“. Standardmäßig wird A2_v2/A2 für die Größen virtueller Computer verwendet, die kostenlos zur Verfügung gestellt werden. In der kommenden Version können Sie die Größe der virtuellen Zookeeper-Computer auswählen, die für Ihr Szenario am besten geeignet ist. Für Zookeeper-Knoten mit einer anderen Größe der virtuellen Computer als A2_v2/A2 fallen Gebühren an. Die virtuellen Computer vom Typ A2_v2 und A2 werden weiterhin kostenlos angeboten.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für dieses Release gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).