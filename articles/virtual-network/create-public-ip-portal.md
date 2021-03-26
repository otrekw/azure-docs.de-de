---
title: 'Erstellen einer öffentlichen IP-Adresse: Azure-Portal'
description: Erfahren Sie, wie Sie eine öffentliche IP-Adresse im Azure-Portal erstellen.
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.subservice: ip-services
ms.topic: how-to
ms.date: 02/22/2021
ms.author: allensu
ms.openlocfilehash: e6b7648188e2307da4ef40e0ab3daf6201f9d89d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101694868"
---
# <a name="create-a-public-ip-address-using-the-azure-portal"></a>Erstellen einer öffentlichen IP-Adresse im Azure-Portal

In diesem Artikel wird gezeigt, wie Sie mithilfe des Azure-Portals eine öffentliche IP-Adressressource erstellen. 

Weitere Informationen zu den Ressourcen, denen diese öffentliche IP-Adresse zugeordnet werden kann, sowie zu den Unterschieden zwischen SKUs des Typs „Basic“ und „Standard“ finden Sie unter [Öffentliche IP-Adressen](./public-ip-addresses.md). 

Dieser Artikel konzentriert sich auf IPv4-Adressen. Weitere Informationen zu IPv6-Adressen finden Sie unter [IPv6 für Azure VNET](./ipv6-overview.md).

# <a name="standard-sku"></a>[**Standard-SKU**](#tab/option-create-public-ip-standard-zones)

Verwenden Sie die folgenden Schritte, um eine zonenredundante öffentliche IP-Standardadresse namens **myStandardZRPublicIP** zu erstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Ressource erstellen**. 
3. Geben Sie im Suchfeld **Öffentliche IP-Adresse** ein. Wählen Sie in den Suchergebnissen **Öffentliche IP-Adresse** aus.
4. Wählen Sie auf der Seite **Öffentliche IP-Adresse** die Option **Erstellen** aus.
5. Geben Sie auf der Seite **Öffentliche IP-Adresse erstellen** die folgenden Informationen ein, oder wählen Sie sie aus: 

    | Einstellung                 | Wert                       |
    | ---                     | ---                         |
    | IP-Version              | Wählen Sie „IPv4“ aus.                 |    
    | SKU                     | Wählen Sie **Standard** aus.         |
    | Tarif*                   | Wählen Sie **Regional** aus.         |
    | Name                    | Geben Sie **myStandardZRPublicIP** ein.          |
    | IP-Adresszuweisung   | Beachten Sie, dass diese Auswahl als „statisch“ gesperrt wird.                                        |
    | Routingpräferenz      | Übernehmen Sie die Standardeinstellung **Microsoft-Netzwerk**. </br> Weitere Informationen zu den Routingpräferenzen finden Sie unter [Was ist Routingpräferenz (Vorschau)?](./routing-preference-overview.md) |
    | Leerlaufzeitüberschreitung (Minuten)  | Übernehmen Sie den Standardwert **4**.        |
    | DNS-Namensbezeichnung          | Lassen Sie den Wert leer.    |
    | Subscription            | Wählen Sie Ihr Abonnement aus.   |
    | Ressourcengruppe          | Wählen Sie **Neue erstellen** aus, und geben Sie **myResourceGroup** ein. </br> Klicken Sie auf **OK**. |
    | Standort                | Wählen Sie **USA, Osten 2** aus.      |
    | Verfügbarkeitszone       | Wählen Sie **Zonenredundant** aus, wählen Sie keine Zone aus, oder wählen Sie eine bestimmte Zone aus (siehe Hinweis unten). |

:::image type="content" source="./media/create-public-ip-portal/create-standard-ip.png" alt-text="Erstellen einer Standard-IP-Adresse im Azure-Portal" border="false":::

> [!NOTE]
> Diese Auswahl gilt für Regionen mit [Verfügbarkeitszonen](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones). </br>
Sie können in diesen Regionen eine bestimmte Zone auswählen, auch wenn dies nicht vor zonenbasierten Ausfällen schützt. </br> Weitere Informationen zu Verfügbarkeitszonen finden Sie in der [Übersicht über Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).

\* = Tarif bezieht sich auf die Funktion [Regionsübergreifender Lastenausgleich](../load-balancer/cross-region-overview.md), die derzeit in der Vorschau erhältlich ist.

# <a name="basic-sku"></a>[**Basic-SKU**](#tab/option-create-public-ip-basic)

Erstellen Sie in diesem Abschnitt eine öffentliche IP-Adresse des Typs „Basic“ mit dem Namen **myBasicPublicIP**. 

> [!NOTE]
> Öffentliche IP-Adressen des Typs „Basic“ unterstützen keine Verfügbarkeitszonen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Ressource erstellen**. 
3. Geben Sie im Suchfeld **Öffentliche IP-Adresse** ein. Wählen Sie in den Suchergebnissen **Öffentliche IP-Adresse** aus.
4. Wählen Sie auf der Seite **Öffentliche IP-Adresse** die Option **Erstellen** aus.
5. Geben Sie auf der Seite **Öffentliche IP-Adresse erstellen** die folgenden Informationen ein, oder wählen Sie sie aus: 

    | Einstellung                 | Wert                       |
    | ---                     | ---                         |
    | IP-Version              | Wählen Sie „IPv4“ aus.                 |    
    | SKU                     | Wählen Sie **Basic** aus.         |
    | Name                    | Geben Sie *myBasicPublicIP* ein.          |
    | IP-Adresszuweisung   | Wählen Sie **Statisch** aus (siehe Hinweis unten).                                     |
    | Leerlaufzeitüberschreitung (Minuten)  | Übernehmen Sie den Standardwert **4**.       |
    | DNS-Namensbezeichnung          | Lassen Sie den Wert leer.    |
    | Subscription            | Wählen Sie Ihr Abonnement aus.   |
    | Ressourcengruppe          | Wählen Sie **Neue erstellen** aus, und geben Sie **myResourceGroup** ein. </br> Klicken Sie auf **OK**. |
    | Standort                | Wählen Sie **USA, Osten 2** aus.      |

:::image type="content" source="./media/create-public-ip-portal/create-basic-ip.png" alt-text="Erstellen einer Standard-IP-Adresse im Azure-Portal" border="false":::

Wenn es zulässig ist, dass sich die IP-Adresse im Laufe der Zeit ändert, können Sie die **dynamische** IP-Zuweisung auswählen.

---

## <a name="additional-information"></a>Zusätzliche Informationen 

Weitere Informationen zu den oben aufgeführten einzelnen Feldern finden Sie unter [Verwalten öffentlicher IP-Adressen](./virtual-network-public-ip-address.md#create-a-public-ip-address).

## <a name="next-steps"></a>Nächste Schritte
- Zuordnen einer [öffentlichen IP-Adresse zu einem virtuellen Computer](./associate-public-ip-address-vm.md#azure-portal)
- Lesen Sie mehr über [öffentliche IP-Adressen](./public-ip-addresses.md#public-ip-addresses) in Azure.
- Erfahren Sie mehr über alle [Einstellungen für öffentliche IP-Adressen](virtual-network-public-ip-address.md#create-a-public-ip-address).