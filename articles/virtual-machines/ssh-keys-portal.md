---
title: Erstellen von SSH-Schlüsseln im Azure-Portal
description: Es wird beschrieben, wie Sie SSH-Schlüssel im Azure-Portal generieren und speichern, um für die Linux-VMs eine Verbindung herzustellen.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/31/2020
ms.author: cynthn
ms.openlocfilehash: 17ab7ee75e335d686bf308c4b15a53dc4e2e6b0e
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88041747"
---
# <a name="generate-and-store-ssh-keys-in-the-azure-portal"></a>Generieren und Speichern von SSH-Schlüsseln im Azure-Portal

Falls Sie das Portal häufig zum Bereitstellen von Linux-VMs verwenden, können Sie die Nutzung von SSH-Schlüsseln vereinfachen, indem Sie sie direkt im Portal erstellen oder von Ihrem Computer hochladen.

Sie können SSH-Schlüssel erstellen, wenn Sie den ersten virtuellen Computer erstellen, und dann für andere VMs wiederverwenden. Alternativ können Sie SSH-Schlüssel auch separat erstellen, damit Sie in Azure über eine gespeicherte Gruppe von Schlüsseln verfügen, um die Anforderungen Ihrer Organisation zu erfüllen. 

Falls Sie über vorhandene Schlüssel verfügen und deren Nutzung im Portal vereinfachen möchten, können Sie sie hochladen und zur Wiederverwendung in Azure speichern.

Ausführlichere Informationen zur Erstellung und Verwendung von SSH-Schlüsseln mit Linux-VMs finden Sie unter [Verwenden von SSH-Schlüsseln zum Herstellen einer Verbindung mit Linux-VMs](./linux/ssh-from-windows.md).

## <a name="generate-new-keys"></a>Generieren von neuen Schlüsseln

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Geben Sie oben auf der Seite *SSH* für die Suche ein. Wählen Sie unter *Marketplace* die Option **SSH-Schlüssel** aus.

1. Wählen Sie auf der Seite **SSH-Schlüssel** die Option **Erstellen** aus.

   :::image type="content" source="./media/ssh-keys/portal-sshkey.png" alt-text="Erstellen einer neuen Ressourcengruppe und Generieren eines SSH-Schlüsselpaars":::

1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, um eine neue Ressourcengruppe zum Speichern Ihrer Schlüssel zu erstellen. Geben Sie einen Namen für Ihre Ressourcengruppe ein, und wählen Sie **OK** aus.

1. Wählen Sie unter **Region** eine Region zum Speichern Ihrer Schlüssel aus. Sie können die Schlüssel in einer beliebigen Region verwenden. Dies ist lediglich die Region, in der sie gespeichert werden.

1. Geben Sie einen Namen für Ihren Schlüssel unter **Schlüsselpaarname** ein.

1. Wählen Sie unter **Quelle für öffentlichen SSH-Schlüssel** die Option **Generate public key source** (Quelle für öffentlichen Schlüssel generieren) aus. 

1. Wählen Sie abschließend **Überprüfen + Erstellen** aus.

1. Wählen Sie nach der erfolgreichen Überprüfung **Erstellen** aus.

1. Ein Popupfenster wird angezeigt, in dem Sie die Option **Download private key and create resource** (Privaten Schlüssel herunterladen und Ressource erstellen) auswählen können. Dies führt dazu, dass der SSH-Schlüssel als PEM-Datei heruntergeladen wird.

   :::image type="content" source="./media/ssh-keys/download-key.png" alt-text="Herunterladen des privaten Schlüssels als PEM-Datei":::

1. Nachdem Sie die PEM-Datei heruntergeladen haben, sollten Sie sie an einen beliebigen Ort auf Ihrem Computer verschieben, auf den von Ihrem SSH-Client aus leicht verwiesen werden kann.


## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Öffnen Sie auf Ihrem lokalen Computer eine PowerShell-Eingabeaufforderung, und geben Sie Folgendes ein:

```powershell
ssh -i <path to the .pem file> username@<ipaddress of the VM>
```

Geben Sie z. B. Folgendes ein: `ssh -i /Downloads/mySSHKey.pem azureuser@123.45.67.890`


## <a name="upload-an-ssh-key"></a>Hochladen eines SSH-Schlüssels

Sie können auch einen öffentlichen SSH-Schlüssel für die Speicherung in Azure hochladen. Weitere Informationen zur Erstellung eines SSH-Schlüsselpaars finden Sie unter [Verwenden von SSH-Schlüsseln zum Herstellen einer Verbindung mit Linux-VMs](./linux/ssh-from-windows.md).

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Geben Sie oben auf der Seite *SSH* für die Suche ein. Wählen Sie unter *Marketplace* die Option **SSH-Schlüssel** aus.

1. Wählen Sie auf der Seite **SSH-Schlüssel** die Option **Erstellen** aus.

   :::image type="content" source="./media/ssh-keys/upload.png" alt-text="Hochladen eines öffentlichen SSH-Schlüssels zur Speicherung in Azure":::

1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, um eine neue Ressourcengruppe zum Speichern Ihrer Schlüssel zu erstellen. Geben Sie einen Namen für Ihre Ressourcengruppe ein, und wählen Sie **OK** aus.

1. Wählen Sie unter **Region** eine Region zum Speichern Ihrer Schlüssel aus. Sie können die Schlüssel in einer beliebigen Region verwenden. Dies ist lediglich die Region, in der sie gespeichert werden.

1. Geben Sie einen Namen für Ihren Schlüssel unter **Schlüsselpaarname** ein.

1. Wählen Sie unter **Quelle für öffentlichen SSH-Schlüssel** die Option **Vorhandenen öffentlichen Schlüssel hochladen** aus. 

1. Fügen Sie den gesamten Inhalt des öffentlichen Schlüssels in **Upload key** (Schlüssel hochladen) ein, und wählen Sie anschließend die Option **Bewerten + erstellen** aus.

1. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus. 

Nachdem der Schlüssel hochgeladen wurde, können Sie ihn bei der Erstellung eines virtuellen Computers verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von SSH-Schlüsseln mit Azure-VMs finden Sie unter [Verwenden von SSH-Schlüsseln zum Herstellen einer Verbindung mit Linux-VMs](./linux/ssh-from-windows.md).
