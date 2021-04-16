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
ms.openlocfilehash: fa8d8ccef7d6ad6e1b5d9f19de61e45ee8c439fa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "82587818"
---
Das PowerShell-Cmdlet **Get-AzPeeringLocation** gibt mit dem obligatorischen Parameter `Kind` eine Liste der Peeringstandorte zurück, die Sie in späteren Schritten verwenden.

```powershell
Get-AzPeeringLocation -Kind "Exchange"
```

Exchange-Peeringstandorte enthalten die folgenden Felder:
* ExchangeName
* PeeringLocation
* Country
* PeeringDBFacilityId
* PeeringDBFacilityLink
* MicrosoftIPv4Address
* MicrosoftIPv6Address

Überprüfen Sie anhand von [PeeringDB](https://www.peeringdb.com), ob Sie in der gewünschten Peeringeinrichtung vorhanden sind.

In diesem Beispiel wird gezeigt, wie Sie Seattle als Peeringstandort zum Erstellen eines Peerings verwenden.

```powershell
$exchangeLocations = Get-AzPeeringLocation -Kind Exchange
$exchangeLocation = $exchangeLocations | where {$_.PeeringLocation -eq "Seattle"}

#check the location metadata
$exchangeLocation

ExchangeName          : Columbia IX
PeeringLocation       : Seattle
Country               : US
PeeringDBFacilityId   : 99999
PeeringDBFacilityLink : https://www.peeringdb.com/ix/99999
MicrosoftIPv4Address  : 10.12.97.129
MicrosoftIPv6Address  :

ExchangeName          : Equinix Seattle
PeeringLocation       : Seattle
Country               : US
PeeringDBFacilityId   : 11
PeeringDBFacilityLink : https://www.peeringdb.com/ix/11
MicrosoftIPv4Address  : 198.32.134.152
MicrosoftIPv6Address  : 2001:504:12::15

...

```
