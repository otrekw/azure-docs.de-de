---
title: Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)
titleSuffix: Azure Storage
description: Point-in-Time-Wiederherstellung für Blockblobs bietet Schutz vor versehentlichem Löschen oder Beschädigungen, da Sie ein Speicherkonto zu einem bestimmten Zeitpunkt in seinem vorherigen Zustand wiederherstellen können.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 09/11/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: references_regions, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 1187b01fa623264055edecf21ea5c9d35d59a152
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068301"
---
# <a name="point-in-time-restore-for-block-blobs-preview"></a>Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)

Point-in-Time-Wiederherstellung bietet Schutz vor versehentlichem Löschen oder Beschädigungen, da Sie Blockblobdaten in einem früheren Zustand wiederherstellen können. Point-in-Time-Wiederherstellung ist in Szenarien nützlich, in denen ein Benutzer oder eine Anwendung versehentlich Daten löscht oder wenn ein Anwendungsfehler Daten beschädigt. Point-in-Time-Wiederherstellung ermöglicht auch Testszenarien, bei denen ein Dataset in einen bekannten Zustand zurückgesetzt werden muss, bevor weitere Tests ausgeführt werden.

Informationen zum Aktivieren von Point-in-Time-Wiederherstellung für ein Speicherkonto finden Sie unter [Aktivieren und Verwalten von Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)](point-in-time-restore-manage.md).

## <a name="how-point-in-time-restore-works"></a>Funktionsweise der Point-in-Time-Wiederherstellung

Zum Aktivieren von Point-in-Time-Wiederherstellung erstellen Sie eine Verwaltungsrichtlinie für das Speicherkonto und geben eine Beibehaltungsdauer an. Während der Beibehaltungsdauer können Sie Blockblobs aus dem aktuellen Zustand in einem Zustand zu einem früheren Zeitpunkt wiederherstellen.

Um eine Point-in-Time-Wiederherstellung zu initiieren, rufen Sie den Vorgang [Restore Blob Ranges](/rest/api/storagerp/storageaccounts/restoreblobranges) auf und geben einen Wiederherstellungspunkt in UTC-Zeit an. Sie können lexikografische Bereiche von Container- und Blobnamen, die wiederhergestellt werden sollen, angeben oder den Bereich weglassen, um alle Container im Speicherkonto wiederherzustellen. Pro Wiederherstellungsvorgang werden bis zu 10 lexikografische Bereiche unterstützt.

Azure Storage analysiert alle Änderungen, die an den angegebenen Blobs zwischen dem angeforderten Wiederherstellungspunkt (angegeben in UTC-Zeit) und dem aktuellen Zeitpunkt vorgenommen wurden. Der Wiederherstellungsvorgang ist atomisch, sodass er entweder bei der Wiederherstellung aller Änderungen vollständig erfolgreich ist oder fehlschlägt. Wenn Blobs vorhanden sind, die nicht wiederhergestellt werden können, schlägt der Vorgang fehl, und Lese- und Schreibvorgänge für die betroffenen Container werden fortgesetzt.

Nur ein Wiederherstellungsvorgang kann für ein Speicherkonto gleichzeitig ausgeführt werden. Ein Wiederherstellungsvorgang kann nicht abgebrochen werden, sobald er ausgeführt wird. Es kann jedoch ein zweiter Wiederherstellungsvorgang ausgeführt werden, um den ersten Vorgang rückgängig zu machen.

Der Vorgang **Restore Blob Ranges** gibt eine Wiederherstellungs-ID zurück, die den Vorgang eindeutig identifiziert. Um den Status einer Point-in-Time-Wiederherstellung zu überprüfen, wenden Sie den Vorgang **Get Restore Status** mit der Wiederherstellungs-ID an, die vom Vorgang **Restore Blob Ranges** zurückgegeben wird.

Beachten Sie die folgenden Einschränkungen für Wiederherstellungsvorgänge:

- Ein Block, der über [Put Block](/rest/api/storageservices/put-block) oder [Put Block from URL](/rest/api/storageservices/put-block-from-url) hochgeladen, aber nicht über [Put Block List](/rest/api/storageservices/put-block-list) committet wurde, ist nicht Teil eines Blobs und wird daher nicht als Teil eines Wiederherstellungsvorgangs wiederhergestellt.
- Ein Blob mit einer aktiven Leasedauer kann nicht wiederhergestellt werden. Wenn ein Blob mit einer aktiven Leasedauer in den Bereich der wiederherzustellenden Blobs eingeschlossen ist, schlägt der Wiederherstellungsvorgang atomisch fehl.
- Momentaufnahmen werden nicht im Rahmen eines Wiederherstellungsvorgangs erstellt oder gelöscht. Nur das Basisblob wird in seinem vorherigen Zustand wiederhergestellt.
- Wenn ein Blob innerhalb des Zeitraums zwischen dem aktuellen Moment und dem Wiederherstellungspunkt zwischen den heißen und kalten Speicherebenen verschoben wurde, wird das Blob auf der vorherigen Ebene wiederhergestellt. Ein Blob, das in die Archivebene verschoben wurde, wird jedoch nicht wiederhergestellt.

> [!IMPORTANT]
> Wenn Sie einen Wiederherstellungsvorgang ausführen, blockiert Azure Storage Datenvorgänge für die Blobs in den wiederhergestellten Bereichen für die Dauer des Vorgangs. Lese-, Schreib- und Löschvorgänge werden am primären Speicherort blockiert. Aus diesem Grund werden Vorgänge wie das Auflisten von Containern im Azure-Portal während des Wiederherstellungsvorgangs möglicherweise nicht erwartungsgemäß ausgeführt.
>
> Lesevorgänge aus dem sekundären Speicherort können während des Wiederherstellungsvorgangs fortgesetzt werden, wenn das Speicherkonto georepliziert wird.

> [!CAUTION]
> Point-in-Time-Wiederherstellung unterstützt nur Wiederherstellungsvorgänge für Blockblobs. Vorgänge für Container können nicht wiederhergestellt werden. Wenn Sie einen Container aus dem Speicherkonto löschen, indem Sie den Vorgang [Delete Container](/rest/api/storageservices/delete-container) während der Vorschau der Point-in-Time-Wiederherstellung aufrufen, kann dieser Container nicht mit einem Wiederherstellungsvorgang wiederhergestellt werden. Löschen Sie während der Vorschau die einzelnen Blobs, anstatt einen Container zu löschen, wenn Sie sie möglicherweise wiederherstellen möchten.

### <a name="prerequisites-for-point-in-time-restore"></a>Voraussetzungen für Point-in-Time-Wiederherstellung

Point-in-Time-Wiederherstellung erfordert, dass die folgenden Azure Storage-Features aktiviert sind:

- [Vorläufiges Löschen](soft-delete-overview.md)
- [Änderungsfeed (Vorschau)](storage-blob-change-feed.md)
- [Blobversionsverwaltung](versioning-overview.md)

Aktivieren Sie diese Features für das Speicherkonto, bevor Sie Point-in-Time-Wiederherstellung aktivieren. Stellen Sie sicher, dass Sie sich für die Vorschauversionen von „Änderungsfeed“ und „Blobversionsverwaltung“ registrieren, bevor Sie diese aktivieren.

### <a name="retention-period-for-point-in-time-restore"></a>Beibehaltungsdauer für Point-in-Time-Wiederherstellung

Wenn Sie Point-in-Time-Wiederherstellung für ein Speicherkonto aktivieren, geben Sie eine Beibehaltungsdauer an. Blockblobs in Ihrem Speicherkonto können während der Beibehaltungsdauer wieder hergestellt werden.

Die Beibehaltungsdauer beginnt, wenn Sie Point-in-Time-Wiederherstellung aktivieren. Beachten Sie, dass Sie Blobs nicht in einem Zustand vor Beginn der Beibehaltungsdauer wiederherstellen können. Wenn Sie z. B. Point-in-Time-Wiederherstellung am 1. Mai mit einer Beibehaltungsdauer von 30 Tagen aktiviert haben, können Sie am 15. Mai maximal 15 Tage wiederherstellen. Am 1. Juni können Sie Daten zwischen 1 Tag und 30 Tagen wiederherstellen.

Die Beibehaltungsdauer für Point-in-Time-Wiederherstellung muss mindestens einen Tag weniger als die für vorläufiges Löschen angegebene Beibehaltungsdauer betragen. Wenn die Beibehaltungsdauer für vorläufiges Löschen z. B. auf 7 Tage festgelegt ist, kann die Beibehaltungsdauer für Point-in-Time-Wiederherstellung zwischen 1 Tag und 6 Tagen liegen.

### <a name="permissions-for-point-in-time-restore"></a>Berechtigungen für Point-in-Time-Wiederherstellung

Um einen Wiederherstellungsvorgang zu initiieren, muss ein Client über Schreibberechtigungen für alle Container im Speicherkonto verfügen. Um Berechtigungen zum Autorisieren eines Wiederherstellungsvorgangs mit Azure Active Directory (Azure AD) zu erteilen, weisen Sie dem Sicherheitsprinzipal auf der Ebene des Speicherkontos, der Ressourcengruppe oder des Abonnements die Rolle **Speicherkontomitwirkender** zu.

## <a name="about-the-preview"></a>Informationen zur Vorschau

Point-in-Time-Wiederherstellung wird nur für Speicherkonten vom Typ „Universell V2“ unterstützt. Nur Daten der heißen und kalten Zugriffsebene können mit Point-in-Time-Wiederherstellung wiederhergestellt werden.

Die folgenden Regionen unterstützen Point-in-Time-Wiederherstellung in der Vorschauversion:

- Kanada, Mitte
- Kanada, Osten
- Frankreich, Mitte

Für die Vorschau gelten die folgenden Einschränkungen:

- Das Wiederherstellen von Premium-Blockblobs wird nicht unterstützt.
- Das Wiederherstellen von Blobs in der Archivebene wird nicht unterstützt. Wenn ein Blob aus der heißen Speicherebene z. B. vor zwei Tagen in die Archivebene verschoben wurde und ein Wiederherstellungsvorgang für einen Punkt vor drei Tagen erfolgt, wird das Blob nicht in der heißen Zugriffsebene wiederhergestellt.
- Die Wiederherstellung flachen und hierarchischen Namespaces von Azure Data Lake Storage Gen2 wird nicht unterstützt.
- Das Wiederherstellen von Speicherkonten kundenseitig bereitgestellten Schlüsseln wird nicht unterstützt.

> [!IMPORTANT]
> Die Vorschau von Point-in-Time-Wiederherstellung ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind derzeit nicht verfügbar.

### <a name="register-for-the-preview"></a>Registrieren für die Vorschau

Wenn Sie sich für die Vorschau registrieren möchten, führen Sie die folgenden Befehle aus:

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# Register for the point-in-time restore preview
Register-AzProviderFeature -FeatureName RestoreBlobRanges -ProviderNamespace Microsoft.Storage

# Register for change feed (preview)
Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage

# Register for Blob versioning
Register-AzProviderFeature -FeatureName Versioning -ProviderNamespace Microsoft.Storage

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az feature register --namespace Microsoft.Storage --name RestoreBlobRanges
az feature register --namespace Microsoft.Storage --name Changefeed
az feature register --namespace Microsoft.Storage --name Versioning
az provider register --namespace 'Microsoft.Storage'
```

---

### <a name="check-registration-status"></a>Überprüfen des Registrierungsstatus

Die Registrierung für die Point-in-Time-Wiederherstellung erfolgt automatisch und sollte weniger als 10 Minuten dauern. Führen Sie die folgenden Befehle aus, um den Status Ihrer Registrierung zu überprüfen:

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName RestoreBlobRanges

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Changefeed

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/RestoreBlobRanges')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Changefeed')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Versioning')].{Name:name,State:properties.state}"
```

---

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Die Abrechnung für Point-in-Time-Wiederherstellung hängt von der Menge der für die Ausführung des Wiederherstellungsvorgangs verarbeiteten Daten ab. Die Menge der verarbeiteten Daten basiert auf der Anzahl von Änderungen, die zwischen dem Wiederherstellungspunkt und dem aktuellen Zeitpunkt aufgetreten sind. Geht man beispielsweise von einer relativ konstanten Änderungsrate für Blockblobdaten in einem Speicherkonto aus, würde ein Wiederherstellungsvorgang, der 1 Tag in der Zeit zurückliegt, ein Zehntel einer Wiederherstellung kosten, die 10 Tage in der Zeit zurückliegt.

Wenn Sie die Kosten für einen Wiederherstellungsvorgang schätzen möchten, überprüfen Sie das Änderungsfeedprotokoll, um die Menge der Daten zu schätzen, die während des Wiederherstellungszeitraums geändert wurden. Wenn die Beibehaltungsdauer für den Änderungsfeed beispielsweise 30 Tage und die Größe des Änderungsfeeds 10 MB beträgt, würde die Wiederherstellung bis zu einem Zeitpunkt von 10 Tagen in der Vergangegheit ungefähr ein Drittel des Preises für ein LRS-Konto in dieser Region betragen. Die Wiederherstellung bis zu einem Punkt, der 27 Tage früher liegt, würde ungefähr neun Zehntel des aufgeführten Preises kosten.

Weitere Informationen zu den Preisen für Point-in-Time-Wiederherstellung finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="ask-questions-or-provide-feedback"></a>Fragen stellen oder Feedback geben

Wenn Sie Fragen zur Vorschau der Point-in-Time-Wiederherstellung haben oder Feedback geben möchten, wenden Sie sich unter pitrdiscussion@microsoft.com an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren und Verwalten der Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)](point-in-time-restore-manage.md)
- [Unterstützung für Änderungsfeeds in Azure Blob Storage (Vorschau)](storage-blob-change-feed.md)
- [Aktivieren von „Vorläufiges Löschen“ für Blobs](soft-delete-enable.md)
- [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)
