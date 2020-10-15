---
title: include file
description: include file
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 07/30/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 59699d493880034ad1d26a56c63a9ed8401ef371
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87507377"
---
| Resource | Begrenzung |
| --- | --- |
| Datendurchsatz |30 GBit/s<sup>1</sup> |
|Regeln|10.000. Alle Regeltypen kombiniert.|
|Maximale Anzahl von DNAT-Regeln|298 für eine einzelne öffentliche IP-Adresse.<br>Alle zusätzlichen öffentlichen IP-Adressen tragen zu den verfügbaren SNAT-Ports bei, verringern jedoch die Anzahl der verfügbaren DNAT-Regeln. Beispielsweise lassen zwei öffentliche IP-Adressen 297 DNAT-Regeln zu. Wenn das Protokoll einer Regel sowohl für TCP als auch für UDP konfiguriert ist, zählt dies als zwei Regeln.|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|1 bis 65535|
|Öffentliche IP-Adressen|Höchstens 250. Alle öffentlichen IP-Adressen können in DNAT-Regeln verwendet werden und zählen zu den verfügbaren SNAT-Ports.|
|IP-Adressen in IP-Gruppen|Maximal 100 IP-Gruppen pro Firewall.<br>Maximal 5000 einzelne IP-Adressen oder IP-Präfixe pro IP-Gruppe.
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten. Standardmäßig unterstützt Azure Firewall keine Tunnelerzwingung für ein lokales Netzwerk.<br><br>Wenn Ihre Konfiguration jedoch die Tunnelerzwingung für ein lokales Netzwerk erfordert, wird Microsoft dies im Einzelfall unterstützen. Wenden Sie sich in diesem Fall an den Support, damit Ihr Fall überprüft werden kann. Bei einer Annahme wird Ihr Abonnement zugelassen, damit die erforderliche Internetkonnektivität der Firewall auch sicher erhalten bleibt.|

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.
