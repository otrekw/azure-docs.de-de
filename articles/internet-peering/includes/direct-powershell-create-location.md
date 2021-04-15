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
ms.openlocfilehash: dbaa0b5fc87cb5393b323b8a9b7a38b72efe9518
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "81680751"
---
Das PowerShell-Cmdlet **Get-AzPeeringLocation** gibt mit dem obligatorischen Parameter `Kind` eine Liste der Peeringstandorte zurück, die Sie in späteren Schritten verwenden.

```powershell
Get-AzPeeringLocation -Kind Direct
```

Direct Peering-Standorte enthalten die folgenden Felder:
* PeeringLocation 
* Country
* PeeringDBFacilityId
* PeeringDBFacilityLink
* BandwidthOffers

Überprüfen Sie anhand der [PeeringDB](https://wwww.peeringdb.com), ob Sie in der gewünschten Peeringeinrichtung vorhanden sind.

In diesem Beispiel wird gezeigt, wie Sie Seattle als Peeringstandort zum Erstellen eines Direct-Peerings verwenden.

```powershell
$peeringLocations = Get-AzPeeringLocation -Kind Direct
$peeringLocation = $peeringLocations | where {$_.PeeringLocation -contains "Seattle"}
$peeringLocation

PeeringLocation       : Seattle
Address               : 2001 Sixth Avenue
Country               : US
PeeringDBFacilityId   : 71
PeeringDBFacilityLink : https://www.peeringdb.com/fac/71
BandwidthOffers       : {10Gbps, 100Gbps}
```