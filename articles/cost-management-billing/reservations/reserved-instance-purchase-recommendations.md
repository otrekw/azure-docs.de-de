---
title: Azure-Reservierungsempfehlungen
description: Hier erfahren Sie mehr über Azure-Reservierungsempfehlungen.
author: banders
ms.author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: 813cf91195a43a2fc68236febe114c67eb5a3378
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684608"
---
# <a name="reservation-recommendations"></a>Reservierungsempfehlungen

Kaufempfehlungen für reservierte Azure-Instanzen werden über die [Reservierungsempfehlungs-API](/rest/api/consumption/reservationrecommendations) des Azure-Verbrauchs, über [Azure Advisor](../../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) und über die Benutzeroberfläche für den Reservierungserwerb im Azure-Portal bereitgestellt.

Im Anschluss erfahren Sie, wie Empfehlungen berechnet werden:

1. Die Empfehlungsengine wertet die stündliche Nutzung Ihrer Ressourcen im angegebenen Bereich für die letzten 7, 30 und 60 Tage aus.
2. Auf der Grundlage der Nutzungsdaten simuliert die Engine Ihre Kosten mit und ohne Reservierungen.
3. Die Kosten werden für unterschiedliche Mengen simuliert, und die Menge mit der maximalen Einsparung wird empfohlen.
4. Falls Ihre Ressourcen regelmäßig heruntergefahren werden, können von der Simulation keine Einsparungen ermittelt werden, und es wird keine Kaufempfehlung abgegeben.

## <a name="recommendations-in-the-azure-portal"></a>Empfehlungen im Azure-Portal

Empfehlungen für den Reservierungserwerb werden auch im Azure-Portal in der Benutzeroberfläche für den Reservierungserwerb gezeigt. Empfehlungen werden mit der **empfohlenen Menge** gezeigt. Beim Erwerb bietet die von Azure empfohlene Menge die maximal möglichen Einsparungen. Obwohl Sie jede beliebige Menge kaufen können, sind Ihre Einsparungen nicht optimal, wenn Sie eine andere Menge kaufen.

Sehen wir uns nun einige Beispiele für die Gründe an.

Im folgenden Beispielbild für die ausgewählte Empfehlung empfiehlt Azure die Kaufmenge 6.

:::image type="content" source="./media/reserved-instance-purchase-recommendations/recommended-quantity.png" alt-text="Beispiel einer Empfehlung für den Reservierungserwerb" lightbox="./media/reserved-instance-purchase-recommendations/recommended-quantity.png" :::

Weitere Informationen zu der Empfehlung werden angezeigt, wenn Sie den Link **Details anzeigen** auswählen. Das folgende Bild zeigt Details zur Empfehlung. Die empfohlene Menge wird – basierend auf Ihrer verlaufsbezogenen Nutzung – für die höchstmögliche Nutzung berechnet. Ihre Empfehlung ist möglicherweise nicht für 100%ige Auslastung vorgesehen, wenn sie nicht durchgehend genutzt wird. Beachten Sie im Beispiel, dass diese Auslastung im Laufe der Zeit geschwankt hat. Gezeigt werden die Kosten der Reservierung, mögliche Einsparungen und die prozentuale Auslastung.

:::image type="content" source="./media/reserved-instance-purchase-recommendations/recommended-quantity-details.png" alt-text="Beispiel mit Details zur Empfehlung für den Reservierungserwerb" :::

Wenn Sie die Reservierungsmenge über die Empfehlung hinaus erhöhen oder verringern, ändern sich das Diagramm und die geschätzten Werte. Durch Erhöhung der Reservierungsmenge werden Ihre Einsparungen verringert, weil dies letztlich zu einer geringeren Reservierungsnutzung führt. Mit anderen Worten: Sie bezahlen für Reservierungen, die nicht vollständig genutzt werden.

Wenn Sie die Reservierungsmenge verringern, werden Ihre Einsparungen ebenfalls verringert. Obwohl Sie die Auslastung erhöht haben, gibt es wahrscheinlich Zeiträume, in denen Ihre Reservierungen ihre Nutzung nicht vollständig abdecken werden. Die über die Reservierungsmenge hinausgehende Nutzung wird von teureren Ressourcen mit nutzungsbasierter Bezahlung verwendet. Das folgende Beispielbild veranschaulicht dies. Wir haben die Reservierungsmenge manuell auf 4 verringert. Die Reservierungsnutzung wird erhöht, die Gesamtersparnis aber verringert, weil Kosten für die nutzungsbasierte Bezahlung anfallen.

:::image type="content" source="./media/reserved-instance-purchase-recommendations/recommended-quantity-details-changed.png" alt-text="Beispiel mit geänderten Details zur Empfehlung für den Reservierungserwerb" :::

Wenn Sie Einsparungen durch Reservierungen maximieren möchten, versuchen Sie, Reservierungen zu erwerben, die der Empfehlung so weit wie möglich entsprechen.

## <a name="recommendations-in-azure-advisor"></a>Empfehlungen in Azure Advisor

Empfehlungen für den Reservierungserwerb stehen in Azure Advisor zur Verfügung. Berücksichtigen Sie dabei Folgendes:

- Von Advisor werden nur Empfehlungen für Einzelabonnementbereiche abgegeben.
- Empfehlungen werden unter Berücksichtigung des Trends der Nutzung über die letzten 30  Tage berechnet.
- Die Menge und Einsparung von Empfehlungen ist – soweit verfügbar – eine 3-jährige Reservierung. Wenn für den Dienst keine 3-jährige Reservierung verkauft wird, wird die Empfehlung mit einem Reservierungspreis für 1 Jahr berechnet.
- Empfehlungen berücksichtigen alle speziellen Rabatte, die es bei Ihren bedarfsgesteuerten Nutzungsraten möglicherweise gibt.
- Wenn Sie eine Reservierung mit gemeinsam genutztem Bereich erwerben, kann es bis zu 30 Tage dauern, bis Advisor-Empfehlungen für den Reservierungserwerb nicht mehr angezeigt werden.

## <a name="other-expected-api-behavior"></a>Sonstiges erwartetes API-Verhalten

- Bei Verwendung eines Rückblickzeitraums von sieben Tagen erhalten Sie möglicherweise keine Empfehlungen, wenn virtuelle Computer länger als einen Tag heruntergefahren waren.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](../manage/understand-vm-reservation-charges.md)
