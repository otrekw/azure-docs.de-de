---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 2/14/2020
ms.author: rohink
ms.openlocfilehash: b674f8e31eb61328f60bb24866f73d02653b655f
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77495354"
---
**Öffentliche DNS-Zonen**

| Resource | Standardlimit |
| --- | --- |
| Öffentliche DNS-Zonen pro Abonnement |250 <sup>1</sup> |
| Datensatzgruppen pro öffentlicher DNS-Zone |10.000 <sup>1</sup> |
| Datensätze pro Datensatzgruppe in öffentlicher DNS-Zone |20 |
| Anzahl von Aliasdatensätzen für eine einzelne Azure-Ressource |20|
| Private DNS-Zonen pro Abonnement |1000|
| Datensatzgruppen pro privater DNS-Zone |25000|
| Datensätze pro Datensatzgruppe für private DNS-Zonen |20|
| Virtuelle Netzwerkverbindungen pro privater DNS-Zone |1000|
| Virtuelle Netzwerkverbindungen pro privater DNS-Zone mit aktivierter automatischer Registrierung |100|
| Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk mit aktivierter automatischer Registrierung verknüpft werden kann |1|
| Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk verknüpft werden kann |1000|
| Anzahl von DNS-Abfragen, die ein virtueller Computer pro Sekunde an den Azure DNS-Resolver senden kann |500 <sup>2</sup> |
| Maximale Anzahl von DNS-Abfragen in der Warteschlange (ausstehende Antwort) pro virtuellem Computer |200 <sup>2</sup> |

<sup>1</sup> Wenden Sie sich an den Azure-Support, falls Sie diese Grenzwerte erhöhen müssen.

<sup>2</sup> Diese Grenzwerte gelten nicht auf der Ebene des virtuellen Netzwerks, sondern für jeden einzelnen virtuellen Computer. DNS-Abfragen, die über diese Grenzwerte hinausgehen, werden verworfen.