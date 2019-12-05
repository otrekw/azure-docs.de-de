---
title: Aktivieren von Blobdaten aus der Archivzugriffsebene
description: Aktivieren Sie Ihre Blobs aus der Archivzugriffsebene, sodass Sie auf die Daten zugreifen können.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/14/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: d6370509b49ae464b53525e7320676b04912bd12
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113711"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Aktivieren von Blobdaten aus der Archivzugriffsebene

Während ein Blob sich auf der Archivzugriffsebene befindet, wird es als offline betrachtet und kann nicht gelesen oder geändert werden. Die Blobmetadaten bleiben online und verfügbar, sodass Sie das Blob und seine Eigenschaften auflisten können. Das Lesen und Ändern von Blobdaten ist nur auf Onlineebenen wie der heißen oder der kalten Zugriffsebene möglich. Es gibt zwei Optionen, um Daten aus der Archivzugriffsebene abzurufen und auf sie zuzugreifen.

1. [Aktivieren eines archivierten Blobs auf einer Onlineebene](#rehydrate-an-archived-blob-to-an-online-tier): Aktivieren Sie ein Archivblob auf der heißen oder kalten Zugriffsebene, indem Sie die Ebene mithilfe des Vorgangs [Blobebene festlegen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) ändern.
2. [Kopieren eines archivierten Blobs auf eine Onlineebene](#copy-an-archived-blob-to-an-online-tier): Erstellen Sie eine neue Kopie eines Archivblobs mithilfe des Vorgangs [Blob kopieren](https://docs.microsoft.com/rest/api/storageservices/copy-blob). Geben Sie einen anderen Blobnamen und als Zielebene eine heiße oder kalte Zugriffsebene an.

 Weitere Informationen zu Ebenen finden Sie unter [Azure Blob Storage: Archivzugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Aktivieren eines archivierten Blobs auf einer Onlineebene

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Kopieren eines archivierten Blobs auf eine Onlineebene

Wenn Sie das Archivblob nicht aktivieren möchten, können Sie einen Vorgang vom Typ [Blob kopieren](https://docs.microsoft.com/rest/api/storageservices/copy-blob) ausführen. Das ursprüngliche Blob bleibt unverändert im Archiv, während ein neues Blobs auf der heißen oder kalten Onlineebene erstellt wird, mit dem Sie arbeiten können. Beim Vorgang „Blob kopieren“ können Sie auch die optionale Eigenschaft *x-ms-rehydrate-priority* auf „Standard“ oder „Hoch“ (Vorschau) festlegen, um die Priorität anzugeben, mit der die Blobkopie erstellt werden soll.

Archivblobs können nur auf Onlinezielebenen innerhalb desselben Speicherkontos kopiert werden. Das Kopieren eines Archivblobs in ein anderes Archivblob wird nicht unterstützt.

Das Kopieren eines Blobs aus dem Archiv kann je nach ausgewählter Aktivierungspriorität mehrere Stunden dauern. Im Hintergrund liest der Vorgang **Blob kopieren** das Archivblob, um ein neues Onlineblob auf der ausgewählten Zielebene zu erstellen. Das neue Blob ist beim Auflisten von Blobs möglicherweise sichtbar, aber die Daten sind erst verfügbar, wenn der Lesevorgang aus dem Quellarchivblob abgeschlossen ist und Daten in das neue Onlinezielblob geschrieben werden. Das neue Blob ist eine unabhängige Kopie, und jegliche Änderungen oder Löschungen wirken sich nicht auf das Quellarchivblob aus.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Die Vorgänge zum Aktivieren von Blobs aus dem Archiv auf einer heißen oder kalten Zugriffsebene werden als Lese- und Datenabrufvorgänge abgerechnet. Vorgänge mit hoher Priorität (Vorschau) verursachen höhere Vorgangs- und Datenabrufkosten als Vorgänge mit Standardpriorität. Aktivierungsvorgänge mit hoher Priorität werden auf Ihrer Rechnung als separater Posten ausgewiesen. Wenn eine Anforderung zum Abruf eines einige Gigabytes großen Archivblobs mit hoher Priorität über fünf Stunden dauert, werden Ihnen nicht die Gebühren für einen Abruf mit hoher Priorität berechnet. Die Standardgebühren für Abrufvorgänge gelten jedoch weiterhin, da die Aktivierung gegenüber anderen Anforderungen priorisiert wurde.

Die Vorgänge zum Kopieren von Blobs aus dem Archiv auf eine heiße oder kalte Zugriffsebene werden als Lese- und Datenabrufvorgänge abgerechnet. Für die Erstellung der neuen Blobkopie wird ein Schreibvorgang in Rechnung gestellt. Gebühren für frühes Löschen fallen beim Kopieren in ein Onlineblob nicht an, weil das Quellblob auf der Archivzugriffsebene unverändert bleibt. Gebühren für einen Abruf mit hoher Priorität gelten, wenn dieser ausgewählt ist.

Blobs auf Archivzugriffsebene müssen mindestens 180 Tage lang gespeichert werden. Wenn archivierte Blobs vor Ablauf von 180 Tagen gelöscht oder aktiviert werden, fallen Gebühren für frühes Löschen an.

> [!NOTE]
> Weitere Informationen zu den Preisen für Blockblobs und Datenaktivierung finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/). Weitere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Blobspeicherebenen](storage-blob-storage-tiers.md)
* Prüfen der Preise für „Heiß“, „Kalt“ und „Archiv“ in Blobspeicher- und GPv2-Konten nach Region unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)
* [Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)
