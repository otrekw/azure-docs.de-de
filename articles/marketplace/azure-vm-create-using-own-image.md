---
title: Erstellen eines Azure-VM-Angebots im Azure Marketplace mit Ihrem eigenen Image
description: Erfahren Sie, wie Sie ein VM-Angebot im Azure Marketplace mit Ihrem eigenen Image veröffentlichen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: krsh
ms.date: 10/20/2020
ms.openlocfilehash: 42022d1204c3b524ee2e9ef2770f616fba89dc8c
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283200"
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

### <a name="perform-additional-security-checks"></a>Durchführen von zusätzlichen Sicherheitsüberprüfungen

[!INCLUDE [Discussion of addition security checks](includes/additional-security-checks.md)]

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Durchführen der benutzerdefinierten Konfiguration und von geplanten Aufgaben

[!INCLUDE [Discussion of custom configuration and scheduled tasks](includes/custom-config.md)]

## <a name="upload-the-vhd-to-azure"></a>Hochladen der VHD in Azure

Konfigurieren und bereiten Sie die VM für den Upload vor, wie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md) oder [Erstellen und Hochladen einer Linux-VHD](../virtual-machines/linux/create-upload-generic.md) beschrieben.

## <a name="extract-the-vhd-from-image-if-using-image-building-services"></a>Extrahieren der VHD aus einem Image (bei Verwendung von Image-Erstellungsdiensten)

Wenn Sie einen Image-Erstellungsdienst wie [Packer](https://www.packer.io/) verwenden, müssen Sie die VHD möglicherweise aus dem Image extrahieren. Hierfür gibt es keine direkte Methode. Sie müssen eine VM erstellen und die VHD aus dem VM-Datenträger extrahieren.

### <a name="create-the-vm-on-the-azure-portal"></a>Erstellen der VM im Azure-Portal

Gehen Sie wie folgt vor, um das VM-Basisimage im [Azure-Portal](https://ms.portal.azure.com/) zu erstellen.

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/) an.
2. Wählen Sie **Virtuelle Computer** aus.
3. Wählen Sie **+ Hinzufügen** aus, um den Bildschirm **Virtuellen Computer erstellen** zu öffnen.
4. Wählen Sie das Image in der Dropdownliste oder **Alle öffentlichen und privaten Images durchsuchen** aus, um nach allen verfügbaren VM-Images zu suchen.
5. Um eine **Gen 2** -VM zu erstellen, navigieren Sie zur Registerkarte **Erweitert** und wählen die Option **Gen 2** aus.

    :::image type="content" source="media/create-vm/vm-gen-option.png" alt-text="Wählen Sie „Gen 1“ oder „Gen 2“ aus.":::

6. Wählen Sie die Größe der bereitzustellenden VM aus.

    :::image type="content" source="media/create-vm/create-virtual-machine-sizes.png" alt-text="Wählen Sie „Gen 1“ oder „Gen 2“ aus.":::

7. Geben Sie die anderen Details ein, die zum Erstellen der VM erforderlich sind.
8. Wählen Sie **Bewerten + erstellen** aus, um Ihre Auswahl zu überprüfen. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Azure beginnt mit der Bereitstellung der von Ihnen angegebenen VM. Sie können den Fortschritt nachverfolgen, indem Sie im linken Menü die Registerkarte **Virtuelle Computer** auswählen. Nach der Erstellung ändert sich der Status des virtuellen Computers in **Wird ausgeführt** .

### <a name="connect-to-your-vm"></a>Herstellen einer Verbindung mit Ihrer VM

In der folgenden Dokumentation erfahren Sie, wie Sie eine Verbindung mit einer [Windows](../virtual-machines/windows/connect-logon.md)- oder [Linux](../virtual-machines/linux/ssh-from-windows.md#connect-to-your-vm)-VM herstellen.

[!INCLUDE [Discussion of addition security checks](includes/size-connect-generalize.md)]

## <a name="next-steps"></a>Nächste Schritte

- Empfohlener nächster Schritt: [Testen Sie das VM-Image](azure-vm-image-test.md), um sicherzustellen, dass es die Veröffentlichungsanforderungen von Azure Marketplace erfüllt. Dies ist optional.
- Wenn Sie Ihr VM-Image nicht testen, fahren Sie mit dem [Generieren des SAS-URIs](azure-vm-get-sas-uri.md) fort.
- Wenn beim Erstellen der neuen Azure-basierten VHD Probleme auftreten, informieren Sie sich in den [FAQ zu VMs für Azure Marketplace](azure-vm-create-faq.md).
