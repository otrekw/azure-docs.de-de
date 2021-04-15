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
ms.openlocfilehash: 12d169697a35af446392843eb57e6ec3a5508e45
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "81678650"
---
Die folgenden Änderungsvorgänge werden für Exchange-Peeringverbindungen unterstützt:
* Hinzufügen von Exchange-Peeringverbindungen
* Entfernen von Exchange-Peeringverbindungen
* Hinzufügen von IPv4-/IPv6-Sitzungen in aktiven Verbindungen
* Entfernen von IPv4-/IPv6-Sitzungen aus aktiven Verbindungen


### <a name="add-exchange-peering-connections"></a>Hinzufügen von Exchange Peering-Verbindungen

In diesem Beispiel wird beschrieben, wie Sie einem vorhandenen Exchange-Peering Verbindungen hinzufügen.

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

In diesem Beispiel wird beschrieben, wie Sie Verbindungen aus einem vorhandenen Exchange-Peering entfernen.

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

Geben Sie im folgenden Befehl anstelle von „0“ die Indexnummer der Verbindung ein, die Sie entfernen möchten.

```powershell

$connection = $exchangePeering.Connections[0]

$exchangePeering.Connections.Remove($connection)

$exchangePeering | Update-AzPeering

```

### <a name="add-an-ipv4-or-ipv6-session-on-active-connections"></a>Hinzufügen von IPv4-/IPv6-Sitzungen in aktiven Verbindungen

In diesem Beispiel wird beschrieben, wie Sie einer vorhandenen Exchange-Verbindung eine IPv6-Sitzung hinzufügen.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$exchangePeering.Connections[0].BgpSession.PeerSessionIPv6Address = "2001:504:12::34"

$exchangePeering | Update-AzPeering

```

### <a name="remove-an-ipv4-or-ipv6-session-on-active-connections"></a>Entfernen von IPv4-/IPv6-Sitzungen aus aktiven Verbindungen

Das Entfernen einer IPv4-/IPv6-Sitzung aus einer vorhandenen Verbindung wird mit PowerShell derzeit nicht unterstützt. Wenden Sie sich für weitere Informationen an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).