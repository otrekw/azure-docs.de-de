---
title: Übertragen des Abrechnungsbesitzes eines Azure-Abonnements
description: In diesem Artikel wird beschrieben, wie der Abrechnungsbesitz eines Azure-Abonnements an ein anderes Konto übertragen wird. Zudem finden Sie hier einige häufig gestellte Fragen (FAQs) zu diesem Vorgang.
keywords: Azure-Abonnement übertragen, Azure Übertragung Abonnement, Verschieben Azure-Abonnement in anderes Konto, Azure Änderung Abonnementsbesitzer, Übertragen Azure-Abonnement an anderes Konto, Azure Abrechnungsbesitz übertragen
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 07/01/2020
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 722d1bca7f983c124c85e6d675f51d29c5357522
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85854939"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto

Wenn Sie Ihre Organisation verlassen oder Ihr Abonnement einem anderen Konto in Rechnung gestellt werden soll, möchten Sie möglicherweise den Abrechnungsbesitz Ihres Azure-Abonnements übertragen. Die Übertragung des Abrechnungsbesitzes an ein anderes Konto ermöglicht es den Administratoren des neuen Kontos, Rechnungsaufgaben durchzuführen. Sie können die Zahlungsmethode ändern, Gebühren anzeigen und das Abonnement kündigen.

Wenn Sie den Abrechnungsbesitz behalten, aber den Typ des Abonnements ändern möchten, finden Sie Informationen hierzu unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](switch-azure-offer.md). Informationen zur Steuerung, wer auf Ressourcen im Abonnement zugreifen kann, finden Sie unter [Integrierte Azure-Rollen](../../role-based-access-control/built-in-roles.md).

Wenn Sie ein Kunde mit Enterprise Agreement (EA) sind, können Administratoren in Ihrem Unternehmen den Abrechnungsbesitz Ihrer Abonnements zwischen den Konten übertragen. Weitere Informationen finden Sie unter [Übertragen des Abrechnungsbesitzes von Abonnements mit Enterprise Agreement (EA)](#EA).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Übertragen des Abrechnungsbesitzes eines Azure-Abonnements

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als Administrator für das Abrechnungskonto des Abonnements an, das Sie übertragen möchten. Wie Sie feststellen, ob Sie ein Administrator sind, ist unter [Häufig gestellte Fragen](#faq) beschrieben.

1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. Wählen Sie links die Option **Abonnements**. Je nach Zugriff müssen Sie möglicherweise einen Abrechnungsbereich auswählen. Wählen Sie dann **Abonnements** oder **Azure-Abonnements** aus.

1. Wählen Sie für das Abonnement, das Sie übertragen möchten, **Abrechnungsbesitz übertragen** aus.

   ![Auswählen des zu übertragenden Abonnements](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Geben Sie die E-Mail-Adresse eines Benutzer ein, der ein Abrechnungsadministrator für das Konto ist, das der neue Besitzer des Abonnements sein soll.

1. Wenn Sie Ihr Abonnement an ein Konto in einem anderen Azure AD-Mandanten übertragen, wählen Sie aus, ob das Abonnement in den Mandanten des neuen Kontos verschoben werden soll. Weitere Informationen finden Sie unter [Übertragen eines Abonnements an ein Konto in einem anderen Azure AD-Mandanten](#transfer-a-subscription-to-another-azure-ad-tenant-account).

    > [!IMPORTANT]
    >
    > Wenn Sie das Abonnement in den Azure AD-Mandanten des neuen Kontos verschieben, werden alle [Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-portal.md) zum Zugreifen auf Ressourcen im Abonnement dauerhaft entfernt. Nur der Benutzer des neuen Kontos, der Ihre Übertragungsanforderung angenommen hat, hat Zugriff auf die Verwaltung von Ressourcen im Abonnement. Weitere Informationen finden Sie im nächsten Abschnitt: [Übertragen eines Abonnements an ein anderes Azure AD-Mandantenkonto](#transfer-a-subscription-to-another-azure-ad-tenant-account). Alternativ können Sie das Kontrollkästchen deaktivieren, mit dem der **Azure AD-Mandant des Abonnements** den Abrechnungsbesitz überträgt, ohne das Abonnement in den Mandanten des neuen Kontos zu verschieben. Wenn Sie dies tun, werden vorhandene Azure-Rollenzuweisungen zum Zugreifen auf Azure-Ressourcen beibehalten.

    ![Seite zum Senden der Übertragungsanforderung](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. Wählen Sie **Übertragungsanforderung senden** aus.

1. Der Benutzer erhält eine E-Mail mit der Aufforderung, Ihre Übertragungsanforderung zu prüfen.

   ![An den Empfänger gesendete E-Mail zur Abonnementübertragung](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Um die Übertragungsanforderung zu genehmigen, muss der Benutzer auf den Link in der E-Mail klicken und den Anweisungen folgen. Anschließend muss der Benutzer eine Zahlungsmethode für das Abonnement auswählen. Sollte der Benutzer über kein Azure-Konto verfügen, muss er sich für ein neues Konto registrieren.

   ![Erste Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Erfolg! Das Abonnement ist jetzt übertragen.

## <a name="transfer-a-subscription-to-another-azure-ad-tenant-account"></a>Übertragen eines Abonnements an ein anderes Azure AD-Mandantenkonto

Wenn Sie sich für Azure registrieren, wird ein Azure Active Directory (AD)-Mandant für Sie erstellt. Der Mandant stellt Ihr Konto dar. Sie verwenden den Mandanten zum Verwalten des Zugriffs auf Ihre Abonnements und Ressourcen.

Wenn Sie ein neues Abonnement erstellen, wird es vom Azure AD-Mandanten Ihres Kontos gehostet. Wenn Sie anderen Benutzern Zugriff auf Ihr Abonnement oder dessen Ressourcen gewähren möchten, müssen Sie diese dazu einladen, Ihrem Mandanten beizutreten. Dadurch können Sie den Zugriff auf Ihre Abonnements und Ressourcen besser steuern.

Wenn Sie den Abrechnungsbesitz Ihres Abonnements an ein Konto in einem anderen Azure AD-Mandanten übertragen, können Sie das Abonnement in den Mandanten des neuen Kontos verschieben. In diesem Fall verlieren alle Benutzer, Gruppen oder Dienstprinzipale, die über [Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-portal.md) zum Verwalten von Abonnements und deren Ressourcen verfügt haben, ihren Zugriff. Nur der Benutzer des neuen Kontos, der Ihre Übertragungsanforderung angenommen hat, hat Zugriff auf die Verwaltung der Ressourcen. Wenn der neue Besitzer den Benutzern, die den Zugriff verloren haben, erneut Zugriff gewähren möchte, muss er diese Benutzer manuell dem Abonnement hinzufügen. Weitere Informationen finden Sie unter [Übertragen eines Azure-Abonnements in ein anderes Azure AD-Verzeichnis (Vorschau)](../../role-based-access-control/transfer-subscription.md).


## <a name="transfer-visual-studio-and-partner-network-subscriptions"></a>Übertragen von Visual Studio- und Partner Network-Abonnements

Mit Abonnements für Visual Studio und Microsoft Partner Network sind monatliche Azure-Gutschriften verbunden. Wenn Sie derartige Abonnements übertragen, ist Ihr Guthaben im Zielabrechnungskonto nicht verfügbar. Für das Abonnement wird das Guthaben des Ziel-Abrechnungskontos verwendet. Beispiel: Bob überträgt am 9. September ein Visual Studio Enterprise-Abonnement an das Konto von Jane, die die Übertragung annimmt. Nach der Übertragung wird für das Abonnement die Gutschrift für Janes Konto verwendet. Das Guthaben wird jeweils am 9. des Monats zurückgesetzt.


<a id="EA"></a>

## <a name="transfer-ea-subscription-billing-ownership"></a>Übertragen des Abrechnungsbesitzes für EA-Abonnements

Der Unternehmensadministrator kann den Besitz von Abonnements zwischen den Konten in einer Registrierung übertragen. Weitere Informationen finden Sie unter [Ändern des Kontobesitzers](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-get-started#change-account-owner) im EA-Portal.

## <a name="next-steps-after-accepting-billing-ownership"></a>Nächste Schritte nach dem Übernehmen des Abrechnungsbesitzes

Wenn Sie den Abrechnungsbesitz für ein Azure-Abonnement angenommen haben, empfiehlt es sich, die folgenden weiteren Schritte zu prüfen:

1. Überprüfen und aktualisieren Sie die Einstellungen für Dienstadministratoren, Co-Admins und Azure-Rollenzuweisungen. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](add-change-subscription-administrator.md) und [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).
1. Aktualisieren Sie die Anmeldeinformationen für die Dienste dieses Abonnements, darunter:
   1. Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../../cloud-services/cloud-services-certs-create.md).
   1. Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../../storage/common/storage-create-storage-account.md).
   1. Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.
1. Wenn Sie mit einem Partner arbeiten, sollten Sie die Partner-ID in diesem Abonnement aktualisieren. Sie können die Partner-ID im [Azure-Portal](https://portal.azure.com) aktualisieren. Weitere Informationen finden Sie unter [Verknüpfen einer Partner-ID mit Ihren Azure-Konten](link-partner-id.md).

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Unterstützte Abonnementtypen

Die Übertragung von Abonnements im Azure-Portal ist für die unten aufgeführten Abonnementtypen verfügbar. Für Abonnements vom Typ [Kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/) oder [Azure in Open (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/) wird derzeit keine Übertragung unterstützt. Wie Sie dies umgehen können, erfahren Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Zum Übertragen anderer Abonnements wie Supportpläne [wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Visual Studio Enterprise-Abonnenten (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)
- [MSDN-Plattformen](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Microsoft Azure-Plan](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\*[Über das EA-Portal](#EA).

\*\* Wird nur für Konten unterstützt, die bei der Registrierung auf der Azure-Website erstellt werden.

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Häufig gestellte Fragen (FAQ) bei der Übertragung

Diese häufig gestellten Fragen richten sich an Benutzer, die den Abrechnungsbesitz eines Azure-Abonnements an ein anderes Konto übertragen möchten.

### <a name="who-is-a-billing-administrator-of-an-account"></a><a name="whoisaa"></a> Wer ist ein Abrechnungsadministrator für ein Konto?

Ein Abrechnungsadministrator ist eine Person, die über Berechtigungen zum Verwalten der Abrechnung für ein Konto verfügt. Sie ist berechtigt, im [Azure-Portal](https://portal.azure.com) auf die Abrechnung zuzugreifen und verschiedene Abrechnungsaufgaben durchzuführen (Abonnements erstellen, Rechnungen anzeigen und bezahlen, Zahlungsmethoden aktualisieren und Ähnliches).

Führen Sie die folgenden Schritte aus, um die Konten zu ermitteln, für die Sie als Abrechnungsadministrator fungieren:

1. Navigieren Sie im Azure-Portal zur Seite [Kostenverwaltung + Abrechnung](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).
1. Wählen Sie im linken Bereich die Option **Alle Abrechnungsbereiche** aus.
1. Auf der Seite „Abonnements“ werden alle Abonnements aufgelistet, für die Sie als Abrechnungsadministrator fungieren.

Verwenden Sie die folgenden Schritte für die Ermittlung, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

1. Navigieren Sie [im Azure-Portal zur Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Wird alles übertragen? Auch Ressourcengruppen, virtuelle Computer, Datenträger und andere aktive Dienste?

Alle Ressourcen wie virtuelle Computer, Datenträger und Websites werden zum neuen Konto übertragen. Wenn Sie aber ein Abonnement an ein Konto in einem anderen Azure AD-Mandanten übertragen, werden [Administratorrollen](add-change-subscription-administrator.md) und [Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-portal.md) für das Abonnement [nicht übertragen](#transfer-a-subscription-to-another-azure-ad-tenant-account). Auch [App-Registrierungen](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) und andere mandantenspezifische Dienste werden nicht mit dem Abonnement übertragen.

### <a name="can-i-transfer-ownership-to-an-account-in-another-countryregion"></a>Kann ich den Besitz an ein Konto in einem anderen Land bzw. in einer anderen Region übertragen?
Länder- bzw. regionsübergreifende Übertragungen können leider nicht über das Azure-Portal abgewickelt werden. Zum Übertragen Ihres Abonnements in ein anderes Land oder in eine andere Region [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>Ich bin Administrator bei zwei Konten. Kann ich ein Abonnement von einem Konto auf das andere übertragen?
Ja. Sie können Abonnements zwischen Ihren Konten übertragen. Vom Konzept her werden Ihre Konten als Konten von zwei verschiedenen Benutzern betrachtet. Daher können Sie mit den oben genannten Schritten Abonnements zwischen Ihren Konten übertragen.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Führt eine Übertragung des Abonnements zu Ausfallzeiten?

Wenn Sie ein Abonnement an ein Konto im gleichen Azure AD-Mandanten übertragen, hat dies keine Auswirkungen auf die Ressourcen, die unter dem Abonnement ausgeführt werden. In PowerShell gespeicherte Kontextinformationen werden jedoch nicht aktualisiert. Daher müssen Sie diese unter Umständen löschen oder die Einstellungen ändern. Wenn Sie das Abonnement an ein Konto in einem anderen Mandanten übertragen und das Abonnement in den Mandanten verschieben, verlieren alle Benutzer, Gruppen und Dienstprinzipale, die über [Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-portal.md) zum Zugreifen auf Ressourcen im Abonnement verfügt haben, ihren Zugriff. Dies kann zu Ausfallzeiten führen.

### <a name="can-users-in-new-account-access-usage-and-billing-history"></a>Können Benutzer des neuen Kontos auf den Nutzungs- und Abrechnungsverlauf zugreifen?

Die einzigen Informationen, die für Benutzer des neuen Kontos verfügbar sind, sind die Kosten für das Abonnement im letzten Monat. Der übrige Nutzungs- und Abrechnungsverlauf wird nicht zusammen mit dem Abonnement übertragen.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Wie migriere ich Daten und Dienste für mein Azure-Abonnement in ein neues Abonnement?

Wenn Sie ein Abonnement nicht übertragen können, können Sie Ihre Ressourcen manuell migrieren. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Wird meine Gutschrift mit dem Abonnement zum neuen Konto übertragen, wenn ich ein Abonnement für Visual Studio oder Microsoft Partner Network übertrage?

Nein. Ihre Gutschrift ist im neuen Konto nicht verfügbar. Der Benutzer, der die Übertragungsanforderung annimmt, muss eine Visual Studio-Lizenz haben, um die Übertragungsanforderung annehmen zu können. Für das Abonnement wird die Visual Studio-Gutschrift verwendet, die für das Konto des Benutzers verfügbar ist. Weitere Informationen finden Sie unter [Übertragen von Visual Studio- und Partner Network-Abonnements](#transfer-visual-studio-and-partner-network-subscriptions).


## <a name="frequently-asked-questions-faq-for-recipients"></a>Häufig gestellte Fragen (FAQ) bei der Übernahme

Diese häufig gestellten Fragen richten sich an Benutzer, die den Abrechnungsbesitz eines Azure-Abonnements von einem anderen Konto annehmen.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Wenn ich den Abrechnungsbesitz eines Abonnements von einem anderen Konto übernehme, haben dann die Benutzer dieses Konto weiterhin Zugriff auf meine Ressourcen?

Ja. Es kann aber sein, dass [Administratorrollen](add-change-subscription-administrator.md) und [Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-portal.md) entfernt werden. Der Zugriff geht verloren, wenn sich Ihr Konto in einem Azure AD-Mandanten befindet, bei dem es sich nicht um den Mandanten des Abonnements handelt, und der Benutzer, der die Übertragungsanforderung gesendet hat, das Abonnement in den Mandanten Ihres Kontos verschiebt. Führen Sie die folgenden Schritte aus, um Benutzer anzuzeigen, die über Azure-Rollenzuweisungen für den Zugriff auf Ressourcen im Abonnement verfügen:

1. Navigieren Sie [im Azure-Portal zur Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das Abonnement aus, das Sie überprüfen möchten, und wählen Sie dann **Zugriffssteuerung (IAM)** im linken Bereich aus.
1. Wählen Sie oben auf der Seite **Rollenzuweisungen** aus. Auf der Seite „Rollenzuweisungen“ werden alle Benutzer mit Zugriff auf das Abonnement aufgelistet.

Auch wenn die [Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-portal.md) während der Übertragung entfernt werden, haben Benutzer im ursprünglichen Besitzerkonto unter Umständen über andere Sicherheitsverfahren weiterhin Zugriff auf das Abonnement. Hierzu zählen:

* Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../../cloud-services/cloud-services-certs-create.md).
* Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../../storage/common/storage-create-storage-account.md).
* Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.

Der Empfänger sollte sich überlegen, ob er die dem Dienst zugeordneten geheimen Schlüssel aktualisiert, wenn er den Zugriff auf die Ressourcen einschränken möchte. Die meisten Ressourcen können durch Ausführen der folgenden Schritte aktualisiert werden:

  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
  2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus.
  3. Wählen Sie die Ressource.
  4. Wählen Sie auf der Ressourcenseite die Option **Einstellungen** aus. Hier können Sie die vorhandenen Schlüssel anzeigen und aktualisieren.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Muss ich den gesamten Abrechnungszyklus bezahlen, wenn ich ein Abonnement in der Mitte des Abrechnungszyklus übernehme?

Ihr Konto ist verantwortlich für die Bezahlung jeglicher Nutzung, die ab dem Zeitpunkt der Übertragung gemeldet wird. Möglicherweise gibt es Nutzung, die vor der Übertragung stattgefunden hat, aber danach gemeldet wurde. Diese Nutzung ist in der Abrechnung für Ihr Konto enthalten.

### <a name="can-i-use-a-different-payment-method"></a>Kann ich eine andere Zahlungsmethode verwenden?

Ja. Bei der Annahme der Übertragungsanforderung können Sie eine vorhandene Zahlungsmethode auswählen, die mit Ihrem Konto verknüpft ist, oder eine neue Zahlungsmethode hinzufügen.

### <a name="how-can-i-transfer-ownership-of-my-enterprise-agreement-ea-subscription-account-ownership-if-the-original-account-owner-is-no-longer-with-the-organization"></a>Wie kann ich den Kontobesitz meines EA-Abonnements (Enterprise Agreement) übertragen, wenn der ursprüngliche Kontobesitzer nicht mehr der Organisation angehört?

Der Unternehmensadministrator kann den Kontobesitz für beliebige Konten aktualisieren, auch wenn der ursprüngliche Kontobesitzer nicht mehr der Organisation angehört. Eine entsprechende Anleitung steht im EA-Portal unter [Übertragen des Kontobesitzes für alle Abonnements](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) zur Verfügung.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="why-dont-i-see-the-transfer-subscription-button"></a><a id="no-button"></a> Warum wird die Schaltfläche „Abonnement übertragen“ nicht angezeigt?

Die Self-Service-Abonnementübertragung ist für Ihr Abrechnungskonto nicht verfügbar. Derzeit wird die Übertragung des Abrechnungsbesitzes von Abonnements in Enterprise Agreement (EA)-Konten im Azure-Portal nicht unterstützt. Darüber hinaus wird die Übertragung des Abrechnungsbesitzes für Konten mit Microsoft-Kundenvereinbarung, die in Zusammenarbeit mit einem Microsoft-Vertreter erstellt werden, nicht unterstützt.

### <a name="why-doesnt-my-subscription-type-support-transfer"></a><a id="no-button"></a> Warum unterstützt mein Abonnementtyp die Übertragung nicht?

Nicht alle Abonnementtypen unterstützen die Übertragung des Abrechnungsbesitzes. Unter [Unterstützte Abonnementtypen](#supported-subscription-types) sind die Abonnementtypen aufgelistet, die eine Übertragung unterstützen.

### <a name="why-am-i-receiving-an-access-denied-error-when-i-try-to-transfer-billing-ownership-of-a-subscription"></a><a id="no-button"></a> Warum erhalte ich den Fehler „Zugriff verweigert“, wenn ich versuche, den Abrechnungsbesitz eines Abonnements zu übertragen?

Dieser Fehler wird angezeigt, wenn Sie versuchen, ein Abonnement für einen Microsoft Azure-Plan zu übertragen und nicht über die erforderliche Berechtigung verfügen. Um das Abonnement für einen Microsoft Azure-Plan zu übertragen, müssen Sie im Rechnungsabschnitt entweder Besitzer oder Mitwirkender sein, dem das Abonnement in Rechnung gestellt wird. Weitere Informationen finden Sie unter [Verwalten von Abonnements für den Rechnungsabschnitt](understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Überprüfen und aktualisieren Sie die Einstellungen für Dienstadministratoren, Co-Admins und Azure-Rollenzuweisungen. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](add-change-subscription-administrator.md) und [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).
