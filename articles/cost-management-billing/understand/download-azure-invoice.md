---
title: Anzeigen und Herunterladen der Microsoft Azure-Rechnung
description: Erfahren Sie, wie Sie Ihre Microsoft Azure-Rechnung anzeigen und herunterladen.
keywords: Rechnung, Rechnungsdownload, Azure-Rechnung, Azure-Nutzung
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 691d27acebf238e84265870e8c01976bfc2412b2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77200264"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Anzeigen und Herunterladen der Microsoft Azure-Rechnung

Für die meisten Abonnements können Sie die Rechnung aus dem [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) herunterladen oder per E-Mail zugeschickt bekommen. Wenn Sie Azure-Kunde mit einem Enterprise Agreement (EA-Kunde) sind, können Sie die Rechnungen Ihrer Organisation nicht herunterladen. Rechnungen werden an den Benutzer gesendet, der für den Empfang von Rechnungen für die Registrierung festgelegt wurde.

Nur bestimmte Rollen sind zum Anzeigen von Rechnungen berechtigt. Zu diesen Rollen zählen beispielsweise der Kontoadministrator und der Unternehmensadministrator. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](../manage/manage-billing-access.md).

Wenn Sie über eine Microsoft-Kundenvereinbarung (MCA) verfügen, benötigen Sie eine der folgenden Rollen, um Ihre Rechnungen zu erhalten:

- Besitzer des Abrechnungsprofils
- Mitwirkender
- Leser
- Rechnungs-Manager

Wenn Sie über eine Microsoft-Partnervereinbarung (MPA) verfügen, müssen Sie der globale Administrator oder Administrator-Agent in der Partnerorganisation sein, um Azure-Rechnungen anzuzeigen und herunterzuladen. [Überprüfen Sie Ihren Abrechnungskontotyp](#check-your-billing-account-type), um zu sehen, welche Berechtigungen Sie benötigen.

<!-- For more information about billing roles for Microsoft Customer Agreements, see [Billing profile roles and tasks](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks). -->

## <a name="why-you-might-not-get-an-invoice"></a><a name="noinvoice"></a> Mögliche Gründe für nicht erhaltene Rechnungen

Mehrere Gründe können dafür ausschlaggebend sein, dass Sie keine Rechnung sehen:

- Es sind weniger als 30 Tage seit dem Abschluss Ihres Azure-Abonnements vergangen.

- Die Abrechnung von Azure erfolgt am Ende Ihres Rechnungszeitraums. Unter Umständen wurde also noch keine Rechnung generiert. Warten Sie bis zum Ende des Abrechnungszeitraums.

- Sie haben keine Berechtigung zur Ansicht von Rechnungen. Wenn Sie über eine MCA oder MPA verfügen, müssen Sie der Besitzer des Abrechnungsprofils, Mitwirkender, Benutzer mit Leseberechtigung oder Rechnungs-Manager sein. Für andere Abonnements werden ältere Rechnungen nur angezeigt, wenn Sie der Kontoadministrator sind. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](../manage/manage-billing-access.md).

- Falls Sie über eine kostenlose Testversion oder über ein monatliches Guthaben für Ihr Abonnement verfügen, erhalten Sie nur dann eine Rechnung, wenn Sie das monatliche Guthaben überschreiten. Wenn Sie über eine Microsoft-Kundenvereinbarung oder eine Microsoft-Partnervereinbarung verfügen, erhalten Sie immer eine Rechnung.

## <a name="download-invoices-in-the-azure-portal"></a>Herunterladen von Rechnungen im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach *Kostenverwaltung + Abrechnung*.
1. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto oder ein Abrechnungsprofil auswählen.
1. Wählen Sie im Menü auf der linken Seite die Option **Rechnungen** unter **Abrechnung** aus.
1. Suchen Sie im Rechnungsraster nach der Zeile der Rechnung, die Sie herunterladen möchten.
1. Klicken Sie am Ende der Zeile auf das Downloadsymbol oder die Auslassungspunkte (`...`).
1. Wählen Sie im Downloadmenü **Rechnung** aus.

Weitere Informationen über Ihre Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](review-individual-bill.md). Hilfreiche Informationen zur Verwaltung Ihrer Kosten finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](../manage/getting-started.md).

## <a name="get-your-subscriptions-invoices-in-email"></a>Abrufen der Rechnungen für Abonnements per E-Mail

Sie können die entsprechende Option aktivieren und zusätzliche Empfänger konfigurieren, die Ihre Azure-Rechnung in einer E-Mail erhalten. Diese Funktion ist für bestimmte Abonnements möglicherweise nicht verfügbar, z.B. für Supportangebote, Enterprise Agreements oder Azure in Open.

1. Wählen Sie auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus. Wählen Sie jedes Abonnement aus, das Sie besitzen. Klicken Sie auf **Rechnungen** und dann auf **Rechnung per E-Mail**.

    ![Screenshot mit dem Ablauf der Aktivierung](./media/download-azure-invoice/invoicesdeeplink01.png)

2. Klicken Sie auf **Aktivieren**, und stimmen Sie den Bedingungen zu.

    ![Screenshot mit dem Ablauf von Schritt 2 der Aktivierung](./media/download-azure-invoice/invoicearticlestep02.png)

3. Nachdem Sie die Vereinbarung akzeptiert haben, können Sie zusätzliche Empfänger konfigurieren. Wenn ein Empfänger entfernt wird, wird die E-Mail-Adresse nicht mehr gespeichert. Wenn Sie Ihre Meinung ändern, müssen Sie ihn erneut hinzufügen.

    ![Screenshot mit dem Ablauf von Schritt 3 der Aktivierung](./media/download-azure-invoice/invoicearticlestep03.png)

Wenn Sie keine E-Mail erhalten, nachdem Sie diese Schritte befolgt haben, stellen Sie sicher, dass Ihre E-Mail-Adresse in den [Kommunikationseinstellungen auf Ihrem Profil](https://account.windowsazure.com/profile) richtig ist.

## <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Kündigen des Erhalts der Rechnungen für Abonnements per E-Mail

Führen Sie zum Deaktivieren des Erhalts der Rechnung per E-Mail die obigen Schritte aus, und klicken Sie auf **Abonnement für Rechnungen per E-Mail kündigen**. Dadurch werden alle E-Mail-Adressen entfernt, an die Rechnungen gesendet werden sollten. Sie können die Empfänger neu konfigurieren, wenn Sie die Rechnungen wieder per E-Mail empfangen möchten.

 ![Screenshot mit dem Ablauf der Kündigung](./media/download-azure-invoice/invoicearticlestep04.png)

<!-- Does following section apply to MPA too? -->
## <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Abrufen von Rechnungen für die Microsoft-Kundenvereinbarung per E-Mail

Wenn Sie über ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung verfügen, können Sie Ihre Rechnungen auf Wunsch per E-Mail empfangen. Alle Benutzer mit der Rolle „Besitzer“, „Mitwirkender“, „Leser“ oder „Rechnungs-Manager“ in einem Abrechnungsprofil erhalten die dazugehörigen Rechnungen per E-Mail.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot der Suche im Portal nach Abonnements](./media/download-azure-invoice/search-cmb.png)

1. Wählen Sie auf der linken Seite die Option **Abrechnungsprofile** aus. Wählen Sie in der Liste mit den Abrechnungsprofilen ein Abrechnungsprofil aus, um die dazugehörigen Rechnungen per E-Mail zu erhalten.

   [![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Wählen Sie auf der linken Seite die Option **Eigenschaften** und anschließend **Einstellung für Rechnung per E-Mail aktualisieren** aus.

   [![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/mca-select-update-email-preferences.png)](./media/download-azure-invoice/mca-select-update-email-preferences.png#lightbox)

1. Wählen Sie **Abonnieren** aus, und klicken Sie anschließend auf **Aktualisieren**.

   [![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/mca-select-email-opt-in.png)](./media/download-azure-invoice/mca-select-email-opt-in.png#lightbox)

## <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Kündigen des Erhalts von Rechnungen für die Microsoft-Kundenvereinbarung per E-Mail

Führen Sie zum Deaktivieren des Erhalts der Rechnung per E-Mail die obigen Schritte aus, und klicken Sie auf **Deaktivieren**. Daraufhin erhalten Benutzer mit der Rolle „Besitzer“, „Mitwirkender“, „Leser“ oder „Rechnungs-Manager“ künftig keine Rechnungen mehr per E-Mail.

## <a name="give-others-access-to-your-microsoft-customer-agreement-invoices"></a>Gewähren des Zugriffs auf Ihre Rechnungen für die Microsoft-Kundenvereinbarung

Sie können anderen Zugriff zum Anzeigen, Herunterladen und Bezahlen von Rechnungen gewähren, indem Sie ihnen die Rolle „Rechnungs-Manager“ für ein Abrechnungsprofil zuweisen. Wenn Sie sich dafür entschieden haben, Ihre Rechnung per E-Mail zu erhalten, erhalten diese Benutzer die Rechnungen ebenfalls per E-Mail.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot der Suche im Portal nach Abonnements](./media/download-azure-invoice/search-cmb.png)

1. Wählen Sie auf der linken Seite die Option **Abrechnungsprofile** aus. Wählen Sie in der Liste mit den Abrechnungsprofilen ein Abrechnungsprofil aus, für das Sie die Rolle „Rechnungs-Manager“ zuweisen möchten.

   [![Screenshot: Liste mit Abrechnungsprofilen](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Wählen Sie auf der linken Seite die Option **Zugriffssteuerung (IAM)** und anschließend oben auf der Seite die Option **Hinzufügen** aus.

   [![Screenshot: Zugriffssteuerungsseite](./media/download-azure-invoice/mca-select-access-control.png)](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png#lightbox)

1. Wählen Sie in der Dropdownliste „Rolle“ die Option **Rechnungs-Manager** aus. Geben Sie die E-Mail-Adresse des Benutzers ein, dem Sie Zugriff erteilen möchten. Wählen Sie **Speichern** aus, um die Rolle zuzuweisen.

   [![Screenshot: Hinzufügen eines Benutzers als Rechnungs-Manager](./media/download-azure-invoice/mca-added-invoice-manager.png)](./media/download-azure-invoice/mca-added-invoice-manager.png#lightbox)

## <a name="check-your-billing-account-type"></a>Überprüfen des Abrechnungskontotyps
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über Ihre Rechnung und Gebühren zu erfahren:

- [Anzeigen und Herunterladen der Microsoft Azure-Nutzung und -Gebühren](download-azure-daily-usage.md)
- [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](review-individual-bill.md)
- [Grundlegendes zu den Benennungen in Ihrer Azure-Rechnung](understand-invoice.md)
- [Grundlegendes über Benennungen zu den Gebühren in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure](understand-usage.md)
- [Anzeigen der Azure Enterprise Agreement-Preise für Ihre Organisation](../manage/ea-pricing.md)

Wenn Sie eine Microsoft-Kundenvereinbarung abgeschlossen haben, lesen Sie die folgenden Artikel:

- [Grundlegendes zu den Gebühren auf der Rechnung für Ihr Abrechnungsprofil](review-customer-agreement-bill.md)
- [Grundlegendes zu den Benennungen auf der Rechnung für Ihr Abrechnungsprofil](mca-understand-your-invoice.md)
- [Grundlegendes zur CSV-Datei für die Azure-Nutzung und -Gebühren für Ihr Abrechnungsprofil](mca-understand-your-usage.md)
- [Anzeigen und Herunterladen von Steuerdokumenten für Ihr Abrechnungsprofil](mca-download-tax-document.md)
- [Anzeigen der Azure Enterprise Agreement-Preise für Ihre Organisation](../manage/ea-pricing.md)
