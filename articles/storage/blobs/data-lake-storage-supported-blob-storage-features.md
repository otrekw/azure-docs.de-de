---
title: Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Blob Storage-Features, die Sie mit Azure Data Lake Storage Gen2 verwenden können.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 06/09/2021
ms.author: normesta
ms.openlocfilehash: 25fe02ea4103dec2539e429fd2c9aac4274cdd45
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112074789"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Verfügbare Blob Storage-Features in Azure Data Lake Storage Gen2

Blob Storage-Features, z. B. [Diagnoseprotokollierung](../common/storage-analytics-logging.md), [Zugriffsebenen](storage-blob-storage-tiers.md) und [Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md) können jetzt für Konten mit einem hierarchischen Namespace verwendet werden. Daher können Sie hierarchische Namespaces in Ihren Blob Storage-Konten aktivieren, ohne den Zugriff auf diese Features zu verlieren.

In dieser Tabelle sind die Blob Storage-Features aufgeführt, die mit Azure Data Lake Storage Gen2 verwendet werden können. Die in diesen Tabellen enthaltenen Elemente ändern sich im Laufe der Zeit, da die Unterstützung laufend erweitert wird. Weitere Informationen zu bestimmen Problemen im Zusammenhang mit dem Vorschaustatus eines Features finden Sie im Artikel [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md).

## <a name="supported-blob-storage-features"></a>Unterstützte Blob Storage-Features

Die folgende Tabelle zeigt, wie jedes Blob Storage-Feature mit Data Lake Storage Gen2 unterstützt wird. Es gibt eine Spalte für die [Leistungsstufen Standard und Premium](premium-tier-for-data-lake-storage.md). 

|Blob Storage-Feature |Standard |Premium |Verwandte Artikel |
|---------------|-------------------|---|
|Zugriffsebene „Hot“|Allgemein verfügbar|Nicht unterstützt|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Zugriffsebene „Cool“|Allgemein verfügbar|Nicht unterstützt|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Events|Allgemein verfügbar|Allgemein verfügbar|[Reacting to Blob storage events (preview)](storage-blob-event-overview.md) (Reagieren auf Blob Storage-Ereignisse (Vorschauversion))|
|Metriken (klassisch)|Allgemein verfügbar|Allgemein verfügbar|[Metriken von Azure Storage Analytics (klassisch)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Metriken in Azure Monitor|Allgemein verfügbar|Vorschau|[Azure Storage-Metriken in Azure Monitor](./monitor-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob Storage-PowerShell-Befehle|Allgemein verfügbar|Allgemein verfügbar|[Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit PowerShell](storage-quickstart-blobs-powershell.md)|
|Blob Storage-Azure CLI-Befehle|Allgemein verfügbar|Allgemein verfügbar|[Schnellstart: Erstellen, Herunterladen und Auflisten von Blobs mit der Azure-Befehlszeilenschnittstelle](storage-quickstart-blobs-cli.md)|
|Blob Storage-APIs|Allgemein verfügbar|Allgemein verfügbar|[Schnellstart: Azure Blob Storage-Clientbibliothek v12 für .NET](storage-quickstart-blobs-dotnet.md)<br>[Schnellstart: Verwalten von Blobs per Java v12 SDK](storage-quickstart-blobs-java.md)<br>[Schnellstart: Verwalten von Blobs per Python v12 SDK](storage-quickstart-blobs-python.md)<br>[Schnellstart: Verwalten von Blobs per JavaScript v12 SDK in Node.js](storage-quickstart-blobs-nodejs.md)|
|Diagnoseprotokolle|Allgemein verfügbar|Vorschau |[Azure Storage Analytics-Protokollierung](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Zugriffsebene „Archiv“|Allgemein verfügbar|Nicht unterstützt|[Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md)|
|Lebenszyklusverwaltung (Tiering)|Allgemein verfügbar|Noch nicht unterstützt|[Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)|
|Richtlinien für die Lebenszyklusverwaltung (Blob löschen)|Allgemein verfügbar|Allgemein verfügbar|[Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)|
|Protokollierung in Azure Monitor|Vorschau |Vorschau|[Überwachen von Azure Storage](./monitor-blob-storage.md)|
|Momentaufnahmen|Vorschau<div role="complementary" aria-labelledby="preview-form"><sup>1</sup></div>|Vorschau<div role="complementary" aria-labelledby="preview-form"><sup>1</sup></div>|[Blobmomentaufnahmen](snapshots-overview.md)|
|Statische Websites|Vorschau<div role="complementary" aria-labelledby="preview-form"></div>|Vorschau<div role="complementary" aria-labelledby="preview-form"></div>|[Hosten von statischen Websites in Azure Storage](storage-blob-static-website.md)|
|Unveränderlicher Speicher|Vorschau<div role="complementary" aria-labelledby="preview-form"><sup>1</sup></div>|Vorschau<div role="complementary" aria-labelledby="preview-form"><sup>1</sup></div>|[Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher](storage-blob-immutable-storage.md)|
|Vorläufiges Löschen von Containern|Vorschau|Vorschau|[Vorläufiges Löschen für Container (Vorschau)](soft-delete-container-overview.md)|
|Azure Storage-Inventar|Vorschau|Vorschau|[Verwalten von Blobdaten mit dem Azure Storage-Inventar (Vorschau)](blob-inventory.md)|
|Benutzerdefinierte Domänen|Vorschau<div role="complementary" aria-labelledby="preview-form-2"><sup>2</sup></div>|Vorschau<div role="complementary" aria-labelledby="preview-form-2"><sup>2</sup></div>|[Zuordnen einer benutzerdefinierten Domäne zu einem Azure Blob Storage-Endpunkt](storage-custom-domain-name.md)|
|Vorläufiges Löschen von Blobs|Noch nicht unterstützt|Noch nicht unterstützt|[Vorläufiges Löschen für Blobs](./soft-delete-blob-overview.md)|
|blobfuse|Allgemein verfügbar|Allgemein verfügbar|[Einbinden von Blob Storage als Dateisystem mit blobfuse](storage-how-to-mount-container-linux.md)|
|Anonymer öffentlicher Zugriff |Allgemein verfügbar|Allgemein verfügbar| Siehe [Konfigurieren des anonymen, öffentlichen Lesezugriffs auf Container und Blobs](anonymous-read-access-configure.md).|
|Failover für kundenseitig verwaltetes Konto|Noch nicht unterstützt|Noch nicht unterstützt|[Notfallwiederherstellung und Kontofailover](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Vom Kunden bereitgestellte Schlüssel|Noch nicht unterstützt|Noch nicht unterstützt|[Angeben eines Verschlüsselungsschlüssels bei Richten einer Anforderung an Blob Storage](encryption-customer-provided-keys.md)|
|Verschlüsselungsbereiche|Noch nicht unterstützt|Noch nicht unterstützt|[Erstellen und Verwalten von Verschlüsselungsbereichen](encryption-scope-manage.md)|
|Änderungsfeed|Noch nicht unterstützt|Noch nicht unterstützt|[Unterstützung für Änderungsfeeds in Azure Blob Storage](storage-blob-change-feed.md)|
|Objektreplikation|Noch nicht unterstützt|Noch nicht unterstützt|[Konfigurieren der Objektreplikation für Blockblobs](object-replication-configure.md)|
|Blobversionsverwaltung|Noch nicht unterstützt|Noch nicht unterstützt|[Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)|
|Wiederherstellung bis zu einem bestimmten Zeitpunkt|Noch nicht unterstützt|Noch nicht unterstützt|[Point-in-Time-Wiederherstellung für Blockblobs](point-in-time-restore-overview.md)|
|Blobindextags|Noch nicht unterstützt|Noch nicht unterstützt|[Verwalten und Finden von Azure-Blobdaten mit Blobindextags](storage-manage-find-blobs.md)|

<div id="preview-form"><sup>1</sup> Für die Verwendung von Momentaufnahmen, unveränderlichem Speicher oder statischen Websites mit Data Lake Storage Gen2 müssen Sie sich für die Vorschau registrieren. Füllen Sie dazu dieses <a href=https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VUOUc3NTNQSUdOTjgzVUlVT1pDTzU4WlRKRy4u>Formular</a> aus.  </div>
<div id="preview-form-2"><sup>2</sup>Ein benutzerdefinierter Domänenname kann nur dem Blob-Dienstendpunkt oder dem statischen Websiteendpunkt zugeordnet werden. Der Data Lake Storage-Endpunkt wird nicht unterstützt.</a>.  </div>

## <a name="see-also"></a>Weitere Informationen

- [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-azure-services.md)
- [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md)
- [Multiprotokollzugriff für Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)
