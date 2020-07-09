---
title: Erstellen eines neuen SaaS-Angebots im kommerziellen Microsoft-Marketplace
description: Hier finden Sie Informationen zum Erstellen eines neuen Software-as-a-Service-Angebots (SaaS-Angebots) zum Auflisten oder Verkaufen in Microsoft AppSource, Azure Marketplace oder über das CSP-Programm (Cloud Solution Provider) im kommerziellen Microsoft-Marketplace in Microsoft Partner Center.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 3c922675619fc877d9d97a43b69a15f5ca4f393e
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849109"
---
# <a name="create-a-new-saas-offer-in-the-commercial-marketplace"></a>Erstellen eines neuen SaaS-Angebots im kommerziellen Marketplace

Um mit dem Erstellen von SaaS-Angeboten (Software-as-a-Service) im kommerziellen Marketplace beginnen zu können, müssen Sie zuerst [ein Partner Center-Konto erstellen](./create-account.md), das [Dashboard „Kommerzieller Marketplace“](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) öffnen und die Registerkarte **Übersicht** auswählen.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie in der Übersicht die Option **+ Neues Angebot** > **SaaS (Software-as-a-Service)** aus.

   ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-saas.png)

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

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

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
- Das Angebot muss für die Integration in den Azure Marketplace [SaaS-Fulfillment-APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) verwenden.

#### <a name="saas-pricing-and-billing-options"></a>Preis- und Abrechnungsoptionen für SaaS

Bei SaaS-Lösungen, die im Azure-Abonnement des Herausgebers ausgeführt werden, umfassen die von den Kunden entrichteten Lizenzgebühren die Kosten für die Infrastruktur, auf der die Software bereitgestellt wird. Die Nutzung der Azure-Infrastruktur wird verwaltet und Ihnen als Partner direkt in Rechnung gestellt. Tatsächliche Nutzungsgebühren für die Infrastruktur werden dem Kunden nicht angezeigt. Herausgeber sollten die Nutzungsgebühren für Azure-Infrastruktur in ihren Softwarelizenzpreis einfließen zu lassen. 

SaaS bietet Unterstützung für die monatliche oder jährliche Abrechnung auf der Grundlage einer Pauschalgebühr, pro Benutzer oder gemäß der über den Gebührenabrechnungsdienst ermittelten Nutzungsgebühren. Der kommerzielle Marketplace von Microsoft arbeitet nach einem Agenturmodell, bei dem die Herausgeber die Preise festlegen, Microsoft den Kunden Rechnungen stellt und Microsoft die Einnahmen dann unter Einbehaltung einer Agenturgebühr an den Herausgeber auszahlt.

Hier sehen Sie eine exemplarische Aufschlüsselung der Kosten und Auszahlungen zur Veranschaulichung des Agenturmodells:

|**Ihre Lizenzkosten**|**100 USD pro Monat**|
|:---|:---|
|Azure-Nutzungskosten (D1/1-Kern)|Abrechnung direkt mit dem Herausgeber, nicht mit dem Kunden|
|Microsoft führt die Abrechnung mit dem Kunden durch|100,00 USD pro Monat (der Herausgeber muss alle anfallenden oder weiterzugebenden Infrastrukturkosten in der Lizenzgebühr berücksichtigen)|

|**Microsoft berechnet**|**100 USD pro Monat**|
|:---|:---|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten <br>**Für qualifizierte SaaS-Apps zahlt Microsoft 90 % Ihrer Lizenzkosten*|80,00 USD pro Monat <br>90,00 *$* pro Monat*|

- In diesem Beispiel stellt Microsoft 100,00 USD dem Kunden für Ihre Softwarelizenz in Rechnung und zahlt 80,00 USD an den Herausgeber aus.
- Für Partner, die für die **reduzierte Marketplace-Dienstgebühr** qualifiziert sind, wird von Mai 2019 bis Juni 2020 eine reduzierte Transaktionsgebühr für die SaaS-Angebote angezeigt. In diesem Szenario stellt Microsoft 100,00 USD für Ihre Softwarelizenz in Rechnung und zahlt 90,00 USD an den Herausgeber aus.

> [!NOTE]
> **Reduzierte Marketplace-Dienstgebühr:** Für SaaS-Angebote, die Sie in unserem kommerziellen Marketplace veröffentlicht haben, reduziert Microsoft die Marketplace-Dienstgebühr von 20 Prozent (wie im Microsoft-Herausgebervertrag angegeben) auf 10 Prozent. Damit Ihr Angebot qualifiziert ist, muss mindestens eines Ihrer Angebote von Microsoft als bereit für IP-Co-Selling oder als priorisiert für IP-Co-Selling gekennzeichnet worden sein. Die Anforderungen für die Berechtigung müssen mindestens fünf (5) Arbeitstage vor dem Ende eines jeden Kalendermonats erfüllt sein, damit die reduzierte Marketplace-Dienstgebühr für den Monat in Anspruch genommen werden kann.  Die reduzierte Marketplace-Dienstgebühr gilt nicht für virtuelle Computer, verwaltete Apps oder andere Produkte, die über unseren kommerziellen Marketplace zur Verfügung gestellt werden. Die reduzierte Marketplace-Dienstgebühr ist nur für qualifizierte Angebote bezüglich Lizenzgebühren verfügbar, die von Microsoft zwischen dem 1. Mai 2019 und dem 30. Juni 2020 vereinnahmt wurden. Nach diesem Zeitpunkt gilt für die Marketplace-Dienstgebühr wieder der normale Betrag.

### <a name="list-through-microsoft"></a>List through Microsoft (Über Microsoft auflisten)

Erstellen Sie eine Marketplace-Liste, um mit Microsoft für Ihr Geschäft zu werben. Wenn Sie Ihr Angebot nur auflisten und die Transaktion nicht über Microsoft ausführen lassen, nimmt Microsoft nicht direkt an Softwarelizenztransaktionen teil. Es gibt keine zugehörige Transaktionsgebühr, und der Herausgeber behält 100 % der vom Kunden gezahlten Softwarelizenzgebühren. Der Herausgeber ist für die Unterstützung aller Aspekte der Softwarelizenztransaktion verantwortlich, einschließlich, aber nicht beschränkt auf: Auftragsabwicklung, Messung, Abrechnung, Rechnungsstellung, Zahlung und Inkasso.

<!-- - **How do you want potential customers to interact with this listing offer?** -->

#### <a name="get-it-now-free"></a>Jetzt abrufen (kostenlos)

Listen Sie Ihr kostenloses Angebot für Kunden, indem Sie eine gültige Adresse (beginnend mit *http* oder *https*) angeben, mit der die Kunden per [1-Klick-Authentifizierung über Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials) eine Testversion erhalten können. Beispiel: `https://contoso.com/saas-app`.

#### <a name="free-trial-listing"></a>Kostenlose Testversion (Listing)

Listen Sie Ihr Angebot für Kunden mit einem Link zu einer kostenlosen Testversion, indem Sie eine gültige Adresse (beginnend mit *http* oder *https*) angeben, mit der die Kunden [per 1-Klick-Authentifizierung über Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials) eine Testversion erhalten können. Beispiel: `https://contoso.com/trial/saas-app`. Kostenlose Testversionen in der Angebotsliste werden von Ihrem Dienst erstellt, verwaltet und konfiguriert und weisen keine von Microsoft verwalteten Abonnements auf.

> [!NOTE]
> Die Token, die Ihre Anwendung über den Testlink erhält, können nur verwendet werden, um zum Automatisieren der Kontoerstellung in Ihrer App Benutzerinformationen aus Azure AD abzurufen. Die Authentifizierung mit diesem Token wird für Microsoft-Konten (MSA) nicht unterstützt.

#### <a name="contact-me"></a>Kontakt mit mir aufnehmen

Sammeln Sie über Ihr CRM-System (Customer Relationship Management) Kundenkontaktinformationen. Der Kunde wird gebeten, die Berechtigung zur Freigabe seiner Informationen zu erteilen. Diese Kundeninformationen sowie der Angebotsname, die Angebots-ID und der Marketplace, auf dem der Kunde Ihr Angebot gefunden hat, werden an das CRM-System gesendet, das Sie konfiguriert haben. Weitere Informationen zum Konfigurieren des CRM-Systems finden Sie unter [Kundenleads](#customer-leads).

#### <a name="example-marketplace-offer-listing"></a>Liste der Marketplace-Beispielangebote

![Liste der Marketplace-Beispielangebote mit Hinweisen](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Aktivieren einer Testversion

Eine Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, das Produkt vor dem Kauf zu testen. Dies führt zu einer höheren Konversionsrate und zur Generierung von aussichtsreichen Leads. Weitere Informationen zu Testversionen finden Sie [hier](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Weitere Informationen finden Sie unter [Ermöglichen Sie Ihren Kunden das Testen Ihres Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive).

### <a name="test-drive-resources"></a>Testversionen von Ressourcen

- [Bewährte Marketingmethoden](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Übersicht](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF-Datei; stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](./includes/connect-lead-management-a.md)]

#### <a name="additional-lead-management-resources"></a>Zusätzliche Ressourcen zur Leadverwaltung
- [Häufig gestellte Fragen zur Leadverwaltung](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Häufige Leadkonfigurationsfehler](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Lead management at a glance](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) (Übersicht über die Leadverwaltung, Informationsblatt in englischer Sprache)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite werden Sie aufgefordert, die Kategorien und Branchen, die zum Gruppieren Ihres Angebots in den Marketplaces verwendet werden, die Verträge für Ihr Angebot und die Version Ihrer App zu definieren.

### <a name="category"></a>Category

Wählen Sie mindestens eine bzw. bis zu drei Kategorien aus, um Ihr Angebot in den entsprechenden Marketplace-Suchbereichen zu platzieren. Geben Sie in der Angebotsbeschreibung an, inwiefern Ihr Angebot diesen Kategorien entspricht.

### <a name="industries"></a>Branchen

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

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

**Bestimmungen der universellen Zusatzvereinbarung zum Standardvertrag für den kommerziellen Microsoft-Marketplace:** Geben Sie in diesem Feld die universellen Zusatzbestimmungen ein. Sie können eine universelle Zusatzvereinbarung pro Angebot angeben. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Die Bestimmungen werden Kunden während des Ermittlungs- und Kaufprozesses in AppSource, dem Azure Marketplace und dem Azure-Portal angezeigt.

**Benutzerdefinierte Bestimmungen der Zusatzvereinbarung zum Standardvertrag für den kommerziellen Microsoft-Marketplace:** Wählen Sie zunächst **Benutzerdefinierte Zusatzbestimmungen hinzufügen** aus. Sie können bis zu 10 benutzerdefinierte Zusatzbestimmungen pro Angebot angeben.

- **Benutzerdefinierte Zusatzvereinbarungen**: Geben Sie Ihre benutzerdefinierten Zusatzvereinbarungen in das Feld für benutzerdefinierte Zusatzvereinbarungen ein. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Diese benutzerdefinierten Zusatzbestimmungen werden im Azure-Portal im Rahmen der Kaufabwicklung für das Angebot nur für Kunden mit den Mandanten-IDs angezeigt, die Sie für diese benutzerdefinierten Bestimmungen angeben.  
- **Mandanten-IDs** (erforderlich): Jede benutzerdefinierte Zusatzvereinbarung kann für bis zu 20 Mandanten-IDs gelten. Wenn Sie eine benutzerdefinierte Zusatzvereinbarung hinzufügen, müssen Sie mindestens eine Mandanten-ID angeben. Die Mandanten-ID identifiziert Ihren Kunden in Azure. Sie können Ihren Kunden bitten, Ihnen diese ID mitzuteilen. Kunden finden ihre ID unter „portal.azure.com“ > „Azure Active Directory“ > „Eigenschaften“. Der Wert „Verzeichnis-ID“ ist die Mandanten-ID (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können die von Ihrer Organisation zugeteilte Mandanten-ID Ihres Kunden auch nachschlagen, indem Sie unter [Wie lautet meine Mandanten-ID für Microsoft Azure und Office 365?](https://www.whatismytenantid.com) die Domänennamen-URL des Kunden verwenden.
- **Beschreibung** (optional): Geben Sie optional eine aussagekräftige Beschreibung für die Mandanten-ID an, mit der Sie den Kunden, für den die Zusatzvereinbarung gelten soll, einfacher identifizieren können.

#### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Wenn Sie Ihre eigenen Geschäftsbedingungen einschließen möchten, können Sie diese in das Feld für die Geschäftsbedingungen eingeben. Sie können bis zu 10.000 Zeichen in dieses Feld eingeben. Wenn Ihre Geschäftsbedingungen eine längere Beschreibung erfordern, geben Sie in diesem Feld einen einzelnen URL-Link ein, über den Ihre Geschäftsbedingungen aufgerufen werden können. Er wird für Kunden als aktiver Link angezeigt.

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
- **Beschreibung** (erforderlich): Geben Sie eine Beschreibung Ihres Angebots ein, die in der Marketplace-Eintragsübersicht angezeigt werden soll. Sie können z.B ein Wertversprechen, wichtige Vorteile, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe, erforderliche Veröffentlichungen und einen Link zu weiteren Informationen eingeben. In dieses Feld können einschließlich Markup bis zu 3.000 Textzeichen eingegeben werden. Weitere Tipps finden Sie unter [Erstellen einer interessanten App-Beschreibung](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).
- **Suchbegriffe**: Geben Sie bis zu drei Suchbegriffe ein, mit denen Kunden in den Marketplaces nach Ihrem Angebot suchen können.
- **Anweisungen für den Einstieg** (erforderlich): Erläutern Sie potenziellen Kunden, wie die App konfiguriert und mit ihrer Verwendung begonnen wird.  Diese Schnellstartanleitung kann Links zu ausführlicherer Onlinedokumentation enthalten. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden.

#### <a name="description"></a>BESCHREIBUNG

Dies ist ein Pflichtfeld.

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

#### <a name="links"></a>Links

- **Datenschutzrichtlinie** (erforderlich): Link zur Datenschutzrichtlinie Ihrer Organisation. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.
- **Marketingmaterial für das CSP-Programm** (optional): Stellen Sie einen Link zu Marketingmaterial bereit, wenn Sie Ihr Angebot auf das [CSP-Programm (Cloud Solution Provider)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) ausdehnen. Durch CSP wird Ihr Angebot auf ein breiteres Spektrum von qualifizierten Kunden erweitert, indem CSP-Partnern das Bündeln, Vermarkten und Verkaufen Ihres Angebots ermöglicht wird. Diese Handelspartner benötigen Zugriff auf Materialien für die Vermarktung Ihres Angebots. Weitere Informationen finden Sie unter [Go-To-Market-Dienste](https://partner.microsoft.com/reach-customers/gtm).
- **Nützliche Links** (optional): Optionale zusätzliche Onlinedokumente zu Ihrer App oder zu zugehörigen Diensten, die durch die Angabe eines **Titels** und einer **URL** aufgeführt werden. Fügen Sie weitere nützliche Links hinzu, indem Sie auf **+ URL hinzufügen** klicken.

#### <a name="contact-information"></a>Kontaktinformationen

- **Kontakte**: Geben Sie für jeden Kundenkontakt **Name**, **Telefonnummer** und **E-Mail-Adresse** eines Mitarbeiters an. (Diese Angaben werden *nicht* öffentlich angezeigt.) Eine **Support-URL** ist für die Gruppe **Supportkontakt** erforderlich. (Sie *wird* öffentlich angezeigt.)

    - **Supportkontakt** (erforderlich): Für allgemeine Supportfragen.
    - **Technischer Ansprechpartner** (erforderlich): Für technische Fragen.
    - **Channel Manager contact** (Channel-Manager-Kontakt) (erforderlich): Für Fragen von Handelspartnern zum CSP-Programm.

#### <a name="files-and-images"></a>Files and Images (Dateien und Bilder)

- **Dokumente** (erforderlich): Fügen Sie für Ihr Angebot Marketingdokumente im PDF-Format hinzu (pro Angebot mindestens ein Dokument und maximal drei Dokumente).
- **Bilder** (optional): In den Marketplaces können Logobilder Ihres Angebots an verschiedenen Stellen angezeigt werden, weshalb folgende Pixelgrößen im PNG-Format erforderlich sind:

    - **Klein** (48 x 48, erforderlich)
    - **Mittel** (90 x 90, erforderlich)
    - **Groß** (216 x 216, erforderlich)
    - **Breit** (255 x 115)
    - **Hero** (815 x 290)

- **Screenshots** (erforderlich): Fügen Sie Screenshots hinzu, um Ihr Angebot zu veranschaulichen. Es können maximal fünf (5) Screenshots hinzugefügt werden. Ihre Größe sollte 1280 x 720 Pixel betragen. Alle Bilder müssen das PNG-Format aufweisen.
- **Videos** (optional): Fügen Sie Links zu Videos hinzu, um Ihr Angebot zu veranschaulichen. Sie können Links zu YouTube und/oder Vimeo-Videos angeben, die zusammen mit Ihrem Angebot bei Kunden angezeigt werden. Sie müssen auch ein Miniaturbild des Videos hinzufügen. Es muss das PNG-Format und eine Größe von 1280 x 720 Pixel aufweisen. Sie können bis zu vier Videos pro Angebot anzeigen.

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

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

Auf dieser Seite werden die technischen Informationen (URL-Pfad, Webhook, Mandanten-ID und App-ID) für die Verbindung mit Ihrem Angebot definiert. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte. Diagramme, die die Verwendung der gesammelten Felder beschreiben, finden Sie in der Dokumentation zu [SaaS-Fulfillment-APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2).

- **URL der Angebotsseite** (erforderlich): Definieren Sie die URL der Website, zu der der Kunde weitergeleitet wird, nachdem er Ihr Angebot über den Marketplace erworben hat. Diese URL wird der Endpunkt sein, der ein Token erhält, wenn ein Kunde auf die Seite weitergeleitet wird. Dieses Token kann über die Auflösung in den Fulfillment-APIs gegen Bereitstellungsdetails ausgetauscht werden. Diese und alle anderen Daten, die Sie sammeln, können als Teil einer interaktiven, in Ihre Benutzeroberfläche integrierten Webseite für Kunden verwendet werden, wo sie die Registrierung abschließen und den Kauf aktivieren können.

- **Verbindungswebhook** (erforderlich): Für alle asynchronen Ereignisse, die Microsoft im Auftrag des Kunden an Sie senden muss (etwa, wenn das SaaS-Abonnement ungültig geworden ist), muss ein Webhook für die Verbindung angegeben werden. Wenn Sie noch nicht über ein Webhooksystem verfügen, ist die einfachste Konfiguration eine Logik-App am HTTP-Endpunkt, die auf alle veröffentlichten Ereignisse lauscht und sie entsprechend behandelt (z. B. https:\//prod-1westus.logic.azure.com:443/work). Weitere Informationen finden Sie unter [Aufrufen, Auslösen oder Schachteln von Workflows mit HTTP-Endpunkten in Logik-Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Azure AD-Mandanten-ID** (erforderlich): Es ist im Azure-Portal erforderlich, dass Sie eine [Azure AD-App (Active Directory) erstellen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal), damit überprüft werden kann, ob die Verbindung zwischen unseren beiden Diensten über eine authentifizierte Kommunikation erfolgt. Um die [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) zu ermitteln, wechseln Sie zu Azure Active Directory, wählen Sie **Eigenschaften** aus, und suchen Sie nach der Nummer, die unter **Verzeichnis-ID** aufgeführt ist (z. B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD-App-ID** (erforderlich): Sie benötigen auch Ihre [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) und einen Authentifizierungsschlüssel. Um diese Werte zu ermitteln, wechseln Sie zu Azure Active Directory, und wählen Sie **App-Registrierungen** aus. Suchen Sie dann nach der Nummer die unter **Anwendungs-ID** aufgeführt ist (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Um den Authentifizierungsschlüssel zu suchen, wechseln Sie zu **Einstellungen**, und wählen Sie **Schlüssel** aus. Sie müssen eine Beschreibung und eine Dauer angeben und erhalten dann einen Zahlenwert.

>[!Note]
>Die Azure-Anwendungs-ID ist Ihrer Herausgeber-ID zugeordnet. Stellen Sie also sicher, dass in all Ihren Angeboten dieselbe Anwendungs-ID verwendet wird.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="plan-overview"></a>Planübersicht

Auf dieser Seite können Sie verschiedene Planoptionen innerhalb des gleichen Angebots angeben. Diese Pläne (auch als SKUs bezeichnet) können sich in Bezug auf Version, Monetarisierung und Dienstebenen unterscheiden. Sie müssen mindestens einen Plan einrichten, um Ihr Angebot im Marketplace zu verkaufen.

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

**Pauschalgebühr**: Ermöglichen Sie den Zugriff auf Ihr Angebot mit einer einzelnen monatlichen oder jährlichen Pauschalgebühr. Dies wird manchmal als arbeitsplatzbasierter Preis bezeichnet. Dieses Preismodell ermöglicht Ihnen das optionale Definieren von Volumentarifen, bei denen die Marketplace-Messungsdienst-API zur Berechnung für Kunden nach nicht standardmäßigen Einheiten verwendet wird.  Weitere Informationen zur getakteten Abrechnung finden Sie unter [Getaktete Abrechnung mit dem Marketplace-Messungsdienst](./saas-metered-billing.md).

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

Wenn Sie Informationen zu Kundenabonnements abrufen möchten, die derzeit an einer kostenlosen Testversion teilnehmen, verwenden Sie die neue API-Eigenschaft `isFreeTrial`, die als „true“ oder „false“ gekennzeichnet ist. Weitere Informationen finden Sie unter der [API zum Abrufen von SaaS-Abonnements](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#get-subscription).

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

![Liste der Marketplace-Beispielpläne mit Hinweisen](./media/marketplace-plan.svg)

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
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Sie müssen zum Partner Center zurückkehren und **Live schalten** auswählen, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

## <a name="next-step"></a>Nächster Schritt

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
