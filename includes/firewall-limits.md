---
title: include file
description: include file
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 06/22/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: b317c2a43352a750d4700fad56d5f7d741b2cc7d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85806115"
---
| Resource | Begrenzung |
| --- | --- |
| Datendurchsatz |30 GBit/s<sup>1</sup> |
|Regeln|10.000. Alle Regeltypen kombiniert.|
|Maximale Anzahl von DNAT-Regeln|298 für eine einzelne öffentliche IP-Adresse.<br>Alle zusätzlichen öffentlichen IP-Adressen tragen zu den verfügbaren SNAT-Ports bei, verringern jedoch die Anzahl der verfügbaren DNAT-Regeln. Beispielsweise lassen zwei öffentliche IP-Adressen 297 DNAT-Regeln zu. Wenn das Protokoll einer Regel sowohl für TCP als auch für UDP konfiguriert ist, zählt dies als zwei Regeln.|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|1 bis 65535|
|Öffentliche IP-Adressen|Höchstens 250. Alle öffentlichen IP-Adressen können in DNAT-Regeln verwendet werden und zählen zu den verfügbaren SNAT-Ports.|
|IP-Adressen in IP-Gruppen|Maximal 100 IP-Gruppen pro Firewall.<br>Maximal 5000 einzelne IP-Adressen oder IP-Präfixe pro IP-Gruppe.<br><br>Weitere Informationen finden Sie unter [IP-Adressgruppen in Azure Firewall](../articles/firewall/ip-groups.md#ip-address-limits).
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten. Standardmäßig unterstützt Azure Firewall keine Tunnelerzwingung für ein lokales Netzwerk.<br><br>Wenn Ihre Konfiguration jedoch die Tunnelerzwingung für ein lokales Netzwerk erfordert, wird Microsoft dies im Einzelfall unterstützen. Wenden Sie sich in diesem Fall an den Support, damit Ihr Fall überprüft werden kann. Bei einer Annahme wird Ihr Abonnement zugelassen, damit die erforderliche Internetkonnektivität der Firewall auch sicher erhalten bleibt.|

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.
