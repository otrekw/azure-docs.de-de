---
title: include file
description: include file
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 01/22/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 92c2e79910e40721a0ef62d44825bd1f3e19fc79
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548205"
---
| Resource | Standardlimit |
| --- | --- |
| Datendurchsatz |30 GBit/s<sup>1</sup> |
|Regeln|10.000. Alle Regeltypen kombiniert.|
|Maximale Anzahl von DNAT-Regeln|299|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|0-64.000. Wir arbeiten daran, diese Einschränkung zu lockern.|
|Öffentliche IP-Adressen|Max. 100 (Derzeit werden SNAT-Ports nur für die ersten fünf öffentlichen IP-Adressen hinzugefügt.)|
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten. Standardmäßig unterstützt Azure Firewall keine Tunnelerzwingung für ein lokales Netzwerk.<br><br>Wenn Ihre Konfiguration jedoch die Tunnelerzwingung für ein lokales Netzwerk erfordert, wird Microsoft dies im Einzelfall unterstützen. Wenden Sie sich in diesem Fall an den Support, damit Ihr Fall überprüft werden kann. Bei einer Annahme wird Ihr Abonnement zugelassen, damit die erforderliche Internetkonnektivität der Firewall auch sicher erhalten bleibt.|

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.
