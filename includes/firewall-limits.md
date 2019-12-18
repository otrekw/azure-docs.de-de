---
title: include file
description: include file
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 12/06/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: a7b6867033e750f476b3d995926f0b670965a5d7
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74875508"
---
| Resource | Standardlimit |
| --- | --- |
| Datendurchsatz |30 GBit/s<sup>1</sup> |
|Regeln|10.000. Alle Regeltypen kombiniert.|
|DNAT-Regeln pro öffentlicher IP-Adresse|299|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|0-64.000. Wir arbeiten daran, diese Einschränkung zu lockern.|
|Öffentliche IP-Adressen|Max. 100 (Derzeit werden SNAT-Ports nur für die ersten fünf öffentlichen IP-Adressen hinzugefügt.)|
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten. Standardmäßig unterstützt Azure Firewall keine Tunnelerzwingung für ein lokales Netzwerk.<br><br>Wenn Ihre Konfiguration jedoch die Tunnelerzwingung für ein lokales Netzwerk erfordert, wird Microsoft dies im Einzelfall unterstützen. Wenden Sie sich in diesem Fall an den Support, damit Ihr Fall überprüft werden kann. Bei einer Annahme wird Ihr Abonnement zugelassen, damit die erforderliche Internetkonnektivität der Firewall auch sicher erhalten bleibt.|

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.
