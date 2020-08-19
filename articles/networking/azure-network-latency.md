---
title: Roundtrip-Latenzstatistiken von Azure-Netzwerken | Microsoft-Dokumentation
description: Erfahren Sie mehr über Statistiken zur Roundtrip-Latenz zwischen Azure-Regionen.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 08/05/2020
ms.author: kumud
ms.openlocfilehash: 1898bcf619f1fc9aaf19384968225e35c9589688
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87847436"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Roundtrip-Latenzstatistiken von Azure-Netzwerken

Azure überwacht kontinuierlich die Latenz (Geschwindigkeit) der Kernbereiche des Netzwerks mithilfe interner Überwachungstools und Messungen, die von [ThousandEyes](https://thousandeyes.com) erfasst werden, einem synthetischen Überwachungsdienst eines Drittanbieters.

## <a name="how-are-the-measurements-collected"></a>Wie werden die Messungen erfasst?

Die Latenzmessungen werden von ThousandEyes-Agents erfasst, die weltweit in Azure-Cloudregionen gehostet werden und fortlaufend in Intervallen von 1 Minute untereinander Netzwerktests senden. Die monatlichen Latenzstatistiken werden aus dem Mittelwert der erfassten Stichproben für den Monat abgeleitet.

## <a name="july-2020-round-trip-latency-figures"></a>Zahlen zur Roundtriplatenz im Juli 2020

Die monatlichen durchschnittlichen Roundtripzeiten zwischen Azure-Regionen für die letzten 31 Tage (Ende am 31. Juli 2020) sind unten dargestellt. Die folgenden Messungen werden von [ThousandEyes](https://thousandeyes.com) unterstützt.

[![Latenzstatistiken zu regionsübergreifender Datenübertragung in Azure](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/).
