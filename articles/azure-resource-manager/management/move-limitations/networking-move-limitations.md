---
title: Verschieben von Azure-Netzwerkressourcen in ein neues Abonnement oder eine neue Ressourcengruppe
description: Verwenden Sie Azure Resource Manager, um virtuelle Netzwerke und andere Netzwerkressourcen in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
ms.topic: conceptual
ms.date: 10/16/2019
ms.openlocfilehash: 0ac503a778b3de8945b074628242233f9bcd6a3b
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108129961"
---
# <a name="move-guidance-for-networking-resources"></a>Leitfaden zum Verschieben von Netzwerkressourcen

In diesem Artikel wird beschrieben, wie Sie virtuelle Netzwerke und andere Netzwerkressourcen für bestimmte Szenarien verschieben.

## <a name="dependent-resources"></a>Abhängige Ressourcen

> [!NOTE]
> Beachten Sie bitte, dass VPN-Gateways, die mit Public IP Standard SKU-Adressen verbunden sind, derzeit nicht zwischen Ressourcengruppen oder Abonnements wechseln können.

Wenn Sie eine Ressource verschieben, müssen Sie auch ihre abhängigen Ressourcen (z. b. öffentliche IP-Adressen, virtuelle Netzwerk Gateways, alle zugeordneten Verbindungsressourcen) verschieben. Gateways des lokalen Netzwerks können sich in einer anderen Ressourcengruppe befinden.

Um einen virtuellen Computer mit einer Netzwerkschnittstellenkarte zu einem neuen Abonnement zu verschieben, müssen Sie alle abhängigen Ressourcen verschieben. Sie verschieben das virtuelle Netzwerk für die Netzwerkschnittstellenkarte, alle anderen Netzwerkschnittstellenkarten für das virtuelle Netzwerk und die VPN-Gateways.

Weitere Informationen finden Sie unter [Szenario für eine abonnementübergreifende Verschiebung](../move-resource-group-and-subscription.md#scenario-for-move-across-subscriptions).

## <a name="peered-virtual-network"></a>Virtuelles Netzwerk mit Peering

Um ein mittels Peering verknüpftes virtuelles Netzwerk zu verschieben, müssen Sie zunächst das Peering des virtuellen Netzwerks deaktivieren. Nach der Deaktivierung können Sie das virtuelle Netzwerk verschieben. Aktivieren Sie nach der Verschiebung das Peering des virtuellen Netzwerks wieder.

## <a name="subnet-links"></a>Subnetzverknüpfungen

Ein virtuelles Netzwerk kann nicht in ein anderes Abonnement verschoben werden, wenn das virtuelle Netzwerk ein Subnetz mit Navigationslinks für Ressourcen enthält. Beispiel: Wenn eine Azure Cache for Redis-Ressource in einem Subnetz bereitgestellt wird, verfügt dieses Subnetz über einen Navigationslink für die Ressource.

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../move-resource-group-and-subscription.md).
