---
title: Datei einfügen
titleSuffix: Azure
description: include file
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 65f282a63e95530549bd0db35c2f42fe5c25993d
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110721490"
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
