---
title: Metriken und Warnungen für Azure Virtual Network NAT
titleSuffix: Azure Virtual Network
description: Hier finden Sie grundlegende Informationen zu verfügbaren Azure Monitor-Metriken und -Warnungen für Virtual Network NAT.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
ms.subservice: nat
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: b22d7823ac961de53914325d7ebf2b5d53b7c5af
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106055823"
---
# <a name="azure-virtual-network-nat-metrics"></a>Metriken für Azure Virtual Network NAT

Azure Virtual Network NAT-Gatewayressourcen bieten mehrdimensionale Metriken. Diese Metriken können zur Überwachung des Betriebs sowie zur [Problembehandlung](troubleshoot-nat.md) verwendet werden.  Für kritische Probleme wie etwa die SNAT-Auslastung können Warnungen konfiguriert werden.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" alt="Figure depicts a NAT gateway resource that consumes all IP addresses for a public IP prefix and directs that traffic to and from two subnets of virtual machines and a virtual machine scale set." width="256" title="Virtual Network NAT für Internetverbindung in ausgehender Richtung">
</p>

*Abbildung: Virtual Network NAT für Internetverbindung in ausgehender Richtung*

## <a name="metrics"></a>Metriken

Für NAT-Gatewayressourcen stehen in Azure Monitor die folgenden mehrdimensionalen Metriken zur Verfügung:

| Metrik | BESCHREIBUNG | Empfohlene Aggregation | Dimensionen |
|---|---|---|---|
| Byte | Verarbeitete Bytes (ein- und ausgehend) | SUM | Richtung (eingehend, ausgehend), Protokoll (6 TCP, 17 UDP) |
| Pakete | Verarbeitete Pakete (ein- und ausgehend) | SUM | Richtung (eingehend, ausgehend), Protokoll (6 TCP, 17 UDP) |
| Verworfene Pakete | Pakete, die vom NAT-Gateway verworfen wurden | SUM | / |
| Anzahl von SNAT-Verbindungen | Statusübergänge pro Intervall | SUM | Verbindungsstatus, Protokoll (6 TCP, 17 UDP) |
| Gesamtanzahl von SNAT-Verbindungen | Aktuell aktive SNAT-Verbindungen (~ verwendete SNAT-Ports) | SUM | Protokolle (6 TCP, 17 UDP) |


## <a name="alerts"></a>Alerts

Warnungen für Metriken können in Azure Monitor für jede der obigen [Metriken](#metrics) konfiguriert werden.

## <a name="limitations"></a>Einschränkungen

Resource Health wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [Virtual Network NAT](nat-overview.md).
* Informieren Sie sich über die [NAT-Gatewayressource](nat-gateway-resource.md).
* Informieren Sie sich über [Azure Monitor](../azure-monitor/overview.md).
* Informieren Sie sich über die [Problembehandlung im Zusammenhang mit der Azure Virtual Network NAT-Konnektivität](troubleshoot-nat.md).
* [Teilen Sie uns bei UserVoice mit, welche Funktionen wir als Nächstes für Virtual Network NAT entwickeln sollen.](https://aka.ms/natuservoice)


