---
title: 'Erstellen und Festlegen benutzerdefinierter IPsec-Richtlinien für Point-to-Site-Netzwerke: PowerShell'
titleSuffix: Azure VPN Gateway
description: Dieser Artikel unterstützt Sie beim Erstellen und Festlegen von benutzerdefinierten IPSec-Richtlinien für VPN-Gateway-P2S-Konfigurationen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/09/2020
ms.author: cherylmc
ms.openlocfilehash: a77a870304e20c179e22b4a4ffe404315894d8a0
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91743691"
---
# <a name="create-and-set-custom-ipsec-policies-for-point-to-site-preview"></a>Erstellen und Festlegen benutzerdefinierter IPsec-Richtlinien für Point-to-Site-Netzwerke (Vorschau)

Wenn Ihre Umgebung eine benutzerdefinierte IPsec-Richtlinie für die Verschlüsselung erfordert, können Sie ganz einfach ein Richtlinienobjekt mit den erforderlichen Einstellungen konfigurieren. In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Richtlinienobjekt erstellen und es dann mithilfe von PowerShell festlegen.

## <a name="before-you-begin"></a>Voraussetzungen

### <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie, ob die Umgebung die folgenden Voraussetzungen erfüllt:

* Ein funktionsfähiges Point-to-Site-VPN ist bereits konfiguriert. Wenn dies nicht der Fall ist, führen Sie zum Konfigurieren die Schritte im Artikel **Erstellen eines Point-to-Site-VPN** aus, entweder mithilfe von [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md) oder im [Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

### <a name="working-with-azure-powershell"></a>Arbeiten mit Azure PowerShell

[!INCLUDE [PowerShell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="1-set-variables"></a><a name="signin"></a>1. Festlegen von Variablen

Deklarieren Sie die gewünschten Variablen. Verwenden Sie das unten gezeigte Beispiel, und ersetzen Sie die Werte nach Bedarf durch Ihre eigenen. Wenn Sie die PowerShell-/Cloud Shell-Sitzung während der Übung schließen, wiederholen Sie das Kopieren und Einfügen der Werte, um die Variablen erneut zu deklarieren.

  ```azurepowershell-interactive
  $RG = "TestRG"
  $GWName = "VNet1GW"
  ```

## <a name="2-create-policy-object"></a><a name="create"></a>2. Erstellen des Richtlinienobjekts

Erstellen Sie ein benutzerdefiniertes IPsec-Richtlinienobjekt. Sie können die Werte so anpassen, dass sie die erforderlichen Kriterien erfüllen.

```azurepowershell-interactive
$vpnclientipsecpolicy = New-AzVpnClientIpsecPolicy -IpsecEncryption AES256 -IpsecIntegrity SHA256 -SALifeTime 86471 -SADataSize 429496 -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup2 -PfsGroup PFS2
```

## <a name="3-update-gateway-and-set-policy"></a><a name="update"></a>3. Aktualisieren des Gateways und Festlegen der Richtlinie

In diesem Schritt aktualisieren Sie das vorhandene P2S-VPN-Gateway und legen die IPsec-Richtlinie fest.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -name $GWName
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway -VpnClientIpsecPolicy $vpnclientipsecpolicy
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu P2S-Konfigurationen finden Sie unter [Informationen zu Point-to-Site-VPN](point-to-site-about.md).
