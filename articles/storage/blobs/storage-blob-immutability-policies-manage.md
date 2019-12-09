---
title: Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher – Azure Storage
description: Hier erfahren Sie, wie Sie mithilfe der WORM-Unterstützung (Write Once, Read Many – Einmal schreiben, oft lesen) für Blobspeicher (Objektspeicher) Daten in einem nicht löschbaren und nicht änderbaren Zustand für einen angegebenen Zeitraum speichern können.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 79d7454722900eb1d9d6280e35313ef2f4a5cd54
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74556079"
---
# <a name="set-and-manage-immutability-policies-for-blob-storage"></a>Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher

Unveränderlicher Speicher für Azure-Blobspeicher ermöglicht es Benutzern, unternehmenskritische Datenobjekte im WORM-Zustand (Write Once, Read Many – Einmal schreiben, oft lesen) zu speichern. In diesem Zustand sind die Daten für einen vom Benutzer angegebenen Zeitraum nicht löschbar und nicht änderbar. Während des Aufbewahrungszeitraums können Blobs erstellt und gelesen, aber nicht geändert oder gelöscht werden. Unveränderlicher Speicher steht für universelle v2-Konten und für Blobspeicherkonten in allen Azure-Regionen zur Verfügung.

In diesem Artikel wird das Festlegen und Verwalten von Unveränderlichkeitsrichtlinien und gesetzlichen Aufbewahrungspflichten für Daten im Blobspeicher über das Azure-Portal, PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI) erläutert. Weitere Informationen zu unveränderlichem Speicher finden Sie unter [Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher](storage-blob-immutable-storage.md).

## <a name="set-retention-policies-and-legal-holds"></a>Festlegen von Aufbewahrungsrichtlinien und gesetzlichen Aufbewahrungspflichten

### <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. Erstellen Sie einen neuen Container, oder wählen Sie einen vorhandenen Container aus, um die Blobs zu speichern, die im unveränderlichen Zustand aufbewahrt werden müssen. Der Container muss sich in einem universellen v2-Konto oder einem Blobspeicherkonto befinden.

2. Wählen Sie in den Containereinstellungen den Befehl **Zugriffsrichtlinie** aus. Wählen Sie dann unter **Unveränderlicher Blobspeicher** die Option **Richtlinie hinzufügen** aus.

    ![Containereinstellungen im Portal](media/storage-blob-immutability-policies-manage/portal-image-1.png)

3. Wählen Sie im Dropdownmenü die Option **Zeitbasierte Aufbewahrung** aus, um die zeitbasierte Aufbewahrung zu aktivieren.

    ![„Zeitbasierte Aufbewahrung“ unter „Richtlinientyp“ ausgewählt](media/storage-blob-immutability-policies-manage/portal-image-2.png)

4. Geben Sie den Aufbewahrungszeitraum in Tagen an (zulässige Werte sind „1“ Tag bis „146.000“ Tage).

    ![Feld „Aufbewahrungszeitraum ändern in:“](media/storage-blob-immutability-policies-manage/portal-image-5-retention-interval.png)

    Der ursprüngliche Zustand der Richtlinie lautet „Entsperrt“, damit Sie das Feature testen und Änderungen an der Richtlinie vornehmen können, bevor Sie das Sperren durchführen. Das Sperren der Richtlinie ist entscheidend für die Einhaltung von Vorschriften wie SEC 17a-4.

5. Sperren Sie die Richtlinie. Wenn Sie mit der rechten Maustaste auf die Auslassungspunkte ( **...** ) klicken, wird das folgende Menü mit zusätzlichen Aktionen angezeigt:

    ![„Richtlinie sperren“ im Menü](media/storage-blob-immutability-policies-manage/portal-image-4-lock-policy.png)

6. Wählen Sie **Richtlinie sperren** aus, und bestätigen Sie die Sperre. Die Richtlinie ist jetzt gesperrt und kann nicht gelöscht werden. Es sind lediglich Verlängerungen des Aufbewahrungszeitraums zulässig. Das Löschen und außer Kraft setzen von Blobs ist nicht erlaubt. 

    ![Bestätigen von „Richtlinie sperren“ im Menü](media/storage-blob-immutability-policies-manage/portal-image-5-lock-policy.png)

7. Zum Aktivieren der gesetzlichen Aufbewahrungspflichten wählen Sie **Richtlinie hinzufügen** aus. Wählen Sie im Dropdownmenü die Option **Gesetzliche Aufbewahrungspflicht**.

    ![„Gesetzliche Aufbewahrungspflicht“ im Menü unter „Richtlinientyp“](media/storage-blob-immutability-policies-manage/portal-image-legal-hold-selection-7.png)

8. Erstellen Sie einen Zeitraum für die gesetzliche Aufbewahrungspflicht mit mindestens einem Tag.

    ![Feld „Tagname“ unter dem Richtlinientyp](media/storage-blob-immutability-policies-manage/portal-image-set-legal-hold-tags.png)

9. Entfernen Sie einfach das entsprechende angewendete ID-Tag, um eine gesetzliche Aufbewahrungspflicht zu löschen.

### <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Die Funktion ist in den folgenden Befehlsgruppen enthalten: `az storage container immutability-policy` und `az storage container legal-hold`. Führen Sie `-h` darauf aus, um die Befehle anzuzeigen.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Das Modul „Az.Storage“ unterstützt unveränderlichen Speicher.  Gehen Sie folgendermaßen vor, um diese Funktion zu aktivieren:

1. Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Entfernen Sie alle vorherigen Installationen von Azure PowerShell.
3. Installieren Sie Azure PowerShell: `Install-Module Az –Repository PSGallery –AllowClobber`.

Das folgende PowerShell-Beispielskript dient als Referenz. Dieses Skript erstellt ein neues Speicherkonto und einen Container. Anschließend wird veranschaulicht, wie Sie Zeiträume für gesetzliche Aufbewahrungspflichten festlegen und löschen und eine zeitbasierte Aufbewahrungsrichtlinie (auch als Unveränderlichkeitsrichtlinie bekannt) erstellen und sperren sowie den Aufbewahrungszeitraum verlängern.

Erstellen Sie zunächst ein Azure-Speicherkonto:

```powershell
$resourceGroup = "<Enter your resource group>"
$storageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$location = "<Enter the storage account location>"

# Log in to Azure
Connect-AzAccount
Register-AzResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Create your Azure storage account
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup -StorageAccountName `
    $storageAccount -SkuName Standard_ZRS -Location $location -Kind StorageV2

# Create a new container using the context
$container = New-AzStorageContainer -Name $container -Context $account.Context

# List the containers in the account
Get-AzStorageContainer -Context $account.Context

# Remove a container
Remove-AzStorageContainer -Name $container -Context $account.Context
```

Festlegen und Löschen gesetzlicher Aufbewahrungspflichten:

```powershell
# Set a legal hold
Add-AzRmStorageContainerLegalHold -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -Name $container -Tag <tag1>,<tag2>,...

# Clear a legal hold
Remove-AzRmStorageContainerLegalHold -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -Name $container -Tag <tag3>
```

Erstellen oder Aktualisieren von Unveränderlichkeitsrichtlinien:

```powershell
# Create an immutablity policy
Set-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container -ImmutabilityPeriod 10
```

Abrufen von Unveränderlichkeitsrichtlinien:

```powershell
# Get an immutability policy
Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName $resourceGroup `
    -StorageAccountName $storageAccount -ContainerName $container
```

Sperren von Unveränderlichkeitsrichtlinien (`-Force` hinzufügen, um die Aufforderung zu schließen):

```powershell
# Lock immutability policies
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container
Lock-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container `
    -Etag $policy.Etag
```

Erweitern von Unveränderlichkeitsrichtlinien:

```powershell
# Extend immutability policies
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container

Set-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy
```

Entfernen einer entsperrten Unveränderlichkeitsrichtlinie (`-Force` hinzufügen, um die Aufforderung zu schließen):

```powershell
# Remove an unlocked immutability policy
$policy = Get-AzRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $resourceGroup -StorageAccountName $storageAccount -ContainerName $container

Remove-AzRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy
```

---

## <a name="next-steps"></a>Nächste Schritte

[Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher](storage-blob-immutable-storage.md)