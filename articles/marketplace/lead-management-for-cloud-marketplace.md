---
title: Leadverwaltung für den kommerziellen Marketplace | Azure Marketplace und AppSource
description: Eine Übersicht über verschiedene Themen zur Veröffentlichung von Angeboten und technischen Artefakten für Azure Marketplace und AppSource
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 04/14/2020
ms.openlocfilehash: e3924e609091f4d0975901024f20db74efb6aac5
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032706"
---
# <a name="lead-management-for-the-commercial-marketplace"></a>Leadverwaltung für den kommerziellen Marketplace

Kunden spielen für erfolgreiche Unternehmen eine entscheidende Rolle. Angesichts des Wandels beim Produktkauf sind Marketingfachleute darauf angewiesen, einen direkten Kontakt mit Kunden herzustellen und eine Beziehung aufzubauen. Aus diesem Grund stellt die Generierung hochwertiger Leads ein unverzichtbares Werkzeug für Ihren Vertriebszyklus dar. Nachdem Sie Ihr Angebot im [Partner Center](https://partner.microsoft.com/) gelistet haben, stehen Ihnen Tools zur Verfügung, mit denen Sie programmgesteuert Kundenkontaktinformationen erhalten können, sobald ein Kunde Interesse bekundet oder Ihr Produkt im Marketplace einsetzt. 

## <a name="what-are-leads-in-the-commercial-marketplace"></a>Was sind Leads im kommerziellen Marketplace?

Die Leads stammen von Kunden, die an Ihren Produkten im kommerziellen Marketplace interessiert sind oder diese bereitstellen. Nachdem Sie Ihr CRM mit Ihren Auflistungen im Partner Center verbunden haben, können Sie Leads von Azure Marketplace- und AppSource-Kunden erhalten.

## <a name="how-to-connect-your-crm-system-with-partner-center"></a>Vorgehensweise zum Verbinden Ihres CRM-Systems mit dem Partner Center

Der Leadverwaltungsconnector im Partner Center ist so konzipiert, dass er einfach mit Ihren CRM-Informationen in eine Liste der verfügbaren CRM-Systeme integriert werden kann. Nun können Sie ohne großen Entwicklungsaufwand für die Integration in ein externes System mühelos die vom Marketplace generierten Leads nutzen.

Nachfolgend finden Sie eine schrittweise Anleitung, wie Sie die einzelnen möglichen Leadziele verbinden:

**Dynamics CRM Online**: Anweisungen, wie Sie Dynamics CRM Online so konfigurieren, dass Sie Leads erhalten, finden Sie unter [Konfigurieren der Leadverwaltung für Dynamics 365 for Customer Engagement](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md).

**Marketo**: Anweisungen zum Einrichten der Marketo-Leadkonfiguration, sodass Sie Leads erhalten, finden Sie unter [Konfigurieren der Leadverwaltung in Marketo](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md).

**Salesforce**: Anweisungen zum Einrichten der Salesforce-Instanz, sodass Sie Leads erhalten, finden Sie unter [Konfigurieren der Leadverwaltung in Salesforce](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md).

**Azure-Tabelle**: Anweisungen zum Einrichten Ihres Azure-Speicherkontos, sodass Sie Leads in einer Azure-Tabelle erhalten, finden Sie unter [Konfigurieren der Leadverwaltung mit Azure-Tabellen](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md).

**HTTPS-Endpunkt**: Anweisungen zum Einrichten Ihres HTTPS-Endpunkts, sodass Sie Leads erhalten, finden Sie unter [Konfigurieren der Leadverwaltung zur Verwendung eines HTTPS-Endpunkts](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md).

Nachdem Sie Ihr Leadziel ordnungsgemäß konfiguriert haben und die Veröffentlichung Ihres Angebots anfordern, überprüfen wir die Verbindung und senden Ihnen einen Testlead zu. Wenn Sie das Angebot vor der Liveschaltung anzeigen, können Sie Ihre Leadverbindung ebenfalls testen, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung zu erwerben. Es muss unbedingt sichergestellt werden, dass Ihre Leadeinstellungen auf dem neuesten Stand sind, damit Sie keine Leads verlieren. Aktualisieren Sie daher immer diese Verbindungen, wenn sich bei Ihnen etwas geändert hat.

### <a name="what-are-the-next-steps"></a>Welche Schritte sind die nächsten?

Nach der technischen Einrichtung sollten Sie diese Leads in Ihre aktuelle Vertriebs- und Marketingstrategie sowie in Ihre Betriebsprozesse integrieren. Wir sind daran interessiert, einen Überblick über Ihren allgemeinen Vertriebsprozess zu erhalten, und möchten bei der Bereitstellung hochwertiger Leads und ausreichender Daten eng mit Ihnen zusammenarbeiten, um zu Ihrem Erfolg beizutragen. Wir freuen uns, wenn Sie uns Ihr Feedback mitteilen würden, wie wir die Leads, die wir Ihnen zusenden, durch zusätzliche Daten optimieren und verbessern können, um diesen Kunden zum Erfolg zu verhelfen. Wir freuen uns auf Ihr Feedback und Ihre Vorschläge. Nur so können wir Ihrem Vertriebsteam mithilfe von Marketplace-Leads zu größerem Erfolg verhelfen.

## <a name="common-lead-configuration-errors-during-publishing-in-partner-center"></a><a id="publishing-config-errors"></a> Häufige Leadkonfigurationsfehler bei Veröffentlichung im Partner Center

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

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Was sind Leads und warum sind sie für mich als Herausgeber auf dem Marketplace wichtig?** 

Leads sind Kunden, die Ihre Produkte über Marketplace bereitstellen. Ganz gleich, ob Ihr Produkt im [Azure Marketplace](https://azuremarketplace.microsoft.com) oder in [AppSource](https://appsource.microsoft.com/) aufgeführt wird – Sie erhalten Leads von Kunden, die an Ihrem Produkt interessiert sind, wenn Sie das Leadziel in Ihrem Angebot eingerichtet haben.  

**Wo erhalte ich Hilfe bei der Einrichtung meines Leadziels?** 

Eine Dokumentation finden Sie unter [Generieren von Kundenleads](./partner-center-portal/commercial-marketplace-get-customer-leads.md), oder senden Sie ein Supportticket unter [Hilfe und Support](https://aka.ms/marketplacepublishersupport). Wählen Sie Angebotstyp und Leadverwaltung aus. 

**Muss ich ein Leadziel konfigurieren, um ein Angebot im Marketplace zu veröffentlichen?**

Ja, wenn Sie eine „Contact Me SaaS-App“ oder Beratungsdienste veröffentlichen.  

**Wie kann ich bestätigen, dass die Leadkonfiguration richtig ist?**

Nachdem Sie Ihr Angebot und Ihr Leadziel eingerichtet haben, veröffentlichen Sie Ihr Angebot. Im Rahmen der Leadvalidierung sendet Marketplace einen Testlead an das in Ihrem Angebot konfigurierte Leadziel. 

**Wie finde ich den Testlead?**

Suchen Sie in Ihrem Leadziel nach „MSFT_TEST“. Beispiel für Testleaddaten: 

```text
company = MSFT_TEST_636573304831318844 

country = US 

description = MSFT_TEST_636573304831318844 

email = MSFT_TEST_636573304831318844@test.com

encoding = UTF-8 

encoding = UTF-8 

first_name = MSFT_TEST_636573304831318844 

last_name = MSFT_TEST_636573304831318844 

lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|\<Offer Name> 

oid = 00Do0000000ZHog 

phone = 1234567890 

title = MSFT_TEST_636573304831318844 
```

**Ich verfüge über ein Liveangebot, aber warum sehe ich keine Leads?**

Für jeden Lead werden Daten in Feldern des ausgewählten Leadziels übergeben. Die Leads werden in folgendem Format bereitgestellt: **Quellaktion|Angebot**

- *Quellen:*
  - AzureMarketplace
  - AzurePortal
  - TestDrive  
  - SPZA (Akronym für AppSource)

- *Aktionen:*
  - „INS“: Steht für „Installation“. Diese Aktion wird immer dann im Azure Marketplace oder in AppSource angezeigt, wenn ein Kunde auf die Schaltfläche zum Erwerb Ihres Produkts klickt.
  - „PLT“: Steht für „Partner Led Trial“ (partnergesteuerte Testversion). Diese Aktion wird immer dann in AppSource angezeigt, wenn ein Kunde auf die Schaltfläche „Kontakt mit mir aufnehmen“ klickt.
  - „DNC“: Steht für „Do Not Contact“ (Nicht kontaktieren). Diese Aktion wird immer dann in AppSource angezeigt, wenn ein Partner, der auf Ihrer App-Seite querverwiesen wurde, um eine Kontaktaufnahme gebeten wird. Wir benachrichtigen Sie vorab, dass dieser Kunde zu Ihrer App querverwiesen wurde, jedoch nicht kontaktiert werden muss.
  - „Erstellen“: Diese Aktion ist nur im Azure-Portal vorhanden und wird immer dann angezeigt, wenn ein Kunde mit seinem Konto Ihr Angebot erwirbt.
  - „StartTestDrive“: Diese Aktion gilt nur für Testversionen (Test Drives) und wird immer dann angezeigt, wenn ein Kunde seine Testversion startet.

- *Angebote:*
  - „checkpoint.check-point-r77-10sg-byol“,
  - „bitnami.openedxcypress“,
  - „docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a“

*Hier sind einige Beispieldaten der Kundeninformationen*

```json
{ 
"FirstName":"John",
"LastName":"Smith",
"Email":"jsmith@microsoft.com",
"Phone":"1234567890",
"Country":"US",
"Company":"Microsoft",
"Title":"CTO"
}
```

Weitere Informationen finden Sie unter [Leadinformationen](./partner-center-portal/commercial-marketplace-get-customer-leads.md). 

**Ich habe Azure BLOB als Leadziel konfiguriert. Warum sehe ich den Lead nicht?** 

Der Lead wird nur geschrieben, wenn Sie Azure Blob Storage als Leadziel auswählen. Wechseln Sie zur Azure-Tabelle, um die Leadechtzeit abzurufen.

**Ich habe eine E-Mail vom Marketplace erhalten. Warum kann ich den Lead in meinem CRM nicht finden?**  

Es ist möglich, dass die E-Mail-Domäne des Endbenutzers die Endung „.edu“ aufweist. Aus Datenschutzgründen geben wir keine personenbezogenen Informationen von EDU-Domänen weiter. Senden Sie ein Supportticket unter [Hilfe und Support](https://aka.ms/marketplacepublishersupport).

**Ich habe Azure-Tabelle/Azure BLOB als Leadziel konfiguriert. Wie kann ich die Leads anzeigen?** 

Sie können auf das Blob oder die Tabelle über das Azure-Portal zugreifen, oder Sie können [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) kostenlos herunterladen und installieren, um die Tabellen/Blobs Ihres Azure Storage-Kontos anzuzeigen. 

**Ich habe die Azure-Tabelle als Leadziel konfiguriert. Kann ich mich benachrichtigen lassen, wenn ein neuer Lead vom Marketplace gesendet wird?** 

Ja, befolgen Sie [hier](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md) die Anweisungen zur Einrichtung der Azure-Tabelle und -Funktion für die Dokumentation. 

**Ich habe Salesforce als Leadziel konfiguriert. Warum kann ich die Leads nicht finden?**

Überprüfen Sie, ob das Web-to-Lead-Formular ein Pflichtfeld auf Grundlage einer Auswahlliste ist. Ändern Sie das Feld in diesem Fall in ein nicht obligatorisches Textfeld.  
 
**Es gab ein Problem mit meinem Leadziel, und ich habe einige Leads verpasst. Kann ich sie mir in einer E-Mail zusenden lassen?** 

Aufgrund von Datenschutzrichtlinien können wir Leadinformationen nicht über ungesicherte E-Mails weitergeben. 

**Ich habe Azure Storage (BLOB/Tabelle) als Leadziel konfiguriert. Wie viel wird es kosten?** 

Daten für die Leadgenerierung sind niedrig (< 1 GB bei fast allen Herausgebern). Die Kosten hängen von der Anzahl der erhaltenen Leads ab. Wenn 1.000 Leads in einem Monat eingehen, kostet es etwa 50 Cent. 
