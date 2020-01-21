---
title: Azure Storage-Beispiele mit JavaScript | Microsoft-Dokumentation
description: Sie können Beispielcode und -anwendungen für Azure Storage anzeigen, herunterladen und ausführen. Entdecken Sie Beispiele für erste Schritte mit Blobs, Warteschlangen, Tabellen und Dateien unter Verwendung der Speicherclientbibliotheken für JavaScript/Node.js.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/26/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 44fe68b8b04a1192c928e04c7d2a9d147f400130
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75748290"
---
# <a name="azure-storage-samples-using-javascript"></a>Azure Storage-Beispiele mit JavaScript

Die folgenden Tabellen enthalten eine Übersicht über unser Beispielrepository und in jedem Beispiel behandelten Szenarien. Klicken Sie auf die Links, um den zugehörigen Beispielcode in GitHub anzuzeigen.

> [!NOTE]
> In diesen Beispielen wird die JavaScript-v10-Bibliothek für Azure Storage verwendet. v12-Code finden Sie in den [Beispielen](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob/samples) im GitHub-Repository.

## <a name="blob-samples-v10"></a>Blobbeispiele (v10)

| **Szenario** | **Beispielcode** |
|--------------|-----------------|
| Blockblob | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L43) |
| Clientseitige Verschlüsselung | [Verwalten von Speicherkontoschlüsseln in Azure Key Vault mit JavaScript](https://github.com/Azure-Samples/key-vault-node-storage-accounts) |
| Kopieren von Blobs | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L73) |
| Create Container | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L54) |
| Delete Blob | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L103) |
| Delete Container | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L110) |
| Blobmetadaten | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L538) |
| Blobeigenschaften | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L478) |
| Container-ACL | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L444) |
| Containermetadaten | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L409) |
| Containereigenschaften | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L377) |
| Get Page Ranges | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L170) |
| Lease Blob | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L216) |
| Lease Container | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L185) |
| Listenblob/-container | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L134) |
| Seitenblob | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L129) |
| SAS | [Shared Access Signature in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L257) |
| Diensteigenschaften | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L308) |
| Festlegen von CORS-Regeln | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/advanced.js#L152) |
| Snapshot Blob | [Erste Schritte mit dem Azure-Blobdienst in JavaScript](https://github.com/Azure-Samples/storage-blob-node-getting-started/blob/master/basic.js#L79) |

## <a name="file-samples-v10"></a>Dateibeispiele (v10)

| **Szenario** | **Beispielcode** |
|--------------|-----------------|
| Erstellen von Freigaben/Verzeichnissen/Dateien | [Erste Schritte mit dem Azure-Dateidienst in JavaScript](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L97) |
| Löschen von Freigaben/Verzeichnissen/Dateien | [Erste Schritte mit dem Azure-Dateidienst in JavaScript](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L135) |
| Herunterladen von Dateien | [Erste Schritte mit dem Azure-Dateidienst in JavaScript](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L128) |
| Auflisten von Verzeichnissen und Dateien | [Erste Schritte mit dem Azure-Dateidienst in JavaScript](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L115) |
| Auflisten von Freigaben | [Erste Schritte mit dem Azure-Dateidienst in JavaScript](https://github.com/Azure-Samples/storage-file-node-getting-started/blob/master/fileSample.js#L187) |

## <a name="queue-samples-v10"></a>Warteschlangenbeispiele (v10)

| **Szenario** | **Beispielcode** |
|--------------|-----------------|
| Hinzufügen von Nachrichten | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L142) |
| Clientseitige Verschlüsselung | [Verwalten von Speicherkontoschlüsseln in Azure Key Vault mit JavaScript](https://github.com/Azure-Samples/key-vault-node-storage-accounts) |
| Erstellen von Warteschlangen | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L57) |
| Löschen einer Nachricht | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L164) |
| Löschen einer Warteschlange | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L203) |
| Auflisten von Warteschlangen | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L111) |
| Nachricht einsehen | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L170) |
| Warteschlangen-ACL | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L192) |
| CORS-Regeln für Warteschlangen | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L55) |
| Warteschlangenmetadaten | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L161) |
| Warteschlangendiensteigenschaften | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L94) |
| Warteschlangenstatistiken | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/advanced.js#L149) |
| Aktualisieren von Nachrichten | [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](https://github.com/Azure-Samples/storage-queue-node-getting-started/blob/master/basic.js#L176) |

## <a name="table-samples-v10"></a>Tabellenbeispiele (v10)

| **Szenario** | **Beispielcode** |
|--------------|-----------------|
| Batch-Entitäten | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L87) |
| Erstellen einer Tabelle | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L41) |
| Löschen von Entitäten/Tabellen | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L67) |
| Einfügen/Zusammenführen/Ersetzen von Entitäten | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L49) |
| Auflisten von Tabellen | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L63) |
| Entitäten abfragen | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L59) |
| Abfragen von Tabellen | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L140) |
| Bereichsabfrage | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L102) |
| SAS | [Shared Access Signature in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L87) |
| Tabellen-ACL | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L255) |
| CORS-Regeln für Tabellen | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L149) |
| Tabelleneigenschaften | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L188) |
| Tabellenstatistiken | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/advanced.js#L243) |
| Entität aktualisieren | [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](https://github.com/Azure-Samples/storage-table-node-getting-started/blob/master/basic.js#L49) |

## <a name="azure-code-samples-library"></a>Bibliothek mit Azure-Codebeispielen

Wechseln Sie zum Anzeigen der vollständigen Beispielbibliothek zur [Bibliothek mit Azure-Codebeispielen](https://azure.microsoft.com/resources/samples/?service=storage), die Beispiele für Azure Storage enthält, die Sie herunterladen und lokal ausführen können. Die Bibliothek mit Codebeispielen enthält Beispielcode im ZIP-Format. Alternativ können Sie die einzelnen Beispiele im GitHub-Repository suchen und klonen.

[!INCLUDE [storage-node-samples-include](../../../includes/storage-node-samples-include.md)]

## <a name="getting-started-guides"></a>Leitfäden mit ersten Schritten

Lesen Sie den folgenden Leitfäden, um Anleitungen zum Installieren und zu den ersten Schritten mit den Azure Storage-Clientbibliotheken zu erhalten.

* [Erste Schritte mit dem Azure-Blobdienst in JavaScript](../blobs/storage-quickstart-blobs-nodejs.md)
* [Erste Schritte mit dem Azure-Warteschlangendienst in JavaScript](../queues/storage-nodejs-how-to-use-queues.md)
* [Erste Schritte mit dem Azure-Tabellendienst in JavaScript](../../cosmos-db/table-storage-how-to-use-nodejs.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Beispielen für andere Sprachen:

* .NET: [Azure Storage-Beispiele mit .NET](storage-samples-dotnet.md)
* Java: [Azure Storage-Beispiele mit Java](storage-samples-java.md)
* Python: [Azure Storage-Beispiele mit Python](storage-samples-python.md)
* Alle anderen Sprachen: [Beispiele für Azure Storage](storage-samples.md)
