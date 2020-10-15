---
title: Dashboard „Zusammenfassung“ für Partner Center-Analysen im kommerziellen Marketplace
description: Erfahren Sie, wie Sie über das Dashboard „Zusammenfassung“ im Partner Center auf Diagramme, Trends und Werte von aggregierten Daten zugreifen, mit deren Hilfe Marketplace-Aktivitäten zusammengefasst werden.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 8e58de7975bc7f5de1fe2ad71f97c02c901c0c21
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87304121"
---
# <a name="summary-dashboard-in-commercial-marketplace-analytics"></a>Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace

Dieser Artikel enthält Informationen zum Dashboard „Zusammenfassung“ im Partner Center. In diesem Dashboard werden Diagramme, Trends und Werte von aggregierten Daten angezeigt, mit deren Hilfe die Marketplace-Aktivitäten für Ihre Angebote zusammengefasst werden.

Um auf das Dashboard „Zusammenfassung“ zuzugreifen, öffnen Sie das **[Dashboard „Analyse“](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** unter „Kommerzieller Marketplace“.

>[!NOTE]
> Ausführliche Definitionen der Analyseterminologie finden Sie unter [Analysen für den kommerziellen Marketplace: Häufig gestellte Fragen und Terminologie](./faq-terminology.md).

## <a name="summary-dashboard"></a>Dashboard „Zusammenfassung“

Das Dashboard **Zusammenfassung** bietet eine Übersicht basierend auf den einzelnen Angebotstypen. Unter **Einblicke** erhalten Sie auf einen Blick wichtige Informationen und einen umfassenden Überblick über die Verkaufsaktivität Ihrer Angebote. Sie können diese Berichte über das Dashboard **Zusammenfassung** anzeigen. In diesem Artikel werden die folgenden Elemente ausführlicher erläutert:

- [Datumsbereich](#date-range)
- [Abschnitt „Zusammenfassung“](#summary-section)
- [Kunden nach Geographie](#customers-by-geography)
- [Wachstumstrenddiagramme](#growth-trend-charts)
- [Kundenrangliste](#customer-leaderboard)
- [Anzahl von Arbeitsplätzen – Trend](#seat-count-trend)
- [Kostenlose Saas-Testversionsaufträge – Trend](#free-trials-saas-orders-trend)

## <a name="elements-of-the-summary-dashboard"></a>Elemente des Dashboards „Zusammenfassung“

In den folgenden Abschnitten wird beschrieben, wie das Dashboard „Zusammenfassung“ verwendet und wie die Daten gelesen werden.

### <a name="date-range"></a>Datumsbereich

Oben rechts auf jeder Seite finden Sie eine Datumsbereichsauswahl. Passen Sie die Ausgabe der Diagramme auf der Seite **Zusammenfassung** an, indem Sie einen Datumsbereich basierend auf den letzten 3, 6 oder 12 Monaten oder einen benutzerdefinierten Datumsbereich mit einer maximalen Dauer von 12 Monaten auswählen. Der Standarddatumsbereich beträgt sechs Monate.

![Partner Center, Dashboard „Analyse“](./media/analyze-dashboard.png)

### <a name="summary-section"></a>Abschnitt „Zusammenfassung“

Im Abschnitt „Zusammenfassung“ wird die Anzahl aller im ausgewählten Datumsbereich erstellten Aufträge, gewonnenen Kunden und gemeldeten Nutzungsstunden angezeigt. Der Teil des aktuellen Monats wird aus der Berechnung dieser Metriken ausgeschlossen. Beispiel: Wenn Sie einen Zeitrahmen von 6 Monaten ausgewählt haben, werden die Nutzungsstunden für die sechs Monate vor dem aktuellen Monat berechnet. Wenn ein benutzerdefinierter Datumsbereich ausgewählt wurde, wird eine Teilmenge des aktuellen Monats aus der Berechnung ausgeschlossen.

![Wachstumstrends im Dashboard „Zusammenfassung“](./media/summary-summary-section.png)

#### <a name="reading-the-summary-section"></a>Lesen des Abschnitts „Zusammenfassung“

- **Aufträge**: Die Anzahl aller erworbenen Aufträge (ohne stornierte Aufträge) für Ihre bisher veröffentlichten Angebote.
- **Kunden**: Die Anzahl aller Kunden, die ihre Angebote erworben haben und über mindestens einen nicht stornierten Auftrag verfügen.
- **Normalisierte Nutzung (Stunden)** : Diese wird als Nutzungsstunden definiert, die normalisiert wurden, um die Anzahl der VM-Kerne ([Anzahl der VM-Kerne] x [Stunden der Rohdatennutzung]) zu berücksichtigen. Für VMs, die als „SHAREDCORE“ festgelegt wurden, wird der Multiplikator 1/6 (oder 0,1666) für [Anzahl der VM-Kerne] verwendet.
- **Tatsächliche Nutzung (Stunden)** : Diese wird als Zeitspanne (in Stunden) definiert, in der VMs ausgeführt wurden. Der Prozentwert neben **Aufträge gesamt**, **Kunden gesamt**, **Normalisierte Nutzung (Stunden)** , **Tatsächliche Nutzung (Stunden)** , **Seitenbesuche** und **Handlungsaufforderungen** spiegeln wider, wie stark die Nutzung im ausgewählten Datumsbereich zugenommen oder abgenommen hat ([Nutzung im letzten Monat – Nutzung im ersten Monat])/Nutzung im ersten Monat). Wie oben beschrieben, wird eine Teilmenge des aktuellen Monats aus dieser Metrik ausgeschlossen.
- **Wachstumstrends**: Wenn Sie auf die Spalten des Diagramms zeigen, wird in Balkendiagrammen der Wert für jeden Monat angezeigt.
- **Grünes Dreieck, das nach oben zeigt**: Gibt einen positiven Wachstumstrend an.
- **Rotes Dreieck, das nach unten zeigt**: Gibt einen negativen Wachstumstrend gegenüber dem Vormonat an.

### <a name="customers-by-geography"></a>Kunden nach Geographie

Die Heatmap **Kunden nach Geografie** zeigt die Kundenanzahl auf einer Weltkarte an.

![Kunden nach Geografie im Dashboard „Zusammenfassung“](./media/summary-customers-by-geography.png)

- Sie können die Karte verschieben, um den genauen Standort anzuzeigen.
- Sie können an einen bestimmten Ort zoomen.
- Das Wärmebild verfügt über ein zusätzliches Raster, um Details zur Anzahl der Kunden oder Aufträge und zu den normalisierten Nutzungsstunden an einem bestimmten Ort anzuzeigen.
- Sie können ein Land oder eine Region im Raster suchen und auswählen, um an den Ort in der Karte zu zoomen. Um wieder zur ursprünglichen Ansicht zurückzukehren, drücken Sie die Taste **Home** in der Karte.
- Ein **Neukunde** hat eines Ihrer Angebote im Laufe des Monats innerhalb des ausgewählten Datumsbereichs zum ersten Mal gekauft.

### <a name="growth-trend-charts"></a>Wachstumstrenddiagramme

Sie können Trends basierend auf dem Wachstum Ihrer **erworbenen Aufträge** (einschließlich stornierter Aufträge), **gewonnenen Kunden** (einschließlich abgewanderter Kunden) und der gemeldeten **Nutzungsstunden** anzeigen. Die Werte werden monatsweise entsprechend dem ausgewählten Datumsbereich angezeigt. Sie können diese Trends weiter analysieren, indem Sie die Links unterhalb des Diagramms auswählen. So gelangen Sie zu den Seiten **Auftrag**, **Nutzung**, **Kunde** oder **Marketplace-Erkenntnisse**.

Die Trenddiagramme **„Seitenbesuche“ und „Handlungsaufforderung“** für Marketplace-Angebote werden für Azure Marketplace und AppSource auf zwei verschiedenen Registerkarten angezeigt.

![Trenddiagramme „Seitenbesuche“ und „Handlungsaufforderungen“ im Dashboard „Zusammenfassung“](./media/summary-page-visits-and-cta.png)

Im Diagramm **Aufträge nach Angeboten** sind Ihre Aufträge gemäß dem Angebotsnamen organisiert.

Im Kreisdiagramm **Aufträge nach Vertriebskanal** werden Ihre Aufträge (einschließlich Aufträgen, die von Kunden storniert wurden) im ausgewählten Datumsbereich nach Vertriebskanal organisiert. „Vertriebskanal“ ist die Art des Lizenzvertrags, über den Kunden Azure erwerben. Es gibt „Cloud Solution Provider (CSP)“, „Enterprise“, „Enterprise über Handelspartner“, „GTM“ und „Vorausbezahlung“.

In den Kreisdiagrammen **Nutzung nach Angeboten** und **Nutzung nach Vertriebskanal** wird die Nutzung nach Top-Angeboten bzw. nach Top-Vertriebskanälen aufgeschlüsselt. Das innere Kreisdiagramm stellt die Rohdatennutzung und das äußere Kreisdiagramm die normalisierte Nutzung dar.

In den Kreisdiagrammen **Aufträge nach Marketplace-Lizenztyp** und **Nutzung nach Marketplace-Lizenztyp** werden die Aufträge und die Nutzung nach ihrem jeweiligen Lizenztyp aufgeschlüsselt. Es gibt z.B. folgende Lizenztypen:

- **Abrechnung über Azure**: Wenn Sie sich entscheiden, Ihr Angebot mit diesem Lizenztyp über Microsoft zu vertreiben, rechnet Microsoft das Angebot in Ihrem Auftrag mit dem Kunden ab. Zahlungsarten sind beispielsweise nutzungsbasierte Bezahlung, Kreditkarte oder Enterprise-Rechnungsstellung.
- **BYOL (Bring Your Own License)** : Microsoft stellt Kunden die Nutzung dieser Art von Marketplace-Angebot nicht in Rechnung. Die Nutzung wird im Marketplace als **Jetzt abrufen (kostenlos)** gelistet.
- **Free:** Microsoft stellt Kunden die Nutzung dieser Art von Marketplace-Angebot nicht in Rechnung. Die Nutzung wird im Marketplace als **Kostenlose Testversion** gelistet.
- **Microsoft als Handelspartner**: Stellt Angebote dar, die von Microsoft-Handelspartnern im Rahmen des **CSP-Programms (Cloud Solution Provider)** vertrieben werden.

### <a name="customer-leaderboard"></a>Kundenrangliste

Die 50 Kunden mit der höchsten Zahl von Bestellungen werden in einer *Rangliste* angezeigt, sortiert nach den höchsten Bestellzahlen und -prozentwerten.

- Wählen Sie einen Kunden aus, um seine Profildaten, Bestellungen nach Angebot oder Bestellungen nach Azure-Lizenztyp und Preiskanal anzuzeigen.
- Das Ringdiagramm **Angebote nach Bestellungen** zeigt die vier besten Angebote (nach Anzahl der Bestellungen) sowie die übrigen Angebote als „Sonstige“ zusammengefasst.
- Im Ringdiagramm **Normalisierte Nutzung nach Angebot** werden die fünf Top-Angebote nach Nutzung dargestellt.

> [!NOTE]
> Persönliche Kundendaten werden nur dann angezeigt, wenn der Kunde seine Einwilligung erteilt hat. Sie können diese Daten einsehen, wenn Sie sich mit der Berechtigungsebene einer **Besitzer**-Rolle angemeldet haben. Benutzer mit der Rolle **Mitwirkender** können diese Informationen nicht einsehen. [Erfahren Sie mehr über Benutzerrollen und Berechtigungen](./manage-account.md#define-user-roles-and-permissions).

### <a name="seat-count-trend"></a>Anzahl von Arbeitsplätzen – Trend

Im Diagramm **Aufträge pro Arbeitsplatz/pro Standort** werden alle erworbenen Aufträge gemäß dem Preismodell aufgeschlüsselt. Im Diagramm **Anzahl von Arbeitsplätzen – Trend** werden für all Ihre SaaS-Angebote (Software-as-a-Service) pro Arbeitsplatz die Arbeitsplätze gegenüber den erworbenen Aufträgen dargestellt.

### <a name="free-trials-saas-orders-trend"></a>Kostenlose Saas-Testversionsaufträge – Trend

Im Diagramm **Kostenlose SaaS-Testversionsaufträge – Trend** wird der Auftragstrend für kostenlose SaaS-Testversionsaufträge mit einem Testzeitraum von 30 Tagen dargestellt.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die im kommerziellen Marketplace im Partner Center verfügbaren Analyseberichte finden Sie unter [Analysen für den kommerziellen Marketplace im Partner Center](./analytics.md).
- Informationen zu Ihren Aufträgen in einem grafischen und herunterladbaren Format finden Sie unter [Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace](./orders-dashboard.md).
- Informationen zu Metriken zur Nutzung und zur getakteten Abrechnung für VM-Angebote finden Sie unter [Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace](./usage-dashboard.md).
- Ausführliche Informationen zu Ihren Kunden, einschließlich Wachstumstrends, finden Sie unter [Dashboard „Kunde“ in Analysen für den kommerziellen Marketplace](./customer-dashboard.md).
- Eine Liste Ihrer Downloadanforderungen der letzten 30 Tagen finden Sie unter [Dashboard „Downloads“ in Analysen für den kommerziellen Marketplace](./downloads-dashboard.md).
- Eine konsolidierte Ansicht des Kundenfeedbacks für Angebote im Azure Marketplace und in AppSource finden Sie unter [Dashboard „Bewertungen und Prüfungen“ in Analysen für den kommerziellen Marketplace](./ratings-reviews.md).
- Häufig gestellte Fragen zu Analysen für den kommerziellen Marketplace und ein umfassendes Wörterbuch mit Datenbegriffen finden Sie unter [Häufig gestellte Fragen und Terminologie zu Analysen für den kommerziellen Marketplace](./faq-terminology.md).
