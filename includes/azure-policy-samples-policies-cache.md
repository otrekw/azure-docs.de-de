---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 1d5e916d9f8256c002f6810d1fc7aedebeba6481
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170008"
---
|Name |Beschreibung |Auswirkungen |Version |
|---|---|---|---|
|[Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |Hiermit wird überwacht, ob die Verbindungsherstellung mit Redis Cache ausschließlich über SSL-Verbindungen erfolgt. Durch die Verwendung sicherer Verbindungen wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, wie z.B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking. |Audit, Deny, Disabled |1.0.0 |
