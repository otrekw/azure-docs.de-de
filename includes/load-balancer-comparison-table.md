---
title: include file
description: include file
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/21/2020
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 33e6fecafa2b9748a27794800365974ebc975d0e
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78202523"
---
| | Load Balancer Standard | Load Balancer Basic |
| --- | --- | --- |
| [Größe des Back-End-Pools](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | Unterstützt bis zu 1.000 Instanzen | Unterstützt bis zu 300 Instanzen |
| Back-End-Pool-Endpunkte | Virtuelle Computer oder VM-Skalierungsgruppen in einem einzelnen Netzwerk | Virtuelle Computer in einer einzelnen Verfügbarkeitsgruppe oder VM-Skalierungsgruppe |
| [Integritätstests](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Verhalten bei Ausfall während Integritätstest](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP-Verbindungen bleiben bei Ausfall eines Instanztests __und__ bei Ausfall aller Tests bestehen. | TCP-Verbindungen bleiben bei Ausfall eines Instanztests bestehen. Bei Ausfall aller Tests werden alle TCP-Verbindungen getrennt. |
| Verfügbarkeitszonen | Zonenredundante und zonale Front-Ends für eingehenden und ausgehenden Datenverkehr | Nicht verfügbar |
| Diagnose | [Mehrdimensionale Azure Monitor-Metriken](../articles/load-balancer/load-balancer-standard-diagnostics.md) | [Azure Monitor-Protokolle](../articles/load-balancer/load-balancer-monitor-log.md) |
| HA-Ports | [Verfügbar für den internen Lastenausgleich](../articles/load-balancer/load-balancer-ha-ports-overview.md) | Nicht verfügbar |
| Standardmäßig sicher | Für eingehende Datenflüsse geschlossen, sofern nicht durch eine Netzwerksicherheitsgruppe zugelassen. Hinweis: Interner Datenverkehr vom VNET zum internen Lastenausgleich ist zulässig. | Standardmäßig geöffnet. Netzwerksicherheitsgruppe optional. |
| Regeln für ausgehenden Datenverkehr | [Deklarative ausgehende NAT-Konfiguration](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Nicht verfügbar |
| TCP-Zurücksetzung bei Leerlauf | [Verfügbar für jede Regel](../articles/load-balancer/load-balancer-tcp-reset.md) | Nicht verfügbar |
| [Mehrere Front-Ends](../articles/load-balancer/load-balancer-multivip-overview.md) | Eingehend und [ausgehend](../articles/load-balancer/load-balancer-outbound-connections.md) | Nur eingehend |
| Verwaltungsvorgänge | Die meisten Vorgänge < 30 Sekunden | Meist 60 bis 90+ Sekunden |
| SLA | [99,99%](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | Nicht verfügbar | 
