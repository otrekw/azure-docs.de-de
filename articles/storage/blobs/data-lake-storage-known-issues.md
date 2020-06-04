---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über Einschränkungen und bekannte Probleme von Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 05/10/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 315bab473ccb42c3ca7b481c16bf8d8fbcfaac67
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193458"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden Einschränkungen und bekannte Probleme von Azure Data Lake Storage Gen2 beschrieben.

## <a name="supported-blob-storage-features"></a>Unterstützte Blob Storage-Features

Immer mehr Blob Storage-Features funktionieren nun mit Konten, die über einen hierarchischen Namespace verfügen. Eine vollständige Liste finden Sie unter [Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Unterstützte Azure-Dienstintegrationen

Azure Data Lake Storage Gen2 unterstützt verschiedene Azure-Dienste, die Sie zum Erfassen von Daten, zum Durchführen von Analysen und zum Erstellen visueller Darstellungen verwenden können. Eine Liste der unterstützten Azure-Dienste finden Sie unter [Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-azure-services.md).

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

<a id="api-scope-data-lake-client-library"></a>

## <a name="file-system-support-in-sdks-powershell-and-azure-cli"></a>Dateisystemunterstützung in SDKs, PowerShell und der Azure CLI

- Get- und Set-Vorgänge für Zugriffssteuerungslisten sind derzeit nicht rekursiv.


## <a name="lifecycle-management-policies"></a>Richtlinien für die Lebenszyklusverwaltung

Das Löschen von Blobmomentaufnahmen wird noch nicht unterstützt. 

## <a name="archive-tier"></a>Archivzugriffsebene

Zurzeit gibt es einen Fehler, der sich auf die Archivzugriffsebene auswirkt.

## <a name="blobfuse"></a>blobfuse

Blobfuse wird nicht unterstützt.

<a id="known-issues-tools"></a>

## <a name="azcopy"></a>AzCopy

Verwenden Sie nur die neueste Version von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Frühere Versionen von AzCopy wie z. B. AzCopy v8.1 werden nicht unterstützt.

<a id="storage-explorer"></a>

## <a name="azure-storage-explorer"></a>Azure Storage-Explorer

Verwenden Sie nur Versionen ab  `1.6.0` .

<a id="explorer-in-portal"></a>

## <a name="storage-explorer-in-the-azure-portal"></a>Storage-Explorer im Azure-Portal

ACLs werden noch nicht unterstützt.

<a id="third-party-apps"></a>

## <a name="thirdpartyapplications"></a>Drittanbieteranwendungen

Drittanbieteranwendungen, die REST-APIs verwenden, funktionieren auch weiterhin, wenn Sie sie mit Data Lake Storage Gen2 verwenden. Anwendungen, die Blob-APIs aufrufen, funktionieren wahrscheinlich.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Zugriffssteuerungslisten (Access Control Lists, ACLs) und anonymer Lesezugriff

Wenn [anonymer Lesezugriff](storage-manage-access-to-resources.md) für einen Container gewährt wurde, haben ACLs keine Auswirkungen auf diesen Container oder die darin enthaltenen Dateien.

## <a name="premium-performance-blockblobstorage-storage-accounts"></a>BlockBlobStorage-Speicherkonten mit Premium-Leistung

### <a name="diagnostic-logs"></a>Diagnoseprotokolle

Diagnoseprotokolle können noch nicht über das Azure-Portal aktiviert werden. Sie können sie mithilfe der PowerShell aktivieren. Beispiel:

```powershell
#To login
Connect-AzAccount

#Set default block blob storage account.
Set-AzCurrentStorageAccount -Name premiumGen2Account -ResourceGroupName PremiumGen2Group

#Enable logging
Set-AzStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays 14
```

### <a name="lifecycle-management-policies"></a>Richtlinien für die Lebenszyklusverwaltung

- Richtlinien für die Lebenszyklusverwaltung werden in BlockBlobStorage-Speicherkonten mit Premium-Leistung noch nicht unterstützt. 

- Daten können nicht aus dem Tarif „Premium“ in niedrigere Tarife verschoben werden. 

- Die Aktion **Blob löschen** wird aktuell nicht unterstützt. 

### <a name="hdinsight-support"></a>HDInsight-Support

Wenn Sie einen HDInsight-Cluster erstellen, können Sie noch kein BlockBlobStorage-Konto auswählen, für das die Funktion „hierarchischer Namespace“ aktiviert ist. Sie können das Konto aber an den Cluster anfügen, nachdem Sie es erstellt haben.

### <a name="dremio-support"></a>Dremio-Unterstützung

Dremio kann noch keine Verbindung mit einem BlockBlobStorage-Konto herstellen, für das die Funktion „hierarchischer Namespace“ aktiviert ist. 

## <a name="windows-azure-storage-blob-wasb-driver-unsupported-with-data-lake-storage-gen2"></a>WASB-Treiber (Windows Azure Storage Blob) (bei Data Lake Storage Gen2 nicht unterstützt)

Derzeit treten beim WASB-Treiber, der nur für die Verwendung mit der Blob-API entwickelt wurde, in einigen gängigen Szenarien Probleme auf. Dies gilt insbesondere, wenn es sich um einen Client für ein Speicherkonto mit aktiviertem hierarchischen Namespace handelt. Durch einen Multiprotokollzugriff für Data Lake Storage lassen sich diese Probleme nicht beheben. 

Zum gegenwärtigen Zeitpunkt (und wahrscheinlich auch in absehbarer Zukunft) werden Kunden, die den WASB-Treiber als Client für ein Speicherkonto mit aktiviertem hierarchischen Namespace verwenden, nicht unterstützt. Stattdessen wird empfohlen, den [ABFS](data-lake-storage-abfs-driver.md)-Treiber (Azure Blob File System) in der Hadoop-Umgebung zu verwenden. Wenn Sie versuchen, eine Migration von einer lokalen Hadoop-Umgebung mit einer früheren Version als Hadoop Branch-3 durchzuführen, öffnen Sie ein Azure-Supportticket, damit wir uns mit Ihnen in Verbindung setzen können, um den richtigen Weg für Sie und Ihre Organisation zu finden.
