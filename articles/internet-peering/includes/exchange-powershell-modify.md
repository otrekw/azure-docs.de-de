---
title: include file
titleSuffix: Azure
description: include file
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 86e1a9cce1864ce259fe07b6949be2e32be242a8
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773701"
---
Die folgenden Änderungsvorgänge werden für Exchange Peering unterstützt
1. Hinzufügen von Exchange Peering-Verbindungen
1. Entfernen von Exchange Peering-Verbindungen
1. Hinzufügen einer IPv4/IPv6-Sitzung in aktiven Verbindungen
1. Entfernen einer IPv4/IPv6-Sitzung aus aktiven Verbindungen


### <a name="add-exchange-peering-connections"></a>Hinzufügen von Exchange Peering-Verbindungen

Das folgende Beispiel beschreibt, wie Sie einem vorhandenen Exchange Peering Verbindungen hinzufügen.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$connection = New-AzPeeringExchangeConnectionObject `
    -PeeringDBFacilityId $exchangeLocation[1].PeeringDBFacilityId `
    -PeerSessionIPv4Address 198.32.134.22 `
    -PeerSessionIPv6Address  2001:504:12::22 `
    -MaxPrefixesAdvertisedIPv4 2000 `
    -MaxPrefixesAdvertisedIPv6 2000 `

$exchangePeering.Connections.Add($connection)

$exchangePeering | Update-AzPeering

```

### <a name="remove-exchange-peering-connections"></a>Entfernen von Exchange Peering-Verbindungen

Das folgende Beispiel beschreibt, wie Sie Verbindungen aus einem vorhandenen Exchange Peering entfernen.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

```

Zeigen Sie alle Verbindungen an, und wählen Sie die Verbindung aus, die Sie entfernen möchten. 

```powershell

$exchangePeering

Name              : SeattleExchangePeering
Sku.Name          : Basic_Exchange_Free
Kind              : Exchange
Connections       : {11}
PeerAsn.Id        : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/{peerAsnName}
PeeringLocation   : Seattle
ProvisioningState : Succeeded
Location          : West US 2
Id                : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleExchangePeering
Type              : Microsoft.Peering/peerings
Tags              : {}

```

Geben Sie im Befehl unten anstelle von „0“ die Indexnummer der Verbindung ein, die Sie entfernen möchten.

```powershell

$connection = $exchangePeering.Connections[0]

$exchangePeering.Connections.Remove($connection)

$exchangePeering | Update-AzPeering

```

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Hinzufügen einer IPv4/IPv6-Sitzung zu den aktiven Verbindungen

Das Beispiel unten beschreibt, wie Sie einer vorhandenen Exchange Peering-Verbindung eine IPv6-Sitzung hinzufügen.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$exchangePeering.Connections[0].BgpSession.PeerSessionIPv6Address = "2001:504:12::34"

$exchangePeering | Update-AzPeering

```

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Entfernen einer IPv4/IPv6-Sitzung aus aktiven Verbindungen

Das Entfernen einer IPv4/IPv6-Sitzung aus einer vorhandenen Verbindung wird derzeit in PowerShell nicht unterstützt. Wenden Sie sich an das [Microsoft Peering-Team](mailto:peeringexperience@microsoft.com).