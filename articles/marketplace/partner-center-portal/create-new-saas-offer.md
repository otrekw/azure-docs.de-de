---
title: Erstellen eines SaaS-Angebots – Azure Marketplace und Microsoft AppSource
description: Hier finden Sie Informationen zum Erstellen eines Software-as-a-Service-Angebots (SaaS-Angebots) zum Auflisten oder Verkaufen in Microsoft AppSource, im Azure Marketplace oder über das CSP-Programm (Cloud Solution Provider) im kommerziellen Microsoft-Marketplace im Microsoft Partner Center.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 08/07/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: c1550b941c6d0f58feba7d505c7206993e14226b
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88856064"
---
# <a name="create-a-saas-offer-in-the-commercial-marketplace"></a>Erstellen eines SaaS-Angebots im kommerziellen Marketplace

Um mit dem Erstellen von SaaS-Angeboten (Software-as-a-Service) im kommerziellen Marketplace beginnen zu können, müssen Sie zuerst [ein Partner Center-Konto erstellen](./create-account.md), das [Dashboard „Kommerzieller Marketplace“](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) öffnen und die Registerkarte **Übersicht** auswählen.

> [!NOTE]
> Wenn Sie ein transaktionsfähiges SaaS-Angebot erstellen, stellen Sie sicher, dass Sie die Integration von [SaaS-Fulfillment-APIs](./pc-saas-fulfillment-apis.md) implementieren.  Die Integration der APIs ist die einzige Möglichkeit, die reibungslose Transaktionsfähigkeit im Marketplace sicherzustellen. Außerdem müssen Sie sicherstellen, dass Ihre App die Azure AD-Authentifizierung mit einmaligem Anmelden (Single Sign On, SSO) verwendet. Weitere Informationen finden Sie unter [Azure AD und transaktionsfähige SaaS-Angebote im kommerziellen Marketplace](../azure-ad-saas.md).

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie in der Übersicht die Option **+ Neues Angebot** > **SaaS (Software-as-a-Service)** aus.

   ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-saas.png)

> [!NOTE]
> Nach der Veröffentlichung eines Angebots werden die daran im Partner Center vorgenommenen Änderungen erst nach der erneuten Veröffentlichung des Angebots in Onlineshops angezeigt. Achten Sie darauf, nach dem Vornehmen von Änderungen immer erneut zu veröffentlichen.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
- Nachdem Sie **Erstellen** ausgewählt haben, kann der Angebotsalias nicht mehr geändert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-overview"></a>Angebotsübersicht

**Veröffentlichungsstatus** enthält eine visuelle Darstellung der zum Veröffentlichen des Angebots erforderlichen Schritte und Angaben zur Dauer der einzelnen Schritte. Die Symbole für unvollständige Veröffentlichungsschritte sind abgeblendet.

Das Menü **Angebotsübersicht** enthält eine Liste mit Links zum Ausführen von Vorgängen für das Angebot. Die Liste der Vorgänge ändert sich basierend auf der Auswahl, die Sie für das Angebot vornehmen.  

- Wenn das Angebot ein Entwurf ist: „Entwurf löschen“
- Wenn das Angebot live ist: „Stop sell offer“ (Verkauf des Angebots einstellen)
- Wenn das Angebot in der Vorschau ist: „Live schalten“
- Wenn keine Bestätigung durch den Herausgeber erfolgt ist: „Veröffentlichung abbrechen“

## <a name="offer-setup"></a>Angebotseinrichtung

Auf dieser Seite werden folgende Informationen angefordert:

- **Möchten Sie über Microsoft verkaufen?** (Ja/Nein)
  - **Ja**, ich möchte über Microsoft verkaufen, und Microsoft soll Transaktionen in meinem Namen hosten.
  - **Nein**, ich möchte mein Angebot lieber nur in den Marketplaces listen und Transaktionen unabhängig verarbeiten.

### <a name="sell-through-microsoft"></a>Über Microsoft verkaufen

Der Verkauf über Microsoft ermöglicht eine bessere Ermittlung und Gewinnung von Kunden sowie das Hosten von Marketplace-Transaktionen durch Microsoft in Ihrem Namen. Außerdem lassen sich die weltweit verfügbaren Handelsfunktionen von Microsoft nutzen.

#### <a name="saas-offer-requirements"></a>Anforderungen für SaaS-Angebote

Um SaaS-Angebote (Software-as-a-Service) mit dem kommerziellen Marketplace im Partner Center aufzulisten, müssen die folgenden Kriterien erfüllt sein:

- Das Angebot muss die Identitätsverwaltung und Benutzerauthentifizierung von [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) verwenden.
- Das Angebot muss für die Integration in den Azure Marketplace [SaaS-Fulfillment-APIs](pc-saas-fulfillment-api-v2.md) verwenden.

#### <a name="saas-pricing-and-billing-options"></a>Preis- und Abrechnungsoptionen für SaaS

Bei SaaS-Lösungen, die im Azure-Abonnement des Herausgebers ausgeführt werden, umfassen die von den Kunden entrichteten Lizenzgebühren die Kosten für die Infrastruktur, auf der die Software bereitgestellt wird. Die Nutzung der Azure-Infrastruktur wird verwaltet und Ihnen als Partner direkt in Rechnung gestellt. Tatsächliche Nutzungsgebühren für die Infrastruktur werden dem Kunden nicht angezeigt. Herausgeber sollten die Nutzungsgebühren für Azure-Infrastruktur in ihren Softwarelizenzpreis einfließen zu lassen.

SaaS bietet Unterstützung für die monatliche oder jährliche Abrechnung auf der Grundlage einer Pauschalgebühr, pro Benutzer oder gemäß der über den Gebührenabrechnungsdienst ermittelten Nutzungsgebühren. Der kommerzielle Marketplace von Microsoft arbeitet nach einem Agenturmodell, bei dem die Herausgeber die Preise festlegen, Microsoft den Kunden Rechnungen stellt und Microsoft die Einnahmen dann unter Einbehaltung einer Agenturgebühr an den Herausgeber auszahlt.

Hier sehen Sie eine exemplarische Aufschlüsselung der Kosten und Auszahlungen zur Veranschaulichung des Agenturmodells (die genannten Preise dienen lediglich als Beispiele und stellen keine tatsächlichen Kosten dar):

|**Ihre Lizenzkosten**|**100 USD pro Monat**|
|:---|:---|
|Azure-Nutzungskosten (D1/1-Kern)|Abrechnung direkt mit dem Herausgeber, nicht mit dem Kunden|
|Microsoft führt die Abrechnung mit dem Kunden durch|100,00 USD pro Monat (der Herausgeber muss alle anfallenden oder weiterzugebenden Infrastrukturkosten in der Lizenzgebühr berücksichtigen)|

|**Microsoft berechnet**|**100 USD pro Monat**|
|:---|:---|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten <br>**Für qualifizierte SaaS-Apps zahlt Microsoft 90 % Ihrer Lizenzkosten*|80,00 USD pro Monat <br>90,00 *$* pro Monat*|

- In diesem Beispiel stellt Microsoft 100,00 USD dem Kunden für Ihre Softwarelizenz in Rechnung und zahlt 80,00 USD an den Herausgeber aus.

> [!NOTE]
> **Reduzierte Marketplace-Dienstgebühr:** Für bestimmte SaaS-Angebote, die Sie im kommerziellen Marketplace veröffentlicht haben, reduziert Microsoft die Marketplace-Dienstgebühr von 20 Prozent (wie im Microsoft-Herausgebervertrag angegeben) auf 10 Prozent. Damit Ihre Angebote qualifiziert sind, müssen sie von Microsoft als gefördert für Azure IP-Co-Selling gekennzeichnet worden sein. Die Anforderungen für die Berechtigung müssen mindestens fünf (5) Arbeitstage vor dem Ende eines jeden Kalendermonats erfüllt sein, damit die reduzierte Marketplace-Dienstgebühr für den Monat in Anspruch genommen werden kann. Die reduzierte Marketplace-Dienstgebühr gilt auch für VMs, verwaltete Apps und andere berechtigte transaktionsfähige IaaS-Angebote im Rahmen der Azure IP-Co-Selling-Incentives, die über den kommerziellen Marketplace bereitgestellt werden.

### <a name="list-through-microsoft"></a>List through Microsoft (Über Microsoft auflisten)

Erstellen Sie eine Marketplace-Liste, um mit Microsoft für Ihr Geschäft zu werben. Wenn Sie Ihr Angebot nur auflisten und die Transaktion nicht über Microsoft ausführen lassen, nimmt Microsoft nicht direkt an Softwarelizenztransaktionen teil. Es gibt keine zugehörige Transaktionsgebühr, und der Herausgeber behält 100 % der vom Kunden gezahlten Softwarelizenzgebühren. Der Herausgeber ist für die Unterstützung aller Aspekte der Softwarelizenztransaktion verantwortlich, einschließlich, aber nicht beschränkt auf: Auftragsabwicklung, Messung, Abrechnung, Rechnungsstellung, Zahlung und Inkasso.

#### <a name="get-it-now-free"></a>Jetzt abrufen (kostenlos)

Listen Sie Ihr kostenloses Angebot für Kunden, indem Sie eine gültige Adresse (beginnend mit *http* oder *https*) angeben, mit der die Kunden per [1-Klick-Authentifizierung über Azure Active Directory (Azure AD)](../marketplace-saas-applications-technical-publishing-guide.md#using-azure-active-directory-to-enable-trials) eine Testversion erhalten können. Beispiel: `https://contoso.com/saas-app`.

#### <a name="free-trial-listing"></a>Kostenlose Testversion (Listing)

Listen Sie Ihr Angebot für Kunden mit einem Link zu einer kostenlosen Testversion, indem Sie eine gültige Adresse (beginnend mit *http* oder *https*) angeben, mit der die Kunden [per 1-Klick-Authentifizierung über Azure Active Directory (Azure AD)](../marketplace-saas-applications-technical-publishing-guide.md#using-azure-active-directory-to-enable-trials) eine Testversion erhalten können. Beispiel: `https://contoso.com/trial/saas-app`. Kostenlose Testversionen in der Angebotsliste werden von Ihrem Dienst erstellt, verwaltet und konfiguriert und weisen keine von Microsoft verwalteten Abonnements auf.

> [!NOTE]
> Die Token, die Ihre Anwendung über den Testlink erhält, können nur verwendet werden, um zum Automatisieren der Kontoerstellung in Ihrer App Benutzerinformationen aus Azure AD abzurufen. Die Authentifizierung mit diesem Token wird für Microsoft-Konten (MSA) nicht unterstützt.

#### <a name="contact-me"></a>Kontakt mit mir aufnehmen

Sammeln Sie über Ihr CRM-System (Customer Relationship Management) Kundenkontaktinformationen. Der Kunde wird gebeten, die Berechtigung zur Freigabe seiner Informationen zu erteilen. Diese Kundeninformationen sowie der Angebotsname, die Angebots-ID und der Marketplace, auf dem der Kunde Ihr Angebot gefunden hat, werden an das CRM-System gesendet, das Sie konfiguriert haben. Weitere Informationen zum Konfigurieren des CRM-Systems finden Sie unter [Kundenleads](#customer-leads).

#### <a name="example-marketplace-offer-listing"></a>Liste der Marketplace-Beispielangebote

Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen in Microsoft AppSource angezeigt werden:

:::image type="content" source="media/example-appsource-saas.png" alt-text="Veranschaulicht, wie dieses Angebot in Microsoft AppSource angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Großes Logo
2. Kategorien
3. Branchen
4. Supportadresse (Link)
5. Nutzungsbedingungen
6. Datenschutzrichtlinie
7. Angebotsname
8. Zusammenfassung
9. BESCHREIBUNG
10. Screenshots/Videos
11. Dokumente

<br>Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen im Azure-Portal angezeigt werden:

:::image type="content" source="media/example-virtual-machine-container-iot-edge-saas.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure-Portal angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Titel
2. BESCHREIBUNG
3. Nützliche Links
4. Screenshots

## <a name="enable-a-test-drive"></a>Aktivieren einer Testversion

Eine Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, das Produkt vor dem Kauf zu testen. Dies führt zu einer höheren Konversionsrate und zur Generierung von aussichtsreichen Leads. Weitere Informationen zu Testversionen finden Sie [hier](../what-is-test-drive.md).

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Weitere Informationen finden Sie unter [Ermöglichen Sie Ihren Kunden das Testen Ihres Angebots](test-drive.md).

### <a name="test-drive-resources"></a>Testversionen von Ressourcen

- [Worum handelt es sich bei einer Testversion?](../what-is-test-drive.md)
- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Übersicht](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF-Datei; stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](./includes/connect-lead-management-a.md)]

#### <a name="additional-lead-management-resources"></a>Zusätzliche Ressourcen zur Leadverwaltung
- [Häufig gestellte Fragen zur Leadverwaltung](../lead-management-for-cloud-marketplace.md#frequently-asked-questions)
- [Häufige Leadkonfigurationsfehler](../lead-management-for-cloud-marketplace.md#publishing-config-errors)
- [Lead management at a glance](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) (Übersicht über die Leadverwaltung, Informationsblatt in englischer Sprache)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite werden Sie aufgefordert, die Kategorien und Branchen, die zum Gruppieren Ihres Angebots in den Marketplaces verwendet werden, die Verträge für Ihr Angebot und die Version Ihrer App zu definieren.

### <a name="category"></a>Category

Ihr Angebot wird in AppSource oder im Azure Marketplace je nach Transaktionsfunktionen veröffentlicht, die Ihrem Angebot und Ihrer Kategorieauswahl zugeordnet sind. Ausführliche Informationen finden Sie unter [Vergleich von Microsoft AppSource und Azure Marketplace](../comparing-appsource-azure-marketplace.md). Wählen Sie Kategorien und Unterkategorien aus, die am besten zu Ihrem Angebot und Ihrer Zielgruppe passen. Wählen Sie Folgendes:

- Mindestens eine und bis zu zwei Kategorien, einschließlich einer primären und einer sekundären Kategorie (optional).
- Bis zu zwei Unterkategorien für jede primäre und/oder sekundäre Kategorie. Wenn keine Unterkategorie für Ihr Angebot infrage kommt, wählen Sie **Nicht zutreffend** aus.

Eine vollständige Liste der Kategorien und Unterkategorien für jeden Onlineshop finden Sie unter [Angebotslistung: Bewährte Methoden](../gtm-offer-listing-best-practices.md).

### <a name="industries"></a>Branchen

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

Die Branchenauswahl gilt nur für in AppSource veröffentlichte Angebote.

### <a name="app-version"></a>App-Version

Dies ist ein optionales Feld, das im AppSource-Marketplace verwendet wird, um die Versionsnummer Ihres Angebots anzugeben.

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Standardvertrag für den kommerziellen Microsoft-Marketplace

Microsoft stellt eine Vorlage für den Standardvertrag bereit.

- **Möchten Sie den Standardvertrag für den kommerziellen Microsoft-Marketplace verwenden?**

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Reduzierung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft eine Vorlage für Standardverträge für den kommerziellen Microsoft-Marketplace bereit, mit der Transaktionen im Marketplace erleichtert werden. Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Herausgeber im kommerziellen Marketplace ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen. Den Standardvertrag finden Sie unter https://go.microsoft.com/fwlink/?linkid=2041178.

Sie können auswählen, ob Sie den Standardvertrag statt Ihrer eigenen benutzerdefinierten Geschäftsbedingungen verwenden möchten, indem Sie das Kontrollkästchen „Standardvertrag für den kommerziellen Marketplace verwenden“ aktivieren.

![Kontrollkästchen „Standardvertrag verwenden“](./media/use-standard-contract.png)

> [!NOTE]
> Nachdem Sie ein Angebot mit dem Standardvertrag für den kommerziellen Microsoft-Marketplace veröffentlicht haben, können Sie Ihre eigenen benutzerdefinierten Geschäftsbedingungen nicht mehr angeben. In diesem Szenario gilt „entweder oder“. Entweder bieten Sie Ihre Lösung im Rahmen des Standardvertrags **oder** Ihrer eigenen Geschäftsbedingungen an. Wenn Sie die Bedingungen des Standardvertrags ändern möchten, können Sie dies über Zusatzvereinbarungen für den Standardvertrag erreichen.

#### <a name="standard-contract-amendments"></a>Zusatzvereinbarungen für den Standardvertrag

Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Herausgeber der Einfachheit halber den Standardvertrag auswählen und die Bedingungen für ihr Produkt oder Unternehmen anpassen. Kunden, die bereits den Microsoft-Standardvertrag geprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag prüfen.

Für kommerzielle Marketplace-Herausgeber stehen zwei Arten von Zusatzvereinbarungen zur Verfügung:

- Universelle Zusatzvereinbarungen: Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Universelle Zusatzvereinbarungen werden jedem Kunden des Angebots im Rahmen der Kaufabwicklung angezeigt. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarungen akzeptieren, bevor sie Ihr Angebot nutzen können.
- Benutzerdefinierte Zusatzvereinbarungen: Diese Zusatzvereinbarungen sind spezielle Zusatzvereinbarungen für den Standardvertrag, die über Azure-Mandanten-IDs nur bestimmte Kunden betreffen. Herausgeber können den gewünschten Mandanten auswählen. Nur den Kunden des Mandanten werden die Bedingungen der Zusatzvereinbarungen im Rahmen der Kaufabwicklung für das Angebot präsentiert.  Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarung(en) akzeptieren, bevor sie Ihr Angebot nutzen können.

>[!NOTE]
> Diese beiden Arten von Zusatzvereinbarungen sind übereinander angeordnet. Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarung für den Standardvertrag.

**Bestimmungen der universellen Zusatzvereinbarung zum Standardvertrag für den kommerziellen Microsoft-Marketplace:** Geben Sie in diesem Feld die universellen Zusatzbestimmungen ein. Sie können eine universelle Zusatzvereinbarung pro Angebot angeben. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Die Bestimmungen werden Kunden während der Suche und in der Kaufabwicklung in AppSource, dem Azure Marketplace und dem Azure-Portal angezeigt.

**Benutzerdefinierte Bestimmungen der Zusatzvereinbarung zum Standardvertrag für den kommerziellen Microsoft-Marketplace:** Wählen Sie zunächst **Benutzerdefinierte Zusatzbestimmungen hinzufügen** aus. Sie können bis zu 10 benutzerdefinierte Zusatzbestimmungen pro Angebot angeben.

- **Benutzerdefinierte Zusatzvereinbarungen**: Geben Sie Ihre benutzerdefinierten Zusatzvereinbarungen in das Feld für benutzerdefinierte Zusatzvereinbarungen ein. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Diese benutzerdefinierten Zusatzbestimmungen werden im Azure-Portal im Rahmen der Kaufabwicklung für das Angebot nur für Kunden mit den Mandanten-IDs angezeigt, die Sie für diese benutzerdefinierten Bestimmungen angeben.  
- **Mandanten-IDs** (erforderlich): Jede benutzerdefinierte Zusatzvereinbarung kann für bis zu 20 Mandanten-IDs gelten. Wenn Sie eine benutzerdefinierte Zusatzvereinbarung hinzufügen, müssen Sie mindestens eine Mandanten-ID angeben. Die Mandanten-ID identifiziert Ihren Kunden in Azure. Sie können Ihren Kunden bitten, Ihnen diese ID mitzuteilen. Kunden finden ihre ID unter „portal.azure.com“ > „Azure Active Directory“ > „Eigenschaften“. Der Wert „Verzeichnis-ID“ ist die Mandanten-ID (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können die von Ihrer Organisation zugeteilte Mandanten-ID Ihres Kunden auch nachschlagen, indem Sie unter [Wie lautet meine Mandanten-ID für Microsoft Azure und Office 365?](https://www.whatismytenantid.com) die Domänennamen-URL des Kunden verwenden.
- **Beschreibung** (optional): Geben Sie optional eine aussagekräftige Beschreibung für die Mandanten-ID an, mit der Sie den Kunden, für den die Zusatzvereinbarung gelten soll, einfacher identifizieren können.

#### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Wenn Sie Ihre eigenen Geschäftsbedingungen einschließen möchten, können Sie diese in das Feld für die Geschäftsbedingungen eingeben. Sie können bis zu 10.000 Zeichen in dieses Feld eingeben. Wenn Ihre Geschäftsbedingungen eine längere Beschreibung erfordern, geben Sie in diesem Feld einen einzelnen URL-Link ein, über den Ihre Geschäftsbedingungen aufgerufen werden können.

Kunden müssen diese Bestimmungen akzeptieren, bevor sie Ihr Angebot testen können.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Auf dieser Seite werden die Sprachen (und Märkte) angezeigt, in denen Ihr Angebot verfügbar ist. Derzeit ist nur der Standort „Englisch (USA)“ verfügbar. Auf dieser Seite werden zudem der Status der sprachspezifischen Liste und das Datum/die Uhrzeit der Hinzufügung angezeigt. Sie müssen für jede Sprache/jeden Markt die Marketplace-Informationen (Angebotsname, Beschreibung, Suchbegriffe usw.) definieren.

> [!NOTE]
> Die Inhalte der Angebotsliste (z. B. Angebotsbeschreibung, Dokumente, Screenshots, Nutzungsbedingungen und Datenschutzrichtlinien) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf [nicht englische Sprache] verfügbar.“. Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="offer-listings"></a>Angebotsauflistungen

Geben Sie Informationen an, die im Marketplace angezeigt werden sollen, einschließlich der Beschreibung des Angebots und der Marketingressourcen.

- **Name** (erforderlich): Der hier definierte Name wird als Titel der Angebotsliste in den von Ihnen gewählten Marketplaces angezeigt. Der Name ist basierend auf Ihrer vorherigen Eingabe in **Neues Angebot** bereits eingetragen. Der Name ist möglicherweise markenrechtlich geschützt. Er darf keine Emojis (mit Ausnahme des Markenzeichen- und Copyrightsymbols) enthalten und muss auf 50 Zeichen begrenzt sein.
- **Zusammenfassung** (erforderlich): Geben Sie eine kurze Beschreibung Ihres Angebots ein, die in den Suchergebnissen für Marketplace-Einträge verwendet werden soll. In diesem Feld können bis zu 100 Zeichen Text eingegeben werden.
- **Beschreibung** (erforderlich): Geben Sie eine Beschreibung Ihres Angebots ein, die in der Marketplace-Eintragsübersicht angezeigt werden soll. Sie können z.B ein Wertversprechen, wichtige Vorteile, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe, erforderliche Veröffentlichungen und einen Link zu weiteren Informationen eingeben. In dieses Feld können einschließlich Markup bis zu 3.000 Textzeichen eingegeben werden. Weitere Tipps finden Sie unter [Erstellen einer interessanten App-Beschreibung](/windows/uwp/publish/write-a-great-app-description).
- **Suchbegriffe**: Geben Sie bis zu drei Suchbegriffe ein, mit denen Kunden in den Marketplaces nach Ihrem Angebot suchen können.
- **Anweisungen für den Einstieg** (erforderlich): Erläutern Sie potenziellen Kunden, wie die App konfiguriert und mit ihrer Verwendung begonnen wird.  Diese Schnellstartanleitung kann Links zu ausführlicherer Onlinedokumentation enthalten. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden.

#### <a name="description"></a>BESCHREIBUNG

Dies ist ein Pflichtfeld.

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

#### <a name="links"></a>Links

- **Datenschutzrichtlinie** (erforderlich): Link zur Datenschutzrichtlinie Ihrer Organisation. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.
- **Marketingmaterial für das CSP-Programm** (optional): Stellen Sie einen Link zu Marketingmaterial bereit, wenn Sie Ihr Angebot auf das [CSP-Programm (Cloud Solution Provider)](../cloud-solution-providers.md) ausdehnen. Durch CSP wird Ihr Angebot auf ein breiteres Spektrum von qualifizierten Kunden erweitert, indem CSP-Partnern das Bündeln, Vermarkten und Verkaufen Ihres Angebots ermöglicht wird. Diese Handelspartner benötigen Zugriff auf Materialien für die Vermarktung Ihres Angebots. Weitere Informationen finden Sie unter [Go-To-Market-Dienste](https://partner.microsoft.com/reach-customers/gtm).
- **Nützliche Links** (optional): Optionale zusätzliche Onlinedokumente zu Ihrer App oder zu zugehörigen Diensten, die durch die Angabe eines **Titels** und einer **URL** aufgeführt werden. Fügen Sie weitere nützliche Links hinzu, indem Sie auf **+ URL hinzufügen** klicken.

#### <a name="contact-information"></a>Kontaktinformationen

- **Kontakte**: Geben Sie für jeden Kundenkontakt **Name**, **Telefonnummer** und **E-Mail-Adresse** eines Mitarbeiters an. (Diese Angaben werden *nicht* öffentlich angezeigt.) Eine **Support-URL** ist für die Gruppe **Supportkontakt** erforderlich. (Sie *wird* öffentlich angezeigt.)

  - **Supportkontakt** (erforderlich): Für allgemeine Supportfragen.
  - **Technischer Ansprechpartner** (erforderlich): Für technische Fragen.
  - **Channel Manager contact** (Channel-Manager-Kontakt) (erforderlich): Für Fragen von Handelspartnern zum CSP-Programm.

#### <a name="files-and-images"></a>Dateien und Bilder

- **Dokumente** (erforderlich): Fügen Sie für Ihr Angebot Marketingdokumente im PDF-Format hinzu (pro Angebot mindestens ein Dokument und maximal drei Dokumente).
- **Bilder** (erforderlich): Logobilder Ihres Angebots können in den Marketplaces an verschiedenen Stellen angezeigt werden. Geben Sie eine PNG-Datei für das **große** Logo an. Dies wird in Partner Center verwendet, um ein **kleines** und ein **mittleres** Logo zu erstellen. Sie können diese Logos später optional durch andere Bilder ersetzen.

  - **Groß** (von 216 x 216 bis 350 x 350 Pixel, erforderlich)
  - **Mittel** (90 x 90 Pixel, optional)
  - **Klein** (48 x 48 Pixel, optional)
  
  Diese Logos werden an unterschiedlichen Stellen in der Auflistung verwendet:

   - Das große Logo wird auf Ihrer Seite mit der Angebotsliste sowie im Azure Marketplace und in Microsoft AppSource angezeigt.
   - Das mittelgroße Logo wird angezeigt, wenn Sie eine neue Ressource in Microsoft Azure erstellen.
   - Das kleine Logo erscheint in den Suchergebnissen von Azure Marketplace sowie auf der Hauptseite und Suchergebnisseite von Microsoft AppSource.

   [!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

- **Screenshots** (erforderlich): Fügen Sie maximal fünf Screenshots hinzu, um Ihr Angebot zu veranschaulichen (mit einer Größe von 1280 x 720 Pixeln). Alle Bilder müssen das PNG-Format aufweisen.
- **Videos** (optional): Fügen Sie Links zu Videos hinzu, um Ihr Angebot zu veranschaulichen. Sie können Links zu YouTube und/oder Vimeo-Videos angeben, die zusammen mit Ihrem Angebot bei Kunden angezeigt werden. Sie müssen auch ein Miniaturbild des Videos hinzufügen. Es muss das PNG-Format und eine Größe von 1280 x 720 Pixel aufweisen. Sie können bis zu vier Videos pro Angebot anzeigen.

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="preview-audience"></a>Vorschauzielgruppe

Auf dieser Seite können Sie eine eingeschränkte **Vorschauzielgruppe** für die Veröffentlichung Ihres Angebots definieren, bevor Sie Ihr Angebot für die allgemeine Marketplace-Zielgruppe live schalten.

> [!IMPORTANT]
> Nachdem Sie Ihr Angebot in der Vorschau überprüft haben, wählen Sie **Live schalten** aus, damit es für die öffentliche Marketplace-Zielgruppe live veröffentlicht werden kann.

Fügen Sie pro Zeile jeweils eine AAD-/MSA-Konto-E-Mail-Adresse und optional eine Beschreibung hinzu.

Fügen Sie manuell bis zu 10 E-Mail-Adressen oder durch Hochladen einer CSV-Datei bis zu 20 E-Mail-Adressen für vorhandene Microsoft-Konten (MSA) oder Azure Active Directory-Konten hinzu, um Unterstützung bei der Überprüfung Ihres Angebots vor der Liveveröffentlichung zu erhalten. Durch das Hinzufügen dieser Konten definieren Sie eine Zielgruppe, die Zugriff auf die Vorschau Ihres Angebot erhält, bevor es in dem/den Marketplace(s) veröffentlicht wird. Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

> [!NOTE]
> Die Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, _bevor_ es in den Marketplaces live veröffentlicht wird. Sie können auch einen Plan erstellen und nur für eine private Zielgruppe verfügbar machen. Auf der Registerkarte **Planlisting** können Sie mit dem Kontrollkästchen **This is a private plan** (Dies ist ein privater Plan) eine private Zielgruppe definieren. Sie können mithilfe von Azure-Mandanten-IDs eine private Zielgruppe von bis zu 20.000 Kunden definieren.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf der Registerkarte **Technische Konfiguration** sind die technischen Details definiert, die vom Marketplace für die Kommunikation mit Ihrem SaaS-Dienst verwendet werden. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen und verwalten möchte.

>[!NOTE]
>Sie müssen die Integration in [SaaS-Fulfillment-APIs](./pc-saas-fulfillment-api-v2.md) implementieren, bevor Sie diese Details in den Angebotsdetails konfigurieren. Sie müssen auch eine Angebotsseite erstellen, und Ihre App muss die Azure AD-Authentifizierung mit einmaligem Anmelden (Single Sign On, SSO) verwenden. Weitere Informationen finden Sie unter [Azure AD und transaktionsfähige SaaS-Angebote im kommerziellen Marketplace](../azure-ad-saas.md).

Diagramme und ausführliche Erläuterungen zur Verwendung der gesammelten Felder finden Sie in der Dokumentation zu den [APIs](./pc-saas-fulfillment-api-v2.md).

- **URL der Angebotsseite** (erforderlich): Definieren Sie die URL der SaaS-Website (z. B. `https://contoso.com/signup`), zu der Endkunden gelangen, nachdem sie Ihr Angebot im Marketplace erworben und den Konfigurationsvorgang vom neu erstellten SaaS-Abonnement aus gestartet haben.  Diese URL wird mit dem Identifizierungstokenparameter für den Marketplace-Kauf aufgerufen, der den SaaS-Kauf eines bestimmten Endkunden eindeutig identifiziert.  Sie müssen dieses Token mithilfe der [Auflösungs](./pc-saas-fulfillment-api-v2.md#resolve-a-purchased-subscription)-API durch die entsprechenden SaaS-Abonnementdetails ersetzen.  Diese und alle anderen Daten, die Sie sammeln möchten, sollten als Teil einer interaktiven, in Ihre Benutzeroberfläche integrierten Webseite für Kunden verwendet werden, wo sie die Endkundenregistrierung abschließen und den Kauf aktivieren können.  Auf dieser Seite sollte sich der Benutzer per 1-Klick-Authentifizierung über Azure Active Directory (Azure AD) registrieren. <br> <br> Diese URL mit dem Identifizierungstokenparameter für den Marketplace-Kauf wird auch aufgerufen, wenn der Endkunde die Oberfläche für verwaltete SaaS-Lösungen über das Azure-Portal oder das M365 Admin Center startet. Sie sollten beide Abläufe handhaben, wenn das Token zum ersten Mal nach dem Kauf für neue Kunden bereitgestellt wird und wenn es für vorhandene Kunden zum Verwalten der SaaS bereitgestellt wird. <br> <br> Die hier konfigurierte Angebotsseite sollte rund um die Uhr aktiv sein. Dies ist die einzige Möglichkeit, Sie über neue Käufe Ihrer SaaS-Angebote im Marketplace oder über Konfigurationsanforderungen eines aktiven Abonnements für ein Angebot zu benachrichtigen.

- **Verbindungswebhook** (erforderlich): Für alle asynchronen Ereignisse, die Microsoft an Sie senden muss (etwa, wenn das SaaS-Abonnement gekündigt wurde), muss eine Verbindungswebhook-URL angegeben werden. Diese URL wird aufgerufen, um Sie über das Ereignis zu informieren. <br> <br> Der von Ihnen bereitgestellte Webhook sollte rund um die Uhr aktiv sein, da dies die einzige Möglichkeit ist, Sie über Aktualisierungen der SaaS-Abonnements Ihrer Kunden, die über den Marketplace erworben wurden, zu benachrichtigen.  Wenn Sie noch nicht über ein Webhooksystem verfügen, ist die einfachste Konfiguration eine Logik-App am HTTP-Endpunkt, die auf alle veröffentlichten Ereignisse lauscht und sie entsprechend behandelt (z. B. `https://prod-1westus.logic.azure.com:443/work`). Weitere Informationen finden Sie unter [Aufrufen, Auslösen oder Schachteln von Workflows mit HTTP-Endpunkten in Logik-Apps](../../logic-apps/logic-apps-http-endpoint.md).

- **Azure AD-Mandanten-ID** (erforderlich): Es ist im Azure-Portal erforderlich, dass Sie eine [Azure AD-App (Active Directory) erstellen](../../active-directory/develop/howto-create-service-principal-portal.md), damit überprüft werden kann, ob die Verbindung zwischen unseren beiden Diensten über eine authentifizierte Kommunikation erfolgt. Um die [Mandanten-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) zu ermitteln, wechseln Sie zu Azure Active Directory, wählen Sie **Eigenschaften** aus, und suchen Sie nach der aufgeführten Nummer der **Verzeichnis-ID** (z. B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD-App-ID** (erforderlich): Sie benötigen auch Ihre [Anwendungs-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in). Rufen Sie Ihre Azure Active Directory-Instanz auf, klicken Sie auf **App-Registrierungen**, und suchen Sie dann nach der **Anwendungs-ID** (z. B. `50c464d3-4930-494c-963c-1e951d15360e`), um diesen Wert abzurufen.

>[!NOTE]
>Die Azure AD-App-ID ist Ihrer Herausgeber-ID in Ihrem Partner Center-Konto zugeordnet.  Stellen Sie sicher, dass bei allen Ihren Angeboten dieselbe Anwendungs-ID verwendet wird.

>[!NOTE]
>Wenn der Herausgeber über zwei oder mehr unterschiedliche Konten im Partner Center verfügt, sollten auch zwei oder mehr unterschiedliche Azure AD-App-IDs verwendet werden – jeweils eine für eines der Konten. Jedes Partnerkonto im Partner Center sollte eine eindeutige Azure AD-App-ID für alle SaaS-Angebote verwenden, die über dieses Konto veröffentlicht werden.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="plan-overview"></a>Planübersicht

Auf dieser Seite können Sie verschiedene Planoptionen innerhalb des gleichen Angebots angeben. Diese Pläne (auch als SKUs bezeichnet) können sich in Bezug auf Version, Monetarisierung und Dienstebenen unterscheiden. Sie müssen mindestens einen Plan einrichten, um Ihr Angebot im Marketplace zu verkaufen.

Sie können bis zu 100 Pläne für jedes Angebot erstellen. Bis zu 45 davon können privat sein. Erfahren Sie unter [Private Angebote im kommerziellen Microsoft-Marketplace](../private-offers.md) mehr über private Pläne.

Nach der Erstellung werden die Plannamen, IDs und Preismodelle, die Verfügbarkeit (öffentlich oder privat), der aktuelle Veröffentlichungsstatus und alle verfügbaren Aktionen angezeigt.

Die unter **Planübersicht** verfügbaren **Aktionen** variieren je nach dem aktuellen Status des Plans und umfassen möglicherweise:

- Wenn der Planstatus **Entwurf** lautet: „Entwurf löschen“
- Wenn der Planstatus **Live** lautet: „Stop sell plan“ (Verkauf des Plans einstellen) oder „Sync private Audience“ (Private Zielgruppe synchronisieren)

**Neuen Plan erstellen** (mindestens ein Plan für diejenigen, die den Verkauf über Microsoft abwickeln möchten)

- **Plan-ID**: Erstellen Sie für jeden Plan im Angebot eine eindeutige Plan-ID. Diese ID wird für Kunden in der Produkt-URL-Adresse und ggf. in Azure Resource Manager-Vorlagen angezeigt. Verwenden Sie nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche. Für die Plan-ID sind maximal 50 Zeichen zulässig. Die ID kann nach der Erstellung nicht geändert werden.
- **Planname:** Für Kunden wird dieser Name angezeigt, wenn sie einen Plan in Ihrem Angebot auswählen. Erstellen Sie für jeden Plan im Angebot einen eindeutigen Angebotsnamen. Der Planname wird verwendet, um Softwarepläne zu unterscheiden, die möglicherweise Teil desselben Angebots sind (z. B. Angebotsname: Windows Server, Pläne: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Planlisting

Auf dieser Seite können Sie den Plannamen und eine Beschreibung angeben. <!-- displays the languages (and markets) where your plan is available, currently English (United States) is the only location available. Additionally, this page displays the status of the language-specific listing and the date/time that it was added. You will need to define the marketplace details (offer name, description, search terms, etc.) for each language / market.-->

<!--#### Plan listing details-->

<!--Selecting one of the plan languages will display the **plan listing** information, including **Name** and **Description.** -->

- **Name**: Wird auf der Grundlage Ihres Vorschaueintrags **Neuer Plan** vorab aufgefüllt und als Titel des Softwareplans für Ihr Angebot im Marketplace angezeigt.
- **Beschreibung**: Hier können Sie erläutern, was diesen Softwareplan einzigartig macht und von anderen Softwareplänen in Ihrem Angebot unterscheidet. Sie darf bis zu 500 Zeichen enthalten.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

#### <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Auf dieser Seite können Sie die Märkte, in denen der Plan verfügbar ist, sowie das gewünschte Monetarisierungsmodell, den Preis und den Abrechnungszeitraum konfigurieren. Darüber hinaus können Sie angeben, ob der Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) angezeigt werden soll.

#### <a name="markets-optional"></a>Märkte (optional)

Jeder Plan muss in mindestens einem Markt verfügbar sein. Wählen Sie **Märkte bearbeiten** aus, und aktivieren Sie das Kontrollkästchen für jeden Marktstandort, an dem Sie diesen Plan zur Verfügung stellen möchten. Auf dieser Seite stehen ein Suchfeld und eine Option zum Auswählen von Ländern/Regionen zur Verfügung, in denen die Umsatz- und Gebrauchssteuer von Microsoft für Sie überwiesen wird.

Wenn Sie bereits Preise in US-Dollar für Ihren Plan festgelegt haben und einen anderen Marktstandort hinzufügen, wird der Preis für den neuen Markt entsprechend dem aktuellen Wechselkurs berechnet. Überprüfen Sie vor dem Veröffentlichen den Preis für jeden Markt. Sehen Sie sich die Preise mithilfe des Links „Preise exportieren (XLSX)“ an, nachdem Sie Ihre Änderungen gespeichert haben.

Wählen Sie **Speichern** aus, bevor Sie den Vorgang fortsetzen.

#### <a name="pricing"></a>Preise

##### <a name="pricing-model"></a>Preismodell

**Pauschalgebühr**: Ermöglichen Sie den Zugriff auf Ihr Angebot mit einer einzelnen monatlichen oder jährlichen Pauschalgebühr. Dies wird manchmal als arbeitsplatzbasierter Preis bezeichnet. Dieses Preismodell ermöglicht Ihnen das optionale Definieren von Volumentarifen, bei denen die Marketplace-Messungsdienst-API zur Berechnung für Kunden nach nicht standardmäßigen Einheiten verwendet wird.  Weitere Informationen zur getakteten Abrechnung finden Sie unter [Getaktete Abrechnung mit dem Marketplace-Messungsdienst](./saas-metered-billing.md).  Sie sollten diese Option auch verwenden, wenn die Nutzung Ihres SaaS-Diensts schubweise erfolgt.  Den Kunden wird nicht empfohlen, Pläne täglich oder stündlich zu wechseln.

**Pro Benutzer**: Ermöglichen Sie den Zugriff auf Ihr Angebot mit einem Preis, der auf der Anzahl von Benutzern, die auf Ihr Angebot zugreifen, oder auf der Anzahl beanspruchter Arbeitsplätze basiert. Mithilfe des benutzerbasierten Modells können Sie die minimal und maximal zulässige Anzahl von Benutzern auf Grundlage des Preises festlegen. Auf diese Weise können durch das Konfigurieren mehrerer Pläne unterschiedliche Preise auf Grundlage der Anzahl der Benutzer festgelegt werden.  Diese Felder sind optional. Wenn keine Auswahl erfolgt, wird angenommen, dass die Anzahl der Arbeitsplätze unbegrenzt ist (mindestens 1 und maximal die vom System unterstützte Anzahl). Diese Felder können beim Aktualisieren des Plans bearbeitet werden.

Nach der Veröffentlichung kann das Abrechnungspreismodell nicht mehr geändert werden. Darüber hinaus müssen alle Pläne für das gleiche Angebot dasselbe Preismodell aufweisen.

##### <a name="user-limits"></a>Benutzergrenzwerte

Wählen Sie ggf. Unter- und Obergrenzen für Benutzer aus, bzw. legen Sie sie fest.

##### <a name="billing-term-and-price"></a>Abrechnungszeitraum und Preis

Wählen Sie die **Laufzeit** und den **Preis** aus, der von Kunden zu zahlen ist. Es muss mindestens ein monatlicher oder jährlicher Preis angegeben werden. Es ist auch möglich, für Kunden beide Optionen verfügbar zu machen.

Preise in USD (USD = US-Dollar) werden beim Speichern gemäß dem aktuellen Wechselkurs in die lokale Währung aller ausgewählten Märkte konvertiert. Überprüfen Sie vor der Veröffentlichung die Preise in jedem Markt, indem Sie die Preistabellenkalkulation exportieren. Wenn Sie benutzerdefinierte Preise in einem einzelnen Markt festlegen möchten, ändern und importieren Sie die Preistabellenkalkulation. Sie sind für die Überprüfung des Preismodells und diese Einstellungen verantwortlich.
*\*Sie müssen Ihre Preisänderungen speichern, damit die Preisdaten exportiert werden können.*

Überprüfen Sie Ihre Preise vor der Veröffentlichung sorgfältig, da nicht alle Elemente eines Plans nach seiner Veröffentlichung geändert werden können:

- Das Preismodell kann nach dem Veröffentlichen eines Plans nicht geändert werden.
- Nachdem ein Abrechnungszeitraum für einen Plan veröffentlicht wurde, kann er nicht mehr entfernt werden.
- Nachdem ein Preis für einen Markt in Ihrem Plan veröffentlicht wurde, kann er nicht mehr geändert werden.

#### <a name="free-trial"></a>Kostenlose Testversion

SaaS-Angebote über den kommerziellen Marketplace ermöglichen Ihnen beim Verkauf über Microsoft, eine einmonatige kostenlose Testversion bereitzustellen. Kostenlose Testversionen werden für alle Abrechnungsmodelle und -bedingungen mit Ausnahme von Volumentarifen unterstützt. Diese Option ermöglicht es Kunden, durch den kostenlosen Zugriff für einen Monat eine niedrige Schwelle für den Einstieg zu erhalten.  Wenn Sie eine kostenlose Testversion für Pläne in Ihrem Angebot aktivieren, kann der Kunde diese Version nicht vor Ende des ersten Monats in ein kostenpflichtiges Abonnement umwandeln.  Während dieses Zeitraums können Kunden, die Ihr Angebot erwerben, alle unterstützten Pläne ausprobieren, bei denen die kostenlose Testversion aktiviert ist, und zwischen diesen wechseln.  Die Umwandlung in ein kostenpflichtiges Abonnement erfolgt automatisch am Ende der Laufzeit.

>[!NOTE]
>Wenn sich der Kunde für die Umwandlung in einen Plan ohne kostenlose Testversionen entscheidet, wird dies umgesetzt, doch geht die kostenlose Testversion sofort verloren. Auch sobald ein Kunde mit dem Bezahlen für einen Plan beginnt, kann er selbst dann keine kostenlose Testversion mehr für dasselbe Abonnement erhalten, wenn eine Umwandlung in eine SKU erfolgt, die kostenlose Testversionen unterstützt.

Hier können Sie für jeden Plan in Ihrem Angebot eine kostenlose Testversion konfigurieren. Aktivieren Sie das Kontrollkästchen, um eine einmonatige Testversion zuzulassen.

![Kontrollkästchen für eine einmonatige kostenlose Testversion](./media/free-trial-enable.png)

>[!NOTE]
>Sobald Ihr transaktionsfähiges Angebot mit einer kostenlosen Testversion veröffentlicht wurde, kann es für den jeweiligen Plan nicht mehr deaktiviert werden. Vergewissern Sie sich, dass diese Einstellung für die erste Veröffentlichung korrekt ist, damit der Plan nicht neu erstellt werden muss.

Wenn Sie Informationen zu Kundenabonnements abrufen möchten, die derzeit an einer kostenlosen Testversion teilnehmen, verwenden Sie die neue API-Eigenschaft `isFreeTrial`, die als „true“ oder „false“ gekennzeichnet ist. Weitere Informationen finden Sie unter der [API zum Abrufen von SaaS-Abonnements](pc-saas-fulfillment-api-v2.md#get-subscription).

>[!NOTE]
>Kostenlose Testversionen werden für Pläne, die den Marketplace-Messungsdienst nutzen, nicht unterstützt.

#### <a name="plan-visibility"></a>Sichtbarkeit des Plans

Sie können festlegen, dass jeder Plan für alle Benutzer oder nur für eine von Ihnen ausgewählte Zielgruppe angezeigt werden soll. Sie können mithilfe von Azure AD-Mandanten-IDs die Mitgliedschaft in dieser eingeschränkten Zielgruppe zuweisen.

##### <a name="privacy"></a>Datenschutz

Aktivieren Sie das Kontrollkästchen **Dies ist ein privater Plan.** , um den Plan als privaten Plan festzulegen und nur für die von Ihnen ausgewählte eingeschränkte Zielgruppe anzuzeigen. Nachdem Sie den Plan als privaten Plan veröffentlicht haben, können Sie die Zielgruppe aktualisieren oder den Plan für alle Benutzer verfügbar machen. Sobald ein Plan als für alle Benutzer als sichtbar veröffentlicht wurde, muss er für alle Benutzer sichtbar bleiben. (Er kann nicht wieder als privater Plan konfiguriert werden.)

##### <a name="restricted-audience-tenant-ids"></a>**Restricted Audience (Tenant IDs)** (Eingeschränkte Zielgruppe (Mandanten-IDs))

Weisen Sie die Zielgruppe zu, die Zugriff auf den privaten Plan hat. Der Zugriff wird mit Mandanten-IDs zugewiesen. Dabei kann eine Beschreibung jeder zugewiesenen Mandanten-ID angegeben werden. Es können maximal 10 Mandanten-IDs oder, wenn eine CSV-Tabellenkalkulationsdatei importiert wird, 20.000 Mandanten-IDs von Kunden hinzugefügt werden.

Ein Mandant stellt eine Organisation dar, wobei die ID als global eindeutiger Bezeichner (Globally Unique Identifier, GUID – eine 128-Bit-Ganzzahl zum Identifizieren von Ressourcen) dargestellt wird. Es handelt sich um eine dedizierte Instanz von Azure AD, die eine Organisation oder ein App-Entwickler erhält, wenn die Organisation oder der App-Entwickler eine Geschäftsbeziehung mit Microsoft eingeht, beispielsweise bei der Registrierung für Azure, Microsoft Intune oder Microsoft 365. Jeder Azure AD-Mandant ist eindeutig und von anderen Azure AD-Mandanten getrennt. Um den Mandanten zu überprüfen, melden Sie sich beim Azure-Portal mit dem Konto an, das Sie für die Verwaltung Ihrer Anwendung verwenden möchten. Falls Sie einen Mandanten besitzen, werden Sie automatisch angemeldet, und der Mandantenname wird direkt unter dem Kontonamen angezeigt. Zeigen Sie rechts oben im Azure-Portal auf Ihren Kontonamen, um Ihren Namen, Ihre E-Mail-Adresse, Ihr Verzeichnis und Ihre Mandanten-ID (GUID) sowie Ihre Domäne anzuzeigen. Falls Ihr Konto mehreren Mandanten zugeordnet ist, können Sie durch Klicken auf Ihren Kontonamen ein Menü öffnen, über das Sie zwischen Mandanten wechseln können. Jeder Mandant besitzt eine eigene Mandanten-ID. Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe einer Domänennamen-URL nachschlagen.

In SaaS-Angeboten werden private Zielgruppen mithilfe von Mandanten-IDs definiert, in anderen Angebotstypen können jedoch Azure-Abonnement-IDs (die auch als GUIDs dargestellt werden) verwendet werden.

> [!NOTE]
> Eine private Zielgruppe (oder eingeschränkte Zielgruppe) unterscheidet sich von einer Vorschauzielgruppe. Auf der Seite **[Vorschau](#preview-audience)** können Sie eine Vorschauzielgruppe definieren. Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, *bevor* es im Marketplace live veröffentlicht wird. Die Festlegung als private Zielgruppe gilt nur für einen bestimmten Plan. Für die private Zielgruppe können alle Pläne (private und nicht private Pläne) angezeigt werden, jedoch nur während des eingeschränkten Vorschauzeitraums, in dem der Plan getestet und überprüft wird.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Beispielliste der Pläne innerhalb eines Marketplace-Angebots

:::image type="content" source="media/marketplace-plan.png" alt-text="Liste der Marketplace-Beispielpläne mit Hinweisen":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Planname
2. Planbeschreibung

## <a name="cloud-solution-provider-csp-reseller-audience"></a>Cloud Solution Provider (CSP) – Zielgruppe „Handelspartner“

Wenn Sie die Option wählen, Ihr Angebot im CSP-Programm verfügbar zu machen, können Cloud Solution Provider Ihr Produkt als Teil einer Paketlösung verkaufen. Weitere Informationen finden Sie unter [Cloud Solution Provider (CSP)](https://go.microsoft.com/fwlink/?linkid=2111109).

## <a name="publish"></a>Veröffentlichen

Sobald Sie alle erforderlichen Abschnitte des Angebots ausgefüllt haben, klicken Sie rechts oben im Portal auf **Überprüfen und veröffentlichen**.

### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
  - **Nicht gestartet**: Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
  - **Unvollständig**: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Sie müssen zum entsprechenden Abschnitt zurückkehren und ihn aktualisieren.
  - **Vollständig**: Der Abschnitt ist vollständig. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Kehren Sie zu Partner Center zurück, und wählen Sie **Live schalten** aus, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
