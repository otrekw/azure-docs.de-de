---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines benutzerdefinierten Themas | Microsoft-Dokumentation'
description: In diesem Artikel wird anhand eines Beispielskripts für die Azure-Befehlszeilenschnittstelle das Erstellen eines benutzerdefinierten Azure Event Grid-Themas veranschaulicht.
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/08/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4e381310db5a21e532b262063ee33de3a23edeb2
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107766737"
---
# <a name="create-event-grid-custom-topic-with-azure-cli"></a>Erstellen eines benutzerdefinierten Event Grid-Themas mit der Azure CLI

Dieses Skript erstellt ein benutzerdefiniertes Event Grid-Thema.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/event-grid/create-custom-topic/create-custom-topic.sh "Create custom topic")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen des benutzerdefinierten Themas. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az eventgrid topic create](/cli/azure/eventgrid/topic#az_eventgrid_topic_create) | Erstellt ein benutzerdefiniertes Event Grid-Thema. |


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
