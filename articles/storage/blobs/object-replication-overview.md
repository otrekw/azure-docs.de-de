---
title: 'Objektreplikation: Übersicht (Vorschau)'
titleSuffix: Azure Storage
description: Die Objektreplikation (Vorschau) kopiert Blockblobs asynchron zwischen einem Quellspeicherkonto und einem Zielkonto. Mithilfe der Objektreplikation können Sie die Wartezeit bei Leseanforderungen minimieren, die Effizienz für Computeworkloads erhöhen, die Datenverteilung optimieren und die Kosten minimieren.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurecli
ms.openlocfilehash: 097b3c71b01a8ad0e930d7aa0d7be46a1e835e1a
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495164"
---
# <a name="object-replication-for-block-blobs-preview"></a>Objektreplikation für Blockblobs (Vorschau)

Die Objektreplikation (Vorschau) kopiert Blockblobs asynchron zwischen einem Quellspeicherkonto und einem Zielkonto. Folgende Szenarien werden unter anderem von der Objektreplikation unterstützt:

- **Minimieren der Wartezeit.** Die Objektreplikation kann die Wartezeit für Leseanforderungen verringern, indem Clients die Nutzung von Daten aus einer Region ermöglicht wird, die sich in größerer physischer Nähe befindet.
- **Erhöhen der Effizienz für Computeworkloads.** Bei der Objektreplikation können Computeworkloads dieselben Sätze von Blockblobs in verschiedenen Regionen verarbeiten.
- **Optimieren der Datenverteilung.** Sie können Daten an einem einzigen Standort verarbeiten bzw. analysieren und dann nur die Ergebnisse in zusätzliche Regionen replizieren.
- **Optimieren der Kosten.** Nachdem Ihre Daten repliziert wurden, können Sie die Kosten senken, indem Sie sie mithilfe von Lebenszyklus-Verwaltungsrichtlinien auf die Archivebene verschieben.

Das folgende Diagramm zeigt, wie die Objektreplikation Blockblobs aus einem Quellspeicherkonto in einer Region in Zielkonten in zwei verschiedenen Regionen repliziert.

:::image type="content" source="media/object-replication-overview/object-replication-diagram.svg" alt-text="Diagramm zur Veranschaulichung der Funktionsweise der Objektreplikation":::

Informationen zum Konfigurieren der Objektreplikation finden Sie unter [Konfigurieren der Objektreplikation (Vorschau)](object-replication-configure.md).

## <a name="object-replication-policies-and-rules"></a>Objektreplikationsrichtlinien und -regeln

Wenn Sie die Objektreplikation konfigurieren, erstellen Sie eine Replikationsrichtlinie, in der das Quellspeicherkonto und das Zielkonto angegeben werden. Eine Replikationsrichtlinie enthält mindestens eine Regel, die einen Quellcontainer und einen Zielcontainer angibt sowie anzeigt, welche Blockblobs im Quellcontainer repliziert werden.

Nachdem Sie die Objektreplikation konfiguriert haben, überprüft Azure Storage den Änderungsfeed für das Quellkonto regelmäßig und repliziert asynchron alle Schreib- oder Löschvorgänge in das Zielkonto. Die Replikationswartezeit hängt von der Größe des Blockblobs ab, das repliziert wird.

> [!IMPORTANT]
> Da Blockblobdaten asynchron repliziert werden, sind das Quell- und das Zielkonto nicht sofort synchron. Zurzeit gibt es keine SLA darüber, wie lange es dauert, Daten in das Zielkonto zu replizieren.

### <a name="replications-policies"></a>Replikationsrichtlinien

Wenn Sie die Objektreplikation konfigurieren, wird über den Azure Storage-Ressourcenanbieter eine Replikationsrichtlinie sowohl für das Quell- als auch für das Zielkonto erstellt. Die Replikationsrichtlinie wird durch eine Richtlinien-ID identifiziert. Die Richtlinie für das Quell- und das Zielkonto muss dieselbe Richtlinien-ID aufweisen, damit die Replikation stattfinden kann.

Ein Speicherkonto kann als Quellkonto für bis zu zwei Zielkonten fungieren. Und ein Zielkonto darf nicht mehr als zwei Quellkonten aufweisen. Quell- und Zielkonten dürfen sich in unterschiedlichen Regionen befinden. Sie können gesonderte Replikationsrichtlinien konfigurieren, um Daten in jedes der Zielkonten zu replizieren.

### <a name="replication-rules"></a>Replikationsregeln

Replikationsregeln geben an, wie Azure Storage Blobs aus einem Quellcontainer in einen Zielcontainer repliziert. Sie können bis zu 10 Replikationsregeln für jede Replikationsrichtlinie angeben. Jede Regel definiert einen einzelnen Quell- und Zielcontainer, und jeder Quell- und Zielcontainer kann nur in einer Regel verwendet werden.

Beim Erstellen einer Replikationsregel werden standardmäßig nur neue Blockblobs kopiert, die anschließend dem Quellcontainer hinzugefügt werden. Sie können auch angeben, dass sowohl neue als auch vorhandene Blockblobs kopiert werden, oder Sie können einen benutzerdefinierten Kopierumfang definieren, der Blockblobs kopiert, die ab einem bestimmten Zeitpunkt erstellt wurden.

Sie können ferner einen oder mehrere Filter als Teil einer Replikationsregel angeben, um Blockblobs anhand eines Präfixes zu filtern. Wenn Sie ein Präfix angeben, werden nur Blobs in den Zielcontainer kopiert, die mit diesem Präfix im Quellcontainer übereinstimmen.

Die Quell- und Zielcontainer müssen beide vorhanden sein, bevor Sie sie in einer Regel angeben können. Nachdem Sie die Replikationsrichtlinie erstellt haben, wird der Zielcontainer schreibgeschützt. Alle Versuche, in den Zielcontainer zu schreiben, schlagen mit dem Fehlercode 409 (Konflikt) fehl. Sie können jedoch den Vorgang [Blobebene festlegen](/rest/api/storageservices/set-blob-tier) für einen Blob im Zielcontainer aufrufen, um ihn auf die Archivebene zu verschieben. Weitere Informationen zur Archivebene finden Sie unter [Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md#archive-access-tier).

## <a name="about-the-preview"></a>Informationen zur Vorschau

Die Objektreplikation wird nur für Speicherkonten vom Typ „Universell V2“ unterstützt. Die Objektreplikation ist in den folgenden Regionen als Vorschauversion verfügbar:

- Frankreich, Mitte
- Kanada, Osten
- Kanada, Mitte
- USA (Ost 2)
- USA, Mitte

Sowohl das Quell- als auch das Zielkonto müssen sich in einer dieser Regionen befinden, um die Objektreplikation verwenden zu können. Die Konten können sich in zwei verschiedenen Regionen befinden.

Während der Vorschauphase fallen keine zusätzlichen Kosten für die Replikation von Daten zwischen Speicherkonten an.

> [!IMPORTANT]
> Die Vorschauversion der Objektreplikation ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind derzeit nicht verfügbar.

### <a name="prerequisites-for-object-replication"></a>Voraussetzungen für die Objektreplikation

Die Objektreplikation erfordert, dass die folgenden Azure Storage-Funktionen aktiviert sind: 
- [Änderungsfeed](storage-blob-change-feed.md)
- [Versionsverwaltung](versioning-overview.md)

Aktivieren Sie vor dem Konfigurieren der Objektreplikation die erforderlichen Komponenten. Der Änderungsfeed muss für das Quellkonto aktiviert sein, und die Blobversionsverwaltung muss für das Quell- und das Zielspeicherkonto aktiviert sein. Weitere Informationen zum Aktivieren dieser Funktionen finden Sie in den folgenden Artikeln:

- [Aktivieren und Deaktivieren des Änderungsfeeds](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)

Stellen Sie sicher, dass Sie sich für die Vorschauversionen von „Änderungsfeed“ und „Blobversionsverwaltung“ registrieren, bevor Sie diese aktivieren.

Durch das Aktivieren des Änderungsfeeds und der Blobversionsverwaltung können zusätzliche Kosten entstehen. Weitere Informationen finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

### <a name="register-for-the-preview"></a>Registrieren für die Vorschau

Sie können sich für die Vorschauversion der Objektreplikation mithilfe von PowerShell oder Azure CLI registrieren. Stellen Sie sicher, dass Sie sich außerdem für die Vorschauversionen von „Änderungsfeed“ und „Blobversionsverwaltung“ registrieren, wenn dies noch nicht geschehen sein sollte.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Um sich für die Vorschau mithilfe von PowerShell zu registrieren, führen Sie die folgenden Befehle aus:

```powershell
# Register for the object replication preview
Register-AzProviderFeature -FeatureName AllowObjectReplication -ProviderNamespace Microsoft.Storage

# Register for change feed (preview)
Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage

# Register for blob versioning (preview)
Register-AzProviderFeature -FeatureName Versioning -ProviderNamespace Microsoft.Storage

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um sich für die Vorschau mithilfe von Azure CLI zu registrieren, führen Sie die folgenden Befehle aus:

```azurecli
az feature register --namespace Microsoft.Storage --name AllowObjectReplication
az feature register --namespace Microsoft.Storage --name Changefeed
az feature register --namespace Microsoft.Storage --name Versioning
az provider register --namespace 'Microsoft.Storage'
```

---

### <a name="check-registration-status"></a>Überprüfen des Registrierungsstatus

Sie können den Status Ihrer Registrierungsanforderungen mithilfe der PowerShell oder der Azure CLI überprüfen.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Führen Sie die folgenden Befehle aus, um den Status Ihrer Registrierungsanforderungen mithilfe der PowerShell zu überprüfen:

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowObjectReplication

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Changefeed

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Führen Sie die folgenden Befehle aus, um den Status Ihrer Registrierungsanforderungen mithilfe der Azure CLI zu überprüfen:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/AllowObjectReplication')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Changefeed')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Versioning')].{Name:name,State:properties.state}"
```

---

## <a name="ask-questions-or-provide-feedback"></a>Fragen stellen oder Feedback geben

Wenn Sie Fragen zur Vorschauversion der Objektreplikation haben oder Feedback geben möchten, wenden Sie sich unter AzureStorageFeedback@microsoft.com an Microsoft. Ideen und Vorschläge zu Azure Storage sind immer willkommen im [Azure Storage-Feedbackforum](https://feedback.azure.com/forums/217298-storage).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Objektreplikation (Vorschau)](object-replication-configure.md)
- [Unterstützung für Änderungsfeeds in Azure Blob Storage (Vorschau)](storage-blob-change-feed.md)
- [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)
