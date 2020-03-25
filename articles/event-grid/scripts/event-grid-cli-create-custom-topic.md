---
title: 'Azure CLI-Skriptbeispiel: Erstellen eines benutzerdefinierten Themas | Microsoft-Dokumentation'
description: In diesem Artikel wird anhand eines Beispielskripts für die Azure-Befehlszeilenschnittstelle das Erstellen eines benutzerdefinierten Azure Event Grid-Themas veranschaulicht.
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 2c6e027d234104d5e45c632dd73e79d8f069deb5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76720842"
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
| [az eventgrid topic create](https://docs.microsoft.com/cli/azure/eventgrid/topic#az-eventgrid-topic-create) | Erstellt ein benutzerdefiniertes Event Grid-Thema. |


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
