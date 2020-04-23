---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: f8331b64792781a89428f8c5070375c31c0da48d
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084888"
---
# <a name="release-notes"></a>Versionshinweise

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist unter Enterprisekunden einer der beliebtesten Dienste für Open-Source-Analysen in Azure.

## <a name="release-date-01092020"></a>Veröffentlichungsdatum: 09.01.2020

Diese Version gilt für HDInsight 3.6 und 4.0. Das HDInsight-Release wird über mehrere Tage für alle Regionen verfügbar gemacht. Das hier angegebene Veröffentlichungsdatum entspricht dem Veröffentlichungsdatum in der ersten Region. Wenn die unten aufgeführten Änderungen nicht angezeigt werden, warten Sie, bis das Release in einigen Tagen in Ihrer Region verfügbar ist.

> [!IMPORTANT]  
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie im Artikel zur [HDInsight-Versionsverwaltung](hdinsight-component-versioning.md).

## <a name="new-features"></a>Neue Funktionen
### <a name="tls-12-enforcement"></a>Erzwingen von TLS 1.2
Transport Layer Security (TLS) und Secure Sockets Layer (SSL) sind kryptografische Protokolle, die Kommunikationssicherheit über ein Computernetzwerk bereitstellen. Erfahren Sie mehr über [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). HDInsight verwendet auf öffentlichen HTTPS-Endpunkten TLS 1.2, TLS 1.1 wird jedoch aus Gründen der Abwärtskompatibilität weiterhin unterstützt. 

Mit diesem Release können Kunden nur TLS 1.2 für alle Verbindungen über den öffentlichen Clusterendpunkt auswählen. Zur Unterstützung dieser Möglichkeit wird die neue **minSupportedTlsVersion**-Eigenschaft eingeführt, die bei der Clustererstellung angegeben werden kann. Wenn die-Eigenschaft nicht festgelegt ist, unterstützt der Cluster weiterhin TLS 1.0, 1.1 und 1.2. Dies entspricht dem derzeitigen Verhalten. Kunden können den Wert für diese Eigenschaft auf „1.2“ festlegen, damit der Cluster nur TLS 1.2 und höher unterstützt. Weitere Informationen finden Sie unter [Transport Layer Security](./transport-layer-security.md).

### <a name="bring-your-own-key-for-disk-encryption"></a>Bring Your Own Key für Datenträgerverschlüsselung
Alle verwalteten Datenträger in HDInsight werden mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) von Azure geschützt. Die Daten auf diesen Datenträgern werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Ab diesem Release können Sie Bring Your Own Key (BYOK) für die Datenträgerverschlüsselung auswählen und mithilfe von Azure Key Vault verwalten. Die BYOK-Verschlüsselung ist eine Konfiguration in einem Schritt bei der Clustererstellung ohne zusätzliche Kosten. Registrieren Sie beim Erstellen Ihres Clusters lediglich HDInsight als verwaltete Identität bei Azure Key Vault, und fügen Sie den Verschlüsselungsschlüssel hinzu. Weitere Informationen finden Sie unter [Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln](https://docs.microsoft.com/azure/hdinsight/disk-encryption).

## <a name="deprecation"></a>Eingestellte Unterstützung
Keine eingestellte Unterstützung für diese Version. Informationen zur Vorbereitung auf anstehende Einstellungen finden Sie unter [Bevorstehende Änderungen](#upcoming-changes).

## <a name="behavior-changes"></a>Verhaltensänderungen
Keine Verhaltensänderungen für diese Version. Informationen zur Vorbereitung auf anstehende Änderungen finden Sie unter [Bevorstehende Änderungen](#upcoming-changes).

## <a name="upcoming-changes"></a>Bevorstehende Änderungen
Die folgenden Änderungen werden in kommenden Versionen durchgeführt. 

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Virtueller Computer mit mindestens vier Kernen erforderlich für den Hauptknoten 
Zum Sicherstellen der Hochverfügbarkeit und Zuverlässigkeit von HDInsight-Clustern ist ein virtueller Computer mit mindestens vier Kernen für den Hauptknoten erforderlich. Ab dem 6. April 2020 können Kunden nur noch VMs mit mindestens vier Kernen als Hauptknoten für einen neuen HDInsight-Cluster auswählen. Vorhandene Cluster funktionieren weiterhin erwartungsgemäß. 

### <a name="esp-spark-cluster-node-size-change"></a>Änderung der Knotengröße für ESP Spark-Cluster 
Im anstehenden Release wird die minimal zulässige Knotengröße für ESP Spark-Cluster in Standard_D13_V2 geändert. VMs der A-Serie können aufgrund der relativ geringen CPU- und Speicherkapazität zu ESP-Clusterproblemen führen. Die Unterstützung von VMs der A-Serie zum Erstellen von neuen ESP-Clustern wird eingestellt.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Wechsel zu Azure-VM-Skalierungsgruppen
HDInsight verwendet jetzt virtuelle Azure-Computer für die Bereitstellung des Clusters. Im anstehenden Release verwendet HDInsight stattdessen Azure-VM-Skalierungsgruppen. Informieren Sie sich ausführlicher über Azure-VM-Skalierungsgruppen.

### <a name="hbase-20-to-21"></a>HBase 2.0 zu 2.1
Im bevorstehenden Release von HDInsight 4.0 erfolgt ein Upgrade der HBase-Version von Version 2.0 auf 2.1.

## <a name="bug-fixes"></a>Behebung von Programmfehlern
HDInsight sorgt weiterhin für Verbesserungen bei der Clusterzuverlässigkeit und -leistung. 

## <a name="component-version-change"></a>Änderung der Komponentenversion
Für dieses Release gibt es keine Änderung der Komponentenversion. Die aktuellen Komponentenversionen für HDInsight 4.0 und HDInsight 3.6 finden Sie hier.

