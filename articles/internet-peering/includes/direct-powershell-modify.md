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
ms.openlocfilehash: 03c67ccf88a8c73fe04f062c6af9520115c185a1
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773653"
---
Dieser Abschnitt beschreibt, wie Sie die folgenden Änderungsvorgänge für direktes Peering ausführen:

* Hinzufügen von direkten Peeringverbindungen
* Entfernen von direkten Peeringverbindungen
* Upgrade oder Downgrade der Bandbreite in aktiven Verbindungen
* Hinzufügen einer IPv4/IPv6-Sitzung in aktiven Verbindungen
* Entfernen einer IPv4/IPv6-Sitzung aus aktiven Verbindungen

### <a name="add-direct-peering-connections"></a>Hinzufügen von direkten Peeringverbindungen

Das folgende Beispiel beschreibt, wie Sie einem vorhandenen direkten Peering Verbindungen hinzufügen.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"

$connection = New-AzPeeringDirectConnection `
    -PeeringDBFacilityId $peeringLocation.PeeringDBFacilityId `
    -SessionPrefixV4 "10.22.31.0/31" `
    -SessionPrefixV6 "fe02::3e:0/127" `
    -MaxPrefixesAdvertisedIPv4 1000 `
    -MaxPrefixesAdvertisedIPv6 100 `
    -BandwidthInMbps 10000

$directPeering.Connections.Add($connection)

$directPeering | Update-AzPeering
```

### <a name="remove-direct-peering-connections"></a>Entfernen von direkten Peeringverbindungen

Das Entfernen einer Verbindung wird derzeit in PowerShell nicht unterstützt. Wenden Sie sich an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).

<!--
```powershell
$directPeering.Connections.Remove($directPeering.Connections[0])

$directPeering | Update-AzPeering
```
-->

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Upgrade oder Downgrade der Bandbreite in aktiven Verbindungen

Das folgende Beispiel beschreibt, wie Sie 10 Gbit/s zu einer vorhandenen Verbindung hinzufügen.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BandwidthInMbps  = 20000
$directPeering | Update-AzPeering

```

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Hinzufügen einer IPv4/IPv6-Sitzung in aktiven Verbindungen

Das folgende Beispiel beschreibt, wie Sie eine IPv6-Sitzung zu einer vorhandenen direkten Verbindung hinzufügt, die nur über eine IPv4-Sitzung verfügt. 

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BGPSession.SessionPrefixv6 = "fe01::3e:0/127"
$directPeering | Update-AzPeering

```

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Entfernen einer IPv4/IPv6-Sitzung aus aktiven Verbindungen

Das Entfernen einer IPv4/IPv6-Sitzung aus einer vorhandenen Verbindung wird derzeit in PowerShell nicht unterstützt. Wenden Sie sich an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).