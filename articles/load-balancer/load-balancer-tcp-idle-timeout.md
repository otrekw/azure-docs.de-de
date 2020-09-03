---
title: Konfigurieren der TCP-Leerlaufzeitüberschreitung in Azure für den Load Balancer
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
ms.date: 01/09/2020
ms.author: allensu
ms.openlocfilehash: 317f6a73812b0e4284564ca9b5593e09e22edf12
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89048702"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurieren der TCP-Leerlauftimeout-Einstellungen für Azure Load Balancer

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="tcp-idle-timeout"></a>TCP-Leerlauftimeout
Für Azure Load Balancer ist ein Leerlauftimeout von 4 bis 30 Minuten eingestellt. Standardmäßig ist der Wert auf vier Minuten festgelegt. Wenn die Dauer einer Inaktivitätsperiode den Timeoutwert überschreitet, gibt es keine Garantie dafür, dass die TCP- oder HTTP-Sitzung zwischen dem Client und Ihrem Clouddienst aufrechterhalten wird.

Sobald die Verbindung geschlossen wird, wird in der Clientanwendung möglicherweise die folgende Fehlermeldung angezeigt: „Die zugrunde liegende Verbindung wurde geschlossen: Eine Verbindung, deren Aufrechterhaltung erwartet wurde, wurde vom Server geschlossen.“

Eine gängige Methode zur Aufrechterhaltung von Verbindungen ist TCP-Keep-Alive. Dadurch bleibt die Verbindung länger aktiv. Weitere Informationen finden Sie in [diesen .NET-Beispielen](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Bei aktivierter Keep-Alive-Funktion werden während inaktiver Phasen Pakete über die Verbindung gesendet. Keep-Alive-Pakete sorgen dafür, dass der Wert für das Leerlauftimeout nicht erreicht und die Verbindung über einen langen Zeitraum aufrechterhalten wird.

Die Einstellung funktioniert nur für eingehende Verbindungen. Um den Verlust der Verbindung zu vermeiden, konfigurieren Sie ein TCP-Keep-Alive-Intervall, das kürzer ist als das Leerlauftimeout, oder erhöhen Sie den Leerlauftimeout-Wert. Für diese Szenarien wurde Unterstützung für konfigurierbare Leerlauftimeouts hinzugefügt.

TCP-Keep-Alive eignet sich für Szenarien, in denen die Akkulaufzeit keine Einschränkung darstellt. Bei mobilen Anwendungen ist die Verwendung dieser Funktion hingegen nicht empfehlenswert. TCP-Keep-Alive kann bei mobilen Anwendungen zu einer schnelleren Entladung des Geräteakkus führen.

![TCP-Timeout](./media/load-balancer-tcp-idle-timeout/image1.png)

In den folgenden Abschnitten wird das Ändern von Leerlauftimeout-Einstellungen für öffentliche IP-Adressen und Lastenausgleichsressourcen beschrieben.

>[!NOTE]
> Der TCP-Leerlauftimeout wirkt sich nicht auf die Lastenausgleichsregeln des UDP-Protokolls aus.


## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Festlegen des TCP-Timeouts für Ihre öffentliche IP auf Instanzebene auf 15 Minuten

```azurepowershell-interactive
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` ist optional. Wenn dieser Wert nicht festgelegt ist, beträgt das Standardtimeout 4 Minuten. Der zulässige Timeoutbereich beträgt 4 bis 30 Minuten.

## <a name="set-the-tcp-timeout-on-a-load-balanced-rule-to-15-minutes"></a>Festlegen des TCP-Timeouts für eine Regel mit Lastenausgleich auf 15 Minuten

Zum Festlegen des Leerlauftimeouts für einen Lastenausgleich wird „IdleTimeoutInMinutes“ für die Regel mit Lastenausgleich festgelegt. Beispiel:

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```
## <a name="next-steps"></a>Nächste Schritte

[Interner Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Konfigurieren eines Lastenausgleichs für Internetverbindungen](quickstart-load-balancer-standard-public-powershell.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)
