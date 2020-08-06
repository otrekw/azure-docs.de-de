---
title: 'Erstellen der Definition für die verwaltete Anwendung: Azure CLI'
description: Hier finden Sie ein Azure CLI-Skriptbeispiel, mit dem die Definition für eine verwaltete Anwendung im Abonnement erstellt wird.
author: tfitzmac
ms.devlang: azurecli
ms.topic: sample
ms.date: 10/25/2017
ms.author: tomfitz
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7feb00b581732cdc1956c4ac23af571180ff09e0
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497833"
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Erstellen einer Definition für die verwaltete Anwendung mit der Azure CLI

Mit diesem Skript wird eine Definition für die verwaltete Anwendung in einem Dienstkatalog veröffentlicht. 


[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen der Definition für die verwaltete Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az managedapp definition create](/cli/azure/managedapp/definition#az-managedapp-definition-create) | Erstellen Sie eine Definition für die verwaltete Anwendung. Stellen Sie das Paket bereit, das die erforderlichen Dateien enthält. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
