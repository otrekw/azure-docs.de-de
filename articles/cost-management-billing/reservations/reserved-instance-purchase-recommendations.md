---
title: Erstellen von Azure-Reservierungsempfehlungen
description: Erfahren Sie, wie Azure-Reservierungsempfehlungen für virtuelle Computer erstellt werden.
author: banders
ms.author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 90967e740b87c2f93bd46bfb78684af96f36193a
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82508478"
---
# <a name="how-reservation-recommendations-are-created"></a>Erstellen von Reservierungsempfehlungen

Kaufempfehlungen für reservierte Azure-Instanzen werden über die [Reservierungsempfehlungs-API](/rest/api/consumption/reservationrecommendations) des Azure-Verbrauchs, über [Azure Advisor](../../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) und über die Benutzeroberfläche für den Reservierungserwerb im Azure-Portal bereitgestellt.

Im Anschluss erfahren Sie, wie Empfehlungen berechnet werden:

1. Die Empfehlungsengine wertet die stündliche Nutzung Ihrer Ressourcen im angegebenen Bereich für die letzten 7, 30 und 60 Tage aus.
2. Auf der Grundlage der Nutzungsdaten simuliert die Engine Ihre Kosten mit und ohne Reservierungen.
3. Die Kosten werden für unterschiedliche Mengen simuliert, und die Menge mit der maximalen Einsparung wird empfohlen.
4. Falls Ihre Ressourcen regelmäßig heruntergefahren werden, können von der Simulation keine Einsparungen ermittelt werden, und es wird keine Kaufempfehlung abgegeben.

## <a name="recommendations-in-azure-advisor"></a>Empfehlungen in Azure Advisor

Empfehlungen für den Reservierungserwerb im Zusammenhang mit virtuellen Computern werden in Azure Advisor bereitgestellt. Berücksichtigen Sie dabei Folgendes:

- Von Advisor werden nur Empfehlungen für Einzelabonnementbereiche abgegeben.
- In Advisor stehen Empfehlungen zur Verfügung, die auf der Grundlage der letzten 30 Tage berechnet wurden.
- Wenn Sie eine Reservierung mit gemeinsam genutztem Bereich erwerben, kann es bis zu 30 Tage dauern, bis Advisor-Empfehlungen für den Reservierungserwerb nicht mehr angezeigt werden.

## <a name="other-expected-api-behavior"></a>Sonstiges erwartetes API-Verhalten

- Bei Verwendung eines Rückblickzeitraums von sieben Tagen erhalten Sie möglicherweise keine Empfehlungen, wenn virtuelle Computer länger als einen Tag heruntergefahren waren.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](../manage/understand-vm-reservation-charges.md)
