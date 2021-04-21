---
title: 'Über die CLI: Manuelles Skalieren einer App'
description: Hier erfahren Sie, wie Sie die Azure CLI zum Automatisieren der Bereitstellung und Verwaltung Ihrer App Service-App verwenden. In diesem Beispiel wird gezeigt, wie Sie eine App manuell skalieren.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 0c2dbd7368cb8cc43c069b54d286e3e7f4f733c6
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107787645"
---
# <a name="scale-an-app-service-app-manually-using-azure-cli"></a>Manuelles Skalieren einer App Service-App mithilfe der Azure CLI

In diesem Beispielskript werden eine Ressourcengruppe, ein App Service-Plan und eine App erstellt. Anschließend wird der App Service-Plan von einer auf mehrere Instanzen skaliert.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Für dieses Tutorial ist mindestens Version 2.0 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine App Service-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [`az group create`](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) | Erstellt einen App Service-Plan. |
| [`az webapp create`](/cli/azure/webapp#az_webapp_create) | Erstellt eine App Service-App. |
| [`az appservice plan update`](/cli/azure/appservice/plan#az_appservice_plan_update) | Aktualisiert Eigenschaften des App Service-Plans. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../samples-cli.md).
