---
title: 'Einführung in die Versionsverwaltung: Azure HDInsight'
description: Erfahren Sie, wie die Versionsverwaltung in Azure HDInsight funktioniert.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: b7b23a1d7e549d5e1e5b712d2290722158d49f38
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122345786"
---
# <a name="how-versioning-works-in-hdinsight"></a>Funktionsweise der Versionsverwaltung in HDInsight

Der HDInsight-Dienst verfügt über zwei Hauptkomponenten: einen Ressourcenanbieter und Apache Hadoop-Komponenten, die in einem Cluster bereitgestellt werden. 

## <a name="hdinsight-resource-provider"></a>HDInsight-Ressourcenanbieter

Ressourcen werden in Azure von einem Ressourcenanbieter zur Verfügung gestellt. Der HDInsight-Ressourcenanbieter ist dafür verantwortlich, Cluster zu erstellen, zu verwalten und zu löschen.

## <a name="hdinsight-images"></a>HDInsight-Images

HDInsight verwendet Images, um Komponenten von Open-Source-Software (OSS) zusammenzustellen, die in einem Cluster bereitgestellt werden können. Diese Images enthalten das grundlegende Ubuntu-Betriebssystem und Kernkomponenten wie Spark, Hadoop, Kafka, HBase oder Hive.

## <a name="versioning-in-hdinsight"></a>Versionsverwaltung in HDInsight

Microsoft führt regelmäßig Upgrades der Images und des HDInsight-Ressourcenanbieters durch, um neue Verbesserungen und Features hinzuzufügen.

Eine neue HDInsight-Version kann erstellt werden, wenn einer oder mehrere der folgenden Punkte zutreffen:

- Wichtige Änderungen oder Aktualisierungen der Funktionalität des HDInsight-Ressourcenanbieters
- Hauptreleases von OSS-Komponenten
- Wichtige Änderungen des Ubuntu-Betriebssystems

Eine neue Imageversion wird erstellt, wenn einer oder mehrere der folgenden Punkte zutreffen:

- Haupt- oder Nebenreleases und Updates von OSS-Komponenten
- Patches oder Korrekturen für eine Komponente im Image

## <a name="next-steps"></a>Nächste Schritte

- [Apache-Komponenten und -Versionen in HDInsight](./hdinsight-component-versioning.md)
