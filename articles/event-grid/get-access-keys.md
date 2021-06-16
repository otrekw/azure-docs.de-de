---
title: Abrufen des Zugriffsschlüssels für eine Event Grid-Ressource
description: In diesem Artikel wird beschrieben, wie Sie den Zugriffsschlüssel für ein Event Grid-Thema oder eine Event Grid-Domäne abrufen.
ms.topic: how-to
ms.date: 07/07/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6052c93dc91403fccaed62719b163c24486557c5
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110702525"
---
# <a name="get-access-keys-for-event-grid-resources-topics-or-domains"></a>Abrufen von Zugriffsschlüsseln für Event Grid-Ressourcen (Themen oder Domänen)
Zugriffsschlüssel werden verwendet, um ein Anwendungsveröffentlichungsereignis für Azure Event Grid-Ressourcen (Themen und Domänen) zu authentifizieren. Wir empfehlen Ihnen, Ihre Schlüssel regelmäßig neu zu generieren und sie sicher zu speichern. Sie erhalten zwei Zugriffsschlüssel, sodass Sie Verbindungen aufrechterhalten können, während der andere Schlüssel neu generiert wird.

In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal, mit PowerShell oder der CLI Zugriffsschlüssel für eine Event Grid-Ressource (Thema oder Domäne) abrufen. 

## <a name="azure-portal"></a>Azure-Portal
Wechseln Sie im Azure-Portal zur Registerkarte **Zugriffsschlüssel** der Seite **Event Grid-Thema** oder **Event Grid-Domäne** für Ihr Thema oder Ihre Domäne.  

:::image type="content" source="./media/get-access-keys/azure-portal.png" alt-text="Seite „Zugriffsschlüssel“":::

## <a name="azure-powershell"></a>Azure PowerShell
Verwenden Sie den Befehl[Get-AzEventGridTopicKey](/powershell/module/az.eventgrid/get-azeventgridtopickey), um Zugriffsschlüssel für Themen abzurufen. 

```azurepowershell-interactive
Get-AzEventGridTopicKey -ResourceGroup <RESOURCE GROUP NAME> -Name <TOPIC NAME>
```

Verwenden Sie den Befehl [Get-AzEventGridDomainKey](/powershell/module/az.eventgrid/get-azeventgriddomainkey), um Zugriffsschlüssel für Domänen abzurufen. 

```azurepowershell-interactive
Get-AzEventGridDomainKey -ResourceGroup <RESOURCE GROUP NAME> -Name <DOMAIN NAME>
```

## <a name="azure-cli"></a>Azure CLI
Verwenden Sie [az eventgrid topic key list](/cli/azure/eventgrid/topic/key#az_eventgrid_topic_key_list), um Zugriffsschlüssel für Themen abzurufen. 

```azurecli-interactive
az eventgrid topic key list --resource-group <RESOURCE GROUP NAME> --name <TOPIC NAME>
```

Verwenden Sie [az eventgrid domain key list](/cli/azure/eventgrid/domain/key#az_eventgrid_domain_key_list), um Zugriffsschlüssel für Domänen abzurufen. 

```azurecli-interactive
az eventgrid domain key list --resource-group <RESOURCE GROUP NAME> --name <DOMAIN NAME>
```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden Artikel: [Authentifizieren von Veröffentlichungsclients](security-authenticate-publishing-clients.md). 
