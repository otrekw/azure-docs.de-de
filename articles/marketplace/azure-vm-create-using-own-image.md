---
title: Erstellen eines Azure-VM-Angebots im Azure Marketplace mit Ihrem eigenen Image
description: Erfahren Sie, wie Sie ein VM-Angebot im Azure Marketplace mit Ihrem eigenen Image veröffentlichen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: krsh
ms.author: krsh
ms.date: 03/10/2021
ms.openlocfilehash: 4711ea76af83594ec529cfda13a308fbe6646398
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103200458"
---
# <a name="how-to-create-a-virtual-machine-using-your-own-image"></a>Erstellen eines virtuellen Computers mit einem eigenen Image

In diesem Artikel wird beschrieben, wie Sie ein vom Benutzer bereitgestelltes VM-Image erstellen und bereitstellen.

> [!NOTE]
> Bevor Sie beginnen, überprüfen Sie die [technischen Anforderungen](marketplace-virtual-machines.md#technical-requirements) für Azure-VM-Angebote, einschließlich der Anforderungen an virtuelle Festplatten (Virtual Hard Disks, VHDs).

Folgen Sie den Anweisungen unter [Erstellen eines VM-Images anhand eines genehmigten Basisimages](azure-vm-create-using-approved-base.md), um stattdessen ein genehmigtes Basisimage zu verwenden.

## <a name="configure-the-vm"></a>Konfigurieren der VM

In diesem Abschnitt wird beschrieben, wie Sie die Größe einer Azure-VM anpassen, sie aktualisieren und generalisieren. Diese Schritte sind erforderlich, um Ihre VM für die Bereitstellung in Azure Marketplace vorzubereiten.

### <a name="size-the-vhds"></a>Festlegen der VHD-Größe

[!INCLUDE [Discussion of VHD sizing](includes/vhd-size.md)]

### <a name="install-the-most-current-updates"></a>Installieren der aktuellsten Updates

[!INCLUDE [Discussion of most current updates](includes/most-current-updates.md)]

### <a name="perform-more-security-checks"></a>Durchführen weiterer Sicherheitsüberprüfungen

[!INCLUDE [Discussion of addition security checks](includes/additional-security-checks.md)]

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Durchführen der benutzerdefinierten Konfiguration und von geplanten Aufgaben

[!INCLUDE [Discussion of custom configuration and scheduled tasks](includes/custom-config.md)]

### <a name="generalize-the-image"></a>Generalisieren des Images

Alle Images im Azure Marketplace müssen allgemein wiederverwendbar sein. Um dies zu erreichen, muss die Betriebssystem-VHD generalisiert werden. Dies ist ein Vorgang, bei dem alle instanzspezifischen Bezeichner und Softwaretreiber von einer VM entfernt werden.

## <a name="bring-your-image-into-azure"></a>Einbinden Ihres Image in Azure

Es gibt drei Möglichkeiten, Ihr Image in Azure einzubinden:

1. Laden Sie die virtuelle Festplatte in eine Shared Image Gallery-Instanz (SIG) hoch.
1. Laden Sie die virtuelle Festplatte in ein Azure Storage-Konto hoch.
1. Extrahieren Sie die virtuelle Festplatte aus einem verwalteten Image (bei Verwendung von Image-Erstellungsdiensten).

In den folgenden drei Abschnitten werden diese Optionen beschrieben.

### <a name="option-1-upload-the-vhd-as-shared-image-gallery"></a>Option 1: Hochladen der virtuellen Festplatte in eine Shared Image Gallery-Instanz

1. Laden Sie die virtuelle(n) Festplatte(n) in das Speicherkonto hoch.
2. Suchen Sie im Azure-Portal nach **Benutzerdefinierte Vorlage bereitstellen**.
3. Wählen Sie **Eigene Vorlage im Editor erstellen**.
4. Kopieren Sie die folgende ARM-Vorlage (Azure Resource Manager).

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sourceStorageAccountResourceId": {
          "type": "string",
          "metadata": {
            "description": "Resource ID of the source storage account that the blob vhd resides in."
          }
        },
        "sourceBlobUri": {
          "type": "string",
          "metadata": {
            "description": "Blob Uri of the vhd blob (must be in the storage account provided.)"
          }
        },
        "sourceBlobDataDisk0Uri": {
          "type": "string",
          "metadata": {
            "description": "Blob Uri of the vhd blob (must be in the storage account provided.)"
          }
        },
        "sourceBlobDataDisk1Uri": {
          "type": "string",
          "metadata": {
            "description": "Blob Uri of the vhd blob (must be in the storage account provided.)"
          }
        },
        "galleryName": {
          "type": "string",
          "metadata": {
            "description": "Name of the Shared Image Gallery."
          }
        },
        "galleryImageDefinitionName": {
          "type": "string",
          "metadata": {
            "description": "Name of the Image Definition."
          }
        },
        "galleryImageVersionName": {
          "type": "string",
          "metadata": {
            "description": "Name of the Image Version - should follow <MajorVersion>.<MinorVersion>.<Patch>."
          }
        }
      },
      "resources": [
        {
          "type": "Microsoft.Compute/galleries/images/versions",
          "name": "[concat(parameters('galleryName'), '/', parameters('galleryImageDefinitionName'), '/', parameters('galleryImageVersionName'))]",
          "apiVersion": "2020-09-30",
          "location": "[resourceGroup().location]",
          "properties": {
            "storageProfile": {
              "osDiskImage": {
                "source": {
                  "id": "[parameters('sourceStorageAccountResourceId')]",
                  "uri": "[parameters('sourceBlobUri')]"
                }
              },
    
              "dataDiskImages": [
                {
                  "lun": 0,
                  "source": {
                    "id": "[parameters('sourceStorageAccountResourceId')]",
                    "uri": "[parameters('sourceBlobDataDisk0Uri')]"
                  }
                },
                {
                  "lun": 1,
                  "source": {
                    "id": "[parameters('sourceStorageAccountResourceId')]",
                    "uri": "[parameters('sourceBlobDataDisk1Uri')]"
                  }
                }
              ]
            }
          }
        }
      ]
    }
    
    ```

5. Fügen Sie die Vorlage in den Editor ein.

    :::image type="content" source="media/create-vm/vm-sample-code-screen.png" alt-text="Screenshot des Codebeispiels für die VM":::

1. Wählen Sie **Speichern** aus.
1. Verwenden Sie die Parameter in dieser Tabelle, um die im darauffolgenden Screenshot angezeigten Felder auszufüllen.

| Parameter | BESCHREIBUNG |
| --- | --- |
| sourceStorageAccountResourceId | Dieser Parameter gibt die Ressourcen-ID des Quellspeicherkontos an, in dem sich die Blob-VHD befindet.<br><br>Zum Abrufen der Ressourcen-ID wechseln Sie im **Azure-Portal** zu Ihrem **Speicherkonto** und dann zu **Eigenschaften**, und kopieren Sie den **ResourceId**-Wert. |
| sourceBlobUri | Dieser Parameter gibt den Blob-URI des VHD-Blobs des Betriebssystem-Datenträgers an (muss sich im angegebenen Speicherkonto befinden).<br><br>Zum Abrufen der Blob-URL wechseln Sie im **Azure-Portal** zu Ihrem **Speicherkonto** und dann zu **Blob**, und kopieren Sie den **URL**-Wert. |
| sourceBlobDataDisk0Uri | Dieser Parameter gibt den Blob-URI des VHD-Blobs des Datenträgers an (muss sich im angegebenen Speicherkonto befinden). Wenn Sie über keinen Datenträger verfügen, entfernen Sie diesen Parameter aus der Vorlage.<br><br>Zum Abrufen der Blob-URL wechseln Sie im **Azure-Portal** zu Ihrem **Speicherkonto** und dann zu **Blob**, und kopieren Sie den **URL**-Wert. |
| sourceBlobDataDisk1Uri | Dieser Parameter gibt den Blob-URI des VHD-Blobs des zusätzlichen Datenträgers an (muss sich im angegebenen Speicherkonto befinden). Wenn Sie über keinen zusätzlichen Datenträger verfügen, entfernen Sie diesen Parameter aus der Vorlage.<br><br>Zum Abrufen der Blob-URL wechseln Sie im **Azure-Portal** zu Ihrem **Speicherkonto** und dann zu **Blob**, und kopieren Sie den **URL**-Wert. |
| galleryName | Name der Shared Image Gallery-Instanz |
| galleryImageDefinitionName | Name der Imagedefinition |
| galleryImageVersionName | Name der zu erstellenden Imageversion im folgenden Format: `<MajorVersion>.<MinorVersion>.<Patch>` |
|

:::image type="content" source="media/create-vm/custom-deployment-window.png" alt-text="Screenshot des Fensters „Benutzerdefinierte Bereitstellung“":::

8. Klicken Sie auf **Überprüfen + erstellen**. Wählen Sie nach Abschluss der Überprüfung **Erstellen** aus.

> [!TIP]
> Für das Herausgeberkonto muss die Zugriffsberechtigung „Besitzer“ festgelegt sein, damit das SIG-Image veröffentlicht werden kann. Führen Sie bei Bedarf die folgenden Schritte durch, um den Zugriff zu gewähren:
>
> 1. Wechseln Sie zur Shared Image Gallery-Instanz (SIG).
> 2. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
> 3. Wählen Sie **Hinzufügen** und dann **Rollenzuweisung hinzufügen** aus.
> 4. Wählen Sie für **Rolle** die Option **Besitzer** aus.
> 5. Wählen Sie für **Zugriff zuweisen zu** die Option **Benutzer, Gruppe oder Dienstprinzipal** aus.
> 6. Geben Sie die Azure-E-Mail-Adresse der Person ein, die das Image veröffentlichen wird.
> 7. Wählen Sie **Speichern** aus.<br><br>
> :::image type="content" source="media/create-vm/add-role-assignment.png" alt-text="Screenshot des Fensters „Rollenzuweisung hinzufügen“":::

### <a name="option-2-upload-the-vhd-to-a-storage-account"></a>Option 2: Hochladen der virtuellen Festplatte in ein Speicherkonto

Konfigurieren und bereiten Sie die VM für den Upload vor, wie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md) oder [Erstellen und Hochladen einer Linux-VHD](../virtual-machines/linux/create-upload-generic.md) beschrieben.

### <a name="option-3-extract-the-vhd-from-managed-image-if-using-image-building-services"></a>Option 3: Extrahieren der virtuellen Festplatte aus einem verwalteten Image (bei Verwendung von Image-Erstellungsdiensten)

Wenn Sie einen Image-Erstellungsdienst wie [Packer](https://www.packer.io/) verwenden, müssen Sie die virtuelle Festplatte möglicherweise aus dem Image extrahieren. Hierfür gibt es keine direkte Methode. Sie müssen eine VM erstellen und die VHD aus dem VM-Datenträger extrahieren.

## <a name="create-the-vm-on-the-azure-portal"></a>Erstellen der VM im Azure-Portal

Gehen Sie wie folgt vor, um das VM-Basisimage im [Azure-Portal](https://ms.portal.azure.com/) zu erstellen.

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/) an.
2. Wählen Sie **Virtuelle Computer** aus.
3. Wählen Sie **+ Hinzufügen** aus, um den Bildschirm **Virtuellen Computer erstellen** zu öffnen.
4. Wählen Sie das Image in der Dropdownliste oder **Alle öffentlichen und privaten Images durchsuchen** aus, um nach allen verfügbaren VM-Images zu suchen.
5. Um eine **Gen 2**-VM zu erstellen, navigieren Sie zur Registerkarte **Erweitert** und wählen die Option **Gen 2** aus.

    :::image type="content" source="media/create-vm/vm-gen-option.png" alt-text="Wählen Sie „Gen 1“ oder „Gen 2“ aus.":::

6. Wählen Sie die Größe der bereitzustellenden VM aus.

    :::image type="content" source="media/create-vm/create-virtual-machine-sizes.png" alt-text="Wählen Sie eine empfohlene VM-Größe für das ausgewählte Image aus.":::

7. Geben Sie die anderen Details ein, die zum Erstellen der VM erforderlich sind.
8. Wählen Sie **Bewerten + erstellen** aus, um Ihre Auswahl zu überprüfen. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Azure beginnt mit der Bereitstellung der von Ihnen angegebenen VM. Sie können den Fortschritt nachverfolgen, indem Sie im linken Menü die Registerkarte **Virtuelle Computer** auswählen. Nach der Erstellung ändert sich der Status des virtuellen Computers in **Wird ausgeführt**.

## <a name="connect-to-your-vm"></a>Herstellen einer Verbindung mit Ihrer VM

In der folgenden Dokumentation erfahren Sie, wie Sie eine Verbindung mit einer [Windows](../virtual-machines/windows/connect-logon.md)- oder [Linux](../virtual-machines/linux/ssh-from-windows.md#connect-to-your-vm)-VM herstellen.

[!INCLUDE [Discussion of addition security checks](includes/size-connect-generalize.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Testen Sie das VM-Image](azure-vm-image-test.md), um sicherzustellen, dass es die Veröffentlichungsanforderungen von Azure Marketplace erfüllt. Diese Eingabe ist optional.
- Wenn Sie Ihr VM-Image nicht testen möchten, melden Sie sich beim [Partner Center](https://partner.microsoft.com/) an, und veröffentlichen Sie das SIG-Image (Option 1).
- Wenn Sie sich für Option 2 oder 3 entschieden haben, [generieren Sie den SAS-URI](azure-vm-get-sas-uri.md).
- Wenn beim Erstellen der neuen Azure-basierten VHD Probleme auftreten, informieren Sie sich in den [FAQ zu VMs für Azure Marketplace](azure-vm-create-faq.md).
