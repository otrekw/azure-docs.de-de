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
ms.openlocfilehash: e084256d9c2043d4382ca180ef3178175b301367
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91745428"
---
Die folgenden Grenzwerte gelten für Azure Event Grid-**Themen** (Systemthemen, benutzerdefinierte Themen und Partnerthemen). 

| Resource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema oder Partnerthema (eingehend) | 5\.000 Ereignisse/Sek. oder 1 MB/Sek. (je nachdem, welcher Wert zuerst erreicht wird)<br/>Gilt nicht für Systemthemen. |
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


