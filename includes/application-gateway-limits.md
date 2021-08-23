---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 3e2b9ab4eaf794e5cf64861d7c641b82f18b64dc
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "114201217"
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
| Anzahl von Regelsätzen für das erneute Generieren |400| |
| Anzahl von Header- oder URL-Konfigurationen pro Regelsatz für das erneute Generieren|40| |
| Anzahl von Bedingungen pro Regelsatz für das erneute Generieren|40| |
| Gleichzeitige WebSocket-Verbindungen |Mittelgroße Gateways 20.000<sup>2</sup><br> Große Gateways 50.000<sup>2</sup>| |
| Maximale URL-Länge|32 KB| |
| Maximale Headergröße für HTTP/2 |16 KB| |
| Maximale Dateiuploadgröße (Standard-SKU) |V2 – 4 GB<br>V1 – 2 GB | |
| Maximale Dateiuploadgröße (WAF-SKU) |V1 Mittel – 100 MB<br>V1 Groß – 500 MB<br>V2 – 750 MB<br>V2 (mit CRS 3.2 oder neuer) – 4 GB| |
| WAF-Textgrößenbeschränkung (ohne Dateien)|V1 oder V2 (mit CRS 3.1 und älter) – 128 KB<br>V2 (mit CRS 3.2 oder neuer) – 2 MB| |
| Maximale benutzerdefinierte WAF-Regeln|100||
| Maximale WAF-Ausschlüsse pro Application Gateway|40||

<sup>1</sup> Bei WAF-fähigen SKUs muss die Anzahl von Ressourcen auf 40 beschränkt werden.

<sup>2</sup> Der Grenzwert gilt pro Application Gateway-Instanz nicht pro Application Gateway-Ressource.
