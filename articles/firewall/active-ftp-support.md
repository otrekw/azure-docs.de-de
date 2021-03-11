---
title: Aktive FTP-Unterstützung in Azure Firewall
description: Standardmäßig ist aktives FTP in Azure Firewall deaktiviert. Sie können es mithilfe der PowerShell, CLI und ARM-Vorlage aktivieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 03/05/2021
ms.author: victorh
ms.openlocfilehash: adbc2a9eb6cd3b054df84911604143ddb711ad20
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102499134"
---
# <a name="azure-firewall-active-ftp-support"></a>Aktive FTP-Unterstützung in Azure Firewall

Bei aktivem FTP initiiert der FTP-Server die Datenverbindung mit dem vorgesehenen Datenport des FTP-Clients. Firewalls im clientseitigen Netzwerk blockieren normalerweise eine externe Verbindungsanforderung an einen internen Clientport. Weitere Informationen finden Sie unter [Aktives FTP und Passives FTP, eine definitive Erläuterung](https://slacksite.com/other/ftp.html).

Standardmäßig ist die Unterstützung für aktives FTP zum Schutz vor FTP-Bounce-Angriffen in Azure Firewall mithilfe des FTP-Befehls `PORT` deaktiviert. Sie können jedoch aktives FTP aktivieren, wenn Sie Azure PowerShell, die Azure-Befehlszeilenschnittstelle oder eine Azure ARM-Vorlage verwenden.


## <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie zur Bereitstellung mithilfe von Azure PowerShell den Parameter `AllowActiveFTP`. Weitere Informationen finden Sie unter [Erstellen einer Firewall durch Zulassen von aktivem FTP](/powershell/module/az.network/new-azfirewall#16---create-a-firewall-with-allow-active-ftp-).

## <a name="azure-cli"></a>Azure CLI

Verwenden Sie zur Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle den Parameter `--allow-active-ftp`. Weitere Informationen finden Sie unter [az network firewall create](/cli/azure/ext/azure-firewall/network/firewall#ext_azure_firewall_az_network_firewall_create-optional-parameters). 

## <a name="azure-resource-manager-arm-template"></a>ARM-Vorlage (Azure Resource Manager)

Für die Bereitstellung mithilfe einer ARM-Vorlage verwenden Sie das Feld `AdditionalProperties`:

```json
"additionalProperties": {
            "Network.FTP.AllowActiveFTP": "True"
        },
```
Weitere Informationen finden Sie unter [Microsoft.Network azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen einer Azure Firewall finden Sie unter [Bereitstellen und Konfigurieren von Azure Firewall mithilfe von Azure PowerShell](deploy-ps.md).