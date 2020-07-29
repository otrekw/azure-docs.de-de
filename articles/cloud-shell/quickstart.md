---
title: Schnellstart für Azure Cloud Shell – Bash
description: Erfahren Sie, wie Sie Bash-Befehlszeile in Ihrem Browser mit Azure Cloud Shell verwenden.
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: 3001e814a5f31725bfc78385d2435bbb0e971d40
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513018"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Schnellstart für Bash in Azure Cloud Shell

Dieses Dokument erläutert die Verwendung von Bash in Azure Cloud Shell im [Azure-Portal](https://ms.portal.azure.com/).

> [!NOTE]
> Ein Schnellstart zu [PowerShell in Azure Cloud Shell](quickstart-powershell.md) ist ebenfalls verfügbar.

## <a name="start-cloud-shell"></a>Starten von Cloud Shell
1. Starten Sie **Cloud Shell** über den oberen Navigationsbereich im Azure-Portal. <br>
![Screenshot: Starten von Azure Cloud Shell im Azure-Portal.](media/quickstart/shell-icon.png)

2. Wählen Sie ein Abonnement aus, in dem ein Speicherkonto und eine Microsoft Azure Files-Freigabe erstellt werden sollen.
3. Wählen Sie „Speicher erstellen“ aus.

> [!TIP]
> Sie werden in jeder Sitzung automatisch bei Azure CLI authentifiziert.

### <a name="select-the-bash-environment"></a>Auswählen der Bash-Umgebung
Stellen Sie sicher, dass auf der linken Seite des Shellfensters im Dropdownmenü für Umgebungen die Option `Bash` ausgewählt ist. <br>
![Screenshot: Auswählen der Bash-Umgebung für die Azure Cloud Shell.](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Festlegen des Abonnements
1. Führen Sie die Abonnements auf, auf die Sie Zugriff haben.
   ```azurecli-interactive
   az account list
   ```

2. Legen Sie Ihr bevorzugtes Abonnement fest:

   ```azurecli-interactive
   az account set --subscription 'my-subscription-name'
   ```

> [!TIP]
> Ihr Abonnement wird mithilfe von `/home/<user>/.azure/azureProfile.json` für künftige Sitzungen gespeichert.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie in „USA, Westen“ eine neue Ressourcengruppe mit dem Namen „MyRG“.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Erstellen eines virtuellen Linux-Computers
Erstellen Sie einen virtuellen Ubuntu-Computer in Ihrer neuen Ressourcengruppe. Azure CLI erstellt SSH-Schlüssel und richtet den virtuellen Computer damit ein. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Durch die Verwendung von `--generate-ssh-keys` wird Azure CLI angewiesen, auf Ihrer VM und im Verzeichnis `$Home` öffentliche und private Schlüssel zu erstellen und einzurichten. Standardmäßig befinden sich Schlüssel in der Cloud Shell unter `/home/<user>/.ssh/id_rsa` und `/home/<user>/.ssh/id_rsa.pub`. Ihr Ordner `.ssh` wird im 5-GB-Image Ihrer angefügten Dateifreigabe zur dauerhaften Aufbewahrung von `$Home` gespeichert.

Ihr Benutzername auf diesem virtuellen Computer ist der Benutzername, der in Cloud Shell verwendet wird ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>Herstellen einer SSH-Verbindung mit Ihrem virtuellen Linux-Computer
1. Suchen Sie über die Suchleiste des Azure-Portals nach dem Namen Ihres virtuellen Computers.
2. Klicken Sie auf „Verbinden“, um Ihren VM-Namen und die öffentliche IP-Adresse abzurufen. <br>
   ![Screenshot: Herstellen einer Verbindung mit einer Linux-VM über SSH.](media/quickstart/sshcmd-copy.png)

3. Stellen Sie mit dem Befehl `ssh` eine SSH-Verbindung mit Ihrer VM her.
   ```
   ssh username@ipaddress
   ```

Nach dem Herstellen der SSH-Verbindung sollte der Willkommensbildschirm von Ubuntu angezeigt werden. <br>
![Screenshot: Ubuntu-Initialisierung und -Willkommensbildschirm nach dem Herstellen einer SSH-Verbindung.](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Bereinigen 
1. Beenden Sie Ihre SSH-Sitzung.
   ```
   exit
   ```

2. Löschen Sie Ihre Ressourcengruppe und alle darin befindlichen Ressourcen.
   ```azurecli-interactive
   az group delete -n MyRG
   ```

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über das Beibehalten von Dateien für Bash in Cloud Shell](persisting-shell-storage.md) <br>
[Erfahren Sie mehr über die Azure CLI](https://docs.microsoft.com/cli/azure/) <br>
[Erfahren Sie mehr über Azure Files-Speicher.](../storage/files/storage-files-introduction.md) <br>
