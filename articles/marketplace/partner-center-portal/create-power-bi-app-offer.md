---
title: Erstellen eines Power BI-App-Angebots – Azure Marketplace
description: Erfahren Sie, wie Sie ein Power BI-App-Angebot erstellen und in Microsoft AppSource veröffentlichen.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: 776311d6e6395cbe462f958bd8685fa0259e1fc2
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674857"
---
# <a name="create-a-power-bi-app"></a>Erstellen einer Power BI-App

> [!IMPORTANT]
> Wir verlagern die Verwaltung Ihrer Power BI-App-Angebote aus dem Cloud-Partnerportal in Partner Center. Befolgen Sie für die Verwaltung Ihrer Angebote bis zur erfolgten Migration die Anweisungen unter [Erstellen eines Power BI-App-Angebots](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/power-bi/cpp-create-offer) für das Cloud-Partnerportal.

In diesem Artikel wird beschrieben, wie Sie ein Power BI-App-Angebot erstellen und in Microsoft [AppSource](https://appsource.microsoft.com/) veröffentlichen.

Damit Sie ein Power BI-App-Angebot erstellen können, benötigen Sie ein Konto im kommerziellen Marketplace in Partner Center. Wenn Sie noch keines erstellt haben, lesen Sie [Erstellen eines Kontos im kommerziellen Marketplace in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account).

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.

    :::image type="content" source="media/power-bi-menu-overview.png" alt-text="Menü für Übersicht über den kommerziellen Marketplace" border="false":::

3. Wählen Sie **+ Neues Angebot** > **Power BI App** (Power BI-App) aus. Das Dialogfeld **Neues Angebot** wird angezeigt.

> [!IMPORTANT]
> Wenn die Option **Power BI App** (Power BI-App) nicht angezeigt wird oder nicht aktiviert ist, verfügt Ihr Konto nicht über die Berechtigung zum Erstellen dieses Angebotstyps. Überprüfen Sie, ob Sie alle [Anforderungen](create-power-bi-app-overview.md) für diesen Angebotstyp erfüllen (einschließlich der Registrierung für ein Entwicklerkonto).

### <a name="offer-id-and-alias"></a>Angebots-ID und Angebotsalias

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der zum Verweisen auf das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
- Dies kann nach dem Auswählen von **Erstellen** nicht mehr geändert werden.

Nachdem Sie diese beiden Werte eingegeben haben, wählen Sie **Erstellen** aus, um mit der Seite „Angebotsübersicht“ fortzufahren.

## <a name="offer-overview"></a>Angebotsübersicht

Die Seite **Angebotsübersicht** zeigt eine visuelle Darstellung der zum Veröffentlichen dieses Angebots erforderlichen Schritte (sowohl der abgeschlossenen als auch der noch ausstehenden) und der erwarteten Dauer ihrer Ausführung.

Sie enthält Links zum Durchführen von Vorgängen für dieses Angebot auf der Grundlage der von Ihnen ausgewählten Optionen. Beispiel:

- Wenn das Angebot einen Entwurf darstellt: [Delete draft offer](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer) (Angebotsentwurf löschen)
- Wenn es sich um ein Liveangebot handelt: [Stop selling the offer](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan) (Angebot nicht mehr verkaufen)
- Wenn das Angebot in der Vorschau ist: [Go-live](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval) (Live schalten)
- Wenn Sie die Abmeldung beim Herausgeber nicht abgeschlossen haben: [Cancel publishing](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing) (Veröffentlichung abbrechen)

## <a name="offer-setup"></a>Angebotseinrichtung

Führen Sie die unten angegebenen Schritte aus, um Ihr Angebot einzurichten.

### <a name="connect-lead-management"></a>Einbinden der Leadverwaltung

Wenn Sie Ihr Angebot mithilfe des Partner Centers im Marketplace veröffentlichen, müssen Sie es mit Ihrem CRM-System (Customer Relationship Management) verbinden. Dadurch empfangen Sie Kontaktinformationen zum Kunden, sobald jemand sein Interesse an Ihrem Produkt bekundet oder es verwendet.

1. Wählen Sie ein Leadziel aus, an das wir Kundenleads senden sollen. Das Partner Center unterstützt die folgenden CRM-Systeme:

    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) for Customer Engagement
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Wenn Ihr CRM-System oben nicht aufgeführt ist, verwenden Sie eine [Azure-Tabelle](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) oder einen [HTTPS-Endpunkt](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https), um die Leaddaten von Kunden zu speichern. Exportieren Sie die Daten dann in Ihr CRM-System.

2. Verbinden Sie Ihr Angebot bei der Veröffentlichung im Partner Center mit dem Leadziel.
3. Vergewissern Sie sich, dass die Verbindung mit dem Leadziel ordnungsgemäß konfiguriert ist. Nach der Veröffentlichung Ihres Angebots im Partner Center überprüfen wir die Verbindung und senden Ihnen einen Testlead. Während Sie das Angebot vor der Liveschaltung als Vorschau anzeigen, können Sie Ihre Leadverbindung ebenfalls testen, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung zu kaufen.
4. Achten Sie darauf, dass die Verbindung mit dem Leadziel immer auf dem neuesten Stand ist, damit keine Leads verloren gehen.

Hier finden Sie einige zusätzliche Ressourcen zur Leadverwaltung:

- [Übersicht über die Leadverwaltung](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Häufig gestellte Fragen zur Leadverwaltung](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Häufige Leadkonfigurationsfehler](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Übersicht über die Leadverwaltung](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) (PDF – vergewissern Sie sich, dass Ihr Popupblocker deaktiviert ist.)

Wählen Sie **Entwurf speichern** aus, bevor Sie mit dem nächsten Abschnitt fortfahren: Eigenschaften.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite können Sie die Kategorien und Branchen, die zum Gruppieren Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren.

### <a name="category"></a>Category

Wählen Sie mindestens eine und höchstens drei Kategorien aus. Diese Kategorien werden verwendet, um Ihr Angebot in den geeigneten Marketplace-Suchbereichen zu platzieren, und werden auf der Detailseite zu Ihrem Angebot angezeigt. Erläutern Sie in der Angebotsbeschreibung, inwiefern Ihr Angebot diesen Kategorien entspricht.

### <a name="industry"></a>Branche

Sie können optional bis zu zwei Branchen und zwei Sektoren in jeder Branche auswählen. Kategorien werden zum Anzeigen Ihres Angebots verwendet, Branchen und Sektoren dienen hingegen als Suchfilter und werden in der Storefront übernommen. Wenn Ihr Angebot auf eine bestimmte Branche und/oder einen Sektor ausgerichtet ist, sollten Sie in der Angebotsbeschreibung erläutern, wie Ihr Angebot die ausgewählten Branchen oder Sektoren unterstützt. Wenn Ihr Angebot nicht für eine bestimmte Branche bestimmt ist, lassen Sie diesen Abschnitt leer.

> [!NOTE]
> Wir arbeiten daran, neue Branchen und Sektoren einzuführen, um die Angebotsermittlung zu verbessern, daher sind einige Branchen oder Sektoren möglicherweise noch nicht in der Storefront präsent. Branchen und Sektoren mit einem Sternchen (*) werden zu einem späteren Zeitpunkt verfügbar sein. Alle veröffentlichten Angebote sind über die Schlüsselwortsuche auffindbar.
<p>&nbsp;

| **Branche** | **Unterbranche** |
| --- | --- |
| *Automobilbau | *Automobilbau |
| Landwirtschaft | *Sonstige – nicht segmentiert |
| Distribution | *Großhandel<br>Päckchen- und Paketversand |
| Fortbildung | *Hochschulbildung<br>* Grund- und Sekundarschulbildung/K-12<br>*Bibliotheken und Museen |
| Finanzdienstleistungen | *Bankwesen und Kapitalmärkte<br>* Versicherungswesen |
| Behörden | *Verteidigung und Geheimdienste (bisher als „Nationale und öffentliche Sicherheit“ bezeichnet)<br>* Öffentliche Sicherheit und Justizwesen<br>*Zivilregierung |
| Gesundheitswesen | *Kostenträger für Gesundheitssystem<br>* Gesundheitsdienstleister<br>*Pharmazeutika |
| Produktion und Ressourcen (bisher als „Fertigungsindustrie“ bezeichnet) | *Chemie und Agrochemie<br>* Diskrete Fertigung<br>*Energieversorgung |
| Einzelhandel und Konsumgüter (bisher als „Einzelhandel“ bezeichnet) | *Konsumgüter<br>* Händler |
| *Medien und Kommunikation (bisher als „Medien und Unterhaltung“ bezeichnet) | *Medien und Unterhaltung<br>* Telekommunikation |
| Dienstleistungsunternehmen | *Recht<br>* Professionelle Partnerdienstleistungen |
| *Architektur und Baugewerbe (bisher als „Architektur und Ingenieurwesen“ bezeichnet) | *Sonstige – nicht segmentiert |
| *Hotel- und Gaststättengewerbe, Tourismus | *Hotels und Freizeit<br>* Reisen und Transportwesen<br>*Restaurants und Gastronomie |
| *Andere Industriezweige im öffentlichen Sektor | *Forstwirtschaft und Fischerei<br>* Gemeinnützige Organisationen |
| *Immobilien | *Sonstige – nicht segmentiert |

### <a name="legal"></a>Rechtliche Hinweise

#### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Um eigene Geschäftsbedingungen anzugeben, geben Sie bis zu 10.000 Zeichen im Feld **Terms and Conditions** (Geschäftsbedingungen) ein. Wenn Ihre Geschäftsbedingungen eine längere Beschreibung erfordern, geben Sie einen einzelnen Weblink zu diesen ein. Er wird für Kunden als aktiver Link angezeigt.

Kunden müssen diesen Bestimmungen zustimmen, bevor sie Ihr Angebot testen können.

Wählen Sie **Entwurf speichern** aus, bevor Sie mit dem nächsten Abschnitt fortfahren, Angebotsliste.

## <a name="offer-listing"></a>Angebotsliste

Hier definieren Sie die Angebotsdetails, die im Marketplace angezeigt werden. Dazu gehören der Name des Angebots, eine Beschreibung, Bilder usw.

### <a name="language"></a>Sprache

Wählen Sie die Sprache aus, in der Ihr Angebot aufgeführt wird. Derzeit ist **Englisch (Vereinigte Staaten)** die einzige verfügbare Option.

Definieren Sie für jede Sprache/jeden Markt Marketplace-Informationen (z. B. Angebotsname, Beschreibung, Bilder usw.). Wählen Sie die Sprache bzw. den Namen des Markts aus, um diese Informationen anzugeben.

> [!NOTE]
> Eine Angabe der Angebotsdetails in englischer Sprache ist nicht erforderlich, sofern die Angebotsbeschreibung mit dem Satz „Diese Anwendung ist nur auf [nicht englische Sprache] erhältlich“ beginnt. Sie können auch einen nützlichen Link mit Inhalten in einer anderen Sprache bereitstellen als der Sprache, die für die Angebotsliste verwendet wird.

### <a name="name"></a>Name

Der Name, den Sie hier eingeben, wird als Titel Ihres Angebots angezeigt. Dieses Feld ist bereits mit dem Text ausgefüllt, den Sie beim Erstellen des Angebots im Feld **Angebotsalias** eingegeben haben. Sie können diesen Namen später ändern.

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

Um Ihre **Angebotsbeschreibung** ansprechender zu gestalten, formatieren Sie die Beschreibung mithilfe des Rich-Text-Editors. Mit dem Rich-Text-Editor können Sie Zahlen, Aufzählungspunkte, fette und kursive Formatierungen und Einzüge hinzufügen, um die Lesbarkeit Ihrer Beschreibung zu verbessern.

:::image type="content" source="media/power-bi-rich-text-editor.png" alt-text="Darstellung des Rich-Text-Editors" border="false":::

### <a name="search-keywords"></a>Suchbegriffe

Geben Sie bis zu drei optionale Suchbegriffe ein, die Kunden helfen, Ihr Angebot im Marketplace zu finden. Um optimale Ergebnisse zu erzielen, sollten Sie diese Schlüsselwörter auch in ihrer Beschreibung verwenden.

### <a name="helpprivacy-web-addresses"></a>Webadressen für Hilfe/Datenschutz

Stellen Sie Links bereit, um den Kunden das Verständnis Ihres Angebots zu erleichtern.

#### <a name="help-link"></a>Hilfelink

Geben Sie die Webadresse ein, unter der Kunden mehr über Ihr Angebot erfahren können.

#### <a name="privacy-policy-url"></a>URL zur Datenschutzrichtlinie

Geben Sie die Webadresse der Datenschutzrichtlinien Ihrer Organisation ein. Es liegt in Ihrer Verantwortung, sicherzustellen, dass Ihr Angebot den Gesetzen und Regelungen zum Datenschutz entspricht. Es liegt ferner in Ihrer Verantwortung, eine gültige Datenschutzrichtlinie auf Ihrer Website zu veröffentlichen.

### <a name="contact-information"></a>Kontaktinformationen

Sie müssen den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Supportkontakt** und einen **Technischen Ansprechpartner** angeben. Diese Informationen werden Kunden nicht angezeigt. Sie sind für Microsoft verfügbar und dürfen gegenüber CSP-Partnern (Cloud Solution Provider) angegeben werden.

- Supportkontakt (erforderlich): Für allgemeine Supportfragen.
- Technischer Ansprechpartner (erforderlich): Für technische Fragen und Zertifizierungsprobleme.
- CSP-Programm-Kontakt (optional): Für Fragen von Handelspartnern zum CSP-Programm.

Geben Sie im Abschnitt **Supportkontakt** die Webadresse der **Supportwebsite** an, auf der Partner Support für Ihr Angebot erhalten.

### <a name="supporting-documents"></a>Unterstützende Dokumente

Geben Sie mindestens ein und bis zu drei verwandte Marketingdokumente im PDF-Format an. Beispiele: Whitepaper, Broschüren, Checklisten oder Präsentationen.

### <a name="marketplace-images"></a>Marketplace-Bilder

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Unscharfe Bilder werden abgelehnt.

#### <a name="store-logos"></a>Store-Logos

Stellen Sie PNG-Dateien für das Logo Ihres Angebots in zwei Größen bereit: **Klein** (48 x 48 Pixel) und **Groß** (216 x 216 Pixel).

Beide Logos sind erforderlich und werden an verschiedenen Stellen in der Marketplace-Liste verwendet.

#### <a name="screenshots"></a>Screenshots

Fügen Sie mindestens einen und bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Jeder muss 1280 x 720 Pixel groß sein und im PNG-Format vorliegen.

#### <a name="videos-optional"></a>Videos (optional)

Fügen Sie optional bis zu fünf Videos hinzu, die Ihr Angebot vorstellen. Geben Sie den Namen des Videos, seine Webadresse und ein Miniaturbild des Videos im PNG-Format mit einer Größe von 1280 x 720 Pixel ein.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

Weitere Informationen zum Erstellen von Angebotslistungen finden Sie unter [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

## <a name="technical-configuration"></a>Technische Konfiguration

Stufen Sie Ihre App im Power BI Dienst zur Produktionsreife hoch, und stellen Sie den Link zum Power BI-App-Installationsprogramm bereit, mit dem Kunden Ihre App installieren können. Weitere Informationen finden Sie unter [Veröffentlichen von Apps mit Dashboards und Berichten in Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).

## <a name="supplemental-content"></a>Ergänzender Inhalt

Stellen Sie zusätzliche Informationen zu Ihrem Angebot bereit, um uns bei seiner Validierung zu helfen. Diese Informationen werden weder den Kunden angezeigt noch im Marketplace veröffentlicht.

### <a name="validation-assets"></a>Validierungsressourcen

Fügen Sie optional Anweisungen (bis zu 3.000 Zeichen) hinzu, um das Microsoft-Überprüfungsteam beim Konfigurieren und Testen Ihrer App sowie beim Herstellen einer Verbindung mit Ihrer App zu unterstützen. Fügen Sie typische Konfigurationseinstellungen, Konten, Parameter oder andere Informationen ein, die zum Testen der Option „Daten verbinden“ verwendet werden können. Diese Informationen werden nur dem Überprüfungsteam angezeigt und dienen nur zu Überprüfungszwecken.

## <a name="review-and-publish"></a>Überprüfen und veröffentlichen

Nachdem Sie alle erforderlichen Abschnitte des Angebots durchgearbeitet haben, können Sie es zum Überprüfen und Veröffentlichen einreichen.

Wählen Sie in der oberen rechten Ecke des Portals **Überprüfen und veröffentlichen** aus.

Auf der Überprüfungsseite haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots. Sie können erst veröffentlichen, wenn alle Abschnitte des Angebots als abgeschlossen gekennzeichnet sind.
  - **Nicht gestartet**: Die Überprüfung des Abschnitts wurde nicht gestartet und muss abgeschlossen werden.
  - **Unvollständig**: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Anweisungen dazu finden Sie in den Abschnitten weiter oben in diesem Dokument.
  - **Abgeschlossen**: Der Abschnitt enthält alle erforderlichen Daten, und es sind keine Fehler aufgetreten. Sie können das Angebot erst einreichen, wenn alle Abschnitte des Angebots abgeschlossen sind.
- Stellen Sie dem Zertifizierungsteam Testanweisungen zur Verfügung, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird. Stellen Sie außerdem alle ergänzenden Anmerkungen bereit, die das Verständnis Ihres Angebots unterstützen.

Um das Angebot zur Veröffentlichung einzureichen, wählen Sie **Veröffentlichen** aus.

Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots zur Überprüfung und Genehmigung verfügbar ist. Um Ihr Angebot für die Öffentlichkeit zugänglich zu machen (oder für eine private Zielgruppe, wenn es sich um ein privates Angebot handelt), navigieren Sie zum Partner Center, und wählen Sie **Live schalten** aus.
