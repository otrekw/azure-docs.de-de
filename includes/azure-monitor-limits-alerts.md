---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 10/01/2020
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 071f2849a877f4ea1e8a84eff6ccfb8343be3ec7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101734040"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Metrikwarnungen (klassisch) |100 aktive Warnungsregeln pro Abonnement. | Wenden Sie sich an den Support. |
| Metrikwarnungen |5\.000 aktive Warnungsregeln pro Abonnement in öffentlichen Azure-Clouds sowie in Azure China 21Vianet- und Azure Government-Clouds. Wird dieser Grenzwert erreicht, versuchen Sie, ob Sie [Warnungen mit mehreren Ressourcen desselben Typs](../articles/azure-monitor/alerts/alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) verwenden können.<br/>5\.000 metrische Zeitreihen pro Warnungsregel. | Wenden Sie sich an den Support. |
| Aktivitätsprotokollwarnungen | 100 aktive Warnungsregeln pro Abonnement (der Wert kann nicht erhöht werden). | Wie Standard. |
| Protokollwarnungen | 512 aktive Warnungsregeln pro Abonnement. 200 aktive Warnungsregeln pro Ressource. | Wenden Sie sich an den Support. |
| Länge der Beschreibungen von Warnungsregeln und Aktionsregeln| Protokollsuchwarnungen: 4.096 Zeichen<br/>Alle anderen: 2.048 Zeichen | Wie Standard. |