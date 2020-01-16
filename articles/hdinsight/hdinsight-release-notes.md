---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 2abdae95e14ecc9dab673216a2c6aef652915988
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435410"
---
# <a name="release-notes"></a>Versionshinweise

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist einer der beliebtesten Dienste unter Enterprise-Kunden für die Open Source-Analyseframeworks Apache Hadoop und Apache Spark in Azure.

## <a name="release-date-12172019"></a>Veröffentlichungsdatum: 17.12.2019

Diese Version gilt für HDInsight 3.6 und 4.0.

> [!IMPORTANT]  
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie im Artikel zur [HDInsight-Versionsverwaltung](hdinsight-component-versioning.md).


## <a name="new-features"></a>Neue Funktionen

### <a name="service-tags"></a>Diensttags
Diensttags bieten vereinfachte Sicherheit für virtuelle Azure-Computer und virtuelle Azure-Netzwerke, indem Sie den Netzwerkzugriff auf die Azure-Dienste problemlos einschränken können. Sie können Diensttags in Ihren Regeln für Netzwerksicherheitsgruppen (NSGs) verwenden, um Datenverkehr für einen bestimmten Azure-Dienst global oder nach Azure-Region zuzulassen oder zu verweigern. Azure bietet die Wartung von IP-Adressen, die den einzelnen Tags zugrunde liegen. HDInsight-Diensttags für Netzwerksicherheitsgruppen (NSGs) sind Gruppen von IP-Adressen für Integritäts- und Verwaltungsdienste. Diese Gruppen reduzieren die Komplexität beim Erstellen von Sicherheitsregeln. HDInsight-Kunden können Diensttags über das Azure-Portal, PowerShell und die REST-API aktivieren. Weitere Informationen finden Sie unter [NSG-Diensttags (Netzwerksicherheitsgruppen) für Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-service-tags).

### <a name="custom-ambari-db"></a>Benutzerdefinierte Ambari-Datenbank
HDInsight ermöglicht nun die Verwendung Ihrer eigenen SQL-Datenbank für Apache Ambari. Sie können diese benutzerdefinierte Ambari-Datenbank über das Azure-Portal oder über eine Resource Manager-Vorlage konfigurieren.  Diese Funktion ermöglicht es Ihnen, die richtige SQL-Datenbank für Ihre Verarbeitungs- und Kapazitätsanforderungen auszuwählen. Sie können auch problemlos ein Upgrade durchführen, um den Anforderungen des Unternehmenswachstums gerecht zu werden. Weitere Informationen finden Sie unter [Einrichten von HDInsight-Clustern mit einer benutzerdefinierten Ambari-Datenbank](hdinsight-custom-ambari-db.md).

![Benutzerdefinierte Ambari-Datenbank](./media/hdinsight-release-notes/custom-ambari-db.png)

## <a name="deprecation"></a>Eingestellte Unterstützung
Keine eingestellte Unterstützung für diese Version. Informationen zur Vorbereitung auf anstehende Einstellungen finden Sie unter [Bevorstehende Änderungen](#upcoming-changes).

## <a name="behavior-changes"></a>Verhaltensänderungen
Keine Verhaltensänderungen für diese Version. Informationen zur Vorbereitung auf anstehende Verhaltensänderungen finden Sie unter [Bevorstehende Änderungen](#upcoming-changes).

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt. 

### <a name="transport-layer-security-tls-12-enforcement"></a>Erzwingung von Transport Layer Security (TLS) 1.2
Transport Layer Security (TLS) und Secure Sockets Layer (SSL) sind kryptografische Protokolle, die Kommunikationssicherheit über ein Computernetzwerk bereitstellen. Weitere Informationen finden Sie unter [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). Zwar werden TLS 1.2-Verbindungen an öffentlichen HTTPS-Endpunkten von Azure HDInsight-Clustern akzeptiert, doch wird TLS 1.1 weiterhin aus Gründen der Abwärtskompatibilität mit älteren Clients unterstützt.

Ab der nächsten Version sind Sie in der Lage, Ihre neuen HDInsight-Cluster so zu konfigurieren, dass nur TLS 1.2-Verbindungen akzeptiert werden. 

Später im Jahr, ab dem 30.06.2020, wird in Azure HDInsight die Verwendung von TLS 1.2 oder höheren Versionen für alle HTTPS-Verbindungen erzwungen. Es wird empfohlen sicherzustellen, dass alle Ihre Clients für die Verarbeitung von TLS 1.2 oder höheren Versionen geeignet sind.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Ab Februar 2020 (das genaue Datum wird noch mitgeteilt) verwendet HDInsight stattdessen Azure-VM-Skalierungsgruppen. Weitere Informationen zu [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).

### <a name="esp-spark-cluster-node-size-change"></a>Änderung der Knotengröße für ESP Spark-Cluster 
In der nächsten Version:
- Die minimal zulässige Knotengröße für ESP Spark-Cluster wird in Standard_D13_V2 geändert. 
- Die Unterstützung von VMs der A-Serie für das Erstellen neuer ESP-Cluster wird eingestellt, da VMS der A-Serie aufgrund der relativ geringen CPU- und Speicherkapazität zu ESP-Clusterproblemen führen können.

### <a name="hbase-20-to-21"></a>HBase 2.0 zu 2.1
Im bevorstehenden Release von HDInsight 4.0 erfolgt ein Upgrade der HBase-Version von Version 2.0 auf 2.1.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Die Unterstützung von HDInsight 3.6 wurde bis zum 31. Dezember 2020 verlängert. Weitere Informationen finden Sie unter [Unterstützte HDInsight-Versionen](hdinsight-component-versioning.md#supported-hdinsight-versions).

Keine Änderung der Komponentenversion für HDInsight 4.0.

Apache Zeppelin in HDInsight 3.6: 0.7.0-->0.7.3. 

Die aktuellsten Komponentenversionen finden Sie [diesem Dokument](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions).

## <a name="new-regions"></a>Neue Regionen

### <a name="uae-north"></a>Vereinigte Arabische Emirate, Norden
Die Verwaltungs-IP-Adressen von „Vereinigte Arabische Emirate, Norden“ sind `65.52.252.96` und `65.52.252.97`.
