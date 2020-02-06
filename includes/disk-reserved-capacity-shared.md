---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 89586f932ee358664a2869c87ced72594336b404
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850521"
---
Nutzen Sie reservierte Kapazität, um bei Ihrer Nutzung von SSD Premium-Datenträgern (Solid State Drive) zu sparen, und kombinieren Sie dies mit reservierten VM-Instanzen, um Ihre VM-Gesamtkosten zu senken. Der Reservierungsrabatt wird automatisch auf die entsprechenden Datenträger im ausgewählten Reservierungsbereich angewendet. Es ist nicht erforderlich, eine Reservierung einem verwalteten Datenträger zuzuweisen, um in den Genuss der Rabatte zu kommen. Rabatte werden stündlich auf die Datenträgernutzung angewendet. Ungenutzte reservierte Kapazität verfällt. Der Reservierungsrabatt für verwaltete Datenträger gilt nicht für nicht verwaltete Datenträger, Ultra Disks oder die Nutzung von Seitenblobs.

## <a name="determine-your-storage-needs"></a>Ermitteln Ihrer Speicheranforderungen

Bevor Sie eine Reservierung erwerben, sollten Sie Ihre Speicheranforderungen ermitteln. Derzeit steht die Datenträgerreservierung nur für ausgewählte SSD Premium-SKUs zur Verfügung. Die SKU eines SSD Premium-Datenträgers bestimmt die Größe und Leistung des Datenträgers. Bei der Ermittlung Ihrer Speicheranforderungen empfiehlt es sich, Datenträger nicht nur als Gesamtkapazität zu betrachten. Eine Reservierung für einen größeren Datenträger (beispielsweise für einen P40-Datenträger) kann nicht für die Bezahlung zweier kleinerer Datenträger (P30) verwendet werden. Wenn Sie eine Reservierung erwerben, erwerben Sie nur die Gesamtzahl von Datenträgern pro SKU.

Die Datenträgerreservierung erfolgt auf der Grundlage der Datenträger-SKU. Daher basiert die Reservierungsnutzung auf der Einheit der Datenträger-SKUs und nicht auf der bereitgestellten Größe. Wenn Sie also beispielsweise einen P40-Datenträger mit einer bereitgestellten Kapazität von 2 TiB reserviert, aber nur zwei P30-Datenträger zugeordnet haben, wird die P30-Nutzung nicht mit der P40-Reservierung verrechnet, und es fällt der Preis für die nutzungsbasierte Bezahlung an.



[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="purchase-considerations"></a>Überlegungen zum Kauf

Im Anschluss finden Sie einige Best Practices im Zusammenhang mit dem Erwerb von Datenträgerreservierungen:

- Analysieren Sie Ihre Nutzungsinformationen, um besser ermitteln zu können, welche Reservierungen Sie erwerben sollten. Verfolgen Sie die Nutzung in Datenträger-SKUs und nicht in bereitgestellter oder verwendeter Datenträgerkapazität. 
- Untersuchen Sie Ihre Datenträgerreservierung zusammen mit Ihrer VM-Reservierung. Wir empfehlen dringend, VM-Reservierungen mit Reservierungen für die Datenträgernutzung zu kombinieren, um möglichst viel zu sparen. Sie können zuerst die passende VM-Reservierung und anschließend eine passende Datenträgerreservierung ermitteln. In der Regel haben Sie eine Standardkonfiguration für jede Ihrer Workloads. Eine SQL Server-Instanz kann beispielsweise über zwei P40-Datenträger und einen P30-Betriebssystemdatenträger verfügen. Dieses Muster kann ggf. bei der Bestimmung der zu erwerbenden Reservierungsmenge hilfreich sein. Diese Vorgehensweise vereinfacht den Evaluierungsprozess und stellt außerdem sicher, dass Sie über einen Tarif verfügen, der in puncto Abonnements, Regionen und Ähnlichem sowohl auf den virtuellen Computer als auch auf die Datenträger abgestimmt ist. 

## <a name="purchase-restrictions"></a>Einschränkungen beim Kauf

Für folgende Datenträger stehen derzeit keine Reservierungsrabatte zur Verfügung:
- Nicht verwaltete Datenträger/Seitenblobs
- SSD Standard- oder HDD Standard-Datenträger
- SSD Premium-SKUs kleiner als P30 (P1/P2/P3/P4/P6/P10/P15/P20)
- Clouds: In den Regionen „Azure Gov“, „Deutschland“ und „China“ können keine Reservierungen erworben werden.
- Kapazitätsbeschränkungen: In seltenen Fällen beschränkt Azure den Erwerb neuer Reservierungen für einen Teil der Datenträger-SKUs aufgrund von geringer Kapazität in einer Region.

## <a name="buy-a-disk-reservation"></a>Kaufen einer Datenträgerreservierung

Azure-Datenträgerreservierungen können über das [Azure-Portal](https://portal.azure.com/) erworben werden. Sie können die Reservierung entweder im Voraus oder monatlich bezahlen. Weitere Informationen zum Erwerb mit monatlicher Zahlung finden Sie unter [Erwerben von Reservierungen mit monatlicher Zahlung](../articles/cost-management-billing/reservations/monthly-payments-reservations.md).

Gehen Sie folgendermaßen vor, um reservierte Kapazität zu erwerben:

1. Navigieren Sie im Azure-Portal zum Blatt [Reservierungen erwerben](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand).
1. Wählen Sie **Azure Managed Disks** aus, um eine Reservierung zu erwerben.

    ![disks-reserved-purchase-reservation.png](media/disks-reserved-capacity/disks-reserved-purchase-reservation.png)    

1. Füllen Sie die Pflichtfelder aus, wie in der folgenden Tabelle beschrieben:

   |Feld  |Beschreibung  |
   |---------|---------|
   |**Umfang**   |  Zeigt an, wie viele Abonnements die mit der Reservierung verbundenen Abrechnungsvorteile nutzen können. Sie steuert auch, wie die Reservierung auf bestimmte Abonnements angewendet wird. <br/><br/> Wenn Sie **Gemeinsam** auswählen, gilt der Reservierungsrabatt für die Azure Storage-Kapazität in einem beliebigen Abonnement innerhalb des Abrechnungskontexts. Der Abrechnungskontext basiert darauf, wie Sie sich für Azure registriert haben. Für Enterprise-Kunden stellt der freigegebene Bereich die Registrierung dar und umfasst alle Abonnements in der Registrierung. Für Kunden mit nutzungsbasierter Bezahlung umfasst der freigegebene Umfang alle Abonnements mit Preisen für nutzungsbasierte Bezahlung, die vom Kontoadministrator erstellt wurden.  <br/><br/>  Wenn Sie **Einzelabonnement** auswählen, gilt der Reservierungsrabatt für die Azure Storage-Kapazität im ausgewählten Abonnement. <br/><br/> Wenn Sie die Option **Einzelne Ressourcengruppe** wählen, gilt der Reservierungsrabatt für die Azure Storage-Kapazität im ausgewählten Abonnement und in der ausgewählten Ressourcengruppe in diesem Abonnement. <br/><br/> Sie können den Reservierungsumfang nach dem Erwerb der Reservierung ändern.  |
   |**Abonnement**  | Das Abonnement, das für die Bezahlung der Azure Storage-Reservierung verwendet wird. Die Zahlungsmethode für das ausgewählte Abonnement wird für das Inrechnungstellen der Kosten verwendet. Es muss einer der folgenden Abonnementtypen vorliegen: <br/><br/>  Enterprise Agreement (Angebotsnummer: MS-AZR-0017P oder MS-AZR-0148P): Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. <br/><br/> Einzelnes Abonnement mit Preisen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P): Bei einem individuellen Abonnement mit Preisen für nutzungsbasierte Bezahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für Zahlung auf Rechnung berechnet.    |
   | **Datenträger** | Die SKU, die Sie erstellen möchten. |
   | **Region** | Die Region, in der die Reservierung wirksam ist. |
   | **Fakturierungsintervall** | Gibt an, wie oft das Konto für die Reservierung belastet wird. Die Optionen umfassen *Monatlich* oder *Im Voraus*. |

    ![premium-ssd-reserved-purchase-selection.png](media/disks-reserved-capacity/premium-ssd-reserved-purchase-selection.png)

1. Nach Auswahl der Parameter für die Reservierung werden die Kosten im Azure-Portal angezeigt. Das Portal zeigt auch den prozentualen Rabatt im Vergleich zur nutzungsbasierten Zahlung an. Wählen Sie **Weiter** aus, um zum Blatt **Reservierungen erwerben** zu gelangen.

1. Im Bereich **Reservierungen erwerben** können Sie Ihre Reservierung benennen und die gewünschte Gesamtmenge der Reservierungen auswählen. Die Anzahl von Reservierungen entspricht der Anzahl von Datenträgern. Wenn Sie also beispielsweise hundert Datenträger reservieren möchten, legen Sie **Menge** auf „100“ fest.
1. Überprüfen Sie die Gesamtkosten der Reservierung.

    ![premium-ssd-reserved-selecting-sku-total-purchase.png](media/disks-reserved-capacity/premium-ssd-reserved-selecting-sku-total-purchase.png)

Nach dem Erwerb einer Reservierung wird sie automatisch auf alle vorhandenen Azure Disk Storage-Ressourcen angewendet, die den Reservierungsbedingungen entsprechen. Sollten Sie noch keine Azure Disk Storage-Ressourcen erstellt haben, wird die Reservierung angewendet, wenn Sie eine Ressource erstellen, die den Reservierungsbedingungen entspricht. In beiden Fällen gelten die Reservierungsbedingungen sofort nach dem erfolgreichen Erwerb.

## <a name="exchange-or-refund-a-reservation"></a>Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können mit bestimmten Einschränkungen umgetauscht oder rückerstattet werden.

Navigieren Sie zum Umtauschen oder Rückerstatten von Reservierungen zu den Reservierungsdetails im Azure-Portal. Wählen Sie **Umtauschen** oder „Rückerstatten“ aus und befolgen Sie die Anweisungen, um eine Supportanfrage zu übermitteln. Nachdem die Anfrage verarbeitet wurde, erhalten Sie von Microsoft eine E-Mail, die die Erledigung der Anfrage bestätigt.

Weitere Informationen zu Azure-Reservierungsrichtlinien finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](../articles/cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

### <a name="exchange-a-reservation"></a>Umtauschen einer Reservierung

Durch Umtausch einer Reservierung können Sie eine anteilige Rückerstattung für den nicht genutzten Teil der Reservierung erhalten. Sie können die Rückerstattung dann auf den Kaufpreis einer neuen Azure-Datenträgerreservierung anwenden.
Die Anzahl der möglichen Umtausche ist nach oben hin nicht begrenzt. Zudem ist der Umtausch gebührenfrei. Der Wert der neu erworbenen Reservierung muss größer-gleich der anteiligen Gutschrift der ursprünglichen Reservierung sein. Eine Azure-Datenträgerreservierung kann nur gegen eine andere Azure-Datenträgerreservierung und nicht gegen eine Reservierung eines anderen Azure-Diensts umgetauscht werden.

### <a name="refund-a-reservation"></a>Rückerstatten einer Reservierung

Azure-Datenträgerreservierungen können jederzeit storniert werden. Im Falle einer Stornierung erhalten Sie eine anteilige Rückerstattung basierend auf der Restlaufzeit der Reservierung abzüglich einer Gebühr für vorzeitige Kündigung in Höhe von 12 Prozent. Die maximale Erstattung pro Jahr beträgt 50.000 USD.
Bei der Stornierung einer Reservierung wird die Reservierung sofort beendet und die verbleibenden Monate werden an Microsoft zurückgegeben. Der verbleibende anteilige Saldo wird in der ursprünglichen Kaufform erstattet, abzüglich der Gebühr.

## <a name="expiration-of-a-reservation"></a>Ablauf einer Reservierung

Wenn eine Reservierung abläuft, wird jegliche Azure-Datenträgerkapazität, die Sie unter dieser Reservierung nutzen, mit dem Preis für nutzungsbasierte Bezahlung abgerechnet. Reservierungen werden nicht automatisch verlängert.
Sie erhalten 30 Tage vor dem Ablauf der Reservierung und am Ablaufdatum eine E-Mail-Benachrichtigung. Um die Vorteile der Kosteneinsparungen einer Reservierung weiter zu nutzen, muss die Reservierung spätestens am Ablaufdatum verlängert werden.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Was sind Azure-Reservierungen?](../articles/cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Grundlegendes zur Anwendung Ihres Reservierungsrabatts auf Azure Disk Storage](../articles/cost-management-billing/reservations/understand-disk-reservations.md)