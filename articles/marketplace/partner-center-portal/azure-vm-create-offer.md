---
title: Erstellen eines Azure-VM-Angebots im Azure Marketplace
description: Erfahren Sie, wie Sie ein VM-Angebot im Azure Marketplace mit der gewünschten SKU erstellen.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 31b8960f5617566a72545510cf03771f7a3bfcbd
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82745002"
---
# <a name="create-an-azure-virtual-machine-offer-in-the-azure-marketplace"></a>Erstellen eines Azure-VM-Angebots im Azure Marketplace

> [!IMPORTANT]
> Wir verlagern die Verwaltung Ihrer Azure-VM-Angebote vom Cloud-Partnerportal ins Partner Center. Befolgen Sie bis zum Abschluss der Migration Ihrer Angebote die Anweisungen zum Verwalten Ihrer Angebote im Cloud-Partnerportal in [Erstellen eines VM-Angebots](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-offer).

In diesem Artikel wird beschrieben, wie Sie ein Azure-VM-Angebot im [Azure Marketplace](https://azuremarketplace.microsoft.com/) erstellen und veröffentlichen. Er bezieht sich auf virtuelle Computer auf Windows- und Linux-Basis, die ein Betriebssystem, eine virtuelle Festplatte (VHD) und bis zu 16 Datenträger enthalten. Bevor Sie beginnen, [erstellen Sie ein Konto im kommerziellen Marketplace in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), sofern dies noch nicht geschehen ist. Vergewissern Sie sich, dass Ihr Konto im Programm „Kommerzieller Marketplace“ registriert ist.

## <a name="introduction"></a>Einführung

### <a name="publishing-benefits"></a>Vorteile der Veröffentlichung

Die Veröffentlichung auf Azure Marketplace bietet die folgenden Vorteile:

- Bewerben Sie Ihr Unternehmen mithilfe der Marke Microsoft
- Erreichen Sie mehr als 100 Millionen Office 365- und Dynamics 365-Benutzer und über 200.000 Organisationen im Azure Marketplace
- Gewinnen Sie hochwertige Leads in diesen Marketplaces
- Lassen Sie Ihre Dienste durch die Außendienst- und Telefonmarketingteams von Microsoft bewerben

### <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie den [Leitfaden für die Veröffentlichung von VM-Angeboten](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines) und dieses Azure-VM-Material, falls Sie dies noch nicht getan haben:

- Schnellstarthandbücher
  - [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/)
  - [Azure-Schnellstartvorlagen auf GitHub](https://github.com/azure/azure-quickstart-templates)
- Tutorials
  - [Virtuelle Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
  - [Virtuelle Windows-Computer](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- Beispiele
  - [Azure CLI-Beispiele für Linux-VMs](https://docs.microsoft.com/azure/virtual-machines/linux/cli-samples)
  - [Azure PowerShell für Linux-VMs](https://docs.microsoft.com/azure/virtual-machines/linux/powershell-samples)
  - [Azure CLI-Beispiele für Windows-VMs](https://docs.microsoft.com/azure/virtual-machines/windows/cli-samples)
  - [Azure PowerShell für Windows-VMs](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-quick)

### <a name="fundamentals-in-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure-Plattform und die Technologien erforderlich, die verwendet werden, um das Angebot zu erstellen.

Ihr Entwicklerteam sollte sich mit den folgenden Microsoft-Technologien auskennen:

- Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/)
- [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage#storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking#networking)

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie auf der Übersichtsseite **+ Neues Angebot** > **Azure Virtual Machine** aus.

    ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-azure-vm.png)

> [!NOTE]
> Nach der Veröffentlichung eines Angebots werden die daran im Partner Center vorgenommenen Änderungen erst nach der erneuten Veröffentlichung des Angebots in der digitalen Ladenzeile angezeigt. Achten Sie darauf, nach dem Vornehmen von Änderungen immer erneut zu veröffentlichen.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure PowerShell und Azure CLI angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

- Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-setup"></a>Angebotseinrichtung

### <a name="test-drive"></a>Testversion

Die Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, vor dem Kauf einen Test durchzuführen. Dies führt zu einer höheren Konversionsrate und zur Generierung von aussichtsreichen Leads. [Erfahren Sie mehr über Testversionen.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Zusätzliche Ressourcen zu Testversionen:

- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Bewährte Marketingmethoden](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [PDF mit Übersicht zu Testversionen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (Stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="lead-management"></a>Leadverwaltung

Wenn Sie Ihr Angebot mithilfe des Partner Centers im kommerziellen Marketplace veröffentlichen, müssen Sie es mit Ihrem CRM-System (Customer Relationship Management) verbinden. Dadurch empfangen Sie Kontaktinformationen zum Kunden, sobald jemand sein Interesse an Ihrem Produkt bekundet oder es verwendet. Das Herstellen einer Verbindung mit einem CRM ist erforderlich, wenn Sie **Testversion** aktivieren (siehe vorheriger Abschnitt), andernfalls ist es optional.

1. Wählen Sie ein Leadziel aus, an das wir Kundenleads senden sollen. Das Partner Center unterstützt die folgenden CRM-Systeme:
    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) for Customer Engagement
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Wenn Ihr CRM-System oben nicht aufgeführt ist, verwenden Sie eine [Azure-Tabelle](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) oder einen [HTTPS-Endpunkt](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https), um die Leaddaten von Kunden zu speichern. Exportieren Sie die Daten dann in Ihr CRM-System.

2. Verbinden Sie Ihr Angebot bei der Veröffentlichung im Partner Center mit dem Leadziel.
3. Vergewissern Sie sich, dass die Verbindung mit dem Leadziel ordnungsgemäß konfiguriert ist. Nach der Veröffentlichung Ihres Angebots im Partner Center überprüfen wir die Verbindung und senden Ihnen einen Testlead. Während Sie das Angebot vor der Liveschaltung als Vorschau anzeigen, können Sie Ihre Leadverbindung ebenfalls testen, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung bereitzustellen.
4. Achten Sie darauf, dass die Verbindung mit dem Leadziel immer auf dem neuesten Stand ist, damit keine Leads verloren gehen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf dieser Seite können Sie die Kategorien und Branchen, die zum Gruppieren Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren.

### <a name="categories"></a>Kategorien

Wählen Sie mindestens eine und höchstens fünf Kategorien aus. Diese Kategorien werden verwendet, um Ihr Angebot in den geeigneten Marketplace-Suchbereichen zu platzieren. Erläutern Sie in der Angebotsbeschreibung, inwiefern Ihr Angebot diesen Kategorien entspricht. VM-Angebote werden in Azure Marketplace unter der Kategorie **Compute** angezeigt.

### <a name="legal"></a>Rechtliche Hinweise

Sie müssen die Nutzungsbedingungen für das Angebot angeben. Sie haben zwei Möglichkeiten:

- Verwenden Ihrer eigenen Geschäftsbedingungen
- Verwenden des Standardvertrags für den kommerziellen Microsoft-Marketplace

#### <a name="use-your-own-terms-and-conditions"></a>Verwenden Ihrer eigenen Geschäftsbedingungen

Um eigene Geschäftsbedingungen anzugeben, geben Sie bis zu 10.000 Zeichen Text im Feld **Geschäftsbedingungen** ein. Wenn Sie eine längere Beschreibung benötigen, geben Sie eine einzelne Webadresse ein, unter der Ihre Geschäftsbedingungen zu finden sind. Er wird für Kunden als aktiver Link angezeigt.

Kunden müssen diesen Bestimmungen zustimmen, bevor sie Ihr Angebot testen können.

#### <a name="use-the-standard-contract-for-the-microsoft-commercial-marketplace"></a>Verwenden des Standardvertrags für den kommerziellen Microsoft-Marketplace

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Reduzierung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft eine Vorlage für Standardverträge für den kommerziellen Marketplace bereit. Wenn Sie Ihre Software unter dem Standardvertrag anbieten, müssen Kunden ihn nur einmal lesen und akzeptieren, und Sie müssen keine benutzerdefinierten Geschäftsbedingungen erstellen.

Um den Standardvertrag zu verwenden, aktivieren Sie das Kontrollkästchen **Standardvertrag für den kommerziellen Marketplace von Microsoft verwenden**, und wählen Sie dann im Popupfenster **Akzeptieren** aus (Sie müssen möglicherweise nach oben scrollen, um es anzuzeigen).

![Die Übersichtsseite im Partner Center mit der Schaltfläche „Neues Angebot“ und dem ausgewählten Dienstangebot „Consulting“ wird veranschaulicht.](media/use-standard-contract.png)

> [!NOTE]
> Nachdem Sie ein Angebot mit dem Standardvertrag für den kommerziellen Marketplace veröffentlicht haben, können Sie keine eigenen benutzerdefinierten Geschäftsbedingungen mehr verwenden. Entweder bieten Sie Ihre Lösung im Rahmen des Standardvertrags **oder** Ihrer eigenen Geschäftsbedingungen an.

Weitere Informationen zum Standardvertrag finden Sie unter [Standardvertrag für den kommerziellen Microsoft-Marketplace](https://docs.microsoft.com/azure/marketplace/standard-contract). Sie können den [Standardvertrag](https://go.microsoft.com/fwlink/?linkid=2041178) als PDF-Datei herunterladen (achten Sie darauf, Ihren Popupblocker zu deaktivieren).

##### <a name="standard-contract-amendments"></a>Zusatzvereinbarungen für den Standardvertrag

Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Sie der Einfachheit halber den Standardvertrag auswählen und die Bedingungen für ihr Produkt oder Unternehmen erstellen. Kunden, die bereits den Microsoft-Standardvertrag geprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag prüfen. Es stehen zwei Arten von Änderungen zur Verfügung: universell und benutzerdefiniert.

**Universelle Zusatzvereinbarungen**: Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Sie werden jedem Kunden des Angebots im Rahmen der Kaufabwicklung angezeigt. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarung(en) akzeptieren, bevor sie Ihr Angebot nutzen können. Sie können eine universelle Zusatzvereinbarung pro Angebot angeben. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Die Bestimmungen werden Kunden während des Ermittlungs- und Kaufprozesses in AppSource, dem Azure Marketplace und dem Azure-Portal angezeigt.

**Benutzerdefinierte Zusatzvereinbarungen**: Dies sind spezielle Zusatzvereinbarungen für den Standardvertrag, die über Azure-Mandanten-IDs nur bestimmte Kunden betreffen. Sie können den gewünschten Mandanten auswählen. Nur den Kunden des Mandanten werden die Bedingungen der Zusatzvereinbarungen im Rahmen der Kaufabwicklung für das Angebot präsentiert. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarung(en) akzeptieren, bevor sie Ihr Angebot nutzen können.

Wählen Sie zunächst **Benutzerdefinierte Zusatzvereinbarungen hinzufügen (Max. 10)** aus. Sie können bis zu zehn benutzerdefinierte Zusatzvereinbarungen pro Angebot angeben.

- **Benutzerdefinierte Zusatzvereinbarungen**: Geben Sie Ihre eigenen Zusatzvereinbarungen in das Feld für benutzerdefinierte Zusatzvereinbarungen ein. Sie können eine unbegrenzte Anzahl von Zeichen eingeben. Diese benutzerdefinierten Vereinbarungen werden im Azure-Portal im Rahmen der Kaufabwicklung für das Angebot nur für Kunden mit den Mandanten-IDs angezeigt, die Sie für diese benutzerdefinierten Vereinbarungen angeben.
- **Mandanten-IDs** (erforderlich): Jede benutzerdefinierte Zusatzvereinbarung kann für bis zu 20 Mandanten-IDs gelten. Wenn Sie eine benutzerdefinierte Zusatzvereinbarung hinzufügen, müssen Sie mindestens eine Mandanten-ID angeben, die Ihren Kunden in Azure identifiziert. Ihr Kunde kann in Azure unter  und dann „Eigenschaften“ nach Ihnen suchen. Der Wert „Verzeichnis-ID“ ist die Mandanten-ID (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können die von Ihrer Organisation zugeteilte Mandanten-ID Ihres Kunden auch suchen, indem Sie unter [Wie lautet meine Mandanten-ID für Microsoft Azure und Office 365?](https://www.whatismytenantid.com/) die Domänennamen-Webadresse des Kunden verwenden.
- **Beschreibung** (optional): Sie können optional eine aussagekräftige Beschreibung für die Mandanten-ID hinzufügen, mit der Sie den Kunden, für den die Zusatzvereinbarung gelten soll, einfacher identifizieren können.

> [!NOTE]
> Diese beiden Arten von Zusatzvereinbarungen sind übereinander angeordnet. Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarung für den Standardvertrag.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Auf dieser Seite können Sie Angebotsdetails wie Name, Beschreibung, Links und Kontakte definieren.

> [!NOTE]
> Die Inhalte der Angebotsliste (z. B. Beschreibung, Dokumente, Screenshots und Nutzungsbedingungen) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf [nicht englische Sprache] verfügbar.“ Sie können auch eine _Nützliche Webadresse_ bereitstellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="marketplace-details"></a>Marketplace-Details

#### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text ausgefüllt, den Sie beim Erstellen des Angebots im Feld **Angebotsalias** eingegeben haben. Sie können diesen Namen später ändern.

Der Name:

- Kann markenrechtlich geschützt sein (und Sie können Marken- und Copyrightsymbole einschließen).
- Darf nicht mehr als 50 Zeichen lang sein.
- Darf keine Emojis enthalten.

#### <a name="search-results-summary"></a>Zusammenfassung der Suchergebnisse

Eine kurze Beschreibung Ihres Angebots. Sie kann bis zu 100 Zeichen lang sein und wird in den Marketplace-Suchergebnissen verwendet.

#### <a name="long-summary"></a>Lange Zusammenfassung

Geben Sie eine längere Beschreibung Ihres Angebots an. Sie kann bis zu 256 Zeichen lang sein und wird in den Marketplace-Suchergebnissen verwendet.

#### <a name="description"></a>BESCHREIBUNG

Geben Sie eine detaillierte Beschreibung Ihres Angebots an (bis zu 3.000 Zeichen). Sie wird Kunden in der Auflistungsübersicht des kommerziellen Marketplace angezeigt.

Schließen Sie eine oder mehrere der folgenden Informationen in die Beschreibung ein:

- Den Nutzen und die wichtigsten Vorteile Ihres Angebots
- Zuordnungen zu Kategorien oder Branchen oder beidem
- Möglichkeit zu In-App-Käufen
- Alle erforderlichen Bekanntmachungen

Hier einige Tipps zum Verfassen Ihrer Beschreibung:

- Schildern Sie in den ersten Sätzen Ihrer Beschreibung deutlich das Nutzenversprechen Ihres Angebots. Berücksichtigen Sie folgende Punkte:
  - Beschreibung Ihres Angebots.
  - Den Typ von Benutzern, denen Ihr Angebot nützt.
  - Kundenbedürfnisse oder -probleme, an die sich das Angebot richtet.
- Beachten Sie, dass die ersten Sätze möglicherweise in den Suchmaschinenergebnissen angezeigt werden.
- Verlassen Sie sich nicht nur auf Features und Funktionen, um Ihr Angebot zu verkaufen. Konzentrieren Sie stattdessen auf den Nutzen, den Ihr Angebot bietet.
- Verwenden Sie branchenspezifische oder den Nutzen betonende Wörter.

Formatieren Sie Ihre Angebotsbeschreibung mithilfe des Rich-Text-Editors, um sie ansprechender zu gestalten.

![Verwenden des Rich-Text-Editors](./media/rich-text-editor.png)

| <center>Textformat ändern | <center>Aufzählungszeichen oder Nummerierung hinzufügen | <center>Texteinzüge hinzufügen oder entfernen |
| --- | --- | --- |
| <center>![Verwenden des Rich-Text-Editors zum Ändern des Textformats](./media/text-editor3.png) |  <center>![Verwenden des Rich-Text-Editors zum Hinzufügen von Listen](./media/text-editor4.png) |  <center>![Verwenden des Rich-Text-Editors für Einzüge](./media/text-editor5.png) |

#### <a name="privacy-policy-link"></a>Link zur Datenschutzrichtlinie

Geben Sie die Webadresse (URL) der Datenschutzrichtlinie Ihrer Organisation ein. Stellen Sie sicher, dass Ihr Angebot den Datenschutzbestimmungen und -vorschriften entspricht. Sie müssen auch eine gültige Datenschutzrichtlinie auf Ihrer Website veröffentlichen.

### <a name="useful-links"></a>Nützliche Links

Stellen Sie ergänzende Onlinedokumente zu Ihrem Angebot bereit. Um einen Link hinzuzufügen, wählen Sie **+ Link hinzufügen** aus, und füllen Sie dann die folgenden Felder aus:

- **Name**: Für Kunden wird der Name auf der Seite „Details“ angezeigt.
- **Link (URL)** : Geben Sie einen Link für Kunden ein, um Ihr Onlinedokument anzuzeigen.

### <a name="customer-support-links"></a>Kundensupportlinks

Geben Sie die Supportwebsite an, auf der Kunden Ihr Supportteam erreichen können.

- Azure Global-Supportwebsite
- Azure Government-Supportwebsite

### <a name="partner-support-contact"></a>Partnersupportkontakt

Geben Sie Kontaktinformationen für Microsoft-Partner an, die sie verwenden können, wenn Ihre Kunden ein Supportticket öffnen. Dies wird nicht im Marketplace aufgeführt.

- Name
- Email
- Phone

### <a name="engineering-contact"></a>Engineering-Kontakt

Geben Sie Kontaktinformationen an, die Microsoft verwenden soll, wenn Probleme (einschließlich Zertifizierungsprobleme) bei Ihrem Angebot auftreten. Dies wird nicht im Marketplace aufgeführt.

- Name
- Email
- Phone

### <a name="marketplace-media"></a>Marketplace-Medien

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Bei unscharfen Bildern wird Ihre Übermittlung abgelehnt.

>[!Note]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="marketplace-logos"></a>Marketplace-Logos

Stellen Sie PNG-Dateien des Logos Ihres Angebots in den folgenden vier Pixelgrößen bereit:

- **Klein** (48 x 48)
- **Mittel** (90 x 90)
- **Groß** (216 x 216)
- **Breit** (255 x 115)

Alle vier Logos sind erforderlich und werden an verschiedenen Stellen in der Marketplace-Auflistung verwendet.

#### <a name="screenshots"></a>Screenshots

Fügen Sie bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Jeder Screenshot muss 1.280 x 720 Pixel groß sein und im PNG-Format vorliegen. Jeder Screenshot muss einen Titel enthalten.

#### <a name="videos"></a>Videos

Fügen Sie optional bis zu fünf Videos hinzu, die Ihr Angebot vorstellen. Diese sollten von einem externen Videodienst gehostet werden. Geben Sie den Namen jedes Videos, die Webadresse und ein Miniaturbild des Videos im PNG-Format mit einer Größe von 1280 × 720 Pixel ein.

Weitere Ressourcen für Marketplace-Listen finden Sie unter [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="preview"></a>Vorschau

Auf der Registerkarte „Vorschau“ können Sie eine eingeschränkte **Vorschauzielgruppe** zur Überprüfung Ihres Angebots auswählen, bevor Sie es live für die breitere Zielgruppe der Marketplacebenutzer veröffentlichen.

> [!IMPORTANT]
> Nachdem Sie Ihr Angebot in der Vorschau überprüft haben, wählen Sie **Live schalten** aus, um es für die öffentliche Zielgruppe des kommerziellen Marketplace zu veröffentlichen.

Ihre Vorschauzielgruppe wird durch Azure-Abonnement-ID-GUIDs zusammen mit einer optionalen Beschreibung für jede Gruppe identifiziert. Keines dieser Felder ist für Kunden sichtbar. Sie finden Ihre Azure-Abonnement-ID auf der Seite **Abonnements** im Azure-Portal.

Fügen Sie mindestens eine Azure-Abonnement-ID hinzu, entweder einzeln (bis zu 10) oder durch Hochladen einer CSV-Datei (bis zu 100). Durch Hinzufügen dieser Abonnement-IDs definieren Sie, wer Ihr Angebot als Vorschau anzeigen kann, bevor es live veröffentlicht wird. Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

> [!NOTE]
> Eine Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, _bevor_ es in den Marketplaces live veröffentlicht wird. Sie kann alle Pläne sehen und überprüfen, einschließlich der Pläne, die nach der vollständigen Veröffentlichung Ihres Angebots im Marketplace nur für eine private Zielgruppe verfügbar sind. Eine private Zielgruppe (definiert in der Registerkarte **Preise und Verfügbarkeit**) hat exklusiven Zugriff auf einen bestimmten Plan.

Wählen Sie **Entwurf speichern** aus, bevor Sie mit dem nächsten Abschnitt „Planübersicht“ fortfahren.

## <a name="plan-overview"></a>Planübersicht

Sie können verschiedene Planoptionen innerhalb des gleichen Angebots im Partner Center angeben. Diese Pläne wurden bisher als SKUs bezeichnet. Ein Angebot erfordert mindestens einen Plan, der sich hinsichtlich Monetarisierungszielgruppe, Azure-Regionen, Features oder VM-Images unterscheiden kann.

Nach dem Erstellen Ihres Plans zeigt die Registerkarte **Planübersicht** folgende Informationen an:

- Plannamen
- Lizenzmodelle
- Zielgruppe (öffentlich oder privat)
- Aktueller Veröffentlichungsstatus
- Verfügbare Aktionen

In der Planübersicht verfügbare Aktionen unterscheiden sich je nach dem aktuellen Status Ihres Plans. Dazu gehören:

- **Entwurf löschen**: Wenn der Planstatus ein Entwurf ist.
- **Verkauf des Plans einstellen** oder **Private Zielgruppe synchronisieren**: Wenn der Planstatus live veröffentlicht wird.

### <a name="create-new-plan"></a>Erstellen eines neuen Plans

Wählen Sie oben **+ Neuen Plan erstellen** aus. Das Dialogfeld **Neuer Plan** wird angezeigt.

Erstellen Sie im Feld **Plan-ID** für jeden Plan im Angebot eine eindeutige Plan-ID. Diese ID ist für Kunden in der Webadresse des Produkts sichtbar. Verwenden Sie nur Kleinbuchstaben und Zahlen, Bindestriche oder Unterstriche bei maximal 50 Zeichen.

> [!NOTE]
> Die Plan-ID kann nicht mehr geändert werden, nachdem Sie **Erstellen** ausgewählt haben.

Geben Sie im Feld **Planname** einen Namen für diesen Plan ein. Kunden sehen diesen Namen, wenn sie sich für einen der Pläne in Ihrem Angebot interessieren. Erstellen Sie einen eindeutigen Namen, der die Unterschiede der einzelnen Pläne eindeutig hervorhebt. Beispielsweise könnten Sie **Windows Server** mit den Plänen **Nutzungsbasierte Bezahlung**, **BYOL**, **Erweitert** und **Enterprise** verwenden.

Klicken Sie auf **Erstellen**.

### <a name="plan-setup"></a>Plansetup

Legen Sie die allgemeine Konfiguration für den Plantyp fest, und geben Sie an, ob die technische Konfiguration von einem anderen Plan wiederverwendet wird und in welchen Azure-Regionen der Plan verfügbar sein soll. Ihre Auswahl hier bestimmt, welche Felder auf anderen Registerkarten desselben Plans angezeigt werden.

#### <a name="re-use-technical-configuration"></a>Wiederverwenden von technischen Konfigurationen

Wenn Sie über mehrere Pläne desselben Typs verfügen und deren Pakete identisch sind, können Sie auswählen, dass **dieser Plan technische Konfiguration aus einem anderen Plan wiederverwendet**. Mit dieser Option können Sie einen der anderen Pläne desselben Typs für dieses Angebot auswählen und seine technische Konfiguration wiederverwenden.

> [!NOTE]
> Wenn Sie die technische Konfiguration aus einem anderen Plan wiederverwenden, wird die gesamte Registerkarte **Technische Konfiguration** nicht mehr in diesem Plan angezeigt. Die Details der technischen Konfiguration aus dem anderen Plan, einschließlich aller Updates, die Sie in Zukunft vornehmen, werden für diesen Plan ebenfalls verwendet. Diese Einstellung kann nicht geändert werden, sobald der Plan veröffentlicht wird.

#### <a name="azure-regions"></a>Azure-Regionen

Ihr Plan muss in mindestens einer Azure-Region verfügbar gemacht werden.

Wählen Sie die Option **Azure Global** aus, um Ihren Plan für Kunden in allen globalen Azure-Regionen mit Integration des kommerziellen Marketplace verfügbar zu machen. Details finden Sie unter [Geografische Verfügbarkeit und Unterstützung von Währungen](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Wählen Sie die Option **Azure Government** aus, um Ihren Plan in der [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)-Region verfügbar zu machen. Diese Region bietet kontrolliertem Zugriff für Kunden der US-Behörden auf Bundes-, Bundesstaats-, Gemeinde- oder Stammesgebietsebene sowie für Partner, die diese beliefern können. Als Herausgeber sind Sie für alle Kompatibilitätskontrollen, Sicherheitsmaßnahmen und bewährten Methoden verantwortlich. Azure Government verwendet physisch isolierte Rechenzentren und Netzwerke (die sich ausschließlich in den USA befinden).

Testen und validieren Sie Ihren Plan vor dem Veröffentlichen für [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners) in der Umgebung, da sich bestimmte Endpunkte möglicherweise unterscheiden. Fordern Sie für Einrichten und Testen Ihres Plans ein Testkonto bei [Microsoft Azure Government-Test](https://azure.microsoft.com/global-infrastructure/government/request/) an.

> [!NOTE]
> Nachdem Ihr Plan veröffentlicht und in einer bestimmten Azure-Region verfügbar gemacht wurde, können Sie diese Region nicht mehr entfernen.

#### <a name="azure-government-certifications"></a>Azure Government-Zertifizierungen

Diese Option wird nur angezeigt, wenn Sie **Azure Government** ausgewählt haben.

Azure Government-Dienste verarbeiten Daten, die bestimmten behördlichen Vorschriften und Anforderungen unterliegen. Beispielsweise FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 und CJIS. Um bei Ihren Zertifizierungen ein Bewusstsein für diese Programme zu schaffen, können Sie bis zu 100 Links bereitstellen, in denen sie beschrieben werden. Diese können entweder direkt mit ihrem Listing im Programm verknüpft sein oder zu Beschreibungen der Konformität mit diesen Programmen auf Ihren eigenen Websites weiterleiten. Diese Links sind nur für Azure Government-Kunden sichtbar.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="plan-listing"></a>Planlisting

Hier konfigurieren Sie die Listendetails des Plans. Auf dieser Registerkarte werden spezifische Informationen angezeigt, die sich je nach Plan innerhalb desselben Angebots unterscheiden können.

#### <a name="plan-name"></a>Planname

Hier ist bereits der Name eingetragen, den Sie Ihrem Plan beim Erstellen gegeben haben. Dieser Name wird im Marketplace als Titel dieses Plans angezeigt und ist auf 100 Zeichen beschränkt.

#### <a name="plan-summary"></a>Planzusammenfassung

Geben Sie eine kurze Zusammenfassung Ihres Plans (nicht des Angebots) an. Diese Zusammenfassung darf maximal 100 Zeichen enthalten.

#### <a name="plan-description"></a>Planbeschreibung

Beschreiben Sie, was Ihren Softwareplan von anderen unterscheidet und worin die Unterschiede zwischen den Plänen in Ihrem Angebot bestehen. Beschreiben Sie nicht das Angebot, sondern nur den Plan. Die Planbeschreibung kann bis zu 2.000 Zeichen enthalten.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Auf dieser Registerkarte konfigurieren Sie Folgendes:

- Märkte, in denen dieser Plan verfügbar sein wird
- Der Preis pro Stunde
- Ob der Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) angezeigt werden soll

#### <a name="markets"></a>Märkte

Jeder Plan muss in mindestens einem Markt verfügbar sein. Aktivieren Sie das Kontrollkästchen für jeden Marktstandort, in dem dieser Plan zum Kauf angeboten werden soll (Benutzer in diesen Märkten können das Angebot nach wie vor in allen Azure-Regionen bereitstellen, die in **[Plansetup](#plan-setup)** ausgewählt wurden). Mit der Schaltfläche **Bezahlte Steuern** können Sie Länder anzeigen, in denen Microsoft für Sie Verkaufserlöse und Steuern überweist. Die Veröffentlichung in China beschränkt sich auf entweder **kostenlose** oder **BYOL**-Pläne (Bring Your Own License).

Wenn Sie bereits Preise in US-Dollar für Ihren Plan festgelegt haben und einen anderen Marktstandort hinzufügen, wird der Preis für den neuen Markt entsprechend dem aktuellen Wechselkurs berechnet. Überprüfen Sie vor dem Veröffentlichen immer den Preis für jeden Markt. Die Preise können nach dem Speichern der Änderungen mithilfe des Links **Preise exportieren (XLSX)** überprüft werden.

Wenn Sie einen Markt entfernen, können Kunden dieses Markts, die aktive Bereitstellungen nutzen, weder neue Bereitstellungen erstellen noch ihre vorhandenen Bereitstellungen nach oben skalieren. Vorhandene Bereitstellungen sind nicht betroffen.

#### <a name="pricing"></a>Preise

**Lizenzmodell**: Wählen Sie **Nutzungsbasierter monatlicher Fakturierungsplan** aus, um die Preise für diesen Plan zu konfigurieren, oder **BYOL (Bring Your Own License)** , damit Kunden diesen Plan mit Ihrer vorhandenen Lizenz verwenden können.

Verwenden Sie für einen nutzungsbasierten monatlichen Fakturierungsplan eine der drei folgenden Preisoptionen:

- **Pro Kern**: Geben Sie den Preis pro Kern in US-Dollar (USD) an. Wir berechnen die Preise pro Kerngröße und konvertieren sie mit dem aktuellen Wechselkurs in lokale Währungen.
- **Preis pro Kerngröße**: Geben Sie die Preise pro Kerngröße in US-Dollar (USD) an. Wir konvertieren die Preise mit dem aktuellen Wechselkurs in lokale Währungen.
- **Pro Markt und Kerngröße**: Geben Sie die Preise für die einzelnen Kerngrößen für alle Märkte an. Sie können Preise aus einer Kalkulationstabelle importieren.

> [!NOTE]
> Speichern Sie Preisänderungen, damit die Preisdaten exportiert werden können. Nachdem ein Preis für einen Markt in Ihrem Plan veröffentlicht wurde, kann er nicht mehr geändert werden. Stellen Sie vor der Veröffentlichung dieser Preise sicher, dass sie richtig sind, indem Sie die Preistabellenkalkulation exportieren und die Preise in jedem Markt überprüfen.

#### <a name="free-trial"></a>Kostenlose Testversion

Sie können Ihren Kunden eine kostenlose Testversion für einen Monat oder drei Monate anbieten.

#### <a name="visibility"></a>Sichtbarkeit

Sie können jeden Plan so entwerfen, dass er für alle Benutzer oder nur für eine vorab ausgewählte Zielgruppe sichtbar ist. Weisen Sie mithilfe von Azure Abonnement-IDs die Mitgliedschaften in dieser eingeschränkten Zielgruppe zu.

**Öffentlich**: Ihr Plan kann von allen Benutzern eingesehen werden.

**Private Zielgruppe**: Machen Sie Ihren Plan nur für eine vorab ausgewählte Zielgruppe sichtbar. Nachdem er als privater Plan veröffentlicht wurde, können Sie die Zielgruppe aktualisieren oder ihn in „öffentlich“ ändern. Nachdem Sie einen Plan öffentlich gemacht haben, muss er öffentlich bleiben. Sie können ihn nicht wieder in „privat“ ändern.

**Eingeschränkte Zielgruppe (Azure-Abonnement-IDs)** : Weisen Sie die Zielgruppe, die auf diesen privaten Plan zugreifen darf, mit Azure-Abonnement-IDs zu. Optional können Sie eine Beschreibung der einzelnen zugewiesenen Azure-Abonnement-IDs einschließen. Fügen Sie bis zu 10 Abonnement-IDs manuell hinzu, oder 20.000, indem Sie ein CSV-Arbeitsblatt importieren. Azure-Abonnement-IDs werden als GUIDs dargestellt. Buchstaben müssen in Kleinbuchstaben angegeben werden.

> [!NOTE]
> Eine private oder eingeschränkte Zielgruppe unterscheidet sich von der auf der Registerkarte **Vorschau** definierten Vorschauzielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, _bevor_ es in den Marketplaces live veröffentlicht wird. Die Auswahl der privaten Zielgruppe gilt nur für einen bestimmten Plan, aber die private Zielgruppe kann alle Pläne (private und nicht private Pläne) zu Validierungszwecken anzeigen.

#### <a name="hide-plan"></a>Plan ausblenden

Wenn Ihre VM nur indirekt bei Verweisen über eine andere Lösungsvorlage oder verwaltete Anwendung bereitgestellt werden soll, aktivieren Sie dieses Kontrollkästchen, um Ihre VM zu veröffentlichen, aber für Kunden, die danach direkt suchen oder blättern, auszublenden.

> [!NOTE]
> Ausgeblendete Pläne unterstützen keine Vorschaulinks.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="technical-configuration"></a>Technische Konfiguration

Geben Sie die Images und andere technische Eigenschaften an, die diesem Plan zugeordnet sind. Weitere Informationen finden Sie unter [Erstellen einer technischen Azure-VM-Ressource](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-technical-assets).

> [!NOTE]
> Diese Registerkarte wird nicht angezeigt, wenn Sie den Plan auf der Registerkarte **Plansetup** so konfiguriert haben, dass Pakete aus einem anderen Plan wiederverwendet werden.

#### <a name="operating-system"></a>Betriebssystem

- **Betriebssystemfamilie**: Wählen Sie das Betriebssystem **Windows** oder **Linux** aus.
- **Release** oder **Anbieter**: Wählen Sie das Windows-Release oder den Linux-Anbieter aus.
- **Anzeigename für Betriebssystem**: Wählen Sie einen benutzerfreundlichen Betriebssystemnamen aus. Dieser Name ist für Kunden sichtbar.

#### <a name="recommended-vm-sizes"></a>Empfohlene VM-Größen

Wählen Sie bis zu sechs empfohlene VM-Größen für die Anzeige im Azure Marketplace aus.

#### <a name="open-ports"></a>Öffnen von Ports

Öffnen Sie öffentliche oder private Ports auf einem bereitgestellten virtuellen Computer.

#### <a name="storage-option-for-deployment"></a>Speicheroption für die Bereitstellung

**Datenträgerbereitstellungs-Option**: Wählen Sie die Datenträgerbereitstellung aus, die Ihre Benutzer beim Einsatz des virtuellen Computers verwenden können. Microsoft empfiehlt, die Bereitstellung auf verwaltete Datenträger einzuschränken.

#### <a name="properties"></a>Eigenschaften

**Unterstützung des beschleunigten Netzwerkbetriebs**: Wählen Sie aus, ob Ihr virtueller Computer [beschleunigten Netzwerkbetrieb](https://go.microsoft.com/fwlink/?linkid=2124513) unterstützt.

#### <a name="vm-images"></a>VM-Images

Geben Sie eine Datenträgerversion und den SAS-URI für die Images der VM an. Fügen Sie bis zu 16 Datenträger für jedes VM-Image hinzu. Geben Sie nur eine neue Imageversion pro Plan in einer bestimmten Übermittlung an. Nachdem ein Image veröffentlicht wurde, können Sie es nicht mehr bearbeiten, aber Sie können es löschen. Das Löschen einer Version verhindert, dass neue und vorhandene Benutzer eine neue Instanz der gelöschten Version bereitstellen.

- **Datenträgerversion** ist die Version des Images, das Sie bereitstellen.
- **SAS-URI** ist der Speicherort in Azure Storage, auf dem Sie die Betriebssystem-VHD gespeichert haben.
- Datenträgerimages sind ebenfalls VHD-SAS-URIs, die in ihrem Azure-Speicher gespeichert sind.
- Fügen Sie einem Plan pro Übermittlung nur ein Image hinzu.

Fügen Sie unabhängig vom verwendeten Betriebssystem immer nur die von der Lösung benötigte Mindestanzahl von Datenträgern hinzu. Kunden können Datenträger, die Teil eines Images sind, zum Zeitpunkt der Bereitstellung nicht entfernen. Sie haben aber immer die Möglichkeit, während oder nach der Bereitstellung Datenträger hinzuzufügen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren, und kehren Sie zur **Planübersicht** zurück.

## <a name="resell-through-csps"></a>Verkaufen über CSPs

Erweitern Sie die Reichweite Ihres Angebots, indem Sie es für Partner im [CSP](https://azure.microsoft.com/offers/ms-azr-0145p/)-Programm (Cloud Solution Providers) verfügbar machen. Alle Bring-Your-Own-License-Pläne (BYOL) werden automatisch ausgewählt. Sie können Ihre Nicht-Byol-Pläne auswählen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="test-drive"></a>Testversion

Richten Sie eine Demonstration (Testversion) ein, mit der Kunden Ihr Angebot vor dem Erwerb ausprobieren können. Informationen zum Erstellen einer Demonstrationsumgebung, mit der Kunden Ihr Angebot für einen bestimmten Zeitraum testen können, finden Sie unter [Ermöglichen Sie Ihren Kunden das Testen Ihres Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive).

Aktivieren Sie auf der Registerkarte [Angebotseinrichtung](#test-drive) das Kontrollkästchen „Aktivieren einer Testversion“, um eine Testversion zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Zusätzliche Ressourcen zu Testversionen:

- [Bewährte Marketingmethoden](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Übersicht](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF-Datei, stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="review-and-publish"></a>Überprüfen und veröffentlichen

Nachdem Sie alle erforderlichen Abschnitte des Angebots abgeschlossen haben, können Sie es zum Überprüfen und Veröffentlichen einreichen.

Wählen Sie in der oberen rechten Ecke des Portals **Überprüfen und veröffentlichen** aus.

Auf der Seite haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
  - **Nicht gestartet**: Die Überprüfung des Abschnitts wurde nicht gestartet und muss abgeschlossen werden.
  - **Unvollständig**: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Anweisungen dazu finden Sie in den Abschnitten zum Aktualisieren dieses Abschnitts weiter oben in diesem Dokument.
  - **Abgeschlossen**: Der Abschnitt ist abgeschlossen und es sind keine Fehler aufgetreten. Sie können das Angebot erst einreichen, wenn alle Abschnitte des Angebots abgeschlossen sind.
- **Hinweise zur Zertifizierung**: Stellen Sie dem Zertifizierungsteam Testanweisungen zur Verfügung, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird. Stellen Sie alle ergänzenden Anmerkungen bereit, die das Verständnis Ihrer App unterstützen.

Um das Angebot zur Veröffentlichung einzureichen, wählen Sie **Überprüfen und veröffentlichen** aus.

Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots zur Überprüfung und Genehmigung verfügbar ist. Um Ihr Angebot für die Öffentlichkeit zugänglich zu machen (oder für eine private Zielgruppe, wenn es sich um ein privates Angebot handelt), navigieren Sie zum Partner Center, rufen Sie die Seite **Übersicht** Ihres Angebots aus, und wählen Sie **Live schalten** aus. Der Status Ihres Angebots wird am oberen Rand der Seite angezeigt, wenn die Veröffentlichung ausgeführt wird.

### <a name="errors-and-review-feedback"></a>Fehler und Prüfungsfeedback

Der Schritt **Manuelle Überprüfung** im Veröffentlichungsprozess stellt eine umfassende Überprüfung Ihres Angebots und der zugehörigen technischen Ressourcen dar. Wenn bei diesem Prozess Fehler in Ihrem Angebot aufgedeckt werden, erhalten Sie einen Zertifizierungsbericht, in dem die Probleme erläutert werden. Nehmen Sie einfach die erforderlichen Korrekturen vor, und veröffentlichen Sie Ihr Angebot erneut.

## <a name="offer-overview"></a>Angebotsübersicht

Die Seite **Angebotsübersicht** zeigt eine visuelle Darstellung der zum Veröffentlichen dieses Angebots erforderlichen Schritte (sowohl der abgeschlossenen als auch der noch laufenden) und der erwarteten Dauer ihrer Ausführung.

Diese Seite enthält Links zum Durchführen von Vorgängen für dieses Angebot auf der Grundlage der von Ihnen ausgewählten Optionen. Beispiel:

- Wenn das Angebot einen Entwurf darstellt: [Delete draft offer](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer) (Angebotsentwurf löschen)
- Wenn es sich um ein Liveangebot handelt: [Stop selling the offer](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan) (Angebot nicht mehr verkaufen)
- Wenn das Angebot in der Vorschau ist: [Go-live](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval) (Live schalten)
- Wenn Sie die Abzeichnung des Verlegers nicht abgeschlossen haben: [Veröffentlichung abbrechen](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="marketplace-examples"></a>Marketplace-Beispiele

In diesen Beispielen wird gezeigt, wie das Angebot in Azure Marketplace angezeigt wird.

### <a name="azure-marketplace-offer-details"></a>Azure Marketplace – Angebotsdetails

![Azure Marketplace – Bildschirmbeispiel für Angebotsdetails](media/avm-create1.png)

### <a name="azure-marketplace-search-results"></a>Azure Marketplace – Suchergebnisse

![Azure Marketplace – Bildschirmbeispiel für Suchdetails](media/avm-create2.png)

### <a name="azure-marketplace-plan-details"></a>Azure Marketplace – Plandetails

![Azure Marketplace – Bildschirmbeispiel für Plandetails](media/avm-create3.png)

### <a name="azure-portal-offer-details"></a>Azure Marketplace – Angebotsdetails

![Azure Marketplace – Bildschirmbeispiel für Angebotsdetails](media/avm-create4.png)

### <a name="azure-portal-search-results"></a>Suchergebnisse im Azure-Portal

![Azure-Portal – Bildschirmbeispiel für Suchergebnisse](media/avm-create5.png)

### <a name="azure-portal-plan-details"></a>Azure-Portal – Plandetails

![Azure-Portal – Bildschirmbeispiel für Plandetails](media/avm-create6.png)

## <a name="next-step"></a>Nächster Schritt

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
