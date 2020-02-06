---
title: Erstellen von Azure-Reservierungsempfehlungen
description: Erfahren Sie, wie Azure-Reservierungsempfehlungen für virtuelle Computer erstellt werden.
author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/28/2020
ms.author: banders
ms.openlocfilehash: a43e0c4d86bd47c953b50ab9fb1fd8df00e7df3d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76851350"
---
# <a name="how-reservation-recommendations-are-created"></a>Erstellen von Reservierungsempfehlungen

Kaufempfehlungen für reservierte Azure-Instanzen (RI) werden von der [Reservierungsempfehlungs-API](/rest/api/consumption/reservationrecommendations) in Azure Consumption generiert. Die Empfehlungen der API werden auch vom [Azure Advisor](../..//advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) verwendet. Der Advisor zeigt Empfehlungen im Azure-Portal an.

Wenn Sie virtuelle Computer in Azure ausführen, können Sie die Rabatte für RIs nutzen und ihre VMs im Voraus bezahlen. Die Empfehlungs-API in Microsoft Consumption wertet ihre Nutzung für 7, 30 und 60 Tage aus und empfiehlt optimale Konfigurationen für RIs. Sie berechnet die Kosten, die ohne RIs zu begleichen wären, und vergleicht sie mit den Kosten, die anfallen, wenn Sie zur optimalen Einsparung RIs verwenden.

Der Azure Advisor zeigt Empfehlungen basierend auf einem Zeitraum von 30 Tagen an.

Der Einfachheit halber zeigt das folgende Beispiel Berechnungen für eine 7-Tage-Empfehlung. Dieselbe Methode wird bei der Berechnung von Empfehlungen für 30 oder 60 Tage angewendet.

## <a name="calculation-method-example"></a>Beispiel für die Berechnungsmethode

Angenommen, die stündliche Nutzung von Windows-VMs für eine bestimmte SKU und eine bestimmte Region variiert im Verlauf von sieben Tagen (168 Stunden). Die Mindestauslastung in den sieben Tagen beträgt 65 Einheiten und die maximale Auslastung 127 Einheiten. In diesem Beispiel wurden in der 79. Stunde 80 VMs verwendet, und Sie haben 75 RIs erworben.

Wenn Sie 75 reservierte Instanzen erwerben, fallen für die 79. Stunde die folgenden Kosten an:

- 75 reservierte Instanzen. Die Kosten werden beim Kauf der RIs vorab bezahlt.
- Reservierte Instanzen decken die Hardwarekosten der ausgeführten VMs ab, sodass Sie für 75 Stunden den reinen Softwarepreis bezahlen.
- Die Nutzung in der 79. Stunde ist 80, sodass Sie fünf Stunden für Windows plus den kombinierten Preis für die Hardwareverbrauchseinheit bezahlen. Der kombinierte Preis basiert entweder auf Ihrem Enterprise Agreement (EA) oder auf dem Preis für die nutzungsbasierte Bezahlung.

Wenn Sie 75 RIs erwerben, können Sie die Gesamtkosten berechnen, indem Sie die vorangegangenen stündlichen Kosten addieren. Sie können auch Ihre aktuellen Kosten anhand Ihres Tarifs berechnen. Der Unterschied zwischen den beiden Beträgen besteht in den Einsparungen, die Sie in diesem Beispiel über sieben Tage erzielen.

Die API führt Berechnungen für jeden spezifischen Nutzungspunkt aus. Anschließend wird die für maximale Einsparungen empfohlene Menge zurückgegeben. Im folgenden Beispiel zeigt das Diagramm eine maximale Einsparung bei 68. Anschließend nehmen die Einsparungen ab, daher empfiehlt die API 68.

![Diagramm mit maximalen Einsparungen](./media/reserved-instance-purchase-recommendations/peak-savings.png)

## <a name="other-expected-api-behavior"></a>Sonstiges erwartetes API-Verhalten

- Die API zeigt mögliche Einsparungen bei Verwendung des [Azure-Hybridvorteils](https://azure.microsoft.com/pricing/hybrid-benefit/) für Windows. Wenn der Vorteil nicht verwendet wird, basieren die API-Empfehlungen auf den Windows-Kernkosten. Wenn er für Sie verfügbar ist, sollten Sie den Azure-Hybridvorteil verwenden, um die Einsparungen zu erhöhen.
- Wenn Sie einen Rückblickzeitraum von sieben Tagen verwenden, erhalten Sie möglicherweise keine Empfehlungen, wenn VMs für mehr als einen Tag heruntergefahren werden.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zur [Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](../manage/understand-vm-reservation-charges.md)
