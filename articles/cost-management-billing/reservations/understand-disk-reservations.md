---
title: Grundlegendes zum Reservierungsrabatt für Azure Disk Storage
description: Hier erfahren Sie, wie ein Rabatt für reservierte Azure-Datenträger auf Ihre verwalteten Azure-SSD Premium-Datenträger angewendet wird.
author: roygara
ms.author: rogarana
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: cd89c3ddc3c58de02f1104109ce7f243c4d1e6fd
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682449"
---
# <a name="understand-how-your-reservation-discount-is-applied-to-azure-disk-storage"></a>Grundlegendes zur Anwendung Ihres Reservierungsrabatts auf Azure-Datenträgerspeicher

Nach dem Erwerb von reservierter Azure-Datenträgerkapazität wird ein Reservierungsrabatt automatisch auf Datenträgerressourcen angewendet, die den Reservierungsbedingungen entsprechen. Der Reservierungsrabatt gilt nur für Datenträger-SKUs. Für Datenträger-Momentaufnahmen werden die Preise für die nutzungsbasierte Bezahlung verwendet.

Weitere Informationen zur Azure-Datenträgerreservierung finden Sie unter [Einsparen von Kosten durch Azure-Datenträgerreservierung](../../virtual-machines/disks-reserved-capacity.md). Informationen zu den Preisen für die Azure-Datenträgerreservierung finden Sie unter [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="how-the-reservation-discount-is-applied"></a>Anwendung des Reservierungsrabatts

Nicht in Anspruch genommener Rabatt für die Azure-Datenträgerreservierung geht verloren. Der Rabatt wird stündlich auf verwaltete Datenträgerressourcen angewendet. Wenn also in einer Stunde keine verwalteten Datenträgerressourcen vorhanden sind, die den Reservierungsbedingungen entsprechen, verlieren Sie eine Reservierungsmenge für diese Stunde. Ungenutzte Stunden werden nicht übertragen.

Wenn Sie eine Ressource löschen, gilt der Reservierungsrabatt automatisch für eine andere entsprechende Ressource im angegebenen Reservierungsumfang. Wird keine entsprechende Ressource gefunden, gehen die reservierten Stunden verloren.

## <a name="discount-examples"></a>Beispiele für die Anwendung des Rabatts

Die folgenden Beispiele zeigen, wie der Rabatt für die Azure-Datenträgerreservierung abhängig der Bereitstellung angewendet wird:

Angenommen, Sie erwerben und reservieren 100 Datenträger vom Typ P30 in der Region „USA, Westen 2“ mit einer Laufzeit von einem Jahr. Jeder Datenträger verfügt über etwa 1 TiB Speicherplatz. Die Kosten dieser Beispielreservierung liegen bei 140.100 USD. Sie können entweder den gesamten Betrag im Voraus bezahlen oder sich für die Zahlung von zwölf festen Monatsraten in Höhe von jeweils 11.675 USD entscheiden.

In den folgenden Szenarien erfahren Sie, was passiert, wenn Sie Ihre reservierte Kapazität unter- oder überschreiten bzw. einteilen. Bei diesen Beispielen wird davon ausgegangen, dass Sie sich für einen monatlichen Zahlungsplan entschieden haben.

### <a name="underusing-your-capacity"></a>Unterschreitung Ihrer Kapazität

Angenommen, Sie stellen für eine Stunde innerhalb des Reservierungszeitraums nur 99 Ihrer 100 reservierten Azure-SSD Premium-Datenträger (Solid State Drive) vom Typ P30 bereit. Der übrige P30-Datenträger wird während dieser Stunde nicht verwendet. Er wird auch nicht übertragen.

### <a name="overusing-your-capacity"></a>Überschreitung Ihrer Kapazität

Angenommen, Sie verwenden in einer Stunde innerhalb des Reservierungszeitraums 101 SSD Premium-Datenträger vom Typ P30. Der Reservierungsrabatt gilt nur für 100 P30-Datenträger. Der zusätzliche P30-Datenträger wird zum Preis für die nutzungsbasierte Bezahlung für diese Stunde abgerechnet. Wenn Ihre Nutzung in der nächsten Stunde auf 100 P30-Datenträger zurückgeht, ist die gesamte Nutzung durch die Reservierung abgedeckt.

### <a name="tiering-your-capacity"></a>Einteilen Ihrer Kapazität

Angenommen, Sie möchten in einer Stunde innerhalb Ihres Reservierungszeitraums insgesamt 200 SSD Premium-Datenträger vom Typ P30 verwenden. Nehmen wir weiter an, dass in den ersten 30 Minuten nur 100 Datenträger genutzt werden. Während dieses Zeitraums ist Ihre Nutzung vollständig abgedeckt, da Ihre Reservierung 100 Datenträger vom Typ P30 umfasst. Wenn Sie dann die Verwendung der ersten 100 beenden (sodass die Nutzung auf null zurückgeht) und dann mit der Verwendung der anderen 100 für die verbleibenden 30 Minuten beginnen, ist auch diese Nutzung im Rahmen Ihrer Reservierung abgedeckt.

![Beispiel für die Unterschreitung, Überschreitung und Einteilung der Kapazität](media/understand-disk-reservations/reserved-disks-example-scenarios.png)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Senken der Kosten durch Azure-Datenträgerreservierung](../../virtual-machines/disks-reserved-capacity.md)
- [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)
