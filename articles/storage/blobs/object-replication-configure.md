---
title: Konfigurieren der Objektreplikation (Vorschau)
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie die Objektreplikation so konfigurieren, dass Blockblobs asynchron aus einem Container in einem Speicherkonto in ein anderes kopiert werden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/20/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 6c951d7875086658763243c7c1973f08233f96e0
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83748649"
---
# <a name="configure-object-replication-for-block-blobs-preview"></a>Konfigurieren der Objektreplikation für Blockblobs (Vorschau)

Die Objektreplikation (Vorschau) kopiert Blockblobs asynchron zwischen einem Quellspeicherkonto und einem Zielkonto. Weitere Informationen zur Objektreplikation finden Sie unter [Objektreplikation (Vorschau)](object-replication-overview.md).

Wenn Sie die Objektreplikation konfigurieren, erstellen Sie eine Replikationsrichtlinie, in der das Quellspeicherkonto und das Zielkonto angegeben werden. Eine Replikationsrichtlinie enthält mindestens eine Regel, die einen Quellcontainer und einen Zielcontainer angibt sowie anzeigt, welche Blockblobs im Quellcontainer repliziert werden.

In diesem Artikel wird beschrieben, wie Sie die Objektreplikation für Ihr Speicherkonto über das Azure-Portal, die PowerShell oder die Azure CLI konfigurieren. Sie können auch eine der Azure Storage-Ressourcenanbieter-Clientbibliotheken verwenden, um die Objektreplikation zu konfigurieren.

## <a name="create-a-replication-policy-and-rules"></a>Erstellen einer Replikationsrichtlinie und von Regeln

Bevor Sie die Objektreplikation konfigurieren, erstellen Sie die Quell- und Zielspeicherkonten, wenn diese noch nicht vorhanden sind. Beide Konten müssen Speicherkonten vom Typ „Universell v2“ sein. Weitere Informationen finden Sie unter [Erstellen eines Azure Storage-Kontos](../common/storage-account-create.md).

Achten Sie auch darauf, dass Sie sich für die folgenden Funktionsvorschauversionen registriert haben:

- [Objektreplikation (Vorschau)](object-replication-overview.md)
- [Blobversionsverwaltung (Vorschau)](versioning-overview.md)
- [Unterstützung für Änderungsfeeds in Azure Blob Storage (Vorschau)](storage-blob-change-feed.md)

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Bevor Sie die Objektreplikation im Azure-Portal konfigurieren, erstellen Sie die Quell- und Zielcontainer in ihren jeweiligen Speicherkonten, wenn diese noch nicht vorhanden sind. Aktivieren Sie außerdem die Blobversionsverwaltung und den Änderungsfeed für das Quellkonto, und aktivieren Sie die Blobversionsverwaltung für das Zielkonto.

Führen Sie die folgenden Schritte aus, um eine Replikationsrichtlinie im Azure-Portal zu erstellen:

1. Navigieren Sie im Azure-Portal zum Quellspeicherkonto.
1. Wählen Sie unter **Einstellungen** die Option **Objektreplikation** aus.
1. Wählen Sie **Replikation einrichten** aus.
1. Wählen Sie das Zielabonnement und das Zielspeicherkonto aus.
1. Wählen Sie im Abschnitt **Containerpaare** einen Quellcontainer aus dem Quellkonto und einen Zielcontainer aus dem Zielkonto aus. Sie können bis zu 10 Containerpaare pro Replikationsrichtlinie erstellen.

    Die folgende Abbildung zeigt einen Satz von Replikationsregeln.

    :::image type="content" source="media/object-replication-configure/configure-replication-policy.png" alt-text="Screenshot von Replikationsregeln im Azure-Portal":::

1. Wenn gewünscht, geben Sie einen oder mehrere Filter an, um nur Blobs zu kopieren, die einem Präfixmuster entsprechen. Wenn Sie z. B. das Präfix `b` angeben, werden nur Blobs repliziert, deren Name mit diesem Buchstaben beginnt. Sie können ein virtuelles Verzeichnis als Teil des Präfixes angeben.

    Die folgende Abbildung zeigt Filter, die einschränken, welche Blobs als Teil einer Replikationsregel kopiert werden.

    :::image type="content" source="media/object-replication-configure/configure-replication-copy-prefix.png" alt-text="Screenshot von Filtern für eine Replikationsregel":::

1. Standardmäßig wird der Kopierumfang so festgelegt, dass nur neue Objekte kopiert werden. Um alle Objekte im Container oder Objekte ab einem benutzerdefinierten Datum und Zeitpunkt zu kopieren, wählen Sie den Link **Ändern** aus, und konfigurieren Sie den Kopierumfang für das Containerpaar.

    Die folgende Abbildung zeigt einen benutzerdefinierten Kopierumfang.

    :::image type="content" source="media/object-replication-configure/configure-replication-copy-scope.png" alt-text="Screenshot mit benutzerdefiniertem Kopierumfang für die Objektreplikation":::

1. Wählen Sie **Speichern und anwenden** aus, um die Replikationsrichtlinie zu erstellen und mit der Replikation von Daten zu beginnen.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Um eine Replikationsrichtlinie mit PowerShell zu erstellen, installieren Sie zunächst Version [2.0.1-preview](https://www.powershellgallery.com/packages/Az.Storage/2.0.1-preview) des PowerShell-Moduls „Az.Storage“. Führen Sie zum Installieren des Vorschaumoduls folgende Schritte aus:

1. Deinstallieren Sie alle früheren Installationen von Azure PowerShell mit der Einstellung **Apps & Features** (unter **Einstellungen**) aus Windows.

1. Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie den folgenden Befehl aus, um die neueste Version zu installieren:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

    Schließen Sie nach dem Installieren von PowerShellGet das PowerShell-Fenster, und öffnen Sie es dann erneut.

1. Installieren Sie die neueste Version von Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Installieren Sie das Az.Storage-Vorschaumodul:

    ```powershell
    Install-Module Az.Storage -Repository PSGallery -RequiredVersion 2.0.1-preview -AllowPrerelease -AllowClobber -Force
    ```

Weitere Informationen zum Installieren von Azure PowerShell finden Sie unter [Installieren von Azure PowerShell mit PowerShellGet](/powershell/azure/install-az-ps).

Im folgenden Beispiel wird gezeigt, wie Sie eine Replikationsrichtlinie für das Quell- und Zielkonto erstellen. Denken Sie daran, die Werte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set variables.
$rgName = "<resource-group>"
$srcAccountName = "<source-storage-account>"
$destAccountName = "<destination-storage-account>"
$srcContainerName1 = "source-container1"
$destContainerName1 = "dest-container1"
$srcContainerName2 = "source-container2"
$destContainerName2 = "dest-container2"

# Enable blob versioning and change feed on the source account.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName `
    -EnableChangeFeed $true `
    -IsVersioningEnabled $true

# Enable blob versioning on the destination account.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName `
    -IsVersioningEnabled $true

# List the service properties for both accounts.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName

# Create containers in the source and destination accounts.
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $srcAccountName |
    New-AzStorageContainer $srcContainerName1
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $destAccountName |
    New-AzStorageContainer $destContainerName1
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $srcAccountName |
    New-AzStorageContainer $srcContainerName2
Get-AzStorageAccount -ResourceGroupName $rgname -StorageAccountName $destAccountName |
    New-AzStorageContainer $destContainerName2

# Define replication rules for each container.
$rule1 = New-AzStorageObjectReplicationPolicyRule -SourceContainer $srcContainerName1 `
    -DestinationContainer $destContainerName1 `
    -PrefixMatch b
$rule2 = New-AzStorageObjectReplicationPolicyRule -SourceContainer $srcContainerName2 `
    -DestinationContainer $destContainerName2  `
    -MinCreationTime 2020-05-10T00:00:00Z

# Create the replication policy on the destination account.
$destPolicy = Set-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName `
    -PolicyId default `
    -SourceAccount $srcAccountName `
    -Rule $rule1,$rule2

# Create the same policy on the source account.
Set-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName `
    -InputObject $destPolicy
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Zum Erstellen einer Replikationsrichtlinie mit Azure CLI installieren Sie zunächst die Vorschauerweiterung für Azure Storage.

```azurecli
az extension add -n storage-or-preview
```

Als Nächstes melden Sie sich mit Ihren Azure-Anmeldeinformationen an:

```azurecli
az login
```

Aktivieren Sie die Blobversionsverwaltung für das Quell- und Zielspeicherkonto, und aktivieren Sie den Änderungsfeed für das Quellkonto. Denken Sie daran, die Werte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli
az storage blob service-properties update --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --enable-versioning

az storage blob service-properties update --resource-group <resource-group> \
    --account-name <source-storage-account> \
    --enable-change-feed

az storage blob service-properties update --resource-group <resource-group> \
    --account-name <dest-storage-account> \
    --enable-versioning
```

Erstellen Sie die Quell- und Zielcontainer in ihren jeweiligen Speicherkonten.

```azurecli
az storage container create --account-name <source-storage-account> --name source-container3 --auth-mode login
az storage container create --account-name <source-storage-account> --name source-container4 --auth-mode login

az storage container create --account-name <dest-storage-account> --name source-container3 --auth-mode login
az storage container create --account-name <dest-storage-account> --name source-container4 --auth-mode login
```

Erstellen Sie eine neue Replikationsrichtlinie und die zugehörigen Regeln für das Zielkonto.

```azurecli
az storage account or-policy create --account-name <dest-storage-account> \
    --resource-group <resource-group> \
    --source-account <source-storage-account> \
    --destination-account <dest-storage-account> \
    --source-container source-container3 \
    --destination-container dest-container3 \
    --min-creation-time '2020-05-10T00:00:00Z' \
    --prefix-match a

az storage account or-policy rule add --account-name <dest-storage-account> \
    --destination-container dest-container4 \
    --policy-id <policy-id> \
    --resource-group <resource-group> \
    --source-container source-container4 \
    --prefix-match b
```

Erstellen Sie die Richtlinie für das Quellkonto mithilfe der Richtlinien-ID.

```azurecli
az storage account or-policy show --resource-group <resource-group> \
    --name <dest-storage-account> \
    --policy-id <policy-id> |
    --az storage account or-policy create --resource-group <resource-group> \
    --name <source-storage-account> \
    --policy "@-"
```

---

## <a name="remove-a-replication-policy"></a>Entfernen einer Replikationsrichtlinie

Um eine Replikationsrichtlinie und ihre zugehörigen Regeln zu entfernen, verwenden Sie das Azure-Portal, die PowerShell oder die CLI.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um eine Replikationsrichtlinie im Azure-Portal zu entfernen:

1. Navigieren Sie im Azure-Portal zum Quellspeicherkonto.
1. Wählen Sie unter **Einstellungen** die Option **Objektreplikation** aus.
1. Klicken Sie auf die Schaltfläche **Mehr** neben dem Richtliniennamen.
1. Wählen Sie **Regeln löschen** aus.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Um eine Replikationsrichtlinie zu entfernen, löschen Sie die Richtlinie sowohl im Quell- als auch im Zielkonto. Wenn Sie die Richtlinie löschen, werden auch alle Regeln gelöscht, die ihr zugeordnet sind.

```powershell
Get-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $destAccountName |
    Remove-AzStorageObjectReplicationPolicy
Get-AzStorageObjectReplicationPolicy -ResourceGroupName $rgname `
    -StorageAccountName $srcAccountName |
    Remove-AzStorageObjectReplicationPolicy
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um eine Replikationsrichtlinie zu entfernen, löschen Sie die Richtlinie sowohl im Quell- als auch im Zielkonto. Wenn Sie die Richtlinie löschen, werden auch alle Regeln gelöscht, die ihr zugeordnet sind.

```azurecli
az storage account or-policy delete \
    --policy-id $policyid \
    --account-name <source-storage-account> \
    --resource-group <resource-group>

az storage account or-policy delete \
    --policy-id $policyid \
    --account-name <dest-storage-account> \
    --resource-group <resource-group>
```

---

## <a name="next-steps"></a>Nächste Schritte

- [Objektreplikation: Übersicht (Vorschau)](object-replication-overview.md)