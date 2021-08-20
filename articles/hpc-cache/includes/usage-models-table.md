---
ms.service: hpc-cache
ms.topic: include
ms.date: 06/17/2021
author: ekpgh
ms.author: v-erkel
ms.openlocfilehash: 37f2895ed476cc0dbe0bdb3def034a17448d8dca
ms.sourcegitcommit: b5508e1b38758472cecdd876a2118aedf8089fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113587221"
---
| Nutzungsmodell | Cachemodus | Back-End-Überprüfung | Maximale Zurückschreibverzögerung |
|--|--|--|--|
| Leseintensiv, unregelmäßige Schreibvorgänge <!--read_heavy_infreq-->| Lesen | Nie | Keine |
| Mehr als 15 % Schreibvorgänge <!--write_workload_15-->| Lesen/Schreiben | 8 Stunden | 1 Stunde |
| Clients umgehen den Cache <!--write_around-->| Lesen | 30 Sekunden | Keine |
| Mehr als 15 % Schreibvorgänge, regelmäßige Back-End-Überprüfungen (30 Sekunden) <!--write_workload_check_30-->| Lesen/Schreiben | 30 Sekunden | 1 Stunde |
| Mehr als 15 % Schreibvorgänge, regelmäßige Back-End-Überprüfungen (60 Sekunden) <!--write_workload_check_60-->| Lesen/Schreiben | 60 Sekunden | 1 Stunde |
| Mehr als 15 % Schreibvorgänge, regelmäßiges Zurückschreiben <!--write_workload_cloudws-->| Lesen/Schreiben | 30 Sekunden | 30 Sekunden |
| Viele Lesezugriffe, Überprüfung des Sicherungsservers alle 3 Stunden <!--read_heavy_check_180-->| Lesen | 3 Stunden | Keine |
