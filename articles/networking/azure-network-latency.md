---
title: Roundtrip-Latenzstatistiken von Azure-Netzwerken | Microsoft-Dokumentation
description: Erfahren Sie mehr über Statistiken zur Roundtrip-Latenz zwischen Azure-Regionen.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 01/08/2020
ms.author: mnayak
ms.openlocfilehash: 91b528cc6900a3ec91ff7189f58f941226b8acd5
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75779721"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Roundtrip-Latenzstatistiken von Azure-Netzwerken

Azure überwacht kontinuierlich die Latenz (Geschwindigkeit) der Kernbereiche des Netzwerks mithilfe interner Überwachungstools und Messungen, die von [ThousandEyes](https://thousandeyes.com) erfasst werden, einem synthetischen Überwachungsdienst eines Drittanbieters.

## <a name="how-are-the-measurements-collected"></a>Wie werden die Messungen erfasst?

Die Latenzmessungen werden von ThousandEyes-Agents erfasst, die weltweit in Azure-Cloudregionen gehostet werden und fortlaufend in Intervallen von 1 Minute untereinander Netzwerktests senden. Die monatlichen Latenzstatistiken werden aus dem Mittelwert der erfassten Stichproben für den Monat abgeleitet.

## <a name="december-2019-latency-figures"></a>Latenzzahlen vom Dezember 2019

Die monatlichen durchschnittlichen Roundtripzeiten zwischen Azure-Regionen für die letzten 30 Tage (Ende am 31. November 2019) sind unten dargestellt. Die folgenden Messungen werden von [ThousandEyes](https://thousandeyes.com) unterstützt.

[![Latenzstatistiken zu regionsübergreifender Datenübertragung in Azure](media/azure-network-latency/december.jpg)](media/azure-network-latency/december.jpg#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/).
