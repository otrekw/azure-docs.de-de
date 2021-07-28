---
title: Datei einfügen
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 05/03/2021
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 1d9ec2d05374412c17493b47f3bb71524f8cf93f
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110631485"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Metrikwarnungen (klassisch) |100 aktive Warnungsregeln pro Abonnement. | Wenden Sie sich an den Support. |
| Metrikwarnungen |5\.000 aktive Warnungsregeln pro Abonnement in öffentlichen Azure-Clouds sowie in Azure China 21Vianet- und Azure Government-Clouds. Wird dieser Grenzwert erreicht, versuchen Sie, ob Sie [Warnungen mit mehreren Ressourcen desselben Typs](../articles/azure-monitor/alerts/alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) verwenden können.<br/>5\.000 metrische Zeitreihen pro Warnungsregel. | Wenden Sie sich an den Support. |
| Aktivitätsprotokollwarnungen | 100 aktive Warnungsregeln pro Abonnement (der Wert kann nicht erhöht werden). | Wie Standard. |
| Protokollwarnungen | 1000 aktive Warnungsregeln pro Abonnement. 1000 aktive Warnungsregeln pro Ressource. | Wenden Sie sich an den Support. |
| Länge der Beschreibungen von Warnungsregeln und Aktionsregeln| Protokollsuchwarnungen: 4.096 Zeichen<br/>Alle anderen: 2.048 Zeichen | Wie Standard. |

### <a name="alerts-api"></a>Warnungs-API
In Azure Monitor-Warnungen gelten mehrere Drosselungsgrenzwerte zum Schutz vor einer übermäßige Anzahl von Aufrufen. Dieses Verhalten kann die System-Back-End-Ressourcen überlasten und die Reaktionsfähigkeit des Diensts gefährden. Die folgenden Grenzwerte sind darauf ausgelegt, Kunden vor Unterbrechungen zu schützen und eine konsistente Dienstebene zu gewährleisten. Die Drosselung und die Grenzwerte für Benutzer betreffen nur Szenarien mit übermäßigem Verbrauch und sind bei normaler Verwendung in der Regel nicht relevant.

| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| GET alertsSummary | 50 Aufrufe pro Minute pro Abonnement | Wie Standard. | 
|   GET alerts (ohne Angabe einer Warnungs-ID) | 100 Aufrufe pro Minute pro Abonnement | Wie Standard. | 
|   Alle anderen Aufrufe | 1000 Aufrufe pro Minute pro Abonnement | Wie Standard. | 

