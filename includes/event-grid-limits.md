---
title: include file
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 05/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e7a0424e4454639211c6494aab0700e75269361
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83720879"
---
Die folgenden Grenzwerte gelten für Systemthemen und benutzerdefinierte Themen von Azure Event Grid, *nicht* für Ereignisdomänen.

| Resource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema (eingehend) | 5\.000 Ereignisse pro Sekunde pro Thema |
| Ereignisgröße | 1 MB. Vorgänge werden jedoch in Schritten von 64 KB in Rechnung gestellt. Daher fallen für Ereignisse über 64 KB Betriebsgebühren wie für mehrere Ereignisse an. Beispielsweise würde ein Ereignis mit einer Größe von 130 KB Vorgangsgebühren verursachen, als würde es sich um drei separate Ereignisse handeln.  |
| Themen pro Ereignisdomäne | 100.000 |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 500 |
| Domänenbereich-Ereignisabonnements | 50 |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5\.000 Ereignisse pro Sekunde |
| Ereignisdomänen pro Azure-Abonnement | 100 |
| Private Endpunktverbindungen pro Thema oder Domäne | 64 | 
| IP-Firewallregeln pro Thema oder Domäne | 16 | 