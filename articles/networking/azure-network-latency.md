---
title: Roundtrip-Latenzstatistiken von Azure-Netzwerken | Microsoft-Dokumentation
description: Erfahren Sie mehr über Statistiken zur Roundtrip-Latenz zwischen Azure-Regionen.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 12/07/2020
ms.author: kumud
ms.openlocfilehash: bc2d7bb7ba17a4a47fecf2144157f79f5367fca7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98059167"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Roundtrip-Latenzstatistiken von Azure-Netzwerken

Azure überwacht kontinuierlich die Latenz (Geschwindigkeit) der Kernbereiche des Netzwerks mithilfe interner Überwachungstools und Messungen, die von [ThousandEyes](https://thousandeyes.com) erfasst werden, einem synthetischen Überwachungsdienst eines Drittanbieters.

## <a name="how-are-the-measurements-collected"></a>Wie werden die Messungen erfasst?

Die Latenzmessungen werden von ThousandEyes-Agents erfasst, die weltweit in Azure-Cloudregionen gehostet werden und fortlaufend in Intervallen von 1 Minute untereinander Netzwerktests senden. Die monatlichen Latenzstatistiken werden aus dem Mittelwert der erfassten Stichproben für den Monat abgeleitet.

## <a name="december-2020-round-trip-latency-figures"></a>Zahlen zur Roundtriplatenz im Dezember 2020

Im Folgenden sind die monatlichen durchschnittlichen Roundtripzeiten zwischen Azure-Regionen für die letzten 30 Tage (Ende am 31. Dezember 2020) dargestellt. Die folgenden Messungen werden von [ThousandEyes](https://thousandeyes.com) unterstützt.

[![Latenzstatistiken zu regionsübergreifender Datenübertragung in Azure](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/).
