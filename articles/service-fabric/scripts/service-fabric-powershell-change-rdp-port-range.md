---
title: Azure PowerShell-Beispielskript – Ändern des RDP-Portbereichs | Microsoft-Dokumentation
description: Azure PowerShell-Beispielskript – ändert den RDP-Portbereich eines bereitgestellten Clusters
services: service-fabric
tags: azure-service-management
author: athinanthny
ms.author: atsenthi
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 03/19/2018
ms.custom: sfrev
ms.openlocfilehash: 5cc72423d7e901f82cdf7e40f5de72bd54e9cec1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87076163"
---
# <a name="update-the-rdp-port-range-values"></a>Aktualisieren der Werte des RDP-Portbereichs

Dieses Beispielskript ändert die Werte des RDP-Portbereichs auf den Clusterknoten-VMs, nachdem der Cluster bereitgestellt wurde.  Azure PowerShell wird verwendet, damit die zugrunde liegenden VMs nicht heruntergefahren werden.  Das Skript ruft die Ressource `Microsoft.Network/loadBalancers` in der Ressourcengruppe des Clusters ab und aktualisiert die Werte `inboundNatPools.frontendPortRangeStart` und `inboundNatPools.frontendPortRangeEnd`. Passen Sie die Parameter nach Bedarf an.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/).

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Ruft die Ressource `Microsoft.Network/loadBalancers` ab |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Aktualisiert die Ressource `Microsoft.Network/loadBalancers`|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).
