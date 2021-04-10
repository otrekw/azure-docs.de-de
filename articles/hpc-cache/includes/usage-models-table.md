---
ms.service: hpc-cache
ms.topic: include
ms.date: 03/15/2021
author: ekpgh
ms.author: v-erkel
ms.openlocfilehash: 2804b3bb36a31c9cfbf4cce8db94b7469c8975c7
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103563237"
---
| Nutzungsmodell | Cachemodus | Back-End-Überprüfung | Maximale Zurückschreibverzögerung |
|--|--|--|--|
| Leseintensiv, unregelmäßige Schreibvorgänge | Lesen | Nie | Keine |
| Mehr als 15 % Schreibvorgänge | Lesen/Schreiben | 8 Stunden | 20 Minuten |
| Clients umgehen den Cache | Lesen | 30 Sekunden | Keine |
| Mehr als 15 % Schreibvorgänge, regelmäßige Back-End-Überprüfungen (30 Sekunden) | Lesen/Schreiben | 30 Sekunden | 20 Minuten |
| Mehr als 15 % Schreibvorgänge, regelmäßige Back-End-Überprüfungen (60 Sekunden) | Lesen/Schreiben | 60 Sekunden | 20 Minuten |
| Mehr als 15 % Schreibvorgänge, regelmäßiges Zurückschreiben | Lesen/Schreiben | 30 Sekunden | 30 Sekunden |
| Viele Lesezugriffe, Überprüfung des Sicherungsservers alle 3 Stunden | Lesen | 3 Stunden | Keine |
