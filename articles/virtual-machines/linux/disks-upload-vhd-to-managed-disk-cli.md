---
title: Hochladen einer VHD in Azure oder regionsübergreifendes Kopieren eines Datenträgers – Azure-Befehlszeilenschnittstelle
description: Hier erfahren Sie, wie Sie per Azure-Befehlszeilenschnittstelle und direktem Upload eine VHD in einen verwalteten Azure-Datenträger hochladen und den verwalteten Datenträger regionsübergreifend kopieren.
services: virtual-machines,storage
author: roygara
ms.author: rogarana
ms.date: 03/27/2020
ms.topic: article
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 6813206aebe67e4e6d2afd0d9c78d03f0c20c952
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420967"
---
# <a name="upload-a-vhd-to-azure-or-copy-a-managed-disk-to-another-region---azure-cli"></a>Hochladen einer VHD in Azure oder Kopieren eines verwalteten Datenträgers in eine andere Region – Azure-Befehlszeilenschnittstelle

[!INCLUDE [disks-upload-vhd-to-disk-intro](../../../includes/disks-upload-vhd-to-disk-intro.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie die neueste [Version von AzCopy V10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy) herunter.
- [Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).
- Wenn Sie beabsichtigen, eine VHD aus der lokalen Umgebung hochzuladen: Eine VHD fester Größe, die [für Azure vorbereitet wurde](../windows/prepare-for-upload-vhd-image.md) und lokal gespeichert ist.
- Oder ein verwalteter Datenträger in Azure, wenn Sie eine Kopieraktion ausführen möchten.

## <a name="getting-started"></a>Erste Schritte

Wenn Sie es vorziehen, Datenträger über eine grafische Benutzeroberfläche hochzuladen, können Sie dazu Azure Storage-Explorer verwenden. Ausführliche Informationen finden Sie unter: [Verwenden von Azure Storage-Explorer zum Verwalten von verwalteten Azure-Datenträgern](disks-use-storage-explorer-managed-disks.md)

Wenn Sie Ihre VHD in Azure hochladen möchten, müssen Sie einen leeren verwalteten Datenträger erstellen, der für diesen Hochladevorgang konfiguriert ist. Bevor Sie einen solchen Datenträger erstellen, sollten Sie einige zusätzliche Informationen über diese Datenträger wissen.

Diese Art des verwalteten Datenträgers verfügt über zwei eindeutige Zustände:

- „ReadyToUpload“: Der Datenträger ist für den Upload bereit, jedoch wurde keine [SAS (Shared Access Signature)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) generiert.
- „ActiveUpload“: Der Datenträger ist für den Upload bereit, und die SAS wurde generiert.

> [!NOTE]
> Für den verwalteten Datenträger fallen in beiden Zuständen Kosten an, die sich aus den [Preisen für HDD Standard-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/) ergeben. Welcher Art von Datenträger verwendet wird, spielt keine Rolle. Das bedeutet, dass beispielsweise der Abrechnungsvorgang für P10 und S10 identisch ist. Dies ist solange der Fall, bis `revoke-access` für den verwalteten Datenträger aufgerufen wird. Dieser Vorgang ist erforderlich, um den Datenträger an eine VM anzufügen.

## <a name="create-an-empty-managed-disk"></a>Erstellen eines leeren verwalteten Datenträgers

Bevor Sie eine leere HDD Standard zum Hochladen erstellen können, benötigen Sie die Dateigröße der VHD in Byte, die Sie hochladen möchten. Um diese abzurufen, können Sie `wc -c <yourFileName>.vhd` oder `ls -al <yourFileName>.vhd` verwenden. Dieser Wert wird verwendet, wenn der Parameter **--upload-size-bytes** angegeben wird.

Erstellen Sie für das Hochladen eine leere HDD Standard, indem Sie in einem [Datenträgererstellungs](/cli/azure/disk#az-disk-create)-Cmdlet sowohl den **--for-upload**- als auch den **--upload-size-bytes**-Parameter angeben:

Ersetzen Sie `<yourdiskname>`, `<yourresourcegroupname>`, `<yourregion>` durch Werte Ihrer Wahl. Der Parameter `--upload-size-bytes` enthält einen Beispielwert von `34359738880`, ersetzen Sie ihn durch einen für Sie geeigneten Wert.

```azurecli
az disk create -n <yourdiskname> -g <yourresourcegroupname> -l <yourregion> --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Wenn Sie für den Upload einen SSD Premium- oder SSD Standard-Datenträger verwenden möchten, ersetzen Sie **standard_lrs** durch **premium_LRS** oder **standardSSD_lrs**. Ultra Disks werden derzeit nicht unterstützt.

Nachdem Sie einen leeren verwalteten Datenträger erstellt haben, der für den Hochladevorgang konfiguriert ist, können Sie jetzt eine VHD auf diesen Datenträger hochladen. Um eine VHD auf den Datenträger hochzuladen, benötigen Sie eine beschreibbare SAS, mit der Sie auf den Datenträger als Hochladeziel verweisen können.

Ersetzen Sie `<yourdiskname>` und `<yourresourcegroupname>`, und verwenden Sie dann den folgenden Befehl, um eine beschreibbare SAS für Ihren leeren verwalteten Datenträger zu generieren:

```azurecli
az disk grant-access -n <yourdiskname> -g <yourresourcegroupname> --access-level Write --duration-in-seconds 86400
```

Beispiel eines Rückgabewerts:

```output
{
  "accessSas": "https://md-impexp-t0rdsfgsdfg4.blob.core.windows.net/w2c3mj0ksfgl/abcd?sv=2017-04-17&sr=b&si=600a9281-d39e-4cc3-91d2-923c4a696537&sig=xXaT6mFgf139ycT87CADyFxb%2BnPXBElYirYRlbnJZbs%3D"
}
```

## <a name="upload-a-vhd"></a>Hochladen einer VHD-Datei

Sie verfügen nun über eine SAS für Ihren leeren verwalteten Datenträger und können diese verwenden, um diesen Datenträger als Ziel für den Uploadbefehl anzugeben.

Verwenden Sie AzCopy v10, um die lokale VHD-Datei auf einen verwalteten Datenträger hochzuladen. Dabei müssen Sie den generierten SAS-URI angeben.

Der Durchsatz für diesen Upload entspricht dem des gleichwertigen [HDD Standard-Datenträgers](disks-types.md#standard-hdd). Wenn Sie beispielsweise als Größe S4 verwenden, beträgt der Durchsatz bis zu 60 MiB/s. Verwenden Sie hingegen als Größe S70, beträgt der Durchsatz bis zu 500 MiB/s.

```bash
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Nachdem der Upload abgeschlossen wurde und Sie keine Daten mehr auf den Datenträger schreiben müssen, widerrufen Sie die SAS. Dadurch ändert sich der Zustand des verwalteten Datenträgers, und Sie können den Datenträger an eine VM anfügen.

Ersetzen Sie `<yourdiskname>`und `<yourresourcegroupname>`, und verwenden Sie dann den folgenden Befehl, damit der Datenträger verwendet werden kann:

```azurecli
az disk revoke-access -n <yourdiskname> -g <yourresourcegroupname>
```

## <a name="copy-a-managed-disk"></a>Kopieren eines verwalteten Datenträgers

Der direkte Upload vereinfacht auch das Kopieren eines verwalteten Datenträgers. Sie können entweder innerhalb der gleichen Region oder regionsübergreifend (in eine andere Region) kopieren.

Das folgende Skript führt dies für Sie aus. Der Vorgang ähnelt mit einigen Unterschieden den zuvor beschriebenen Schritten, da Sie nun mit einem vorhandenen Datenträger arbeiten.

> [!IMPORTANT]
> Sie müssen ein Offset von 512 hinzufügen, wenn Sie die Datenträgergröße eines verwalteten Datenträgers aus Azure in Byte bereitstellen. Dies liegt daran, dass Azure die Fußzeile beim Zurückgeben der Datenträgergröße auslässt. Der Kopiervorgang schlägt fehl, wenn Sie nicht so vorgehen. Das folgende Skript fügt dieses Offset bereits für Sie ein.

Ersetzen Sie `<sourceResourceGroupHere>`, `<sourceDiskNameHere>`, `<targetDiskNameHere>`, `<targetResourceGroupHere>` und `<yourTargetLocationHere>` (ein Beispiel für einen Standortwert ist „uswest2“) durch Ihre Werte, und führen Sie dann das folgende Skript aus, um einen verwalteten Datenträger zu kopieren.

```azurecli
sourceDiskName = <sourceDiskNameHere>
sourceRG = <sourceResourceGroupHere>
targetDiskName = <targetDiskNameHere>
targetRG = <targetResourceGroupHere>
targetLocale = <yourTargetLocationHere>

sourceDiskSizeBytes= $(az disk show -g $sourceRG -n $sourceDiskName --query '[uniqueId]' -o tsv)

az disk create -g $targetRG -n $targetDiskName -l $targetLocale --for-upload --upload-size-bytes $(($sourceDiskSizeBytes+512)) --sku standard_lrs

targetSASURI = $(az disk grant-access -n $targetDiskName -g $targetRG  --access-level Write --duration-in-seconds 86400 -o tsv)

sourceSASURI=$(az disk grant-access -n $sourceDiskName -g $sourceRG --duration-in-seconds 86400 --query [accessSas] -o tsv)

.\azcopy copy $sourceSASURI $targetSASURI --blob-type PageBlob

az disk revoke-access -n $sourceDiskName -g $sourceRG

az disk revoke-access -n $targetDiskName -g $targetRG
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfolgreich eine VHD auf einen verwalteten Datenträger hochgeladen haben, können Sie den Datenträger als [Datenträger für Daten an eine vorhandene VM](add-disk.md) anfügen oder [den Datenträger als Betriebssystemdatenträger](upload-vhd.md#create-the-vm) an eine VM anfügen, um eine neue VM zu erstellen. 

