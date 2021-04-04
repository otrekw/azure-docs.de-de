---
title: 'Zurücksetzen einer fehlerhaften Verbindung: ExpressRoute: Mit PowerShell: Azure | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie eine ExpressRoute-Verbindung zurücksetzen, die sich in einem fehlerhaften Zustand befindet.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 01/07/2021
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 84c8275b7a7257530a735e8612047ef42e2f8a7c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98011338"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Zurücksetzen einer fehlerhaften ExpressRoute-Verbindung

Wenn ein Vorgang über eine ExpressRoute-Verbindung nicht erfolgreich abgeschlossen wird, wird die Verbindung möglicherweise in einen Fehlerzustand versetzt. Die Informationen in diesem Artikel helfen Ihnen dabei, eine fehlerhafte Azure ExpressRoute-Verbindung zurückzusetzen.

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

## <a name="reset-a-circuit"></a>Zurücksetzen einer Verbindung

1. Installieren Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps).

2. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

   ```azurepowershell-interactive
   Connect-AzAccount
   ```
3. Überprüfen Sie die Abonnements für das Konto, wenn Sie über mehrere Azure-Abonnements verfügen.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
4. Geben Sie das Abonnement an, das Sie verwenden möchten.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```
5. Führen Sie die folgenden Befehle aus, um eine Verbindung zurückzusetzen, die sich in einem Fehlerzustand befindet:

   ```azurepowershell-interactive
   $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

Die Verbindung sollte jetzt fehlerfrei sein. Öffnen Sie ein Supportticket beim [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls die Verbindung weiterhin fehlerhaft ist.

## <a name="next-steps"></a>Nächste Schritte

Öffnen eines Supporttickets beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), wenn weiterhin Probleme auftreten
