---
title: Upgrade von Azure Dev Spaces-Tools
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Erfahren Sie, wie Sie ein Upgrade der Azure Dev Spaces-Befehlszeilentools, der Visual Studio Code-Erweiterung und der Visual Studio-Erweiterung ausführen
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
ms.openlocfilehash: ba9bb220862396c7265eb012996ae93b8208473d
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108075463"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Upgrade von Azure Dev Spaces-Tools

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

Wenn eine neue Release vorliegt und Sie Azure Dev Spaces bereits verwenden, müssen Sie die Azure Dev Spaces-Clienttools eventuell aktualisieren.

## <a name="update-the-azure-cli"></a>Aktualisieren der Azure-Befehlszeilenschnittstelle

Wenn Sie die neueste Azure-Befehlszeilenschnittstelle aktualisieren, erhalten Sie auch die neueste Version der Dev Spaces-CLI-Erweiterung.

Sie müssen die frühere Version nicht deinstallieren, sondern nur den geeigneten Download unter [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli) suchen.


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

Testen Sie die neuen Tools durch Erstellen eines neuen Clusters. Probieren Sie die Schnellstarts und Tutorials unter [Azure Dev Spaces](../index.yml) aus.
