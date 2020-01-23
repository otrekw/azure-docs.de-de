---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines ACS-DC/OS-Clusters'
description: 'Azure CLI-Skriptbeispiel: Erstellen eines ACS-DC/OS-Clusters'
author: iainfoulds
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: ''
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: b01d8d58da5a25ca9aa3d1ac16f10495fde8fc2b
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76270542"
---
# <a name="deprecated-create-an-azure-container-service-dcos-cluster"></a>(VERALTET) Erstellen eines Azure Container Service-DC/OS-Clusters

[!INCLUDE [ACS deprecation](../../../../includes/container-service-kubernetes-deprecation.md)]

In diesem Beispiel wird ein Azure Container Service-Cluster erstellt, der DCOS ausführt.

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az acs create](https://docs.microsoft.com/cli/azure/acs#az-acs-create) | Erstellt einen ACS-Cluster. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere CLI-Skriptbeispiele von Azure Container Service finden Sie in der [Dokumentation zu Azure Container Service](../cli-samples.md).
