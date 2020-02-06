---
title: Grundlegendes zur Anwendung eines Reservierungsrabatts auf Azure Disk Storage
description: Erfahren Sie, wie der Rabatt für reservierte Azure-Datenträger auf Ihre verwalteten SSD Premium-Datenträger angewendet wird.
author: roygara
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2020
ms.author: rogarana
ms.openlocfilehash: 7f23aaebc20b562768fb04ae988e4aff1b62fb21
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902145"
---
# <a name="understand-how-your-reservation-discount-is-applied-to-azure-disk-storage"></a>Grundlegendes zur Anwendung Ihres Reservierungsrabatts auf Azure Disk Storage

Nach dem Erwerb von reservierter Azure-Datenträgerkapazität wird der Reservierungsrabatt automatisch auf Datenträgerressourcen angewendet, die den Reservierungsbedingungen entsprechen. Der Reservierungsrabatt gilt nur für die Datenträgerkapazität. Datenträgermomentaufnahmen werden zu den Preisen der nutzungsbasierten Bezahlung abgerechnet.

Weitere Informationen zur Azure-Datenträgerreservierung finden Sie unter [Einsparen von Kosten durch Azure-Datenträgerreservierung](../../virtual-machines/linux/disks-reserved-capacity.md).
Informationen zu den Preisen für die Azure-Datenträgerreservierung finden Sie unter [Preise für Azure-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="how-the-reservation-discount-is-applied"></a>Anwendung des Reservierungsrabatts

Der Rabatt für die Azure-Datenträgerreservierung verfällt bei Nichtnutzung und wird stündlich auf verwaltete Datenträgerressourcen angewendet. Wenn Sie also in einer bestimmten Stunde keine verwalteten Datenträgerressourcen verwenden, die den Reservierungsbedingungen entsprechen, verlieren Sie eine Reservierungsmenge für die betreffende Stunde. Nicht genutzte Stunden verfallen.

Wenn Sie eine Ressource löschen, gilt der Reservierungsrabatt automatisch für eine andere entsprechende Ressource im angegebenen Reservierungsumfang. Werden im angegebenen Reservierungsumfang keine entsprechenden Ressourcen gefunden, gehen die reservierten Stunden verloren.

## <a name="discount-examples"></a>Beispiele für die Anwendung des Rabatts

Die folgenden Beispiele zeigen, wie der Rabatt für die Azure-Datenträgerreservierung abhängig von den jeweiligen Bereitstellungen angewendet wird:

Angenommen, Sie haben für eine Laufzeit von einem Jahr eine reservierte Kapazität von 100 P30-Datenträgern (~1 TiB pro Datenträger) in der Region „USA, Westen 2“ erworben. Die Kosten dieser Beispielreservierung liegen bei 140 100 USD. Sie können entweder den gesamten Betrag vorab bezahlen oder sich für die Zahlung von zwölf festen Monatsraten in Höhe von jeweils 11.675 USD entscheiden.
Für diese Beispiele gehen wir davon aus, dass Sie sich für einen monatlichen Zahlungsplan entschieden haben. In den folgenden Szenarien erfahren Sie, was passiert, wenn Sie Ihre reservierte Kapazität unter- oder überschreiten bzw. einteilen.

### <a name="underusing-your-capacity"></a>Unterschreitung Ihrer Kapazität

Angenommen, Sie haben in einer bestimmten Stunde innerhalb des Reservierungszeitraums von Ihrer Datenträgerreservierung von 100 P30 SSD Premium-Datenträgern nur 99 bereitgestellt. Der verbleibende P30-Datenträger wird in dieser Stunde nicht verwendet und nicht übertragen.

### <a name="overusing-your-capacity"></a>Überschreitung Ihrer Kapazität

Angenommen, Sie haben in einer bestimmten Stunde innerhalb des Reservierungszeitraums 101 P30 SSD Premium-Datenträger verwendet. Der Reservierungsrabatt gilt nur für 100 P30-Datenträger, und der überzählige P30-Datenträger wird für diese Stunde zu den Preisen für die nutzungsbasierte Bezahlung abgerechnet. Wenn Ihre Nutzung in der nächsten Stunde auf 100 P30-Datenträger zurückgeht, wird die gesamte Nutzung durch die Reservierung abgedeckt.

### <a name="tiering-your-capacity"></a>Einteilen Ihrer Kapazität

Angenommen, Sie möchten in einer bestimmten Stunde innerhalb Ihres Reservierungszeitraums insgesamt 200 P30 SSD Premium-Datenträger verwenden. In den ersten 30 Minuten verwenden Sie nur 100 Datenträger. Während dieses Zeitraums wird Ihre Nutzung vollständig abgedeckt, da Sie eine Reservierung für 100 P30-Datenträger vorgenommen haben. Wenn Sie dann die Verwendung der ersten 100 beenden (sodass die Nutzung auf null zurückgeht) und dann mit der Verwendung der anderen 100 für die verbleibenden 30 Minuten beginnen, ist auch diese Nutzung im Rahmen Ihrer Reservierung abgedeckt.

![Beispieldarstellung für die Unterschreitung, Überschreitung und Einteilung der Kapazität](media/understand-disk-reservations/reserved-disks-example-scenarios.png)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Senken der Kosten durch die Azure-Datenträgerreservierung (Linux)](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Senken der Kosten durch die Azure-Datenträgerreservierung (Windows)](../../virtual-machines/windows/disks-reserved-capacity.md)
- [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)