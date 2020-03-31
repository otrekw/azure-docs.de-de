---
title: Partner Center-Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace
description: Erfahren Sie, wie Sie auf Analyseberichte in einem grafischen und herunterladbaren Format zugreifen, die Informationen zu Aufträgen für Ihre Marketplace-Angebote enthalten.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: d0db6553a774a69bb8a55538cbd2b4a333be9316
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281407"
---
# <a name="orders-dashboard-in-commercial-marketplace-analytics"></a>Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace

Dieser Artikel enthält Informationen zum **Dashboard „Aufträge“** im Partner Center. In diesem Dashboard werden Informationen zu Ihren Aufträgen in einem grafischen und herunterladbaren Format angezeigt.

Um auf das **Dashboard „Aufträge“** in den Partner Center-Analysetools zuzugreifen, öffnen Sie unter „Kommerzieller Marketplace“ das **[Dashboard „Analyse“](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** .

>[!NOTE]
> Ausführliche Definitionen der Analyseterminologie finden Sie unter [Analysen für den kommerziellen Marketplace: Häufig gestellte Fragen und Terminologie](./faq-terminology.md).

## <a name="orders-dashboard"></a>Dashboard „Bestellungen“

Im Dashboard **Aufträge** des Menüs **Analyse** werden die aktuellen Aufträge für all Ihre SaaS-Angebote angezeigt. Sie können grafische Darstellungen folgender Elemente anzeigen:

- [Auftragsübersicht](#order-summary)
- [Bestellungen nach Geographie](#orders-by-geography)
- [Aufträge nach Angeboten](#orders-by-offers)
- [Auftragstrend: pro Standort/pro Arbeitsplatz im Vergleich](#orders-trend-per-site-versus-per-seat)
- [Aufträge nach SKUs](#orders-by-skus)
- [Trend im Hinblick auf Aufträge und Arbeitsplätze](#orders-and-seats-trend)
- [Tabelle der Bestelldetails](#order-details-table)

> [!NOTE]
> Es gibt Unterschiede zwischen der Anzeige von Analyseberichten im Cloud Partner Portal (CPP) und dem neuen Programm „Kommerzieller Marketplace“ im Partner Center. Eine Besonderheit besteht darin, dass **Verkäufer-Insights** in CPP über eine Registerkarte **Aufträge und Nutzung** verfügt, auf der Daten für nutzungsbasierte Angebote und nicht nutzungsbasierte Angebote angezeigt werden. Im Partner Center verfügt die Seite **Bestellungen** über eine separate Registerkarte für SaaS-Angebote.

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

Im Wärmebild **Aufträge nach Geografie** wird die Anzahl Ihrer Aufträge auf einer Weltkarte angezeigt. Außerdem werden die zugeordneten Arbeitsplätze basierend auf dem Land des Kunden angezeigt. Dieses Wärmebild funktioniert genauso wie das **[Wärmebild „Kunden nach Geografie“](./customer-dashboard.md#customer-by-geography)** .

![Partner Center, Aufträge nach Geografie analysieren](./media/orders-by-geography.png)

### <a name="orders-by-offers"></a>Aufträge nach Angeboten

Im Ringdiagramm **Aufträge nach Angeboten** sind Aufträge (einschließlich stornierter Aufträge) nach Angebotsnamen organisiert.

- Die Top-Angebote werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Angebote gruppiert sind.
- Sie können bestimmte Angebote in der Legende auswählen, um nur diese Angebote im Diagramm anzuzeigen.
- Wenn Sie den Cursor über ein Segment im Diagramm bewegen, wird die Anzahl der Bestellungen und der Prozentsatz dieses Angebots im Vergleich zur Gesamtzahl der Bestellungen für alle Ihre Angebote angezeigt.
- Der **Trend „Aufträge nach Angeboten“** zeigt Wachstumstrends nach Monaten an. Die Spalte „Monat“ gibt die Anzahl der Bestellungen nach Angebotsname an. Das Liniendiagramm stellt den prozentualen Wachstumstrend auf einer z-Achse dar.
- Mit dem Schieberegler oben im Diagramm können Sie nach rechts und links entlang der x-Achse scrollen, um sich auf bestimmte Datenpunkte zu konzentrieren.
- Sie können das Trenddiagramm anzeigen, indem Sie ein bestimmtes Punkt Element in der Legende auswählen.
- Sie können auch Trends und Daten für **stornierte Bestellungen** anzeigen. Das Diagramm funktioniert genauso wie das Diagramm **Aufträge nach Angeboten**.

### <a name="orders-trend-per-site-versus-per-seat"></a>Auftragstrend: pro Standort/pro Arbeitsplatz im Vergleich

Im Ringdiagramm **Pro Standort/pro Arbeitsplatz im Vergleich** sind von Kunden erworbene SaaS-Aufträge pro Standort und pro Arbeitsplatz aufgeschlüsselt (dieses Diagramm umfasst stornierte Aufträge). Im Säulendiagramm ist der Trend der von Kunden erworbenen SaaS-Aufträge pro Standort und pro Arbeitsplatz dargestellt (dieses Diagramm umfasst stornierte Aufträge).

### <a name="orders-by-skus"></a>Aufträge nach SKUs

Im Diagramm **Aufträge nach SKUs** ist der Auftragstrend auf für all Ihre Angebote auf SKU-Ebene (Stock Keeping Unit) dargestellt (dieses Diagramm umfasst stornierte Aufträge). Im Ringdiagramm ist die Aufschlüsselung der fünf Top-SKU-Aufträge und im Säulendiagramm der Auftragstrend für die fünf Top-SKUs dargestellt.

### <a name="orders-and-seats-trend"></a>Trend im Hinblick auf Aufträge und Arbeitsplätze

Im Diagramm **Trend im Hinblick auf Aufträge und Arbeitsplätze** werden die 50 Top-Angebote mit der höchsten Anzahl von Aufträgen dargestellt. Diese werden in einer Bestenliste angezeigt und sind nach der höchsten Anzahl von Aufträgen und dem höchsten Prozentsatz der Aufträge sortiert.

- **Aufträge nach SKUs**: Wählen Sie ein Angebot aus, um die Aufschlüsselung der Auftragsanzahl für die fünf Top-SKUs im Diagramm anzuzeigen.
- **Arbeitsplätze nach SKUs**: Der monatliche Trend der Arbeitsplätze für die fünf Top-SKUs. Wenn es sich bei dem ausgewählten Angebot nicht um ein Angebot pro Arbeitsplatz handelt, werden in diesem Flächendiagramm keine Daten angezeigt.

### <a name="canceled-orders-by-offers"></a>Stornierte Aufträge nach Angeboten

Im Kreisdiagramm **Stornierte Aufträge nach Angeboten** sind all Ihre stornierten Aufträge nach Angebotsnamen organisiert. Die Top-Angebote werden im Diagramm angezeigt, während die übrigen unter der Kategorie für sonstige Angebote gruppiert sind. Sie können bestimmte Angebote in der Legende auswählen, um sie im Diagramm anzuzeigen.

- Wenn Sie auf ein Segment im Diagramm zeigen, werden die Anzahl der Aufträge und der Prozentsatz des ausgewählten Angebots im Vergleich zur Gesamtanzahl der Aufträge für alle Angebote angezeigt.
- Im Säulendiagramm werden Trends nach Monaten angezeigt. Die Spalten stellen die Anzahl der stornierten Aufträge nach Angebotsname dar. Mit dem Schieberegler oben im Diagramm können Sie entlang der x-Achse nach rechts und links scrollen, um sich auf bestimmte Datenpunkte zu konzentrieren. Sie können das Trenddiagramm anzeigen, indem Sie ein bestimmtes Punkt Element in der Legende auswählen.

### <a name="order-details-table"></a>Tabelle der Bestelldetails

In der Tabelle „Auftragsdetails“ wird eine nummerierte Liste der 1000 Top-Aufträge nach dem Kaufdatum sortiert angezeigt.

- Jede Spalte im Raster ist sortierbar.
- Die Daten können in eine TSV-Datei extrahiert werden, wenn die Anzahl der Datensätze weniger als 1000 beträgt.
- Liegt die Anzahl der Datensätze über 1000, werden die exportierten Daten für die nächsten 30 Tage asynchron auf einer Downloadseite abgelegt.
- Sie können auf die **Tabelle „Auftragsdetails“** Filter anwenden, um nur die Daten anzuzeigen, die für Sie von Interesse sind. Daten können nach Land, Azure-Lizenztyp, Marketplace-Lizenztyp, Angebotstyp, Bestellstatus, kostenlosen Testversionen, Marketplace-Abonnement-ID, Kunden-ID und Firmenname gefiltert werden.

#### <a name="orders-page-filters"></a>Filter für die Seite „Bestellungen“

Diese Filter werden auf Seitenebene angewendet.

Sie können mehrere Filter auswählen, um das Diagramm nach den Kriterien zu rendern, die Sie für die Anzeige ausgewählt haben, und um die gewünschten Daten im Raster/Export **Ausführliche Auftragsdaten** anzuzeigen. Filter werden auf die Daten angewendet, die für den in der rechten oberen Ecke der Seite „Bestellungen“ ausgewählten Datenbereich extrahiert wurden.

- Angebotstypen und Angebotsnamen werden nur für Angebote aufgelistet, für die im ausgewählten Datumsbereich Aufträge vorhanden sind. In der Liste enthaltene Angebotsnamen werden für Angebotstypen angezeigt, die Sie aus der Liste ausgewählt haben.
- Durch angewendete Filter werden die Gesamtmetriken innerhalb der einzelnen Auswahl(en) für jeden ausgewählten Filter angezeigt. Bei Verwendung der Standardauswahl werden keine angewendeten Filter angezeigt.
- Wenn für eine der Dropdownlisten **Alle** ausgewählt wird, werden alle Metriken auf der ausgewählten Seite aggregiert. Beispiel: Die Option „Alle“ in der Filteroption für Angebotstypen" bedeutet, dass alle Angebotstypen ausgewählt wurden. Dies ist die Standardauswahl für die Dropdownlisten. Bei Auswahl von **Alle** werden durch die angewendeten Filter keine Daten angezeigt.
- **Auswahl mehrerer Werte**: Alle Metriken auf der Seite werden für alle in der Dropdownliste getroffenen Auswahlen aggregiert. Bei einer Mehrfachauswahl wird durch den angewendeten Filter die Anzahl aller vorgenommenen Auswahlen angezeigt. Siehe dazu folgende Abbildung.

    ![Partner Center, Auftrag analysieren, während mehrere Werte auf den Filter angewendet werden](./media/filters-applied.png)

- **Auswahl eines einzelnen Werts**: Bei Auswahl eines Werts wird durch den angewendeten Filter die zum ausgewählten Filter gehörige Anzahl angezeigt. Siehe dazu folgende Abbildung.

     ![Partner Center, Auftrag analysieren, während ein einzelner Wert auf den Filter angewendet wird](./media/filters-applied-single.png)

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die im kommerziellen Marketplace im Partner Center verfügbaren Analyseberichte finden Sie unter [Analysen für den kommerziellen Marketplace im Partner Center](./analytics.md).
- Informationen zu Diagrammen, Trends und Werten von aggregierten Daten, mit deren Hilfe Marketplace-Aktivitäten für Ihr Angebot zusammengefasst werden, finden Sie unter [Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace](./summary-dashboard.md).
- Informationen zu Metriken zur Nutzung und zur getakteten Abrechnung für VM-Angebote finden Sie unter [Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace](./usage-dashboard.md).
- Ausführliche Informationen zu Ihren Kunden, einschließlich Wachstumstrends, finden Sie unter [Dashboard „Kunde“ in Analysen für den kommerziellen Marketplace](./customer-dashboard.md).
- Eine Liste Ihrer Downloadanforderungen in den letzten 30 Tagen finden Sie unter [Dashboard „Downloads“ in Analysen für den kommerziellen Marketplace](./downloads-dashboard.md).
- Eine konsolidierte Ansicht des Kundenfeedbacks für Angebote in Azure Marketplace und AppSource finden Sie unter [Dashboard „Bewertungen und Prüfungen“ in Analysen für den kommerziellen Marketplace](./ratings-reviews.md).
- Häufig gestellte Fragen zu Analysen für den kommerziellen Marketplace und ein umfassendes Wörterbuch mit Datenbegriffen finden Sie unter [Analysen für den kommerziellen Marketplace: Häufig gestellte Fragen und Terminologie](./faq-terminology.md).
