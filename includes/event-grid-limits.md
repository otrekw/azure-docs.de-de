---
title: include file
description: include file
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: ee80c04a6365c2cf337c4033a90df8d2993c299d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76845971"
---
Die folgenden Grenzwerte gelten für Systemthemen und benutzerdefinierte Themen von Azure Event Grid, *nicht* für Ereignisdomänen.

| Resource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema (eingehend) | 5\.000 Ereignisse pro Sekunde pro Thema |
| Veröffentlichungsanforderungen | 250 pro Sekunde |
| Ereignisgröße | 1 MB (in Chargen von mehreren 64-KB-Ereignissen) |

Die folgenden Grenzwerte gelten ausschließlich für Ereignisdomänen.

| Resource | Begrenzung |
| --- | --- |
| Themen pro Ereignisdomäne | 100.000 |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 500 |
| Domänenbereich-Ereignisabonnements | 50 |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5\.000 Ereignisse pro Sekunde |
| Veröffentlichungsanforderungen | 250 pro Sekunde |
| Ereignisdomänen pro Azure-Abonnement | 100 |