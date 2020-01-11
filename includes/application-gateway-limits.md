---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 0ba21fe3789fba03cd4814d19fb103c3a2559c13
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75662905"
---
| Resource | Standard/maximales Limit | Hinweis |
| --- | --- | --- |
| Azure Application Gateway |1\.000 pro Abonnement | |
| Front-End-IP-Konfigurationen |2 |1 öffentliche und 1 private |
| Front-End-Ports |100<sup>1</sup> | |
| Back-End-Adresspools |100<sup>1</sup> | |
| Back-End-Server pro Pool |1\.200 | |
| HTTP-Listener |100<sup>1</sup> | |
| HTTP-Lastenausgleichsregeln |100<sup>1</sup> | |
| Back-End-HTTP-Einstellungen |100<sup>1</sup> | |
| Instanzen pro Gateway |32 | |
| SSL-Zertifikate |100<sup>1</sup> |1 pro HTTP-Listener |
| Maximale SSL-Zertifikatgröße |V1 SKU – 10 KB<br>V2 SKU - 16 KB| |
| Authentifizierungszertifikate |100 | |
| Vertrauenswürdige Stammzertifikate |100 | |
| Minimales Anforderungstimeout |1 Sekunde | |
| Maximales Anforderungstimeout |24 Stunden | |
| Anzahl der Websites |100<sup>1</sup> |1 pro HTTP-Listener |
| URL-Zuordnungen pro Listener |1 | |
| Maximale Anzahl pfadbasierter Regeln pro URL-Zuordnung|100||
| Umleitungskonfiguration |100<sup>1</sup>| |
| Gleichzeitige WebSocket-Verbindungen |Mittelgroße Gateways 20K<br> Große Gateways 50K| |
| Maximale URL-Länge|32 KB| |
| Maximale Headergröße für HTTP/2 |4 KB| |
| Maximale Dateiuploadgröße (Standard) |2 GB | |
| Maximale Dateiuploadgröße (WAF) |v1 Mittlere WAF-Gateways, 100 MB<br>v1 Große WAF-Gateways, 500 MB<br>v2 WAF, 750 MB| |
| WAF-Textgrößenbeschränkung (ohne Dateien)|128 KB||
| Maximale benutzerdefinierte WAF-Regeln|100||
| Maximale WAF-Ausschlüsse|100||

<sup>1</sup> Bei WAF-fähigen SKUs sollten Sie für optimale Leistung die Anzahl der Ressourcen auf 40 beschränken.
