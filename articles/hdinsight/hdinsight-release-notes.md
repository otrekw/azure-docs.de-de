---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: 88e2161cfddf95f7f250b8b76c067d045f1529da
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092233"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

Wenn Sie die Anmerkungen zu dieser Version abonnieren möchten, sehen Sie sich die Releases auf [diesem GitHub-Repository](https://github.com/hdinsight/release-notes/releases) an.

## <a name="release-date-11182020"></a>Veröffentlichungsdatum: 11/18/2020

Diese Version gilt sowohl für HDInsight 3.6 als auch für HDInsight 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="auto-key-rotation-for-customer-managed-key-encryption-at-rest"></a>Automatische Schlüsselrotation für kundenseitig verwaltete Schlüsselverschlüsselung ruhender Daten
Ab dieser Version können Kunden die versionslosen Verschlüsselungsschlüssel-URLs von Azure Key Vault für die Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln verwenden. HDInsight rotiert die Schlüssel automatisch, wenn sie ablaufen oder durch neue Versionen ersetzt werden. Ausführlichere Informationen finden Sie [hier](./disk-encryption.md).

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Möglichkeit zum Auswählen anderer Größen virtueller Zookeeper-Computer für Spark, Hadoop und ML Services
HDInsight unterstützte bisher nicht die Anpassung der Zookeeper-Knotengröße für die Clustertypen „Spark“, „Hadoop“ und „ML Services“. Standardmäßig wird A2_v2/A2 für die Größen virtueller Computer verwendet, die kostenlos zur Verfügung gestellt werden. Ab dieser Version können Sie die Größe der virtuellen Zookeeper-Computer auswählen, die für Ihr Szenario am besten geeignet ist. Für Zookeeper-Knoten mit einer anderen Größe der virtuellen Computer als A2_v2/A2 fallen Gebühren an. Die virtuellen Computer vom Typ A2_v2 und A2 werden weiterhin kostenlos angeboten.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Ab dieser Version wird der Dienst schrittweise zu [Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/overview.md) migriert. Der gesamte Prozess kann Monate dauern. Nachdem Ihre Regionen und Abonnements migriert wurden, werden neu erstellte HDInsight-Cluster ohne Kundenaktionen in VM-Skalierungsgruppen ausgeführt. Es wird kein Breaking Change erwartet.

## <a name="deprecation"></a>Eingestellte Unterstützung
### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>Eingestellte Unterstützung des HDInsight 3.6 ML Services-Clusters
Der Support für den Clustertyp „HDInsight 3.6 ML Services“ wird am 31. Dezember 2020 auslaufen. Kunden können ab 31. Dezember 2020 keine neuen 3.6 ML Services-Cluster mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Prüfen Sie [hier](./hdinsight-component-versioning.md#available-versions) das Auslaufen des Supports für HDInsight-Versionen und Clustertypen.

### <a name="disabled-vm-sizes"></a>Deaktivierte VM-Größen
Ab dem 16. November 2020 hindert HDInsight neue Kunden daran, Cluster mit den VM-Größen standand_A8, standand_A9, standand_A10 und standand_A11 zu erstellen. Bestandskunden, die diese VM-Größen in den letzten drei Monaten verwendet haben, sind nicht betroffen. Ab dem 9. Januar 2021 hindert HDInsight alle Kunden daran, Cluster mit den VM-Größen standand_A8, standand_A9, standand_A10 und standand_A11 zu erstellen. Vorhandene Cluster werden unverändert ausgeführt. Sie sollten zu HDInsight 4.0 wechseln, um potenzielle System-/Supportunterbrechungen zu vermeiden.

## <a name="behavior-changes"></a>Verhaltensänderungen
### <a name="add-nsg-rule-checking-before-scaling-operation"></a>Hinzufügen der NSG-Regelüberprüfung vor dem Skalierungsvorgang
HDInsight hat die Überprüfung von Netzwerksicherheitsgruppen (NSGs) und benutzerdefinierten Routen (UDRs, User-Defined Routes) mit dem Skalierungsvorgang hinzugefügt. Die gleiche Überprüfung erfolgt neben der Clustererstellung auch für die Clusterskalierung. Diese Überprüfung hilft, unvorhersehbare Fehler zu vermeiden. Wenn die Überprüfung nicht bestanden wird, tritt bei der Skalierung ein Fehler auf. Weitere Informationen zum ordnungsgemäßen Konfigurieren von NSGs und UDRs finden Sie unter [HDInsight-Verwaltungs-IP-Adressen](./hdinsight-management-ip-addresses.md).

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt.

### <a name="breaking-change-for-net-for-apache-spark-100"></a>Breaking Change für .NET für Apache Spark 1.0.0
HDInsight führt die erste offizielle Hauptversion von .NET für Apache Spark in der nächsten Version ein. Sie stellt neben anderen Features DataFrame-API-Vollständigkeit für Spark 2.4.x und Spark 3.0.x bereit. Es gibt Breaking Changes für diese Hauptversion. Informationen zu den Schritten zum Aktualisieren von Code und Pipelines finden Sie in [diesem Migrationsleitfaden](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10). [Hier](https://docs.microsoft.com/azure/hdinsight/spark/spark-dotnet-version-update#using-net-for-apache-spark-v10-in-hdinsight)erhalten Sie weitere Informationen.

### <a name="default-cluster-vm-size-will-be-changed-to-ev3-family"></a>Standardgrößen für Cluster-VMs werden in Ev3-Familie geändert
Ab dem nächsten Release (ca. Ende Januar), ändern sich die Standardgrößen für Cluster-VMs von der D-Familie zur Ev3-Familie. Diese Änderung gilt für Hauptknoten und Workerknoten. Um diese Änderung zu verhindern, geben Sie die VM-Größen, die Sie verwenden möchten, in der ARM-Vorlage an.

### <a name="default-cluster-version-will-be-changed-to-40"></a>Standardclusterversion wird in 4.0 geändert
Ab Februar 2021 wird die Standardversion des HDInsight-Clusters von 3.6 in 4.0 geändert. Weitere Informationen zu verfügbaren Versionen finden Sie unter [Verfügbare Versionen](./hdinsight-component-versioning.md#available-versions). Erfahren Sie mehr über die Neuerungen in [HDInsight 4.0](./hdinsight-version-release.md).

### <a name="os-version-upgrade"></a>Upgrade der Betriebssystemversion
Für HDInsight ein Upgrade der Betriebssystemversion von 16.04 auf 18.04 ausgeführt. Das Upgrade wird vor April 2021 abgeschlossen sein.

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>Ende der Unterstützung von HDInsight 3.6 am 30. Juni 2021
Die Unterstützung von HDInsight 3.6 endet bald. Ab dem 30. Juni 2021 können Kunden keine neuen HDInsight 3.6-Cluster mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Sie sollten zu HDInsight 4.0 wechseln, um potenzielle System-/Supportunterbrechungen zu vermeiden.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für dieses Release gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](./hdinsight-component-versioning.md).

## <a name="known-issues"></a>Bekannte Probleme
### <a name="prevent-hdinsight-cluster-vms-from-rebooting-periodically"></a>Verhindern, dass HDInsight-Cluster-VMs regelmäßig neu gestartet werden

Es ist Ihnen möglicherweise aufgefallen, dass seit Mitte November 2020 HDInsight-Cluster-VMs in regelmäßigen Abständen neu gestartet werden. Dies könnte folgende Ursachen haben:

1.  ClamAV ist in Ihrem Cluster aktiviert. Das neue azsec-clamav-Paket beansprucht sehr viel Arbeitsspeicher, wodurch ein Neustart des Knotens ausgelöst wird. 
2.  Es wurde ein täglicher Cronjob geplant, der Änderungen an der von Azure-Diensten verwendeten Liste der Zertifizierungsstellen überwacht. Wenn ein neues Zertifizierungsstellenzertifikat verfügbar ist, fügt das Skript das Zertifikat dem JDK-Vertrauensspeicher hinzu und plant einen Neustart.

HDInsight stellt für beide Probleme Fixes bereit und wendet Patches auf alle laufenden Cluster an. Wenn Sie den Fix sofort anwenden und unerwartete VM-Neustarts vermeiden möchten, können Sie die Skriptaktionen unten auf allen Clusterknoten als persistente Skriptaktion ausführen. HDInsight sendet einen weiteren Hinweis, nachdem der Fix und der Patch angewandt wurden.
```
https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/replace_cacert_script.sh
https://healingscriptssa.blob.core.windows.net/healingscripts/ChangeOOMPolicyAndApplyLatestConfigForClamav.sh
```