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
ms.openlocfilehash: 86d768db7a31c634bdaca6c93f633c7bbaf10a65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773661"
---
Das PowerShell-Cmdlet **Get-AzPeeringLocation** gibt mit dem obligatorischen Parameter `Kind` eine Liste der Peeringstandorte zurück, die Sie in späteren Schritten verwenden:

```powershell
Get-AzPeeringLocation -Kind Direct
```

Direct Peering-Standorte enthalten die folgenden Felder:
* PeeringLocation 
* Country
* PeeringDBFacilityId
* PeeringDBFacilityLink
* BandwidthOffers

Überprüfen Sie anhand von [PeeringDB](https://wwww.peeringdb.com), ob Sie in der gewünschten Peeringeinrichtung vorhanden sind.

Unten sehen Sie ein Beispiel, das zeigt, wie Sie Seattle als Peeringstandort zum Erstellen eines Direct Peerings verwenden:

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