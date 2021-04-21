---
title: Azure CLI-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät
description: Azure CLI-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Firewall-Netzwerkgerät.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7970ab4472000c53e23f7962a9cbf4ec05ea3465
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107763179"
---
# <a name="use-an-azure-cli-script-to-route-traffic-through-a-network-virtual-appliance"></a>Verwenden Sie ein Azure CLI-Skript zum Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät.

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Es wird auch ein virtueller Computer mit IP-Weiterleitung erstellt, der zur Weiterleitung von Datenverkehr zwischen den zwei Subnetzen aktiviert ist. Nach dem Ausführen des Skripts können Sie dem virtuellen Computer Netzwerksoftware bereitstellen, z.B. eine Firewallanwendung.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Beispielskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Erstellt Back-End- und DMZ-Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [az network nic create](/cli/azure/network/nic) | Erstellt eine virtuelle Netzwerkschnittstelle und aktiviert die IP-Weiterleitung dafür. |
| [az network nsg create](/cli/azure/network/nsg) | Erstellt eine Netzwerksicherheitsgruppe (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule) | Erstellt NSG-Regeln, die eingehende HTTP- und HTTPS-Ports für den virtuellen Computer ermöglichen. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet)| Ordnet die NSGs und Routentabellen Subnetzen zu. |
| [az network route-table create](/cli/azure/network/route-table#az_network_route_table_create)| Erstellt eine Routingtabelle für alle Routen. |
| [az network route-table route create](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Erstellt die Routen zum Weiterleiten von Datenverkehr zwischen Subnetzen und dem Internet über den virtuellen Computer. |
| [az vm create](/cli/azure/vm) | Erstellt einen virtuellen Computer und fügt ihm eine NIC an. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [az group delete](/cli/azure/group) | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche Netzwerk-CLI-Skriptbeispiele finden Sie unter [Azure CLI Samples for networking](../cli-samples.md) (Azure CLI-Beispiele für Netzwerke).
