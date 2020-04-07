---
title: Anzeigen und Herunterladen Ihrer Azure-Rechnung
description: Hier erfahren Sie, wie Sie Ihre Azure-Rechnung anzeigen und herunterladen.
keywords: Rechnung, Rechnungsdownload, Azure-Rechnung, Azure-Nutzung
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: banders
ms.openlocfilehash: 16534343a831f0802a60a9214f567742153360e0
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478997"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Anzeigen und Herunterladen der Microsoft Azure-Rechnung

Sie können Ihre Rechnung über das [Azure-Portal](https://portal.azure.com/) herunterladen oder per E-Mail erhalten. Azure-Kunden mit einem Enterprise Agreement (EA-Kunden) können die Rechnung ihrer Organisation nicht herunterladen. Stattdessen werden Rechnungen an die Person gesendet, die für den Empfang von Rechnungen für die Registrierung festgelegt wurde.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="when-invoices-are-generated"></a>Zeitpunkt der Rechnungsgenerierung

Die Rechnungsgenerierung basiert auf der Art Ihres Abrechnungskontos. Rechnungen werden für Abrechnungskonten vom Typ „Microsoft Online Services-Programm“ (MOSP), „Microsoft-Kundenvereinbarung“ (Microsoft Customer Agreement, MCA) und „Microsoft Partner-Vereinbarung“ (Microsoft Partner Agreement, MPA) erstellt. Rechnungen werden auch für EA-Abrechnungskonten (Enterprise Agreement) generiert. Allerdings werden Rechnungen für EA-Abrechnungskonten nicht im Azure-Portal angezeigt.

Weitere Informationen zu Abrechnungskonten und zur Ermittlung der Art Ihres Abrechnungskontos finden Sie unter [Anzeigen Ihrer Abrechnungskonten im Azure-Portal](../manage/view-all-accounts.md).

## <a name="invoices-for-mosp-billing-accounts"></a>Rechnungen für MOSP-Abrechnungskonten

Ein MOSP-Abrechnungskonto wird erstellt, wenn Sie sich über die Azure-Website für Azure registrieren. Beispiele hierfür sind die Registrierung für ein [Kostenloses Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/), ein [Angebot mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) oder als [Visual Studio-Abonnent](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

Kunden in ausgewählten Regionen, die sich über die Azure-Website für ein [Angebot mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) oder ein [kostenloses Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/) registrieren, verfügen möglicherweise über ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung.

Sollten Sie sich hinsichtlich der Art Ihres Abrechnungskontos nicht sicher sein, lesen Sie den [Abschnitt zum Überprüfen des Typs Ihres Abrechnungskontos](../manage/view-all-accounts.md#check-the-type-of-your-account), bevor Sie mit der Anleitung in diesem Artikel fortfahren. 

Ein MOSP-Abrechnungskonto kann über folgende Rechnungen verfügen:

**Gebühren für Azure-Dienste:** Eine Rechnung wird für jedes Azure-Abonnement mit Azure-Ressourcen generiert, die durch das Abonnement verwendet werden. Die Rechnung enthält Gebühren für einen Abrechnungszeitraum. Der Abrechnungszeitraum wird durch den Tag des Monats bestimmt, an dem das Abonnement erstellt wurde.

Ein Beispiel: John erstellt *Azure-Abonnement 01* am 5. März und *Azure-Abonnement 02* am 10. März. Die Rechnung für *Azure-Abonnement 01* enthält Gebühren, die zwischen dem fünften Tag eines Monats und dem vierten Tag des Folgemonats angefallen sind. Die Rechnung für *Azure-Abonnement 02* enthält Gebühren, die zwischen dem zehnten Tag eines Monats und dem neunten Tag des Folgemonats angefallen sind. Die Rechnungen für alle Azure-Abonnements werden in der Regel an dem Tag des Monats generiert, an dem das Konto erstellt wurde. Die Generierung kann sich jedoch um bis zu zwei Tage verzögern. Wenn John sein Konto also beispielsweise am 2. Februar erstellt hat, werden die Rechnungen für *Azure-Abonnement 01* und *Azure-Abonnement 02* in der Regel jeweils am zweiten Tag des Monats generiert. Manchmal kann es aber auch bis zu zwei Tage länger dauern.

**Azure Marketplace, Reservierungen und Spot-VMs:** Eine Rechnung wird für Reservierungen, Marketplace-Produkte und Spot-VMs generiert, die unter Verwendung eines Abonnements erworben wurden. Die Rechnung enthält jeweils die entsprechenden Gebühren des Vormonats. Ein Beispiel: John hat am 1. März und am 30. März jeweils eine Reservierung erworben. Im April wird eine einzelne Rechnung für beide Reservierungen generiert. Die Rechnungen für Azure Marketplace, Reservierungen und Spot-VMs werden immer ungefähr am neunten Tag des Monats generiert.

Wenn Sie für Azure mit Kreditkarte bezahlen und eine Reservierung erwerben, wird von Azure sofort eine Rechnung generiert. Bei Zahlung auf Rechnung wird die Reservierung dagegen in der nächsten Monatsrechnung abgerechnet.

**Azure-Supportplan:** Für Ihr Supportplanabonnement wird jeweils eine Monatsrechnung generiert. Die erste Rechnung wird am Tag des Kaufs (oder bis zu zwei Tage später) generiert. Nachfolgende Supportplanrechnungen werden in der Regel an dem Tag des Monats generiert, an dem das Konto erstellt wurde. Die Generierung kann sich jedoch um bis zu zwei Tage verzögern.

## <a name="download-your-mosp-azure-subscription-invoice"></a>Herunterladen Ihrer Azure-Abonnementrechnung für ein MOSP

Eine Rechnung wird nur für ein Abonnement generiert, das zu einem Abrechnungskonto für ein MOSP gehört. [Überprüfen Sie Ihren Zugriff auf ein MOSP-Konto.](../manage/view-all-accounts.md#check-the-type-of-your-account) 

Sie müssen über eine Kontoadministratorrolle für ein Abonnement verfügen, um die zugehörige Rechnung herunterladen zu können. Benutzer mit der Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ können die Rechnung herunterladen, wenn der Kontoadministrator ihnen die entsprechende Berechtigung erteilt hat. Weitere Informationen finden Sie unter [Berechtigen von Benutzern zum Herunterladen von Rechnungen](../manage/manage-billing-access.md#opt-in).

1. Wählen Sie im Azure-Portal auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus.
1. Wählen Sie im Abrechnungsabschnitt die Option **Rechnungen** aus.  
    ![Screenshot: Auswählen der Option „Rechnungen“ für ein Abonnement](./media/download-azure-invoice/select-subscription-invoice.png)
1. Wählen Sie **Herunterladen** aus, um eine PDF-Version Ihrer Rechnung herunterzuladen, und wählen Sie anschließend im Rechnungsabschnitt die Option **Herunterladen** aus.  
    [![Screenshot: Abrechnungszeiträume, Option „Herunterladen“ und Gesamtgebühren für die einzelnen Abrechnungszeiträume](./media/download-azure-invoice/download-invoice-subscription.png)](./media/download-azure-invoice/download-invoice-subscription-zoomed.png#lightbox)
1. Sie können auch die Option **Herunterladen** im Bereich mit den Nutzungsdetails auswählen, um eine tagesbasierte Aufschlüsselung der verbrauchten Mengen und Gebühren herunterladen. Die Vorbereitung der CSV-Datei kann einige Minuten dauern.  
    ![Screenshot: Seite zum Herunterladen von Rechnung und Nutzungsdaten](./media/download-azure-invoice/usage-and-invoice-subscription.png)

Weitere Informationen über Ihre Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](../understand/review-individual-bill.md). Hilfreiche Informationen zur Verwaltung Ihrer Kosten finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](../manage/getting-started.md).

## <a name="download-your-mosp-support-plan-invoice"></a>Herunterladen Ihrer MOSP-Supportplanrechnung

Eine Rechnung wird nur für ein Supportplanabonnement generiert, das zu einem MOSP-Abrechnungskonto gehört. [Überprüfen Sie Ihren Zugriff auf ein MOSP-Konto.](../manage/view-all-accounts.md#check-the-type-of-your-account) 

Sie müssen über eine Kontoadministratorrolle für das Supportplanabonnement verfügen, um die zugehörige Rechnung herunterladen zu können.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1.  Suchen Sie nach **Kostenverwaltung + Abrechnung**.  
    ![Screenshot: Suche nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/download-azure-invoice/search-cmb.png)
1.  Wählen Sie auf der linken Seite die Option **Rechnungen** aus.
1.  Wählen Sie Ihr Supportplanabonnement und anschließend **Herunterladen** aus.  
    [![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/cmb-invoices.png)](./media/download-azure-invoice/cmb-invoices-zoomed-in.png#lightbox)
1.  Wählen Sie **Herunterladen** aus, um eine PDF-Version Ihrer Rechnung herunterzuladen.  
    ![Screenshot: Abrechnungszeiträume, Option „Herunterladen“ und Gesamtgebühren für die einzelnen Abrechnungszeiträume](./media/download-azure-invoice/download-invoice-support-plan.png)

## <a name="allow-others-to-download-an-mosp-subscription-invoice"></a>Erteilen der Berechtigung zum Herunterladen einer MOSP-Abonnementrechnung

Da die PDF-Version der Rechnung persönliche Informationen des Kontoadministrators enthält, muss der Kontoadministrator für dieses Abonnement anderen Benutzern die Berechtigung zum Herunterladen der Rechnung erteilen. Nachdem die Berechtigung erteilt wurde, können folgende Personen und Benutzer mit entsprechenden Rollen Abonnementrechnungen herunterladen:

- Benutzer
- Gruppen
- Dienstprinzipale mit einem Besitzer
- Mitwirkender
- Leser
- Benutzerzugriffsadministrator
- Abrechnungsleser
- Co-Administrator
- Dienstadministrator

So laden Sie eine Rechnung herunter:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Kontoadministrator für das Abonnement an.
1.  Suchen Sie nach **Kostenverwaltung + Abrechnung**.  
1.  Wählen Sie auf der linken Seite die Option **Rechnungen** aus.
1.  Wählen Sie Ihr Azure-Abonnement und anschließend **Zugriff auf Rechnung** aus.  
    [![Screenshot: Auswählen von „Zugriff auf Rechnung“](./media/download-azure-invoice/cmb-select-access-to-invoice.png)](./media/download-azure-invoice/cmb-select-access-to-invoice-zoomed-in.png#lightbox)
1.  Wählen Sie **Ein** und anschließend im oberen Bereich der Seite die Option **Speichern** aus.  
    ![Screenshot: Auswählen von „Ein“ für „Zugriff auf Rechnung“](./media/download-azure-invoice/cmb-access-to-invoice.png)

## <a name="get-mosp-subscription-invoice-in-email"></a>Erhalten der MOSP-Abonnementrechnung per E-Mail

Sie müssen über eine Kontoadministratorrolle für ein Abonnement oder einen Supportplan verfügen, um die zugehörige Rechnung per E-Mail erhalten zu können. E-Mail-Rechnungen sind nur für Abonnements und Supportpläne verfügbar, nicht für Reservierungen oder Azure Marketplace-Käufe. Nach der Aktivierung können weitere Empfänger hinzugefügt werden, die die Rechnung ebenfalls per E-Mail erhalten sollen.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1.  Suchen Sie nach **Kostenverwaltung + Abrechnung**.  
1.  Wählen Sie auf der linken Seite die Option **Rechnungen** aus.
1.  Wählen Sie Ihr Azure- oder Supportplanabonnement und anschließend **Rechnung per E-Mail** aus.  
    [![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/cmb-email-invoice.png)](./media/download-azure-invoice/cmb-email-invoice-zoomed-in.png#lightbox)
1. Wählen Sie **Aktivieren** aus, und akzeptieren Sie die Bedingungen.  
    ![Screenshot: Schritt 2 der Aktivierung](./media/download-azure-invoice/invoice-article-step02.png)
1. Die Rechnung wird an Ihre bevorzugte E-Mail-Adresse für die Kommunikation gesendet. Die E-Mail-Adresse kann in den [Kontaktinformationen Ihres Abrechnungskontos](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade/Properties) aktualisiert werden.  
    ![Screenshot: Schritt 3 der Aktivierung](./media/download-azure-invoice/invoice-article-step03-verify-email.png)

## <a name="share-your-mosp-invoices-with-others-by-email"></a>Weitergeben Ihrer MOSP-Rechnungen per E-Mail

Sie können die Rechnungen für Ihr Abonnement und Ihren Supportplan jeden Monat an Ihr Buchhaltungsteam weitergeben oder sie an eine Ihrer anderen E-Mail-Adressen senden.

1. Gehen Sie dazu wie unter [Erhalten der MOSP-Abonnementrechnung per E-Mail](#get-mosp-subscription-invoice-in-email) vor, und wählen Sie **Empfänger konfigurieren** aus.  
    ![Screenshot: Auswählen von „Empfänger konfigurieren“](./media/download-azure-invoice/invoice-article-step03.png)
1. Geben Sie eine E-Mail-Adresse ein, und wählen Sie anschließend **Empfänger hinzufügen** aus. Sie können mehrere E-Mail-Adressen hinzufügen.  
    ![Screenshot: Hinzufügen weiterer Empfänger](./media/download-azure-invoice/invoice-article-step04.png)
1. Wählen Sie nach dem Hinzufügen aller E-Mail-Adressen am unteren Bildschirmrand die Option **Fertig** aus.

## <a name="invoices-for-mca-and-mpa-billing-accounts"></a>Rechnungen für MCA- und MPA-Abrechnungskonten

Ein MCA-Abrechnungskonto (Microsoft Customer Agreement, Microsoft-Kundenvereinbarung) wird erstellt, wenn Ihre Organisation mit einem Microsoft-Vertreter zusammenarbeitet, um eine Microsoft-Kundenvereinbarung zu unterzeichnen. Einige Kunden in ausgewählten Regionen, die sich über die Azure-Website für ein [Angebot mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) oder ein [kostenloses Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/) registrieren, verfügen möglicherweise ebenfalls über ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. Weitere Informationen finden Sie unter [Erste Schritte mit dem Abrechnungskonto für eine Microsoft-Kundenvereinbarung](../understand/mca-overview.md).

Ein MPA-Abrechnungskonto (Microsoft Partner Agreement, Microsoft-Partnervereinbarung) wird für CSP-Partner (Cloud Solution Provider, Cloudlösungsanbieter) erstellt, um die Kundenverwaltung in der neuen Handelsumgebung zu ermöglichen. Partner benötigen mindestens einen Kunden mit einem [Azure-Plan](https://docs.microsoft.com/partner-center/purchase-azure-plan), damit sie das Abrechnungskonto im Azure-Portal verwalten können. Weitere Informationen finden Sie unter [Erste Schritte mit Ihrem Abrechnungskonto für eine Microsoft-Partnervereinbarung](../understand/mpa-overview.md).

Am Anfang jedes Kalendermonats wird in Ihrem Konto eine monatliche Rechnung für jedes Abrechnungsprofil erstellt. Die Rechnung enthält die jeweiligen Gebühren für alle Azure-Abonnements und andere Käufe aus dem Vormonat. Ein Beispiel: John hat *Azure-Abonnement 01* am 5. März und *Azure-Abonnement 02* am 10. März erstellt. Am 28. März hat er das Abonnement *Azure-Support 01* unter Verwendung von *Abrechnungsprofil 01* erworben. John erhält Anfang April eine einzelne Rechnung mit den Gebühren für die Azure-Abonnements und den Supportplan.

##  <a name="download-an-mca-or-mpa-billing-profile-invoice"></a>Herunterladen einer Rechnung für ein MCA-oder MPA-Abrechnungsprofil

Sie müssen über eine Rolle vom Typ „Besitzer“, „Mitwirkender“, „Leser“ oder „Rechnungs-Manager“ für ein Abrechnungsprofil verfügen, um die zugehörige Rechnung über das Azure-Portal herunterladen zu können. Benutzer mit einer Rolle vom Typ „Besitzer“, „Mitwirkender“ oder „Leser“ für ein Abrechnungskonto können Rechnungen für alle Abrechnungsprofile des Kontos herunterladen.

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1.  Suchen Sie nach **Kostenverwaltung + Abrechnung**.  
1. Wählen Sie auf der linken Seite die Option **Rechnungen** aus.
    [![Screenshot: Seite „Rechnungen“ für ein MCA-Abrechnungskonto](./media/download-azure-invoice/mca-billing-profile-invoices.png)](./media/download-azure-invoice/mca-billing-profile-invoices-zoomed.png#lightbox)
1. Wählen Sie in der Rechnungstabelle die Rechnung aus, die Sie herunterladen möchten.
1. Wählen Sie oben auf der Seite die Option **Rechnungs-PDF herunterladen** aus.  
    ![Screenshot: Herunterladen der PDF-Rechnung](./media/download-azure-invoice/mca-billing-profile-download-invoice.png)
1. Sie können auch die Option **Azure-Nutzung herunterladen** auswählen, um eine CSV-Datei mit der tagesbasierten Aufschlüsselung der verbrauchten Mengen und voraussichtlichen Gebühren herunterladen. Die Vorbereitung der CSV-Datei kann einige Minuten dauern.

## <a name="get-your-billing-profiles-invoice-in-email"></a>Erhalten der Rechnung Ihres Abrechnungsprofils per E-Mail

Sie müssen über eine Rolle vom Typ „Besitzer“ oder „Mitwirkender“ für das Abrechnungsprofil oder das zugehörige Abrechnungskonto verfügen, um die Einstellung zur Rechnung per E-Mail aktualisieren zu können. Nach der Aktivierung erhalten alle Benutzer, die über eine Rolle vom Typ „Besitzer“, „Mitwirkender“, „Leser“ oder „Rechnungs-Manager“ für ein Abrechnungsprofil verfügen, die zugehörige Rechnung per E-Mail. 

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1.  Suchen Sie nach **Kostenverwaltung + Abrechnung**.  
1.  Wählen Sie auf der linken Seite die Option **Rechnungen** und anschließend oben auf der Seite die Option **Rechnung per E-Mail** aus.  
    [![Screenshot: Seite „Rechnungen“ für ein MCA-Abrechnungskonto](./media/download-azure-invoice/mca-billing-profile-select-email-invoice.png)](./media/download-azure-invoice/mca-billing-profile-select-email-invoice-zoomed.png#lightbox)
1.  Sollten Sie über mehrere Abrechnungsprofile verfügen, wählen Sie ein Abrechnungsprofil und anschließend **Aktivieren** aus.  
    ![Screenshot: Seite „Rechnungen“ für ein MCA-Abrechnungskonto](./media/download-azure-invoice/mca-billing-profile-email-invoice.png)
1.  Wählen Sie **Update** aus.

## <a name="give-others-access-to-mca-or-mpa-invoices"></a>Ermöglichen des Zugriffs auf MCA- oder MPA-Rechnungen durch andere Benutzer

Sie können anderen Benutzern Zugriff zum Anzeigen, Herunterladen und Bezahlen von Rechnungen gewähren, indem Sie ihnen die Rolle „Rechnungs-Manager“ für ein MCA- oder MPA-Abrechnungsprofil zuweisen. Wenn Sie sich dafür entschieden haben, Ihre Rechnung per E-Mail zu erhalten, erhalten die Benutzer die Rechnungen ebenfalls per E-Mail.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.  
1. Wählen Sie auf der linken Seite die Option **Abrechnungsprofile** aus. Wählen Sie in der Liste mit den Abrechnungsprofilen ein Abrechnungsprofil aus, für das Sie die Rolle „Rechnungs-Manager“ zuweisen möchten.  
   ![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/mca-select-profile-zoomed-in.png)
1. Wählen Sie auf der linken Seite die Option **Zugriffssteuerung (IAM)** und anschließend oben auf der Seite die Option **Hinzufügen** aus.  
    ![Screenshot: Zugriffssteuerungsseite](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png)
1. Wählen Sie in der Dropdownliste „Rolle“ die Option **Rechnungs-Manager** aus. Geben Sie die E-Mail-Adresse des Benutzers ein, dem Sie Zugriff gewähren möchten. Wählen Sie **Speichern** aus, um die Rolle zuzuweisen.  
   ![Screenshot: Hinzufügen eines Benutzers als Rechnungs-Manager](./media/download-azure-invoice/mca-added-invoice-manager.png)

## <a name="why-you-might-not-see-an-invoice"></a><a name="noinvoice"></a> Mögliche Gründe für nicht angezeigte Rechnungen

Mehrere Gründe können dafür ausschlaggebend sein, dass Sie keine Rechnung sehen:

- Es sind weniger als 30 Tage seit dem Abschluss Ihres Azure-Abonnements vergangen. 
- Die Abrechnung von Azure erfolgt wenige Tage nach dem Ende Ihres Abrechnungszeitraums. Unter Umständen wurde also noch keine Rechnung generiert.
- Sie haben keine Berechtigung zur Ansicht von Rechnungen.
    Im Falle eines MCA- oder MPA-Abrechnungskontos müssen Sie über eine Rolle vom Typ „Besitzer“, „Mitwirkender“, „Leser“ oder „Rechnungs-Manager“ (für ein Abrechnungsprofil) bzw. über eine Rolle vom Typ „Besitzer“, „Mitwirkender“ oder „Leser“ (für das Abrechnungskonto) verfügen, um Rechnungen anzeigen zu können. Bei anderen Abonnements werden die Rechnungen unter Umständen nur für den Kontoadministrator angezeigt. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](../manage/manage-billing-access.md).
- Falls Sie über ein MOSP-Abrechnungskonto verfügen und sich für ein kostenloses Azure-Konto oder für ein Abonnement mit einem monatlichen Guthaben registriert haben, erhalten Sie nur dann eine Rechnung, wenn Sie das monatliche Guthaben überschreiten. Bei einem MCA- oder MPA-Abrechnungskonto erhalten Sie immer eine Rechnung.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über Ihre Rechnung und Gebühren zu erfahren:

- [Anzeigen und Herunterladen der Microsoft Azure-Nutzung und -Gebühren](download-azure-daily-usage.md)
- [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](review-individual-bill.md)
- [Grundlegendes zu den Benennungen in Ihrer Azure-Rechnung](understand-invoice.md)

MCA-bezogene Informationen finden Sie hier:

- [Grundlegendes zu den Gebühren auf der Rechnung für Ihr Abrechnungsprofil](review-customer-agreement-bill.md)
- [Grundlegendes zu den Benennungen auf der Rechnung für Ihr Abrechnungsprofil](mca-understand-your-invoice.md)
- [Grundlegendes zur CSV-Datei für die Azure-Nutzung und -Gebühren für Ihr Abrechnungsprofil](mca-understand-your-usage.md)


