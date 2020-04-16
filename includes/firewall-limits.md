---
title: include file
description: include file
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 04/07/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: fb95301d4faa958a677c42bc8092ac52a7c95c8b
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "80813716"
---
| Resource | Begrenzung |
| --- | --- |
| Datendurchsatz |30 GBit/s<sup>1</sup> |
|Regeln|10.000. Alle Regeltypen kombiniert.|
|Maximale Anzahl von DNAT-Regeln|298<br>Wenn das Protokoll einer Regel sowohl für TCP als auch für UDP konfiguriert ist, zählt dies als zwei Regeln.|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|1 bis 65535|
|Öffentliche IP-Adressen|Max. 100 (Derzeit werden SNAT-Ports nur für die ersten fünf öffentlichen IP-Adressen hinzugefügt.)|
|IP-Adressen von IP-Gruppen|Maximal 50 IP-Gruppen: höchstens 5.000 individuelle IP-Adressen pro Firewallinstanz<br>51 bis 100 IP-Gruppen: 500 individuelle IP-Adressen pro Firewall-Instanz<br><br>Weitere Informationen finden Sie unter [IP-Adressgruppen (Vorschau) in Azure Firewall](../articles/firewall/ip-groups.md#ip-address-limits).
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten. Standardmäßig unterstützt Azure Firewall keine Tunnelerzwingung für ein lokales Netzwerk.<br><br>Wenn Ihre Konfiguration jedoch die Tunnelerzwingung für ein lokales Netzwerk erfordert, wird Microsoft dies im Einzelfall unterstützen. Wenden Sie sich in diesem Fall an den Support, damit Ihr Fall überprüft werden kann. Bei einer Annahme wird Ihr Abonnement zugelassen, damit die erforderliche Internetkonnektivität der Firewall auch sicher erhalten bleibt.|

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.
