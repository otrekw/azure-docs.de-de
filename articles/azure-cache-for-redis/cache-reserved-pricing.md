---
title: Vorauszahlung für Compute mit reservierter Kapazität – Azure Cache for Redis
description: Vorauszahlung für Azure Cache for Redis-Computeressourcen mit reservierter Kapazität
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: aded023c9f4c045f612e33d32c1e3ac71afddf02
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77530207"
---
# <a name="prepay-for-azure-cache-for-redis-compute-resources-with-reserved-capacity"></a>Vorauszahlung für Azure Cache for Redis-Computeressourcen mit reservierter Kapazität

Mit Azure Cache for Redis können Sie jetzt Geld im Vergleich zur nutzungsbasierten Bezahlung sparen, indem Sie Computeressourcen im Voraus bezahlen. Mit reservierten Azure Cache for Redis-Kapazitäten leisten Sie eine Vorauszahlung für Cache für einen Zeitraum von einem oder drei Jahren und erhalten dafür einen signifikanten Rabatt auf die Computekosten. Um reservierte Azure Cache for Redis-Kapazität zu erwerben, müssen Sie die Azure-Region, die Dienstebene und die Laufzeit angeben.

Sie müssen die Reservierung keinen bestimmten Azure Cache for Redis-Instanzen zuweisen. Azure Cache for Redis-Instanzen, die bereits ausgeführt oder neu bereitgestellt werden, profitieren automatisch bis zur reservierten Cachegröße von dem Reservierungspreisvorteil. Beim Kauf einer Reservierung bezahlen Sie im Voraus die Computekosten für einen Zeitraum von einem oder drei Jahren. Sobald Sie eine Reservierung gekauft haben, werden die Azure Cache for Redis-Computegebühren, die den Reservierungsattributen entsprechen, nicht mehr zu den Preisen der nutzungsbasierten Bezahlung abgerechnet. Eine Reservierung deckt keine mit dem Cache verbundenen Netzwerk- oder Speichergebühren ab. Nach Ablauf der Reservierungslaufzeit erlischt der Abrechnungsvorteil, und Azure Cache for Redis wird mit den Preisen für die nutzungsbasierte Bezahlung in Rechnung gestellt. Reservierungen werden nicht automatisch verlängert. Weitere Informationen zu den Preisen finden Sie unter [Angebot für reservierte Azure Cache for Redis-Kapazität](https://azure.microsoft.com/pricing/details/cache).

Sie können reservierte Azure Cache for Redis-Kapazität über das [Azure-Portal](https://portal.azure.com/) erwerben. So erwerben Sie reservierte Kapazität:

* Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein individuelles Abonnement mit nutzungsbasierter Bezahlung zugeordnet sein.
* Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com/) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Wenn diese Einstellung deaktiviert ist, müssen Sie ein EA-Administrator für das Abonnement sein.
* Für das Cloud Solution Provider-Programm (CSP) können nur die Administrator- oder Vertriebs-Agents reservierte Azure Cache for Redis-Kapazität kaufen.

Einzelheiten zur Berechnung der Reservierung von Kapazitäten für Unternehmenskunden und Kunden mit nutzungsbasierter Bezahlung finden Sie unter [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) und [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).


## <a name="determine-the-right-cache-size-before-purchase"></a>Bestimmen der passenden Cachegröße vor dem Kauf

Die Größe der Reservierung sollte auf der Gesamtmenge der Computeressourcen basieren, die von dem bereits vorhandenen oder in Kürze bereitzustellenden Cache innerhalb einer bestimmten Region genutzt werden und die gleiche Dienstebene verwenden.

Nehmen wir beispielsweise an, Sie führen einen universellen Gen5-Cache mit 32 virtuellen Kernen sowie zwei arbeitsspeicheroptimierte Gen5-Caches mit 16  virtuellen Kernen aus. Außerdem möchten Sie innerhalb des nächsten Monats einen weiteren universellen Gen5-Datenbankserver mit 32 virtuellen Kernen sowie einen arbeitsspeicheroptimierten Gen5-Datenbankserver mit 16 virtuellen Kernen bereitstellen. Sie wissen außerdem, dass Sie diese Ressourcen mindestens 1 Jahr benötigen. In diesem Fall sollten Sie Folgendes erwerben: eine 1-Jahres-Reservierung für eine universelle Gen5-Einzeldatenbank mit 64 virtuellen Kernen (2 × 32) sowie eine 1-Jahres-Reservierung für eine arbeitsspeicheroptimierte Gen5-Einzeldatenbank mit 48 virtuellen Kernen (2 × 16 + 16).


## <a name="buy-azure-cache-for-redis-reserved-capacity"></a>Kaufen von reservierter Azure Cache for Redis-Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie **Hinzufügen** und dann im Bereich „Reservierungen erwerben“ die Option **Azure Cache for Redis** aus, um eine neue Reservierung für Ihre Caches zu kaufen.
4. Füllen Sie die Pflichtfelder aus. Bei vorhandenen oder neuen Datenbanken, die den von Ihnen ausgewählten Attributen entsprechen, wird der Rabatt auf reservierte Kapazitäten angewendet. Die tatsächliche Anzahl Ihrer Azure Cache for Redis-Instanzen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und der ausgewählten Menge ab.


![Übersicht über Reservierungspreise](media/cache-reserved-pricing/cache-reserved-price.png)


In der folgenden Tabelle werden die erforderlichen Felder beschrieben.

| Feld | Beschreibung |
| :------------ | :------- |
| Subscription   | Das Abonnement, das für die Zahlung der Reservierung von Azure Cache for Redis-Kapazitäten verwendet wird. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung von Azure Cache for Redis-Kapazitäten belastet. Der Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder eine einzelne Vereinbarung mit Preisen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) sein. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem individuellen Abonnement mit Preisen für nutzungsbasierte Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für Zahlung auf Rechnung für das Abonnement in Rechnung gestellt.
| `Scope` | Der Bereich der Reservierung kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: </br></br> **Gemeinsam** – Der Reservierungsrabatt wird auf Azure Cache for Redis-Instanzen angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Für Enterprise-Kunden stellt der freigegebene Bereich die Registrierung dar und umfasst alle Abonnements in der Registrierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</br></br> **Einzelabonnement** – Der Reservierungsrabatt wird auf Azure Cache for Redis-Instanzen in diesem Abonnement angewendet. </br></br> **Einzelne Ressourcengruppe**: Der Reservierungsrabatt wird auf Azure Cache for Redis-Instanzen im ausgewählten Abonnement und die ausgewählte Ressourcengruppe in diesem Abonnement angewendet.
| Region | Die Azure-Region, die durch die Reservierung von Azure Cache for Redis-Kapazitäten abgedeckt wird.
| Tarif | Die Dienstebene für die Azure Cache for Redis-Server.
| Begriff | Ein Jahr oder drei Jahre.
| Menge | Die Menge an Computeressourcen, die im Rahmen der Azure Cache for Redis-Kapazitätsreservierung erworben werden. Die Menge entspricht einer Anzahl von Caches in der ausgewählten Azure-Region und Dienstebene, die reserviert werden und den Abrechnungsrabatt erhalten. Wenn Sie beispielsweise Azure Cache for Redis-Server mit der Gesamtcomputekapazität von 26 GB in der Region „USA, Osten“ ausführen oder deren Ausführung beabsichtigen, geben Sie als Menge „26“ an, um den Vorteil für alle Caches zu maximieren.

## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können unter bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="cache-size-flexibility"></a>Flexibilität der Cachegröße

Die Flexibilität der Cachegröße ermöglicht ein zentrales Hoch- oder Herunterskalieren innerhalb einer Dienstebene und Region, ohne den Vorteil reservierter Kapazität einzubüßen.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

Der Reservierungsrabatt wird automatisch auf die Azure Cache for Redis-Instanzen angewendet, die dem Reservierungsbereich und den Reservierungsattributen entsprechen. Sie können den Bereich der Reservierung über das Azure-Portal, PowerShell, die Azure CLI oder die API aktualisieren.

*  Informationen dazu, wie Kapazitätsrabatte auf Azure Cache for Redis angewendet werden, finden Sie unter [Grundlegendes zum Rabatt für Azure-Reservierungen](../cost-management-billing/reservations/understand-azure-cache-for-redis-reservation-charges.md).

* Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

    * [Was sind Azure-Reservierungen?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
    * [Verwalten von Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
    * [Grundlegendes zum Rabatt für Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
    * [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-mysql)
    * [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
    * [Verkaufen Microsoft Azure Reserved Instances](https://docs.microsoft.com/partner-center/azure-reservations)

