---
title: Upgrade von Azure Dev Spaces-Tools
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Erfahren Sie, wie Sie ein Upgrade der Azure Dev Spaces-Befehlszeilentools, der Visual Studio Code-Erweiterung und der Visual Studio-Erweiterung ausführen
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
ms.openlocfilehash: 1dad455b834bbef046b295b2cba040831a74f757
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873479"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Upgrade von Azure Dev Spaces-Tools

Wenn eine neue Release vorliegt und Sie Azure Dev Spaces bereits verwenden, müssen Sie die Azure Dev Spaces-Clienttools eventuell aktualisieren.

## <a name="update-the-azure-cli"></a>Aktualisieren der Azure-Befehlszeilenschnittstelle

Wenn Sie die neueste Azure-Befehlszeilenschnittstelle aktualisieren, erhalten Sie auch die neueste Version der Dev Spaces-CLI-Erweiterung.

Sie müssen die frühere Version nicht deinstallieren, sondern nur den geeigneten Download unter [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest) suchen.


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Upgrade der Dev Spaces-CLI-Erweiterung und -Befehlszeilentools

Führen Sie den folgenden Befehl aus:

```azurecli
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Aktualisieren der VS Code-Erweiterung

Nach der Installation wird die Erweiterung automatisch aktualisiert. Sie müssen die Erweiterung möglicherweise erneut laden, um die neuen Features nutzen zu können. Öffnen Sie in VS Code den Bereich **Erweiterungen**, wählen Sie die **Azure Dev Spaces-Erweiterungen** aus, und wählen Sie **Erneut laden** aus.

## <a name="update-visual-studio"></a>Aktualisieren von Visual Studio

Azure Dev Spaces ist Teil der Workload „Azure-Entwicklung“ und in allen Visual Studio-Updates enthalten.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie die neuen Tools durch Erstellen eines neuen Clusters. Probieren Sie die Schnellstarts und Tutorials unter [Azure Dev Spaces](/azure/dev-spaces) aus.
