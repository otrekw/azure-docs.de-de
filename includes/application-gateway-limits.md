---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 92ec7c0a1469c9f02855cd6191faa8514e54c8f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91828968"
---
| Resource | Begrenzung | Hinweis |
| --- | --- | --- |
| Azure Application Gateway |1\.000 pro Abonnement | |
| Front-End-IP-Konfigurationen |2 |1 öffentliche und 1 private |
| Front-End-Ports |100<sup>1</sup> | |
| Back-End-Adresspools |100<sup>1</sup> | |
| Back-End-Server pro Pool |1\.200 | |
| HTTP-Listener |200<sup>1</sup> |Beschränkt auf 100 aktive Listener, die Datenverkehr weiterleiten. Aktive Listener = Gesamtanzahl von Listenern - nicht aktive Listener.<br>Wenn eine Standardkonfiguration innerhalb einer Routingregel zur Weiterleitung von Datenverkehr (z. B. mit einem Listener, einem Back-End-Pool und HTTP-Einstellungen) festgelegt ist, dann zählt dies ebenfalls als Listener.|
| HTTP-Lastenausgleichsregeln |100<sup>1</sup> | |
| Back-End-HTTP-Einstellungen |100<sup>1</sup> | |
| Instanzen pro Gateway |V1 SKU – 32<br>V2 SKU – 125 | |
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
| Maximale Dateiuploadgröße (WAF) |V1 Mittlere WAF-Gateways, 100 MB<br>V1 Große WAF-Gateways, 500 MB<br>V2 WAF, 750 MB| |
| WAF-Textgrößenbeschränkung (ohne Dateien)|128 KB||
| Maximale benutzerdefinierte WAF-Regeln|100||
| Maximale WAF-Ausschlüsse|100||

<sup>1</sup> Bei WAF-fähigen SKUs muss die Anzahl von Ressourcen auf 40 beschränkt werden.
