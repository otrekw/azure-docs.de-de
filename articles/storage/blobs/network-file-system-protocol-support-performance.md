---
title: Überlegungen zur NFS 3.0-Leistung in Azure Blob Storage (Vorschau) | Microsoft-Dokumentation
description: Optimieren Sie mit den Empfehlungen in diesem Artikel die Leistung Ihrer NFS 3.0-Speicheranforderungen (Network File System).
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 02/23/2021
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions
ms.openlocfilehash: f8a780afba1f5703fbe457e113ed1b455f1e9b64
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743374"
---
# <a name="network-file-system-nfs-30-performance-considerations-in-azure-blob-storage-preview"></a>Überlegungen zur NFS 3.0-Leistung (Network File System) in Azure Blob Storage (Vorschau)

Blob Storage unterstützt jetzt das NFS 3.0-Protokoll (Network File System). Dieser Artikel enthält Empfehlungen, mit deren Hilfe Sie die Leistung Ihrer Speicheranforderungen optimieren können. Weitere Informationen zur Unterstützung von NFS 3.0 in Azure Blob Storage finden Sie unter [Unterstützung für Network File System 3.0 (NFS) in Azure Blob Storage (Vorschau)](network-file-system-protocol-support.md).

> [!NOTE]
> Die Unterstützung für das NFS 3.0-Protokoll in Azure Blob Storage befindet in der öffentlichen Vorschau. Das Protokoll unterstützt GPV2-Speicherkonten mit der Leistungsstufe „Standard“ in den folgenden Regionen: „Australien, Osten“, „Südkorea, Mitte“ und „USA, Süden-Mitte“. Die Vorschau unterstützt auch Blockblobs mit der Leistungsstufe „Premium“ in allen öffentlichen Regionen.

## <a name="add-clients-to-increase-throughput"></a>Hinzufügen von Clients zum Erhöhen des Durchsatzes 

Azure Blob Storage wird linear skaliert, bis der Maximalwert für eingehende und ausgehende Daten für das Speicherkonto erreicht ist. Daher können Ihre Anwendungen durch Verwenden von mehr Clients einen höheren Durchsatz erzielen.  Die Maximalwerte für eingehende und ausgehende Daten für das Speicherkonto finden Sie unter [Skalierbarkeits- und Leistungsziele für Standardspeicherkonten](../common/scalability-targets-standard-account.md).

Das folgende Diagramm zeigt, wie die Bandbreite zunimmt, wenn Sie mehr Clients hinzufügen. In diesem Diagramm ist ein Client ein virtueller Computer (VM), und das Konto nutzt die Leistungsstufe „Standard“. 

> [!div class="mx-imgBorder"]
> ![Standardleistung](./media/network-file-system-protocol-support-performance/standard-performance-tier.png)

Das folgende Diagramm zeigt den Effekt, wenn dieser Vorgang auf ein Konto in der Leistungsstufe „Premium“ angewendet wird.

> [!div class="mx-imgBorder"]
> ![Standardleistung](./media/network-file-system-protocol-support-performance/premium-performance-tier.png)

## <a name="use-premium-performance-tier-for-small-scale-applications"></a>Verwenden der Leistungsstufe „Premium“ für kleinere Anwendungen

Nicht alle Anwendungen lassen sich durch Hinzufügen von mehr Clients hochskalieren. Für solche Anwendungen bietet das [Azure-Premium-Blockblob-Speicherkonto](storage-blob-create-account-block-blob.md) konsistente hohe Transaktionsraten mit niedriger Latenz. Das Premium-Blockblob-Speicherkonto kann die maximale Bandbreite mit weniger Threads und Clients erreichen. Ein Beispiel: Mit einem einzelnen Client kann ein Premium-Blockblob-Speicherkonto **2,3-mal** so viel Bandbreite erreichen wie dasselbe Setup mit einem Speicherkonto vom Typ „Universell v2“ mit Standardleistung. 

Jeder Balken im folgenden Diagramm zeigt den Unterschied in der erreichten Bandbreite zwischen Speicherkonten mit der Leistungsstufe „Premium“ und „Standard“. In dem Maß, in dem die Anzahl von Clients steigt, verringert sich der Unterschied.  

> [!div class="mx-imgBorder"]
> ![Relative Leistung](./media/network-file-system-protocol-support-performance/relative-performance.png)

## <a name="avoid-frequent-overwrites-on-date"></a>Vermeiden von häufigen Überschreibungen der Daten

Ein Überschreibungsvorgang dauert länger als ein neuer Schreibvorgang. Das liegt daran, dass ein NFS-Überschreibungsvorgang – insbesondere eine direkte Dateiteilbearbeitung – eine Kombination aus mehreren zugrunde liegenden Blobvorgängen ist: ein Lesevorgang, ein Änderungsvorgang und ein Schreibvorgang. Aus diesem Grund ist eine Anwendung, die häufige direkte Bearbeitungen erfordert, für NFS-fähige Blobspeicherkonten nicht geeignet. 

## <a name="other-best-practice-recommendations"></a>Weitere Empfehlungen zu Best Practices 

- Verwenden Sie VMs mit ausreichender Netzwerkbandbreite.

- Verwenden Sie mehrere Bereitstellungspunkte, wenn Ihre Workloads dies zulassen.

- Verwenden Sie so viele Threads wie möglich.

- Verwenden Sie große Blöcke.

- Führen Sie Speicheranforderungen von einem Client aus, der sich in derselben Region befindet wie das Speicherkonto. Damit lässt sich die Netzwerklatenz verbessern.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Unterstützung von NFS 3.0 in Azure Blob Storage finden Sie unter [Unterstützung für Network File System 3.0 (NFS) in Azure Blob Storage (Vorschau)](network-file-system-protocol-support.md).

- Informationen zu den ersten Schritten finden Sie unter [Einbinden von Blob-Speicher mithilfe des NFS 3.0-Protokolls (Network File System) (Vorschau)](network-file-system-protocol-support-how-to.md).
