---
title: Azure Storage-Verschlüsselung für ruhende Daten
description: Azure Storage schützt Ihre Daten, indem der Dienst diese automatisch verschlüsselt, bevor diese in der Cloud gespeichert werden. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung der Daten in Ihrem Speicherkonto nutzen oder die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten.
services: storage
author: tamram
ms.service: storage
ms.date: 03/23/2021
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 0688e14b77d885132d6c3fbaa44bed117cc7cf9d
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105641129"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Azure Storage-Verschlüsselung für ruhende Daten

Azure Storage verwendet serverseitige Verschlüsselung (Server Side Encryption, SSE), um Ihre Daten beim Speichern in der Cloud automatisch zu verschlüsseln. Durch die Azure Storage-Verschlüsselung werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden.

## <a name="about-azure-storage-encryption"></a>Informationen zur Azure Storage-Verschlüsselung

Daten in Azure Storage werden auf transparente Weise mit der [AES-256-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) ver- und entschlüsselt, einer der stärksten verfügbaren Blockchiffren, und sind mit dem FIPS 140-2-Standard konform. Die Azure Storage-Verschlüsselung ähnelt der BitLocker-Verschlüsselung unter Windows.

Die Azure Storage-Verschlüsselung ist für alle Speicherkonten aktiviert, einschließlich Resource Manager-Speicherkonten und klassischer Speicherkonten. Die Azure Storage-Verschlüsselung kann nicht deaktiviert werden. Da Ihre Daten standardmäßig geschützt werden, müssen Sie weder Code noch Anwendungen ändern, um die Azure Storage-Verschlüsselung nutzen zu können.

Daten in einem Speicherkonto werden unabhängig von der Leistungsstufe (Standard oder Premium), der Zugriffsebene (heiß oder kalt) und dem Bereitstellungsmodell (Azure Resource Manager oder klassisch) verschlüsselt. Außerdem werden auch alle Blobs auf der Archivebene verschlüsselt. Die Verschlüsselung wird von allen Azure Storage-Redundanzoptionen unterstützt, und bei aktivierter Georeplikation werden alle Daten in der primären und sekundären Region verschlüsselt. Alle Azure Storage-Ressourcen werden verschlüsselt, z. B. Blobs, Datenträger, Dateien, Warteschlangen und Tabellen. Objektmetadaten werden ebenfalls verschlüsselt. Es fallen keine zusätzlichen Kosten für die Azure Storage-Verschlüsselung an.

Alle Blockblobs, Anfügeblobs und Seitenblobs, die nach dem 20. Oktober 2017 in Azure Storage geschrieben wurden, sind verschlüsselt. Blobs, die vor diesem Datum erstellt wurden, werden weiterhin durch einen Hintergrundprozess verschlüsselt. Um die Verschlüsselung eines Blobs zu erzwingen, das vor dem 20. Oktober 2017 erstellt wurde, können Sie das Blob neu schreiben. Informationen zum Überprüfen des Verschlüsselungsstatus eines Blobs finden Sie unter [Überprüfen des Verschlüsselungsstatus eines Blobs](../blobs/storage-blob-encryption-status.md).

Weitere Informationen zu den kryptografischen Modulen, die der Azure Storage-Verschlüsselung zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](/windows/desktop/seccng/cng-portal).

Weitere Informationen zur Verschlüsselung und Schlüsselverwaltung für verwaltete Azure-Datenträger finden Sie unter [Serverseitige Verschlüsselung von verwalteten Azure-Datenträgern](../../virtual-machines/disk-encryption.md).

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Daten in einem neuen Speicherkonto werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihrer Daten weiterhin nutzen oder die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten. Wenn Sie die Verschlüsselungsverwaltung mit Ihren eigenen Schlüsseln wählen, haben Sie zwei Optionen. Sie können eine der beiden Arten der Schlüsselverwaltung oder beide verwenden:

- Sie können einen *kundenseitig verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln von Daten in Blob Storage und in Azure Files verwendet werden soll.<sup>1,2</sup> Kundenseitig verwaltete Schlüssel müssen in Azure Key Vault oder Azure Key Vault Managed Hardware Security Model (HSM, Vorschau) gespeichert werden. Weitere Informationen zu kundenseitig verwalteten Schlüsseln finden Sie unter [Verwenden kundenseitig verwalteter Schlüssel für die Azure Storage-Verschlüsselung](./customer-managed-keys-overview.md).
- Sie können einen *vom Kunden bereitgestellten Schlüssel* für Blob-Speichervorgänge angeben. Ein Client, der eine Lese- oder Schreibanforderung für Blob Storage sendet, kann einen Verschlüsselungsschlüssel für die Anforderung enthalten, um genau steuern zu können, wie Blobdaten verschlüsselt und entschlüsselt werden. Weitere Informationen zu vom Kunden bereitgestellten Schlüsseln finden Sie unter [Angeben eines Verschlüsselungsschlüssels bei Stellen einer Anforderung für Blob Storage](../blobs/encryption-customer-provided-keys.md).

In der folgenden Tabelle werden die Schlüsselverwaltungsoptionen für Azure Storage-Verschlüsselung verglichen.

| Schlüsselverwaltungsparameter | Von Microsoft verwaltete Schlüssel | Vom Kunden verwaltete Schlüssel | Vom Kunden bereitgestellte Schlüssel |
|--|--|--|--|
| Verschlüsselungs-/Entschlüsselungsvorgänge | Azure | Azure | Azure |
| Unterstützte Azure Storage-Dienste | All | Blob Storage, Azure Files<sup>1,2</sup> | Blob Storage |
| Schlüsselspeicher | Microsoft-Schlüsselspeicher | Azure Key Vault oder Key Vault HSM | Eigener Schlüsselspeicher des Kunden |
| Verantwortlich für die Schlüsselrotation | Microsoft | Kunde | Kunde |
| Schlüsselkontrolle | Microsoft | Kunde | Kunde |

<sup>1</sup> Informationen zum Erstellen eines Kontos, das die Verwendung von kundenseitig verwalteten Schlüsseln mit Queue Storage unterstützt, finden Sie unter [Erstellen eines Kontos, das kundenseitig verwaltete Schlüssel für Warteschlangen](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) unterstützt.<br />
<sup>2</sup> Informationen zum Erstellen eines Kontos, das die Verwendung von kundenseitig verwalteten Schlüsseln mit Table Storage unterstützt, finden Sie unter [Erstellen eines Kontos, das kundenseitig verwaltete Schlüssel für Tabellen](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json) unterstützt.

> [!NOTE]
> Von Microsoft verwaltete Schlüssel werden entsprechend den Complianceanforderungen gedreht. Wenn Sie bestimmte Anforderungen an die Schlüsselrotation haben, empfiehlt Microsoft, dass Sie zu kundenseitig verwalteten Schlüsseln wechseln, sodass Sie die Rotation selbst verwalten und überprüfen können.

## <a name="doubly-encrypt-data-with-infrastructure-encryption"></a>Doppelte Verschlüsselung von Daten mit Infrastrukturverschlüsselung

Kunden, die hohe Anforderungen an den Schutz ihrer Daten stellen, können auch die 256-Bit-AES-Verschlüsselung auf Azure Storage-Infrastrukturebene aktivieren. Wenn die Infrastrukturverschlüsselung aktiviert ist, werden Daten in einem Speicherkonto zweimal &mdash; einmal auf dem Servicelevel und einmal auf der Infrastrukturebene &mdash; mit zwei unterschiedlichen Verschlüsselungsalgorithmen und zwei verschiedenen Schlüsseln verschlüsselt. Die doppelte Verschlüsselung von Azure Storage-Daten schützt vor dem Szenario, dass einer der Verschlüsselungsalgorithmen oder Schlüssel kompromittiert wurde. In diesem Szenario werden die Daten weiterhin durch die zusätzliche Verschlüsselungsebene geschützt.

Die Verschlüsselung auf dem Servicelevel unterstützt die Verwendung von Microsoft verwalteter Schlüssel oder vom Kunden verwalteter Schlüssel mit Azure Key Vault. Die Verschlüsselung auf Infrastrukturebene basiert auf von Microsoft verwalteten Schlüsseln und verwendet immer einen separaten Schlüssel.

Weitere Informationen zum Erstellen eines Speicherkontos, das die Infrastrukturverschlüsselung ermöglicht, finden Sie unter [Erstellen eines Speicherkontos mit aktivierter Infrastrukturverschlüsselung für die doppelte Datenverschlüsselung](infrastructure-encryption-enable.md).

## <a name="next-steps"></a>Nächste Schritte

- [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md)
- [Kundenseitig verwaltete Schlüssel für die Azure Storage-Verschlüsselung](customer-managed-keys-overview.md)
- [Verschlüsselungsbereiche für Blobspeicher](../blobs/encryption-scope-overview.md)
- [Angeben eines Verschlüsselungsschlüssels bei Richten einer Anforderung an Blob Storage](../blobs/encryption-customer-provided-keys.md)
