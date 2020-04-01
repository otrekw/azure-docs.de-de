---
title: include file
description: include file
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: 148ec3eccce71ab7a4a6c1391c0fa4753c248bd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334579"
---
| Resource | Begrenzung |
| --- | --- |
| Azure Front Door-Ressourcen pro Abonnement | 100 |
| Front-End-Hosts (einschließlich benutzerdefinierter Domänen) pro Ressource | 500 |
| Routingregeln pro Ressource | 500 |
| Back-End-Pools pro Ressource | 50 |
| Back-Ends pro Back-End-Pool | 100 |
| Abzustimmende Pfadmuster für eine Routingregel | 25 |
| URLs in einem einzigen Cachebereinigungsaufruf | 100 |
| Benutzerdefinierte Web Application Firewall-Regeln pro Richtlinie | 100 |
| Web Application Firewall-Richtlinie pro Abonnement | 100 |
| Web Application Firewall – Übereinstimmungsbedingungen pro benutzerdefinierter Regel | 10 |
| Web Application Firewall – IP-Adressbereiche pro Übereinstimmungsbedingung | 600 |
| Web Application Firewall – Zeichenfolgen-Übereinstimmungswerte pro Übereinstimmungsbedingung | 10 |
| Web Application Firewall – Länge des Zeichenfolgen-Übereinstimmungswerts | 256 |
| Web Application Firewall – Länge des POST-Textparameternamens | 256 |
| Web Application Firewall – Länge des HTTP-Headernamens | 256 |
| Web Application Firewall – Länge des Cookienamens | 256 |
| Web Application Firewall – Größe des überprüften HTTP-Anforderungstexts | 128 KB |
| Web Application Firewall – Länge des benutzerdefinierten Antworttexts | 2 KB |

### <a name="timeout-values"></a>Timeoutwerte
#### <a name="client-to-front-door"></a>Client zu Front Door
* Front Door verfügt über ein Timeout für eine TCP-Verbindung im Leerlauf von 61 Sekunden.

#### <a name="front-door-to-application-back-end"></a>Front Door zum Anwendungs-Back-End
* Wenn es sich bei der Antwort um eine segmentierte Antwort handelt, wird der Status 200 zurückgegeben, falls/wenn der erste Block empfangen wird.
* Nach der Weiterleitung der HTTP-Anforderung an das Back-End wartet Front Door 30 Sekunden lang auf das erste Paket vom Back-End. Anschließend wird ein 503-Fehler an den Client zurückgegeben. Dieser Wert kann über das Feld „sendRecvTimeoutSeconds“ in der API konfiguriert werden.
    * Bei Zwischenspeicherungsszenarien kann dieses Timeout nicht konfiguriert werden. Wenn also eine Anforderung zwischengespeichert wird und dieser Vorgang länger als 30 Sekunden für das erste Paket von Front Door oder vom Back-End dauert, wird ein 504-Fehler an den Client zurückgegeben. 
* Nach dem Empfang des ersten Pakets vom Back-End wartet Front Door 30 Sekunden in einem Leerlauftimeout. Anschließend wird ein 503-Fehler an den Client zurückgegeben. Dieser Timeoutwert ist nicht konfigurierbar.
* Das Timeout für die Front Door-TCP-Sitzung mit dem Back-End beträgt 90 Sekunden.

### <a name="upload-and-download-data-limit"></a>Datenlimit beim Hoch- und Herunterladen

|  | Mit Codierung für segmentierte Übertragung (Chunked Transfer Encoding, CTE) | Ohne HTTP-Segmentierung |
| ---- | ------- | ------- |
| **Download** | Es gibt keine Beschränkung für die Downloadgröße. | Es gibt keine Beschränkung für die Downloadgröße. |
| **Upload** |    Es gibt keine Beschränkung, so lange jeder CTE-Upload kleiner als 2 GB ist. | Die Größe darf 2 GB nicht überschreiten. |

### <a name="other-limits"></a>Andere Limits
* Maximum URL size (Maximale Größe der URL) – 8.192 Byte: Hiermit wird die maximale Länge der Rohdaten-URL angegeben (Schema + Hostname + Port + Pfad + Abfragezeichenfolge der URL)
* Maximum Query String size (Maximale Größe der Abfragezeichenfolge) – 4.096 Byte: Hiermit wird die maximale Länge der Abfragezeichenfolge in Byte angegeben.
* Die maximale Headergröße für HTTP-Antworten von der Integritätstest-URL – 4.096 Bytes: Gibt die maximale Länge aller Antwortheader von Integritätstests an. 
