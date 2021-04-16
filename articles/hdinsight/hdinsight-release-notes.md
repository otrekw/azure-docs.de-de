---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
ms.custom: references_regions
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/23/2021
ms.openlocfilehash: a648ff3aa0c042aaefe16eaae0f9d73953241b3d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106065496"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

Wenn Sie die Anmerkungen zu dieser Version abonnieren möchten, sehen Sie sich die Releases auf [diesem GitHub-Repository](https://github.com/hdinsight/release-notes/releases) an.

## <a name="release-date-03242021"></a>Veröffentlichungsdatum: 24.03.2021

Diese Version gilt sowohl für HDInsight 3.6 als auch für HDInsight 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="spark-30-preview"></a>Spark 3.0 (Vorschau)
HDInsight hat [Spark 3.0.0](https://spark.apache.org/docs/3.0.0/)-Support zu HDInsight 4.0 als Previewfunktion hinzugefügt. 

### <a name="kafka-24-preview"></a>Kafka 2.4 (Vorschau)
HDInsight hat [Kafka 2.4.1](http://kafka.apache.org/24/documentation.html)-Support zu HDInsight 4.0 als Previewfunktion hinzugefügt.

### <a name="eav4-series-support"></a>Support der Eav4-Serie
In HDInsight wurde in diesem Release Support für die Eav4-Serie hinzugefügt. Weitere Informationen zur [Dav4-Serie finden Sie hier](../virtual-machines/eav4-easv4-series.md). Die Serie wurde in den folgenden Regionen zur Verfügung gestellt: 

* Australien (Osten)
* Brasilien Süd
* USA (Mitte)
* Asien, Osten
* East US
* Japan, Osten
* Asien, Südosten
* UK, Süden
* Europa, Westen
* USA, Westen 2

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Der Dienst wird schrittweise zu [Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/overview.md) migriert. Der gesamte Prozess kann Monate dauern. Nachdem Ihre Regionen und Abonnements migriert wurden, werden neu erstellte HDInsight-Cluster ohne Kundenaktionen in VM-Skalierungsgruppen ausgeführt. Es wird kein Breaking Change erwartet.

## <a name="deprecation"></a>Eingestellte Unterstützung
Keine eingestellte Unterstützung für dieses Release.

## <a name="behavior-changes"></a>Verhaltensänderungen
### <a name="default-cluster-version-is-changed-to-40"></a>Standardclusterversion wird auf 4.0 geändert
Ab Februar wird die Standardversion des HDInsight-Clusters von 3.6 auf 4.0 geändert. Weitere Informationen zu verfügbaren Versionen finden Sie unter [Verfügbare Versionen](./hdinsight-component-versioning.md). Erfahren Sie mehr über die Neuerungen in [HDInsight 4.0](./hdinsight-version-release.md).

### <a name="default-cluster-vm-sizes-are-changed-to-ev3-series"></a>Änderung der VM-Standardgröße für Cluster in Ev3-Serie 
Die VM-Standardgröße für Cluster wird von der D-Serie in die Ev3-Serie geändert. Diese Änderung gilt für Hauptknoten und Workerknoten. Um zu vermeiden, dass sich diese Änderung auf Ihre getesteten Workflows auswirkt, geben Sie die VM-Größen an, die Sie in der ARM-Vorlage verwenden möchten.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>Netzwerkschnittstellen-Ressource nicht sichtbar für Cluster, die in Azure-VM-Skalierungsgruppen ausgeführt werden
HDInsight wird schrittweise zu Azure-VM-Skalierungsgruppen migriert. Netzwerkschnittstellen virtueller Computer sind für Kunden in Clustern, die Azure-VM-Skalierungsgruppen verwenden, nicht mehr sichtbar.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt.

### <a name="os-version-upgrade"></a>Upgrade der Betriebssystemversion
Für HDInsight erfolgt ein Upgrade der Betriebssystemversion von Ubuntu 16.04 auf 18.04. Das Upgrade wird vor April 2021 abgeschlossen sein.

### <a name="basic-support-for-hdinsight-36-starting-july-1-2021"></a>Basic-Support für HDInsight 3.6 ab dem 1. Juli 2021
Ab dem 1. Juli 2021 bietet Microsoft [Basic-Suppor](hdinsight-component-versioning.md#support-options-for-hdinsight-versions)t für bestimmte HDInsight 3.6-Clustertypen an. Der Basic-Support-Plan wird bis 3. April 2022 verfügbar sein. Sie werden automatisch ab dem 1. Juli 2021 im Basic-Support registriert. Es ist keine Aktion erforderlich, damit Sie sich anmelden können. In [unserer Dokumentation](hdinsight-36-component-versioning.md) finden Sie Informationen dazu, welche Clustertypen unter Basic-Support enthalten sind. 

Es wird nicht empfohlen, neue Lösungen in HDInsight 3.6 zu entwickeln, frieren Sie Änderungen an vorhandenen 3.6-Umgebungen ein. Es wird empfohlen, die [Cluster in HDInsight 4.0 zu migrieren](hdinsight-version-release.md#how-to-upgrade-to-hdinsight-40). Erfahren Sie mehr über die Neuerungen in [HDInsight 4.0](hdinsight-version-release.md#whats-new-in-hdinsight-40).

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Zusätzlicher Support für Spark 3.0.0 und Kafka 2.4.1 als Preview hinzugefügt. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](./hdinsight-component-versioning.md).

## <a name="recommanded-features"></a>Empfohlene Features
### <a name="service-tags"></a>Diensttags
Diensttags vereinfachen das Einschränken des Netzwerkzugriffs auf die Azure-Dienste für virtuelle Azure-Computer und virtuelle Azure-Netzwerke. Sie können Diensttags in Ihren Regeln für Netzwerksicherheitsgruppen (NSGs) verwenden, um Datenverkehr für einen bestimmten Azure-Dienst global oder nach Azure-Region zuzulassen oder zu verweigern. Die Regel kann global oder pro Azure-Region festgelegt werden. Azure bietet die Wartung von IP-Adressen, die den einzelnen Tags zugrunde liegen. HDInsight-Diensttags für Netzwerksicherheitsgruppen (NSGs) sind Gruppen von IP-Adressen für Integritäts- und Verwaltungsdienste. Diese Gruppen reduzieren die Komplexität beim Erstellen von Sicherheitsregeln. HDInsight-Kunden können Diensttags über das Azure-Portal, PowerShell und die REST-API aktivieren. Weitere Informationen finden Sie unter [NSG-Diensttags (Netzwerksicherheitsgruppen) für Azure HDInsight](./hdinsight-service-tags.md).
