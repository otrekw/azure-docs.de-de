---
title: Erstellen eines Bastionhosts über Azure Powershell | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Bastion-Host erstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 43d834f0c834696cd4a836466c9663fe7c31a392
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520496"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Erstellen eines Azure Bastion-Hosts über Azure PowerShell

In diesem Artikel wird gezeigt, wie Sie mit PowerShell einen Azure Bastion-Host erstellen. Wenn Sie den Azure Bastion-Dienst in Ihrem virtuellen Netzwerk bereitgestellt haben, steht die nahtlose RDP-/SSH-Funktion für alle virtuellen Computer im selben virtuellen Netzwerk zur Verfügung. Diese Bereitstellung von Azure Bastion erfolgt pro virtuellem Netzwerk und nicht pro Abonnement/Konto oder virtuellem Computer.

Optional können Sie einen Azure Bastion-Host auch im [Azure-Portal](bastion-create-host-portal.md) erstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial) registrieren.

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Erstellen eines Bastion-Hosts

In diesem Abschnitt erfahren Sie, wie Sie eine neue Azure Bastion-Ressource über Azure PowerShell erstellen.

1. Erstellen Sie ein virtuelles Netzwerk und ein Azure Bastion-Subnetz. Sie müssen ein Azure Bastion-Subnetz mit dem Namen **AzureBastionSubnet** erstellen. Dadurch weiß Azure, in welchem Subnetz die Bastion-Ressourcen bereitgestellt werden sollen. Dieses Subnetz unterscheidet sich von einem Gatewaysubnetz. Verwenden Sie mindestens ein Subnetz mit „/27“ oder ein größeres Subnetz („/27“, „/26“ usw.). Erstellen Sie **AzureBastionSubnet** ohne Routentabellen oder Delegierungen. Weitere Informationen zum Verwenden von Netzwerksicherheitsgruppen in **AzureBastionSubnet** finden Sie im Artikel [Arbeiten mit Netzwerksicherheitsgruppen](bastion-nsg.md).

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myBastionRG" -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Erstellen Sie eine öffentliche IP-Adresse für Azure Bastion. Die öffentliche IP-Adresse ist die öffentliche IP-Adresse der Bastion-Ressource für den RDP-/SSH-Zugriff (über den Port 443). Die öffentliche IP-Adresse muss sich in der gleichen Region befinden wie die Bastion-Ressource, die Sie erstellen.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. Erstellen Sie eine neue Azure Bastion-Ressource im AzureBastionSubnet des virtuellen Netzwerks. Die Erstellung und Bereitstellung der Bastion-Ressource dauert etwa fünf Minuten.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName "myBastionRG" -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Bastion](bastion-faq.md).

* Informationen zur Verwendung von Netzwerksicherheitsgruppen mit dem Azure Bastion-Subnetz finden Sie unter [Arbeiten mit Netzwerksicherheitsgruppen](bastion-nsg.md).
