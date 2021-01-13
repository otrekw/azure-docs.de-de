---
title: Unterstützte Azure HDInsight-Knotenkonfigurationen
description: Erfahren Sie, wie die Mindest- und die empfohlenen Konfigurationen für HDInsight-Clusterknoten aussehen.
keywords: VM-Größen, Clustergrößen, Clusterkonfiguration
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 05/14/2020
ms.openlocfilehash: 0bf5559590b66400fc4fc4dc27ea88c3522effb1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83646618"
---
# <a name="what-are-the-default-and-recommended-node-configurations-for-azure-hdinsight"></a>Was sind die Standard- und die empfohlenen Knotenkonfigurationen für Azure HDInsight?

In diesem Artikel sind die Standard- und die empfohlenen Knotenkonfigurationen für Azure HDInsight erläutert.

## <a name="default-and-minimum-recommended-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standard- und mindestens erforderliche empfohlene Knotenkonfigurationen sowie -VM-Größen für Cluster

In den folgenden Tabellen sind die Standard- und die empfohlenen VM-Größen für HDInsight-Cluster aufgeführt.  Diese Informationen sind erforderlich, um die VM-Größen zu verstehen, die beim Erstellen von PowerShell- oder Azure CLI-Skripten verwendet werden sollten, mit denen HDInsight-Cluster bereitgestellt werden.

Wenn Sie mehr als 32 Workerknoten in einem Cluster benötigen, müssen Sie eine Hauptknotengröße mit mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen.

Die einzigen Clustertypen, die Datenträger haben, sind Kafka- und HBase-Cluster, wenn für diese das Accelerated Writes-Feature aktiviert ist. HDInsight unterstützt die Datenträgergröße P30 und S30 in diesen Szenarien. Für alle anderen Clustertypen stellt HDInsight mit dem Cluster verwalteten Speicherplatz auf dem Datenträger zur Verfügung. Seit dem 11.07.2019 beträgt die Größe des verwalteten Datenträgers jedes Knotens in dem neu erstellten Cluster 128 GB. Dies kann nicht geändert werden.

In dieser Tabelle finden Sie die in diesem Dokument verwendeten Spezifikationen aller mindestens erforderlichen empfohlenen VM-Typen.

| Size              | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBps Schreiben | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D3_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 16/16 x 500           | 4/3000                                       |
| Standard_D4_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 32/32 x 500           | 8/6000                                       |
| Standard_D5_v2 | 16   | 56          | 800                    | 48000/750/375                                          | 64/64 x 500           | 8 / 12000                                    |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16 x 500                         | 4/3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32 x 500                       | 8/6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/64 x 500                       | 8 / 12000          |
| Standard_A1_v2  | 1         | 2           | 10             | 1000/20/10                                           | 2/2 x 500               | 2/250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000/40/20                                           | 4/4 x 500               | 2/500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000/80/40                                           | 8/8 x 500               | 4/1000                     |

Weitere Informationen zu Spezifikationen jedes VM-Typs finden Sie in den folgenden Dokumenten:

* [Universelle VM-Größen: Dv2-Serie 1-5](../virtual-machines/dv2-dsv2-series.md)
* [Arbeitsspeicheroptimierte Größen virtueller Computer: Dv2-Serie 11-15](../virtual-machines/dv2-dsv2-series-memory.md)
* [Universelle VM-Größen: Av2-Serie 1-8](../virtual-machines/av2-series.md)

### <a name="all-supported-regions-except-brazil-south-and-japan-west"></a>Alle unterstützten Regionen, mit Ausnahme von „Brasilien, Süden“ und „Japan, Westen“

> [!Note]
> Um den SKU-Bezeichner für die Verwendung in PowerShell und anderen Skripts zu erhalten, fügen Sie `Standard_` am Anfang aller VM-SKUs in den folgenden Tabellen hinzu. Beispielsweise wird `D12_v2` zu `Standard_D12_v2`.

| Clustertyp | Hadoop | hbase | Interactive Query | Storm | Spark | ML Server | Kafka |
|---|---|---|---|---|---|---|---|
| Head – Standard-VM-Größe | D12_v2 | D12_v2 | D13_v2 | A4_v2 | D12_v2, <br/>D13_v2* | D12_v2 | D3_v2 |
| Head – mindestens erforderliche empfohlene VM-Größen | D5_v2 | D3_v2 | D13_v2 | A4_v2 | D12_v2, <br/>D13_v2* | D12_v2 | D3_v2 |
| Worker – Standard-VM-Größe | D4_v2 | D4_v2 | D14_v2 | D3_v2 | D13_v2 | D4_v2 | 4 D12_v2 mit 2 S30-Datenträgern pro Broker |
| Worker – mindestens erforderliche empfohlene VM-Größen | D5_v2 | D3_v2 | D13_v2 | D3_v2 | D12_v2 | D4_v2 | D3_v2 |
| Zookeeper – Standard-VM-Größen |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 | A4_v2 |
| ZooKeeper – mindestens erforderliche empfohlene VM-Größen |  | A4_v2 | A4_v2 | A2_v2 |  | A2_v2 | A4_v2 |
| ML Services: VM-Standardgröße |  |  |  |  |  | D4_v2 |  |
| ML Services – mindestens erforderliche empfohlene VM-Größen |  |  |  |  |  | D4_v2 |  |

\* = die VM-Größen für die Cluster des Spark-Enterprise-Sicherheitspakets (ESP)

### <a name="brazil-south-and-japan-west-only"></a>Nur „Brasilien, Süden“ und „Japan, Westen“

| Clustertyp | Hadoop | hbase | Interactive Query | Storm | Spark | ML Services |
|---|---|---|---|---|---|---|
| Head – Standard-VM-Größe | D12 | D12 | D13 | A4_v2 | D12 | D12 |
| Head – mindestens erforderliche empfohlene VM-Größen | D5_v2 | D3_v2 | D13_v2 | A4_v2 | D12_v2 | D12_v2 |
| Worker – Standard-VM-Größe | D4 | D4 | D14 | D3 | D13 | D4 |
| Worker – mindestens erforderliche empfohlene VM-Größen | D5_v2 | D3_v2 | D13_v2 | D3_v2 | D12_v2 | D4_v2 |
| Zookeeper – Standard-VM-Größen |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| ZooKeeper – mindestens erforderliche empfohlene VM-Größen |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| ML Services: VM-Standardgrößen |  |  |  |  |  | D4 |
| ML Services – mindestens erforderliche empfohlene VM-Größen |  |  |  |  |  | D4_v2 |

> [!NOTE]
> - Head wird für den Storm-Clustertyp als *Nimbus* bezeichnet.
> - Worker wird für den Storm-Clustertyp als *Supervisor* bezeichnet.
> - Worker wird für den HBase-Clustertyp als *Region* bezeichnet.

## <a name="next-steps"></a>Nächste Schritte

* [Welche Apache Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md)
