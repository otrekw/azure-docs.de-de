---
title: 'Azure CLI-Skript-Beispiel: Abonnieren eines benutzerdefinierten Themas | Microsoft-Dokumentation'
description: In diesem Artikel wird anhand eines Beispielskripts für die Azure-Befehlszeilenschnittstelle das Abonnieren von Event Grid-Ereignissen für ein benutzerdefiniertes Thema veranschaulicht.
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/08/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: f6004961a43fe13db617f98ae2c65fa5829924e5
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107766549"
---
# <a name="subscribe-to-events-for-a-custom-topic-with-azure-cli"></a>Abonnieren von Ereignissen für ein benutzerdefiniertes Thema mit der Azure CLI

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für ein benutzerdefiniertes Thema.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Für das Vorschaubeispielskript ist die Event Grid-Erweiterung erforderlich. Führen Sie zum Installieren `az extension add --name eventgrid` aus.

## <a name="sample-script---stable"></a>Beispielskript: stabil

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-custom-topic/subscribe-to-custom-topic.sh "Subscribe to custom topic")]

## <a name="sample-script---preview-extension"></a>Beispielskript: Vorschauerweiterung

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-custom-topic-preview/subscribe-to-custom-topic-preview.sh "Subscribe to custom topic")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription#az_eventgrid_event_subscription_create) | Erstellen Sie ein Event Grid-Abonnement. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - extension version | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
