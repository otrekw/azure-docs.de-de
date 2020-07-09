---
title: Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen
description: Es wird beschrieben, wie Sie für Azure-Reservierungen einen Umtausch durchführen oder Rückerstattungen erhalten können.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/12/2020
ms.author: banders
ms.openlocfilehash: 3c2f92db1f65a4e9e57ad443a9f000ab3f6567d5
ms.sourcegitcommit: 51718f41d36192b9722e278237617f01da1b9b4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101019"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen

Mit Azure-Reservierungen können Sie Ihre sich ändernden Anforderungen flexibel erfüllen. Sie können eine Reservierung in eine andere Reservierung desselben Typs umtauschen. Das heißt, dass Sie eine Reservierung für einen virtuellen Computer in eine Reservierung für eine beliebige Größe oder Region des virtuellen Computers umtauschen können. Ebenso können Sie eine Reservierung für eine SQL-PaaS-Datenbank in eine Reservierung für einen beliebigen Typ oder eine beliebige Region der SQL-PaaS-Datenbank umtauschen. Sie können sich Reservierungen auch zurückerstatten lassen, doch darf die Summe der stornierten Reservierungen einen Betrag von 50.000 US-Dollar in einem rollierenden Zeitfenster von zwölf Monaten nicht überschreiten. Reservierte Azure Databricks-Kapazität, Reservierungen für Azure VMware Solution by CloudSimple, Reservierungen für Azure Red Hat Open Shift, Red Hat-Pläne sowie SUSE Linux-Pläne sind nicht erstattungsfähig.

Die Self-Service-Umtausch- und Stornierungsfunktion steht für US Government Enterprise Agreement-Kunden nicht zur Verfügung. Andere US Government-Abonnementtypen, einschließlich nutzungsbasierter Bezahlung und Cloud Solution Provider (CSP), werden unterstützt.

Sie benötigen Besitzerzugriff auf den Reservierungsauftrag, um eine vorhandene Reservierung umzutauschen oder zurückzuerstatten. Sie können [Benutzer, die eine Reservierung verwalten können, hinzufügen oder ändern](https://docs.microsoft.com/azure/cost-management-billing/reservations/manage-reserved-vm-instance#add-or-change-users-who-can-manage-a-reservation).

## <a name="how-to-exchange-or-refund-an-existing-reservation"></a>Umtauschen oder Zurückerstatten einer vorhandenen Reservierung

Sie können Ihre Reservierung über das [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) umtauschen.

1. Wählen Sie die Reservierungen aus, die Sie umtauschen möchten, und klicken Sie auf **Umtausch**.  
    [![Beispielbild mit Reservierungen, die zurückgegeben werden sollen](./media/exchange-and-refund-azure-reservations/exchange-refund-return.png)](./media/exchange-and-refund-azure-reservations/exchange-refund-return.png#lightbox)
1. Wählen Sie das VM-Produkt aus, das Sie erwerben möchten, und geben Sie eine Menge ein. Achten Sie darauf, dass der neue Gesamtbetrag des Kaufs höher als der Umtauschwert ist. [Bestimmen der richtigen Größe vor dem Kauf](../../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy)  
    [![Beispielbild mit dem VM-Produkt, das per Umtausch erworben werden soll](./media/exchange-and-refund-azure-reservations/exchange-refund-select-purchase.png)](./media/exchange-and-refund-azure-reservations/exchange-refund-select-purchase.png#lightbox)
1. Überprüfen Sie die Transaktion, und schließen Sie sie ab.  
    [![Beispielbild mit dem VM-Produkt, das per Umtausch erworben werden soll, und Abschluss des Vorgangs](./media/exchange-and-refund-azure-reservations/exchange-refund-confirm-exchange.png)](./media/exchange-and-refund-azure-reservations/exchange-refund-confirm-exchange.png#lightbox)

Navigieren Sie zum Erhalten einer Erstattung für eine Reservierung zu **Reservierungsdetails**, und klicken Sie auf **Erstattung**.

## <a name="exchange-non-premium-storage-for-premium-storage"></a>Umtauschen einer Reservierung ohne Storage Premium gegen eine Reservierung mit Storage Premium

Sie können eine erworbene Reservierung für eine VM-Größe, für die Storage Premium nicht unterstützt wird, gegen eine entsprechende VM-Größe umtauschen, für die diese Unterstützung vorhanden ist. Beispiel: _F1_ gegen _F1s_. Navigieren Sie zum Durchführen des Umtauschs zu „Reservierungsdetails“, und klicken Sie auf **Umtausch**. Beim Umtausch wird der Zeitraum der reservierten Instanz nicht zurückgesetzt und auch keine neue Transaktion erstellt. 

## <a name="how-transactions-are-processed"></a>Verarbeitung von Transaktionen

Zunächst storniert Microsoft die vorhandene Reservierung und erstattet den entsprechenden anteiligen Betrag. Bei einem Umtausch wird der neue Kauf verarbeitet. Microsoft nutzt für die Verarbeitung von Erstattungen eine der folgenden Methoden. Dies richtet sich nach Ihrem Kontotyp und der Zahlungsmethode:

### <a name="enterprise-agreement-customers"></a>Enterprise Agreement-Kunden

Ein Geldbetrag wird der finanziellen Verpflichtung für Umtausch und Rückerstattungen hinzugefügt, wenn dies Teil des ursprünglichen Kaufs war. Falls der Zeitraum der finanziellen Verpflichtung für die Nutzung der erworbenen Reservierung nicht mehr aktiv ist, wird das Guthaben dem Zeitraum Ihrer finanziellen Verpflichtung unter dem derzeitigen Enterprise Agreement hinzugefügt. Das Guthaben ist ab dem Datum der Rückerstattung 90 Tage lang gültig. Nicht verwendetes Guthaben verfällt nach 90 Tagen.

Wenn der ursprüngliche Kauf als Überschreitung erfolgte, werden die Ursprungsrechnung für den Erwerb der Reservierung und alle nachfolgenden Rechnungen erneut geöffnet und angepasst. Microsoft gibt eine Gutschrift für die Rückerstattungen aus.

### <a name="pay-as-you-go-invoice-payments-and-csp-program"></a>Nutzungsbasierte Bezahlung und das CSP-Programm

Die ursprüngliche Rechnung für den Kauf der Reservierung wird storniert, und anschließend wird eine neue Rechnung für die Erstattung erstellt. Bei einem Umtausch werden für die neue Rechnung die Erstattung und der neue Kauf angezeigt. Der Erstattungsbetrag wird anhand des Kaufbetrags angepasst. Falls Sie nur eine Erstattung für eine Reservierung durchgeführt haben, verbleibt der anteilige Betrag bei Microsoft, und die Anpassung wird bei einem zukünftigen Kauf einer Reservierung vorgenommen. Wenn Sie eine Reservierung zu Tarifen für die nutzungsbasierte Bezahlung gekauft haben und später zu CSP wechseln, kann die Reservierung zurückgegeben und ohne Strafgebühr erneut erworben werden.

### <a name="pay-as-you-go-credit-card-customers"></a>Kreditkartenkunden mit nutzungsbasierter Bezahlung

Die ursprüngliche Rechnung wird storniert, und eine neue Rechnung wird erstellt. Der Betrag wird über die Kreditkarte erstattet, die für den ursprünglichen Kauf genutzt wurde. [Wenden Sie sich an den Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest), falls sich Ihre Kartenangaben geändert haben.

## <a name="cancel-exchange-and-refund-policies"></a>Richtlinien für Stornierung, Umtausch und Rückerstattung

In Azure gelten folgende Richtlinien für Stornierungen, Umtausch und Rückerstattungen:

**Umtauschrichtlinien**

- Sie können mehrere vorhandene Reservierungen zurückgeben, um eine neue Reservierung des gleichen Typs zu erwerben. Ein Wechsel des Typs von Reservierungen ist beim Umtausch nicht möglich. Beispielsweise können Sie keine VM-Reservierung zurückgeben, um eine SQL-Reservierung zu erwerben. Bei einem Umtausch können Sie eine Reservierungseigenschaft ändern, z. B. Familie, Serie, Version, SKU, Region, Menge und Zeitraum.
- Nur Besitzer von Reservierungen können einen Umtausch verarbeiten. [Informieren Sie sich über das Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Ein Umtausch wird als Erstattung und erneuter Kauf verarbeitet. Für die Stornierung und den neuen Reservierungserwerb werden separate Transaktionen erstellt. Der anteilige Reservierungsbetrag für die zurückgegebenen Reservierungen wird erstattet. Der neue Kauf wird Ihnen vollständig berechnet. Der anteilige Reservierungsbetrag ist der tägliche anteilige Restwert der zurückgegebenen Reservierung.
- Sie können Reservierungen auch dann umtauschen oder zurückgeben, wenn das für den Kauf der Reservierung verwendete Enterprise Agreement abgelaufen ist und in Form eines neuen Vertrags verlängert wurde.
- Die Laufzeit der neuen Reservierung sollte gleich oder größer als die verbleibende Laufzeit der zurückgegebenen Reservierung sein. Beispiel: Bei einer dreijährigen Reservierung mit einem Wert von 100 US-Dollar pro Monat, die nach der 18. Zahlung umgetauscht wird, sollte die Laufzeit der neuen Reservierung mindestens einem Wert von 1.800 US-Dollar entsprechen (monatlich oder im Voraus bezahlt).
- Für die neue Reservierung, die im Rahmen des Umtauschs erworben wird, gilt ein neuer Zeitraum, der ab dem Umtauschzeitpunkt beginnt.
- Für den Umtausch gibt es keine Strafzahlungen oder jährlichen Grenzwerte.

**Rückerstattungsrichtlinien**

- Möglicherweise fällt für die vorzeitige Beendigung eine Gebühr in Höhe von 12 % an.
- Die stornierte Verpflichtung insgesamt darf in einem rollierenden Zeitfenster von zwölf Monaten den Wert von 50.000 US-Dollar nicht überschreiten. Beispiel: Bei einer dreijährigen Reservierung mit einem Wert von 100 US-Dollar pro Monat, die im 18. Monat zurückerstattet wird, beträgt die stornierte Verpflichtung 1.800 US-Dollar. Nach der Rückerstattung beträgt das neue verfügbare Rückerstattungslimit 48.200 US-Dollar. 365 Tage nach der Erstattung wird das Limit von 48.200 US-Dollar um 1.800 US-Dollar erhöht, und der neue Pool beläuft sich auf 50.000 US-Dollar. Durch jede weitere Reservierungsstornierung wird derselbe Pool verringert, und die gleiche Auffüllungslogik wird angewandt.
- Rückerstattungen werden auf der Grundlage des niedrigsten Preises (Ihr Kaufpreis oder der aktuelle Preis für die Reservierung) berechnet.
- Nur Besitzer von Reservierungsaufträgen können eine Erstattung verarbeiten. [Informieren Sie sich über das Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md).
- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
    - [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)
    - [Verwalten von Reservierungen für Ressourcen in Azure](manage-reserved-vm-instance.md)
    - [Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](../manage/understand-vm-reservation-charges.md)
    - [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](understand-reserved-instance-usage.md)
    - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](understand-reserved-instance-usage-ea.md)
    - [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](reserved-instance-windows-software-costs.md)
    - [Azure-Reservierungen im CSP-Programm](/partner-center/azure-reservations)
