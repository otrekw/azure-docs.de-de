---
title: Erstellen eines Dynamics 365 Business Central-Angebots in Microsoft AppSource
description: Erfahren Sie, wie Sie ein Dynamics 365 for Business Central-Angebot in Microsoft AppSource erstellen. Sie können Ihr Angebot in AppSource oder über das CSP-Programm (Cloud Solution Provider) anbieten oder verkaufen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: navits09
ms.author: navits
ms.date: 12/02/2020
ms.openlocfilehash: 65df5f178ce0ec845fca6343c51049b6bf426e18
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107106036"
---
# <a name="create-a-dynamics-365-for-business-central-offer"></a>Erstellen eines Angebots für Dynamics 365 for Business Central

In diesem Artikel wird beschrieben, wie Sie ein neues Angebot für Dynamics 365 Business Central erstellen. [Microsoft Dynamics 365 Business Central](https://dynamics.microsoft.com/business-central) ist ein ERP-Dienst (Enterprise Resource Planning), mit dem eine Vielzahl von Geschäftsprozessen verarbeitet werden kann, z. B. für Finanzierung, operative Vorgänge, Lieferketten-, CRM- und Projektmanagement sowie E-Commerce. Premium-Pakete unterstützen außerdem das klassische Bereitstellungsmodell und die Fertigung. Alle Angebote für Dynamics 365 Business Central müssen unseren Zertifizierungsprozess durchlaufen.

Bevor Sie beginnen, [erstellen Sie ein Konto im kommerziellen Marketplace in Partner Center](../create-account.md), sofern Sie dies noch nicht getan haben. Vergewissern Sie sich, dass Ihr Konto im Programm „Kommerzieller Marketplace“ registriert ist.

>[!NOTE]
> Nach der Veröffentlichung eines Angebots werden Änderungen am Angebot nur in Partner Center und im Onlineshop aktualisiert, wenn Sie das Angebot erneut zur Veröffentlichung übermittelt haben.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Klicken Sie im linken Navigationsmenü auf **Kommerzieller Marketplace** > **Übersicht**.
3. Klicken Sie auf der Übersichtsseite auf **+ Neues Angebot** > **Dynamics 365 Business Central**.

    ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-dynamics-365-business-central.png)

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt.
- Die Angebots-ID darf zusammen mit der Herausgeber-ID nicht länger als 40 Zeichen lang sein.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen. Wenn die Herausgeber-ID z. B. `testpublisherid` lautet und Sie **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://appsource.microsoft.com/product/dynamics-365/testpublisherid.test-offer-1`.
- Diese ID kann nach dem Auswählen von **Erstellen** nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
- Dieser Name kann nach der Auswahl von **Erstellen** nicht mehr geändert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-setup"></a>Angebotseinrichtung

### <a name="alias"></a>Alias

Geben Sie einen beschreibenden Namen ein, mit dem ausschließlich in Partner Center auf dieses Angebot verwiesen wird. Dieser Name (vorausgefüllt mit Ihrer Eingabe bei der Erstellung des Angebots) wird nicht im Marketplace verwendet und unterscheidet sich von dem Angebotsnamen, der Kunden angezeigt wird. Wenn Sie den Angebotsnamen später ändern möchten, wechseln Sie zur Seite [Angebotsliste](#offer-listing).

### <a name="setup-details"></a>Details zur Konfiguration

Wählen Sie den **Pakettyp** aus, der für Ihr Angebot gilt:

- Eine **Add-On**-App erweitert die Oberfläche und vorhandene Funktionalität von Dynamics 365 Business Central. Weitere Informationen finden Sie unter [Add-On-Apps](/dynamics365/business-central/dev-itpro/developer/readiness/readiness-add-on-apps).
- Eine **Connect**-App kann in Szenarien verwendet werden, in denen eine Point-to-Point-Verbindung zwischen Dynamics 365 Business Central und einer Drittanbieterlösung oder einem Drittanbieterdienst hergestellt werden muss. Weitere Informationen finden Sie unter [Connect-Apps](/dynamics365/business-central/dev-itpro/developer/readiness/readiness-connect-apps).

Wählen Sie für **Wie sollen potenzielle Kunden mit diesem Angebot interagieren?** die Option aus, die Sie für dieses Angebot verwenden möchten.

- **Jetzt abrufen (kostenlos):** Listen Sie Ihr Angebot für Kunden als kostenlos auf.
- **Kostenlose Testversion (Listing):** Listen Sie Ihr Angebot für Kunden als kostenlos auf, und stellen Sie einen Link zu einer kostenlose Testversion bereit. Kostenlose Testversionen in der Angebotsliste werden von Ihrem Dienst erstellt, verwaltet und konfiguriert und weisen keine von Microsoft verwalteten Abonnements auf.

    > [!NOTE]
    > Die Token, die Ihre Anwendung über den Testlink erhält, können nur verwendet werden, um zum Automatisieren der Kontoerstellung in Ihrer App Benutzerinformationen aus Azure Active Directory (Azure AD) abzurufen. Die Authentifizierung mit diesem Token wird für Microsoft-Konten nicht unterstützt.

- **Kontakt mit mir aufnehmen:** Sammeln Sie über Ihr CRM-System (Customer Relationship Management) Kundenkontaktinformationen. Der Kunde wird gebeten, die Berechtigung zur Freigabe seiner Informationen zu erteilen. Diese Kundeninformationen sowie der Angebotsname, die Angebots-ID und der Marketplace, auf dem der Kunde Ihr Angebot gefunden hat, werden an das CRM-System gesendet, das Sie konfiguriert haben. Weitere Informationen zum Konfigurieren des CRM-Systems finden Sie unter [Kundenleads](#customer-leads).

### <a name="test-drive"></a>Testversion

Eine Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, das Produkt vor dem Kauf zu testen. Dies führt zu einer höheren Konversionsrate und zur Generierung von aussichtsreichen Leads. Um mehr zu erfahren, lesen Sie zunächst [Was ist die Testversion?](../what-is-test-drive.md)

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

### <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite können Sie die Kategorien und Branchen, die zum Zuordnen Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren.

### <a name="categories"></a>Kategorien

Wählen Sie Kategorien und Unterkategorien aus, um Ihr Angebot in den geeigneten Marketplace-Suchbereichen zu platzieren. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, inwiefern Ihr Angebot diesen Kategorien entspricht. Wählen Sie Folgendes:

- Mindestens eine und bis zu zwei Kategorien, einschließlich einer primären und einer sekundären Kategorie (optional).
- Bis zu zwei Unterkategorien für jede primäre und/oder sekundäre Kategorie. Wenn keine Unterkategorie auf Ihr Angebot anwendbar ist, wählen Sie **Nicht zutreffend** aus.

Eine vollständige Liste der Kategorien und Unterkategorien finden Sie unter [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md).

### <a name="industries"></a>Branchen

[!INCLUDE [Industry Taxonomy](includes/industry-taxonomy.md)]

### <a name="app-version"></a>App-Version

Geben Sie die Versionsnummer Ihres Angebots ein. Für Kunden wird diese Version auf der Detailseite des Angebots aufgeführt.

### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Stellen Sie hier Ihre eigenen Geschäftsbedingungen bereit. Sie können auch die Adresse angeben, unter der Ihre Geschäftsbedingungen zu finden sind. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihr Angebot testen können.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Auf dieser Seite können Sie Angebotsdetails wie Name, Beschreibung, Links und Kontakte definieren.

> [!NOTE]
> Geben Sie die Details eines Angebotseintrags nur in einer Sprache an. Eine Angabe in englischer Sprache ist nicht erforderlich, sofern die Angebotsbeschreibung mit dem Satz „Diese Anwendung ist nur auf [nicht englische Sprache] erhältlich“ beginnt. Es ist auch zulässig, eine *URL mit einem nützlichen Link* bereitzustellen, um Inhalte in einer anderen Sprache als der im Angebotseintrag verwendeten anzubieten.

Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen in Microsoft AppSource angezeigt werden (die aufgelisteten Preise dienen lediglich als Beispiele und stellen keine tatsächlichen Kosten dar):
<!-- update screen? -->
:::image type="content" source="media/example-d365-business-central.png" alt-text="Veranschaulicht, wie dieses Angebot in Microsoft AppSource angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Logo
2. Produkte
3. Kategorien
4. Supportadresse (Link)
5. Nutzungsbedingungen
6. Datenschutzrichtlinie
7. Angebotsname
8. Zusammenfassung
9. BESCHREIBUNG
10. Screenshots/Videos

### <a name="marketplace-details"></a>Marketplace-Details

Der hier eingegebene **Name** wird Kunden als Titel Ihres Angebotseintrags angezeigt. Dieses Feld ist bereits mit dem Text gefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

Geben Sie eine kurze Beschreibung Ihres Angebots ein (bis zu 100 Zeichen), die in der **Zusammenfassung der Suchergebnisse** verwendet werden kann. Diese Beschreibung kann in Marketplace-Suchergebnissen verwendet werden.

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

Sie können optional bis zu drei **Suchbegriffe** eingeben, die Kunden helfen, Ihr Angebot im Marketplace zu finden. Um optimale Ergebnisse zu erzielen, sollten Sie diese Schlüsselwörter auch in ihrer Beschreibung verwenden.

Wenn Sie den Kunden mitteilen möchten, **mit welchen Produkten Ihre App zusammenarbeitet**, geben Sie bis zu drei Produktnamen ein.

### <a name="helpprivacy-urls"></a>Hilfe-/Datenschutz-URLs

Geben Sie den **Hilfelink für Ihre App** (URL) ein, unter dem Kunden mehr über Ihr Angebot erfahren können. Ihre Hilfe-URL darf nicht mit der Support-URL identisch sein.

Geben Sie den **Link zur Datenschutzrichtlinie** (URL) Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="contact-information"></a>Kontaktinformationen

Geben Sie den Namen, die E-Mail-Adresse und die Telefonnummer eines **Supportkontakts** und eines **technischen Ansprechpartners** an. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und können an CSP-Partner weitergegeben werden.

Geben Sie im Abschnitt **Supportkontakt** die **Support-URL** an, unter der CSP-Partner Support für Ihr Angebot erhalten. Ihre Support-URL darf nicht mit der Hilfe-URL identisch sein.

### <a name="supporting-documents"></a>Unterstützende Dokumente

Geben Sie mindestens ein zugehöriges Marketingdokument (und höchstens drei) im PDF-Format an, z. B. Whitepapers, Broschüren, Checklisten oder Präsentationen.

### <a name="marketplace-media"></a>Marketplace-Medien

Stellen Sie Logos und Bilder bereit, die beim Anzeigen Ihres Angebots für Kunden verwendet werden. Alle Bilder müssen das PNG-Format aufweisen.

[!INCLUDE [logo tips](../includes/graphics-suggestions.md)]

>[!Note]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="logos"></a>Logos

Geben Sie eine PNG-Datei für das **große** Logo an. Dies wird in Partner Center verwendet, um die anderen erforderlichen Größen zu erstellen. Später können Sie dieses Logo optional durch ein anderes Bild ersetzen.

Diese Logos werden an unterschiedlichen Stellen in der Auflistung verwendet:

[!INCLUDE [logos-appsource-only](../includes/logos-appsource-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots"></a>Screenshots

Fügen Sie Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Mindestens drei Screenshots sind erforderlich, und Sie können bis zu fünf hinzufügen. Alle Screenshots müssen im PNG-Format in der Größe 1.280 × 720 Pixel vorliegen.

#### <a name="videos"></a>Videos

Optional können Sie bis zu vier Videos hinzufügen, die Ihr Angebot veranschaulichen. Videos müssen auf einer externen Website gehostet werden. Geben Sie für jedes Video den Namen, die Adresse und ein Miniaturbild (1280 × 720 Pixel) ein.

Weitere Ressourcen für Marketplace-Listen finden Sie unter [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="availability"></a>Verfügbarkeit

Auf dieser Seite können Sie definieren, wo und wie Ihr Angebot zur Verfügung gestellt werden soll.

### <a name="markets"></a>Märkte

Um die Märkte anzugeben, in denen Ihr Angebot verfügbar sein soll, wählen Sie **Märkte bearbeiten** aus, um das Popupfenster **Marktauswahl** anzuzeigen.

Wählen Sie mindestens einen Markt aus. Wählen Sie **Alle auswählen** aus, um Ihr Angebot in allen möglichen Märkten verfügbar zu machen, oder wählen Sie nur bestimmte Märkte aus, die Sie hinzufügen möchten. Wählen Sie abschließend **Speichern** aus.

Ihre hier getroffene Auswahl gilt nur für neue Käufe. Wenn Ihre App bereits von einem Benutzer in einem bestimmten Markt verwendet wird und Sie den Markt später entfernen, können Benutzer, die in diesem Markt bereits über Ihr Angebot verfügen, es weiterhin nutzen, aber Ihr Angebot ist nicht für neue Kunden in diesem Markt erhältlich.

> [!IMPORTANT]
> Es liegt in ihrer Verantwortung, alle lokalen rechtlichen Anforderungen zu erfüllen, auch wenn diese Anforderungen nicht hier oder im Partner Center aufgeführt sind. Auch wenn Sie alle Märkte auswählen, kann es sein, dass bestimmte Angebote in einigen Ländern und Regionen aufgrund lokaler Gesetze und Einschränkungen oder anderer Faktoren nicht gelistet werden.

### <a name="preview-audience"></a>Vorschauzielgruppe

Bevor Sie Ihr Angebot im Marketplace für die breite Öffentlichkeit live schalten, müssen Sie es zunächst für eine begrenzte **Vorschauzielgruppe** verfügbar machen. Geben Sie hier einen **Hide-Schlüssel** (eine beliebige Zeichenfolge, die nur Kleinbuchstaben und/oder Ziffern enthält) ein. Mitglieder Ihrer Vorschauzielgruppe können diesen Hide-Schlüssel als Token verwenden, um eine Vorschau Ihres Angebots im Marketplace anzuzeigen.

Wenn Sie anschließend bereit sind, Ihr Angebot verfügbar zu machen und die Vorschaueinschränkung zu entfernen, müssen Sie den **Hide-Schlüssel** entfernen und das Angebot erneut veröffentlichen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf dieser Seite werden die technischen Informationen definiert, die für das Herstellen einer Verbindung mit Ihrem Angebot verwendet werden. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte.

### <a name="file-upload"></a>Dateiupload

Wenn Sie zuvor **Add-On** ausgewählt haben, laden Sie dort die Paketdatei Ihres Angebots zusammen mit den Paketdateien für alle Erweiterungen hoch, für die sie Abhängigkeiten aufweist.

#### <a name="extension-package-file"></a>Erweiterungspaketdatei

Laden Sie die Erweiterungspaketdatei (APP-Datei) für Ihr Angebot hoch.

#### <a name="library-extension-package-file"></a>Bibliothekserweiterung-Paketdatei

Erforderlich, wenn Ihr Angebot zusammen mit einer anderen Erweiterung installiert werden muss, die nicht im Marketplace veröffentlicht wird. Wenn dies der Fall ist, laden Sie deren APP-Datei hier hoch.

>[!NOTE]
>Die Abhängigkeitspaketdatei wird nicht mehr verwendet. Laden Sie stattdessen eine Bibliothekserweiterung-Paketdatei hoch.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

<!-- ## Test drive technical configuration

This page lets you set up a demonstration ("test drive") that allows customers to try your offer before purchasing it. Learn more in [What is test drive](../what-is-test-drive.md).

To enable a test drive, select the **Enable a test drive** check box on the [Offer setup](#test-drive) tab. To remove test drive from your offer, clear this check box.

When you've finished setting up your test drive, select **Save draft** before continuing.
-->
## <a name="supplemental-content"></a>Ergänzender Inhalt

Auf dieser Seite können Sie zusätzliche Informationen bereitstellen, um uns bei der Validierung Ihres Angebots zu helfen. Diese Informationen werden weder den Kunden angezeigt noch im Marketplace veröffentlicht.

### <a name="target-release"></a>Zielrelease

Geben Sie an, auf welches Release von Microsoft Dynamics Business Central Ihre Lösung abzielt: die **aktuelle Version**, die **nächste Hauptversion** oder die **nächste Nebenversion**. Diese Informationen ermöglichen es uns, Ihre Lösung entsprechend zu testen.

### <a name="supported-editions"></a>Unterstützte Editionen

Wenn Ihr Angebot die Premium Edition von Microsoft Dynamics 365 Business Central erfordert, wählen Sie nur **Premium** aus. Wählen Sie andernfalls sowohl **Essentials** als auch **Premium** aus.

### <a name="key-usage-scenario"></a>Wichtige Verwendungsszenarien

Laden Sie eine PDF-Datei hoch, in der die wichtigsten Verwendungsszenarios für Ihr Angebot aufgeführt sind. Alle hier aufgeführten Szenarien können von unserem Validierungsteam überprüft werden, bevor wir Ihr Angebot für den Marketplace genehmigen.

### <a name="test-accounts"></a>Testkonten

Wenn unser Zertifizierungsteam ein Testkonto benötigt, um Ihr Angebot ordnungsgemäß zu überprüfen, laden Sie eine PDF-, DOC- oder DOCX-Datei mit den Informationen zu Ihren **Testkonten** hoch.

### <a name="app-tests-automation"></a>App-Testautomatisierung

Wenn es sich bei Ihrem Angebot um eine Add-On-App handelt, müssen Sie eine **App-Testautomatisierungsdatei** (.app) hochladen. Diese Datei ist für Connect-Apps nicht anwendbar.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="publish"></a>Veröffentlichen

### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Wenn Sie alle erforderlichen Abschnitte des Angebots ausgefüllt haben, wählen Sie rechts oben im Portal **Überprüfen und veröffentlichen** aus.

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
    - **Nicht gestartet**: Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
    - **Unvollständig**: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - **Vollständig**: Der Abschnitt ist vollständig. Alle erforderlichen Daten wurden angegeben, und es liegen keine Fehler vor. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam im Abschnitt **Hinweise zur Zertifizierung** Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Sie erhalten eine E-Mail von uns, wenn eine Vorschauversion des Angebots zur Überprüfung und Freigabe verfügbar ist. Kehren Sie zu Partner Center zurück, und wählen Sie **Live schalten** aus, um das Angebot für eine öffentliche Zielgruppe zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)