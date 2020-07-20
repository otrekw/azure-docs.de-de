---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/11/2020
ms.openlocfilehash: a4faab9ac6d5e1c39c1120e09dae792b95892d60
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564399"
---
# <a name="release-notes"></a>Versionshinweise

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

## <a name="release-date-06112020"></a>Veröffentlichungsdatum: 11.06.2020

Diese Version gilt für HDInsight 3.6 und 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
Von HDInsight werden jetzt virtuelle Azure-Computer für die Clusterbereitstellung verwendet. Ab diesem Release werden von neu erstellten HDInsight-Clustern Azure-VM-Skalierungsgruppen verwendet. Die Änderung wird schrittweise eingeführt. Dabei ist nicht mit einem Breaking Change zu rechnen. Weitere Informationen zu [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).
 
### <a name="reboot-vms-in-hdinsight-cluster"></a>Neustarten von virtuellen Computern im HDInsight-Cluster
In dieser Version wird das Neustarten von virtuellen Computern in einem HDInsight-Cluster unterstützt, um nicht reagierende Knoten neu starten zu können. Momentan ist dies nur per API möglich, an der PowerShell- und CLI-Unterstützung wird jedoch bereits gearbeitet. Weitere Informationen zur API finden Sie in [dieser Dokumentation](https://github.com/Azure/azure-rest-api-specs/codeowners/master/specification/hdinsight/resource-manager/Microsoft.HDInsight/stable/2018-06-01-preview/virtualMachines.json).
 
## <a name="deprecation"></a>Eingestellte Unterstützung
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>Einstellung der Unterstützung von Spark 2.1 und 2.2 für Spark-Cluster in HDInsight 3.6
Ab dem 1. Juli 2020 können Kunden in HDInsight 3.6 keine neuen Spark-Cluster mit Spark 2.1 und 2.2 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Ziehen Sie in Erwägung, bis 30. Juni 2020 in HDInsight 3.6 zu Spark 2.3 zu wechseln, um eine potenzielle Unterbrechung von System/Support zu vermeiden.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>Einstellung der Unterstützung von Spark 2.3 für Spark-Cluster in HDInsight 4.0
Ab dem 1. Juli 2020 können Kunden in HDInsight 4.0 keine neuen Spark-Cluster mit Spark 2.3 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Ziehen Sie in Erwägung, bis 30. Juni 2020 in HDInsight 4.0 zu Spark 2.4 zu wechseln, um eine potenzielle Unterbrechung von System/Support zu vermeiden.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>Einstellung von Kafka 1.1 für Kafka-Cluster in HDInsight 4.0
Ab dem 1. Juli 2020 können Kunden in HDInsight 4.0 keine neuen Kafka-Cluster mit Kafka 1.1 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Es empfiehlt sich, in HDInsight 4.0 bis zum 30. Juni 2020 auf Kafka 2.1 umzustellen, um potenzielle System-/Supportunterbrechungen zu vermeiden.
 
## <a name="behavior-changes"></a>Verhaltensänderungen
### <a name="esp-spark-cluster-head-node-size-change"></a>Änderung der Hauptknotengröße für Spark-Cluster mit Enterprise-Sicherheitspaket (ESP) 
Die kleinstmögliche Hauptknotengröße für Spark-Cluster mit Enterprise-Sicherheitspaket wird in „Standard_D13_V2“ geändert. Virtuelle Computer mit einer geringen Anzahl von Kernen und wenig Arbeitsspeicher können aufgrund der relativ geringen CPU- und Speicherkapazität als Hauptknoten zu ESP-Clusterproblemen führen. Verwenden Sie als Hauptknoten für Spark-Cluster mit Enterprise-Sicherheitspaket ab dem Release SKUs, die über „Standard_D13_V2“ und „Standard_E16_V3“ liegen.
 
### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Virtueller Computer mit mindestens vier Kernen erforderlich für den Hauptknoten 
Um die Hochverfügbarkeit und Zuverlässigkeit von HDInsight-Clustern zu gewährleisten, wird für den Hauptknoten ein virtueller Computer mit mindestens vier Kernen benötigt. Ab dem 6. April 2020 können Kunden nur noch VMs mit mindestens vier Kernen als Hauptknoten für die neuen HDInsight-Cluster auswählen. Vorhandene Cluster funktionieren weiterhin erwartungsgemäß. 
 
### <a name="cluster-worker-node-provisioning-change"></a>Änderung bei der Bereitstellung von Workerknoten für Cluster
Wenn 80 Prozent der Workerknoten bereit sind, ist der Cluster **betriebsbereit**. Kunden können nun alle Vorgänge auf der Datenebene nutzen und beispielsweise Skripts und Aufträge ausführen. Vorgänge auf der Steuerungsebene (etwa das Hoch-/Herunterskalieren) stehen dagegen nicht zur Verfügung. Nur Löschvorgänge werden unterstützt.
 
Im Anschluss an die Phase **Betriebsbereit** wartet der Cluster noch 60 Minuten auf die restlichen 20 Prozent der Workerknoten. Nach diesen 60 Minuten wechselt der Cluster in die **Ausführungsphase**, auch wenn noch nicht alle Workerknoten verfügbar sind. Sobald sich ein Cluster in der **Ausführungsphase** befindet, können er ganz normal verwendet werden. Sowohl Vorgänge auf der Steuerungsebene (etwa das Hoch-/Herunterskalieren) als auch Vorgänge auf der Datenebene (beispielsweise das Ausführen von Skripts und Aufträgen) werden akzeptiert. Sollten einige der angeforderten Workerknoten nicht verfügbar sein, wird der Cluster als teilweise erfolgreich gekennzeichnet. Ihnen werden die Knoten in Rechnung gestellt, die erfolgreich bereitgestellt wurden. 
 
### <a name="create-new-service-principal-through-hdinsight"></a>Erstellen eines neuen Dienstprinzipals über HDInsight
Bislang konnten Kunden mit der Clustererstellung einen neuen Dienstprinzipal für den Zugriff auf das verbundene ADLS Gen 1-Konto im Azure-Portal erstellen. Ab dem 15. Juni 2020 können Kunden im Rahmen des HDInsight-Erstellungsworkflows keinen neuen Dienstprinzipal mehr erstellen, und es wird nur noch ein bereits vorhandener Dienstprinzipal unterstützt. Weitere Informationen finden Sie unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

### <a name="time-out-for-script-actions-with-cluster-creation"></a>Timeout für Skriptaktionen mit Clustererstellung
HDInsight unterstützt das Ausführen von Skriptaktionen mit Clustererstellung. Ab diesem Release müssen alle Skriptaktionen mit Clustererstellung innerhalb von **60 Minuten** abgeschlossen werden. Andernfalls tritt ein Timeout auf. An aktive Cluster übermittelte Skriptaktionen sind davon nicht betroffen. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#script-action-in-the-cluster-creation-process).
 
## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Es stehen keine Breaking Changes an, um die Sie sich kümmern müssen.
 
## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 
 
## <a name="component-version-change"></a>Änderung der Komponentenversion
### <a name="hbase-20-to-216"></a>HBase 2.0 auf 2.1.6
Die HBase-Version wird von 2.0 auf 2.1.6 aktualisiert.
 
### <a name="spark-240-to-244"></a>Spark 2.4.0 auf 2.4.4
Die Spark-Version wird von 2.4.0 auf 2.4.4 aktualisiert.
 
### <a name="kafka-210-to-211"></a>Kafka 2.1.0 auf 2.1.1
Die Kafka-Version wird von 2.1.0 auf 2.1.1 aktualisiert.
 
Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).

## <a name="known-issues"></a>Bekannte Probleme

### <a name="hive-warehouse-connector-issue"></a>Problem mit Hive Warehouse Connector
In diesem Release gibt es ein Problem mit Hive Warehouse Connector. Eine entsprechende Korrektur wird in das nächste Release aufgenommen. Cluster, die vor diesem Release erstellt wurden, werden dadurch nicht beeinträchtigt. Vermeiden Sie nach Möglichkeit eine Löschung und Neuerstellung des Clusters. Sollten Sie weitere Unterstützung benötigen, erstellen Sie ein Supportticket.
