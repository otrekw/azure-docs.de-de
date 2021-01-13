---
title: include file
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 10/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b884bc72381c98af77f2f49336f3dd5762c68734
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91859825"
---
Die folgenden Grenzwerte gelten für Azure Event Grid-**Themen** (Systemthemen, benutzerdefinierte Themen und Partnerthemen). 

| Resource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema oder Partnerthema (eingehend) | 5\.000 Ereignisse/Sek. oder 1 MB/Sek. (je nachdem, welcher Wert zuerst erreicht wird) |
| Ereignisgröße | 1 MB  |
| Private Endpunktverbindungen pro Thema  | 64 | 
| IP-Firewallregeln pro Thema | 16 | 

Für Azure Event Grid-**Domänen** gelten die folgenden Grenzwerte: 

| Resource | Begrenzung |
| --- | --- |
| Themen pro Ereignisdomäne | 100.000 |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 500 |
| Domänenbereich-Ereignisabonnements | 50 |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5\.000 Ereignisse/Sek. oder 1 MB/Sek. (je nachdem, welcher Wert zuerst erreicht wird) |
| Ereignisdomänen pro Azure-Abonnement | 100 |
| Private Endpunktverbindungen pro Domäne | 64 | 
| IP-Firewallregeln pro Domäne | 16 | 


