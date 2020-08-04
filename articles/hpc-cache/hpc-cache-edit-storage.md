---
title: Aktualisieren von Azure HPC Cache-Speicherzielen
description: Bearbeiten von Azure HPC Cache-Speicherzielen
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/02/2020
ms.author: v-erkel
ms.openlocfilehash: f11e12c4f30977514e04b09c7e1c3012eb7888a7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092455"
---
# <a name="edit-storage-targets"></a>Bearbeiten von Speicherzielen

Sie können ein Speicherziel auf der Portalseite **Speicherziele** des Caches oder mithilfe der Azure CLI entfernen oder ändern.

> [!TIP]
> Im [Video zur Verwaltung von Azure HPC Cache](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) wird gezeigt, wie Sie ein Speicherziel im Azure-Portal bearbeiten.

## <a name="remove-a-storage-target"></a>Entfernen eines Speicherziels

### <a name="portal"></a>[Portal](#tab/azure-portal)

Wenn Sie ein Speicherziel entfernen möchten, wählen Sie es in der Liste aus, und klicken Sie auf die Schaltfläche **Löschen**.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Verwenden Sie [az hpc-cache storage-target remove](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-remove), um ein Speicherziel aus dem Cache zu löschen.

```azurecli
$ az hpc-cache storage-target remove --resource-group cache-rg --cache-name doc-cache0629 --name blob1

{- Finished ..
  "endTime": "2020-07-09T21:45:06.1631571+00:00",
  "name": "2f95eac1-aded-4860-b19c-3f089531a7ec",
  "startTime": "2020-07-09T21:43:38.5461495+00:00",
  "status": "Succeeded"
}
```

---

Durch diese Aktion wird die Zuordnung des Speicherziels zu diesem Azure HPC Cache-System entfernt, das Back-End-Speichersystem wird jedoch nicht geändert. Wenn Sie beispielsweise einen Azure Blob Storage-Container verwendet haben, sind der Container und sein Inhalt nach dem Löschen aus dem Cache weiterhin vorhanden. Im Azure-Portal können Sie den Container einer anderen Azure HPC Cache-Instanz hinzufügen, ihn erneut diesem Cache hinzufügen oder löschen.

Alle im Cache gespeicherten Dateiänderungen werden in das Back-End-Speichersystem geschrieben, bevor das Speicherziel entfernt wird. Dieser Vorgang kann eine Stunde oder länger dauern, wenn sich viele geänderte Daten im Cache befinden.

## <a name="update-storage-targets"></a>Aktualisieren von Speicherzielen

Sie können Speicherziele bearbeiten, um einige ihrer Eigenschaften zu ändern. Für die unterschiedlichen Speichertypen können verschiedene Eigenschaften bearbeitet werden:

* Für Bobspeicherziele können Sie den Namespacepfad ändern.

* Für NFS-Speicherziele können Sie die folgenden Eigenschaften ändern:

  * Namespacepfad
  * Nutzungsmodell
  * Exportieren
  * Exportunterverzeichnis

Sie können den Namen, den Typ oder das Back-End-Speichersystem eines Speicherziels (Blobcontainer oder NFS-Hostname/IP-Adresse) nicht bearbeiten. Wenn Sie diese Eigenschaften ändern müssen, müssen Sie das Speicherziel löschen und einen Ersatz mit dem neuen Wert erstellen.

Im Azure-Portal können Sie sehen, welche Felder bearbeitet werden können, indem Sie auf den Namen des Speicherziels klicken und seine Detailseite öffnen. Sie können die Speicherziele auch mit der Azure CLI ändern.

![Screenshot der Bearbeitungsseite für ein NFS-Speicherziel](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>Aktualisieren eines neuen NFS-Speicherziels

Für ein NFS-Speicherziel können Sie mehrere Eigenschaften aktualisieren. (Ein Beispiel für eine Bearbeitungsseite sehen Sie im obigen Screenshot.)

* **Nutzungsmodell**: Das Nutzungsmodell beeinflusst, wie Daten im Cache gespeichert werden. Weitere Informationen finden Sie unter [Auswählen eines Nutzungsmodells](hpc-cache-add-storage.md#choose-a-usage-model).
* **Pfad des virtuellen Namespace**: Der Pfad, den Clients zum Bereitstellen dieses Speicherziels verwenden. Weitere Informationen finden Sie unter [Planen des aggregierten Namespace](hpc-cache-namespace.md).
* **NFS-Exportpfad**: Das Speichersystemexportverzeichnis, das für diesen Namespacepfad verwendet werden soll.
* **Unterverzeichnispfad**: Das Unterverzeichnis (unter dem Exportverzeichnis), das diesem Namespacepfad zugeordnet werden soll. Lassen Sie dieses Feld leer, wenn Sie kein Unterverzeichnis angeben müssen.

Für jeden Namespacepfad ist eine eindeutige Kombination von Exportverzeichnis und Unterverzeichnis erforderlich. Das heißt, Sie können auf dem Client nicht zwei verschiedene Pfade zum selben Verzeichnis auf dem Back-End-Speichersystem erstellen.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Klicken Sie auf **OK**, nachdem Sie die Änderungen vorgenommen haben, um das Speicherziel zu aktualisieren, oder klicken Sie auf **Abbrechen**, um die Änderungen zu verwerfen.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Verwenden Sie den Befehl [az nfs-storage-target](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target), um das Verwendungsmodell, den Pfad des virtuellen Namespace sowie die Werte für den NFS-Export oder das Unterverzeichnis für ein Speicherziel zu ändern.

* Um das Verwendungsmodell zu ändern, verwenden Sie die Option ``--nfs3-usage-model``. Beispiel: ``--nfs3-usage-model WRITE_WORKLOAD_15``

* Um den Namespacepfad, den Export oder das Exportunterverzeichnis zu ändern, verwenden Sie die Option ``--junction``.

  Der Parameter ``--junction`` verwendet diese Werte:

  * ``namespace-path``: Der clientseitige virtuelle Dateipfad.
  * ``nfs-export``: Der Speichersystemexport, der dem clientseitigen Pfad zugeordnet werden soll.
  * ``target-path`` (optional): Ein Unterverzeichnis des Exports, falls erforderlich.

  Beispiel: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

Der Cachename, der Name des Speicherziels und die Ressourcengruppe sind für alle Aktualisierungsbefehle erforderlich.

Befehlsbeispiel: <!-- having problem testing this -->

```azurecli
az hpc-cache nfs-storage-target update --cache-name mycache \
    --name rivernfs0 --resource-group doc-rg0619 \
    --nfs3-usage-model READ_HEAVY_INFREQ
```

Wenn der Cache beendet wurde oder sich nicht in einem fehlerfreien Zustand befindet, wird das Update angewendet, nachdem der Cache fehlerfrei ist.

---

## <a name="update-an-azure-blob-storage-target"></a>Hinzufügen eines neuen Azure-Blobspeicherziels

Für ein Blobspeicherziel können Sie den Pfad des virtuellen Namespace ändern.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Auf der Detailseite eines Blobspeicherziels können Sie den Pfad des virtuellen Namespace ändern.

![Screenshot der Bearbeitungsseite für ein Blobspeicherziel](media/hpc-cache-edit-storage-blob.png)

Klicken Sie auf **OK**, wenn Sie damit fertig sind, um das Speicherziel zu aktualisieren, oder klicken Sie auf **Abbrechen**, um die Änderungen zu verwerfen.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Verwenden Sie [az hpc-cache blob-storage-target update](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-update), um den Namespacepfad eines Ziels zu aktualisieren.

```azurecli
az hpc-cache blob-storage-target update --cache-name cache-name --name target-name \
    --resource-group rg --storage-account "/subscriptions/<subscription_ID>/resourceGroups/erinazcli/providers/Microsoft.Storage/storageAccounts/rg"  \
    --container-name "container-name" --virtual-namespace-path "/new-path"
```

Wenn der Cache beendet wurde oder sich nicht in einem fehlerfreien Zustand befindet, wird das Update angewendet, nachdem der Cache fehlerfrei ist.

---

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu diesen Optionen finden Sie unter [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md).
* Weitere Tipps zur Verwendung virtueller Pfade finden Sie unter [Planen des aggregierten Namespace](hpc-cache-namespace.md).
