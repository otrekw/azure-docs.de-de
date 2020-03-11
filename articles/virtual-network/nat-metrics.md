---
title: Metriken und Warnungen für Azure Virtual Network NAT
titleSuffix: Azure Virtual Network
description: Hier finden Sie grundlegende Informationen zu verfügbaren Azure Monitor-Metriken und -Warnungen für Virtual Network NAT.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to understand available Azure Monitor metrics and alerts for Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2020
ms.author: allensu
ms.openlocfilehash: 268f5f5b7bf1bed27a01085974bea72af6a84f3b
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304054"
---
# <a name="azure-virtual-network-nat-metrics"></a>Metriken für Azure Virtual Network NAT

Azure Virtual Network NAT-Gatewayressourcen bieten mehrdimensionale Metriken. Diese Metriken können zur Überwachung des Betriebs sowie zur [Problembehandlung](nat-metrics.md) verwendet werden.  Für kritische Probleme wie etwa die SNAT-Auslastung können Warnungen konfiguriert werden.

<p align="center">
  <img src="media/nat-overview/flow-direction1.svg" width="256" title="Virtual Network NAT für Internetverbindung in ausgehender Richtung">
</p>

*Abbildung: Virtual Network NAT für Internetverbindung in ausgehender Richtung*

>[!NOTE] 
>Virtual Network NAT ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar. Dieser Dienst ist bisher nur in einer begrenzten Zahl von [Regionen](nat-overview.md#region-availability) erhältlich. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="metrics"></a>Metriken

Für NAT-Gatewayressourcen stehen in Azure Monitor die folgenden mehrdimensionalen Metriken zur Verfügung:

| Metrik | Beschreibung | Empfohlene Aggregation | Dimensionen |
|---|---|---|---|
| Byte | Verarbeitete Bytes (ein- und ausgehend) | Summe | Richtung (eingehend, ausgehend), Protokoll (6 TCP, 17 UDP) |
| Pakete | Verarbeitete Pakete (ein- und ausgehend) | Summe | Richtung (eingehend, ausgehend), Protokoll (6 TCP, 17 UDP) |
| Verworfene Pakete | Pakete, die vom NAT-Gateway verworfen wurden | Summe | / |
| Anzahl von SNAT-Verbindungen | Statusübergänge pro Intervall | Summe | Verbindungsstatus, Protokoll (6 TCP, 17 UDP) |
| Gesamtanzahl von SNAT-Verbindungen | Aktuell aktive SNAT-Verbindungen (~ verwendete SNAT-Ports) | Summe | Protokolle (6 TCP, 17 UDP) |


## <a name="alerts"></a>Warnungen

Warnungen für Metriken können in Azure Monitor für jede der obigen [Metriken](#metrics) konfiguriert werden.

## <a name="limitations"></a>Einschränkungen

Resource Health wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Virtual Network NAT](nat-overview.md).
- Informieren Sie sich über [NAT-Gatewayressourcen](nat-gateway-resource.md).
- Informieren Sie sich über [Azure Monitor](../azure-monitor/overview.md).
- Informieren Sie sich über die [Problembehandlung im Zusammenhang mit der Azure Virtual Network NAT-Konnektivität](troubleshoot-nat.md).

