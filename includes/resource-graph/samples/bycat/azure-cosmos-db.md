---
author: DCtheGeek
ms.service: resource-graph
ms.topic: include
ms.date: 07/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: b2facd700fa0bf5943cca679f9dac6616f27b8f8
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114457117"
---
### <a name="list-cosmos-db-with-specific-write-locations"></a>Auflisten von Cosmos DB mit bestimmten Schreibstandorten

Die folgende Abfrage schränkt die Cosmos DB-Ressourcen ein, verwendet `mv-expand`, um den Eigenschaftenbehälter für **properties.writeLocations** zu erweitern, anschließend bestimmte Felder zu projizieren und die Ergebnisse weiter auf **properties.writeLocations.locationName**-Werte einzuschränken, die „USA, Osten“ oder „USA, Westen“ entsprechen.

```kusto
Resources
| where type =~ 'microsoft.documentdb/databaseaccounts'
| project id, name, writeLocations = (properties.writeLocations)
| mv-expand writeLocations
| project id, name, writeLocation = tostring(writeLocations.locationName)
| where writeLocation in ('East US', 'West US')
| summarize by id, name
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.documentdb/databaseaccounts' | project id, name, writeLocations = (properties.writeLocations) | mv-expand writeLocations | project id, name, writeLocation = tostring(writeLocations.locationName) | where writeLocation in ('East US', 'West US') | summarize by id, name"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.documentdb/databaseaccounts' | project id, name, writeLocations = (properties.writeLocations) | mv-expand writeLocations | project id, name, writeLocation = tostring(writeLocations.locationName) | where writeLocation in ('East US', 'West US') | summarize by id, name"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.documentdb%2fdatabaseaccounts%27%0a%7c%20project%20id%2c%20name%2c%20writeLocations%20%3d%20(properties.writeLocations)%0a%7c%20mv-expand%20writeLocations%0a%7c%20project%20id%2c%20name%2c%20writeLocation%20%3d%20tostring(writeLocations.locationName)%0a%7c%20where%20writeLocation%20in%20(%27East%20US%27%2c%20%27West%20US%27)%0a%7c%20summarize%20by%20id%2c%20name" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.documentdb%2fdatabaseaccounts%27%0a%7c%20project%20id%2c%20name%2c%20writeLocations%20%3d%20(properties.writeLocations)%0a%7c%20mv-expand%20writeLocations%0a%7c%20project%20id%2c%20name%2c%20writeLocation%20%3d%20tostring(writeLocations.locationName)%0a%7c%20where%20writeLocation%20in%20(%27East%20US%27%2c%20%27West%20US%27)%0a%7c%20summarize%20by%20id%2c%20name" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.documentdb%2fdatabaseaccounts%27%0a%7c%20project%20id%2c%20name%2c%20writeLocations%20%3d%20(properties.writeLocations)%0a%7c%20mv-expand%20writeLocations%0a%7c%20project%20id%2c%20name%2c%20writeLocation%20%3d%20tostring(writeLocations.locationName)%0a%7c%20where%20writeLocation%20in%20(%27East%20US%27%2c%20%27West%20US%27)%0a%7c%20summarize%20by%20id%2c%20name" target="_blank">portal.azure.cn</a>

---

