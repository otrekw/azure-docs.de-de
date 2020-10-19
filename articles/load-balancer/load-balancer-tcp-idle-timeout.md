---
title: Konfigurieren der TCP-Zurücksetzung und des Leerlauftimeouts in Azure für Load Balancer
titleSuffix: Azure Load Balancer
description: In diesem Artikel erfahren Sie, wie die TCP-Leerlaufzeitüberschreitung in Azure Load Balancer konfiguriert wird.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/09/2020
ms.author: allensu
ms.openlocfilehash: 26c4c01aaf6abe6b9c9ac6daf6836d7b660ba21e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91649829"
---
# <a name="configure-tcp-idle-timeout-for-azure-load-balancer"></a>Konfigurieren des TCP-Leerlauftimeouts für Azure Load Balancer

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Für Azure Load Balancer ist ein Leerlauftimeout von 4 bis 120 Minuten festgelegt. Standardmäßig ist der Wert auf vier Minuten festgelegt. Wenn die Dauer einer Inaktivitätsperiode den Timeoutwert überschreitet, gibt es keine Garantie dafür, dass die TCP- oder HTTP-Sitzung zwischen dem Client und Ihrem Clouddienst aufrechterhalten wird. Hier erfahren Sie mehr zum [TCP-Leerlauftimeout](load-balancer-tcp-reset.md).

In den folgenden Abschnitten wird das Ändern von Leerlauftimeout-Einstellungen für öffentliche IP-Adressen und Lastenausgleichsressourcen beschrieben.


## <a name="configure-the-tcp-idle-timeout-for-your-public-ip"></a>Konfigurieren des TCP-Leerlauftimeouts für Ihre öffentliche IP-Adresse

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` ist optional. Wenn dieser Wert nicht festgelegt ist, beträgt das Standardtimeout 4 Minuten. Der zulässige Timeoutbereich beträgt 4 bis 120 Minuten.

## <a name="set-the-tcp-idle-timeout-on-rules"></a>Festlegen des TCP-Leerlauftimeouts für Regeln

Zum Festlegen des Leerlauftimeouts für einen Lastenausgleich wird „IdleTimeoutInMinutes“ für die Regel mit Lastenausgleich festgelegt. Beispiel:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```

## <a name="next-steps"></a>Nächste Schritte

[Interner Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Konfigurieren eines Lastenausgleichs für Internetverbindungen](quickstart-load-balancer-standard-public-powershell.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)
