---
title: Supportmatrix für Azure-Blobsicherung
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Sicherung von Azure-Blobs (Vorschau).
ms.topic: conceptual
ms.date: 02/16/2021
ms.custom: references_regions
ms.openlocfilehash: ade43350bbe3fa1bcf58f47e93b948db3a5b21bc
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743397"
---
# <a name="support-matrix-for-azure-blobs-backup-in-preview"></a>Supportmatrix für Azure-Blobsicherung (Vorschau)

In diesem Artikel erhalten Sie einen Überblick über die regionale Verfügbarkeit, die unterstützten Szenarien und die Einschränkungen der operativen Sicherung von Blobs.

## <a name="supported-regions"></a>Unterstützte Regionen

Die operative Sicherung für Blobs ist derzeit in den folgenden Regionen verfügbar: Australien, Mitte; Australien, Osten; Brasilien, Süden; Kanada, Mitte; Indien, Mitte; USA, Mitte; Asien, Osten; USA, Osten; USA, Osten 2, Deutschland, Westen-Mitte; Japan, Osten; Japan, Westen; Südkorea, Mitte; Südkorea, Süden; Europa, Norden; USA, Süden-Mitte, Asien, Südosten; Schweiz, Norden, VAE, Norden; Vereinigtes Königreich, Süden; Vereinigtes Königreich, Westen; USA, Westen-Mitte; Europa, Westen; USA, Westen; USA, Westen 2

## <a name="limitations"></a>Einschränkungen

Die operative Sicherung von Blobs verwendet die Zeitpunktwiederherstellung von Blobs, Blobversionierung, vorläufiges Löschen für Blobs, einen Änderungsfeed für Blobs und eine Löschsperre, um eine lokale Sicherungslösung bereitzustellen. Daher gelten Einschränkungen dieser einzelnen Funktionen auch für die operative Sicherung.

**Unterstützte Szenarien:** Die operative Sicherung unterstützt nur Blockblobs in Standardspeicherkonten vom Typ „Allgemein v2“. ADLS Gen2-Konten werden daher nicht unterstützt. Außerdem werden Seitenblobs, Anfügeblobs und Premium-Blobs in Ihrem Speicherkonto nicht wiederhergestellt. Nur Blockblobs werden wiederhergestellt.

**Weitere Einschränkungen:**

- Wenn Sie während der Beibehaltungsdauer einen Container gelöscht haben, wird dieser Container bei einer Zeitpunktwiederherstellung nicht wiederhergestellt. Bei dem Versuch, einen Bereich von Blobs wiederherzustellen, der Blobs in einem gelöschten Container beinhaltet, tritt bei der Zeitpunktwiederherstellung ein Fehler auf. Weitere Informationen zum Schutz von Containern vor dem Löschen finden Sie unter [Vorläufiges Löschen für Container (Vorschau)](https://docs.microsoft.com/azure/storage/blobs/soft-delete-container-overview).
- Wenn ein Blob innerhalb des Zeitraums zwischen dem aktuellen Moment und dem Wiederherstellungspunkt zwischen den heißen und kalten Speicherebenen verschoben wurde, wird das Blob auf der vorherigen Ebene wiederhergestellt. Das Wiederherstellen von Blockblobs auf Archivebene wird nicht unterstützt. Wenn ein Blob aus der heißen Speicherebene beispielsweise vor zwei Tagen in die Archivebene verschoben wurde und ein Wiederherstellungsvorgang für einen Zeitpunkt vor drei Tagen erfolgt, wird das Blob nicht in der heißen Zugriffsebene wiederhergestellt. Um ein archiviertes Blob wiederherzustellen, verschieben Sie es zuerst aus der Archivebene. Weitere Informationen finden Sie unter [Aktivieren von Blobdaten aus der Archivspeicherebene](https://docs.microsoft.com/azure/storage/blobs/storage-blob-rehydration).
- Ein Block, der über [Put Block](https://docs.microsoft.com/rest/api/storageservices/put-block) oder [Put Block from URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) hochgeladen, aber nicht über [Put Block List](https://docs.microsoft.com/rest/api/storageservices/put-block-list) committet wurde, ist nicht Teil eines Blobs und wird daher nicht als Teil eines Wiederherstellungsvorgangs wiederhergestellt.
- Ein Blob mit einer aktiven Leasedauer kann nicht wiederhergestellt werden. Wenn ein Blob mit einer aktiven Leasedauer in den Bereich der wiederherzustellenden Blobs eingeschlossen ist, schlägt der Wiederherstellungsvorgang automatisch fehl. Unterbrechen Sie alle aktiven Leases, bevor Sie den Wiederherstellungsvorgang starten.
- Momentaufnahmen werden im Rahmen eines Wiederherstellungsvorgangs nicht erstellt oder gelöscht. Nur das Basisblob wird in seinem vorherigen Zustand wiederhergestellt.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die operative Sicherung für Azure-Blobs (Vorschau)](blob-backup-overview.md)
