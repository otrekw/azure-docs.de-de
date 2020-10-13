---
title: Konfigurieren von VM-Skalierungsgruppen mit einer vorhandenen Azure Load Balancer-Instanz – Azure CLI
description: Hier erfahren Sie, wie Sie eine VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz mithilfe der Azure CLI konfigurieren.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: 990380d553cc12d1a87b2e1c7ca9b09864801294
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91333984"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-cli"></a>Konfigurieren einer VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz mithilfe der Azure CLI

In diesem Artikel erfahren Sie, wie Sie eine VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz konfigurieren. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement.
- Ein vorhandener Standard-SKU-Lastenausgleich in dem Abonnement, in dem die VM-Skalierungsgruppe bereitgestellt werden soll.
- Eine Azure Virtual Network-Instanz für die VM-Skalierungsgruppe.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Wenn Sie sich für die lokale Verwendung der Befehlszeilenschnittstelle entscheiden, setzt dieser Artikel voraus, dass Sie die Azure CLI-Version 2.0.28 oder höher installiert haben. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="sign-in-to-azure-cli"></a>Anmelden bei der Azure-Befehlszeilenschnittstelle

Melden Sie sich bei Azure an.

```azurecli-interactive
az login
```

## <a name="deploy-a-virtual-machine-scale-set-with-existing-load-balancer"></a>Bereitstellen einer VM-Skalierungsgruppe mit einem vorhandenen Lastenausgleich

Ersetzen Sie die Werte in den spitzen Klammern durch die Namen der Ressourcen in Ihrer Konfiguration.

```azurecli-interactive
az vmss create \
    --resource-group <resource-group> \
    --name <vmss-name>\
    --image <your-image> \
    --admin-username <admin-username> \
    --generate-ssh-keys  \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name <virtual-network-name> \
    --subnet <subnet-name> \
    --lb <load-balancer-name> \
    --backend-pool-name <backend-pool-name>
```

Im folgenden Beispiel wird eine VM-Skalierungsgruppe mit den folgenden Ressourcen bereitgestellt:

- Eine VM-Skalierungsgruppe namens **myVMSS**
- Ein Azure Load Balancer namens **myLoadBalancer**
- Ein Back-End-Pool für den Lastenausgleich namens **myBackendPool**
- Ein virtuelles Azure-Netzwerk namens **myVnet**
- Ein Subnetz namens **mySubnet**
- Eine Ressourcengruppe namens **myResourceGroup**
- Ubuntu-Serverimage für die VM-Skalierungsgruppe

```azurecli-interactive
az vmss create \
    --resource-group myResourceGroup \
    --name myVMSS \
    --image Canonical:UbuntuServer:18.04-LTS:latest \
    --admin-username adminuser \
    --generate-ssh-keys \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name myVnet\
    --subnet mySubnet \
    --lb myLoadBalancer \
    --backend-pool-name myBackendPool
```
> [!NOTE]
> Nachdem die Skalierungsgruppe erstellt wurde, kann der Back-End-Port für eine Lastenausgleichsregel, die von einem Integritätstest des Load Balancers verwendet wird, nicht mehr geändert werden. Zum Ändern des Ports können Sie den Integritätstest entfernen, indem Sie die Azure-VM-Skalierungsgruppe aktualisieren, den Port aktualisieren und dann den Integritätstest erneut konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz konfiguriert.  Weitere Informationen über VM-Skalierungsgruppen und Lastenausgleiche finden Sie unter:

- [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md)
- [Was sind Skalierungsgruppen für virtuelle Computer?](../virtual-machine-scale-sets/overview.md)
                                