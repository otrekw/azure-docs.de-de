---
title: Apache Hadoop-Komponenten und -Versionen – Azure HDInsight
description: Lernen Sie die Apache Hadoop-Komponenten und -Versionen in Azure HDInsight kennen.
ms.service: hdinsight
ms.topic: conceptual
author: deshriva
ms.author: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: 53ca2ac73fdec9d3b39ffc04cbb24aca707a72eb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103490448"
---
# <a name="azure-hdinsight-versions"></a>Versionen von Azure HDInsight

HDInsight bündelt Apache Hadoop-Umgebungskomponenten und die HDInsight-Platform in einem Paket, das in einem Cluster bereitgestellt wird. Weitere Informationen finden Sie unter [How versioning works in HDInsight](hdinsight-overview-versioning.md) (Funktionsweise der Versionsverwaltung in HDInsight, in englischer Sprache).

## <a name="supported-hdinsight-versions"></a>Unterstützte HDInsight-Versionen

Die Tabelle enthält die im Azure-Portal und über andere Bereitstellungsmethoden, z. B. PowerShell, die CLI und das .NET SDK, verfügbaren Versionen von HDInsight.

| HDInsight-Version | Betriebssystem des virtuellen Computers | Veröffentlichungsdatum | Supportablaufdatum | Deaktivierungstermin | Hochverfügbarkeit |
| --- | --- | --- | --- | --- | --- |
| [HDInsight 4.0](hdinsight-40-component-versioning.md) |Ubuntu 16.0.4 LTS |24. September 2018 | | |Ja |
| [HDInsight 3.6](hdinsight-36-component-versioning.md) |Ubuntu 16.0.4 LTS |4\. April 2017      | *30. Juni 2021 |30. Juni 2021 |Ja |

\* Wir verlängern den Supportzeitraum für bestimmte HDInsight 3.6-Clustertypen. Siehe [HDInsight 3.6 component versions](hdinsight-36-component-versioning.md) (HDInsight 3.6-Komponentenversionen, in englischer Sprache)

## <a name="release-notes"></a>Versionshinweise

In den [HDInsight-Versionshinweisen](hdinsight-release-notes.md) finden Sie zusätzliche Anmerkungen zu den aktuellen Versionen von HDInsight.

## <a name="support-options-for-hdinsight-versions"></a>Supportoptionen für HDInsight-Versionen

HDInsight bietet Standardsupport. Dies ist der Zeitraum, in dem eine HDInsight-Version vom Microsoft-Kundendienst und -Support unterstützt wird.

**Supportablauf** bedeutet, dass Microsoft für die jeweilige HDInsight-Version keinen Support mehr bereitstellt. Die Version ist zudem nicht mehr über das Azure-Portal für die Clustererstellung verfügbar.

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