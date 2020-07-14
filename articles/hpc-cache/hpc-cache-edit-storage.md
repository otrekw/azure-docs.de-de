---
title: Aktualisieren von Azure HPC Cache-Speicherzielen
description: Bearbeiten von Azure HPC Cache-Speicherzielen
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 1/08/2020
ms.author: v-erkel
ms.openlocfilehash: 0fa8be58db9754c652d6e1ee5349c950a1c19109
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513843"
---
# <a name="edit-storage-targets"></a>Bearbeiten von Speicherzielen

Sie können ein Speicherziel auf der Seite **Speicherziele** des Cache entfernen oder ändern.

> [!TIP]
> Im [Video zur Verwaltung von Azure HPC Cache](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) wird gezeigt, wie Sie ein Speicherziel bearbeiten.

## <a name="remove-a-storage-target"></a>Entfernen eines Speicherziels

Wenn Sie ein Speicherziel entfernen möchten, wählen Sie es in der Liste aus, und klicken Sie auf die Schaltfläche **Löschen**.

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

Wenn Sie ein Speicherziel ändern möchten, klicken Sie auf den Namen des Speicherziels, um die zugehörige Detailseite zu öffnen. Einige Felder auf der Seite können bearbeitet werden.

![Screenshot der Bearbeitungsseite für ein NFS-Speicherziel](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>Aktualisieren eines neuen NFS-Speicherziels

Für ein NFS-Speicherziel können Sie mehrere Eigenschaften aktualisieren. (Ein Beispiel für eine Bearbeitungsseite sehen Sie im obigen Screenshot.)

* **Nutzungsmodell**: Das Nutzungsmodell beeinflusst, wie Daten im Cache gespeichert werden. Weitere Informationen finden Sie unter [Auswählen eines Nutzungsmodells](hpc-cache-add-storage.md#choose-a-usage-model).
* **Pfad des virtuellen Namespace**: Der Pfad, den Clients zum Bereitstellen dieses Speicherziels verwenden. Weitere Informationen finden Sie unter [Planen des aggregierten Namespace](hpc-cache-namespace.md).
* **NFS-Exportpfad**: Das Speichersystemexportverzeichnis, das für diesen Namespacepfad verwendet werden soll.
* **Unterverzeichnispfad**: Das Unterverzeichnis (unter dem Exportverzeichnis), das diesem Namespacepfad zugeordnet werden soll. Lassen Sie dieses Feld leer, wenn Sie kein Unterverzeichnis angeben müssen.

Für jeden Namespacepfad ist eine eindeutige Kombination von Exportverzeichnis und Unterverzeichnis erforderlich. Das heißt, Sie können auf dem Client nicht zwei verschiedene Pfade zum selben Verzeichnis auf dem Back-End-Speichersystem erstellen.

Klicken Sie auf **OK**, nachdem Sie die Änderungen vorgenommen haben, um das Speicherziel zu aktualisieren, oder klicken Sie auf **Abbrechen**, um die Änderungen zu verwerfen.

## <a name="update-an-azure-blob-storage-target"></a>Hinzufügen eines neuen Azure-Blobspeicherziels

Auf der Detailseite eines Blobspeicherziels können Sie den Pfad des virtuellen Namespace ändern.

![Screenshot der Bearbeitungsseite für ein Blobspeicherziel](media/hpc-cache-edit-storage-blob.png)

Klicken Sie auf **OK**, wenn Sie damit fertig sind, um das Speicherziel zu aktualisieren, oder klicken Sie auf **Abbrechen**, um die Änderungen zu verwerfen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu diesen Optionen finden Sie unter [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md).
* Weitere Tipps zur Verwendung virtueller Pfade finden Sie unter [Planen des aggregierten Namespace](hpc-cache-namespace.md).
