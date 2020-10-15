---
title: Partner Center-Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace, Microsoft AppSource und dem Azure Marketplace
description: Erfahren Sie, wie Sie auf Analyseberichte in einem grafischen und herunterladbaren Format zugreifen, die Informationen zu Aufträgen für Ihre Angebote im kommerziellen Marketplace enthalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/22/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: d5adc1bfe19de48568d0e77bb488bea0e5a02818
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87327377"
---
# <a name="orders-dashboard-in-commercial-marketplace-analytics"></a>Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace

Dieser Artikel enthält Informationen zum **Dashboard „Aufträge“** im Partner Center. In diesem Dashboard werden Informationen zu Ihren Aufträgen in einem grafischen und herunterladbaren Format angezeigt.

Um auf das **Dashboard „Aufträge“** in den Partner Center-Analysetools zuzugreifen, öffnen Sie unter „Kommerzieller Marketplace“ das **[Dashboard „Analyse“](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** .

>[!NOTE]
> Ausführliche Definitionen der Analyseterminologie finden Sie unter [Häufig gestellte Fragen und Terminologie zu Analysen für den kommerziellen Marketplace](./faq-terminology.md).

## <a name="orders-dashboard"></a>Dashboard „Bestellungen“

Im Dashboard **Aufträge** des Menüs **Analyse** werden die aktuellen Aufträge für all Ihre SaaS-Angebote angezeigt. Sie können grafische Darstellungen folgender Elemente anzeigen:

- [Auftragsübersicht](#order-summary)
- [Bestellungen nach Geographie](#orders-by-geography)
- [Aufträge nach Angeboten](#orders-by-offers)
- [Auftragstrend: pro Standort/pro Arbeitsplatz im Vergleich](#orders-trend-per-site-versus-per-seat)
- [Aufträge nach Plänen](#orders-by-plans)
- [Trend im Hinblick auf Aufträge und Arbeitsplätze](#orders-and-seats-trend)
- [Tabelle der Bestelldetails](#order-details-table)

Die maximale Latenz zwischen der Auftragserstellung und der Berichterstellung in Partner Center beträgt 48 Stunden.

## <a name="order-dashboard-details"></a>Details zum Dashboard „Aufträge“

In diesem Abschnitt werden die Analyseberichte ausführlicher beschrieben.

### <a name="order-summary"></a>Auftragsübersicht

Im Abschnitt „Auftragsübersicht“ wird die Anzahl aller erworbenen Aufträge (außer stornierter Aufträge) sowie aller stornierten Aufträge und Arbeitsplätze angezeigt.

Der Prozentwert neben „Bestellungen insgesamt“ stellt den Zuwachs im ausgewählten Datumsbereich dar.

![Partner Center, Auftragsübersicht analysieren](./media/order-summary.png)

- Ein grünes, nach oben zeigendes Dreieck gibt einen positiven Wachstumstrend an.
- Ein rotes, nach unten zeigendes Dreieck gibt einen negativen Wachstumstrend gegenüber dem Vormonat an.
- Wachstumstrends werden durch Mikrobalkendiagramme dargestellt. Sie können den Wert für jeden Monat anzeigen, indem Sie auf die Spalten innerhalb des Diagramms zeigen.
- Die Anzahl stornierter Aufträge entspricht den Aufträgen, die zuvor erworben und dann während des ausgewählten Datumsbereichs storniert wurden.
- Die Anzahl von Arbeitsplätzen entspricht den Arbeitsplätzen, die während des ausgewählten Datumsbereichs erstellt wurden.

### <a name="orders-by-geography"></a>Bestellungen nach Geographie

Im Wärmebild **Aufträge nach Geografie** wird die Anzahl Ihrer Aufträge auf einer Weltkarte angezeigt. Außerdem werden die zugeordneten Arbeitsplätze basierend auf dem Land/der Region des Kunden angezeigt. Dieses Wärmebild funktioniert genauso wie das **[Wärmebild „Kunden nach Geografie“](./customer-dashboard.md#customer-by-geography)** .

![Partner Center, Aufträge nach Geografie analysieren](./media/orders-by-geography.png)

### <a name="orders-by-offers"></a>Aufträge nach Angeboten

Im Ringdiagramm **Aufträge nach Angeboten** sind Aufträge (einschließlich stornierter Aufträge) nach Angebotsnamen organisiert.

- Die Topangebote werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Angebote gruppiert sind.
- Sie können bestimmte Angebote in der Legende auswählen, um nur diese Angebote im Diagramm anzuzeigen.
- Wenn Sie mit der Maus auf ein Segment im Diagramm zeigen, wird die Anzahl der Bestellungen und der Prozentsatz dieses Angebots im Vergleich zur Gesamtanzahl der Bestellungen für alle Ihre Angebote angezeigt.
- Der **Trend „Aufträge nach Angeboten“** zeigt Wachstumstrends nach Monaten an. Die Spalte „Monat“ gibt die Anzahl der Bestellungen nach Angebotsname an. Das Liniendiagramm stellt den prozentualen Wachstumstrend auf einer z-Achse dar.
- Mit dem Schieberegler oben im Diagramm können Sie nach rechts und links entlang der x-Achse scrollen, um sich auf bestimmte Datenpunkte zu konzentrieren.
- Sie können das Trenddiagramm anzeigen, indem Sie ein bestimmtes Punkt Element in der Legende auswählen.
- Sie können auch Trends und Daten für **stornierte Bestellungen** anzeigen. Das Diagramm funktioniert genauso wie das Diagramm **Aufträge nach Angeboten**.

### <a name="orders-trend-per-site-versus-per-seat"></a>Auftragstrend: pro Standort/pro Arbeitsplatz im Vergleich

Im Ringdiagramm **Pro Standort/pro Arbeitsplatz im Vergleich** sind von Kunden erworbene SaaS-Aufträge pro Standort und pro Arbeitsplatz aufgeschlüsselt. (Dieses Diagramm schließt stornierte Aufträge ein.) Im Säulendiagramm ist der Trend der von Kunden erworbenen SaaS-Aufträge pro Standort und pro Arbeitsplatz dargestellt (dieses Diagramm umfasst stornierte Aufträge).

### <a name="orders-by-plans"></a>Aufträge nach Plänen

Im Diagramm **Aufträge nach Plänen** ist der Auftragstrend für Ihre gesamten Angebote auf Planebene dargestellt (dieses Diagramm enthält stornierte Aufträge). Im Ringdiagramm ist die Aufschlüsselung der fünf Top-Planaufträge und im Säulendiagramm der Auftragstrend für die fünf Top-Pläne dargestellt.

### <a name="orders-and-seats-trend"></a>Trend im Hinblick auf Aufträge und Arbeitsplätze

Im Diagramm **Trend im Hinblick auf Aufträge und Arbeitsplätze** werden die 50 Top-Angebote mit der höchsten Anzahl von Aufträgen dargestellt. Diese werden in einer Bestenliste angezeigt und nach der höchsten Anzahl von Aufträgen und dem höchsten Prozentsatz der Aufträge sortiert.

- **Aufträge nach Plänen**: Wählen Sie ein Angebot aus, um die Aufschlüsselung der Auftragsanzahl für die fünf Top-Pläne im Diagramm anzuzeigen.
- **Arbeitsplätze nach Plänen**: Der monatliche Trend der Arbeitsplätze für die fünf Top-Pläne. Wenn es sich bei dem ausgewählten Angebot nicht um ein Angebot pro Arbeitsplatz handelt, werden hier keine Daten angezeigt.

### <a name="canceled-orders-by-offers"></a>Stornierte Aufträge nach Angeboten

Im Kreisdiagramm **Stornierte Aufträge nach Angeboten** sind all Ihre stornierten Aufträge nach Angebotsnamen organisiert. Die Topangebote werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Angebote gruppiert sind. Sie können bestimmte Angebote in der Legende auswählen, um sie im Diagramm anzuzeigen.

- Wenn Sie mit der Maus auf ein Segment im Diagramm zeigen, werden die Anzahl der Aufträge und der Prozentsatz des ausgewählten Angebots im Vergleich zur Gesamtanzahl der Aufträge für alle Angebote angezeigt.
- Im Säulendiagramm werden Trends nach Monaten angezeigt. Die Spalten stellen die Anzahl der stornierten Aufträge nach Angebotsname dar. Mit dem Schieberegler oben im Diagramm können Sie entlang der x-Achse nach rechts und links scrollen, um sich auf bestimmte Datenpunkte zu konzentrieren. Sie können das Trenddiagramm anzeigen, indem Sie ein bestimmtes Punkt Element in der Legende auswählen.

### <a name="order-details-table"></a>Tabelle der Bestelldetails

In der Tabelle „Auftragsdetails“ wird eine nummerierte Liste der 1000 Top-Aufträge nach dem Kaufdatum sortiert angezeigt.

- Jede Spalte im Raster ist sortierbar.
- Die Daten können in eine TSV-Datei extrahiert werden, wenn die Anzahl der Datensätze weniger als 1000 beträgt.
- Liegt die Anzahl der Datensätze über 1000, werden die exportierten Daten für die nächsten 30 Tage asynchron auf einer Downloadseite abgelegt.
- Wenden Sie Filter auf die **Tabelle „Auftragsdetails“** an, um nur die Daten anzuzeigen, die für Sie von Interesse sind. Filtern Sie nach Land/Region, Azure-Lizenztyp, Lizenztyp im kommerziellen Marketplace, Angebotstyp, Bestellstatus, kostenlosen Testversionen, Abonnement-ID im kommerziellen Marketplace, Kunden-ID und Unternehmensname.
- Da für SaaS-Angebote, die über den Azure Marketplace oder AppSource erworben werden, kein Azure-Abonnement erforderlich ist, wird im Abschnitt **Daten zu detaillierten Aufträgen** die Marketplace-Abonnement-ID „00000000-0000-0000-0000-000000000000“ angezeigt.

#### <a name="orders-page-filters"></a>Filter für die Seite „Bestellungen“

Diese Filter werden auf Seitenebene angewendet.

Sie können mehrere Filter auswählen, um das Diagramm nach den Kriterien zu rendern, die Sie für die Anzeige ausgewählt haben, und um die gewünschten Daten im Raster/Export **Ausführliche Auftragsdaten** anzuzeigen. Filter werden auf die Daten angewandt, die für den in der rechten oberen Ecke der Seite „Aufträge“ ausgewählten Datenbereich extrahiert wurden.

- Angebotstypen und Angebotsnamen werden nur für Angebote aufgelistet, für die im ausgewählten Datumsbereich Aufträge vorhanden sind. In der Liste enthaltene Angebotsnamen werden für Angebotstypen angezeigt, die Sie aus der Liste ausgewählt haben.
- Durch angewendete Filter werden die Gesamtmetriken innerhalb der einzelnen Auswahl(en) für jeden ausgewählten Filter angezeigt. Bei Verwendung der Standardauswahl werden keine angewendeten Filter angezeigt.
- Wenn für eine der Dropdownlisten **Alle** ausgewählt wird, werden alle Metriken auf der ausgewählten Seite aggregiert. Beispiel: Die Option „Alle“ in der Filteroption für Angebotstypen bedeutet, dass alle Angebotstypen ausgewählt wurden. Dies ist die Standardauswahl für die Dropdownlisten. Bei Auswahl von **Alle** werden durch die angewendeten Filter keine Daten angezeigt.
- **Auswahl mehrerer Werte**: Alle Metriken auf der Seite werden für alle in der Dropdownliste getroffenen Auswahlen aggregiert. Bei einer Mehrfachauswahl wird durch den angewendeten Filter die Anzahl aller vorgenommenen Auswahlen angezeigt. Siehe dazu folgende Abbildung.

    ![Partner Center, Auftrag analysieren, während mehrere Werte auf den Filter angewendet werden](./media/filters-applied.png)

- **Auswahl eines einzelnen Werts**: Bei Auswahl eines Werts wird durch den angewendeten Filter die zum ausgewählten Filter gehörige Anzahl angezeigt. Siehe dazu folgende Abbildung.

     ![Partner Center, Auftrag analysieren, während ein einzelner Wert auf den Filter angewendet wird](./media/filters-applied-single.png)

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die im kommerziellen Marketplace in Partner Center verfügbaren Analyseberichte finden Sie unter [Analysen für den kommerziellen Marketplace in Partner Center](./analytics.md).
- Informationen zu Diagrammen, Trends und Werten von aggregierten Daten, mit deren Hilfe Marketplace-Aktivitäten für Ihr Angebot zusammengefasst werden, finden Sie unter [Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace](./summary-dashboard.md).
- Informationen zu Metriken zur Nutzung und zur getakteten Abrechnung für VM-Angebote finden Sie unter [Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace](./usage-dashboard.md).
- Ausführliche Informationen zu Ihren Kunden, einschließlich Wachstumstrends, finden Sie unter [Dashboard „Kunde“ in Analysen für den kommerziellen Marketplace](./customer-dashboard.md).
- Eine Liste Ihrer Downloadanforderungen der letzten 30 Tagen finden Sie unter [Dashboard „Downloads“ in Analysen für den kommerziellen Marketplace](./downloads-dashboard.md).
- Eine konsolidierte Ansicht des Kundenfeedbacks für Angebote im Azure Marketplace und in AppSource finden Sie unter [Dashboard „Bewertungen und Prüfungen“ in Analysen für den kommerziellen Marketplace](./ratings-reviews.md).
- Häufig gestellte Fragen zu Analysen für den kommerziellen Marketplace und ein umfassendes Wörterbuch mit Datenbegriffen finden Sie unter [Häufig gestellte Fragen und Terminologie zu Analysen für den kommerziellen Marketplace](./faq-terminology.md).
