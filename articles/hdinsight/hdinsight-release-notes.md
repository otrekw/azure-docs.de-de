---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: 1ae4e2a1e0d67a0a09c19b517245ffc6d92d17df
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2020
ms.locfileid: "94629919"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

Wenn Sie die Anmerkungen zu dieser Version abonnieren möchten, sehen Sie sich die Releases auf [diesem GitHub-Repository](https://github.com/hdinsight/release-notes/releases) an.

## <a name="release-date-11092020"></a>Veröffentlichungsdatum: 09.11.2020

Diese Version gilt sowohl für HDInsight 3.6 als auch für HDInsight 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="hdinsight-identity-broker-hib-is-now-ga"></a>HDInsight Identity Broker (HIB) ist jetzt GA
Der HDInsight Identity Broker (HIB), der die OAuth-Authentifizierung für ESP-Cluster ermöglicht, ist jetzt mit diesem Release allgemein verfügbar. HIB-Cluster, die nach diesem Release erstellt werden, verfügen über die neuesten HIB-Features:
- Hochverfügbarkeit (High Availability, HA)
- Unterstützung von Multi-Factor Authentication (MFA)
- Verbundbenutzer melden sich ohne Kennworthashsynchronisierung bei AAD-DS an. Weitere Informationen finden Sie in der [HIB-Dokumentation](https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker).

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Ab dieser Version wird der Dienst schrittweise zu [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) migriert. Der gesamte Prozess kann Monate dauern. Nachdem Ihre Regionen und Abonnements migriert wurden, werden neu erstellte HDInsight-Cluster ohne Kundenaktionen in VM-Skalierungsgruppen ausgeführt. Es wird kein Breaking Change erwartet.

## <a name="deprecation"></a>Eingestellte Unterstützung
### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>Eingestellte Unterstützung des HDInsight 3.6 ML Services-Clusters
Der Support für den Clustertyp „HDInsight 3.6 ML Services“ wird am 31. Dezember 2020 auslaufen. Kunden werden nach dem 31. Dezember 2020 keine neuen 3.6 ML Services-Cluster mehr erstellen können. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Prüfen Sie [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#available-versions) das Auslaufen des Supports für HDInsight-Versionen und Clustertypen.

### <a name="disabled-vm-sizes"></a>Deaktivierte VM-Größen
Ab dem 16. November 2020 hindert HDInsight neue Kunden daran, Cluster mit den VM-Größen standand_A8, standand_A9, standand_A10 und standand_A11 zu erstellen. Bestandskunden, die diese VM-Größen in den letzten drei Monaten verwendet haben, sind nicht betroffen. Ab dem 9. Januar 2021 hindert HDInsight alle Kunden daran, Cluster mit den VM-Größen standand_A8, standand_A9, standand_A10 und standand_A11 zu erstellen. Vorhandene Cluster werden unverändert ausgeführt. Sie sollten zu HDInsight 4.0 wechseln, um potenzielle System-/Supportunterbrechungen zu vermeiden.

## <a name="behavior-changes"></a>Verhaltensänderungen
Keine Verhaltensänderung für diese Version.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt.

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Möglichkeit zum Auswählen anderer Größen virtueller Zookeeper-Computer für Spark, Hadoop und ML Services
HDInsight unterstützt derzeit keine Anpassung der Zookeeper-Knotengröße für die Clustertypen „Spark“, „Hadoop“ und „ML Services“. Standardmäßig wird A2_v2/A2 für die Größen virtueller Computer verwendet, die kostenlos zur Verfügung gestellt werden. In der kommenden Version können Sie die Größe der virtuellen Zookeeper-Computer auswählen, die für Ihr Szenario am besten geeignet ist. Für Zookeeper-Knoten mit einer anderen Größe der virtuellen Computer als A2_v2/A2 fallen Gebühren an. Die virtuellen Computer vom Typ A2_v2 und A2 werden weiterhin kostenlos angeboten.

### <a name="default-cluster-version-will-be-changed-to-40"></a>Standardclusterversion wird in 4.0 geändert
Ab Februar 2021 wird die Standardversion des HDInsight-Clusters von 3.6 in 4.0 geändert. Weitere Informationen zu verfügbaren Versionen finden Sie unter [Verfügbare Versionen](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#available-versions). Erfahren Sie mehr über die Neuerungen in [HDInsight 4.0](https://docs.microsoft.com/azure/hdinsight/hdinsight-version-release).

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>Ende der Unterstützung von HDInsight 3.6 am 30. Juni 2021
Die Unterstützung von HDInsight 3.6 endet bald. Ab dem 30. Juni 2021 können Kunden keine neuen HDInsight 3.6-Cluster mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Sie sollten zu HDInsight 4.0 wechseln, um potenzielle System-/Supportunterbrechungen zu vermeiden.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 
### <a name="fix-issue-for-restarting-vms-in-cluster"></a>Problembehebung beim Neustarten von VMs im Cluster
Das Problem beim Neustarten von VMs im Cluster wurde behoben. Sie können [PowerShell oder die REST-API verwenden, um die Knoten im Cluster](https://docs.microsoft.com/azure/hdinsight/cluster-reboot-vm) erneut zu starten.

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für dieses Release gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](./hdinsight-component-versioning.md).
