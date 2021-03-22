---
title: include file
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 02/17/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2f52e8a89ec9dd78a1951836053cb2c698310bbd
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100645468"
---
Die folgenden Grenzwerte gelten für Azure Event Grid-**Themen** (Systemthemen, benutzerdefinierte Themen und Partnerthemen). 

| Resource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema oder Partnerthema (eingehend) | 5\.000 Ereignisse/Sek. oder 5 MB/Sek. (je nachdem, welcher Wert zuerst erreicht wird) |
| Ereignisgröße | 1 MB  |
| Private Endpunktverbindungen pro Thema  | 64 | 
| IP-Firewallregeln pro Thema | 16 | 

Für Azure Event Grid-**Domänen** gelten die folgenden Grenzwerte: 

| Resource | Begrenzung |
| --- | --- |
| Themen pro Ereignisdomäne | 100.000 |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 500 |
| Domänenbereich-Ereignisabonnements | 50 |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5\.000 Ereignisse/Sek. oder 5 MB/Sek. (je nachdem, welcher Wert zuerst erreicht wird) |
| Ereignisdomänen pro Azure-Abonnement | 100 |
| Private Endpunktverbindungen pro Domäne | 64 | 
| IP-Firewallregeln pro Domäne | 16 | 


