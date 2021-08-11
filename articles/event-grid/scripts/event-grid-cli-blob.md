---
title: Azure CLI-Skriptbeispiel – Abonnieren eines Blobspeicherkontos | Microsoft-Dokumentation
description: In diesem Artikel wird anhand eines Beispielskripts für die Azure-Befehlszeilenschnittstelle das Abonnieren von Ereignissen für ein Azure Blob Storage-Konto veranschaulicht.
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/22/2021
ms.custom: devx-track-azurecli
ms.openlocfilehash: a9b00b2c042896c3eb29d4311ab5b2b4437a6f09
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114463774"
---
# <a name="subscribe-to-events-for-a-blob-storage-account-with-azure-cli"></a>Abonnieren von Ereignissen für ein Blobspeicherkonto mit der Azure CLI

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für ein Blobspeicherkonto. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-blob-storage/subscribe-to-blob-storage.sh "Subscribe to Blob storage")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription#az_eventgrid_event_subscription_create) | Erstellen Sie ein Event Grid-Abonnement. |
| [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription#az_eventgrid_event_subscription_create) - extension version | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
