---
title: Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Blob Storage-Features, die Sie mit Azure Data Lake Storage Gen2 verwenden können.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 08/25/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 87e0fe7baaf3d83c18b12227e9ecb5bf1d2fb361
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88869455"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2

Blob Storage-Features, z. B. [Diagnoseprotokollierung](../common/storage-analytics-logging.md), [Zugriffsebenen](storage-blob-storage-tiers.md) und  [Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md), können jetzt für Konten mit einem hierarchischen Namespace verwendet werden. Daher können Sie hierarchische Namespaces in Ihren Blob Storage-Konten aktivieren, ohne den Zugriff auf diese Features zu verlieren.

In dieser Tabelle sind die Blob Storage-Features aufgeführt, die mit Azure Data Lake Storage Gen2 verwendet werden können. Die in diesen Tabellen enthaltenen Elemente ändern sich im Laufe der Zeit, da die Unterstützung laufend erweitert wird. Weitere Informationen zu bestimmen Problemen im Zusammenhang mit dem Vorschaustatus eines Features finden Sie im Artikel [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md).

## <a name="supported-blob-storage-features"></a>Unterstützte Blob Storage-Features

> [!NOTE]
> Die Supportebene bezieht sich lediglich darauf, wie das Feature mit Data Lake Storage Gen2 unterstützt wird. 
>
> [BlockBlobStorage Premium-Konten](storage-blob-create-account-block-blob.md) für Data Lake Storage Gen2 befinden sich aktuell in der öffentlichen Vorschau. Die Unterstützungsebenen für diese Arten von Konten werden in der Spalte **BlockBlobStorage (Premium)** angezeigt.

|Blob Storage-Feature |Allgemein v2 |BlockBlobStorage (Premium) |Verwandte Artikel |
|---------------|-------------------|---|
|Zugriffsebene „Hot“|Allgemein verfügbar|Nicht unterstützt|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Zugriffsebene „Cool“|Allgemein verfügbar|Nicht unterstützt|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Events|Allgemein verfügbar|Vorschau|[Reacting to Blob storage events (preview)](storage-blob-event-overview.md) (Reagieren auf Blob Storage-Ereignisse (Vorschauversion))|
|Metriken (klassisch)|Allgemein verfügbar|Nicht unterstützt|[Metriken von Azure Storage Analytics (klassisch)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Metriken in Azure Monitor|Allgemein verfügbar|Vorschau|[Azure Storage-Metriken in Azure Monitor](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob Storage-PowerShell-Befehle|Allgemein verfügbar|Vorschau|[Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit PowerShell](storage-quickstart-blobs-powershell.md)|
|Blob Storage-Azure CLI-Befehle|Allgemein verfügbar|Vorschau|[Schnellstart: Erstellen, Herunterladen und Auflisten von Blobs mit der Azure-Befehlszeilenschnittstelle](storage-quickstart-blobs-cli.md)|
|Blob Storage-APIs|Allgemein verfügbar|Vorschau|[Schnellstart: Azure Blob Storage-Clientbibliothek v12 für .NET](storage-quickstart-blobs-dotnet.md)<br>[Schnellstart: Verwalten von Blobs per Java v12 SDK](storage-quickstart-blobs-java.md)<br>[Schnellstart: Verwalten von Blobs per Python v12 SDK](storage-quickstart-blobs-python.md)<br>[Schnellstart: Verwalten von Blobs per JavaScript v12 SDK in Node.js](storage-quickstart-blobs-nodejs.md)|
|Diagnoseprotokolle|Allgemein verfügbar|Vorschau <div role="complementary" aria-labelledby="diagnostic-logging"><sup>1</sup></div> |[Azure Storage Analytics-Protokollierung](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Zugriffsebene „Archiv“|Allgemein verfügbar|Nicht unterstützt|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Richtlinien für die Lebenszyklusverwaltung|Allgemein verfügbar|Noch nicht unterstützt|[Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)|
|Protokollierung in Azure Monitor|Vorschau |Noch nicht unterstützt|[Überwachen von Azure Storage](../common/monitor-storage.md)|
|Momentaufnahmen|Vorschau|Noch nicht unterstützt|[Blobmomentaufnahmen](snapshots-overview.md)|
|Statische Websites|Vorschau|Noch nicht unterstützt|[Hosten von statischen Websites in Azure Storage](storage-blob-static-website.md)|
|Unveränderlicher Speicher|Vorschau|Noch nicht unterstützt|[Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher](storage-blob-immutable-storage.md)|
|Richtlinien für die Lebenszyklusverwaltung|Vorschau|Noch nicht unterstützt|[Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)|
|Vorläufiges Löschen von Containern|Vorschau|Vorschau|[Vorläufiges Löschen für Container (Vorschau)](soft-delete-container-overview.md)|
|Vorläufiges Löschen von Blobs|Noch nicht unterstützt|Noch nicht unterstützt|[Vorläufiges Löschen für Blobs](storage-blob-soft-delete.md)|
|blobfuse|Vorschau|Noch nicht unterstützt|[Einbinden von Blob Storage als Dateisystem mit blobfuse](storage-how-to-mount-container-linux.md)|
|Änderungsfeed|Noch nicht unterstützt|Noch nicht unterstützt|[Unterstützung für Änderungsfeeds in Azure Blob Storage](storage-blob-change-feed.md)|
|Failover des Kontos|Noch nicht unterstützt|Noch nicht unterstützt|[Notfallwiederherstellung und Kontofailover](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blobcontainer-ACL|Noch nicht unterstützt<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|Noch nicht unterstützt<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|[Set Container ACL](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Benutzerdefinierte Domänen|Noch nicht unterstützt|Noch nicht unterstützt|[Zuordnen einer benutzerdefinierten Domäne zu einem Azure Blob Storage-Endpunkt](storage-custom-domain-name.md)|
|Verschlüsselungsbereiche|Noch nicht unterstützt|Noch nicht unterstützt|[Erstellen und Verwalten von Verschlüsselungsbereichen (Vorschauversion)](encryption-scope-manage.md)|

<div id="diagnostic-logging"><sup>1</sup> Für Blockblobspeicher Premium-Konten, können Diagnoseprotokolle (klassisch) nicht über das Azure-Portal aktiviert werden. Sie können sie mithilfe von PowerShell aktivieren.</div><br>

<div id="blob-container-ACL"><sup>2</sup> Sie können ACLs für den Stammordner des Containers, jedoch nicht für den Container selbst festlegen.</div><br>

<div id="preview-form"><sup>3</sup> Für die Verwendung von Momentaufnahmen, unveränderlichem Speicher oder statischen Websites mit Data Lake Storage Gen2 müssen Sie sich für die Vorschau registrieren. Füllen Sie dazu dieses <a href=https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VUOUc3NTNQSUdOTjgzVUlVT1pDTzU4WlRKRy4u>Formular</a> aus.  </div>

## <a name="see-also"></a>Weitere Informationen

- [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-azure-services.md)
- [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md)
- [Multiprotokollzugriff für Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)
