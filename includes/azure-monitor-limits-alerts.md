---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 05/03/2021
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 0a56be93309fd68f73bd542222ff14b8b99f3e5e
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108791987"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Metrikwarnungen (klassisch) |100 aktive Warnungsregeln pro Abonnement. | Wenden Sie sich an den Support. |
| Metrikwarnungen |5\.000 aktive Warnungsregeln pro Abonnement in öffentlichen Azure-Clouds sowie in Azure China 21Vianet- und Azure Government-Clouds. Wird dieser Grenzwert erreicht, versuchen Sie, ob Sie [Warnungen mit mehreren Ressourcen desselben Typs](../articles/azure-monitor/alerts/alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) verwenden können.<br/>5\.000 metrische Zeitreihen pro Warnungsregel. | Wenden Sie sich an den Support. |
| Aktivitätsprotokollwarnungen | 100 aktive Warnungsregeln pro Abonnement (der Wert kann nicht erhöht werden). | Wie Standard. |
| Protokollwarnungen | 512 aktive Warnungsregeln pro Abonnement. 200 aktive Warnungsregeln pro Ressource. | Wenden Sie sich an den Support. |
| Länge der Beschreibungen von Warnungsregeln und Aktionsregeln| Protokollsuchwarnungen: 4.096 Zeichen<br/>Alle anderen: 2.048 Zeichen | Wie Standard. |

### <a name="alerts-api"></a>Warnungs-API
In Azure Monitor-Warnungen gelten mehrere Drosselungsgrenzwerte zum Schutz vor einer übermäßige Anzahl von Aufrufen. Dieses Verhalten kann die System-Back-End-Ressourcen überlasten und die Reaktionsfähigkeit des Diensts gefährden. Die folgenden Grenzwerte sind darauf ausgelegt, Kunden vor Unterbrechungen zu schützen und eine konsistente Dienstebene zu gewährleisten. Die Drosselung und die Grenzwerte für Benutzer betreffen nur Szenarien mit übermäßigem Verbrauch und sind bei normaler Verwendung in der Regel nicht relevant.

| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| GET alertsSummary | 50 Aufrufe pro Minute pro Abonnement | Wie Standard. | 
|   GET alerts (ohne Angabe einer Warnungs-ID) | 100 Aufrufe pro Minute pro Abonnement | Wie Standard. | 
|   Alle anderen Aufrufe | 1000 Aufrufe pro Minute pro Abonnement | Wie Standard. | 

