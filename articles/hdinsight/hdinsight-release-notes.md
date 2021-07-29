---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
ms.custom: references_regions
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/02/2021
ms.openlocfilehash: 61b32852de0cb15db3860b72561f87e56db8f40d
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111903849"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

Wenn Sie die Anmerkungen zu dieser Version abonnieren möchten, sehen Sie sich die Releases auf [diesem GitHub-Repository](https://github.com/hdinsight/release-notes/releases) an.


## <a name="release-date-06022021"></a>Veröffentlichungsdatum: 02.06.2021

Diese Version gilt sowohl für HDInsight 3.6 als auch für HDInsight 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

Die Betriebssystemversionen für dieses Release sind:
- HDInsight 3.6: Ubuntu 16.04.7 LTS
- HDInsight 4.0: Ubuntu 18.04.5 LTS

## <a name="new-features"></a>Neue Funktionen
### <a name="os-version-upgrade"></a>Upgrade der Betriebssystemversion
Wie im [Releasezyklus](https://ubuntu.com/about/release-cycle) von Ubuntu verwiesen, erreicht der Ubuntu 16.04-Kernel im April 2021 das Ende der Lebensdauer (End of Life, EOL). Wir haben mit diesem Release mit dem Rollout des neuen HDInsight 4.0-Clusterimages unter Ubuntu 18.04 begonnen. Neu erstellte HDInsight 4.0-Cluster werden standardmäßig unter Ubuntu 18.04 ausgeführt, sobald sie verfügbar sind. Vorhandene Cluster unter Ubuntu 16.04 werden ohne vollständige Unterstützung ausgeführt.

HDInsight 3.6 wird weiterhin unter Ubuntu 16.04 ausgeführt. Ab dem 1. Juli 2021 erfolgt ein Wechsel zum Basic-Support (vom Standard-Support). Weitere Informationen zu Datumsangaben und Supportoptionen finden Sie unter [Azure HDInsight-Versionen](./hdinsight-component-versioning.md#supported-hdinsight-versions). Ubuntu 18.04 wird für HDInsight 3.6 nicht unterstützt. Wenn Sie Ubuntu 18.04 verwenden möchten, müssen Sie Ihre Cluster zu HDInsight 4.0 migrieren. 

Sie müssen Ihre Cluster löschen und neu erstellen, wenn Sie vorhandene HDInsight 4.0-Cluster nach Ubuntu 18.04 verschieben möchten. Planen Sie ein, die Cluster zu erstellen oder neu zu erstellen, nachdem die Unterstützung für Ubuntu 18.04 verfügbar ist.

Nachdem Sie den neuen Cluster erstellt haben, können Sie eine SSH-Shell für den Cluster öffnen und `sudo lsb_release -a` ausführen, um zu überprüfen, ob er unter Ubuntu 18.04 ausgeführt wird. Es wird empfohlen, Anwendungen zuerst in Testabonnements zu testen, bevor Sie in die Produktion wechseln. [Weitere Informationen zum HDInsight Ubuntu 18.04-Update](./hdinsight-ubuntu-1804-qa.md).

### <a name="scaling-optimizations-on-hbase-accelerated-writes-clusters"></a>Skalierungsoptimierungen für HBase-Cluster mit beschleunigten Schreibvorgängen
Von HDInsight wurden einige Verbesserungen und Optimierungen bei der Skalierung für HBase-Cluster mit beschleunigtem Schreibzugriff vorgenommen. [Weitere Informationen zu beschleunigten HBase-Schreibvorgängen](./hbase/apache-hbase-accelerated-writes.md).

## <a name="deprecation"></a>Eingestellte Unterstützung
Keine eingestellte Unterstützung für dieses Release.

## <a name="behavior-changes"></a>Verhaltensänderungen
### <a name="disable-stardard_a5-vm-size-as-head-node-for-hdinsight-40"></a>Deaktivieren der Stardard_A5 VM-Größe als Hauptknoten für HDInsight 4.0
Der Hauptknoten des HDInsight-Clusters ist für die Initialisierung und Verwaltung des Clusters verantwortlich. Die Standard_A5 VM-Größe hat Zuverlässigkeitsprobleme als Hauptknoten für HDInsight 4.0. Ab dem nächsten Release können Kunden keine neuen Cluster mit einer Standard_A5 VM-Größe als Hauptknoten erstellen. Sie können andere VMs mirv 2 Kernen wie E2_v3 oder E2s_v3 verwenden. Vorhandene Cluster werden unverändert ausgeführt. Um die Hochverfügbarkeit und Zuverlässigkeit von HDInsight-Produktionsclustern zu gewährleisten, wird für den Hauptknoten ein virtueller Computer mit vier Kernen dringend empfohlen.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>Netzwerkschnittstellen-Ressource nicht sichtbar für Cluster, die in Azure-VM-Skalierungsgruppen ausgeführt werden
HDInsight wird schrittweise zu Azure-VM-Skalierungsgruppen migriert. Netzwerkschnittstellen virtueller Computer sind für Kunden in Clustern, die Azure-VM-Skalierungsgruppen verwenden, nicht mehr sichtbar.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt.

### <a name="hdinsight-interactive-query-only-supports-schedule-based-autoscale"></a>HDInsight Interactive Query unterstützt nur die zeitplanbasierte Autoskalierung

Aufgrund der zunehmenden Reife und Vielfalt von Kundenszenarien haben wir einige Einschränkungen bei der lastbasierten Autoskalierung von Interactive Query (LLAP) festgestellt. Diese Einschränkungen werden durch die Art der LLAP-Abfragedynamik, Probleme bei der Genauigkeit der Vorhersage der zukünftigen Auslastung und Probleme bei der Neuverteilung von Aufgaben des LLAP-Schedulers verursacht. Aufgrund dieser Einschränkungen können Benutzer u. U. feststellen, dass Abfragen in LLAP-Clustern langsamer ausgeführt werden, wenn die Autoskalierung aktiviert ist. Die Auswirkungen auf die Leistung können die Kostenvorteile der Autoskalierung aufwiegen.

Ab Juli 2021 unterstützt die Interactive Query-Workload in HDInsight nur die zeitplanbasierte Autoskalierung. Sie können die Autoskalierung für neue Interactive Query Cluster nicht mehr aktivieren. Vorhandene aktive Cluster können weiterhin mit den oben beschriebenen bekannten Einschränkungen ausgeführt werden. 

Microsoft empfiehlt, zu einer zeitplanbasierten Autoskalierung für LLAP zu wechseln.  Sie können das aktuelle Nutzungsmuster des Clusters über das Grafana Hive-Dashboard analysieren. Weitere Informationen finden Sie unter [Automatische Skalierung von Azure HDInsight-Clustern](hdinsight-autoscale-clusters.md). 

### <a name="basic-support-for-hdinsight-36-starting-july-1-2021"></a>Basic-Support für HDInsight 3.6 ab dem 1. Juli 2021
Ab dem 1. Juli 2021 bietet Microsoft [Basic-Suppor](hdinsight-component-versioning.md#support-options-for-hdinsight-versions)t für bestimmte HDInsight 3.6-Clustertypen an. Der Basic-Support-Plan wird bis 3. April 2022 verfügbar sein. Sie werden automatisch ab dem 1. Juli 2021 im Basic-Support registriert. Es ist keine Aktion erforderlich, damit Sie sich anmelden können. In [unserer Dokumentation](hdinsight-36-component-versioning.md) finden Sie Informationen dazu, welche Clustertypen unter Basic-Support enthalten sind. 

Es wird nicht empfohlen, neue Lösungen in HDInsight 3.6 zu entwickeln, frieren Sie Änderungen an vorhandenen 3.6-Umgebungen ein. Es wird empfohlen, die [Cluster in HDInsight 4.0 zu migrieren](hdinsight-version-release.md#how-to-upgrade-to-hdinsight-40). Erfahren Sie mehr über die Neuerungen in [HDInsight 4.0](hdinsight-version-release.md#whats-new-in-hdinsight-40).

### <a name="vm-host-naming-will-be-changed-on-july-1-2021"></a>Die Benennung von VM-Hosts wird am 1. Juli 2021 geändert.
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Der Dienst wird schrittweise zu [Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/overview.md) migriert. Bei dieser Migration wird das FQDN-Namensformat für Clusterhostnamen geändert, sodass die Zahlen im Hostnamen nicht notwendigerweise fortlaufend sind. Weitere Informationen zum Ermitteln der FQDN-Namen für die einzelnen Knoten finden Sie unter [Suchen der Hostnamen von Clusterknoten](./find-host-name.md).

### <a name="move-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Der Dienst wird schrittweise zu [Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/overview.md) migriert. Der gesamte Prozess kann Monate dauern. Nachdem Ihre Regionen und Abonnements migriert wurden, werden neu erstellte HDInsight-Cluster ohne Kundenaktionen in VM-Skalierungsgruppen ausgeführt. Es wird kein Breaking Change erwartet.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](./hdinsight-component-versioning.md).
