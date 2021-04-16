---
title: Verschlüsselungsbereiche für Blobspeicher
description: Verschlüsselungsbereiche ermöglichen Ihnen die Verwaltung der Verschlüsselung auf der Ebene des Containers oder eines einzelnen Blobs. Sie können Verschlüsselungsbereiche verwenden, um sichere Grenzen zwischen Daten zu erstellen, die sich im selben Speicherkonto befinden, aber zu unterschiedlichen Kunden gehören.
services: storage
author: tamram
ms.service: storage
ms.date: 03/26/2021
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 16a600d7caf65f880ffb5c2a2abfe5a9774a7795
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105640458"
---
# <a name="encryption-scopes-for-blob-storage"></a>Verschlüsselungsbereiche für Blobspeicher

Verschlüsselungsbereiche ermöglichen Ihnen die Verwaltung der Verschlüsselung mit einem Schlüssel, der auf einen Container oder ein einzelnes Blob beschränkt ist. Sie können Verschlüsselungsbereiche verwenden, um sichere Grenzen zwischen Daten zu erstellen, die sich im selben Speicherkonto befinden, aber zu unterschiedlichen Kunden gehören.

Weitere Informationen zum Erstellen eines Verschlüsselungsbereichs finden Sie unter [Erstellen und Verwalten von Verschlüsselungsbereichen](encryption-scope-manage.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="how-encryption-scopes-work"></a>Funktionsweise von Verschlüsselungsbereichen

Standardmäßig wird ein Speicherkonto mit einem Schlüssel verschlüsselt, der für das gesamte Speicherkonto gilt. Wenn Sie einen Verschlüsselungsbereich definieren, geben Sie einen Schlüssel an, der für einen Container oder für ein einzelnes Blob festgelegt werden kann. Wenn der Verschlüsselungsbereich auf ein Blob angewendet wird, wird das Blob mit diesem Schlüssel verschlüsselt. Wenn der Verschlüsselungsbereich auf einen Container angewendet wird, fungiert er als Standardbereich für Blobcontainer in diesem Container, sodass alle Blobwerte, die in diesen Container hochgeladen werden, mit demselben Schlüssel verschlüsselt werden können. Der Container kann so konfiguriert werden, dass der Standard-Verschlüsselungsbereich für alle Blobs im Container erzwungen wird, oder, um zuzulassen, dass ein einzelnes Blob mit einem anderen als dem Standard-Verschlüsselungsbereich in den Container hochgeladen wird.

Lesevorgänge für ein Blob, das zu einem Verschlüsselungsbereich gehört, erfolgen transparent, sofern der Verschlüsselungsbereich nicht deaktiviert ist.

### <a name="key-management"></a>Schlüsselverwaltung

Beim Erstellen eines Verschlüsselungsbereichs geben Sie an, ob der Bereich durch einen von Microsoft verwalteten Schlüssel oder einen kundenseitig verwalteten Schlüssel, der in Azure Key Vault gespeichert ist, geschützt wird. Unterschiedliche Verschlüsselungsbereiche im gleichen Speicherkonto können entweder von Microsoft verwaltete oder kundenseitig verwaltete Schlüssel verwenden. Sie können auch den Schlüsseltyp, der zum Schutz eines Verschlüsselungsbereichs verwendet wird, von einem kundenseitig verwalteten Schlüssel zu einem von Microsoft verwalteten Schlüssel oder umgekehrt wechseln. Weitere Informationen zu kundenseitig verwalteten Schlüsseln finden Sie unter [Verwendung von kundenseitig verwalteten Schlüsseln für die Azure Storage-Verschlüsselung](../common/customer-managed-keys-overview.md). Weitere Informationen zu von Microsoft verwalteten Schlüsseln finden Sie unter [Informationen zur Verwaltung von Verschlüsselungsschlüsseln](../common/storage-service-encryption.md#about-encryption-key-management).

Wenn Sie einen Verschlüsselungsbereich mit einem kundenseitig verwalteten Schlüssel definieren, können Sie entscheiden, ob Sie die Schlüsselversion entweder automatisch oder manuell aktualisieren möchten. Wenn Sie die Schlüsselversion automatisch aktualisieren, prüft Azure Storage den Schlüsseltresor oder das verwaltete HSM täglich auf eine neue Version des kundenseitig verwalteten Schlüssels, und der Schlüssel wird automatisch auf die neueste Version aktualisiert. Weitere Informationen zum Aktualisieren der Schlüsselversion für einen kundenseitig verwalteten Schlüssel finden Sie unter [Aktualisieren der Schlüsselversion](../common/customer-managed-keys-overview.md#update-the-key-version).

Ein Speicherkonto kann bis zu 10 000 Verschlüsselungsbereiche aufweisen, die mit kundenseitig verwalteten Schlüsseln geschützt sind, für die die Schlüsselversion automatisch aktualisiert wird. Wenn Ihr Speicherkonto bereits über 10 000 Verschlüsselungsbereiche verfügt, die mit kundenseitig verwalteten Schlüsseln geschützt sind, die automatisch aktualisiert werden, muss die Schlüsselversion für alle zusätzlichen Verschlüsselungsbereiche, die mit kundenseitig verwalteten Schlüsseln geschützt sind, manuell aktualisiert werden.  

### <a name="encryption-scopes-for-containers-and-blobs"></a>Verschlüsselungsbereiche für Container und Blobs

Wenn Sie einen Container erstellen, können Sie einen Standard-Verschlüsselungsbereich für die Blobwerte angeben, die anschließend in diesen Container hochgeladen werden. Wenn Sie einen Standard-Verschlüsselungsbereich für einen Container angeben, können Sie festlegen, wie der Standard-Verschlüsselungsbereich erzwungen wird:

- Sie können verlangen, dass alle in den Container hochgeladenen Blobwerte den Standard-Verschlüsselungsbereich verwenden. In diesem Fall wird jedes Blob im Container mit demselben Schlüssel verschlüsselt.
- Sie können zulassen, dass ein Client den Standard-Verschlüsselungsbereich für den Container außer Kraft setzt, damit ein Blob mit einem anderen Verschlüsselungsbereich als dem Standardbereich hochgeladen werden kann. In diesem Fall können die Blobdaten im Container mit anderen Schlüsseln verschlüsselt werden.

In der folgenden Tabelle wird das Verhalten eines Blob-Uploadvorgangs zusammengefasst, je nachdem, wie der Standard-Verschlüsselungsbereich für den Container konfiguriert ist:

| Der für den Container definierte Verschlüsselungsbereich ist ... | Ein Blob wird mit dem Standard-Verschlüsselungsbereich hochgeladen ... | Ein Blob wird mit einem anderen Verschlüsselungsbereich als dem Standardbereich hochgeladen ... |
|--|--|--|
| Einen Standard-Verschlüsselungsbereich mit Außerkraftsetzungen zulassen | Ist erfolgreich | Ist erfolgreich |
| Einen Standard-Verschlüsselungsbereich mit Außerkraftsetzungen ablehnen | Ist erfolgreich | Gescheitert |

Ein Standard-Verschlüsselungsbereich muss für einen Container angegeben werden, wenn der Container erstellt wird.

Wenn für den Container kein Standard-Verschlüsselungsbereich angegeben ist, können Sie ein Blob mit einem beliebigen Verschlüsselungsbereich hochladen, den Sie für das Speicherkonto definiert haben. Der Verschlüsselungsbereich muss zu dem Zeitpunkt angegeben werden, zu dem das Blob hochgeladen wird.

## <a name="disabling-an-encryption-scope"></a>Deaktivieren eines Verschlüsselungsbereichs

Wenn Sie einen Verschlüsselungsbereich deaktivieren, schlagen alle nachfolgenden Lese- oder Schreibvorgänge, die mit dem Verschlüsselungsbereich vorgenommen werden, mit dem HTTP-Fehlercode 403 (unzulässig) fehl. Wenn Sie den Verschlüsselungsbereich erneut aktivieren, werden Lese- und Schreibvorgänge in der Regel wieder normal ausgeführt.

Wenn ein Verschlüsselungsbereich deaktiviert ist, wird Ihnen dieser nicht mehr in Rechnung gestellt. Deaktivieren Sie alle Verschlüsselungsbereiche, die nicht benötigt werden, um unnötige Gebühren zu vermeiden.

Wenn Ihr Verschlüsselungsbereich durch einen kundenseitig verwalteten Schlüssel geschützt ist und Sie den Schlüssel im Schlüsseltresor löschen, kann auf die Daten nicht mehr zugegriffen werden. Stellen Sie sicher, dass Sie auch den Verschlüsselungsbereich deaktivieren, damit keine Gebühren anfallen.

Beachten Sie, dass kundenseitig verwaltete Schlüssel gegen vorläufiges und endgültiges Löschen im Schlüsseltresor geschützt sind und dass sich ein gelöschter Schlüssel entsprechend den dafür definierten Eigenschaften verhält. Weitere Information finden Sie in den folgenden Themen in der Azure Key Vault-Dokumentation:

- [Verwenden des vorläufigen Löschens mit PowerShell](../../key-vault/general/key-vault-recovery.md)
- [Verwenden des vorläufigen Löschens mit der CLI](../../key-vault/general/key-vault-recovery.md)

> [!IMPORTANT]
> Das Löschen eines Verschlüsselungsbereichs ist nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](../common/storage-service-encryption.md)
- [Erstellen und Verwalten von Verschlüsselungsbereichen](encryption-scope-manage.md)
- [Kundenseitig verwaltete Schlüssel für die Azure Storage-Verschlüsselung](../common/customer-managed-keys-overview.md)
- [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md)