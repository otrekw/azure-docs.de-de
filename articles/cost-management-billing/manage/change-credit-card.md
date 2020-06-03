---
title: Ändern Ihrer Kreditkarte für Azure
description: Es wird beschrieben, wie Sie die für die Zahlung eines Azure-Abonnements verwendete Kreditkarte ändern.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: 99b6d86a65ec05a1299abfffae779113e573310c
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834967"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Hinzufügen, Aktualisieren oder Entfernen einer Kreditkarte für Azure

Dieses Dokument gilt für Kunden, die sich online mit einer Kreditkarte für Azure registriert haben.

Im Azure-Portal können Sie Ihre Standardzahlungsmethode auf eine neue Kreditkarte festlegen, Ihre Kreditkartendetails aktualisieren und nicht mehr verwendete Kreditkarten löschen. Sie können diese Änderungen nur als [Kontoadministrator](billing-subscription-transfer.md#whoisaa) vornehmen.

Für Microsoft Azure werden Kreditkarten und Scheck/Überweisung als Zahlungsmethode unterstützt. Informationen zum Beantragen der Zahlung per Scheck/Überweisung finden Sie unter [Zahlen für Ihr Azure-Abonnement auf Rechnung](pay-by-invoice.md).

Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, sind Ihre Zahlungsmethoden mit Abrechnungsprofilen verknüpft. Informationen zum Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung finden Sie [hier](#check-the-type-of-your-account). Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, fahren Sie mit [Verwalten von Kreditkarten für eine Microsoft-Kundenvereinbarung](#manage-credit-cards-for-a-microsoft-customer-agreement) fort.

<a id="addcard"></a>

## <a name="manage-credit-cards-for-an-azure-subscription"></a>Verwalten von Kreditkarten für ein Azure-Abonnement

Die folgenden Abschnitte gelten für Kunden, die über ein Abrechnungskonto für das Microsoft Online Services-Programm verfügen. Informationen zum Überprüfen des Abrechnungskontotyps finden Sie [hier](#check-the-type-of-your-account). Wenn es sich bei Ihrem Abrechnungskonto um ein Konto vom Typ „Microsoft Online Services-Programm“ handelt, sind Zahlungsmethoden mit einzelnen Azure-Abonnements verknüpft. Wenn nach dem Hinzufügen der Kreditkarte eine Fehlermeldung ausgegeben wird, befolgen Sie die Anleitung unter [Ablehnung der Kreditkarte bei der Azure-Registrierung](../../billing/billing-credit-card-fails-during-azure-sign-up.md).

### <a name="change-credit-card-for-a-subscription-by-adding-a-new-credit-card"></a>Ändern der Kreditkarte für ein Abonnement durch Hinzufügen einer neuen Kreditkarte

Sie können die Standardkreditkarte Ihres Azure-Abonnements im Azure-Portal auf eine neue oder zuvor gespeicherte Kreditkarte festlegen. Sie müssen der Kontoadministrator sein, um die Kreditkarte ändern zu können. Falls mehr als eines Ihrer Abonnements über die gleiche aktive Zahlungsmethode verfügt, wird durch die Änderung der aktiven Zahlungsmethode für eines dieser Abonnements auch die aktive Zahlungsmethode für die anderen Abonnements aktualisiert.


Wenn Sie die Standardkreditkarte Ihres Abonnements auf eine neue Kreditkarte festlegen möchten, gehen Sie wie folgt vor:

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suche](./media/change-credit-card/search.png)

1. Wählen Sie das Abonnement aus, dem Sie die Kreditkarte hinzufügen möchten.
1. Wählen Sie die Option **Zahlungsmethoden**.

    ![Screenshot mit ausgewählter Option „Zahlungsmethoden verwalten“.](./media/change-credit-card/payment-methods-blade-x.png)

1. Wählen Sie oben links das Pluszeichen („+“), um eine Karte hinzuzufügen. Rechts wird ein Kreditkartenformular angezeigt.
1. Geben Sie die Details der Kreditkarte ein.

    ![Screenshot: Hinzufügen einer neuen Karte](./media/change-credit-card/sub-add-new-x.png)

1. Aktivieren Sie oberhalb des Formulars das Kontrollkästchen **Diese Zahlungsmethode als aktive Methode festlegen**, um diese Karte als Ihre aktive Zahlungsmethode festzulegen. Diese Karte wird zum aktiven Zahlungsmittel für alle Abonnements, indem dieselbe Karte wie für das ausgewählte Abonnement verwendet wird.

1. Wählen Sie **Weiter** aus.

### <a name="change-credit-card-for-a-subscription-to-a-previously-saved-credit-card"></a>Ändern der Kreditkarte für ein Abonnement in eine zuvor gespeicherte Kreditkarte

Die Standardkreditkarte Ihres Abonnements kann auch in eine Kreditkarte geändert werden, die bereits in Ihrem Konto gespeichert ist:

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suche](./media/change-credit-card/search.png)

1. Wählen Sie das Abonnement aus, dem Sie die Kreditkarte hinzufügen möchten.
1. Wählen Sie die Option **Zahlungsmethoden**.

    ![Screenshot mit ausgewählter Option „Zahlungsmethoden verwalten“.](./media/change-credit-card/payment-methods-blade-x.png)

1. Aktivieren Sie das Kontrollkästchen neben der Karte, die Sie als aktive Zahlungsmethode festlegen möchten.
1. Klicken Sie auf **Als aktiv festlegen**.
    ![Screenshot: Ausgewählte Kreditkarte und Auswahl von „Als aktiv festlegen“](./media/change-credit-card/sub-change-active-x.png)

### <a name="edit-credit-card-details"></a>Bearbeiten von Kreditkartendetails

Wenn Ihre Kreditkarte verlängert wird und die Kreditkartennummer gleich bleibt, können Sie die bereits vorhandenen Kreditkartendetails (z. B. das Ablaufdatum) aktualisieren. Wenn sich die Kreditkartennummer aufgrund von Verlust, Diebstahl oder Ablauf ändert, führen Sie die Schritte im Abschnitt [Hinzufügen einer Kreditkarte als Zahlungsmethode](#addcard) aus. Die Kartenprüfnummer müssen Sie nicht aktualisieren.

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suche](./media/change-credit-card/search.png)

1. Wählen Sie die Option **Zahlungsmethoden**.

    ![Screenshot mit ausgewählter Option „Zahlungsmethoden verwalten“.](./media/change-credit-card/payment-methods-blade-x.png)

1. Klicken Sie auf die Kreditkarte, die Sie bearbeiten möchten. Rechts wird ein Kreditkartenformular angezeigt.

    ![Screenshot: Auswahl der Kreditkarte](./media/change-credit-card/edit-card-x.png)

1. Aktualisieren Sie die Kreditkartendetails.
1. Wählen Sie **Speichern** aus.

### <a name="delete-a-credit-card-from-the-account"></a>Löschen einer Kreditkarte aus dem Konto

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie links auf der Seite die Option **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suche](./media/change-credit-card/search.png)

1. Wählen Sie unter **Abrechnung** die Option **Zahlungsmethoden**.

    ![Screenshot mit ausgewählter Option „Zahlungsmethoden verwalten“.](./media/change-credit-card/payment-methods-blade-x.png)

1. Aktivieren Sie das Kontrollkästchen neben der Karte, die Sie entfernen möchten.
1. Klicken Sie auf **Löschen**.

Wenn Ihre Kreditkarte die aktive Zahlungsmethode für eines Ihrer Microsoft-Abonnements ist, können Sie sie nicht aus Ihrem Azure-Konto entfernen. Ändern Sie die aktive Zahlungsmethode für alle Abonnements, die mit dieser Kreditkarte verknüpft sind, und versuchen Sie es noch einmal.

## <a name="manage-credit-cards-for-a-microsoft-customer-agreement"></a>Verwalten von Kreditkarten für eine Microsoft-Kundenvereinbarung

Die folgenden Abschnitte gelten für Kunden, die über eine Microsoft-Kundenvereinbarung verfügen und sich online mit einer Kreditkarte für Azure registriert haben. Informationen zum Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung finden Sie [hier](#check-the-type-of-your-account).

### <a name="change-default-credit-card"></a>Ändern der Standardkreditkarte

Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, ist Ihre Zahlungsmethoden mit einem Abrechnungsprofil verknüpft. Die Zahlungsmethode für ein Abrechnungsprofil kann nur von der Person geändert werden, die sich für Azure registriert und das Abrechnungskonto erstellt hat.

Wenn Sie die Standardzahlungsmethode Ihres Abrechnungsprofils in „Scheck/Überweisung“ ändern möchten, lesen Sie [Zahlen für Ihr Azure-Abonnement auf Rechnung](pay-by-invoice.md).

Führen Sie zum Ändern Ihrer Kreditkarte die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Klicken Sie im Menü auf der linken Seite auf **Abrechnungsprofile**.
1. Wählen Sie ein Abrechnungsprofil aus.
1. Wählen Sie im Menü links die Option **Zahlungsmethoden**.

   ![Screenshot: Zahlungsmethoden im Menü](./media/change-credit-card/payment-methods-tab-mca.png)

1. Klicken Sie im Abschnitt **Standardzahlungsmethode** auf **Ändern**.

    ![Screenshot: Schaltfläche „Ändern“](./media/change-credit-card/change-payment-method-mca.png)

1. Wählen Sie auf dem neuen Blatt auf der rechten Seite entweder eine bereits vorhandene Karte aus der Dropdownliste aus, oder klicken Sie auf den blauen Link „Neue Zahlungsmethode hinzufügen“, um eine neue Karte hinzuzufügen.

### <a name="edit-or-delete-a-credit-card"></a>Bearbeiten oder Löschen einer Kreditkarte

Sie können Kreditkartendetails bearbeiten, um beispielsweise das Ablaufdatum zu aktualisieren, und Kreditkarten aus Ihrem Konto im Azure-Portal löschen. Eine Kreditkarte kann nur gelöscht werden, wenn sie mit keinem Azure-Abonnement oder Abrechnungsprofil verknüpft ist. Sollte sie mit einem deaktivierten Azure-Abonnement verknüpft sein, kann sie erst gelöscht werden, nachdem das Abonnement gelöscht wurde (30 bis 90 Tage nach der Kündigung).

Gehen Sie zum Bearbeiten oder Löschen einer Kreditkarte wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Klicken Sie im Menü auf der linken Seite auf **Abrechnungsprofile**.
1. Wählen Sie ein Abrechnungsprofil aus.
1. Wählen Sie im Menü links die Option **Zahlungsmethoden**.

   ![Screenshot: Zahlungsmethoden im Menü](./media/change-credit-card/payment-methods-tab-mca.png)

1. Suchen Sie im Abschnitt **Ihre Kreditkarten** nach der Kreditkarte, die Sie bearbeiten oder löschen möchten.
1. Wählen Sie die Auslassungspunkte (`...`) am Ende der Zeile aus.

    ![Screenshot: Auslassungspunkte](./media/change-credit-card/edit-delete-credit-card-mca.png)

1. Wenn Sie Ihre Kreditkartendetails bearbeiten möchten, wählen Sie im Kontextmenü **Bearbeiten** aus.
1. Wenn Sie Ihre Kreditkarte löschen möchten, wählen Sie im Kontextmenü **Löschen** aus.

## <a name="troubleshooting"></a>Problembehandlung

Virtuelle Karten oder Prepaidkarten werden nicht unterstützt. Wenn beim Hinzufügen oder Aktualisieren einer gültigen Kreditkarte Fehler auftreten, versuchen Sie, den Browser im privaten Modus zu öffnen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

In den folgenden Abschnitten werden häufig gestellte Fragen zum Ändern der Kreditkarteninformationen beantwortet.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Das Abonnement ist deaktiviert. Warum kann ich jetzt meine Kreditkarte nicht entfernen?

Nach der Deaktivierung oder Kündigung Ihres Abonnements warten wir 90 Tage, bevor wir Ihr Abonnement endgültig löschen. Während der Aufbewahrungsdauer wird die Zahlungsmethode beibehalten, für den Fall, dass Sie das Abonnement reaktivieren möchten. Anschließend wird das Abonnement endgültig gelöscht.

Wenn Sie Ihre Kreditkarte vor Ablauf des Aufbewahrungszeitraums von 90 Tagen entfernen möchten, [müssen Sie Ihr Abonnement reaktivieren](subscription-disabled.md). Wenn die Reaktivierung nicht möglich ist, [wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Warum wird beständig die Meldung „Ihre Anmeldesitzung ist abgelaufen. Klicken Sie hier, um sich wieder anzumelden.“ angezeigt?

Wenn diese Fehlermeldung weiterhin angezeigt wird, selbst wenn Sie sich bereits abgemeldet und dann erneut angemeldet haben, versuchen Sie es erneut mit einer privaten Browsersitzung.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Wie kann ich für jedes meiner Abonnements eine andere Karte verwenden?

Wenn für Ihre Abonnements bereits ein und dieselbe Karte verwendet wird, ist es leider nicht möglich, sie zu trennen, um verschiedene Karten zu verwenden. Wenn Sie sich jedoch für ein neues Abonnement registrieren, können Sie eine neue Zahlungsmethode für dieses Abonnement auswählen.

### <a name="how-do-i-make-payments"></a>Wie tätige ich Zahlungen?

Wenn Sie eine Kreditkarte als Zahlungsmethode eingerichtet haben, wird diese Karte nach jedem Abrechnungszeitraum automatisch belastet. Sie müssen nichts weiter tun.

Wenn Sie [auf Rechnung zahlen](pay-by-invoice.md), senden Sie Ihre Zahlung an den Empfänger, der unten auf der Rechnung angegeben ist.

### <a name="how-do-i-change-the-tax-id"></a>Wie ändere ich die Steuernummer?

Aktualisieren Sie zum Hinzufügen oder Aktualisieren der Steuernummer Ihr Profil im [Azure-Kontocenter](https://account.azure.com/Profile), und wählen Sie anschließend die Option **Steuereintrag**. Diese Steuernummer wird auf Ihrer Rechnung angezeigt und für die Berechnung von Steuerbefreiungen verwendet.

## <a name="check-the-type-of-your-account"></a>Überprüfen Sie den Typ Ihres Abrechnungskontos.

[!INCLUDE [billing-check-mca](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Azure-Reservierungen](../reservations/save-compute-costs-reservations.md), um zu ermitteln, ob Sie damit Kosten sparen können.
