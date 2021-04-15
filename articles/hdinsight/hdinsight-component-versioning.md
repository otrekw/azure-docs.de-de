---
title: Apache Hadoop-Komponenten und -Versionen – Azure HDInsight
description: Lernen Sie die Apache Hadoop-Komponenten und -Versionen in Azure HDInsight kennen.
ms.service: hdinsight
ms.topic: conceptual
author: deshriva
ms.author: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: dbd5b507fd4a7b2434158dbdc80584a7fd348732
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105726582"
---
# <a name="azure-hdinsight-versions"></a>Versionen von Azure HDInsight

HDInsight bündelt Apache Hadoop-Umgebungskomponenten und die HDInsight-Platform in einem Paket, das in einem Cluster bereitgestellt wird. Weitere Informationen finden Sie unter [How versioning works in HDInsight](hdinsight-overview-versioning.md) (Funktionsweise der Versionsverwaltung in HDInsight, in englischer Sprache).

## <a name="supported-hdinsight-versions"></a>Unterstützte HDInsight-Versionen

Die Tabelle enthält die im Azure-Portal und über andere Bereitstellungsmethoden, z. B. PowerShell, die CLI und das .NET SDK, verfügbaren Versionen von HDInsight.

| HDInsight-Version | Betriebssystem des virtuellen Computers | Veröffentlichungsdatum| Supporttyp | Supportablaufdatum | Deaktivierungstermin | Hochverfügbarkeit |
| --- | --- | --- | --- | --- | --- | ---|
| [HDInsight 4.0](hdinsight-40-component-versioning.md) |Ubuntu 16.0.4 LTS |24. September 2018 | [Standard](hdinsight-component-versioning.md#support-options-for-hdinsight-versions) | | |Ja |
| [HDInsight 3.6](hdinsight-36-component-versioning.md) |Ubuntu 16.0.4 LTS |4\. April 2017      | [Grundlegend](hdinsight-component-versioning.md#support-options-for-hdinsight-versions) | Ablaufdatum für Standard-Support: 30. Juni 2021 <br> Ablaufdatum für Basic-Support: 3. April 2022 |4\. April 2022 |Ja |

\* Ab dem 1. Juli 2021 bietet Microsoft Basic-Support für bestimmte HDI 3.6-Clustertypen an. Siehe [HDInsight 3.6 component versions](hdinsight-36-component-versioning.md) (HDInsight 3.6-Komponentenversionen, in englischer Sprache)

## <a name="release-notes"></a>Versionshinweise

In den [HDInsight-Versionshinweisen](hdinsight-release-notes.md) finden Sie zusätzliche Anmerkungen zu den aktuellen Versionen von HDInsight.

## <a name="support-options-for-hdinsight-versions"></a>Supportoptionen für HDInsight-Versionen

Support ist als ein Zeitraum definiert, in dem eine HDInsight-Version vom Microsoft-Kundendienst und -Support unterstützt wird. HDInsight bietet zwei Arten von Support: 
- **Standard-Support** ist ein Zeitraum, in dem Microsoft Updates und Unterstützung für HDInsight-Cluster bereitstellt.  
    Es wird empfohlen, Lösungen mit der neuesten, vollständig unterstützten Version zu entwickeln. 
- **Basic-Support** ist ein Zeitraum, in dem Microsoft eingeschränkte Wartungsfunktionen für den HDInsight-Ressourcenanbieter bereitstellt. HDInsight-Images und OSS-Komponenten (Open-Source-Software) werden nicht gewartet.   Nur wichtige Sicherheitsfixes werden in HDInsight-Clustern gepatcht.  
  Microsoft rät davon ab, neue Cluster zu erstellen oder neue Lösungen zu entwickeln, wenn für eine Version Basic-Support besteht. Es wird empfohlen, vorhandene Cluster zur neuesten, vollständig unterstützten Version zu migrieren. 

**Supportablauf** bedeutet, dass Microsoft für die jeweilige HDInsight-Version keinen Support mehr bereitstellt. Die Version ist zudem möglicherweise nicht mehr über das Azure-Portal für die Clustererstellung verfügbar.

**Deaktivierung** bedeutet, dass vorhandene Cluster einer HDInsight-Version weiterhin unverändert ausgeführt werden. Neue Cluster dieser Version können jedoch nicht erstellt werden – auch nicht mithilfe der CLI und SDKs. Auch andere Funktionen auf Steuerungsebene, z. B. die manuelle und automatische Skalierung, können nach dem Datum der Deaktivierung möglicherweise nicht mehr verwendet werden. Für deaktivierte Versionen ist kein Support verfügbar.

## <a name="versioning-considerations"></a>Überlegungen zur Versionsverwaltung
- Nachdem ein Cluster mit einem Image bereitgestellt wurde, wird dieser Cluster nicht automatisch auf eine neuere Imageversion aktualisiert. Beim Erstellen neuer Cluster wird die aktuelle Imageversion bereitgestellt.
- Kunden sollten überprüfen, ob Anwendungen bei Verwendung einer neuen HDInsight-Version ordnungsgemäß ausgeführt werden.
- HDInsight behält sich das Recht vor, die Standardversion ohne vorherige Ankündigung zu ändern. Wenn eine Versionsabhängigkeit vorliegt, geben Sie beim Erstellen der Cluster die HDInsight-Version an.
- HDInsight deaktiviert möglicherweise eine OSS-Komponentenversion vor dem Deaktivieren der HDInsight-Version.

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten von Clustern in HDInsight mit Apache Hadoop, Spark, Kafka usw.](hdinsight-hadoop-provision-linux-clusters.md)
- [Enterprise-Sicherheitspaket](./enterprise-security-package.md)
- [Verwenden eines Windows-Computers mit Apache Hadoop in HDInsight](hdinsight-hadoop-windows-tools.md)
