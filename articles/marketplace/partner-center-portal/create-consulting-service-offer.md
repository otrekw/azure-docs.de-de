---
title: Erstellen eines Beratungsdienstangebots – kommerzieller Microsoft-Marketplace
description: Erfahren Sie, wie Sie mit dem Partner Center ein Beratungsdienstangebot in Microsoft AppSource oder Azure Marketplace veröffentlichen.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: 626dc5a7d1cd52182e68eb0d217b2ac4c653330e
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2020
ms.locfileid: "82994355"
---
# <a name="create-a-consulting-service-offer"></a>Erstellen eines Beratungsdienstangebots

In diesem Artikel wird beschrieben, wie Sie ein Beratungsdienstangebot in [Microsoft AppSource](https://appsource.microsoft.com/) oder [Azure Marketplace](https://azuremarketplace.microsoft.com/) veröffentlichen. Beratungsdienstangebote auf Basis von Microsoft [Dynamics 365](https://dynamics.microsoft.com/) und Power Platform sollten in AppSource aufgelistet werden. Beratungsdienstangebote auf Basis von Microsoft Azure sollten im Azure Marketplace aufgelistet werden. Bevor Sie beginnen, [erstellen Sie ein Konto im kommerziellen Marketplace in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), sofern Sie dies noch nicht getan haben. Vergewissern Sie sich, dass Ihr Konto im Programm „Kommerzieller Marketplace“ registriert ist.

Bevor Sie Ihr Angebot erstellen, sollten Sie sich unter [Voraussetzungen für Beratungsdienst](https://docs.microsoft.com/azure/marketplace/partner-center-portal/consulting-service-prerequisites) über die Voraussetzungen informieren.

## <a name="publishing-benefits"></a>Vorteile der Veröffentlichung

Vorteile der Veröffentlichung im kommerziellen Marketplace:

- Bewerben Sie Ihr Unternehmen, indem Sie die Marke Microsoft nutzen.
- In AppSource können Sie mehr als 100 Millionen Office 365- und Dynamics 365-Benutzer erreichen und im Azure Marketplace über 200.000 Organisationen.
- Erhalten Sie hochwertige Leads in diesen Marketplaces.
- Lassen Sie Ihre Dienste durch die Außendienst- und Telefonmarketingteams von Microsoft bewerben.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Klicken Sie im linken Navigationsmenü auf **Kommerzieller Marketplace** > **Übersicht**.
3. Wählen Sie auf der Übersichtsseite die Option **+ Neues Angebot** > **Beratungsdienst** aus.

    ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-consulting-service.png)

>[!NOTE]
>Nach der Veröffentlichung eines Angebots werden die daran im Partner Center vorgenommenen Änderungen erst nach der erneuten Veröffentlichung des Angebots in der digitalen Ladenzeile angezeigt. Achten Sie darauf, nach dem Vornehmen von Änderungen immer erneut zu veröffentlichen.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID ist für Kunden in der Webadresse des Marketplace-Angebots sichtbar.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
- Nachdem Sie **Erstellen** ausgewählt haben, kann der Angebotsalias nicht mehr geändert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-setup"></a>Angebotseinrichtung

### <a name="connect-lead-management"></a>Einbinden der Leadverwaltung

Wenn Sie Ihr Angebot mithilfe des Partner Centers im Marketplace veröffentlichen, _müssen_ Sie es mit einem CRM (Customer Relationship Management)- oder Marketingautomatisierungssystem verbinden. Dadurch empfangen Sie Kontaktinformationen zum Kunden, sobald jemand sein Interesse an Ihrem Produkt bekundet oder es verwendet.

1. Wählen Sie **Verbinden** aus, um ein Ziel anzugeben, an das wir Kundenleads übermitteln sollen. Das Partner Center unterstützt die folgenden Systeme:

    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) for Customer Engagement
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Wenn Ihr CRM-System oben nicht aufgeführt ist, verwenden Sie eine [Azure-Tabelle](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) oder einen [HTTPS-Endpunkt](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https), um die Leaddaten von Kunden zu speichern, und exportieren Sie die Daten dann in Ihr CRM-System.

2. Verbinden Sie Ihr Angebot bei der Veröffentlichung im Partner Center mit dem Leadziel.
3. Vergewissern Sie sich, dass die Verbindung mit dem Leadziel ordnungsgemäß konfiguriert ist. Nach der Veröffentlichung Ihres Angebots im Partner Center überprüfen wir die Verbindung und senden Ihnen einen Testlead. Während Sie das Angebot vor der Liveschaltung als Vorschau anzeigen, können Sie Ihre Leadverbindung ebenfalls testen, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung zu kaufen.
4. Achten Sie darauf, dass die Verbindung mit dem Leadziel immer auf dem neuesten Stand ist, damit keine Leads verloren gehen.

Hier finden Sie einige zusätzliche Ressourcen zur Leadverwaltung:

- [Übersicht über die Leadverwaltung](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Häufig gestellte Fragen zur Leadverwaltung](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Häufige Leadkonfigurationsfehler](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#publishing-config-errors)
- [Übersicht über die Leadverwaltung](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) (PDF – vergewissern Sie sich, dass Ihr Popupblocker deaktiviert ist.)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="properties"></a>Eigenschaften

Auf dieser Seite können Sie das primäre Produkt festlegen, für das Ihr Beratungsdienst am besten geeignet ist, den Typ des Beratungsdiensts festlegen und die anwendbaren Produkte auswählen.

1. Wählen Sie ein **Primäres Produkt** aus der Dropdownliste aus.
2. Wählen Sie den **Typ des Beratungsdiensts** aus der Dropdownliste aus:

    - **Bewertung:** Eine Auswertung der Umgebung eines Kunden, um die Anwendbarkeit einer Lösung zu bestimmen und eine Schätzung der Kosten und des zeitlichen Umfangs aufzustellen
    - **Einweisung**: Eine Einführung in eine Lösung oder einen Beratungsdienst, um anhand von Frameworks, Demos und Kundenbeispielen das Kundeninteresse zu wecken.
    - **Implementierung**: Eine umfassende Installation, durch die eine vollständig funktionierenden Lösung erstellt wird. Sie sollten sich auf Lösungen beschränken, die in maximal zwei Wochen implementiert werden können.
    - **Proof of Concept**: Eine Implementierung in einem begrenzten Bereich, um zu bestimmen, ob eine Lösung die Anforderungen eines Kunden erfüllt
    - **Workshop**: Ein interaktives Angebot, das beim Kunden vor Ort durchgeführt wird. Dabei kann es sich um Schulungen, Einweisungen, Bewertungen oder Demos handeln, die auf den Daten oder der Umgebung des Kunden aufbauen.

3. Wenn Sie ein **Azure-Produkt** als primäres Produkt ausgewählt haben, wählen Sie bis zu drei **Lösungsbereiche** aus. Diese erleichtern es Kunden, Ihr Angebot in Azure Marketplace zu finden. Wenn Sie sich nicht für Azure entschieden haben, überspringen Sie diesen Schritt.
4. Wenn Sie ein _anderes_ als ein Azure-Produkt als primäres Produkt ausgewählt haben, wählen Sie bis zu drei **Anwendbare Produkte** aus. Diese erleichtern es Kunden, Ihr Angebot in AppSource zu finden. Ausführliche Informationen finden Sie unter [Richtlinien zum Listing von Microsoft AppSource-Beratungsdiensten](https://go.microsoft.com/fwlink/?LinkId=828734&amp;clcid=0x409) (PDF).
5. Wählen Sie bis zu sechs **Branchen** aus, für die das Angebot gilt. Diese erleichtern es Kunden, Ihr Angebot zu finden.
6. Fügen Sie bis zu drei **Kompetenzen** hinzu, die Ihr Unternehmen erworben hat und die in der Angebotsliste Ihres Beratungsdiensts angezeigt werden sollen. Es ist mindestens eine Kompetenz erforderlich, außer für Azure Expert MSPs und Azure Networking MSPs.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Hier definieren Sie die Angebotsdetails, die im Marketplace angezeigt werden. Dazu gehören der Name des Angebots, eine Beschreibung, Bilder usw. Achten Sie darauf, dass Sie beim Konfigurieren dieses Angebots die Richtlinien befolgen, die auf der [Seite mit Zertifizierungsrichtlinien für den kommerziellen Marketplace](https://docs.microsoft.com/legal/marketplace/certification-policies#800-consulting-services) beschrieben sind.

> [!NOTE]
> Eine Angabe der Angebotsdetails in englischer Sprache ist nicht erforderlich, sofern die Angebotsbeschreibung mit dem Satz &quot;Diese Anwendung ist nur auf [nicht englische Sprache] erhältlich&quot; beginnt. Es ist auch in Ordnung, einen Nützlichen Link mit Inhalten in einer anderen Sprache als der für die Details der Angebotsliste verwendeten bereitzustellen.

### <a name="name"></a>Name

Der Name, den Sie hier eingeben, wird als Titel Ihres Angebots angezeigt. In dieses Feld wurde bereits der Text eingefügt, den Sie beim Erstellen des Angebots im Feld **Angebotsalias** eingegeben haben. Sie können diesen Namen später ändern.

Der Name:

- Kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen).
- Darf nicht mehr als 50 Zeichen lang sein.
- Darf keine Emojis enthalten.

### <a name="search-results-summary"></a>Zusammenfassung der Suchergebnisse

Geben Sie eine kurze Beschreibung Ihres Angebots an. Sie kann bis zu 100 Zeichen lang sein und wird in den Marketplace-Suchergebnissen verwendet.

### <a name="description"></a>BESCHREIBUNG

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 3.000 Zeichen). Sie wird Kunden in der Übersicht der Marketplace-Auflistung angezeigt.

Schließen Sie eine oder mehrere der folgenden Informationen in die Beschreibung ein:

- Den Nutzen und die wichtigsten Vorteile Ihres Angebots
- Zuordnungen zu Kategorien oder Branchen oder beidem
- Möglichkeit zu In-App-Käufen
- Alle erforderlichen Bekanntmachungen

Hier einige Tipps zum Verfassen Ihrer Beschreibung:

- Schildern Sie in den ersten Sätzen Ihrer Beschreibung deutlich den Nutzen Ihres Angebots. Berücksichtigen Sie folgende Punkte:
  - Beschreibung des Angebots.
  - Typ von Benutzern, die von dem Produkt profitieren.
  - Kundenbedürfnisse oder -probleme, an die sich das Angebot richtet.
- Beachten Sie, dass die ersten Sätze möglicherweise in den Suchergebnissen angezeigt werden.
- Setzen Sie sich nicht nur auf Features und Funktionen, um Ihr Produkt zu verkaufen. Konzentrieren Sie stattdessen auf den Nutzen, den Ihr Angebot bietet.
- Verwenden Sie möglichst branchenspezifisches Vokabular oder nutzenorientierte Formulierungen.

Formatieren Sie Ihre Angebotsbeschreibung mithilfe des Rich-Text-Editors, um sie ansprechender zu gestalten.

![Verwenden des Rich-Text-Editors](./media/rich-text-editor.png)

| <center>Ändern des Textformats | <center>Hinzufügen von Aufzählungszeichen oder Nummerierungen | <center>Hinzufügen oder Entfernen von Texteinzügen |
| --- | --- | --- |
| <center>![Verwenden des Rich-Text-Editors zum Ändern des Textformats](./media/text-editor3.png) |  <center>![Verwenden des Rich-Text-Editors zum Hinzufügen von Listen](./media/text-editor4.png) |  <center>![Verwenden des Rich-Text-Editors für Einzüge](./media/text-editor5.png) |

### <a name="keywords"></a>Keywords

Geben Sie bis zu drei Suchbegriffe ein, die für Ihr primäres Produkt und Ihren Beratungsdienst relevant sind. Dadurch lässt sich Ihr Angebot leichter finden.

### <a name="duration"></a>Duration

Legen Sie die erwartete Dauer der Interaktion mit Ihrem Kunden fest.

### <a name="contact-information"></a>Kontaktinformationen

Sie müssen den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Hauptkontakt** und eine **Alternative Kontaktperson** angeben. Diese Informationen werden Kunden nicht angezeigt. Sie sind für Microsoft verfügbar und dürfen gegenüber CSP-Partnern (Cloud Solution Provider) angegeben werden.

### <a name="supporting-documents"></a>Unterstützende Dokumente

Fügen Sie bis zu drei unterstützende PDF-Dokumente für Ihr Angebot hinzu (mindestens ein Dokument ist erforderlich).

### <a name="marketplace-images"></a>Marketplace-Bilder

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Unscharfe Bilder werden abgelehnt.

>[!Note]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="store-logos"></a>Store-Logos

Stellen Sie PNG-Dateien für das Logo Ihres Angebots in jeder der folgenden Pixelgrößen bereit:

- **Klein (48 x 48)**
- **Groß (216 x 216)**

Alle Logos sind erforderlich und werden an verschiedenen Stellen in der Marketplace-Auflistung verwendet.

#### <a name="screenshots-optional"></a>Screenshots (optional)

Fügen Sie bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Jeder muss 1.280 x 720 Pixel groß sein und im PNG-Format vorliegen.

#### <a name="videos-optional"></a>Videos (optional)

Fügen Sie bis zu vier Videos hinzu, die Ihr Angebot vorstellen. Geben Sie den Namen des Videos, dessen Webadresse (URL) und ein Miniaturbild des Videos im PNG-Format mit einer Größe von 1280 x 720 Pixeln ein.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Hier definieren Sie Elemente wie Preise, den Markt und einen privaten Schlüssel.

1. **Markt**: Legen Sie den Markt fest, in dem Ihr Angebot verfügbar sein wird. Sie können nur einen Markt pro Angebot auswählen.
    1. Wählen Sie für die Märkte „USA“ oder „Kanada“ **Edit States** (Bundesstaaten bearbeiten) oder **Edit Provinces** (Provinzen bearbeiten) aus, um anzugeben, wo das Angebot verfügbar sein soll.
2. **Vorschauzielgruppe**: Konfigurieren Sie den **Ausblendschlüssel**, der verwendet wird, um die private Zielgruppe für Ihr Angebot festzulegen.
3. **Preise:** Geben Sie an, ob Ihr Angebot **Kostenlos** oder **Kostenpflichtig** ist.

    > [!NOTE]
    > Beratungsdienstangebote gelten nur für die Auflistung. Transaktionen erfolgen direkt außerhalb des kommerziellen Marketplace.

4. Geben Sie bei einem kostenpflichtigen Angebot neben dem **Preis und der Währung** an, ob es sich um einen **festen** oder **geschätzten** Preis handelt. Bei einem geschätzten Preis müssen Sie in der Beschreibung angeben, welche Faktoren sich auf den Preis auswirken.
5. Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="review-and-publish"></a>Überprüfen und veröffentlichen

Nachdem Sie alle erforderlichen Abschnitte des Angebots durchgearbeitet haben, können Sie es zum Überprüfen und Veröffentlichen einreichen.

1. Wenn Sie bereit sind, Ihr Beratungsdienstangebot zu veröffentlichen, klicken Sie auf **Überprüfen und veröffentlichen**.
2. Überprüfen Sie die Details auf der Seite für die abschließende Übermittlung.
3. Schreiben Sie ggf. eine Notiz an das Zertifizierungsteam, wenn Sie der Meinung sind, dass Details Ihres Angebots näher erläutert werden müssen.
4. Wählen Sie **Senden** aus, wenn Sie fertig sind.
5. Auf der Seite **Angebotsübersicht** wird angezeigt, in welcher Veröffentlichungsphase sich Ihr Angebot befindet.

Weitere Informationen dazu, wie lange Ihr Angebot zum Durchlaufen der einzelnen Veröffentlichungsphasen benötigt, finden Sie unter [Überprüfen des Veröffentlichungsstatus Ihres Commercial Marketplace-Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status).

## <a name="update-your-existing-consulting-service-offers"></a>Aktualisieren Ihrer bestehenden Beratungsdienstangebote

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
