---
title: Erwerben von Azure-Reservierungen mit Vorauszahlung oder monatlicher Zahlung
description: Hier erfahren Sie, wie Sie Azure-Reservierungen mit Vorauszahlung oder monatlicher Zahlung erwerben.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: banders
ms.openlocfilehash: 77d663fa01e24acf63acd68d0b8d7cf4cc741055
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587089"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Erwerben von Reservierungen mit monatlicher Zahlung

In der Vergangenheit mussten Azure-Reservierungen im Voraus bezahlt werden. Inzwischen können Sie Reservierungen aber auch monatlich bezahlen. Bei einem Kauf mit Vorauszahlung zahlen Sie den gesamten Betrag. Bei der monatlichen Zahlungsoption werden dagegen die Gesamtkosten der Reservierung gleichmäßig auf die einzelnen Monate der Laufzeit aufgeteilt. Die Gesamtkosten für vorab bezahlte und monatliche Reservierungen sind gleich. Es fallen keine zusätzlichen Gebühren an, wenn Sie sich für die monatliche Zahlung entscheiden.

Ihr monatlicher Zahlungsbetrag variiert möglicherweise abhängig vom Wechselkurs des aktuellen Monats für Ihre lokale Währung.

Monatliche Zahlungen sind für Folgendes verfügbar:

- Virtuelle Computer
- Azure Storage
- SQL-DATENBANKEN
- SQL Data Warehouse
- Cosmos DB
- App Service-Stempelgebühr
- Verwalteter Datenträger
- Azure-Daten-Explorer
- Azure Database for MariaDB, MySQL und PostgreSQL
- Azure VMware Solution by CloudSimple


Reservierungen können über das [Azure-Portal](https://portal.azure.com/?Microsoft_Azure_Reservations_EnableMultiCart=true&amp;paymentPlan=true#blade/Microsoft_Azure_Reservations/CreateBlade) erworben werden.

![Beispiel für Reservierungserwerb](./media/monthly-payments-reservations/purchase-reservation.png)

Wenn Sie eine Reservierung erwerben, können Sie den Zahlungsplan anzeigen. Klicken Sie auf **Vollständigen Zahlungsplan anzeigen**.

![Beispiel für den Zahlungsplan einer Reservierung](./media/monthly-payments-reservations/prepurchase-schedule.png)

Wenn Sie den Zahlungsplan nach dem Kauf anzeigen möchten, wählen Sie eine Reservierung aus, klicken Sie auf die **ID des Reservierungsauftrags**, und klicken Sie anschließend auf die Registerkarte **Zahlungen**.

## <a name="view-payments-made"></a>Anzeigen geleisteter Zahlungen

Geleistete Zahlungen können mithilfe von APIs und Nutzungsdaten sowie in der Kostenanalyse angezeigt werden. Bei Reservierungen mit monatlicher Zahlung wird der Häufigkeitswert in Nutzungsdaten und in der Reservierungsgebühren-API als **Serie** angezeigt. Bei Reservierungen mit Vorauszahlung wird der Wert als **Einmalig** angezeigt.

In der Standardansicht der Kostenanalyse werden monatliche Käufe angezeigt. Wenden Sie auf **Gebührentyp** den Filter **Einkauf** und auf **Häufigkeit** den Filter **Serie** an, um alle Käufe anzuzeigen. Sollen nur Reservierungen angezeigt werden, wenden Sie einen Filter für **Reservierung** an.

![Beispiel für Reservierungserwerbskosten in der Kostenanalyse](./media/monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Umstellen auf monatliche Zahlung bei der Verlängerung

Wenn Sie eine Reservierung verlängern, können Sie auf eine monatliche Abrechnungshäufigkeit umstellen.

## <a name="exchange-and-refunds"></a>Umtausch und Rückerstattungen

Mit monatlicher Abrechnung erworbene Reservierungen können genau wie andere Reservierungen erstattet oder umgetauscht werden. Aktuell können Sie eine Supportanfrage übermitteln, um einen Umtausch oder eine Rückerstattung für eine mit monatlicher Abrechnung erworbene Reservierung zu initiieren.

Wenn Sie eine Reservierung mit monatlicher Zahlung umtauschen, müssen die Kosten für die gesamte Lebensdauer des neuen Einkaufs höher sein als die Restzahlungen, die für die zurückgegebene Reservierung storniert werden. Ansonsten gelten für den Umtausch keine weiteren Einschränkungen, und es fallen auch keine Gebühren an. Sie können eine Reservierung mit Vorauszahlung umtauschen, um eine neue Reservierung mit monatlicher Abrechnung zu erwerben. Der Wert für die Lebensdauer der neuen Reservierung muss jedoch höher sein als der anteilige Wert der zurückgegebenen Reservierung.

Wenn Sie eine Reservierung mit monatlicher Zahlung stornieren, kann Microsoft für die stornierten zugesagten Zahlungen ggf. eine Stornogebühr in Höhe von 12 % erheben. Diese Strafgebühr wird von Microsoft derzeit aber nicht berechnet. Stornierte zugesagte Zahlungen werden mit dem Rückerstattungslimit von 50.000 USD verrechnet. Falls eine Strafgebühr für die Stornierung berechnet wird, hat dies keine Auswirkung auf das Rückerstattungslimit.

Weitere Informationen zu Umtausch und Rückerstattungen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](exchange-and-refund-azure-reservations.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Reservierungen finden Sie unter [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md).
- Informationen zu Aufgaben, die vor dem Erwerb einer Reservierung ausgeführt werden sollten, finden Sie unter [Bestimmen der passenden Größe des virtuellen Computers vor dem Kauf](../../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).
