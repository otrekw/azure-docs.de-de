---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über Einschränkungen und bekannte Probleme von Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 4f8fae6580272ed53b8d440ba3e74c6a1ed1e61a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061511"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden Einschränkungen und bekannte Probleme von Azure Data Lake Storage Gen2 beschrieben.

## <a name="supported-blob-storage-features"></a>Unterstützte Blob Storage-Features

Immer mehr Blob Storage-Features funktionieren nun mit Konten, die über einen hierarchischen Namespace verfügen. Eine vollständige Liste finden Sie unter [Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Unterstützte Azure-Dienstintegrationen

Data Lake Storage Gen2 unterstützt verschiedene Azure-Dienste, die Sie zum Erfassen von Daten, zum Durchführen von Analysen und zum Erstellen visueller Darstellungen verwenden können. Eine Liste der unterstützten Azure-Dienste finden Sie unter [Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-azure-services.md).

Siehe [Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-azure-services.md).

## <a name="supported-open-source-platforms"></a>Unterstützte Open-Source-Plattformen

Mehrere Open-Source-Plattformen unterstützen Data Lake Storage Gen2. Eine vollständige Liste finden Sie unter [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md).

Siehe [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md).

## <a name="blob-storage-apis"></a>Blob Storage-APIs

Blob-APIs und Data Lake Storage Gen2-APIs können mit denselben Daten arbeiten.

In diesem Abschnitt werden Probleme und Einschränkungen bei der Verwendung von Blob-APIs und Data Lake Storage Gen2-APIs für dieselben Daten beschrieben.

* Es ist nicht möglich, sowohl Blob-APIs als auch Data Lake Storage-APIs zu verwenden, um in dieselbe Instanz einer Datei zu schreiben. Wenn Sie in eine Datei schreiben, indem Sie Data Lake Storage Gen2-APIs verwenden, sind die Blöcke dieser Datei für Aufrufe der [Get Block List](https://docs.microsoft.com/rest/api/storageservices/get-block-list)-Blob-API nicht sichtbar. Sie können eine Datei überschreiben, indem Sie entweder Data Lake Storage Gen2-APIs oder Blob-APIs verwenden. Dies wirkt sich nicht auf die Dateieigenschaften aus.

* Wenn Sie den Vorgang [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) verwenden, ohne ein Trennzeichen anzugeben, enthalten die Ergebnisse sowohl Verzeichnisse als auch Blobs. Wenn Sie sich für Trennzeichen entscheiden, sollten Sie nur einen Schrägstrich (`/`) verwenden. Dies ist das einzige Trennzeichen, das unterstützt wird.

* Wenn Sie die [Delete Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob)-API zum Löschen eines Verzeichnisses verwenden, wird es nur gelöscht, sofern es leer ist. Dies bedeutet, dass Sie die Blob-API zum Löschen von Verzeichnissen nicht rekursiv verwenden können.

Diese Blob-Rest-APIs werden nicht unterstützt:

* [Put Blob (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Get Page Ranges](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Incremental Copy Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [Put Page from URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Put Blob (Append)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Append Block from URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

Nicht verwaltete VM-Datenträger werden für Konten, die über einen hierarchischen Namespace verfügen, nicht unterstützt. Wenn Sie einen hierarchischen Namespace für ein Speicherkonto aktivieren möchten, sollten Sie verwaltete VM-Datenträger in einem Speicherkonto anordnen, für das die Funktion für hierarchische Namespaces nicht aktiviert ist.

<a id="api-scope-data-lake-client-library" />

## <a name="file-system-support-in-sdks"></a>Dateisystemunterstützung in SDKs

Get- und Set-Vorgänge für Zugriffssteuerungslisten sind derzeit nicht rekursiv.

## <a name="file-system-support-in-powershell-and-azure-cli"></a>Dateisystemunterstützung in PowerShell und der Azure CLI

- Unterstützung für [PowerShell](data-lake-storage-directory-file-acl-powershell.md) und [Azure CLI](data-lake-storage-directory-file-acl-cli.md) befinden sich in der Public Preview.
- Get- und Set-Vorgänge für Zugriffssteuerungslisten sind derzeit nicht rekursiv.

## <a name="lifecycle-management-policies"></a>Richtlinien für die Lebenszyklusverwaltung

* Das Löschen von Blobmomentaufnahmen wird noch nicht unterstützt.  

## <a name="archive-tier"></a>Archivzugriffsebene

Zurzeit gibt es einen Fehler, der sich auf die Archivzugriffsebene auswirkt.


## <a name="blobfuse"></a>blobfuse

Blobfuse wird nicht unterstützt.

<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

Verwenden Sie nur die neueste Version von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Frühere Versionen von AzCopy wie z. B. AzCopy v8.1 werden nicht unterstützt.

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Azure Storage-Explorer

Verwenden Sie nur Versionen ab  `1.6.0` .

<a id="explorer-in-portal" />

## <a name="storage-explorer-in-the-azure-portal"></a>Storage-Explorer im Azure-Portal

ACLs werden noch nicht unterstützt.

<a id="third-party-apps" />

## <a name="thirdpartyapplications"></a>Drittanbieteranwendungen

Drittanbieteranwendungen, die REST-APIs verwenden, funktionieren auch weiterhin, wenn Sie sie mit Data Lake Storage Gen2 verwenden. Anwendungen, die Blob-APIs aufrufen, funktionieren wahrscheinlich.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Zugriffssteuerungslisten (Access Control Lists, ACLs) und anonymer Lesezugriff

Wenn [anonymer Lesezugriff](storage-manage-access-to-resources.md) für einen Container gewährt wurde, haben ACLs keine Auswirkungen auf diesen Container oder die darin enthaltenen Dateien.

## <a name="windows-azure-storage-blob-wasb-driver"></a>WASB-Treiber (Windows Azure Storage Blob)

Derzeit gibt es mehrere Probleme im Zusammenhang mit der Verwendung des WASB-Treibers in Verbindung mit Konten mit einem hierarchischen Namespace. Es wird empfohlen, in Ihren Workloads den [ABFS](data-lake-storage-abfs-driver.md)-Treiber (Azure Blob File System) zu verwenden. 





