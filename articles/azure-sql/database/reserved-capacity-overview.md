---
title: Sparen von Computekosten mit reservierter Kapazität
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Erfahren Sie, wie Sie reservierte Kapazitäten für Azure SQL-Datenbank- und verwaltete SQL-Instanzen kaufen, um Computekosten einzusparen.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 08/29/2019
ms.openlocfilehash: c02368b5713c0743cdca764275f76dda0e0926d2
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84119098"
---
# <a name="save-costs-for-resources-with-reserved-capacity---azure-sql-database--sql-managed-instance"></a>Sparen von Kosten für Ressourcen mit reservierter Kapazität – Azure SQL-Datenbank und SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)] 

Sparen Sie mit Azure SQL-Datenbank und SQL Managed Instance Geld im Vergleich zur nutzungsbasierten Bezahlung, indem Sie sich auf eine Reservierung für Computeressourcen festlegen. Mit reservierten Kapazitäten legen Sie sich für die Nutzung von SQL-Datenbank und/oder SQL Managed Instance für einen Zeitraum von einem oder drei Jahren fest und profitieren dafür von einem immensen Rabatt auf Computekosten. Um reservierte Kapazität zu erwerben, müssen Sie die Azure-Region, den Bereitstellungstyp, die Leistungsstufe und die Laufzeit angeben.

Sie müssen die Reservierung nicht einer bestimmten SQL-Datenbank- oder verwalteten SQL-Instanz zuweisen. Entsprechende vorhandene Bereitstellungen, die bereits ausgeführt oder neu bereitgestellt werden, profitieren automatisch von dem Vorteil. Beim Kauf einer Reservierung zahlen Sie für einen Zeitraum von einem oder drei Jahren die für die Nutzung anfallenden Computekosten. Sobald Sie eine Reservierung erworben haben, werden die Computegebühren, die den Reservierungsattributen entsprechen, nicht mehr zu den Preisen der nutzungsbasierten Bezahlung abgerechnet. Eine Reservierung deckt nicht die mit dem Dienst verknüpften Software-, Netzwerk- oder Speichergebühren ab. Nach Ablauf der Reservierungslaufzeit erlischt der Abrechnungsvorteil, und die SQL-Datenbank- bzw. verwaltete SQL-Instanz wird mit den Preisen für die nutzungsbasierte Bezahlung in Rechnung gestellt. Reservierungen werden nicht automatisch verlängert. Weitere Informationen zu den Preisen finden Sie unter [Azure SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Reservierte Kapazität kann über das [Azure-Portal](https://portal.azure.com) erworben werden. Bezahlen Sie die Reservierung [im Voraus oder monatlich](../../cost-management-billing/reservations/prepare-buy-reservation.md). So erwerben Sie reservierte Kapazität:

- Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein individuelles Abonnement mit nutzungsbasierter Bezahlung zugeordnet sein.
- Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Wenn diese Einstellung deaktiviert ist, müssen Sie ein EA-Administrator für das Abonnement sein. Reservierte Kapazität.

Einzelheiten zur Berechnung der Reservierung von Kapazitäten für Unternehmenskunden und Kunden mit nutzungsbasierter Bezahlung finden Sie unter [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) und [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../../cost-management-billing/reservations/understand-reserved-instance-usage.md).

## <a name="determine-correct-size-before-purchase"></a>Ermitteln der richtigen Größe vor dem Erwerb

Die Größe der Reservierung sollte auf der Gesamtmenge der Computeressourcen basieren, die von den bereits vorhandenen oder in Kürze zu erstellenden SQL-Datenbank- bzw. verwaltete SQL-Instanzen innerhalb einer bestimmten Region genutzt werden und die gleiche Leistungsstufe und Hardwaregeneration verwenden.

Nehmen Sie beispielsweise an, Sie führen einen Pool für elastische Gen5-Datenbanken mit der SKU „Universell“ und 16 virtuellen Kernen sowie zwei Gen5-Einzeldatenbanken mit der SKU „Unternehmenskritisch“ und 4 virtuellen Kernen aus. Sie möchten innerhalb des nächsten Monats einen weiteren Pool für elastische Gen5-Datenbanken mit der SKU „Universell“ und 16 virtuellen Kernen sowie einen Pool für elastische Gen5-Datenbanken mit der SKU „Unternehmenskritisch“ und 32 virtuellen Kernen bereitstellen. Darüber hinaus wissen Sie, dass Sie diese Ressourcen mindestens 1 Jahr benötigen. In diesem Fall sollten Sie Folgendes erwerben: eine 1-Jahres-Reservierung für Singletons oder Pools für elastische Datenbanken mit der SKU „Universell“ (Gen 5) und 32 virtuellen Kernen (2 × 16) sowie eine 1-Jahres-Reservierung für Singletons bzw. Pools für elastische Datenbanken (Gen 5) mit der SKU „Unternehmenskritisch“ und 40 virtuellen Kernen (2 × 4 + 32).

## <a name="buy-reserved-capacity"></a>Erwerben von reservierter Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie **Hinzufügen** und dann im Bereich **Reservierungen erwerben** die Option **SQL-Datenbank** aus, um eine neue Reservierung für SQL-Datenbank zu erwerben.
4. Füllen Sie die Pflichtfelder aus. Vorhandene SQL-Datenbank-Instanzen (einzeln oder in einem Pool) oder verwaltete SQL-Instanzen, die den von Ihnen ausgewählten Attributen entsprechen, wird der Rabatt auf reservierte Kapazitäten angewandt. Die tatsächliche Anzahl Ihrer SQL-Datenbank- oder verwalteten SQL-Instanzen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und der ausgewählten Menge ab.
    ![Screenshot vor der Übermittlung des Kaufs der reservierten Kapazität](./media/reserved-capacity-overview/sql-reserved-vcores-purchase.png)

In der folgenden Tabelle werden die erforderlichen Felder beschrieben.

| Feld      | BESCHREIBUNG|
|------------|--------------|
|Subscription|Das zum Bezahlen für die Kapazitätsreservierung verwendete Abonnement. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung belastet. Der Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder eine einzelne Vereinbarung mit Preisen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) sein. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem individuellen Abonnement mit Preisen für nutzungsbasierte Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für Zahlung auf Rechnung für das Abonnement in Rechnung gestellt.|
|`Scope`       |Die Reservierung virtueller Kerne kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <br/><br/>**Gemeinsam**: Der Rabatt auf die Reservierung virtueller Kerne wird auf eine SQL-Datenbank- oder verwaltete SQL-Instanz angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt wird. Für Enterprise-Kunden stellt der freigegebene Bereich die Registrierung dar und umfasst alle Abonnements in der Registrierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.<br/><br/>**Einzelabonnement**: Der Rabatt auf die Reservierung virtueller Kerne wird auf SQL-Datenbank- oder verwaltete SQL-Instanzen in diesem Abonnement angewendet. <br/><br/>**Einzelne Ressourcengruppe**: Der Reservierungsrabatt wird auf die SQL-Datenbank- bzw. verwalteten SQL-Instanzen im ausgewählten Abonnement und die ausgewählte Ressourcengruppe in diesem Abonnement angewendet.|
|Region      |Die Azure-Region, die durch die Kapazitätsreservierung abgedeckt wird.|
|Bereitstellungstyp|Der SQL-Ressourcentyp, für den Sie die Reservierung erwerben möchten.|
|Leistungsstufe|Die Dienstebene für die SQL-Datenbank- oder verwalteten SQL-Instanzen. |
|Begriff        |Ein Jahr oder drei Jahre|
|Menge    |Die Menge an Computeressourcen, die innerhalb der Kapazitätsreservierung erworben werden. Die Menge entspricht einer Anzahl von virtuellen Kernen in der ausgewählten Azure-Region und der Leistungsstufe, die reserviert werden und den Abrechnungsrabatt erhalten. Wenn Sie beispielsweise mehrere SQL-Datenbank-Instanzen mit der gesamten Computekapazität von 16 virtuellen Gen5-Kernen in der Region „USA, Osten“ ausführen oder ausführen möchten, geben Sie als Menge „16“ an, um die Vorteile für alle Datenbanken zu maximieren. |

1. Überprüfen Sie die Kosten für die Kapazitätsreservierung im Abschnitt **Kosten**.
1. Wählen Sie die Option **Kaufen**.
1. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können unter bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](../../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="vcore-size-flexibility"></a>Flexibilität der V-Kern-Größe

Die Flexibilität der V-Kern-Größe ermöglicht ein Hoch- oder Herunterskalieren innerhalb einer Leistungsstufe und Region, ohne den Vorteil reservierter Kapazität einzubüßen. Reservierte Kapazität bietet Ihnen auch die Flexibilität, Ihre aktiven SQL-Datenbank-Instanzen als Teil Ihres normalen Betriebs vorübergehend in Pools für elastische Datenbanken (innerhalb derselben Region und Leistungsstufe) zu verschieben, ohne den Vorteil der reservierten Kapazität zu verlieren. Dank eines nicht angewendeten Puffers in Ihrer Reservierung können Sie effektiv Leistungsspitzen verwalten, ohne Ihr Budget zu überschreiten.

## <a name="limitation"></a>Einschränkung

In SQL-Datenbank können Sie keine serverlosen bzw. DTU-basierten Datenbanken (Basic, Standard oder Premium) reservieren.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

Der Rabatt auf die Reservierung virtueller Kerne wird automatisch auf die Anzahl der SQL-Datenbank- bzw. verwalteten SQL-Instanzen angewendet, die dem Reservierungsumfang und entsprechenden Attributen entsprechen. Sie können den Bereich der Kapazitätsreservierung über das [Azure-Portal](https://portal.azure.com), PowerShell, die CLI oder die API aktualisieren.

Informationen zum Verwalten der Kapazitätsreservierung finden Sie unter [Verwalten von Reservekapazität](../../cost-management-billing/reservations/manage-reserved-vm-instance.md).

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](../../cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Verwalten von Azure-Reservierungen](../../cost-management-billing/reservations/manage-reserved-vm-instance.md)
- [Grundlegendes zum Rabatt für Azure-Reservierungen](../../cost-management-billing/reservations/understand-reservation-charges.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../../cost-management-billing/reservations/understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
- [Verkaufen Microsoft Azure Reserved Instances](https://docs.microsoft.com/partner-center/azure-reservations)
