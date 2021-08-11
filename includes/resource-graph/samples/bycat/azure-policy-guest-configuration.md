---
author: DCtheGeek
ms.service: resource-graph
ms.topic: include
ms.date: 07/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 97bf2986271e2c7317ffb6d1ff4dc315dcbcafe7
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114457132"
---
### <a name="count-machines-in-scope-of-guest-configuration-policies"></a>Computer im Bereich der Gastkonfigurationsrichtlinien zählen

Zeigt die Anzahl der virtuellen Azure-Computer und der mit Arc verbundenen Server im Bereich der Zuweisungen für [Azure Policy-Gastkonfiguration](../../../../articles/governance/policy/concepts/guest-configuration.md) an.

```kusto
GuestConfigurationResources
| extend vmid = split(properties.targetResourceId,'/')
| mvexpand properties.latestAssignmentReport.resources
| where properties_latestAssignmentReport_resources.resourceId != 'Invalid assignment package.'
| project machine = tostring(vmid[(-1)]),type = tostring(vmid[(-3)])
| distinct machine, type
| summarize count() by type
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | where properties_latestAssignmentReport_resources.resourceId != 'Invalid assignment package.' | project machine = tostring(vmid[(-1)]),type = tostring(vmid[(-3)]) | distinct machine, type | summarize count() by type"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | where properties_latestAssignmentReport_resources.resourceId != 'Invalid assignment package.' | project machine = tostring(vmid[(-1)]),type = tostring(vmid[(-3)]) | distinct machine, type | summarize count() by type"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20where%20properties_latestAssignmentReport_resources.resourceId%20!%3d%20%27Invalid%20assignment%20package.%27%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2ctype%20%3d%20tostring(vmid%5b(-3)%5d)%0a%7c%20distinct%20machine%2c%20type%0a%7c%20summarize%20count()%20by%20type" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20where%20properties_latestAssignmentReport_resources.resourceId%20!%3d%20%27Invalid%20assignment%20package.%27%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2ctype%20%3d%20tostring(vmid%5b(-3)%5d)%0a%7c%20distinct%20machine%2c%20type%0a%7c%20summarize%20count()%20by%20type" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20where%20properties_latestAssignmentReport_resources.resourceId%20!%3d%20%27Invalid%20assignment%20package.%27%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2ctype%20%3d%20tostring(vmid%5b(-3)%5d)%0a%7c%20distinct%20machine%2c%20type%0a%7c%20summarize%20count()%20by%20type" target="_blank">portal.azure.cn</a>

---

### <a name="count-of-non-compliant-guest-configuration-assignments"></a>Anzahl der nicht konformen Gastkonfigurationszuweisungen

Zeigt die Anzahl der nicht kompatiblen Computer pro [Gastkonfigurationszuweisungs-Grund](../../../../articles/governance/policy/how-to/determine-non-compliance.md#compliance-details-for-guest-configuration). Die Ergebnisse werden aus Leistungsgründen auf die ersten 100 beschränkt.

```kusto
GuestConfigurationResources
| extend vmid = split(properties.targetResourceId,'/')
| where properties.complianceStatus == 'NonCompliant'
| mvexpand properties.latestAssignmentReport.resources
| mvexpand properties_latestAssignmentReport_resources.reasons
| project machine = tostring(vmid[(-1)]),
    type = tostring(vmid[(-3)]),
    name,
    status = tostring(properties.complianceStatus),
    resource = tostring(properties_latestAssignmentReport_resources.resourceId),
    phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase)
| summarize count() by resource, name
| order by count_
| limit 100
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | where properties.complianceStatus == 'NonCompliant' | mvexpand properties.latestAssignmentReport.resources | mvexpand properties_latestAssignmentReport_resources.reasons | project machine = tostring(vmid[(-1)]), type = tostring(vmid[(-3)]), name, status = tostring(properties.complianceStatus), resource = tostring(properties_latestAssignmentReport_resources.resourceId), phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase) | summarize count() by resource, name | order by count_ | limit 100"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | where properties.complianceStatus == 'NonCompliant' | mvexpand properties.latestAssignmentReport.resources | mvexpand properties_latestAssignmentReport_resources.reasons | project machine = tostring(vmid[(-1)]), type = tostring(vmid[(-3)]), name, status = tostring(properties.complianceStatus), resource = tostring(properties_latestAssignmentReport_resources.resourceId), phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase) | summarize count() by resource, name | order by count_ | limit 100"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20where%20properties.complianceStatus%20%3d%3d%20%27NonCompliant%27%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2c%0a%09type%20%3d%20tostring(vmid%5b(-3)%5d)%2c%0a%09name%2c%0a%09status%20%3d%20tostring(properties.complianceStatus)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)%0a%7c%20summarize%20count()%20by%20resource%2c%20name%0a%7c%20order%20by%20count_%0a%7c%20limit%20100" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20where%20properties.complianceStatus%20%3d%3d%20%27NonCompliant%27%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2c%0a%09type%20%3d%20tostring(vmid%5b(-3)%5d)%2c%0a%09name%2c%0a%09status%20%3d%20tostring(properties.complianceStatus)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)%0a%7c%20summarize%20count()%20by%20resource%2c%20name%0a%7c%20order%20by%20count_%0a%7c%20limit%20100" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20where%20properties.complianceStatus%20%3d%3d%20%27NonCompliant%27%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2c%0a%09type%20%3d%20tostring(vmid%5b(-3)%5d)%2c%0a%09name%2c%0a%09status%20%3d%20tostring(properties.complianceStatus)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)%0a%7c%20summarize%20count()%20by%20resource%2c%20name%0a%7c%20order%20by%20count_%0a%7c%20limit%20100" target="_blank">portal.azure.cn</a>

---

### <a name="find-all-reasons-a-machine-is-non-compliant-for-guest-configuration-assignments"></a>Ermitteln aller Gründe dafür, warum ein Computer nicht konform für Gastkonfigurationszuweisungen ist

Zeigt alle [Gastkonfigurationszuweisungs-Gründe](../../../../articles/governance/policy/how-to/determine-non-compliance.md#compliance-details-for-guest-configuration) für einen bestimmten Computer an. Entfernen Sie die erste `where`-Klausel, um auch Überwachungen mit konformen Computern einzubeziehen.

```kusto
GuestConfigurationResources
| where properties.complianceStatus == 'NonCompliant'
| extend vmid = split(properties.targetResourceId,'/')
| mvexpand properties.latestAssignmentReport.resources
| mvexpand properties_latestAssignmentReport_resources.reasons
| extend machine = tostring(vmid[(-1)])
| where machine == 'MACHINENAME'
| project phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase),
    resource = tostring(properties_latestAssignmentReport_resources.resourceId),
    name,
    machine,
    resourceGroup,
    subscriptionId
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "GuestConfigurationResources | where properties.complianceStatus == 'NonCompliant' | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | mvexpand properties_latestAssignmentReport_resources.reasons | extend machine = tostring(vmid[(-1)]) | where machine == 'MACHINENAME' | project phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase), resource = tostring(properties_latestAssignmentReport_resources.resourceId), name, machine, resourceGroup, subscriptionId"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "GuestConfigurationResources | where properties.complianceStatus == 'NonCompliant' | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | mvexpand properties_latestAssignmentReport_resources.reasons | extend machine = tostring(vmid[(-1)]) | where machine == 'MACHINENAME' | project phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase), resource = tostring(properties_latestAssignmentReport_resources.resourceId), name, machine, resourceGroup, subscriptionId"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20where%20properties.complianceStatus%20%3d%3d%20%27NonCompliant%27%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20extend%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%0a%7c%20where%20machine%20%3d%3d%20%27MACHINENAME%27%0a%7c%20project%20phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09name%2c%0a%09machine%2c%0a%09resourceGroup%2c%0a%09subscriptionId" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20where%20properties.complianceStatus%20%3d%3d%20%27NonCompliant%27%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20extend%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%0a%7c%20where%20machine%20%3d%3d%20%27MACHINENAME%27%0a%7c%20project%20phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09name%2c%0a%09machine%2c%0a%09resourceGroup%2c%0a%09subscriptionId" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20where%20properties.complianceStatus%20%3d%3d%20%27NonCompliant%27%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20extend%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%0a%7c%20where%20machine%20%3d%3d%20%27MACHINENAME%27%0a%7c%20project%20phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09name%2c%0a%09machine%2c%0a%09resourceGroup%2c%0a%09subscriptionId" target="_blank">portal.azure.cn</a>

---

### <a name="query-details-of-guest-configuration-assignment-reports"></a>Abfragen von Details aus dem Bericht zur Gastkonfigurationszuweisung

Zeigt den Bericht aus den Details für den [Gastkonfigurationszuweisungs-Grund](../../../../articles/governance/policy/how-to/determine-non-compliance.md#compliance-details-for-guest-configuration) an. Im folgenden Beispiel gibt die Abfrage nur Ergebnisse zurück, bei denen der Name der Gastzuweisung `installed_application_linux` lautet und die Ausgabe die Zeichenfolge `Python` enthält, um alle Linux-Computer aufzulisten, auf denen ein Paket mit dem Namen **Python** installiert ist. Entfernen Sie die zweite `where`-Klausel, um die Konformität aller Computer für eine bestimmte Zuweisung abzufragen.

```kusto
GuestConfigurationResources
| extend vmid = split(properties.targetResourceId,'/')
| mvexpand properties.latestAssignmentReport.resources
| mvexpand properties_latestAssignmentReport_resources.reasons
| where name in ('installed_application_linux')
| where properties_latestAssignmentReport_resources_reasons.phrase contains 'Python'
| project machine = tostring(vmid[(-1)]),
    type = tostring(vmid[(-3)]),
    name,
    status = tostring(properties.complianceStatus),
    resource = tostring(properties_latestAssignmentReport_resources.resourceId),
    phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase)
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | mvexpand properties_latestAssignmentReport_resources.reasons | where name in ('installed_application_linux') | where properties_latestAssignmentReport_resources_reasons.phrase contains 'Python' | project machine = tostring(vmid[(-1)]), type = tostring(vmid[(-3)]), name, status = tostring(properties.complianceStatus), resource = tostring(properties_latestAssignmentReport_resources.resourceId), phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | mvexpand properties_latestAssignmentReport_resources.reasons | where name in ('installed_application_linux') | where properties_latestAssignmentReport_resources_reasons.phrase contains 'Python' | project machine = tostring(vmid[(-1)]), type = tostring(vmid[(-3)]), name, status = tostring(properties.complianceStatus), resource = tostring(properties_latestAssignmentReport_resources.resourceId), phrase = tostring(properties_latestAssignmentReport_resources_reasons.phrase)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20where%20name%20in%20(%27installed_application_linux%27)%0a%7c%20where%20properties_latestAssignmentReport_resources_reasons.phrase%20contains%20%27Python%27%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2c%0a%09type%20%3d%20tostring(vmid%5b(-3)%5d)%2c%0a%09name%2c%0a%09status%20%3d%20tostring(properties.complianceStatus)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20where%20name%20in%20(%27installed_application_linux%27)%0a%7c%20where%20properties_latestAssignmentReport_resources_reasons.phrase%20contains%20%27Python%27%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2c%0a%09type%20%3d%20tostring(vmid%5b(-3)%5d)%2c%0a%09name%2c%0a%09status%20%3d%20tostring(properties.complianceStatus)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%0a%7c%20extend%20vmid%20%3d%20split(properties.targetResourceId%2c%27%2f%27)%0a%7c%20mvexpand%20properties.latestAssignmentReport.resources%0a%7c%20mvexpand%20properties_latestAssignmentReport_resources.reasons%0a%7c%20where%20name%20in%20(%27installed_application_linux%27)%0a%7c%20where%20properties_latestAssignmentReport_resources_reasons.phrase%20contains%20%27Python%27%0a%7c%20project%20machine%20%3d%20tostring(vmid%5b(-1)%5d)%2c%0a%09type%20%3d%20tostring(vmid%5b(-3)%5d)%2c%0a%09name%2c%0a%09status%20%3d%20tostring(properties.complianceStatus)%2c%0a%09resource%20%3d%20tostring(properties_latestAssignmentReport_resources.resourceId)%2c%0a%09phrase%20%3d%20tostring(properties_latestAssignmentReport_resources_reasons.phrase)" target="_blank">portal.azure.cn</a>

---

