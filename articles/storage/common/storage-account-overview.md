---
title: Speicherkontoübersicht
titleSuffix: Azure Storage
description: Erfahren Sie mehr über die verschiedenen Typen von Speicherkonten in Azure Storage. Überprüfen Sie Kontobenennung, Leistungsstufen, Zugriffsebenen, Redundanz, Verschlüsselung, Endpunkte und mehr.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/08/2021
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5cf43310c68c8446b9465a39d85f84c8273a68d8
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98051223"
---
# <a name="storage-account-overview"></a>Speicherkontoübersicht

Ein Azure Storage-Konto enthält all Ihre Azure Storage-Datenobjekte: Blobs, Dateien, Warteschlangen, Tabellen und Datenträger. Das Speicherkonto stellt einen eindeutigen Namespace für Ihre Azure Storage-Daten bereit, auf den von jedem Ort der Welt aus über HTTP oder HTTPS zugegriffen werden kann. Daten in Ihrem Azure Storage-Konto sind dauerhaft und hochverfügbar, sicher und extrem skalierbar.

Um zu erfahren, wie Sie ein Azure Storage-Konto erstellen, lesen Sie den Artikel [Erstellen eines Speicherkontos](storage-account-create.md).

## <a name="types-of-storage-accounts"></a>Speicherkontentypen

Azure Storage bietet mehrere Arten von Speicherkonten. Jeder Typ unterstützt unterschiedliche Features und verfügt über ein eigenes Preismodell. Informieren Sie sich vor dem Erstellen eines Speicherkontos genau über diese Unterschiede, um den Kontotyp zu ermitteln, der sich für Ihre Anwendungen am besten eignet. Folgende Speicherkontotypen stehen zur Verfügung:

- **Konten vom Typ „Allgemein v2“:** Grundlegender Speicherkontotyp für Blobs, Dateien, Warteschlangen und Tabellen. Empfohlen für die meisten Azure Storage-Szenarien.
- **Konten vom Typ „Allgemein v1“:** Legacy-Speicherkontotyp für Blobs, Dateien, Warteschlangen und Tabellen. Verwenden Sie nach Möglichkeit Konten vom Typ „Allgemein v2“.
- **BlockBlobStorage-Konten**: Speicherkonten mit Premium-Leistungsmerkmalen für Blockblobs und Anfügeblobs. Empfohlen für Szenarien mit hohen Transaktionsraten oder Szenarien, die kleinere Objekte verwenden oder aber eine gleichbleibend geringe Speicherlatenz erfordern.
- **FileStorage-Konten**: Reine Dateispeicherkonten mit Premium-Leistungsmerkmalen Empfohlen für Unternehmens- oder Hochleistungsanwendungen
- **BlobStorage-Konten**: Ältere reine Blobspeicherkonten. Verwenden Sie nach Möglichkeit Konten vom Typ „Allgemein v2“.

In der folgenden Tabelle werden die Typen von Speicherkonten, die jeweils unterstützten Dienste und die unterstützten Bereitstellungsmodelle der einzelnen Kontotypen beschrieben:

| Speicherkontotyp | Unterstützte Dienste | Redundanzoptionen | Bereitstellungsmodell<sup>1</sup> |
|--|--|--|--|
| Allgemein v2 | Blob, Datei, Warteschlange, Tabelle, Datenträger und Data Lake Gen2<sup>2</sup> | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS<sup>3</sup> | Ressourcen-Manager |
| Allgemein v1 | Blob, Datei, Warteschlange, Tabelle und Datenträger | LRS, GRS, RA-GRS | Resource Manager, klassisch |
| BlockBlobStorage | Blob (nur Blockblobs und Anfügeblobs) | LRS, ZRS<sup>3</sup> | Ressourcen-Manager |
| FileStorage | Nur Datei | LRS, ZRS<sup>3</sup> | Ressourcen-Manager |
| BlobStorage | Blob (nur Blockblobs und Anfügeblobs) | LRS, GRS, RA-GRS | Ressourcen-Manager |

<sup>1</sup>Die Verwendung des Azure Resource Manager-Bereitstellungsmodells wird empfohlen. Speicherkonten, die das klassische Bereitstellungsmodell verwenden, können an einigen Standorten weiterhin erstellt werden, und vorhandene klassische Konten werden weiterhin unterstützt. Weitere Informationen finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../../azure-resource-manager/management/deployment-models.md).

<sup>2</sup> Azure Data Lake Storage Gen2 basiert auf Azure Blob Storage und bietet eine Reihe von Funktionen für die Big Data-Analyse. Data Lake Storage Gen2 wird nur für Speicherkonten vom Typ „GPv2“ mit aktiviertem hierarchischen Namespace unterstützt. Weitere Informationen zu Data Lake Storage Gen2 finden Sie in der [Einführung in Azure Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md).

<sup>3</sup> Zonenredundanter Speicher (ZRS) und geozonenredundanter Speicher (GZRS/RA-GZRS) sind in bestimmten Regionen nur für Standardkonten des Typs „Universell V2“ sowie für Konten des Typs „BlockBlobStorage“ und „FileStorage“ verfügbar. Weitere Informationen zu den Azure Storage-Redundanzoptionen finden Sie unter [Azure Storage-Redundanz](storage-redundancy.md).

### <a name="storage-account-redundancy"></a>Speicherkontenredundanz

Redundanzoptionen für ein Speicherkonto umfassen Folgendes:

- **Lokal redundanter Speicher (LRS):** Eine einfache, kostengünstige Redundanzstrategie. Daten werden drei Mal synchron innerhalb eines einzigen physischen Speicherorts in der primären Region kopiert.
- **Zonenredundanter Speicher (ZRS):** Redundanz für Szenarien, die Hochverfügbarkeit erfordern. Daten werden synchron über drei Azure-Verfügbarkeitszonen hinweg in der primären Region kopiert.
- **Georedundanter Speicher (Geo-Redundant Storage, GRS)** : Regionsübergreifende Redundanz zum Schutz vor regionalen Ausfällen. Die Daten werden in der primären Region drei Mal synchron kopiert und dann asynchron in die sekundäre Region kopiert. Aktivieren Sie für den Lesezugriff auf die Daten in der sekundären Region den georedundanten Speicher mit Lesezugriff (RA-GRS).
- **Geozonenredundanter Speicher (GZRS)** : Redundanz für Szenarien, die sowohl Hochverfügbarkeit als auch maximale Dauerhaftigkeit erfordern. Die Daten werden synchron über drei Azure-Verfügbarkeitszonen in die primären Region kopiert und anschließend asynchron in die sekundäre Region kopiert. Aktivieren Sie für den Lesezugriff auf Daten in der sekundären Region den geozonenredundanten Speicher mit Lesezugriff (RA-GZRS).

Weitere Informationen zu Redundanzoptionen in Azure Storage finden Sie unter [Azure Storage-Redundanz](storage-redundancy.md).

### <a name="general-purpose-v2-accounts"></a>Allgemeines Konto vom Typ „General Purpose v2“

Speicherkonten vom Typ „Allgemein v2“ unterstützen die neuesten Azure Storage-Features und umfassen die gesamte Funktionalität von Konten des Typs „Allgemein v1“ und Blob Storage-Konten. Konten vom Typ „Allgemein v2“ bieten die niedrigsten Preise pro Gigabyte für Azure Storage sowie wettbewerbsfähige Transaktionspreise. Speicherkonten vom Typ „Allgemein v2“ unterstützen die folgenden Azure Storage-Dienste:

- Blobs (alle Typen: Blockblob, Anfügeblob, Seitenblob)
- Data Lake Gen2
- Dateien
- Datenträger
- Warteschlangen
- Tabellen

> [!NOTE]
> Microsoft empfiehlt für die meisten Szenarien die Verwendung von Speicherkonten vom Typ „Allgemein v2“. Sie können ganz einfach ein Upgrade von einem Allgemein v1-Konto oder einem Blob Storage-Konto auf ein Allgemein v2-Konto durchführen. Dabei treten keine Ausfallzeiten auf, und Sie müssen keine Daten kopieren.
>
> Weitere Informationen zum Upgrade auf ein Allgemein v2-Konto finden Sie unter [Upgrade auf ein Allgemein v2-Speicherkonto](storage-account-upgrade.md).

Allgemein v2-Speicherkonten bieten mehrere Zugriffsebenen zum Speichern von Daten basierend auf Ihren Nutzungsmustern. Weitere Informationen finden Sie unter [Zugriffsebenen für Blockblobdaten](#access-tiers-for-block-blob-data).

### <a name="general-purpose-v1-accounts"></a>Allgemeines Konto vom Typ „General Purpose v1“

Speicherkonten vom Typ „Universell v1“ ermöglichen Zugriff auf alle Azure Storage-Dienste, bieten aber möglicherweise nicht die neuesten Features oder die niedrigsten Preise pro Gigabyte. Speicherkonten vom Typ „Allgemein v1“ unterstützen die folgenden Azure Storage-Dienste:

- Blobs (alle Typen)
- Dateien
- Datenträger
- Warteschlangen
- Tabellen

Microsoft empfiehlt für die meisten Szenarien Allgemein v2-Konten. Konten vom Typ „Allgemein v1“ können für folgende Szenarien verwendet werden:

- Ihre Anwendungen erfordern das klassische Azure-Bereitstellungsmodell. Allgemein v2- und Blob Storage-Konten unterstützen nur das Azure Resource Manager-Bereitstellungsmodell.

- Ihre Anwendungen verursachen eine hohe Transaktionslast oder nutzen eine erhebliche Bandbreite für die Georeplikation, erfordern aber keine großen Kapazitäten. In diesem Fall sind Allgemein v1-Konten möglicherweise die wirtschaftlich sinnvollste Wahl.

- Sie verwenden eine ältere Version der [REST-API für Speicherdienste](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) (vor 2014-02-14) oder eine Clientbibliothek mit einer niedrigeren Version als 4.x. Sie können kein Upgrade für Ihre Anwendung durchführen.

### <a name="blockblobstorage-accounts"></a>BlockBlobStorage-Konten

Ein BlockBlobStorage-Konto ist ein spezielles Speicherkonto in der Leistungsstufe „Premium“ zum Speichern von unstrukturierten Objektdaten als Blockblobs oder Anfügeblobs. Im Vergleich zu universellen V2- und BlobStorage-Konten bieten BlockBlobStorage-Konten niedrige, einheitliche Latenzzeiten und höhere Transaktionsraten.

BlockBlobStorage-Konten unterstützen derzeit kein Tiering zu den Zugriffsebenen „Heiß“, „Kalt“ oder „Archiv“. Diese Art von Speicherkonto unterstützt keine Seitenblobs, Tabellen oder Warteschlangen.

### <a name="filestorage-accounts"></a>FileStorage-Konten

Ein FileStorage-Konto ist ein spezielles Speicherkonto, das zum Speichern und Erstellen von Premium-Dateifreigaben verwendet wird. Diese Speicherkontoart unterstützt Dateien, aber keine Block-, Anfüge- oder Seitenblobs, Tabellen oder Warteschlangen.

FileStorage-Konten bieten einzigartige leistungsorientierte Merkmale wie IOPS-Bursting. Weitere Informationen zu diesen Merkmalen finden Sie im Abschnitt [Speicherebenen für Dateifreigaben](../files/storage-files-planning.md#storage-tiers) des Planungshandbuchs für Azure Files.

## <a name="naming-storage-accounts"></a>Benennen von Speicherkonten

Beachten Sie bei der Benennung Ihres Speicherkontos folgende Regeln:

- Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.
- Der Name Ihres Speicherkontos muss innerhalb von Azure eindeutig sein. Zwei Speicherkonten können nicht denselben Namen haben.

## <a name="performance-tiers"></a>Leistungsstufen

Je nach dem von Ihnen erstellten Speicherkontotyp können Sie zwischen den Leistungsstufen „Standard“ und „Premium“ wählen. In der folgenden Tabelle ist zusammengefasst, welche Leistungsstufen für welchen Typ von Speicherkonto verfügbar sind.

| Speicherkontotyp | Unterstützte Leistungsstufen |
|--|--|
| Allgemein v2 | Standard, Premium<sup>1</sup> |
| Allgemein v1 | Standard, Premium<sup>1</sup> |
| BlockBlobStorage | Premium |
| FileStorage | Premium |
| BlobStorage | Standard |

<sup>1</sup> Premium-Leistung für Konten vom Typ „Universell V1“ und „Universell V2“ ist nur für Datenträger und Seitenblobs verfügbar. Premium-Leistung für Block- oder Anfügeblobs ist nur für BlockBlobStorage-Konten verfügbar. Premium-Leistung für Dateien ist nur für FileStorage-Konten verfügbar.

### <a name="general-purpose-storage-accounts"></a>Allgemeine Speicherkonten

Allgemeine Speicherkonten können für jede der folgenden Leistungsstufen konfiguriert werden:

- Eine Standard-Leistungsstufe zum Speichern von Blobs, Dateien, Tabellen, Warteschlangen und Azure-VM-Datenträgern. Weitere Informationen zu Skalierbarkeitszielen für Standardspeicherkonten finden Sie unter [Skalierbarkeitsziele für Standardspeicherkonten](scalability-targets-standard-account.md).
- Eine Premium-Leistungsstufe zum Speichern von nicht verwalteten VM-Datenträgern. Microsoft empfiehlt, für virtuelle Azure-Computer verwaltete Datenträger anstelle nicht verwalteter Datenträger zu verwenden. Weitere Informationen zu Skalierbarkeitszielen für die Leistungsstufe „Premium“ finden Sie unter [Skalierbarkeitsziele für Seitenblobspeicher mit Premium-Leistung](../blobs/scalability-targets-premium-page-blobs.md).

### <a name="blockblobstorage-storage-accounts"></a>BlockBlobStorage-Speicherkonten

BlockBlobStorage-Konten bieten die Leistungsebene „Premium“ für das Speichern von Block- und Anfügeblobs. Weitere Informationen finden Sie unter [Skalierbarkeitsziele für Blockblob-Speicherkonten mit Premium-Leistung](../blobs/scalability-targets-premium-block-blobs.md).

### <a name="filestorage-storage-accounts"></a>FileStorage-Speicherkonten

FileStorage-Speicherkonten bieten die Leistungsebene „Premium“ für Azure-Dateifreigaben. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](../files/storage-files-scale-targets.md).

## <a name="access-tiers-for-block-blob-data"></a>Zugriffsebenen für Blockblobdaten

Azure Storage bietet verschiedene Optionen für den Zugriff auf Blockblobdaten basierend auf Nutzungsmustern. Jede Zugriffsebene in Azure Storage ist für ein bestimmtes Muster der Datennutzung optimiert. Indem Sie die richtige Zugriffsebene für Ihre Anforderungen auswählen, können Sie Ihre Blockblobdaten auf möglichst kostengünstige Art und Weise speichern.

Folgende Zugriffsebenen sind verfügbar:

- Die Zugriffsebene **Heiß**. Diese Ebene ist für häufigen Zugriff auf Objekte im Speicherkonto optimiert. Der Zugriff auf Daten auf der Zugriffsebene „Heiß“ ist besonders kostengünstig, dafür liegen aber die Speicherkosten etwas höher. Neue Speicherkonten werden standardmäßig auf dieser Ebene erstellt.
- Die Zugriffsebene **Kalt**. Diese Ebene ist für die Speicherung von großen Datenmengen optimiert, auf die selten zugegriffen wird und die mindestens 30 Tage lang gespeichert werden. Das Speichern von Daten auf der Ebene „Kalt“ ist kostengünstiger, doch kann der Zugriff auf die Daten teurer sein als der Zugriff auf der Ebene „Heiß“.
- Die Zugriffsebene **Archiv**. Diese Ebene ist nur für einzelne Blockblobs verfügbar. Die Zugriffsebene „Archiv“ ist für Daten optimiert, die eine Abrufwartezeit von mehreren Stunden tolerieren und die mindestens 180 Tage lang auf der Ebene „Archiv“ verbleiben. Die Zugriffsebene „Archiv“ ist die kosteneffizienteste Option zum Speichern von Daten. Der Zugriff auf diese Daten ist jedoch teurer als der Zugriff auf Daten auf den Ebenen „Heiß“ oder „Kalt“.

Wenn sich das Nutzungsmuster Ihrer Daten ändert, können Sie jederzeit zwischen diesen Zugriffsebenen wechseln. Weitere Informationen zu Zugriffsebenen finden Sie unter [Azure Blob Storage: Zugriffsebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“](../blobs/storage-blob-storage-tiers.md).

In der folgenden Tabelle wird gezeigt, welche Leistungsstufen für Blobs bei den einzelnen Typen von Speicherkonten verfügbar sind.

| Speicherkontotyp | Unterstützte Zugriffsebenen |
|--|--|
| Allgemein v2 | Heiß, Kalt, Archiv<sup>1</sup> |
| Allgemein v1 | – |
| BlockBlobStorage | – |
| FileStorage | – |
| BlobStorage | Heiß, Kalt, Archiv<sup>1</sup> |

<sup>1</sup> Für die Archivspeicherebene und das Blobebenentiering werden nur Blockblobs unterstützt. Die Ebene „Archiv“ ist nur auf Ebene der einzelnen Blobs verfügbar, nicht auf Ebene des Speicherkontos. Weitere Informationen finden Sie unter [Zugriffsebenen für Azure Blob Storage: „Heiß“, „Kalt“ und „Archiv“](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Das Ändern der Zugriffsebene für ein vorhandenes Speicherkonto oder Blob zieht möglicherweise zusätzliche Gebühren nach sich. Weitere Informationen finden Sie unter [Speicherkontoabrechnung](#storage-account-billing).

## <a name="encryption"></a>Verschlüsselung

Alle Daten in Ihrem Speicherkonto werden auf Dienstseite verschlüsselt. Weitere Informationen zur Verschlüsselung finden Sie unter [Azure Storage Service Encryption für ruhende Daten](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Speicherkontoendpunkte

Ein Speicherkonto bietet einen eindeutigen Namespace für Ihre Daten in Azure. Jedes Objekt, das Sie in Azure speichern, besitzt eine Adresse, die den eindeutigen Namen Ihres Speicherkontos enthält. Die Kombination aus Kontoname und Azure Storage-Dienstendpunkt bildet die Endpunkte für Ihr Speicherkonto.

In der folgenden Tabelle sind die Endpunkte für die einzelnen Azure Storage-Dienste aufgeführt.

| Speicherdienst | Endpunkt |
|--|--|
| Blobspeicher | `https://<storage-account>.blob.core.windows.net` |
| Azure Data Lake Storage Gen2 | `https://<storage-account>.dfs.core.windows.net` |
| Azure Files | `https://<storage-account>.file.core.windows.net` |
| Queue Storage | `https://<storage-account>.queue.core.windows.net` |
| Tabellenspeicher | `https://<storage-account>.table.core.windows.net` |

> [!NOTE]
> Blockblob- und Blockspeicherkonten machen nur den Endpunkt des Blob-Diensts verfügbar.

Erstellen Sie die URL für den Zugriff auf ein Objekt in einem Speicherkonto, indem Sie den Objektstandort im Speicherkonto an den Endpunkt anhängen. Eine Blob-Adresse kann beispielsweise das folgende Format haben: http://*meinspeicherkonto*.blob.core.windows.net/*meincontainer*/*meinblob*.

Sie können Ihr Speicherkonto auch für die Verwendung einer benutzerdefinierten Domäne konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für Ihr Azure Storage-Konto](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Steuern des Zugriffs auf Kontodaten

Standardmäßig sind die Daten in Ihrem Konto nur für Sie als Kontobesitzer verfügbar. Sie können steuern, welche Personen auf Ihre Daten zugreifen dürfen und welche Berechtigungen diese Personen besitzen.

Jede Anforderung, die für Ihr Speicherkonto gesendet wird, muss autorisiert werden. Auf Ebene des Diensts muss die Anforderung einen gültigen *Autorisierungsheader* aufweisen. Dieser Header enthält insbesondere alle Informationen, die der Dienst benötigt, um die Anforderung vor der Ausführung zu überprüfen.

Sie können eine der folgenden Methoden verwenden, um Zugriff auf die Daten in Ihrem Speicherkonto zu gewähren:

- **Azure Active Directory:** Verwenden Sie Anmeldeinformationen von Azure Active Directory (Azure AD), um einen Benutzer, eine Gruppe oder eine andere Identität für den Zugriff auf Blob- und Warteschlangendaten zu authentifizieren. Wenn die Authentifizierung einer Identität erfolgreich war, gibt Azure AD ein Token zurück, das zum Autorisieren der Anforderung bei Azure Blob Storage oder Azure Queue Storage verwendet wird. Weitere Informationen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory](storage-auth-aad.md).
- **Autorisierung mit freigegebenen Schlüsseln**: Verwenden Sie den Zugriffsschlüssel für Ihr Speicherkonto, um eine Verbindungszeichenfolge zu erstellen, die Ihre Anwendung zur Laufzeit für den Zugriff auf Azure Storage verwendet. Die Werte in der Verbindungszeichenfolge werden verwendet, um den *Autorisierungsheader* zu generieren, der an Azure Storage übergeben wird. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](storage-configure-connection-string.md).
- **Shared Access Signature (SAS):** Eine Shared Access Signature (SAS) ist ein Token, das delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto ermöglicht. Das SAS-Token kapselt alle Informationen, die zum Autorisieren einer Zugriffsanforderung für Azure Storage in der URL erforderlich sind. Beim Erstellen eines SAS können Sie die Berechtigungen angeben, die das Token einer Ressource erteilt, sowie den Zeitraum, in dem die Berechtigungen gültig sind. Ein SAS-Token kann mit Azure AD-Anmeldeinformationen oder einem gemeinsam genutzten Schlüssel signiert werden. Weitere Informationen finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](storage-sas-overview.md).

> [!NOTE]
> Die Authentifizierung von Benutzern oder Anwendungen mithilfe von Azure AD-Anmeldeinformationen bietet mehr Sicherheit und Benutzerfreundlichkeit als andere Autorisierungsmethoden. Während Sie weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel mit Ihren Anwendungen verwenden können, macht Azure AD das Speichern Ihrer Kontozugriffsschlüssel mit Ihrem Code überflüssig. Sie können auch weiterhin Shared Access Signatures für zum Gewähren eines differenzierten Zugriffs auf Ressourcen in Ihrem Speicherkonto verwenden. Azure AD bietet jedoch ähnliche Funktionen, bei denen Sie weder SAS-Token verwalten noch sich um das Widerrufen einer gefährdeten SAS kümmern müssen.
>
> Microsoft empfiehlt, nach Möglichkeit die Azure AD-Autorisierung für Ihre Blob- und Warteschlangenanwendungen in Azure Storage zu verwenden.

## <a name="copying-data-into-a-storage-account"></a>Kopieren von Daten in ein Speicherkonto

Microsoft bietet Hilfsprogramme und Bibliotheken für den Import Ihrer Daten von lokalen Speichergeräten oder Drittanbietern von Cloudspeichern. Welche Lösung Sie nutzen, richtet sich nach der Menge an Daten, die Sie übertragen müssen.

Wenn Sie ein Upgrade eines Allgemein v1- oder Blob Storage-Kontos auf ein Allgemein v2-Konto durchführen, werden Ihre Daten automatisch migriert. Microsoft empfiehlt diese Vorgehensweise für das Upgrade Ihres Kontos. Wenn Sie jedoch Daten aus einem Allgemein v1-Konto in ein Blob Storage-Konto verschieben möchten, migrieren Sie die Daten manuell mithilfe der unten beschriebenen Tools und Bibliotheken.

### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Mit AzCopy können Sie Daten aus einem vorhandenen allgemeinen Speicherkonto in ein Blob Storage-Konto kopieren oder Daten von lokalen Speichergeräten hochladen. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](./storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Data Movement-Bibliothek

Die Azure Storage Data Movement-Bibliothek für .NET basiert auf dem Kernframework für die Datenverschiebung, das AzCopy zugrunde liegt. Die Bibliothek ist ähnlich wie AzCopy auf leistungsfähige, zuverlässige und einfache Datenübertragungsvorgänge ausgelegt. Bei Verwendung der Data Movement-Bibliothek können Sie die AzCopy-Funktionen nativ nutzen. Weitere Informationen finden Sie unter [Azure Storage Data Movement-Bibliothek für .NET](https://github.com/Azure/azure-storage-net-data-movement).

### <a name="rest-api-or-client-library"></a>REST-API oder Clientbibliothek

Sie können eine benutzerdefinierte Anwendung erstellen, um Ihre Daten aus einem Speicherkonto vom Typ „Allgemein v1“ in ein Blob Storage-Konto zu migrieren. Verwenden Sie eine der Azure-Clientbibliotheken oder die REST-API für Azure Storage-Dienste. Azure Storage bietet umfassende Clientbibliotheken für mehrere Programmiersprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Weitere Informationen zur Azure Storage REST-API finden Sie unter [Azure Storage-Dienste – REST-API-Referenz](/rest/api/storageservices/).

> [!IMPORTANT]
> Mit clientseitiger Verschlüsselung verschlüsselte Blobs speichern Metadaten im Zusammenhang mit der Verschlüsselung mit dem Blob. Wenn Sie einen mit clientseitiger Verschlüsselung verschlüsselten Blob kopieren, müssen Sie sicherstellen, dass die Blobmetadaten und insbesondere die verschlüsselungsbezogenen Metadaten beim Kopiervorgang erhalten bleiben. Wenn Sie einen Blob ohne die Verschlüsselungsmetadaten kopieren, ist der Blobinhalt nicht mehr abrufbar. Weitere Informationen zu den auf die Verschlüsselung bezogenen Metadaten finden Sie unter [Clientseitige Azure Storage-Verschlüsselung](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="storage-account-billing"></a>Speicherkontoabrechnung

Die Abrechnung für Azure Storage basiert auf der Nutzung Ihres Speicherkontos. Alle Objekte in einem Speicherkonto werden zusammen als Gruppe abgerechnet. Die Speicherkosten werden nach den folgenden Faktoren berechnet:

- **Region** bezieht sich auf die geografische Region, in der sich Ihr Konto befindet.
- **Kontotyp** verweist auf den Typ des Speicherkontos, das Sie verwenden.
- **Zugriffsebene** bezieht sich auf das Datenverwendungsmuster, das Sie für Ihr Konto vom Typ „Allgemein v2“ oder Ihr Blobspeicherkonto angegeben haben.
- **Kapazität** bezieht sich darauf, wie viel von Ihrer Speicherkontozuweisung zum Speichern von Daten verwendet wird.
- Die **Replikation** bestimmt, wie viele Kopien Ihrer Daten jeweils an welchen Standorten unterhalten werden.
- **Transaktionen** beziehen sich auf alle Lese- und Schreibvorgänge in Azure Storage.
- **Datenausgang** bezieht sich alle Daten, die aus einer Azure-Region übertragen werden. Wenn eine Anwendung, die nicht in der gleichen Region ausgeführt wird, auf die Daten in Ihrem Speicherkonto zugreift, fallen Gebühren für den Datenausgang an. Weitere Informationen zur Verwendung von Ressourcengruppen zum Gruppieren von Daten und Diensten in der gleichen Region zur Beschränkung vom Gebühren für den Datenausgang, finden Sie unter [Was ist eine Azure-Ressourcengruppe?](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group).

Die Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) bietet detaillierte Preisinformationen basierend auf Kontotyp, Speicherkapazität, Replikation und Transaktionen. In der [Datenübertragungs-Preisübersicht](https://azure.microsoft.com/pricing/details/data-transfers/) finden Sie detaillierte Preisinformationen für den Datenausgang. Sie können den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) verwenden, um Ihre Kosten zu bestimmen.

[!INCLUDE [cost-management-horizontal](../../../includes/cost-management-horizontal.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Speicherkontos](storage-account-create.md)
- [Erstellen eines Blockblob-Speicherkontos](../blobs/storage-blob-create-account-block-blob.md)
- [Durchführen eines Upgrades auf ein Speicherkonto vom Typ „Allgemein v2“](storage-account-upgrade.md)
- [Wiederherstellen eines gelöschten Speicherkontos](storage-account-recover.md)