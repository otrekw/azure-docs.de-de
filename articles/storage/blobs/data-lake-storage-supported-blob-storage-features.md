---
title: Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Blob Storage-Features, die Sie mit Azure Data Lake Storage Gen2 verwenden können.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 215244204aa58cc2fdedc639d48e01b514759694
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78196007"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2

Blob Storage-Features, z. B. [Diagnoseprotokollierung](../common/storage-analytics-logging.md), [Zugriffsebenen](storage-blob-storage-tiers.md) und  [Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md), können jetzt für Konten mit einem hierarchischen Namespace verwendet werden. Daher können Sie hierarchische Namespaces in Ihren Blob Storage-Konten aktivieren, ohne den Zugriff auf diese Features zu verlieren.

In dieser Tabelle sind die Blob Storage-Features aufgeführt, die mit Azure Data Lake Storage Gen2 verwendet werden können. Die in diesen Tabellen enthaltenen Elemente ändern sich im Laufe der Zeit, da die Unterstützung laufend erweitert wird.

## <a name="supported-blob-storage-features"></a>Unterstützte Blob Storage-Features

> [!NOTE]
> Die Supportebene bezieht sich lediglich darauf, wie das Feature mit Data Lake Storage Gen2 unterstützt wird.

|Blob Storage-Feature |Supportebene |Verwandte Artikel |
|---------------|-------------------|---|
|Zugriffsebene „Hot“|Allgemein verfügbar|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Zugriffsebene „Cool“|Allgemein verfügbar|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Events|Allgemein verfügbar|[Reacting to Blob storage events (preview)](storage-blob-event-overview.md) (Reagieren auf Blob Storage-Ereignisse (Vorschauversion))|
|Metriken (klassisch)|Allgemein verfügbar|[Metriken von Azure Storage Analytics (klassisch)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Metriken in Azure Monitor|Allgemein verfügbar|[Azure Storage-Metriken in Azure Monitor](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob Storage-PowerShell-Befehle|Allgemein verfügbar|[Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit PowerShell](storage-quickstart-blobs-powershell.md)|
|Blob Storage-Azure CLI-Befehle|Allgemein verfügbar|[Schnellstart: Erstellen, Herunterladen und Auflisten von Blobs mit der Azure-Befehlszeilenschnittstelle](storage-quickstart-blobs-cli.md)|
|Blob Storage-APIs|Allgemein verfügbar|[Schnellstart: Azure Blob Storage-Clientbibliothek v12 für .NET](storage-quickstart-blobs-dotnet.md)<br>[Schnellstart: Verwalten von Blobs per Java v12 SDK](storage-quickstart-blobs-java.md)<br>[Schnellstart: Verwalten von Blobs per Python v12 SDK](storage-quickstart-blobs-python.md)<br>[Schnellstart: Verwalten von Blobs per JavaScript v12 SDK in Node.js](storage-quickstart-blobs-nodejs.md)|
|Zugriffsebene „Archiv“|Vorschau|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Richtlinien für die Lebenszyklusverwaltung|Vorschau|[Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)|
|Diagnoseprotokolle|Vorschau|[Azure Storage Analytics-Protokollierung](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Änderungsfeed|Noch nicht unterstützt|[Unterstützung für Änderungsfeeds in Azure Blob Storage](storage-blob-change-feed.md)|
|Failover des Kontos|Noch nicht unterstützt|[Notfallwiederherstellung und Kontofailover](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blobcontainer-ACL|Noch nicht unterstützt|[Set Container ACL](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Benutzerdefinierte Domänen|Noch nicht unterstützt|[Zuordnen einer benutzerdefinierten Domäne zu einem Azure Blob Storage-Endpunkt](storage-custom-domain-name.md)|
|Unveränderlicher Speicher|Noch nicht unterstützt|[Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher](storage-blob-immutable-storage.md)|
|Momentaufnahmen|Noch nicht unterstützt|[Erstellen und Verwalten einer Blobmomentaufnahme in .NET](storage-blob-snapshots.md)|
|Vorläufiges Löschen|Noch nicht unterstützt|[Vorläufiges Löschen für Azure Storage-Blobs](storage-blob-soft-delete.md)|
|Statische Websites|Noch nicht unterstützt|[Hosten von statischen Websites in Azure Storage](storage-blob-static-website.md)|
|Protokollierung in Azure Monitor|Noch nicht unterstützt|Noch nicht verfügbar|
|Premium-Blockblobs|Noch nicht unterstützt|[Erstellen eines BlockBlobStorage-Kontos](storage-blob-create-account-block-blob.md)|

## <a name="see-also"></a>Weitere Informationen

- [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-azure-services.md)
- [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md)
- [Multiprotokollzugriff für Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)