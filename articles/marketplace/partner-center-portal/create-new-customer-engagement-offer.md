---
title: Erstellen eines Angebots für Dynamics 365 for Customer Engagement & PowerApps im kommerziellen Microsoft-Marketplace
description: Erstellen eines neuen Angebots für Dynamics 365 for Customer Engagement & PowerApps für das Aufführen oder Verkaufen im Azure Marketplace, in AppSource oder über das CSP-Programm (Cloud Solution Provider) in Partner Center
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: e49ed542af140b28e8ebe58e5c8920d0959c9387
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848898"
---
# <a name="create-a-dynamics-365-for-customer-engagement--powerapps-offer"></a>Erstellen eines Angebots für Dynamics 365 for Customer Engagement und PowerApps

In diesem Thema wird erläutert, wie Sie ein neues Angebot für Dynamics 365 for Customer Engagement und PowerApps erstellen. Alle Apps für Dynamics 365 for Customer Engagement (PowerApps, Sales, Service, Project Service und Field Service) müssen unseren Zertifizierungsprozess durchlaufen und eine Testversion unterstützen. Der Zertifizierungsprozess überprüft Ihre Lösung auf Standardanforderungen, Kompatibilität und richtige Methoden. Die Testversion ermöglicht es Benutzern, Ihre Lösung in einer Dynamics 365-Liveumgebung bereitzustellen.

Bevor Sie beginnen, [erstellen Sie ein Konto im kommerziellen Marketplace in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), sofern Sie dies noch nicht getan haben. Vergewissern Sie sich, dass Ihr Konto im Programm „Kommerzieller Marketplace“ registriert ist.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Klicken Sie im linken Navigationsmenü auf **Kommerzieller Marketplace** > **Übersicht**.
3. Klicken Sie auf der Übersichtsseite auf **+ Neues Angebot** > **Dynamics 365 for Customer Engagement & PowerApps**.

    ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-dynamics-365-cepa.png)

> [!NOTE]
> Nach der Veröffentlichung eines Angebots werden die daran im Partner Center vorgenommenen Änderungen erst nach der erneuten Veröffentlichung des Angebots in der digitalen Ladenzeile angezeigt. Achten Sie darauf, nach dem Vornehmen von Änderungen immer erneut zu veröffentlichen.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
- Dies kann nach dem Auswählen von **Erstellen** nicht mehr geändert werden.

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

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

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

- [Bewährte technische Methoden](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Bewährte Marketingmethoden](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [PDF mit Übersicht zu Testversionen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (Stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite können Sie die Kategorien und Branchen, die zum Zuordnen Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren.

### <a name="category"></a>Category

Wählen Sie mindestens eine und maximal drei Kategorien aus. Diese Kategorien werden verwendet, um Ihr Angebot in den geeigneten Marketplace-Suchbereichen zu platzieren. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, inwiefern Ihr Angebot diesen Kategorien entspricht.

### <a name="industry"></a>Branche

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="applicable-dynamics-365-products"></a>Gilt für Dynamics 365-Produkte

Wählen Sie alle Dynamics 365-Produkte aus, für die dieses Angebot gilt.

### <a name="app-version"></a>App-Version

Geben Sie die Versionsnummer Ihres Angebots ein. Für Kunden wird diese Version auf der Detailseite des Angebots aufgeführt. Wenn Sie die Versionsnummer nur aufgrund von Änderungen bei Marketing/Beschreibungen aktualisieren, aktivieren Sie das Kontrollkästchen **Ausschließliche Marketingänderung**. Durch diese Option kann das Angebot die Zertifizierungs- und Bereitstellungsphase umgehen.

### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Stellen Sie hier Ihre eigenen Geschäftsbedingungen bereit. Sie können auch die Adresse angeben, unter der Ihre Geschäftsbedingungen zu finden sind. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihr Angebot testen können.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Auf dieser Seite werden die Sprachen angezeigt, in denen Ihr Angebot aufgeführt wird. Derzeit ist **Englisch (Vereinigte Staaten)** die einzige verfügbare Option.

Definieren Sie hier für jede Sprache/jeden Markt Marketplace-Informationen (z. B. Angebotsname, Beschreibung und Bilder). Wählen Sie die Sprache bzw. den Namen des Markts aus, um diese Informationen anzugeben.

> [!NOTE]
> Die Inhalte der Angebotsliste (z. B. Beschreibung, Dokumente, Screenshots und Nutzungsbedingungen) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf [nicht englische Sprache] verfügbar.“ Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text aufgefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

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

### <a name="support-urls"></a>Support-URLs

In diesem Abschnitt können Sie Links bereitstellen, die Kunden dabei helfen, mehr über Ihr Angebot zu erfahren.

#### <a name="help-link"></a>Hilfelink

Geben Sie die Adresse ein, unter der Kunden mehr über Ihr Angebot erfahren können.

#### <a name="privacy-policy-url"></a>URL zur Datenschutzrichtlinie

Geben Sie die Adresse der Datenschutzrichtlinie Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="contacts"></a>Kontakte

Geben Sie den Namen, die E-Mail-Adresse und die Telefonnummer eines **Supportkontakts** und eines **technischen Ansprechpartners** an. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und werden möglicherweise an CSP-Partner weitergegeben.

Im Abschnitt **Supportkontakt** müssen Sie auch die **Support-URL** angeben, unter der CSP-Partner Support für Ihr Angebot erhalten.

### <a name="supporting-documents"></a>Unterstützende Dokumente

Geben Sie mindestens ein zugehöriges Marketingdokument (und höchstens drei) im PDF-Format an, z. B. Whitepapers, Broschüren, Checklisten oder Präsentationen.

### <a name="marketplace-images"></a>Marketplace-Bilder

Stellen Sie Logos und Bilder für Ihr Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen.

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="store-logos"></a>Store-Logos

Stellen Sie das Logo Ihres Angebots in den folgenden drei in Pixeln angegebenen Größen bereit:
- **Klein** (erforderlich; 48 x 48)
- **Groß** (erforderlich; 216 x 216)
- **Breit** (optional; 255 x 115)

#### <a name="hero"></a>Hero

Das Herobild ist optional. Wenn Sie ein Herobild angeben, muss es in der Größe 815 x 290 Pixel vorliegen.

#### <a name="screenshots"></a>Screenshots

Fügen Sie Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Mindestens ein Screenshot ist erforderlich, und Sie können bis zu fünf hinzufügen. Alle Screenshots müssen in der Größe 1280 x 720 Pixel vorliegen.

#### <a name="videos"></a>Videos

Optional können Sie bis zu vier Videos hinzufügen, die Ihr Angebot veranschaulichen. Diese Videos sollten auf YouTube und/oder Vimeo gehostet werden. Geben Sie für jedes Video den Namen, seine URL und ein Miniaturbild (1280 x 720 Pixel) ein.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

[Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="availability"></a>Verfügbarkeit

Auf dieser Seite können Sie definieren, wo und wie Ihr Angebot zur Verfügung gestellt werden soll.

### <a name="markets"></a>Märkte

Geben Sie die Märkte an, in denen Ihr Angebot verfügbar sein soll. Wählen Sie hierzu **Edit markets** (Märkte bearbeiten) aus. Dadurch wird das Popupfenster **Marktauswahl** angezeigt.

Standardmäßig sind keine Märkte ausgewählt, Sie müssen jedoch mindestens einen Markt auswählen, um Ihr Angebot zu veröffentlichen. Klicken Sie auf **Alle auswählen**, um Ihr Angebot in allen möglichen Märkten verfügbar zu machen, oder wählen Sie bestimmte Märkte aus, die Sie hinzufügen möchten.

Ihre hier getroffene Auswahl gilt nur für neue Käufe. Wenn Ihre App bereits von einem Benutzer in einem bestimmten Markt verwendet wird und Sie den Markt später entfernen, können Benutzer, die in diesem Markt bereits über Ihr Angebot verfügen, es weiterhin nutzen, aber Ihr Angebot ist nicht für neue Kunden in diesem Markt erhältlich.

> [!IMPORTANT]
> Es liegt in ihrer Verantwortung, alle lokalen rechtlichen Anforderungen zu erfüllen, auch wenn diese Anforderungen nicht hier oder im Partner Center aufgeführt sind. Auch wenn Sie alle Märkte auswählen, kann es sein, dass bestimmte Angebote in einigen Ländern und Regionen aufgrund lokaler Gesetze und Einschränkungen oder anderer Faktoren nicht gelistet werden.

### <a name="preview-audience"></a>Vorschauzielgruppe

Bevor Sie Ihr Angebot im Marketplace für die breite Öffentlichkeit live schalten, müssen Sie es zunächst für eine begrenzte **Vorschauzielgruppe** verfügbar machen. Geben Sie hier einen **Hide-Schlüssel** (eine beliebige Zeichenfolge, die nur Kleinbuchstaben und/oder Ziffern enthält) ein. Mitglieder Ihrer Vorschauzielgruppe können diesen Hide-Schlüssel als Token verwenden, um eine Vorschau Ihres Angebots im Marketplace anzuzeigen.

Wenn Sie anschließend bereit sind, Ihr Angebot verfügbar zu machen und die Vorschaueinschränkung zu entfernen, müssen Sie den **Hide-Schlüssel** entfernen und das Angebot erneut veröffentlichen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf dieser Seite werden die technischen Informationen definiert, die für das Herstellen einer Verbindung mit Ihrem Angebot verwendet werden. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte.

### <a name="base-license-model"></a>Basislizenzmodell

Das Basislizenzmodell bestimmt, wie Kunden im CRM Admin Center Ihrer Anwendung zugewiesen werden. Wählen Sie **Ressource** für instanzbasierte Lizenzierung bzw. **Benutzer** aus, wenn Lizenzen pro Mandant zugewiesen werden.

### <a name="requires-s2s-outbound-and-crm-secure-store-access"></a>Erfordert ausgehenden S2S- und CRM Secure Store-Zugriff

Aktivieren Sie dieses Kontrollkästchen, um die Konfiguration von CRM Secure Store- oder ausgehendem Server-zu-Server-Zugriff (S2S) zu ermöglichen. Dieses Feature muss während der Zertifizierungsphase besonders vom Dynamics 365-Team berücksichtigt werden. Microsoft kontaktiert Sie, um zusätzliche Schritte ausführen, die dieses Feature unterstützen.

### <a name="application-configuration-url"></a>Anwendungskonfigurations-URL

Geben Sie die URL für die Konfigurationswebseite an, über die der Kunde Ihre APP konfigurieren kann.

### <a name="crm-package"></a>CRM-Paket

Geben Sie im Feld **URL Ihres Paketspeicherorts** die URL eines Azure Blob Storage-Kontos ein, das die hochgeladene ZIP-Datei des CRM-Pakets enthält. Schließen Sie einen schreibgeschützten SAS-Schlüssel in die URL ein, damit Microsoft Ihr Paket zur Überprüfung öffnen kann.

> [!IMPORTANT]
> Um eine Veröffentlichungssperre zu vermeiden, stellen Sie sicher, dass das Ablaufdatum in der URL Ihres Blobspeichers nicht abgelaufen ist. Sie können das Datum überarbeiten, indem Sie auf Ihre Richtlinie zugreifen. Es wird empfohlen, den **Ablaufzeitpunkt** mindestens einen Monat in der Zukunft anzusetzen.

Aktivieren Sie ggf. das Kontrollkästchen **In der Paketdatei sind mehrere CRM-Pakete enthalten**. Wenn dies der Fall ist, stellen Sie sicher, dass Sie alle Pakete in die ZIP-Datei einschließen.

Ausführliche Informationen zum Erstellen des Pakets und zum Aktualisieren seiner Struktur finden Sie in [Schritt 3: Erstellen eines AppSource-Pakets für Ihre App](https://docs.microsoft.com/powerapps/developer/common-data-service/create-package-app-appsource).

### <a name="crm-package-availability"></a>Verfügbarkeit von CRM-Paketen

Wählen Sie in diesem Abschnitt **+ Region hinzufügen** aus, um die geografischen Regionen anzugeben, in denen Ihr CRM-Paket für Kunden verfügbar sein wird. Die Bereitstellung in den folgenden unabhängigen Regionen erfordert eine spezielle Berechtigung und Überprüfung während des Zertifizierungsprozesses: [Deutschland](https://docs.microsoft.com/azure/germany/), [Cloud for US Government](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) und TIP.

Standardmäßig wird die zuvor eingegebene **Anwendungskonfigurations-URL** für jede Region verwendet. Wenn Sie möchten, können Sie eine separate Anwendungskonfigurations-URL für eine oder mehrere bestimmte Regionen eingeben. 

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="test-drive-technical-configuration"></a>Technische Konfiguration der Testversion

Auf dieser Seite können Sie eine Demonstration („Testversion“) einrichten, mit der Kunden Ihr Angebot vor dem Erwerb ausprobieren können. Erfahren Sie mehr im Artikel [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Aktivieren Sie auf der Registerkarte [Angebotseinrichtung](#test-drive) das Kontrollkästchen „Aktivieren einer Testversion“, um eine Testversion zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Erfahren Sie mehr im Artikel [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Die folgenden Typen von Testversionen sind verfügbar, jeweils mit eigenen Anforderungen an die technische Konfiguration:

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Logik-App](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (keine technische Konfiguration erforderlich)

### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Technical configuration for Azure Resource Manager test drive (Technische Konfiguration für Azure Resource Manager-Testversion)

Eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet. Erfahren Sie mehr über das Einrichten einer [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regionen** (erforderlich): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. In der Regel sollten Sie Ihre Testversion in den Regionen zur Verfügung stellen, in denen Sie die größte Anzahl von Kunden erwarten, damit sie für eine optimale Leistung die nächstgelegene Region auswählen können. Sie müssen sicherstellen, dass in Ihrem Abonnement alle Ressourcen in jeder von Ihnen ausgewählten Region bereitgestellt werden können.
- **Instanzen**: Wählen Sie den Typ („Heiß“ oder „Kalt“) und die Anzahl von verfügbaren Instanzen aus. Diese wird mit der Anzahl der Regionen multipliziert, in denen Ihr Angebot verfügbar ist.

    **Heiß**: Dieser Instanztyp wird bereits bereitgestellt, und auf ihn kann in jeder ausgewählten Region zugegriffen werden. Die Kunden haben sofort Zugriff auf Instanzen vom Typ *Heiß* und müssen nicht auf die Bereitstellung warten. Der Nachteil dabei ist, dass diese Instanzen immer unter Ihrem Azure-Abonnement ausgeführt werden, sodass höhere Betriebszeitkosten anfallen. Es wird dringend empfohlen, mindestens eine Instanz des Typs *Heiß* festzulegen, da die meisten Kunden nicht auf eine vollständige Bereitstellung warten möchten. Wenn keine Instanz vom Typ *Heiß* verfügbar ist, nimmt die Verwendung durch die Kunden ab.

    **Kalt**: Dieser Instanztyp stellt die Gesamtzahl von Instanzen dar, die pro Region bereitgestellt werden können. Bei Instanzen des Typs „Kalt“ muss die Bereitstellung der gesamten Resource Manager-Vorlage für die Testversion durchgeführt werden, wenn der Kunde die Testversion anfordert. Daher dauert das Laden von Instanzen des Typs *Kalt* weitaus länger als das Laden von Instanzen des Typs *Heiß*. Sie müssen jedoch nur für die Dauer der Testversion bezahlen. Instanzen vom Typ „Kalt“ werden *nicht* wie Instanzen vom Typ *Heiß* stets im Azure-Abonnement ausgeführt.

- **Azure Resource Manager-Vorlage für Testversion:** Laden Sie die ZIP-Datei mit Ihrer Azure Resource Manager-Vorlage hoch. Im Schnellstartartikel [Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) erhalten Sie weitere Informationen zum Erstellen einer Azure Resource Manager-Vorlage.

- **Dauer der Testversion** (erforderlich): Geben Sie die Anzahl von Stunden ein, für die die Testversion aktiv bleiben soll. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet. Diese Dauer kann nur in ganzen Stunden angegeben werden. („2“ ist z. B. ein gültiger Wert, „1.5“ nicht.)

### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Technical configuration for Dynamics 365 test drive (Technische Konfiguration für Dynamics 365-Testversion)

Microsoft kann mit diesem Typ von Testversion die Dienstbereitstellung hosten und verwalten, um das Einrichten einer Testversion zu vereinfachen. Die Konfiguration für diesen Typ von gehosteter Testversion ist gleich, unabhängig davon ob die Testversion für Business Central, Customer Engagement oder Operations vorgesehen ist.

- **Maximale Anzahl gleichzeitig ausgeführter Testversionen:** Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Jeder gleichzeitige Benutzer nutzt eine Dynamics 365-Lizenz, während die Testversion aktiv ist. Daher müssen Sie sicherstellen, dass genügend Lizenzen für die festgelegte maximale Anzahl verfügbar sind. Es wird ein Wert zwischen 3 und 5 empfohlen.

- **Dauer der Testversion** (erforderlich): Geben Sie die Anzahl von Stunden ein, für die die Testversion aktiv bleiben soll. Nach Ablauf dieses Zeitraums wird die Sitzung beendet und nutzt keine Ihrer Lizenzen mehr. Der empfohlene Wert beträgt 2 bis 24 Stunden, abhängig von der Komplexität Ihres Angebots. Diese Dauer kann nur in ganzen Stunden angegeben werden. („2“ ist z. B. ein gültiger Wert, „1.5“ nicht.) Der Benutzer kann eine neue Sitzung anfordern, wenn die Zeit abgelaufen ist und er erneut auf die Testversion zugreifen möchte.

- **Instanz-URL** (erforderlich): Geben Sie die URL an, über die der Kunde die Testversion startet. Dies ist in der Regel die URL Ihrer Dynamics 365-Instanz, auf der Ihre App mit installierten Beispieldaten ausgeführt wird (z. B. `https://testdrive.crm.dynamics.com`).

- **Web-API-URL der Instanz** (erforderlich): Rufen Sie die Web-API-URL für Ihre Dynamics 365-Instanz ab, indem Sie sich bei Ihrem Microsoft 365-Konto anmelden, zu **Einstellungen** \&gt; **Anpassung** \&gt; **Entwicklerressourcen** \&gt; **Instance Web API (Service Root URL)** (Instanz-Web-API (Stamm-URL des Diensts)) navigieren und die dort angegebene URL kopieren (z. B. `https://testdrive.crm.dynamics.com/api/data/v9.0`).

- **Rollenname** (erforderlich): Geben Sie den Namen der Sicherheitsrolle ein, den Sie in der benutzerdefinierten Dynamics 365-Testversion definiert haben. Der Name der Sicherheitsrolle wird dem Benutzer während der Testversion zugewiesen (z. B. „Testversionsrolle“).

### <a name="technical-configuration-for-logic-app-test-drive"></a>Technical configuration for Logic app test drive (Technische Konfiguration für Logik-App-Testversion)

Für alle benutzerdefinierten Produkte sollte für die Bereitstellung der Testversion dieser Vorlagentyp verwendet werden, der vielfältige komplexe Lösungsarchitekturen umfasst. Um weitere Informationen über das Einrichten von Logik-App-Testversionen zu erhalten, besuchen Sie [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) und [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) auf GitHub (in englischer Sprache).

- **Region** (erforderlich; Dropdownliste für Einfachauswahl): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. Die Ressourcen für Ihre Logik-App werden in der von Ihnen gewählten Region bereitgestellt. Wenn die Logik-App benutzerdefinierte Ressourcen umfasst, die in einer bestimmten Region gespeichert sind, müssen Sie diese Region hier auswählen. Dazu empfiehlt es sich, die Logik-App lokal in Ihrem Azure-Abonnement im Portal vollständig bereitzustellen und zu überprüfen, ob sie ordnungsgemäß ausgeführt wird, bevor Sie diese Auswahl vornehmen.

- **Maximale Anzahl gleichzeitig ausgeführter Testversionen:** Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Diese Testversionen sind bereits bereitgestellt, sodass Kunden sofort auf sie zugreifen können, ohne auf eine Bereitstellung warten zu müssen.

- **Dauer der Testversion** (erforderlich): Geben Sie die Dauer in Stunden ein, für die die Testversion aktiv bleiben soll. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet.

- **Name der Azure-Ressourcengruppe** (erforderlich): Geben Sie den Namen der [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) ein, in der die Testversion für die Logik-App gespeichert ist.

- **Azure logic app name** (Name der Azure-Logik-App) (erforderlich): Geben Sie den Namen der Logik-App ein, von der die Testversion dem Benutzer zugewiesen wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

- **Deprovision logic app name** (Name der Logik-App für die Aufhebung der Bereitstellung) (erforderlich): Geben Sie den Namen der Logik-App ein, mit der die Bereitstellung der Testversion aufgehoben wird, wenn sie vom Kunden nicht mehr verwendet wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Technical configuration not required for Power BI test drives (Power BI-Testversionen erfordern keine technische Konfiguration)

Für Produkte, mit denen ein interaktives Power BI-Visual veranschaulicht werden soll, kann ein eingebetteter Link zum Freigeben eines benutzerdefinierten Dashboards als Testversion verwendet werden. Deshalb ist keine technische Konfiguration erforderlich. Erfahren Sie mehr über das Einrichten von[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)-Vorlagen-Apps.

### <a name="deployment-subscription-details"></a>Abonnementdetails für Bereitstellung der Testversion

Erstellen Sie ein separates eindeutiges Azure-Abonnement, und stellen Sie es bereit (für Power BI-Testversionen nicht erforderlich), damit Microsoft die Testversion in Ihrem Auftrag bereitstellen kann.

- **Azure-Abonnement-ID** (erforderlich für den Azure Resource Manager und Logic Apps): Geben Sie die Abonnement-ID ein, um für Ressourcennutzungsberichte und Abrechnungszwecke Zugriff auf die Dienste Ihres Azure-Kontos zu gewähren. Es wird empfohlen, [ein eigenes Azure-Abonnement zu erstellen](https://docs.microsoft.com/azure/billing/billing-create-subscription), das für Testversionen verwendet wird, falls Sie es noch nicht getan haben. Sie finden die Azure-Abonnement-IDs, indem Sie sich beim [Azure-Portal](https://portal.azure.com/) anmelden und im Menü auf der linken Seite zu den **Abonnements** navigieren. Durch Auswahl der Registerkarte wird Ihre Abonnement-ID angezeigt (z. B. a83645ac-1234-5ab6-6789-1h234g764ghty).

- **ID des Azure AD-Mandanten** (erforderlich): Geben Sie Ihre [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) für Azure Active Directory (AD) ein. Um diese ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen im linken Menü die Registerkarte „Active Directory“ und dann **Eigenschaften** aus und suchen die unter **Verzeichnis-ID** aufgeführte Nummer (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe Ihrer Domänennamen-URL nachschlagen.

- **Azure AD tenant name** (Name des Azure AD-Mandanten) (erforderlich für Dynamics 365): Geben Sie den Namen Ihrer Azure Active Directory-Instanz (AD) ein. Um diesen Namen zu suchen, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Oben rechts wird der Mandantenname unter Ihrem Kontonamen angegeben.

- **Azure AD-App-ID** (erforderlich): Geben Sie Ihre [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) für Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ und dann **App-Registrierungen** aus, und suchen Sie die aufgeführte Nummer der **Anwendungs-ID** (z. B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD app client secret** (Geheimer Clientschlüssel der Azure AD-App) (erforderlich): Geben Sie den [geheimen Clientschlüssel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) Ihrer Azure AD-Anwendung ein. Um diesen Wert zu finden, melden Sie sich beim [Azur-Portal](https://portal.azure.com/) an. Wählen Sie im linken Menü die Registerkarte **Azure Active Directory** aus, wählen Sie **App-Registrierungen** aus und dann Ihre Testversion-App. Wählen Sie als Nächstes **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, geben Sie eine Beschreibung ein, wählen Sie **Nie** unter **Läuft ab** aus, und wählen Sie dann **Hinzufügen** aus. Stellen Sie sicher, dass Sie den Wert kopieren. Sie sollten die Seite nicht verlassen, bevor Sie den Wert kopiert haben, da Sie andernfalls nicht auf den Wert zugreifen können.)

### <a name="test-drive-marketplace-listings"></a>Testversionen für Marketplace-Auflistungen

Die Option **Marketplace-Auflistungen** auf der Registerkarte **Testversion** zeigt die Sprachen an, in denen Ihre Testversion verfügbar ist. Derzeit ist **Englisch (USA)** der einzige verfügbare Standort. Wählen Sie den Namen der Sprache aus, um Informationen zur Beschreibung der Testversion einzugeben.

- **Beschreibung** (erforderlich): Beschreiben Sie Ihre Testversion. Geben Sie an, was in der Demonstration enthalten ist, nennen Sie die Ziele, mit denen der Benutzer experimentieren soll, sowie die zu erkundenden Features, und geben Sie relevante Informationen an, die dem Benutzer helfen, zu entscheiden, ob er Ihr Angebot erwerben soll. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 

- **Zugriffsinformationen** (erforderlich für Azure Resource Manager- und Logik-Testversionen): Erläutern Sie, was ein Kunde wissen muss, um auf diese Testversion zuzugreifen und diese zu verwenden. Beschreiben Sie ein Szenario für die Verwendung Ihres Angebots, und teilen Sie genau mit, was der Kunde wissen sollte, um auf die Funktionen in der gesamten Testversion zuzugreifen. In diesem Feld können bis zu 10.000 Zeichen Text eingegeben werden.

- **Benutzerhandbuch** (erforderlich): Hierbei handelt es sich um eine ausführliche Beschreibung der Verwendung Ihrer Testversion. Das Benutzerhandbuch sollte genau beschreiben, was der Kunde durch die Erkundung der Testversion erlangen soll, und als Referenz für die Fragen des Kunden dienen. Die Datei muss das PDF-Format aufweisen und nach dem Hochladen mit einem Namen (max. 255 Zeichen) versehen werden.

- **Videos** (optional): Laden Sie Videos auf einer externen Hostingsite hoch, und verweisen Sie hier mit dem Link und einer Vorschauminiatur (533 x 324 Pixel) darauf. Auf diese Weise können Kunden Informationen zu einzelnen Schritten erhalten, um sich besser mit der Testversion vertraut zu machen. Unter anderem können Sie ihnen zeigen, wie sie die Features Ihres Angebots erfolgreich verwenden, und Szenarios erläutern, in denen deren Vorteile deutlich werden.
  - **Name** (erforderlich)
  - **Adresse** (erforderlich; nur YouTube oder Vimeo)
  - **Vorschauminiatur** (Bilddatei muss im PNG-Format und 533 x 324 Pixel groß sein)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="supplemental-content"></a>Ergänzender Inhalt

Auf dieser Seite können Sie zusätzliche Informationen zu Ihrem Angebot bereitstellen, um uns bei der Validierung Ihres Angebots zu helfen. Diese Informationen werden weder den Kunden angezeigt noch im Marketplace veröffentlicht.

### <a name="key-usage-scenario"></a>Wichtige Verwendungsszenarien

Laden Sie eine PDF-Datei hoch, in der die wichtigsten Verwendungsszenarios für Ihr Angebot aufgeführt sind. Alle Szenarios werden möglicherweise von unserem Überprüfungsteam überprüft, bevor wir Ihr Angebot für den Marketplace genehmigen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="publish"></a>Veröffentlichen

### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Wenn Sie alle erforderlichen Abschnitte des Angebots ausgefüllt haben, klicken Sie rechts oben im Portal auf **Review and publish** (Überprüfen und veröffentlichen).

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
    - **Nicht gestartet:** Der Abschnitt wurde nicht bearbeitet und sollte ausgefüllt werden.
    - **Unvollständig:** Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - **Vollständig:** Der Abschnitt ist vollständig. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam im Abschnitt **Hinweise zur Zertifizierung** Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Kehren Sie zum Partner Center zurück, und wählen Sie **Live schalten** aus, um das Angebot für die Öffentlichkeit freizugeben (oder bei einem privaten Angebot für eine private Zielgruppe zu veröffentlichen).

## <a name="next-step"></a>Nächster Schritt

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
