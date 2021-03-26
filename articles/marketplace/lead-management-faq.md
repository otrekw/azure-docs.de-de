---
title: Problembehandlung und Fragen zur Leadverwaltung – Microsoft Partner Center
description: Hier erfahren Sie mehr über häufige Fehler und Fragen beim Konfigurieren von Leads im kommerziellen Marketplace in Partner Center.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 10/01/2020
ms.openlocfilehash: 681498073a5f111e3babd50443623589ffeec02f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92330919"
---
# <a name="common-questions-and-troubleshooting-for-lead-configuration"></a>Problembehandlung und häufig gestellte Fragen zur Leadkonfiguration

In diesem Artikel werden einige häufig gestellte Fragen zur Leadverwaltung für Ihre Angebote auf dem kommerziellen Marketplace beantwortet. Außerdem werden Fehler thematisiert, die auftreten können, wenn Sie Leads in Ihr System zur Verwaltung von Kundenbeziehungen (Customer Relationship Management, CRM) im Partner Center konfigurieren.

## <a name="common-questions-about-lead-management"></a>Häufige Fragen zur Leadverwaltung

#### <a name="where-can-i-get-help-in-setting-up-my-lead-destination"></a>Wo erhalte ich Hilfe bei der Einrichtung meines Leadziels?

Eine Übersicht dazu, wie Sie Ihr CRM-System mit Ihren Angeboten auf dem kommerziellen Marketplace verbinden können, finden Sie unter [Kundenleads aus Ihrem Angebot im kommerziellen Marketplace](partner-center-portal/commercial-marketplace-get-customer-leads.md). Wenn ein Fehler auftreten sollte, finden Sie weitere Informationen weiter unten in der Anleitung zur Problembehandlung. Sollten Sie zusätzliche Unterstützung benötigen, senden Sie ein Supportticket über die [Partner Center-Hilfe](https://aka.ms/marketplacepublishersupport). Klicken Sie dann auf **Offer creation** (Angebotserstellung) > **Your type of offer** (Ihr Angebotstyp) > **Lead management configuration** (Konfiguration der Leadverwaltung).

#### <a name="am-i-required-to-configure-a-lead-destination-in-order-to-publish-an-offer-in-the-commercial-marketplace"></a>Muss ich ein Leadziel konfigurieren, um ein Angebot im kommerziellen Marketplace zu veröffentlichen?

Dies hängt vom Typ des zu veröffentlichenden Angebots ab. Für Software-as-a-Service- und Dynamics 365 for Customer Engagement-Angebote wird die Option **Kontakt mit mir aufnehmen** verwendet, um alle Dynamics 365 for Operations-Angebote, alle Dynamics 365 Business Central-Angebote sowie alle Beratungsdienstangebote aufzulisten. Daher ist eine Verknüpfung mit einem Leadziel erforderlich. Wenn Ihr Angebotstyp hier nicht aufgeführt wurde, müssen Sie kein Leadziel dafür festlegen. Es wird dennoch empfohlen, ein Leadziel zu konfigurieren, damit Sie keine Geschäftschancen verpassen.

#### <a name="how-can-i-find-the-test-lead"></a>Wie finde ich den Testlead?

Suchen Sie in Ihrem Leadziel nach `"MSFT_TEST"`. Nachfolgend sehen Sie einen Beispieltestlead von Microsoft. Beachten Sie, dass das Format des Testleads abhängig vom Leadziel variiert.

```
{
    "userDetails": {
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

#### <a name="i-have-a-live-offer-but-why-am-i-not-seeing-any-leads"></a>Ich habe ein Liveangebot, aber warum werden mir keine Leads angezeigt?

Stellen Sie sicher, dass die Verbindung mit dem Leadziel gültig ist. Wir senden Ihnen einen Testlead, nachdem Sie in Partner Center für Ihr Angebot auf **Veröffentlichen** geklickt haben. Wenn der Testlead angezeigt wird, ist die Verbindung gültig. Sie können Ihre Leadverbindung auch testen, indem während des Vorschauschritts versuchen, eine Vorschau des Angebots zu generieren. Klicken Sie im Angebot im kommerziellen Marketplace auf **Jetzt holen**, **Kontakt mit mir aufnehmen** oder **Kostenlose Testversion**.

Stellen Sie außerdem sicher, dass Sie nach den richtigen Daten suchen. Erläuterungen zu den Leaddaten, die wir an Ihr Leadziel senden, finden Sie unter [Grundlegendes zu Leaddaten](partner-center-portal/commercial-marketplace-get-customer-leads.md).

#### <a name="i-configured-azure-blob-storage-as-my-lead-destination-but-why-dont-i-see-the-lead"></a>Ich habe Azure Blob Storage als Leadziel konfiguriert. Warum wird mir der Lead nicht angezeigt?

Azure Blob Storage wird nicht mehr als Leadziel unterstützt. Alle Kundenleads, die über ein solches Angebot generiert werden, gehen somit verloren. Wechseln Sie zu einer der anderen [Optionen für das Leadziel](partner-center-portal/commercial-marketplace-get-customer-leads.md). 

#### <a name="i-received-an-email-from-the-commercial-marketplace-but-why-cant-i-find-the-lead-in-my-crm"></a>Ich habe eine E-Mail vom Marketplace erhalten. Warum kann ich den Lead in meinem CRM-System nicht finden?

Es ist möglich, dass die E-Mail-Domäne des Endbenutzers die Endung „.edu“ aufweist. Aus Datenschutzgründen geben wir keine privaten personenbezogenen Informationen aus der Domäne „.edu“ weiter. Senden Sie ein Supportticket über die [Partner Center-Hilfe](https://aka.ms/marketplacepublishersupport).

#### <a name="i-configured-an-azure-table-as-my-lead-destination-how-can-i-view-the-leads"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Wie kann ich mir die Leads anzeigen lassen?

Sie können über das Azure-Portal auf die in der Azure-Tabelle gespeicherten Leaddaten zugreifen. Alternativ können Sie den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) kostenlos herunterladen und installieren, um die Tabellendaten Ihres Azure-Speicherkontos aufzurufen.

#### <a name="i-configured-an-azure-table-as-my-lead-destination-can-i-get-notified-whenever-a-new-commercial-marketplace-lead-is-sent"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Kann ich mich benachrichtigen lassen, wenn ein neuer Lead über den kommerziellen Marketplace gesendet wird?

Ja. Befolgen Sie die Anleitung unter [Konfigurieren der Leadverwaltung mit Azure-Tabellen](partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md), um einen Microsoft-Flow einzurichten, über den eine E-Mail gesendet wird, wenn der Azure-Tabelle ein Lead hinzugefügt wird.

#### <a name="i-configured-salesforce-as-my-lead-destination-but-why-cant-i-find-the-leads"></a>Ich habe Salesforce als Leadziel konfiguriert. Warum kann ich die Leads nicht finden?

Überprüfen Sie, ob das Web-to-Lead-Formular ein Pflichtfeld auf der Grundlage einer Auswahlliste ist. Wenn ja, ändern Sie das Feld in ein nicht obligatorisches Textfeld.

#### <a name="there-was-an-issue-with-my-lead-destination-and-i-missed-some-leads-can-i-have-them-sent-to-me-in-an-email"></a>Es gab ein Problem mit meinem Leadziel, und ich habe einige Leads verpasst. Kann ich sie mir in einer E-Mail senden lassen?

Aufgrund von Richtlinien zu personenbezogenen Informationen können wir Leadinformationen nicht über ungesicherte E-Mails weitergeben.

#### <a name="i-configured-an-azure-table-as-my-lead-destination-how-much-will-it-cost"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Wie viel wird das kosten?

Bei der Generierung von Leads entstehen wenig Daten. Bei fast allen Herausgebern handelt es sich um weniger als 1 GB. Die Kosten hängen von der Anzahl der empfangenen Leads ab. Wenn Sie z. B. 1.000 Leads innerhalb eines Monats empfangen, liegen die Kosten bei etwa 50 Cent. Weitere Informationen zu den Preisen finden Sie unter [Übersicht über die Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Wenn Ihre Frage darin nicht beantwortet wird, wenden Sie sich über die [Partner Center-Hilfe](https://aka.ms/marketplacepublishersupport) an den Microsoft-Support. Klicken Sie dann auf **Offer creation** (Angebotserstellung) > **Your type of offer** (Ihr Angebotstyp) > **Lead management configuration** (Konfiguration der Leadverwaltung).

#### <a name="im-receiving-email-notifications-when-new-customer-leads-are-received-how-can-i-configure-someone-else-to-receive-these-emails"></a>Ich erhalte E-Mail-Benachrichtigungen, wenn neue Kundenleads empfangen werden. Wie kann ich konfigurieren, dass jemand anderes diese E-Mails erhalten soll?

Öffnen Sie Ihr Angebot in Partner Center, navigieren Sie zur Seite **Angebotseinrichtung**, und klicken Sie auf **Leadverwaltung** > **Bearbeiten**. Aktualisieren Sie die E-Mail-Adressen im Feld **Kontakt-E-Mail**.

## <a name="troubleshooting-lead-configuration-errors"></a><a id="publishing-config-errors"></a> Behandeln von Problemen bei der Leadkonfiguration

**Lead konnte nicht in Dynamics CRM gespeichert werden. Überprüfen Sie die Einstellungen des Dynamics CRM-Kontos. LastCRMError: Anmeldung bei Dynamics CRM nicht möglich, LastCRMException:** 

> Wenn die Microsoft 365-Authentifizierung ausgewählt wurde, überprüfen Sie, ob Benutzerkonto und Kennwort gültig sind. Wenn Azure Active Directory ausgewählt wurde, überprüfen Sie, ob die Mandanten-ID, die Anwendungs-ID und der geheime Schlüssel für die Anwendung mit den für Azure Active Directory eingerichteten Werten übereinstimmen. Befolgen Sie die [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) aufgeführten Anweisungen. Wenn der Benutzername und das Kennwort des Kontos gültig sind, stellen Sie sicher, dass es Zugriff auf Dynamics 365 hat und eine Lizenz zugewiesen ist (Schritte 11-15 bei Verwendung von Azure Active Directory oder Sicherheitseinstellungen bei Verwendung eines Office-Benutzers). 

**Lead konnte nicht in Dynamics CRM gespeichert werden. Der Benutzer hat keine Erstellungsberechtigungen für das Attribut „leadsourcecode“ in der Leadentität**. 

> Der Anwendung bzw. dem Benutzer fehlen die Sicherheitsrollen für den Microsoft Marketplace-Leadschreibdienst. Befolgen Sie die Schritte 11–15, wenn Sie Azure Active Directory verwenden, oder [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) die Schritte unter „Sicherheitseinstellungen“, wenn Sie einen Office-Benutzer verwenden.

**Lead konnte nicht mit AAD in Dynamics CRM gespeichert werden. Ausnahme:: Mandant wurde nicht gefunden. Diese Instanz kann auftreten, wenn es für den Mandanten keine aktiven Abonnements gibt.**  

> Die im Abschnitt zur Leadverwaltung angegebene Verzeichnis-ID ist kein gültiges Verzeichnis. Rufen Sie die Verzeichnis-ID basierend auf den Anweisungen in Schritt 2 ab ([hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) unter Azure Active Directory).

**Lead konnte nicht in Dynamics CRM gespeichert werden. LastCRMError: Fehler bei SecLib::RetrievePrivilegeForUser – dem Benutzer sind keine Rollen zugewiesen.**  

> Lösung: Weisen Sie dem Microsoft Marketplace-Leadschreibdienst eine Sicherheitsrolle zu. Folgen Sie den Anweisungen [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md) unter Sicherheitseinstellungen.

**Lead konnte nicht mit AAD in Dynamics CRM gespeichert werden. Ausnahme:: Die Anwendung mit dem Bezeichner wurde im Verzeichnis nicht gefunden**. 

> Die im Abschnitt zur Leadverwaltung angegebene Anwendungs-ID ist kein gültiges Verzeichnis. Rufen Sie die Verzeichnis-ID basierend auf den Anweisungen in Schritt 8 ab (unter Azure Active Directory, von [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)). 

**Lead konnte nicht mit AAD in Dynamics CRM gespeichert werden. Ausnahme:: Angeforderter Mandantenbezeichner ist nicht gültig und weist kein gültiges externes Domänenformat auf**. 

> Die im Abschnitt zur Leadverwaltung angegebene Verzeichnis-ID ist kein gültiges Verzeichnis. Rufen Sie die Verzeichnis-ID basierend auf den Anweisungen in Schritt 2 ab (unter Azure Active Directory, von [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)). 

**Lead konnte nicht mit AAD in Dynamics CRM gespeichert werden. Ausnahme:: Fehler beim Überprüfen der Anmeldeinformationen: Ungültiges Clientgeheimnis bereitgestellt.** 

> Lösung: Melden Sie sich beim Azure-Portal an, und überprüfen Sie, ob der Anwendungsschlüssel mit der Angabe im Partner Center übereinstimmt. Generieren Sie das Kennwort basierend auf den Anweisungen in Schritt 10 (unter Azure Active Directory, von [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md)). 

**Lead konnte nicht in Dynamics CRM gespeichert werden. LastCRMError: Timeout beim Anforderungskanal beim Warten auf eine Antwort nach 00:02:00. Erhöhen Sie den Zeitlimitwert, der an den Aufruf der Anforderung übergeben wird, oder erhöhen Sie den SendTimeout-Wert bei der Binding. Der für diesen Vorgang zugewiesene Zeitraum war möglicherweise ein Teil eines längeren Zeitlimits.**  

> Lösung: Melden Sie sich beim Partner Center an, wählen Sie „Angebotseinrichtung“ > „Kundenleads“ > „URL“ aus, und überprüfen Sie, ob es sich um eine gültige Dynamic CRM-Instanz handelt.

