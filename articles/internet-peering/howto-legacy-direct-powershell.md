---
title: Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource mit PowerShell
titleSuffix: Azure
description: Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource mit PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: ba41f4ad8014ba3e85174b7c32e11394f0068643
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773885"
---
# <a name="convert-a-legacy-direct-peering-to-azure-resource-using-powershell"></a>Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource mit PowerShell

In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell-Cmdlets ein vorhandenes älteres direktes Peering in eine Azure-Ressource konvertieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe des [Portals](howto-legacy-direct-portal.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [exemplarische Vorgehensweise für direktes Peering](walkthrough-direct-all.md).

### <a name="working-with-azure-powershell"></a>Arbeiten mit Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-legacy-direct-peering-to-azure-resource"></a>Konvertieren eines älteren direkten Peerings in eine Azure-Ressource

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-direct-peering-for-conversion"></a><a name= get></a>Abrufen des direkten Peerings für die Konvertierung
Im Folgenden finden Sie ein Beispiel für das Abrufen einer älteren Instanz für direktes Peering an einem Peeringstandort in Seattle.

```powershell
$legacyPeering = Get-AzLegacyPeering `
    -Kind Direct -PeeringLocation "Seattle"
$legacyPeering
```

Unten finden Sie eine Beispielantwort:
```powershell
Name                       :
Sku                        : Basic_Direct_Free
Kind                       : Direct
PeeringLocation            : Seattle
UseForPeeringService       : False
PeerAsn.Id                 :
Connection                 : ------------------------
PeeringDBFacilityId        : 71
SessionPrefixIPv4          : 4.71.156.72/30
PeerSessionIPv4Address     : 4.71.156.73
MicrosoftIPv4Address       : 4.71.156.74
SessionStateV4             : Established
MaxPrefixesAdvertisedV4    : 20000
SessionPrefixIPv6          : 2001:1900:2100::1e10/126
MaxPrefixesAdvertisedV6    : 2000
ConnectionState            : Active
BandwidthInMbps            : 0
ProvisionedBandwidthInMbps : 20000
Connection                 : ------------------------
PeeringDBFacilityId        : 71
SessionPrefixIPv4          : 4.68.70.140/30
PeerSessionIPv4Address     : 4.68.70.141
MicrosoftIPv4Address       : 4.68.70.142
SessionStateV4             : Established
MaxPrefixesAdvertisedV4    : 20000
SessionPrefixIPv6          : 2001:1900:4:3::cc/126
PeerSessionIPv6Address     : 2001:1900:4:3::cd
MicrosoftIPv6Address       : 2001:1900:4:3::ce
SessionStateV6             : Established
MaxPrefixesAdvertisedV6    : 2000
ConnectionState            : Active
BandwidthInMbps            : 0
ProvisionedBandwidthInMbps : 20000
ProvisioningState          : Succeeded
```

### <a name="convert-legacy-direct-peering"></a>Konvertieren einer älteren Instanz für direktes Peering

&nbsp;
> [!IMPORTANT]
> Beachten Sie, dass Änderungen beim Konvertieren einer älteren Peering-Instanz in eine Azure-Ressource nicht unterstützt werden. &nbsp;

Verwenden Sie den folgenden Befehl zum Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleDirectPeering" `
    -ResourceGroupName "PeeringResourceGroup" `

```

Unten finden Sie eine Beispielantwort:

```powershell
Name                 : SeattleDirectPeering
Sku.Name             : Basic_Direct_Free
Kind                 : Direct
Connections          : {11, 11}
PeerAsn.Id           : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/{asnNumber}
UseForPeeringService : False
PeeringLocation      : Seattle
ProvisioningState    : Succeeded
Location             : centralus
Id                   : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleDirectPeering
Type                 : Microsoft.Peering/peerings
Tags                 : {}
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den folgenden Befehl ausführen:

```powershell
Get-Help Get-AzPeering -detailed
```

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für direktes Peering mit PowerShell](howto-direct-powershell.md).
