---
title: 'Erstellen einer öffentlichen IP-Adresse: Azure CLI'
description: Hier erfahren Sie, wie Sie mithilfe der Azure CLI eine öffentliche IP-Adresse erstellen.
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: 2c469324db11d2e65f8eb958e68f77fd77020865
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2021
ms.locfileid: "99491046"
---
# <a name="quickstart-create-a-public-ip-address-using-azure-cli"></a>Schnellstart: Erstellen einer öffentlichen IP-Adresse mithilfe der Azure CLI

In diesem Artikel wird gezeigt, wie Sie mithilfe der Azure CLI eine öffentliche IP-Adressressource erstellen. Weitere Informationen zu den Ressourcen, denen diese zugeordnet werden kann, zum Unterschied zwischen der SKU „Basic“ und „Standard“ und andere zugehörige Informationen finden Sie unter [Öffentliche IP-Adressen](./public-ip-addresses.md).  In diesem Beispiel konzentrieren wir uns ausschließlich auf IPv4-Adressen. Weitere Informationen zu IPv6-Adressen finden Sie unter [IPv6 für Azure VNET](./ipv6-overview.md).

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Artikel ist mindestens Version 2.0.28 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe namens **myResourceGroup** am Speicherort **eastus2**.

```azurecli-interactive
  az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-public-ip"></a>Öffentliche IP-Adresse erstellen

---
# <a name="standard-sku---using-zones"></a>[**Standard-SKU: Verwenden von Zonen**](#tab/option-create-public-ip-standard-zones)

>[!NOTE]
>Der folgende Befehl funktioniert für die API-Version 2020-08-01 oder höher.  Weitere Informationen zur derzeit verwendeten API-Version finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).

Verwenden Sie [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create), um in **myResourceGroup** eine zonenredundante öffentliche Standard-IP-Adresse namens **myStandardZRPublicIP** zu erstellen.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myStandardZRPublicIP \
    --sku Standard
    --zone 1 2 3
```
> [!IMPORTANT]
> Führen Sie für Versionen der API, die älter als 2020-08-01 ist, den oben genannten Befehl aus, ohne einen Zonenparameter anzugeben, um eine zonenredundante IP-Adresse zu erstellen. 
>

Verwenden Sie den folgenden Befehl, um eine standardmäßige zonale öffentliche IP-Adresse in Zone 2 namens **myStandardZonalPublicIP** in **myResourceGroup** zu erstellen:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myStandardZonalPublicIP \
    --sku Standard \
    --zone 2
```

Beachten Sie, dass die oben genannten Optionen für Zonen nur eine gültige Auswahl in Regionen mit [Verfügbarkeitszonen](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones) sind.

# <a name="standard-sku---no-zones"></a>[**Standard-SKU: Keine Zonen**](#tab/option-create-public-ip-standard)

>[!NOTE]
>Der folgende Befehl funktioniert für die API-Version 2020-08-01 oder höher.  Weitere Informationen zur derzeit verwendeten API-Version finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).

Verwenden Sie [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create), um eine öffentliche Standard-IP-Adresse als nicht zonale Ressource namens **myStandardPublicIP** in **myResourceGroup** zu erstellen.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myStandardPublicIP \
    --sku Standard
```
Diese Auswahl ist in allen Regionen gültig und die Standardauswahl für öffentliche Standard-IP-Adressen in Regionen ohne [Verfügbarkeitszonen](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones).

# <a name="basic-sku"></a>[**Basic-SKU**](#tab/option-create-public-ip-basic)

Verwenden Sie [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create), um in **myResourceGroupLB** eine statische öffentliche IP-Adresse des Typs „Basic“ namens **myBasicPublicIP** zu erstellen.  Öffentliche IP-Adressen des Typs „Basic“ verwenden nicht das Konzept von Verfügbarkeitszonen.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myBasicPublicIP \
    --sku Basic \
    --allocation-method Static
```
Wenn es akzeptabel ist, dass sich die IP-Adresse im Laufe der Zeit ändert, kann **dynamische** IP-Zuweisung ausgewählt werden, indem die Zuweisungsmethode (allocation-method) in „Dynamic“ geändert wird.

---

## <a name="additional-information"></a>Zusätzliche Informationen 

Weitere Informationen zu den oben aufgeführten einzelnen Variablen finden Sie unter [Verwalten öffentlicher IP-Adressen](./virtual-network-public-ip-address.md#create-a-public-ip-address).

## <a name="next-steps"></a>Nächste Schritte
- Zuordnen einer [öffentlichen IP-Adresse zu einem virtuellen Computer](./associate-public-ip-address-vm.md#azure-portal)
- Lesen Sie mehr über [öffentliche IP-Adressen](./public-ip-addresses.md#public-ip-addresses) in Azure.
- Erfahren Sie mehr über alle [Einstellungen für öffentliche IP-Adressen](virtual-network-public-ip-address.md#create-a-public-ip-address).
