---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/05/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7c3438631dca921989309bb8701e113cb5ce3ff2
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89570154"
---
Azure Blob Storage ist die Objektspeicherlösung von Microsoft für die Cloud. Blobspeicher ist für die Speicherung großer Mengen unstrukturierter Daten optimiert. Unstrukturierte Daten sind Daten, die keinem bestimmten Datenmodell und keiner bestimmten Definition entsprechen (also beispielsweise Text- oder Binärdaten).

## <a name="about-blob-storage"></a>Informationen zu Azure-Blobspeicher

Blobspeicher ist für Folgendes konzipiert:

* Speichern von Bildern oder Dokumenten direkt für einen Browser
* Speichern von Dateien für verteilten Zugriff
* Video- und Audio-Streaming
* Schreiben in Protokolldateien
* Speichern von Daten für Sicherung und Wiederherstellung, Notfallwiederherstellung und Archivierung
* Speichern von Daten für Analysen durch einen lokalen oder von Azure gehosteten Dienst

Benutzer- oder Clientanwendungen können von überall auf der Welt über HTTP/HTTPS auf Objekte in Blobspeicher zugreifen. Auf Objekte in Blobspeicher kann über die [Azure Storage-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), über [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.storage), über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/storage) oder über eine Azure Storage-Clientbibliothek zugegriffen werden. Clientbibliotheken sind für verschiedene Sprachen verfügbar, einschließlich:

* [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Node.js](https://azure.github.io/azure-storage-node)
* [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python)
* [Go](https://github.com/azure/azure-storage-blob-go/)
* [PHP](https://azure.github.io/azure-storage-php/)
* [Ruby](https://azure.github.io/azure-storage-ruby)

## <a name="about-azure-data-lake-storage-gen2"></a>Informationen zu Azure Data Lake Storage Gen2

Blobspeicher unterstützt Azure Data Lake Storage Gen2, die cloudbasierte Big Data-Analyselösung von Microsoft für Unternehmen. Azure Data Lake Storage Gen2 bietet ein hierarchisches Dateisystem sowie die Vorteile von Blobspeicher, u. a.:

* Kostengünstiger, mehrstufiger Speicher
* Hochverfügbarkeit
* Hohe Konsistenz
* Funktionen für die Notfallwiederherstellung

Weitere Informationen zu Data Lake Storage Gen2 finden Sie in der [Einführung in Azure Data Lake Storage Gen2](../articles/storage/data-lake-storage/introduction.md).
