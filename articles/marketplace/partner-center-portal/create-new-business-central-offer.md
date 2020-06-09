---
title: Erstellen eines Angebots für Dynamics 365 Business Central im kommerziellen Microsoft-Marketplace
description: Hier lernen Sie die Schritte und Aspekte des Erstellens eines neuen Angebots für Dynamics 365 Business Central im kommerziellen Marketplace-Portal in Partner Center kennen. Sie können Ihr Angebot im Azure Marketplace oder über das CSP-Programm (Cloud Solution Provider) anbieten oder verkaufen.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 04de89624dd0e6857e96327bb408cf8700a1f6a2
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848880"
---
# <a name="create-a-dynamics-365-business-central-offer"></a>Erstellen eines Angebots für Dynamics 365 Business Central

In diesem Artikel wird erläutert, wie Sie ein neues Angebot für Dynamics 365 Business Central erstellen. [Microsoft Dynamics 365 Business Central](https://dynamics.microsoft.com/business-central) ist ein ERP-System (Enterprise Resource Planning), mit dem eine Vielzahl von Geschäftsprozessen verarbeitet werden kann, z. B. für Finanzierung, operative Vorgänge, Lieferketten-, CRM- und Projektmanagement sowie E-Commerce. Premium-Pakete unterstützen außerdem das klassische Bereitstellungsmodell und die Fertigung. Alle Angebote für Dynamics 365 Business Central müssen unseren Zertifizierungsprozess durchlaufen.

Bevor Sie beginnen, [erstellen Sie ein Konto im kommerziellen Marketplace in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), sofern Sie dies noch nicht getan haben. Vergewissern Sie sich, dass Ihr Konto im Programm „Kommerzieller Marketplace“ registriert ist.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Klicken Sie im linken Navigationsmenü auf **Kommerzieller Marketplace** > **Übersicht**.
3. Klicken Sie auf der Übersichtsseite auf **+ Neues Angebot** > **Dynamics 365 Business Central**.

    ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-dynamics-365-bc.png)

> [!NOTE]
> Nach der Veröffentlichung eines Angebots werden die daran im Partner Center vorgenommenen Änderungen erst nach der erneuten Veröffentlichung des Angebots in der digitalen Ladenzeile angezeigt. Achten Sie darauf, nach dem Vornehmen von Änderungen immer erneut zu veröffentlichen.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
- Nachdem Sie **Erstellen** ausgewählt haben, kann der Angebotsalias nicht mehr geändert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-setup"></a>Angebotseinrichtung

Führen Sie die unten angegebenen Schritte aus, um Ihr Angebot einzurichten.

### <a name="how-do-you-want-potential-customers-to-interact-with-this-listing-offer"></a>Wie sollen potenzielle Kunden mit diesem Angebot interagieren?

Wählen Sie die Option aus, die Sie für dieses Angebot verwenden möchten.

#### <a name="get-it-now-free"></a>Jetzt abrufen (kostenlos)

Listen Sie Ihr Angebot für Kunden als kostenlos, indem Sie eine gültige URL (beginnend mit *http* oder *https*) angeben, mit der die Kunden auf Ihre App zugreifen können.  Beispiel: `https://contoso.com/my-app`.

#### <a name="free-trial-listing"></a>Kostenlose Testversion (Listing)

Listen Sie Ihr Angebot für Kunden mit einem Link zu einer kostenlosen Testversion auf, indem Sie eine gültige URL (mit *http* oder *https* beginnend) angeben, über die auf die Testversion zugegriffen werden kann.  Beispiel: `https://contoso.com/trial/my-app`. Kostenlose Testversionen in der Angebotsliste werden von Ihrem Dienst erstellt, verwaltet und konfiguriert und weisen keine von Microsoft verwalteten Abonnements auf.

> [!NOTE]
> Die Token, die Ihre Anwendung über den Testlink erhält, können nur verwendet werden, um zum Automatisieren der Kontoerstellung in Ihrer App Benutzerinformationen aus Azure Active Directory (Azure AD) abzurufen. Die Authentifizierung mit diesem Token wird für Microsoft-Konten nicht unterstützt.

#### <a name="contact-me"></a>Kontakt mit mir aufnehmen

Sammeln Sie über Ihr CRM-System (Customer Relationship Management) Kundenkontaktinformationen. Der Kunde wird gebeten, die Berechtigung zur Freigabe seiner Informationen zu erteilen. Diese Kundeninformationen sowie der Angebotsname, die Angebots-ID und der Marketplace, auf dem der Kunde Ihr Angebot gefunden hat, werden an das CRM-System gesendet, das Sie konfiguriert haben. Weitere Informationen zum Konfigurieren des CRM-Systems finden Sie unter [Kundenleads](#customer-leads).

### <a name="test-drive"></a>Testversion

Die Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, vor dem Kauf einen Test durchzuführen. Dies führt zu einer höheren Konversionsrate und zur Generierung von aussichtsreichen Leads. Weitere Informationen zu Testversionen finden Sie [hier](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen. Konfigurieren Sie die Umgebung für die Testversion wie im Abschnitt [Technische Konfiguration der Testversion](#test-drive-technical-configuration) weiter unten in diesem Artikel beschrieben.

Weitere Informationen finden Sie unter [Ermöglichen Sie Ihren Kunden das Testen Ihres Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive).

#### <a name="type-of-test-drive"></a>Typ der Testversion

Sie können zwischen folgenden Optionen wählen:

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Hierbei handelt es sich um eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet.
- **[Dynamics 365 for Business Central](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft hostet und verwaltet den Testversionsdienst (einschließlich Bereitstellung) für ein Business Central-Planungssystem für Unternehmensressourcen (Finanzen, Betrieb, Lieferkette, CRM usw.).  
- **[Dynamics 365 for Customer Engagement](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft hostet und verwaltet den Testversionsdienst (einschließlich Bereitstellung) für ein Customer Engagement-System (Vertrieb, Kundendienst, Projektservice, Vor-Ort-Service usw.).  
- **[Dynamics 365 for Operations](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft hostet und verwaltet den Testversionsdienst (einschließlich Bereitstellung) für ein Finance and Operations-Planungssystem für Unternehmensressourcen (Finanzen, Betrieb, Fertigung, Lieferkette usw.). 
- **[Logik-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : Hierbei handelt es sich um eine Bereitstellungsvorlage, die alle komplexen Lösungsarchitekturen umfasst. Alle benutzerdefinierten Produkte sollten mit diesem Testversionstyp genutzt werden.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Hierbei handelt es sich um einen eingebetteten Link zu einem benutzerdefinierten Dashboard. Für Produkte, mit denen ein interaktives Power BI-Visual veranschaulicht werden soll, sollte dieser Testversionstyp verwendet werden. Sie müssen hier lediglich Ihre eingebettete Power BI-URL hochladen.

#### <a name="additional-test-drive-resources"></a>Zusätzliche Ressourcen zu Testversionen

- [Test Drive Technical Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte technische Methoden für Testversionen, in englischer Sprache)
- [Testversion: Marketing und Best Practices](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [PDF mit Übersicht zu Testversionen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (Stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite können Sie die Kategorien und Branchen, die zum Zuordnen Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren.

### <a name="category"></a>Category

Wählen Sie mindestens eine und höchstens drei Kategorien aus, die zum Platzieren Ihres Angebots in den entsprechenden Marketplace-Suchbereichen verwendet werden. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, wie Ihr Angebot diesen Kategorien entspricht. 

### <a name="industry"></a>Branche

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>App-Version

Geben Sie die Versionsnummer Ihres Angebots ein. Für Kunden wird diese Version auf der Detailseite des Angebots aufgeführt.

### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Geben Sie im Feld **Geschäftsbedingungen** Ihre eigenen rechtlichen Bestimmungen an. Sie können auch die URL angeben, unter der Ihre Geschäftsbedingungen zu finden sind. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihr Angebot testen können.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Auf der Seite können Sie Details für Ihr Angebot definieren, z. B. den Namen, eine Beschreibung und Bilder.

> [!NOTE]
> Die Details eines Angebotseintrags können nur in einer Sprache angegeben werden. Eine Angabe in englischer Sprache ist nicht erforderlich, sofern die Angebotsbeschreibung mit dem Satz „Diese Anwendung ist nur auf [nicht englische Sprache] erhältlich“ beginnt. Es ist auch zulässig, eine *Hilfelink-URL* bereitzustellen, um Inhalte in einer anderen als der im Angebotseintrag verwendeten Sprache anzubieten.

### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text gefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

### <a name="short-description"></a>Kurze Beschreibung

Geben Sie eine kurze Beschreibung Ihres Angebots an (bis zu 100 Zeichen). Diese Beschreibung kann in Marketplace-Suchergebnissen verwendet werden.

### <a name="description"></a>BESCHREIBUNG

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

### <a name="search-keywords"></a>Suchbegriffe

Sie können optional bis zu drei Suchbegriffe eingeben, die Kunden helfen, Ihr Angebot im Marketplace zu finden. Um optimale Ergebnisse zu erzielen, sollten Sie diese Schlüsselwörter auch in ihrer Beschreibung verwenden.

### <a name="products-your-app-works-with"></a>Produkte, mit denen Ihre App zusammenarbeitet

Wenn Sie den Kunden mitteilen möchten, dass Ihre App mit bestimmten Produkten zusammenarbeitet, geben Sie hier bis zu drei Produktnamen ein.

### <a name="helpprivacy-urls"></a>Hilfe-/Datenschutz-URLs

In diesem Abschnitt können Sie Links bereitstellen, die Kunden dabei helfen, mehr über Ihr Angebot zu erfahren.

#### <a name="help-link"></a>Hilfelink

Geben Sie die URL ein, unter der Kunden mehr über Ihr Angebot erfahren können. Ihr **Hilfelink** und Ihre **Support-URL** dürfen nicht identisch sein (siehe unten).

#### <a name="privacy-policy-link"></a>Link zur Datenschutzrichtlinie

Geben Sie die URL zur Datenschutzrichtlinie Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="contact-information"></a>Kontaktinformationen

In diesem Abschnitt müssen Sie den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Supportkontakt** und einen **Engineering-Kontakt** angeben. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und können CSP-Partnern bereitgestellt werden.

Im Abschnitt **Supportkontakt** müssen Sie auch die **Support-URL** angeben, unter der CSP-Partner Support für Ihr Angebot erhalten. Ihre Support-URL und Ihr **Hilfelink** dürfen nicht identisch sein.

### <a name="supporting-documents"></a>Unterstützende Dokumente

Geben Sie mindestens ein (und höchstens drei) zugehörige Marketingdokumente an, z. B. Whitepaper, Broschüren, Checklisten oder Präsentationen. Diese Dokumente müssen im PDF-Format vorliegen.

### <a name="marketplace-images"></a>Marketplace-Bilder

Stellen Sie Logos und Bilder für Ihr Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Laden Sie das Logo Ihres Angebots in zwei Größen hoch:

* **Klein** (48 x 48 Pixel)
* **Groß** (216 x 216 Pixel)

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst `https://upload.xboxlive.com` nicht blockiert.

#### <a name="screenshots"></a>Screenshots

Fügen Sie Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Mindestens drei Screenshots sind erforderlich, und Sie können bis zu fünf hinzufügen. Alle Screenshots müssen in der Größe 1280 x 720 Pixel vorliegen.

#### <a name="videos"></a>Videos

Optional können Sie bis zu fünf Videos hinzufügen, die Ihr Angebot veranschaulichen. Diese Videos sollten auf YouTube und/oder Vimeo gehostet werden. Geben Sie für jedes Video den Namen, seine URL und ein Miniaturbild (1280 x 720 Pixel) ein.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

[Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="availability"></a>Verfügbarkeit

Auf dieser Seite finden Sie Optionen, mit denen Sie festlegen können, wo und wie Sie Ihr Angebot zur Verfügung stellen.

### <a name="markets"></a>Märkte

In diesem Abschnitt können Sie angeben, in welchen Märkten das Angebot verfügbar sein soll. Wählen Sie hierzu **Edit markets** (Märkte bearbeiten) aus. Dadurch wird das Popupfenster **Marktauswahl** angezeigt.

Wählen Sie mindestens einen Markt aus, um Ihr Angebot zu veröffentlichen. Klicken Sie auf **Alle auswählen**, um Ihr Angebot in allen möglichen Märkten verfügbar zu machen, oder wählen Sie bestimmte Märkte aus, die Sie hinzufügen möchten.

Ihre hier getroffene Auswahl gilt nur für neue Käufe. Wenn Ihre App bereits von einem Benutzer in einem bestimmten Markt verwendet wird und Sie den Markt später entfernen, können Benutzer, die in diesem Markt bereits über Ihr Angebot verfügen, es weiterhin nutzen, aber Ihr Angebot ist nicht für neue Kunden in diesem Markt erhältlich.

> [!IMPORTANT]
> Es liegt in ihrer Verantwortung, alle lokalen rechtlichen Anforderungen zu erfüllen, auch wenn diese Anforderungen nicht hier oder im Partner Center aufgeführt sind.

Beachten Sie Folgendes: Auch wenn Sie alle Märkte auswählen, kann es sein, dass bestimmte Angebote in einigen Ländern und Regionen aufgrund lokaler Gesetze und Einschränkungen oder anderer Faktoren nicht gelistet werden.

### <a name="preview-audience"></a>Vorschauzielgruppe

Bevor Sie Ihr Angebot im Marketplace für die breite Öffentlichkeit live schalten, müssen Sie es zunächst für eine begrenzte **Vorschauzielgruppe** verfügbar machen. Geben Sie hier einen **Hide-Schlüssel** (eine beliebige Zeichenfolge, die nur Kleinbuchstaben und/oder Ziffern enthält) ein. Mitglieder Ihrer Vorschauzielgruppe können diesen Hide-Schlüssel als Token verwenden, um eine Vorschau Ihres Angebots im Marketplace anzuzeigen.

Wenn Sie anschließend bereit sind, Ihr Angebot verfügbar zu machen und die Vorschaueinschränkung zu entfernen, müssen Sie den **Hide-Schlüssel** entfernen und das Angebot erneut veröffentlichen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf dieser Seite werden die technischen Informationen definiert, die für das Herstellen einer Verbindung mit Ihrem Angebot verwendet werden. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte.

### <a name="package-type"></a>Pakettyp

Wählen Sie die Option aus, die für Ihr Angebot gilt:

* **Add-On**: Eine Add-On-App erweitert die Oberfläche und die vorhandene Funktionalität von Dynamics 365 Business Central. Weitere Informationen finden Sie unter [Add-On-Apps](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/developer/readiness/readiness-add-on-apps).
* **Connect**: Eine Connect-App kann in Szenarios verwendet werden, in denen eine Point-to-Point-Verbindung zwischen Dynamics 365 Business Central und einer Drittanbieterlösung oder einem Drittanbieterdienst hergestellt werden muss. Weitere Informationen finden Sie unter [Connect-Apps](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/developer/readiness/readiness-connect-apps).

### <a name="file-upload"></a>Dateiupload

Wenn Sie oben **Add-On** ausgewählt haben, laden Sie dort die Paketdatei Ihres Angebots zusammen mit den Paketdateien für alle Erweiterungen hoch, für die sie Abhängigkeiten aufweist.

#### <a name="extensions-package-file"></a>Erweiterungspaketdatei

Laden Sie die Erweiterungspaketdatei (APP-Datei) für Ihr Angebot hoch.

#### <a name="library-package-file"></a>Bibliothekspaketdatei

Erforderlich, wenn Ihr Angebot zusammen mit einer anderen Erweiterung installiert werden muss, die nicht im Marketplace veröffentlicht wird. Wenn dies der Fall ist, laden Sie deren APP-Datei hier hoch.

#### <a name="dependency-package-file"></a>Abhängigkeitspaketdatei

Erforderlich, wenn Ihr Angebot zusammen mit einer anderen Erweiterung installiert werden muss, die bereits im Marketplace veröffentlicht wurde. Wenn dies der Fall ist, laden Sie die zugehörige `.app`- oder `.zip`-Datei hier hoch.

### <a name="url-to-app-installation"></a>URL zur App-Installation

Wenn Sie oben **Connect** ausgewählt haben, geben Sie hier die URL-Adresse für Ihre App-Installation an. Bei verbundenen Diensten, die keine Installation erfordern, geben Sie die URL-Adresse für die Angebotsseite oder Registrierungsseite Ihres Diensts an.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="test-drive-technical-configuration"></a>Technische Konfiguration der Testversion

Auf dieser Seite können Sie eine Demonstration („Testversion“) einrichten, mit der Kunden Ihr Angebot vor dem Erwerb ausprobieren können. Erfahren Sie mehr im Artikel [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Aktivieren Sie auf der Registerkarte [Angebotseinrichtung](#test-drive) das Kontrollkästchen **Testversion aktivieren**, um eine Testversion zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Die folgenden Typen von Testversionen sind verfügbar, jeweils mit eigenen Anforderungen an die technische Konfiguration.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Logik-App](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (keine technische Konfiguration erforderlich)

Zusätzliche Ressourcen zu Testversionen:

- [Bewährte Marketingmethoden](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [PDF mit Übersicht](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (Stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Technical configuration for Azure Resource Manager test drive (Technische Konfiguration für Azure Resource Manager-Testversion)

Eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet. Erfahren Sie mehr über das Einrichten einer [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regionen** (erforderlich): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. In der Regel sollten Sie Ihre Testversion in den Regionen zur Verfügung stellen, in denen Sie die größte Anzahl von Kunden erwarten, damit sie für eine optimale Leistung die nächstgelegene Region auswählen können. Sie müssen sicherstellen, dass in Ihrem Abonnement alle Ressourcen in jeder von Ihnen ausgewählten Region bereitgestellt werden können.

- **Instanzen**: Wählen Sie den Typ („Heiß“ oder „Kalt“) und die Anzahl von verfügbaren Instanzen aus. Diese wird mit der Anzahl der Regionen multipliziert, in denen Ihr Angebot verfügbar ist.

    **Heiß**: Dieser Instanztyp wird bereits bereitgestellt, und auf ihn kann in jeder ausgewählten Region zugegriffen werden. Die Kunden haben sofort Zugriff auf Instanzen vom Typ *Heiß* und müssen nicht auf die Bereitstellung warten. Der Nachteil dabei ist, dass diese Instanzen immer unter Ihrem Azure-Abonnement ausgeführt werden, sodass höhere Betriebszeitkosten anfallen. Es wird dringend empfohlen, mindestens eine Instanz des Typs *Heiß* festzulegen, da die meisten Kunden nicht auf eine vollständige Bereitstellung warten möchten. Wenn keine Instanz vom Typ *Heiß* verfügbar ist, nimmt die Verwendung durch die Kunden ab.

    **Kalt**: Dieser Instanztyp stellt die Gesamtzahl von Instanzen dar, die pro Region bereitgestellt werden können. Bei Instanzen des Typs „Kalt“ muss die Bereitstellung der gesamten Resource Manager-Vorlage für die Testversion durchgeführt werden, wenn der Kunde die Testversion anfordert. Daher dauert das Laden von Instanzen des Typs *Kalt* weitaus länger als das Laden von Instanzen des Typs *Heiß*. Sie müssen jedoch nur für die Dauer der Testversion bezahlen. Instanzen vom Typ „Kalt“ werden *nicht* wie Instanzen vom Typ *Heiß* stets im Azure-Abonnement ausgeführt.

- **Azure Resource Manager-Vorlage für Testversion:** Laden Sie die ZIP-Datei mit Ihrer Azure Resource Manager-Vorlage hoch.  Im Schnellstartartikel [Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) erhalten Sie weitere Informationen zum Erstellen einer Azure Resource Manager-Vorlage.

- **Dauer der Testversion** (erforderlich): Geben Sie die Dauer in Stunden ein, für die die Testversion aktiv bleiben soll. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet. Verwenden Sie nur ganze Zahlen (z. B. „2“ Stunden ist zulässig, „1,5“ hingegen nicht).

### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Technical configuration for Dynamics 365 test drive (Technische Konfiguration für Dynamics 365-Testversion)

Microsoft kann mit diesem Typ von Testversion die Dienstbereitstellung hosten und verwalten, um das Einrichten einer Testversion zu vereinfachen. Die Konfiguration für diesen Typ von gehosteter Testversion ist gleich, unabhängig davon ob die Testversion für Business Central, Customer Engagement oder Operations vorgesehen ist.

- **Maximale Anzahl gleichzeitig ausgeführter Testversionen** (erforderlich): Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Jeder gleichzeitige Benutzer nutzt eine Dynamics 365-Lizenz, während die Testversion aktiv ist. Daher müssen Sie sicherstellen, dass genügend Lizenzen für die festgelegte maximale Anzahl verfügbar sind. Empfohlener Wert liegt zwischen 3 und 5.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum ein, in dem die Testversion aktiv bleibt, indem Sie die Anzahl der Stunden festlegen. Nach Ablauf dieser Anzahl von Stunden wird die Sitzung beendet, und sie belegt nicht mehr eine Ihrer Lizenzen. Der empfohlene Wert beträgt 2 bis 24 Stunden, abhängig von der Komplexität Ihres Angebots. Diese Dauer kann nur als ganze Anzahl von Stunden festgelegt werden (z. B. „2“ Stunden, die Angabe „1,5“ ist nicht zulässig).  Der Benutzer kann eine neue Sitzung anfordern, wenn die Zeit abgelaufen ist und er erneut auf die Testversion zugreifen möchte.

- **Instanz-URL** (erforderlich): Geben Sie die URL an, über die der Kunde die Testversion startet. Dies ist in der Regel die URL Ihrer Dynamics 365-Instanz, auf der Ihre App mit installierten Beispieldaten ausgeführt wird (z. B. `https://testdrive.crm.dynamics.com`).

- **Web-API-URL der Instanz** (erforderlich): Rufen Sie die Web-API-URL für Ihre Dynamics 365-Instanz ab, indem Sie sich bei Ihrem Microsoft 365-Konto anmelden, zu **Einstellungen** \&gt; **Anpassung** \&gt; **Entwicklerressourcen** \&gt; **Instanz-Web-API (Stamm-URL des Diensts)** navigieren und die dort angegebene URL kopieren (z. B. `https://testdrive.crm.dynamics.com/api/data/v9.0`).

- **Rollenname** (erforderlich): Geben Sie den Namen der Sicherheitsrolle an, den Sie in der benutzerdefinierten Dynamics 365-Testversion definiert haben. Dieser wird dem Benutzer für die Dauer der Testversion zugewiesen (z. B. „Testversionsrolle“).

### <a name="technical-configuration-for-logic-app-test-drive"></a>Technical configuration for Logic app test drive (Technische Konfiguration für Logik-App-Testversion)

Für alle benutzerdefinierten Produkte sollte dieser Typ von Testversionsbereitstellungsvorlage, der vielfältige komplexe Lösungsarchitekturen umfasst, verwendet werden. Um weitere Informationen über das Einrichten von Logik-App-Testversionen zu erhalten, besuchen Sie [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) und [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) auf GitHub (in englischer Sprache).

- **Region** (erforderlich; Dropdownliste für Einfachauswahl): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. Die Ressourcen für Ihre Logik-App werden in der von Ihnen gewählten Region bereitgestellt. Wenn die Logik-App benutzerdefinierte Ressourcen umfasst, die in einer bestimmten Region gespeichert sind, müssen Sie diese Region hier auswählen. Dazu empfiehlt es sich, die Logik-App lokal in Ihrem Azure-Abonnement im Portal vollständig bereitzustellen und zu überprüfen, ob sie ordnungsgemäß ausgeführt wird, bevor Sie diese Auswahl vornehmen.

- **Maximale Anzahl gleichzeitig ausgeführter Testversionen** (erforderlich): Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Diese Testversionen sind bereits bereitgestellt, sodass Kunden sofort auf sie zugreifen können, ohne auf eine Bereitstellung warten zu müssen.

- **Dauer der Testversion** (erforderlich): Geben Sie die Dauer in Stunden ein, für die die Testversion aktiv bleiben soll. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet.

- **Name der Azure-Ressourcengruppe** (erforderlich): Geben Sie den Namen der [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) ein, in der die Testversion für die Logik-App gespeichert ist.

- **Azure logic app name** (Name der Azure-Logik-App) (erforderlich): Geben Sie den Namen der Logik-App ein, von der die Testversion dem Benutzer zugewiesen wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

- **Deprovision logic app name** (Name der Logik-App für die Aufhebung der Bereitstellung) (erforderlich): Geben Sie den Namen der Logik-App ein, mit der die Bereitstellung der Testversion aufgehoben wird, wenn sie vom Kunden nicht mehr verwendet wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Technical configuration not required for Power BI test drives (Power BI-Testversionen erfordern keine technische Konfiguration)

Für Produkte, mit denen ein interaktives Power BI-Visual veranschaulicht werden soll, kann ein eingebetteter Link zum Freigeben eines benutzerdefinierten Dashboards als Testversion verwendet werden. Deshalb ist keine technische Konfiguration erforderlich. Erfahren Sie mehr über das Einrichten von [Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)-Vorlagen-Apps.

### <a name="deployment-subscription-details"></a>Abonnementdetails für Bereitstellung der Testversion

Um die Testversion in Ihrem Namen bereitzustellen, erstellen Sie ein separates eindeutiges Azure-Abonnement, und stellen Sie es bereit. (Power BI-Testversionen nicht erforderlich).

- **Azure-Abonnement-ID** (erforderlich für den Azure Resource Manager und Logic Apps): Geben Sie die Abonnement-ID ein, um für Ressourcennutzungsberichte und Abrechnungszwecke Zugriff auf die Dienste Ihres Azure-Kontos zu gewähren. Es wird empfohlen, [ein eigenes Azure-Abonnement zu erstellen](https://docs.microsoft.com/azure/billing/billing-create-subscription), das für Testversionen verwendet wird, falls Sie es noch nicht getan haben. Sie finden die Azure-Abonnement-IDs, indem Sie sich beim [Azure-Portal](https://portal.azure.com/) anmelden und im Menü auf der linken Seite zu den **Abonnements** navigieren. Durch Auswahl der Registerkarte wird Ihre Abonnement-ID angezeigt (z. B. a83645ac-1234-5ab6-6789-1h234g764ghty).

- **ID des Azure AD-Mandanten** (erforderlich): Geben Sie Ihre [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) für Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ und dann „Eigenschaften“ aus, und suchen Sie die aufgeführte Nummer der **Verzeichnis-ID** (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe der Adresse eines Domänennamens nachschlagen.

- **Azure AD tenant name** (Name des Azure AD-Mandanten) (erforderlich für Dynamics 365): Geben Sie den Namen Ihrer Azure Active Directory-Instanz (AD) ein. Um diesen Namen zu suchen, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Oben rechts wird der Mandantenname unter Ihrem Kontonamen angegeben.

- **Azure AD-App-ID** (erforderlich): Geben Sie Ihre [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) für Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ und dann **App-Registrierungen** aus, und suchen Sie die aufgeführte Nummer der **Anwendungs-ID** (z. B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD app client secret** (Geheimer Clientschlüssel der Azure AD-App) (erforderlich): Geben Sie den [geheimen Clientschlüssel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) Ihrer Azure AD-Anwendung ein. Um diesen Wert zu finden, melden Sie sich beim [Azur-Portal](https://portal.azure.com/) an. Wählen Sie im linken Menü die Registerkarte **Azure Active Directory** aus, wählen Sie **App-Registrierungen** aus und dann Ihre Testversion-App. Wählen Sie als Nächstes **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, geben Sie eine Beschreibung ein, wählen Sie **Nie** unter **Läuft ab** aus, und wählen Sie dann **Hinzufügen** aus. Stellen Sie sicher, dass Sie den Wert kopieren. (Navigieren Sie nicht von der Seite weg, bevor Sie dies tun, sonst haben Sie keinen Zugriff auf den Wert.)

### <a name="test-drive-marketplace-listings"></a>Testversionen für Marketplace-Auflistungen

Auf der Registerkarte **Testversion** können Sie mithilfe der Option **Marketplace-Auflistung** Details für die Oberfläche der Testversion definieren.

> [!NOTE]
> Die Details einer Testversionsauflistung können nur in einer Sprache angegeben werden. Eine Angabe in englischer Sprache ist nicht erforderlich, sofern die Angebotsbeschreibung mit dem Satz „Diese Anwendung ist nur auf [nicht englische Sprache] erhältlich“ beginnt. Es ist auch zulässig, eine *Hilfelink-URL* bereitzustellen, um Inhalte in einer anderen als der im Inhalt der Testversionsauflistung verwendeten Sprache anzubieten.

- **Beschreibung** (erforderlich): Beschreiben Sie Ihre Testversion. Geben Sie an, was in der Demonstration enthalten ist, nennen Sie die Ziele, mit denen der Benutzer experimentieren soll, sowie die zu erkundenden Features, und geben Sie relevante Informationen an, die dem Benutzer helfen, zu entscheiden, ob er Ihr Angebot erwerben soll. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 

- **Zugriffsinformationen** (erforderlich für Azure Resource Manager- und Logik-Testversionen): Erläutern Sie, was ein Kunde wissen muss, um auf diese Testversion zuzugreifen und diese zu verwenden. Beschreiben Sie ein Szenario für die Verwendung Ihres Angebots, und teilen Sie genau mit, was der Kunde wissen sollte, um auf die Funktionen in der gesamten Testversion zuzugreifen. In diesem Feld können bis zu 10.000 Zeichen Text eingegeben werden.

- **Benutzerhandbuch** (erforderlich): Hierbei handelt es sich um eine ausführliche Beschreibung der Verwendung Ihrer Testversion. Das Benutzerhandbuch sollte genau beschreiben, was der Kunde durch die Erkundung der Testversion erlangen soll, und als Referenz für die Fragen des Kunden dienen. Die Datei muss das PDF-Format aufweisen und nach dem Hochladen mit einem Namen (max. 255 Zeichen) versehen werden.

- **Videos** (optional): Videos können auf YouTube oder Vimeo hochgeladen werden, und hier kann mit einem Link und einer Miniaturansicht (533 × 324 Pixel) auf sie verwiesen werden. So erhalten Kunden Informationen zu einzelnen Schritten, um sich besser mit der Testversion vertraut zu machen. Unter anderem können Sie ihnen zeigen, wie sie die Funktionen Ihres Angebots erfolgreich verwenden, und Szenarios erläutern, in denen die Vorteile hervorgehoben werden.
  - **Name** (erforderlich)
  - **URL (nur YouTube oder Vimeo)** (erforderlich)
  - **Vorschauminiatur** (Bilddatei muss im PNG-Format und 533 x 324 Pixel groß sein)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="supplemental-content"></a>Ergänzender Inhalt

Auf dieser Seite können Sie zusätzliche Informationen zu Ihrem Angebot bereitstellen, um uns bei der Validierung Ihres Angebots zu helfen. Diese Informationen werden weder den Kunden angezeigt noch im Marketplace veröffentlicht.

### <a name="target-release"></a>Zielrelease

Geben Sie an, auf welches Release von Microsoft Dynamics Business Central Ihre Lösung abzielt: die **aktuelle Version**, die **nächste Hauptversion** oder die **nächste Nebenversion**. Diese Informationen ermöglichen es uns, Ihre Lösung entsprechend zu testen.

### <a name="supported-editions"></a>Unterstützte Editionen

Wenn Ihr Angebot die Premium Edition von Microsoft Dynamics 365 Business Central erfordert, wählen Sie nur **Premium** aus. Wählen Sie andernfalls sowohl **Essentials** als auch **Premium** aus.

### <a name="key-usage-scenario"></a>Wichtige Verwendungsszenarien

Sie müssen eine PDF-Datei hochladen, in der die wichtigsten Verwendungsszenarien für Ihr Angebot in einem Dokument (PDF-Format) aufgelistet sind. Alle hier aufgeführten Szenarien können von unserem Validierungsteam überprüft werden, bevor wir Ihr Angebot für den Marketplace genehmigen.

### <a name="app-tests-automation"></a>App-Testautomatisierung

Wenn es sich bei Ihrem Angebot um eine Add-On-App handelt, müssen Sie eine **App-Testautomatisierungsdatei** (.app) hochladen. Diese Datei ist für Connect-Apps nicht anwendbar.

### <a name="test-accounts"></a>Testkonten

Wenn unser Zertifizierungsteam ein Testkonto benötigt, um Ihr Angebot ordnungsgemäß zu überprüfen, laden Sie eine PDF-, DOC- oder DOCX-Datei mit den Informationen zu Ihren **Testkonten** hoch.

## <a name="publish"></a>Veröffentlichen

### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Nachdem Sie alle erforderlichen Abschnitte des Angebots abgeschlossen haben, klicken Sie rechts oben im Portal auf **Veröffentlichen**. Sie werden zur Seite **Review and publish** (Überprüfen und veröffentlichen) weitergeleitet. 

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
    - *Nicht gestartet:* Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
    - *Unvollständig:* Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - *Abgeschlossen:* Der Abschnitt ist abgeschlossen. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam im Abschnitt **Hinweise zur Zertifizierung** Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, wenn eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Kehren Sie zu Partner Center zurück, und wählen Sie **Live schalten** aus, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
