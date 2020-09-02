---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 0587a179b98a410cdba46b7817d86567f275f25d
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88826819"
---
# <a name="azure-hdinsight-release-notes"></a>Versionshinweise zu Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

## <a name="release-date-08092020"></a>Veröffentlichungsdatum: 09.08.2020

Diese Version gilt nur für HDInsight 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Es kann sein, dass die unten angegebenen Änderungen bei Ihnen erst ein paar Tage später verfügbar werden.

## <a name="new-features"></a>Neue Funktionen
### <a name="support-for-sparkcruise"></a>Unterstützung für SparkCruise
SparkCruise ist ein System für die automatische Wiederverwendung von Berechnungsergebnissen für Spark. Es wählt gängige zu materialisierende Teilausdrücke anhand der vergangenen Abfrageworkload aus. SparkCruise materialisiert diese Teilausdrücke im Rahmen der Abfrageverarbeitung, und die Wiederverwendung von Berechnungsergebnissen wird automatisch im Hintergrund angewendet. Sie können von SparkCruise profitieren, ohne Änderungen am Spark-Code vornehmen zu müssen.
 
### <a name="support-hive-view-for-hdinsight-40"></a>Unterstützung der Hive-Ansicht für HDInsight 4.0
Die Apache Ambari-Hive-Ansicht ist dazu konzipiert, Sie beim Erstellen, Optimieren und Ausführen von Hive-Abfragen über Ihren Webbrowser zu unterstützen. Die Hive-Ansicht wird ab diesem Release nativ für HDInsight 4.0-Cluster unterstützt. Dies gilt nicht für vorhandene Cluster. Sie müssen den Cluster löschen und neu erstellen, um die integrierte Hive-Ansicht zu nutzen.
 
### <a name="support-tez-view-for-hdinsight-40"></a>Unterstützung der Tez-Ansicht für HDInsight 4.0
Die Apache Tez-Ansicht wird zum Nachverfolgen und Debuggen der Ausführung von Hive Tez-Aufträgen verwendet. Die Tez-Ansicht wird ab diesem Release nativ für HDInsight 4.0 unterstützt. Dies gilt nicht für vorhandene Cluster. Sie müssen den Cluster löschen und neu erstellen, um die integrierte Tez-Ansicht zu nutzen.

## <a name="deprecation"></a>Eingestellte Unterstützung
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>Einstellung der Unterstützung von Spark 2.1 und 2.2 für Spark-Cluster in HDInsight 3.6
Ab dem 1. Juli 2020 können Kunden in HDInsight 3.6 keine neuen Spark-Cluster mit Spark 2.1 und 2.2 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Ziehen Sie in Erwägung, bis 30. Juni 2020 in HDInsight 3.6 zu Spark 2.3 zu wechseln, um eine potenzielle Unterbrechung von System/Support zu vermeiden.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>Einstellung der Unterstützung von Spark 2.3 für Spark-Cluster in HDInsight 4.0
Ab dem 1. Juli 2020 können Kunden in HDInsight 4.0 keine neuen Spark-Cluster mit Spark 2.3 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Ziehen Sie in Erwägung, bis 30. Juni 2020 in HDInsight 4.0 zu Spark 2.4 zu wechseln, um eine potenzielle Unterbrechung von System/Support zu vermeiden.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>Einstellung von Kafka 1.1 für Kafka-Cluster in HDInsight 4.0
Ab dem 1. Juli 2020 können Kunden in HDInsight 4.0 keine neuen Kafka-Cluster mit Kafka 1.1 mehr erstellen. Vorhandene Cluster werden unverändert ohne Unterstützung durch Microsoft ausgeführt. Es empfiehlt sich, in HDInsight 4.0 bis zum 30. Juni 2020 auf Kafka 2.1 umzustellen, um potenzielle System-/Supportunterbrechungen zu vermeiden.

## <a name="behavior-changes"></a>Verhaltensänderungen
### <a name="ambari-stack-version-change"></a>Änderung der Ambari-Stapelversion
Mit diesem Release wurde die Ambari-Version von 2.x.x.x in 4.1 geändert. Sie können die Ambari-Version in der Ambari-Benutzeroberfläche über „About“ (Info) abrufen.

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Es stehen keine Breaking Changes an, um die Sie sich kümmern müssen.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

Für die folgenden JIRA-Tickets wurden Backports für Hive durchgeführt:
* [HIVE-23619](https://issues.apache.org/jira/browse/HIVE-23619)
* [HIVE-21223](https://issues.apache.org/jira/browse/HIVE-21223)
* [HIVE-22599](https://issues.apache.org/jira/browse/HIVE-22599)
* [HIVE-22121](https://issues.apache.org/jira/browse/HIVE-22121)
* [HIVE-22136](https://issues.apache.org/jira/browse/HIVE-22136)
* [HIVE-18786](https://issues.apache.org/jira/browse/HIVE-18786)

Für die folgenden JIRA-Tickets wurden Backports für HBase durchgeführt:
* [HBASE-21458](https://issues.apache.org/jira/browse/HBASE-21458)
* [HBASE-24208](https://issues.apache.org/jira/browse/HBASE-24208)
* [HBASE-24205](https://issues.apache.org/jira/browse/HBASE-24205)

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für dieses Release gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).

## <a name="known-issues"></a>Bekannte Probleme

Es wurde ein Problem im Azure-Portal behoben, bei dem ein Fehler auftrat, wenn Benutzer einen Azure HDInsight-Cluster mit einem öffentlichen Schlüssel als SSH-Authentifizierungstyp erstellten. Wenn Benutzer auf **Überprüfen + erstellen** klickten, erhielten sie die Fehlermeldung „Es dürfen keine drei aufeinanderfolgenden Zeichen aus dem SSH-Benutzernamen enthalten sein“. Dieses Problem wurde behoben, aber es kann erforderlich sein, den Browsercache zu aktualisieren, indem Sie STRG+F5 drücken, um die korrigierte Ansicht zu laden. Dieses Problem konnten Sie umgehen, indem Sie einen Cluster mit einer ARM-Vorlage erstellten. 
