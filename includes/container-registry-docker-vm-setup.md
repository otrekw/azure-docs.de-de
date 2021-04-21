---
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 05/07/2020
ms.author: danlep
ms.openlocfilehash: 429377cd50e83195cb1c3a422416fdb35644a28e
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107773464"
---
## <a name="create-a-docker-enabled-virtual-machine"></a>Erstellen eines Docker-fähigen virtuellen Computers

Verwenden Sie zu Testzwecken einen Docker-fähigen virtuellen Ubuntu-Computer für den Zugriff auf eine Azure-Containerregistrierung. Damit für die Registrierung die Azure Active Directory-Authentifizierung verwendet werden kann, installieren Sie auch die [Azure-Befehlszeilenschnittstelle (CLI)][azure-cli] auf dem virtuellen Computer. Wenn Sie bereits über einen virtuellen Azure-Computer verfügen, überspringen Sie diesen Erstellungsschritt.

Sie können die gleiche Ressourcengruppe für Ihren virtuellen Computer und Ihre Containerregistrierung verwenden. Dieses Setup vereinfacht die Bereinigung am Übungsende, ist aber nicht erforderlich. Wenn Sie eine separate Ressourcengruppe für den virtuellen Computer und das virtuelle Netzwerk erstellen möchten, führen Sie den Befehl [az group create][az-group-create] aus. Das folgende Beispiel setzt voraus, dass Sie Umgebungsvariablen für den Ressourcengruppennamen und den Speicherort der Registrierung festgelegt haben:

```azurecli
az group create --name $RESOURCE_GROUP --location $REGISTRY_LOCATION
```

Stellen Sie nun mithilfe von [az vm create][az-vm-create] einen virtuellen Azure-Standardcomputer mit Ubuntu bereit. Im folgenden Beispiel wird ein virtueller Computer namens *myDockerVM* erstellt:

```azurecli
VM_NAME=myDockerVM

az vm create \
  --resource-group $RESOURCE_GROUP \
  --name $VM_NAME \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Notieren Sie nach Abschluss der Befehlsausführung die von der Azure CLI angezeigte `publicIpAddress`. Verwenden Sie diese Adresse, um SSH-Verbindungen mit der VM herzustellen.

### <a name="install-docker-on-the-vm"></a>Installieren von Docker auf der VM

Wenn die VM ausgeführt wird, stellen Sie eine SSH-Verbindung mit der VM her. Ersetzen Sie *publicIpAddress* durch die öffentliche IP-Adresse Ihrer VM.

```bash
ssh azureuser@publicIpAddress
```

Führen Sie die folgenden Befehle aus, um Docker auf dem virtuellen Ubuntu-Computer zu installieren:

```bash
sudo apt-get update
sudo apt install docker.io -y
```

Führen Sie nach der Installation den folgenden Befehl aus, um sicherzustellen, dass Docker ordnungsgemäß auf der VM ausgeführt wird:

```bash
sudo docker run -it hello-world
```

Ausgabe:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Installieren der Azure CLI

Führen Sie die in [Installieren der Azure CLI mit apt](/cli/azure/install-azure-cli-apt) beschriebenen Schritte aus, um die Azure CLI auf Ihrer Ubuntu-VM zu installieren. Beispiel:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Beenden Sie die SSH-Verbindung.

[azure-cli]: /cli/azure/install-azure-cli
[az-vm-create]: /cli/azure/vm#az_vm_create
[az-group-create]: /cli/azure/group