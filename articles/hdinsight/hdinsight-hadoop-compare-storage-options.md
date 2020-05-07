---
title: Vergleich der Speicheroptionen für die Verwendung mit Azure HDInsight-Clustern
description: Dieser Artikel bietet einen Überblick über die Speichertypen und deren Funktionsweise mit Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: ed93ba937a843618f36bac6e88b15ff77355ca75
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610699"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Vergleich der Speicheroptionen für die Verwendung mit Azure HDInsight-Clustern

Sie haben beim Erstellen von HDInsight-Clustern die Auswahl zwischen verschiedenen Azure Storage-Diensten:

* [Azure Storage (in englischer Sprache)](./overview-azure-storage.md)
* [Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen2.md)
* [Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen1.md)

Dieser Artikel bietet eine Übersicht über diese Speichertypen und ihre besonderen Merkmale.

## <a name="storage-types-and-features"></a>Speichertypen und Features

Die folgende Tabelle enthält die Azure Storage-Dienste, die mit den unterschiedlichen Versionen von HDInsight unterstützt werden:

| Speicherdienst | Kontotyp | Namespacetyp | Unterstützte Dienste | Unterstützte Leistungsstufen | Unterstützte Zugriffsebenen | HDInsight-Version | Clustertyp |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Allgemein v2 | Hierarchisch (Dateisystem) | Blob | Standard | Heiß, Kalt, Archiv | 3.6 und höher | Alle außer Spark 2.1 und 2.2|
|Azure Storage| Allgemein v2 | Object | Blob | Standard | Heiß, Kalt, Archiv | 3.6 und höher | All |
|Azure Storage| Allgemein v1 | Object | Blob | Standard | – | All | All |
|Azure Storage| Blob Storage** | Object | Blockblob | Standard | Heiß, Kalt, Archiv | All | All |
|Azure Data Lake Storage Gen1| – | Hierarchisch (Dateisystem) | – | – | – | Nur 3.6 | Alle mit Ausnahme von HBase |

**Für HDInsight-Cluster können nur sekundäre Speicherkonten den Typ „BlobStorage“ aufweisen, und der Seitenblob ist keine unterstützte Speicheroption.

Weitere Informationen zu Azure Storage-Kontotypen finden Sie unter [Azure-Speicherkonten – Übersicht](../storage/common/storage-account-overview.md).

Weitere Informationen zu Azure Storage-Zugriffsebenen finden Sie unter [Azure Blob Storage: Speicherebenen „Premium“ (Vorschauversion), „Heiß“, „Kalt“ und „Archiv“](../storage/blobs/storage-blob-storage-tiers.md).

Sie können einen Cluster mit Kombinationen von Diensten für den primären und optional für den sekundären Speicher erstellen. Die folgende Tabelle enthält die Clusterspeicherkonfigurationen, die derzeit in HDInsight unterstützt werden:

| HDInsight-Version | Primärer Speicher | Sekundärer Speicher | Unterstützt |
|---|---|---|---|
| 3.6 und 4.0 | Universell V1, Universell V2 | Universell V1, Universell V2, BlobStorage (Blockblobs) | Ja |
| 3.6 und 4.0 | Universell V1, Universell V2 | Data Lake Storage Gen2 | Nein  |
| 3.6 und 4.0 | Data Lake Storage Gen2* | Data Lake Storage Gen2 | Ja |
| 3.6 und 4.0 | Data Lake Storage Gen2* | Universell V1, Universell V2, BlobStorage (Blockblobs) | Ja |
| 3.6 und 4.0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Nein  |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Ja |
| 3.6 | Data Lake Storage Gen1 | Universell V1, Universell V2, BlobStorage (Blockblobs) | Ja |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Nein  |
| 4,0 | Data Lake Storage Gen1 | Any | Nein  |
| 4,0 | Universell V1, Universell V2 | Data Lake Storage Gen1 | Nein  |

\* Dies können etwa Data Lake Storage Gen2-Konten sein, sofern sie alle für die Verwendung derselben verwalteten Identität für den Clusterzugriff eingerichtet wurden.

> [!NOTE]
> Primärer Data Lake Storage Gen2-Speicher wird für Spark 2.1- und 2.2-Cluster nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Azure Storage](./overview-azure-storage.md)
* [Übersicht über Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen1.md)
* [Übersicht über Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen2.md)
* [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Einführung in Azure Storage](../storage/common/storage-introduction.md)
