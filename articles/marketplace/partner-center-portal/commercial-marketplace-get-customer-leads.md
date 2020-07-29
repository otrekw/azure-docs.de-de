---
title: Leadverwaltung im kommerziellen Microsoft-Marketplace
description: Erfahren Sie, wie Sie Kundenleads zu Ihren Angeboten bei Microsoft AppSource und im Azure Marketplace generieren und empfangen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: keferna
ms.author: keferna
ms.date: 03/30/2020
ms.openlocfilehash: 9ee433f226b37c8ffd6ad466cca7cbd844d53524
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535976"
---
# <a name="customer-leads-from-your-commercial-marketplace-offer"></a>Kundenleads aus Ihrem Angebot im kommerziellen Marketplace

Leads sind Kunden, die an Ihren Angeboten bei [Microsoft AppSource](https://appsource.microsoft.com) oder im [Azure Marketplace](https://azuremarketplace.microsoft.com) interessiert sind oder diese bereitstellen. Leads werden generiert, indem Sie ein Angebot im kommerziellen Marketplace veröffentlichen. In diesem Artikel werden die folgenden Konzepte der Leadverwaltung erläutert:

* Wie Ihr Angebot im kommerziellen Marketplace Kundenleads generiert, damit Sie keine Geschäftschancen mehr verpassen 
* Wie Sie Ihr System für das Kundenbeziehungsmanagement (Customer Relationship Management, CRM) mit Ihrem Angebot verknüpfen, damit Sie Ihre Leads an einem zentralen Ort verwalten können
* Welche Daten Sie erhalten, um Leads nachverfolgen zu können, die Kontakt zu Ihnen aufgenommen haben

## <a name="generate-customer-leads"></a>Generieren von Kundenleads

Stellen, an denen Leads generiert werden:

- Ein Kunde klickt im Marketplace auf **Kontakt mit mir aufnehmen** und erklärt sich so mit der Weitergabe seiner Daten einverstanden. Dieser Lead wird als Lead mit *initialem Interesse* bezeichnet. Sie erhalten Informationen zu Kunden, die Interesse am Erwerb Ihres Produkts bekundet haben. Der Lead befindet sich an oberster Stelle des Verkaufstrichters.

    ![Dynamics 365 – Kontakt mit mir aufnehmen](./media/commercial-marketplace-get-customer-leads/dynamics-365-contact-me.png)

- Ein Kunde klickt auf **Jetzt holen** (oder auf **Erstellen** im [Azure-Portal](https://portal.azure.com/)), um Ihr Angebot zu beziehen. Dieser Lead ist ein *aktiver* Lead. Sie erhalten Informationen zu Kunden, die begonnen haben, Ihr Produkt bereitzustellen.

    ![Schaltfläche „Jetzt holen“ für SQL](./media/commercial-marketplace-get-customer-leads/sql-get-it-now.png)

    ![Schaltfläche „Erstellen“ für Windows Server](./media/commercial-marketplace-get-customer-leads/windows-server-create.png)

- Ein Kunde klickt auf **Testlaufwerk** oder **Kostenlose Testversion**, um Ihr Angebot auszuprobieren. (Kostenlose) Testversionen sind der optimale Weg, um Ihre Produkte umgehend und ohne Zugangsbeschränkungen für potenzielle Kunden zugänglich zu machen.

    ![Schaltfläche „Testlaufwerk“ für Dynamics 365](./media/commercial-marketplace-get-customer-leads/dynamics-365-test-drive.png)

    ![Schaltfläche „Kostenlose Testversion“ für Dynamics 365](./media/commercial-marketplace-get-customer-leads/dynamics-365-free-trial.png)

## <a name="connect-to-your-crm-system"></a>Verbinden mit Ihrem CRM-System

[!INCLUDE [Test drive content](./includes/connect-lead-management.md)]

## <a name="understand-lead-data"></a>Grundlegendes zu Leaddaten

Für jeden Lead, den Sie während des Kundengewinnungsprozesses empfangen, gibt es Daten in bestimmten Feldern. Das erste Feld, auf das Sie achten sollten, ist das Feld `LeadSource` in folgendem Format: **Quelle-Aktion** | **Angebot**.

**Quellen**: Der Wert für dieses Feld wird basierend auf dem Marketplace gefüllt, über den der Lead generiert wurde. Mögliche Werte sind `"AzureMarketplace"`, `"AzurePortal"` und `"AppSource (SPZA)"`.

**Aktionen**: Der Wert in diesem Feld spiegelt die Aktion wider, die der Kunden in dem Marketplace ausgeführt hat, über den der Lead generiert wurde.

Mögliche Werte:

- **"INS":** Steht für *Installation*. Diese Aktion erfolgt im Azure Marketplace oder in AppSource, wenn ein Kunde Ihr Produkt erwirbt.
- **"PLT":** Steht für *Partner Led Trial* (Über Partner angebotene Testversion). Diese Aktion findet in AppSource statt, wenn ein Kunde auf **Kontakt mit mir aufnehmen** klickt.
- **"DNC":** Steht für *Do Not Contact* (Nicht kontaktieren). Diese Aktion findet in AppSource statt, wenn ein Partner, zu dem es auf Ihrer App-Seite einen Querverweis gab, um eine Kontaktaufnahme gebeten wird. Sie werden benachrichtigt, dass auf Ihrer App-Seite auf den Kunden verwiesen wurde, dieser jedoch nicht kontaktiert werden muss.
- **"Create":** Diese Aktion findet nur im Azure-Portal statt, wenn ein Kunde über sein Konto Ihr Angebot erwirbt.
- **"StartTestDrive":** Diese Aktion gilt nur für die Option **Testlaufwerke** und wird generiert, wenn ein Kunde seine Testversion startet.

**Angebote**: Sie können mehrere Angebote im kommerziellen Marketplace veröffentlichen. Der Wert für dieses Feld wird basierend auf dem Angebot ausgefüllt, über das der Lead generiert wurde. Die Herausgeber-ID und die Angebots-ID werden in dieses Feld eingefügt. Dabei handelt es sich jeweils um die Werte, die Sie angegeben haben, als Sie das Angebot im Marketplace veröffentlicht haben.

Die folgenden Beispiele zeigen Werte im erwarteten Format `publisherid.offerid`: 

- `checkpoint.check-point-r77-10sg-byol`
- `bitnami.openedxcypress`
- `docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145`

## <a name="customer-information"></a>Kundeninformationen

Die Informationen des Kunden werden über mehrere Felder gesendet. Im folgenden Beispiel werden die Kundeninformationen gezeigt, die in einem Lead enthalten sind:

- Vorname: John
- Nachname: Smith
- E-Mail: jsmith\@Microsoft.com
- Telefon: 1234567890
- Land: US
- Unternehmen: Microsoft
- Titel: CTO

>[!NOTE]
>Nicht alle Daten in diesem Beispiel sind immer für jeden Leads verfügbar. Da Leads, wie im Abschnitt zu Kundenleads erwähnt, an mehreren Stellen generiert werden können, sollten Sie unbedingt die Datensätze deduplizieren und die Nachverfolgungen personalisieren. Auf diese Weise erhält jeder Kunde eine auf ihn zugeschnittene Mitteilung, und Sie bauen eine individuelle Beziehung auf.

## <a name="best-practices-for-lead-management"></a>Bewährte Methoden bei der Leadverwaltung

Im Anschluss finden Sie einige Empfehlungen für die Leadgenerierung in Ihrem Vertriebszyklus:

- **Vorgehensweise:** Definieren Sie einen klaren Vertriebsprozess mit Meilensteinen, KPIs und klarer Teamverantwortung.
- **Qualifizierung:** Definieren Sie die Voraussetzungen, die angeben, ob ein Lead vollständig qualifiziert wurde. Stellen Sie sicher, dass Vertriebs- oder Marketingmitarbeiter die Leads sorgfältig qualifizieren, bevor diese den vollständigen Vertriebsprozess durchlaufen.
- **Nachverfolgung:** Vergessen Sie die Nachverfolgung innerhalb von 24 Stunden nicht. Der Lead wird sofort nach der Bereitstellung der Testversion in das CRM Ihrer Wahl aufgenommen. Daraufhin sollten Sie ihm eine E-Mail senden, solange seine Eindrücke noch frisch sind. Fordern Sie die Planung eines Telefonats an, um besser zu verstehen, ob Ihr Produkt eine gute Lösung für sein Problem ist. Eine typische Transaktion erfordert eine Reihe von Folgeanrufen.
- **Pflege:** Pflegen Sie Ihre Leads, um langfristig eine höhere Gewinnspanne zu erzielen. Seien Sie engagiert, aber überrennen Sie sie nicht. Sie sollten den Leads noch einige E-Mails senden, bevor Sie aufgeben. Bleiben Sie ruhig eine Weile dran. Denken Sie daran: Diese Interessenten haben direkt mit Ihrem Produkt interagiert und Zeit mit einer kostenlosen Testversion verbracht; sie sind potenzielle Kunden erster Wahl.

## <a name="common-questions-about-lead-management"></a>Häufige Fragen zur Leadverwaltung

### <a name="where-can-i-get-help-in-setting-up-my-lead-destination"></a>Wo erhalte ich Hilfe bei der Einrichtung meines Leadziels?

Führen Sie die Schritte im Abschnitt [Verbinden mit Ihrem CRM-System](#connect-to-your-crm-system) aus, oder übermitteln Sie ein Supportticket über die [Partner Center-Hilfe](https://aka.ms/marketplacepublishersupport). Klicken Sie dann auf **Offer creation** (Angebotserstellung) > **Your type of offer** (Ihr Angebotstyp) > **Lead management configuration** (Konfiguration der Leadverwaltung).

### <a name="am-i-required-to-configure-a-lead-destination-in-order-to-publish-an-offer-in-the-commercial-marketplace"></a>Muss ich ein Leadziel konfigurieren, um ein Angebot im kommerziellen Marketplace zu veröffentlichen?

Dies hängt vom Typ des zu veröffentlichenden Angebots ab. Für Software-as-a-Service- und Dynamics 365 for Customer Engagement-Angebote wird die Option **Kontakt mit mir aufnehmen** verwendet, um alle Dynamics 365 for Operations-Angebote, alle Dynamics 365 Business Central-Angebote sowie alle Beratungsdienstangebote aufzulisten. Daher ist eine Verknüpfung mit einem Leadziel erforderlich. Wenn Ihr Angebotstyp hier nicht aufgeführt wurde, müssen Sie kein Leadziel dafür festlegen. Es wird dennoch empfohlen, ein Leadziel zu konfigurieren, damit Sie keine Geschäftschancen verpassen.

### <a name="how-can-i-find-the-test-lead"></a>Wie finde ich den Testlead?

Suchen Sie in Ihrem Leadziel nach `"MSFT_TEST"`. Nachfolgend sehen Sie einen Beispieltestlead von Microsoft. Beachten Sie, dass das Format des Testleads abhängig vom Leadziel variiert.

```
{
    "UserDetails": {
      "FirstName": "MSFT_TEST_636573304831318844",
      "LastName": "MSFT_TEST_636573304831318844",
      "Email": "MSFT_TEST_636573304831318844@test.com",
      "Phone": "1234567890",
      "Country": "US",
      "Company": "MSFT_TEST_636573304831318844",
      "Title": "MSFT_TEST_636573304831318844"
    },
    "LeadSource": "AzureMarketplace",
    "ActionCode": "INS",
    "OfferTitle": "Contoso Test"
    "Description": "MSFT_TEST_636573304831318844"
}
```

### <a name="i-have-a-live-offer-but-why-am-i-not-seeing-any-leads"></a>Ich habe ein Liveangebot, aber warum werden mir keine Leads angezeigt?

Stellen Sie sicher, dass die Verbindung mit dem Leadziel gültig ist. Wir senden Ihnen einen Testlead, nachdem Sie in Partner Center für Ihr Angebot auf **Veröffentlichen** geklickt haben. Wenn der Testlead angezeigt wird, ist die Verbindung gültig. Sie können Ihre Leadverbindung auch testen, indem während des Vorschauschritts versuchen, eine Vorschau des Angebots zu generieren. Klicken Sie im Angebot im kommerziellen Marketplace auf **Jetzt holen**, **Kontakt mit mir aufnehmen** oder **Kostenlose Testversion**.

Stellen Sie außerdem sicher, dass Sie nach den richtigen Daten suchen. Im Abschnitt [Grundlegendes zu Leaddaten](#understand-lead-data) dieses Artikels werden die Leaddaten beschrieben, die wir an Ihr Leadziel senden.

### <a name="i-configured-azure-blob-storage-as-my-lead-destination-but-why-dont-i-see-the-lead"></a>Ich habe Azure Blob Storage als Leadziel konfiguriert. Warum wird mir der Lead nicht angezeigt?

Azure Blob Storage wird nicht mehr als Leadziel unterstützt. Alle Kundenleads, die über ein solches Angebot generiert werden, gehen somit verloren. Wechseln Sie zu einer der anderen [Optionen für das Leadziel](./commercial-marketplace-get-customer-leads.md). 

### <a name="i-received-an-email-from-the-commercial-marketplace-but-why-cant-i-find-the-lead-in-my-crm"></a>Ich habe eine E-Mail vom Marketplace erhalten. Warum kann ich den Lead in meinem CRM-System nicht finden?

Es ist möglich, dass die E-Mail-Domäne des Endbenutzers die Endung „.edu“ aufweist. Aus Datenschutzgründen geben wir keine privaten personenbezogenen Informationen aus der Domäne „.edu“ weiter. Senden Sie ein Supportticket über die [Partner Center-Hilfe](https://aka.ms/marketplacepublishersupport).

### <a name="i-configured-an-azure-table-as-my-lead-destination-how-can-i-view-the-leads"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Wie kann ich mir die Leads anzeigen lassen?

Sie können über das Azure-Portal auf die in der Azure-Tabelle gespeicherten Leaddaten zugreifen. Alternativ können Sie den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) kostenlos herunterladen und installieren, um die Tabellendaten Ihres Azure-Speicherkontos aufzurufen.

### <a name="i-configured-an-azure-table-as-my-lead-destination-can-i-get-notified-whenever-a-new-commercial-marketplace-lead-is-sent"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Kann ich mich benachrichtigen lassen, wenn ein neuer Lead über den kommerziellen Marketplace gesendet wird?

Ja. Befolgen Sie die Anleitung unter [Konfigurieren der Leadverwaltung mit Azure-Tabellen](./commercial-marketplace-lead-management-instructions-azure-table.md), um einen Microsoft-Flow einzurichten, über den eine E-Mail gesendet wird, wenn der Azure-Tabelle ein Lead hinzugefügt wird.

### <a name="i-configured-salesforce-as-my-lead-destination-but-why-cant-i-find-the-leads"></a>Ich habe Salesforce als Leadziel konfiguriert. Warum kann ich die Leads nicht finden?

Überprüfen Sie, ob das Web-to-Lead-Formular ein Pflichtfeld auf der Grundlage einer Auswahlliste ist. Wenn ja, ändern Sie das Feld in ein nicht obligatorisches Textfeld.

### <a name="there-was-an-issue-with-my-lead-destination-and-i-missed-some-leads-can-i-have-them-sent-to-me-in-an-email"></a>Es gab ein Problem mit meinem Leadziel, und ich habe einige Leads verpasst. Kann ich sie mir in einer E-Mail senden lassen?

Aufgrund von Richtlinien zu personenbezogenen Informationen können wir Leadinformationen nicht über ungesicherte E-Mails weitergeben.

### <a name="i-configured-an-azure-table-as-my-lead-destination-how-much-will-it-cost"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Wie viel wird das kosten?

Bei der Generierung von Leads entstehen wenig Daten. Bei fast allen Herausgebern handelt es sich um weniger als 1 GB. Die Kosten hängen von der Anzahl der empfangenen Leads ab. Wenn Sie z. B. 1.000 Leads innerhalb eines Monats empfangen, liegen die Kosten bei etwa 50 Cent. Weitere Informationen zu den Preisen finden Sie unter [Übersicht über die Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Wenn Ihre Frage darin nicht beantwortet wird, wenden Sie sich über die [Partner Center-Hilfe](https://aka.ms/marketplacepublishersupport) an den Microsoft-Support. Klicken Sie dann auf **Offer creation** (Angebotserstellung) > **Your type of offer** (Ihr Angebotstyp) > **Lead management configuration** (Konfiguration der Leadverwaltung).

### <a name="im-receiving-email-notifications-when-new-customer-leads-are-received-how-can-i-configure-someone-else-to-receive-these-emails"></a>Ich erhalte E-Mail-Benachrichtigungen, wenn neue Kundenleads empfangen werden. Wie kann ich konfigurieren, dass jemand anderes diese E-Mails erhalten soll?

Öffnen Sie Ihr Angebot in Partner Center, navigieren Sie zur Seite **Angebotseinrichtung**, und klicken Sie auf **Leadverwaltung** > **Bearbeiten**. Aktualisieren Sie die E-Mail-Adressen im Feld **Kontakt-E-Mail**.

## <a name="next-steps"></a>Nächste Schritte

Nach der technischen Einrichtung sollten Sie diese Leads in Ihre aktuelle Vertriebs- und Marketingstrategie sowie in Ihre Betriebsprozesse integrieren. Wir möchten gern mehr über Ihren allgemeinen Vertriebsprozess erfahren und enger mit Ihnen zusammenarbeiten, um Ihnen hochwertige Leads und ausreichend Daten zu bieten, um Ihren Erfolg zu sichern. Wir freuen uns, wenn Sie uns Ihr Feedback mitteilen würden, wie wir die Leads, die wir Ihnen zusenden, durch zusätzliche Daten optimieren und verbessern können, um diesen Kunden zum Erfolg zu verhelfen. Wir freuen uns auf [Ihr Feedback und Ihre Vorschläge](mailto:AzureMarketOnboard@microsoft.com). Nur so können wir Ihrem Vertriebsteam mithilfe von Marketplace-Leads zu größerem Erfolg verhelfen.
