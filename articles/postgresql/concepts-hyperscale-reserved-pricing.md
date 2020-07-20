---
title: Preise für reserviertes Compute – Azure Database for PostgreSQL – Hyperscale (Citus)
description: Vorauszahlung für Azure Database for PostgreSQL – Hyperscale (Citus)-Computeressourcen mit reservierter Kapazität
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: dcf38a3bcd41ba41bb7cf3b16d022a7a45734c4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85217934"
---
# <a name="prepay-for-azure-database-for-postgresql---hyperscale-citus-compute-resources-with-reserved-capacity"></a>Vorauszahlung für Azure Database for PostgreSQL – Hyperscale (Citus)-Computeressourcen mit reservierter Kapazität

Mit Azure Database for PostgreSQL – Hyperscale (Citus) können Sie jetzt Geld im Vergleich zur nutzungsbasierten Bezahlung sparen, indem Sie Computeressourcen im Voraus bezahlen. Mit reservierter Hyperscale (Citus)-Kapazität leisten Sie eine Vorauszahlung für eine Hyperscale (Citus)-Servergruppe für einen Zeitraum von einem Jahr oder drei Jahren und erhalten dafür einen umfangreichen Rabatt auf die Computekosten. Um reservierte Hyperscale (Citus)-Kapazität zu erwerben, müssen Sie die Azure-Region, den Reservierungszeitraum und die Abrechnungshäufigkeit angeben.

> [!IMPORTANT]
> Auf dieser Seite geht es um die reservierte Kapazität für Azure Database for PostgreSQL – Hyperscale (Citus). Auf [dieser Seite](/azure/postgresql/concept-reserved-pricing) finden Sie Informationen zur reservierten Kapazität für einen Azure Database for PostgreSQL-Einzelserver.

Sie müssen die Reservierung nicht bestimmten Hyperscale (Citus)-Servergruppen zuweisen. Eine Hyperscale (Citus)-Servergruppe, die bereits ausgeführt oder neu bereitgestellt wird, profitiert automatisch von dem Reservierungspreisvorteil. Beim Kauf einer Reservierung bezahlen Sie im Voraus die Computekosten für einen Zeitraum von einem oder drei Jahren. Sobald Sie eine Reservierung erworben haben, werden die Hyperscale (Citus)-Computegebühren, die den Reservierungsattributen entsprechen, nicht mehr zu den Preisen der nutzungsbasierten Bezahlung abgerechnet. Eine Reservierung deckt nicht die Software-, Netzwerk- oder Speichergebühren für die Hyperscale (Citus)-Servergruppen ab. Nach Ablauf des Reservierungszeitraums erlischt der Abrechnungsvorteil, und die Hyperscale (Citus)-Servergruppen werden mit den Preisen für die nutzungsbasierte Bezahlung in Rechnung gestellt. Reservierungen werden nicht automatisch verlängert. Weitere Informationen zu den Preisen finden Sie unter [Azure Database for PostgreSQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/).

Reservierte Hyperscale (Citus)-Kapazität kann über das [Azure-Portal](https://portal.azure.com/) erworben werden. Bezahlen Sie die Reservierung [im Voraus oder monatlich](https://docs.microsoft.com/azure/cost-management-billing/reservations/monthly-payments-reservations). So erwerben Sie reservierte Kapazität:

* Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein individuelles Abonnement mit nutzungsbasierter Bezahlung zugeordnet sein.
* Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com/) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Wenn diese Einstellung deaktiviert ist, müssen Sie ein EA-Administrator für das Abonnement sein.
* Für das Cloud Solution Provider-Programm (CSP) kann nur die Administrator- oder Vertriebs-Agents reservierte Hyperscale (Citus)-Kapazität erwerben.

Einzelheiten zur Berechnung der Reservierung von Kapazitäten für Unternehmenskunden und Kunden mit nutzungsbasierter Zahlung finden Sie unter [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) und [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).

## <a name="determine-the-right-server-group-size-before-purchase"></a>Bestimmen der richtigen Servergruppengröße vor dem Kauf

Die Größe der Reservierung sollte auf der Gesamtmenge der Computeressourcen basieren, die von dem bereits vorhandenen oder in Kürze bereitzustellenden Koordinator- und Workerknoten in Hyperscale (Citus)-Servergruppen innerhalb einer bestimmten Region verwendet wird.

Beispiel: Sie führen eine Hyperscale (Citus)-Servergruppe mit einem Koordinatorknoten mit 16 virtuellen Kernen und drei Workerknoten mit 8 virtuellen Kernen aus. Angenommen, Sie planen die Bereitstellung einer zusätzlichen Hyperscale (Citus)-Servergruppe mit einem Koordinatorknoten mit 32 virtuellen Kernen und zwei Workerknoten mit 4 virtuellen Kernen. Sie benötigen diese Ressourcen mindestens ein Jahr lang.

In diesem Fall sollten Sie eine einjährige Reservierung kaufen für:

* Insgesamt 16 virtuelle Kerne + 32 virtuelle Kerne = 48 virtuelle Kerne für Koordinatorknoten
* Insgesamt drei Knoten x 8 virtuelle Kerne + 2 Knoten x 4 virtuelle Kerne = 24 + 8 = 32 virtuelle Kerne für Workerknoten

## <a name="buy-azure-database-for-postgresql-reserved-capacity"></a>Kaufen von reservierter Kapazität für Azure Database for PostgreSQL

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie **Hinzufügen** und dann im Bereich „Reservierungen erwerben“ die Option **Azure Database for PostgreSQL** aus, um eine neue Reservierung für Ihre PostgreSQL-Datenbanken zu erwerben.
4. Wählen Sie den Hyperscale (Citus)-Computetyp zum Erwerb aus, und klicken Sie anschließend auf die Schaltfläche **Auswählen**.
5. Überprüfen Sie auf der Registerkarte **Produkte** die Menge für den ausgewählten Computetyp.
4. Fahren Sie mit der Registerkarte **Buy + Review** (Kaufen und überprüfen) fort, um Ihren Kauf abzuschließen.

In der folgenden Tabelle werden die erforderlichen Felder beschrieben.

| Feld        | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Subscription | Das Abonnement, das für die Zahlung der Reservierung von Azure Database for PostgreSQL-Kapazität verwendet wird. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung von Azure Database for PostgreSQL-Kapazität belastet. Der Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder eine einzelne Vereinbarung mit Preisen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) sein. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem individuellen Abonnement mit Preisen für nutzungsbasierte Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für Zahlung auf Rechnung für das Abonnement in Rechnung gestellt.                                                                                  |
| `Scope`        | Der Umfang der Reservierung virtueller Kerne kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <br><br> **Gemeinsam**: Der Rabatt auf die Reservierung virtueller Kerne wird auf Hyperscale (Citus)-Servergruppen angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Für Enterprise-Kunden stellt der freigegebene Bereich die Registrierung dar und umfasst alle Abonnements in der Registrierung.  Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden. <br><br> **Einzelabonnement**: Der Rabatt auf die Reservierung virtueller Kerne wird auf Hyperscale (Citus)-Servergruppen in diesem Abonnement angewendet. <br><br> **Einzelne Ressourcengruppe**: Der Reservierungsrabatt wird auf Hyperscale (Citus)-Servergruppen im ausgewählten Abonnement und der ausgewählten Ressourcengruppe in diesem Abonnement angewendet. |
| Region       | Die Azure-Region, die durch die Reservierung von Azure Database for PostgreSQL – Hyperscale (Citus)-Kapazität abgedeckt wird                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Begriff         | Ein Jahr oder drei Jahre                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Menge     | Die Menge an Computeressourcen, die innerhalb der reservierten Hyperscale (Citus)-Kapazitätsreservierung erworben werden. Insbesondere die Anzahl der virtuellen Kerne der Koordinator- oder Workerknoten in der ausgewählten Azure-Region, die reserviert werden und den Abrechnungsrabatt erhalten. Wenn Sie z. B. Hyperscale (Citus)-Servergruppen mit einer gesamten Computekapazität von Koordinatorknoten mit 64 virtuellen Kernen und Workerknoten mit 32 virtuellen Kernen in der Region „USA, Osten“ ausführen (oder dies planen), geben Sie die Menge als 64 und 32 für Koordinator- bzw. Workerknoten an, um den Vorteil für alle Server zu maximieren.                                                                                                                                                                                                                                                     |



## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können unter bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="vcore-size-flexibility"></a>Flexibilität der V-Kern-Größe

Die Flexibilität der V-Kern-Größe ermöglicht ein Hoch- oder Herunterskalieren der Koordinator- und Workerknoten innerhalb Region, ohne den Vorteil reservierter Kapazität einzubüßen.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

Der Rabatt auf die Reservierung virtueller Kerne wird automatisch auf die Anzahl der Hyperscale (Citus)-Servergruppen angewendet, die dem Reservierungsumfang und den Attributen der Azure Database for PostgreSQL-Kapazitätsreservierung entsprechen. Sie können den Umfang der Azure Database for PostgreSQL – Hyperscale (Citus)-Kapazitätsreservierung über das Azure-Portal, PowerShell, die CLI oder die API aktualisieren.

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

* [Was sind Azure-Reservierungen?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
* [Verwalten von Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
* [Grundlegendes zum Rabatt für Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
* [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-postgresql)
* [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
* [Verkaufen Microsoft Azure Reserved Instances](https://docs.microsoft.com/partner-center/azure-reservations)
