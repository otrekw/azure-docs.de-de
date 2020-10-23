---
title: Erstellen eines Azure-VM-Angebots im Azure Marketplace
description: Erfahren Sie, wie Sie ein VM-Angebot im Azure Marketplace veröffentlichen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: mingshen
ms.date: 08/07/2020
ms.openlocfilehash: 46749015d5141053eaea3f2f251bbba7d9455481
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91709015"
---
# <a name="create-an-azure-virtual-machine-offer-on-azure-marketplace"></a>Erstellen eines Azure-VM-Angebots im Azure Marketplace

In diesem Artikel wird beschrieben, wie Sie ein Azure-VM-Angebot im [Azure Marketplace](https://azuremarketplace.microsoft.com/) erstellen und veröffentlichen. Er bezieht sich auf virtuelle Computer auf Windows- und Linux-Basis, die ein Betriebssystem, eine virtuelle Festplatte (VHD) und bis zu 16 Datenträger für Daten enthalten. 

Bevor Sie beginnen, [erstellen Sie in Partner Center ein Konto für den kommerziellen Marketplace](create-account.md). Stellen Sie sicher, dass Ihr Konto im Programm für den kommerziellen Marketplace registriert ist.

## <a name="introduction"></a>Einführung

### <a name="the-benefits-of-publishing-to-azure-marketplace"></a>Vorteile der Veröffentlichung im Azure Marketplace

Wenn Sie Ihre Angebote im Azure Marketplace veröffentlichen, haben Sie folgende Möglichkeiten:

- Bewerben Sie Ihr Unternehmen mit der Unterstützung der Marke Microsoft.
- Erreichen Sie über 100 Millionen Microsoft 365- und Dynamics 365-Benutzer und mehr als 200.000 Unternehmen.
- Gewinnen Sie hochwertige Leads in diesen Marketplaces.
- Lassen Sie Ihre Dienste durch die Außendienstvertriebs- und Telefonmarketingteams von Microsoft bewerben.

### <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie den [Leitfaden für die Veröffentlichung von VM-Angeboten](../marketplace-virtual-machines.md) und dieses Azure-VM-Material, falls Sie dies noch nicht getan haben:

- Schnellstarthandbücher
  - [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/)
  - [Azure-Schnellstartvorlagen auf GitHub](https://github.com/azure/azure-quickstart-templates)
- Tutorials
  - [Virtuelle Linux-Computer](../../virtual-machines/linux/tutorial-manage-vm.md)
  - [Virtuelle Windows-Computer](../../virtual-machines/windows/tutorial-manage-vm.md)
- Beispiele
  - [Azure CLI-Beispiele für Linux-VMs](../../virtual-machines/linux/cli-samples.md)
  - [Azure PowerShell für Linux-VMs](../../virtual-machines/linux/powershell-samples.md)
  - [Azure CLI-Beispiele für Windows-VMs](../../virtual-machines/windows/cli-samples.md)
  - [Azure PowerShell für Windows-VMs](../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md)

### <a name="fundamentals-in-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange und erfordert Erfahrung sowohl mit der Azure-Plattform als auch mit den Technologien, mit deren Hilfe das Angebot erstellt wird.

Ihr Engineeringteam sollte ein grundlegendes Verständnis und Arbeitskenntnisse zu den folgenden Microsoft-Technologien mitbringen:

- [Azure-Dienste](https://azure.microsoft.com/services/)
- [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
- [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage#storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking#networking)

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Bereich **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie auf der Seite **Übersicht** die Option **Neues Angebot** > **Virtueller Azure-Computer** aus.

    ![Screenshot: Menüoptionen im linken Bereich und Schaltfläche „Neues Angebot“.](./media/new-offer-azure-virtual-machine.png)

> [!NOTE]
> Nachdem Ihr Angebot veröffentlicht wurde, werden Änderungen, die Sie in Partner Center vornehmen, im Azure Marketplace erst angezeigt, nachdem Sie das Angebot erneut veröffentlicht haben. Achten Sie darauf, die Veröffentlichung immer erneut durchzuführen, nachdem Sie Änderungen vorgenommen haben.

## <a name="new-offer"></a>Neues Angebot

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird Kunden unter der Webadresse für das Azure Marketplace-Angebot und ggf. in Azure PowerShell und der Azure-Befehlszeilenschnittstelle angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Die ID kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Der Angebotsalias ist der Name, der für das Angebot in Partner Center verwendet wird.

- Dieser Name wird nicht im Azure Marketplace verwendet. Er unterscheidet sich vom Angebotsnamen und anderen Werten, die Kunden präsentiert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-setup"></a>Angebotseinrichtung

### <a name="test-drive"></a>Testversion

Eine *Testversion* ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden vorzustellen. Sie bietet ihnen die Option, das Angebot vor dem Kauf auszuprobieren. Dies kann das Interesse steigern und qualitativ hochwertigere Leads generieren. Weitere Informationen finden Sie unter [Was ist eine Testversion?](../what-is-test-drive.md).

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Weitere Ressourcen zu Testversionen:

- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Bewährte Marketingmethoden](../what-is-test-drive.md)
- [Laden Sie die Übersicht über Testversionen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) als PDF-Datei herunter. (Stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

### <a name="customer-leads"></a>Kundenleads

Wenn Sie Ihr Angebot mithilfe von Partner Center im kommerziellen Marketplace veröffentlichen, sollten Sie es mit Ihrem CRM-System (Customer Relationship Management) verbinden. Dadurch empfangen Sie Kontaktinformationen zum Kunden, sobald jemand sein Interesse an Ihrem Produkt bekundet oder es verwendet. Das Herstellen einer Verbindung mit einem CRM ist erforderlich, wenn Sie eine Testversion aktivieren möchten (siehe vorheriger Abschnitt). Andernfalls ist das Herstellen einer Verbindung mit einem CRM optional.

1. Wählen Sie ein Leadziel aus, an das wir Kundenleads senden sollen. Das Partner Center unterstützt die folgenden CRM-Systeme:
    - [Dynamics 365](commercial-marketplace-lead-management-instructions-dynamics.md) for Customer Engagement
    - [Marketo](commercial-marketplace-lead-management-instructions-marketo.md)
    - [Salesforce](commercial-marketplace-lead-management-instructions-salesforce.md)

    > [!NOTE]
    > Wenn Ihr CRM-System hier nicht aufgeführt ist, verwenden Sie [Azure-Tabellenspeicher](commercial-marketplace-lead-management-instructions-azure-table.md) oder einen [HTTPS-Endpunkt](commercial-marketplace-lead-management-instructions-https.md), um Daten zu Ihren Kundenleads zu speichern. Exportieren Sie die Daten dann in Ihr CRM-System.

1. Verbinden Sie Ihr Angebot bei der Veröffentlichung in Partner Center mit dem Leadziel.
1. Vergewissern Sie sich, dass die Verbindung mit dem Leadziel ordnungsgemäß konfiguriert ist. Nach der Veröffentlichung Ihres Angebots in Partner Center überprüft Microsoft die Verbindung und sendet Ihnen einen Testlead. Während Sie das Angebot vor der Liveschaltung als Vorschau anzeigen, können Sie Ihre Leadverbindung ebenfalls testen, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung bereitzustellen.
1. Achten Sie darauf, dass die Verbindung mit dem Leadziel immer auf dem neuesten Stand ist, damit keine Leads verloren gehen.

1. Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf der Seite **Eigenschaften** definieren Sie die Kategorien zum Gruppieren Ihres Angebots im Azure Marketplace, die Anwendungsversion und die Verträge für Ihr Angebot.

### <a name="category"></a>Category

Wählen Sie Kategorien und Unterkategorien aus, um Ihr Angebot in den geeigneten Marketplace-Suchbereichen zu platzieren. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, inwiefern Ihr Angebot diesen Kategorien entspricht. Wählen Sie Folgendes:

- Mindestens eine und bis zu zwei Kategorien, einschließlich einer primären und einer sekundären Kategorie (optional).
- Bis zu zwei Unterkategorien für jede primäre und/oder sekundäre Kategorie. Wenn keine Unterkategorie auf Ihr Angebot anwendbar ist, wählen Sie **Nicht zutreffend** aus.

Eine vollständige Liste der Kategorien und Unterkategorien finden Sie unter [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md). Angebote für virtuelle Computer werden im Azure Marketplace immer unter der Kategorie **Compute** angezeigt.

### <a name="legal"></a>Rechtliche Hinweise

Sie müssen Ihren Kunden Angebotsbedingungen angeben. Sie haben zwei Möglichkeiten:

- **Verwenden Ihrer eigenen Geschäftsbedingungen**

   Um eigene Geschäftsbedingungen anzugeben, geben Sie bis zu 10.000 Zeichen Text im Feld **Geschäftsbedingungen** ein. Wenn Sie eine längere Beschreibung benötigen, geben Sie eine einzelne Webadresse für Ihre Geschäftsbedingungen ein. Sie wird Kunden als aktiver Link angezeigt.

   Kunden müssen diesen Bestimmungen zustimmen, bevor sie Ihr Angebot ausprobieren können.

- **Verwenden des Standardvertrags für den kommerziellen Microsoft-Marketplace**

   Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Reduzierung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft eine Vorlage für Standardverträge für den kommerziellen Marketplace bereit. Wenn Sie Ihre Software unter dem Standardvertrag anbieten, müssen Kunden ihn nur einmal lesen und akzeptieren, und Sie müssen keine benutzerdefinierten Geschäftsbedingungen erstellen.

   Um den Standardvertrag zu verwenden, aktivieren Sie das Kontrollkästchen **Use the Standard Contract for Microsoft's commercial marketplace** (Standardvertrag für den kommerziellen Marketplace von Microsoft verwenden), und wählen Sie dann im Popupfenster **Akzeptieren** aus. (Möglicherweise müssen Sie nach oben scrollen, um diese Option anzuzeigen.)

   ![Screenshot: Bereich „Rechtliche Hinweise“ in Partner Center mit aktiviertem Kontrollkästchen „Use the Standard Contract for Microsoft's commercial marketplace“ (Standardvertrag für den kommerziellen Marketplace von Microsoft verwenden)](media/use-standard-contract.png)

  > [!NOTE]
  > Nachdem Sie ein Angebot mit dem Standardvertrag für den kommerziellen Marketplace veröffentlicht haben, können Sie keine eigenen benutzerdefinierten Geschäftsbedingungen mehr verwenden. Sie können eine Lösung entweder im Rahmen des Standardvertrags oder nach Ihren eigenen Geschäftsbedingungen anbieten.

  Weitere Informationen finden Sie unter [Standardvertrag für den kommerziellen Microsoft-Marketplace](../standard-contract.md). Laden Sie den [Standardvertrag](https://go.microsoft.com/fwlink/?linkid=2041178) als PDF-Datei herunter. (Stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

  **Zusatzvereinbarungen für den Standardvertrag**

  Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Sie der Einfachheit halber den Standardvertrag auswählen und die Bedingungen für Ihr Produkt oder Unternehmen erstellen. Kunden, die bereits den Microsoft-Standardvertrag überprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag lesen. Es gibt zwei Arten von Zusatzvereinbarungen:

  * **Universelle Zusatzvereinbarungen:** Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Sie werden jedem Kunden des Angebots im Rahmen der Kaufabwicklung angezeigt. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarungen akzeptieren, bevor sie Ihr Angebot nutzen können. Sie können eine universelle Zusatzvereinbarung pro Angebot angeben. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Die Bestimmungen werden Kunden während der Suche und in der Kaufabwicklung in AppSource, dem Azure Marketplace und dem Azure-Portal angezeigt.

  * **Benutzerdefinierte Zusatzvereinbarungen:** Diese speziellen Zusatzvereinbarungen für den Standardvertrag sind nur an bestimmte Kunden über Azure-Mandanten-IDs gerichtet. Sie können den gewünschten Mandanten auswählen. Nur Kunden dieses Mandanten werden die Bedingungen der Zusatzvereinbarungen im Rahmen der Kaufabwicklung für das Angebot präsentiert. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarungen akzeptieren, bevor sie Ihr Angebot nutzen können.

    1. Wählen Sie zunächst **Benutzerdefinierte Zusatzvereinbarungen hinzufügen (Max. 10)** aus. Sie können bis zu zehn benutzerdefinierte Zusatzvereinbarungen pro Angebot angeben. Gehen Sie folgendermaßen vor:

       a. Geben Sie Ihre Zusatzbestimmungen im Feld **Benutzerdefinierte Zusatzbedingungen** ein. Sie können eine unbegrenzte Anzahl von Zeichen eingeben. Diese benutzerdefinierten Vereinbarungen werden im Azure-Portal im Rahmen der Kaufabwicklung für das Angebot nur Kunden mit den Mandanten-IDs angezeigt, die Sie für sie angeben.

       b. (Erforderlich:) Geben Sie **Mandanten-IDs** an. Jede benutzerdefinierte Zusatzvereinbarung kann für bis zu 20 Mandanten-IDs gelten. Wenn Sie eine benutzerdefinierte Zusatzvereinbarung hinzufügen, müssen Sie mindestens eine Mandanten-ID angeben, die Ihren Kunden in Azure identifiziert. Ihr Kunde kann diese für Sie in Azure finden, indem er **Azure Active Directory** > **Eigenschaften** auswählt. Der Wert „Verzeichnis-ID“ ist die Mandanten-ID (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können die von Ihrem Unternehmen zugeteilte Mandanten-ID Ihres Kunden auch suchen, indem Sie unter [Wie lautet meine Mandanten-ID für Microsoft Azure und Microsoft 365?](https://www.whatismytenantid.com/) die Domänennamen-Webadresse des Kunden verwenden.

       c. (Optional:) Geben Sie eine aussagekräftige **Beschreibung** für die Mandanten-ID an, mit der Sie den Kunden, für den die Zusatzvereinbarung gelten soll, einfacher identifizieren können.

        > [!NOTE]
        > Diese beiden Arten von Zusatzvereinbarungen sind miteinander gekoppelt. Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarungen für den Standardvertrag.

    1. Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="offer-listing"></a>Angebotsliste

Auf der Seite **Angebotsliste** können Sie Angebotsdetails wie Name, Beschreibung, Links und Kontakte definieren.

> [!NOTE]
> Die Inhalte der Angebotsliste wie Beschreibung, Dokumente, Screenshots und Nutzungsbedingungen müssen nicht in englischer Sprache vorliegen, sofern die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf \<non-English language> verfügbar“. Sie können auch eine URL als Link zu einer Website bereitstellen, die Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="marketplace-details"></a>Marketplace-Details

#### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihres Angebots angezeigt. In dieses Feld wird automatisch der Name eingetragen, den Sie beim Erstellen des Angebots im Feld **Angebotsalias** eingegeben haben. Sie können diesen Namen später ändern. Der Name:

- Kann markenrechtlich geschützt sein. Sie können Marken- und Copyrightsymbole einschließen.
- Darf nicht mehr als 50 Zeichen umfassen.
- Darf keine Emojis enthalten.

#### <a name="search-results-summary"></a>Zusammenfassung der Suchergebnisse

Geben Sie eine kurze Beschreibung Ihres Angebots ein, die in den Azure Marketplace-Suchergebnissen angezeigt werden soll. Sie darf bis zu 100 Zeichen enthalten.

#### <a name="long-summary"></a>Lange Zusammenfassung

Geben Sie eine längere Beschreibung Ihres Angebots ein, die in den Azure Marketplace-Suchergebnissen angezeigt werden soll. Sie darf bis zu 256 Zeichen enthalten.

#### <a name="description"></a>BESCHREIBUNG

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

#### <a name="privacy-policy-link"></a>Link zur Datenschutzrichtlinie

Geben Sie die Webadresse (URL) der Datenschutzrichtlinie Ihrer Organisation ein. Stellen Sie sicher, dass Ihr Angebot den Datenschutzbestimmungen und -gesetzen entspricht. Sie müssen auch eine gültige Datenschutzrichtlinie auf Ihrer Website veröffentlichen.

### <a name="useful-links"></a>Nützliche Links

Stellen Sie ergänzende Onlinedokumente zu Ihrem Angebot bereit. Um einen Link hinzuzufügen, wählen Sie **Link hinzufügen** aus, und füllen Sie dann die folgenden Felder aus:

- **Name**: Kunden wird der Name auf der Seite mit den Details angezeigt.
- **Link (URL):** Geben Sie einen Link ein, über den Kunden Ihr Onlinedokument anzeigen können.

### <a name="customer-support-links"></a>Kundensupportlinks

Geben Sie die Supportwebsite an, auf der Kunden Ihr Supportteam erreichen können.

- Azure Global-Supportwebsite
- Azure Government-Supportwebsite

### <a name="partner-support-contact"></a>Partnersupportkontakt

Geben Sie Kontaktinformationen für Microsoft-Partner an, die sie verwenden können, wenn Ihre Kunden ein Supportticket öffnen. Diese Informationen werden nicht im Azure Marketplace aufgeführt.

- Name
- Email
- Phone

### <a name="engineering-contact"></a>Engineering-Kontakt

Geben Sie Kontaktinformationen an, die Microsoft verwenden soll, wenn Probleme (einschließlich Zertifizierungsprobleme) bei Ihrem Angebot auftreten. Diese Informationen werden nicht im Azure Marketplace aufgeführt.

- Name
- Email
- Phone

### <a name="azure-marketplace-media"></a>Azure Marketplace-Medien

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Bei unscharfen Bildern wird Ihre Übermittlung abgelehnt.

[!INCLUDE [logo tips](../includes/graphics-suggestions.md)]

>[!NOTE]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="azure-marketplace-logos"></a>Azure Marketplace-Logos

Geben Sie eine PNG-Datei für das **große** Logo an. Dies wird in Partner Center verwendet, um ein **kleines** und ein **mittleres** Logo zu erstellen. Sie können die Logos später durch andere Bilder ersetzen.

- **Groß** (von 216 x 216 bis 350 x 350 px, erforderlich)
- **Mittel** (90 x 90 px, optional)
- **Klein** (48 x 48 px, optional)

Diese Logos werden an unterschiedlichen Stellen in der Auflistung verwendet:

[!INCLUDE [logos-azure-marketplace-only](../includes/logos-azure-marketplace-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots"></a>Screenshots

Fügen Sie bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Jeder Screenshot muss 1.280 &times; 720 Pixel groß sein und im PNG-Format vorliegen. Jeder Screenshot muss einen Titel enthalten.

#### <a name="videos"></a>Videos

Fügen Sie optional bis zu fünf Videos hinzu, die Ihr Angebot vorstellen. Die Videos sollten bei einem externen Videodienst gehostet werden. Geben Sie den Namen jedes Videos, die Webadresse und ein Miniaturbild des Videos im PNG-Format mit einer Größe von 1.280 &times; 720 Pixel ein.

Weitere Ressourcen für Marketplace-Listen finden Sie unter [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="preview"></a>Vorschau

Wählen Sie die Registerkarte **Vorschau** und dann eine eingeschränkte **Vorschauzielgruppe** zur Überprüfung Ihres Angebots aus, bevor Sie es live für die breitere Zielgruppe des kommerziellen Marketplace veröffentlichen.

> [!IMPORTANT]
> Nachdem Sie Ihr Angebot im Bereich **Vorschau** überprüft haben, wählen Sie **Live schalten** aus, um es für die öffentliche Zielgruppe des kommerziellen Marketplace zu veröffentlichen.

Ihre Vorschauzielgruppe wird durch Azure-Abonnement-ID-GUIDs zusammen mit einer optionalen Beschreibung für jede Gruppe identifiziert. Keines dieser Felder ist für Kunden sichtbar. Sie finden Ihre Azure-Abonnement-ID auf der Seite **Abonnement** im Azure-Portal.

Fügen Sie mindestens eine Azure-Abonnement-ID hinzu, entweder einzeln (bis zu 10) oder durch Hochladen einer CSV-Datei (bis zu 100). Durch Hinzufügen dieser Abonnement-IDs definieren Sie, wer Ihr Angebot als Vorschau anzeigen kann, bevor es live veröffentlicht wird. Wenn Ihr Angebot bereits live geschaltet ist, können Sie trotzdem eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

> [!NOTE]
> Eine Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, _bevor_ es im Azure Marketplace live veröffentlicht wird. Die Vorschauzielgruppe kann alle Pläne sehen und überprüfen, einschließlich der Pläne, die nach der vollständigen Veröffentlichung Ihres Angebots im Azure Marketplace nur für eine private Zielgruppe verfügbar sind. Eine private Zielgruppe (definiert jeweils im Bereich **Preise und Verfügbarkeit**) hat exklusiven Zugriff auf einen bestimmten Plan.

Wählen Sie **Entwurf speichern** aus, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="plan-overview"></a>Planübersicht

Sie können verschiedene Planoptionen innerhalb desselben Angebots in Partner Center angeben. Ein Angebot erfordert mindestens einen Plan (zuvor als SKU bezeichnet), der sich hinsichtlich der Monetarisierungszielgruppe, Azure-Region, Features oder VM-Images unterscheiden kann.

Sie können bis zu 100 Pläne für jedes Angebot erstellen. Bis zu 45 davon können privat sein. Erfahren Sie unter [Private Angebote im kommerziellen Microsoft-Marketplace](../private-offers.md) mehr über private Pläne.

Wählen Sie nach dem Erstellen Ihrer Pläne die Registerkarte **Planübersicht** aus, um folgende Informationen anzuzeigen:

- Plannamen
- Lizenzmodelle
- Zielgruppe (öffentlich oder privat)
- Aktueller Veröffentlichungsstatus
- Verfügbare Aktionen

Die im Bereich **Planübersicht** verfügbaren Aktionen unterscheiden sich je nach dem aktuellen Status Ihres Plans.

- Wenn es sich bei dem Plan um einen Entwurf handelt, wählen Sie **Entwurf löschen** aus.
- Wenn der Planstatus live veröffentlicht wurde, wählen Sie **Stop sell plan** (Verkauf des Plans einstellen) oder **Sync private audience** (Private Zielgruppe synchronisieren) aus.

### <a name="create-a-new-plan"></a>Erstellen eines neuen Plans

Wählen Sie oben **Neuen Plan erstellen** aus. Das Dialogfeld **Neuer Plan** wird angezeigt.

Erstellen Sie im Feld **Plan-ID** für jeden Plan im Angebot eine eindeutige Plan-ID. Diese ID ist für Kunden in der Webadresse des Produkts sichtbar. Verwenden Sie nur Kleinbuchstaben und Zahlen, Bindestriche oder Unterstriche bei maximal 50 Zeichen.

> [!NOTE]
> Die Plan-ID kann nicht mehr geändert werden, nachdem Sie **Erstellen** ausgewählt haben.

Geben Sie im Feld **Planname** einen Namen für diesen Plan ein. Kunden sehen diesen Namen, wenn sie sich für einen der Pläne in Ihrem Angebot interessieren. Erstellen Sie einen eindeutigen Namen, der die Unterschiede der einzelnen Pläne deutlich macht. Beispielsweise könnten Sie **Windows Server** mit den Plänen *Nutzungsbasierte Bezahlung*, *BYOL*, *Erweitert* und *Enterprise* verwenden.

Klicken Sie auf **Erstellen**.

### <a name="plan-setup"></a>Plansetup

Legen Sie die allgemeine Konfiguration für den Plantyp fest, geben Sie an, ob die technische Konfiguration von einem anderen Plan wiederverwendet wird, und nennen Sie die Azure-Regionen, in denen der Plan verfügbar sein soll. Ihre hier getroffene Auswahl legt fest, welche Felder in anderen Bereichen desselben Plans angezeigt werden.

#### <a name="reuse-a-technical-configuration"></a>Wiederverwenden einer technischen Konfiguration

Wenn Sie über mehrere Pläne desselben Typs verfügen und deren Pakete identisch sind, können Sie **This plan reuses technical configuration from another plan** (Dieser Plan verwendet eine technische Konfiguration aus einem anderen Plan wieder) auswählen. Mit dieser Option können Sie einen der anderen Pläne desselben Typs für dieses Angebot auswählen und seine technische Konfiguration wiederverwenden.

> [!NOTE]
> Wenn Sie die technische Konfiguration aus einem anderen Plan wiederverwenden, wird die gesamte Registerkarte **Technische Konfiguration** nicht mehr in diesem Plan angezeigt. Die Details der technischen Konfiguration aus dem anderen Plan, einschließlich aller Updates, die Sie in Zukunft vornehmen, werden für diesen Plan ebenfalls verwendet. Diese Einstellung kann nicht mehr geändert werden, nachdem der Plan veröffentlicht wurde.

#### <a name="azure-regions"></a>Azure-Regionen

Ihr Plan muss in mindestens einer Azure-Region verfügbar gemacht werden.

Wählen Sie die Option **Azure Global** aus, um Ihren Plan für Kunden in allen globalen Azure-Regionen mit Integration des kommerziellen Marketplace verfügbar zu machen. Weitere Informationen finden Sie unter [Geografische Verfügbarkeit und Unterstützung von Währungen](../marketplace-geo-availability-currencies.md).

Wählen Sie die Option **Azure Government** aus, um Ihren Plan in der [Azure Government](../../azure-government/documentation-government-welcome.md)-Region verfügbar zu machen. Diese Region bietet kontrolliertem Zugriff für Kunden von US-Behörden auf Bundes-, Bundesstaats-, Gemeinde- oder Stammesgebietsebene sowie für Partner, die diese beliefern dürfen. Als Herausgeber sind Sie für alle Kompatibilitätskontrollen, Sicherheitsmaßnahmen und bewährten Methoden verantwortlich. Azure Government verwendet physisch isolierte Rechenzentren und Netzwerke (die sich ausschließlich in den USA befinden).

Testen und validieren Sie Ihren Plan vor dem Veröffentlichen für [Azure Government](../../azure-government/documentation-government-manage-marketplace-partners.md) in der Umgebung, da bestimmte Endpunkte möglicherweise abweichen. Fordern Sie für das Einrichten und Testen Ihres Plans auf der Seite [Microsoft Azure Government-Testversion](https://azure.microsoft.com/global-infrastructure/government/request/) ein Testkonto an.

> [!NOTE]
> Nachdem Ihr Plan veröffentlicht und in einer bestimmten Azure-Region verfügbar gemacht wurde, können Sie diese Region nicht mehr entfernen.

#### <a name="azure-government-certifications"></a>Azure Government-Zertifizierungen

Diese Option ist nur sichtbar, wenn Sie im vorherigen Abschnitt **Azure Government** als Azure-Region ausgewählt haben.

Azure Government-Dienste verarbeiten Daten, die bestimmten behördlichen Vorschriften und Anforderungen unterliegen. Beispielsweise FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 und CJIS. Um bei Ihren Zertifizierungen ein Bewusstsein für diese Programme zu schaffen, können Sie bis zu 100 Links bereitstellen, in denen sie beschrieben werden. Diese können entweder direkt mit ihrem Listing im Programm verknüpft sein oder zu Beschreibungen der Konformität mit diesen Programmen auf Ihren eigenen Websites weiterleiten. Diese Links sind nur für Azure Government-Kunden sichtbar.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="plan-listing"></a>Planlisting

In diesem Abschnitt konfigurieren Sie die Auflistungsdetails des Plans. In diesem Bereich werden bestimmte Informationen angezeigt, die sich von anderen Plänen im selben Angebot unterscheiden können.

#### <a name="plan-name"></a>Planname

In dieses Feld wird automatisch der Name eingetragen, den Sie Ihrem Plan beim Erstellen gegeben haben. Dieser Name wird im Azure Marketplace als Titel dieses Plans angezeigt. Er ist auf 100 Zeichen beschränkt.

#### <a name="plan-summary"></a>Planzusammenfassung

Geben Sie eine kurze Zusammenfassung Ihres Plans (nicht des Angebots) an. Diese Zusammenfassung darf maximal 100 Zeichen enthalten.

#### <a name="plan-description"></a>Planbeschreibung

Beschreiben Sie, was Ihren Softwareplan von anderen unterscheidet und worin die Unterschiede zwischen den Plänen in Ihrem Angebot bestehen. Beschreiben Sie nur den Plan, nicht das Angebot. Die Planbeschreibung kann bis zu 2.000 Zeichen enthalten.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

In diesem Bereich konfigurieren Sie Folgendes:

- Märkte, in denen dieser Plan verfügbar ist Jeder Plan muss in mindestens einem [Markt](../marketplace-geo-availability-currencies.md) verfügbar sein.
- Preis pro Stunde
- Ob der Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) angezeigt werden soll

#### <a name="markets"></a>Märkte

Jeder Plan muss in mindestens einem Markt verfügbar sein. Aktivieren Sie das Kontrollkästchen für jeden Marktstandort, an dem dieser Plan zum Kauf verfügbar sein soll. (Benutzer in diesen Märkten können das Angebot weiterhin in allen Azure-Regionen bereitstellen, die im Abschnitt [Plansetup](#plan-setup) ausgewählt wurden.) Über die Schaltfläche **Bezahlte Steuern** können Sie Länder/Regionen anzeigen, in denen Microsoft für Sie Verkaufserlöse und Steuern überweist. Die Veröffentlichung in China ist auf *kostenlose* oder *BYOL*-Pläne (Bring Your Own License) beschränkt.

Wenn Sie bereits Preise für Ihren Plan in der Währung US-Dollar (USD) festgelegt haben und einen anderen Marktstandort hinzufügen, wird der Preis für den neuen Markt entsprechend dem aktuellen Wechselkurs berechnet. Überprüfen Sie vor dem Veröffentlichen immer den Preis für jeden Markt. Überprüfen Sie Ihre Preise, indem Sie **Export prices (xlsx)** (Preise exportieren (XLSX)) auswählen, nachdem Sie die Änderungen gespeichert haben.

Wenn Sie einen Markt entfernen, können Kunden in diesem Markt, die aktive Bereitstellungen nutzen, keine neuen Bereitstellungen mehr erstellen und ihre vorhandenen Bereitstellungen nicht mehr hochskalieren. Vorhandene Bereitstellungen sind nicht betroffen.

#### <a name="pricing"></a>Preise

Wählen Sie für **License model** (Lizenzmodell) die Option **Nutzungsbasierter monatlicher Fakturierungsplan** aus, um die Preise für diesen Plan zu konfigurieren, oder **BYOL (Bring Your Own License)** , damit Kunden diesen Plan mit Ihrer vorhandenen Lizenz verwenden können.

Verwenden Sie für einen nutzungsbasierten monatlichen Fakturierungsplan eine der drei folgenden Preisoptionen:

- **Pro Kern:** Geben Sie die Preise pro Kern in USD an. Microsoft berechnet die Preise pro Kerngröße und rechnet sie anhand des aktuellen Wechselkurses in lokale Währungen um.
- **Pro Kerngröße:** Geben Sie die Preise pro Kerngröße in USD an. Microsoft berechnet die Preise und rechnet sie anhand des aktuellen Wechselkurses in lokale Währungen um.
- **Pro Markt und Kerngröße:** Geben Sie die Preise für die einzelnen Kerngrößen für alle Märkte an. Sie können die Preise aus einem Arbeitsblatt importieren.

> [!NOTE]
> Speichern Sie Preisänderungen, damit die Preisdaten exportiert werden können. Nachdem ein Preis für einen Markt in Ihrem Plan veröffentlicht wurde, kann er nicht mehr geändert werden. Um vor der Veröffentlichung dieser Preise sicherzustellen, dass sie richtig sind, exportieren Sie das Preisarbeitsblatt, und überprüfen Sie die Preise für jeden Markt.

#### <a name="free-trial"></a>Kostenlose Testversion

Sie können Ihren Kunden eine *kostenlose Testversion* für einen Monat, drei Monate oder sechs Monate anbieten.

#### <a name="visibility"></a>Sichtbarkeit

Sie können jeden Plan so entwerfen, dass er für alle Benutzer oder nur für eine vorab ausgewählte Zielgruppe sichtbar ist. Weisen Sie die Mitgliedschaften in dieser eingeschränkten Zielgruppe mithilfe von Azure-Abonnement-IDs zu.

**Öffentlich**: Ihr Plan kann von allen Benutzern eingesehen werden.

**Private Zielgruppe:** Machen Sie Ihren Plan nur für eine vorab ausgewählte Zielgruppe sichtbar. Nachdem er als privater Plan veröffentlicht wurde, können Sie die Zielgruppe aktualisieren oder ihn in „öffentlich“ ändern. Nachdem Sie einen Plan öffentlich gemacht haben, muss er auch öffentlich bleiben. Er kann nicht wieder in einen privaten Plan geändert werden.

> [!NOTE]
> Eine private oder eingeschränkte Zielgruppe unterscheidet sich von der im Bereich **Vorschau** definierten Vorschauzielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, _bevor_ es im Azure Marketplace live veröffentlicht wird. Auch wenn die Auswahl der privaten Zielgruppe nur für einen bestimmten Plan gilt, kann die private Zielgruppe alle Pläne (private und nicht private Pläne) zu Validierungszwecken anzeigen.

**Eingeschränkte Zielgruppe (Azure-Abonnement-IDs):** Weisen Sie anhand von Azure-Abonnement-IDs die Zielgruppe zu, die auf diesen privaten Plan zugreifen darf. Optional können Sie eine Beschreibung der einzelnen zugewiesenen Azure-Abonnement-IDs einschließen. Fügen Sie bis zu 10 Abonnement-IDs manuell oder bis zu 20.000 durch Importieren eines CSV-Arbeitsblatts hinzu. Azure-Abonnement-IDs werden als GUIDs dargestellt, und alle Buchstaben müssen in Kleinschreibung angegeben werden.

>[!Note]
>Private Angebote werden bei Azure-Abonnements, die über einen Handelspartner des Cloud Solution Provider-Programms (CSP) eingerichtet wurden, nicht unterstützt.

#### <a name="hide-a-plan"></a>Ausblenden eines Plans

Wenn Ihre VM bei Verweisen über eine andere Lösungsvorlage oder verwaltete Anwendung nur indirekt bereitgestellt werden soll, aktivieren Sie dieses Kontrollkästchen, um Ihre VM zu veröffentlichen, aber für Kunden bei einer direkten Suche auszublenden.

> [!NOTE]
> Ausgeblendete Pläne unterstützen keine Vorschaulinks.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="technical-configuration"></a>Technische Konfiguration

Geben Sie die Images und andere technische Eigenschaften an, die diesem Plan zugeordnet sind. Weitere Informationen finden Sie unter [Erstellen einer technischen Azure-VM-Ressource](create-azure-container-technical-assets.md).

> [!NOTE]
> Die Registerkarte **Technische Konfiguration** wird nicht angezeigt, wenn Sie den Plan auf der Registerkarte **Plansetup** zur Wiederverwendung von Paketen aus einem anderen Plan konfiguriert haben.

#### <a name="operating-system"></a>Betriebssystem

Gehen Sie im Bereich **Betriebssystem** folgendermaßen vor:

- Wählen Sie unter **Betriebssystemfamilie** eines der Betriebssysteme **Windows** oder **Linux** aus.
- Wählen Sie unter **Release** oder **Anbieter** das Windows-Release bzw. den Linux-Anbieter aus.
- Wählen Sie unter **Betriebssystem-Anzeigename** einen benutzerfreundlichen Betriebssystemnamen aus. Dieser Name ist für Kunden sichtbar.

#### <a name="recommended-vm-sizes"></a>Empfohlene VM-Größen

Wählen Sie bis zu sechs empfohlene VM-Größen für die Anzeige im Azure Marketplace aus.

#### <a name="open-ports"></a>Öffnen von Ports

Öffnen Sie öffentliche oder private Ports auf einem bereitgestellten virtuellen Computer.

#### <a name="storage-option-for-deployment"></a>Speicheroption für die Bereitstellung

Wählen Sie unter **Disk deployment option** (Datenträgerbereitstellungs-Option) die Art der Datenträgerbereitstellung aus, die Ihre Kunden für den virtuellen Computer verwenden können. Microsoft empfiehlt, die Bereitstellung auf **verwaltete Datenträger** einzuschränken.

#### <a name="properties"></a>Eigenschaften

Wählen Sie unter **Support Accelerated Networking** (Beschleunigten Netzwerkbetrieb unterstützen) aus, ob Ihre VM den [beschleunigten Netzwerkbetrieb](https://go.microsoft.com/fwlink/?linkid=2124513) unterstützt.

#### <a name="vm-images"></a>VM-Images

Geben Sie eine Datenträgerversion und den SAS-URI (Shared Access Signature) für die VM-Images an. Fügen Sie bis zu 16 Datenträger für jedes VM-Image hinzu. Geben Sie nur eine neue Imageversion pro Plan in einer bestimmten Übermittlung an. Nachdem ein Image veröffentlicht wurde, können Sie es nicht mehr bearbeiten, Sie können es jedoch löschen. Das Löschen einer Version verhindert, dass neue und vorhandene Benutzer eine neue Instanz der gelöschten Version bereitstellen.

- **Datenträgerversion:** Die Version des Images, das Sie bereitstellen.
- **SAS-URI:** Der Speicherort in Ihrem Azure Storage-Konto, an dem Sie die Betriebssystem-VHD gespeichert haben. Informationen zum Abrufen eines SAS-URI finden Sie unter [Abrufen des Shared Access Signature-URI für Ihr VM-Image](get-sas-uri.md).
- Datenträgerimages sind ebenfalls VHD-SAS-URIs, die in den zugehörigen Azure Storage-Konten gespeichert sind.
- Fügen Sie einem Plan pro Übermittlung nur ein Image hinzu.

Fügen Sie unabhängig vom verwendeten Betriebssystem immer nur die für die Lösung erforderliche Mindestanzahl von Datenträgern für Daten hinzu. Bei der Bereitstellung können Kunden Datenträger, die Teil eines Images sind, nicht entfernen. Sie haben aber immer die Möglichkeit, während oder nach der Bereitstellung Datenträger hinzuzufügen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren und zur **Planübersicht** zurückkehren.

## <a name="resell-through-csps"></a>Verkaufen über CSPs

Erweitern Sie die Reichweite Ihres Angebots, indem Sie es für Partner im [Cloud Solution Provider](https://azure.microsoft.com/offers/ms-azr-0145p/)-Programm (CSP) verfügbar machen. Alle BYOL-Pläne (Bring-Your-Own-License) werden im Programm automatisch aktiviert. Sie können auch Ihre Nicht-BYOL-Pläne aktivieren.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="test-drive"></a>Testversion

Richten Sie eine Demonstration (oder *Testversion*) ein, damit Kunden Ihr Angebot für einen festen Zeitraum testen können, bevor sie es erwerben. Informationen zum Erstellen einer Demonstrationsumgebung für Ihre Kunden finden Sie unter [Informationen zu einer Testversion Ihres Angebots](test-drive.md).

Aktivieren Sie auf der Registerkarte **Angebotseinrichtung** das Kontrollkästchen **Testversion aktivieren**, um eine Testversion zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

Weitere Ressourcen zu Testversionen:

- [Bewährte Marketingmethoden](../what-is-test-drive.md)
- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Übersicht über Testversionen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) als PDF-Datei (stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="review-and-publish"></a>Überprüfen und veröffentlichen

Nachdem Sie alle erforderlichen Abschnitte des Angebots abgeschlossen haben, können Sie es zum Überprüfen und Veröffentlichen übermitteln.

Wählen Sie in der rechten oberen Ecke **Überprüfen und veröffentlichen** aus.

In diesem Bereich können Sie folgende Aktionen ausführen:

- Anzeigen des Abschlussstatus der einzelnen Abschnitte des Angebots:

  - **Nicht gestartet**: Der Abschnitt wurde noch nicht begonnen und muss abgeschlossen werden.
  - **Unvollständige Informationen**: Der Abschnitt enthält Fehler, die behoben werden müssen, oder er erfordert eine Ergänzung von Informationen. Anleitungen zum Aktualisieren unvollständiger Informationen finden Sie in den vorherigen Abschnitten dieses Artikels.
  - **Vollständigkeit**: Der Abschnitt ist abgeschlossen, und es liegen keine Fehler vor. Sie können das Angebot erst einreichen, wenn alle Abschnitte des Angebots abgeschlossen sind.
- Geben Sie **Hinweise zur Zertifizierung** für das Zertifizierungsteam an, um sicherzustellen, dass Ihre Anwendung ordnungsgemäß getestet wird. Schließen Sie Testanweisungen und ggf. ergänzende Notizen ein, durch die das Team Ihre Anwendung besser verstehen kann.

Um das Angebot zur Veröffentlichung einzureichen, wählen Sie **Überprüfen und veröffentlichen** aus.

Microsoft sendet Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots zur Überprüfung und Genehmigung verfügbar ist. Um Ihr Angebot für die Öffentlichkeit zugänglich zu machen (oder für eine private Zielgruppe, wenn es sich um ein privates Angebot handelt), navigieren Sie zu Partner Center, rufen Sie die Seite **Übersicht** für Ihr Angebot auf, und wählen Sie **Live schalten** aus. Der Status Ihres Angebots wird im oberen Bereich der Seite angezeigt, wenn die Veröffentlichung stattfindet.

### <a name="errors-and-review-feedback"></a>Fehler und Prüfungsfeedback

Der Schritt **Manuelle Überprüfung** im Veröffentlichungsprozess stellt eine umfassende Überprüfung Ihres Angebots und der zugehörigen technischen Ressourcen dar. Wenn bei diesem Vorgang Fehler in Ihrem Angebot aufgedeckt werden, erhalten Sie einen Zertifizierungsbericht, in dem die Probleme erläutert werden. Nehmen Sie einfach die erforderlichen Korrekturen vor, und veröffentlichen Sie Ihr Angebot erneut.

## <a name="offer-overview"></a>Angebotsübersicht

Die Seite **Angebotsübersicht** weist eine visuelle Darstellung sowohl der abgeschlossenen als auch der noch laufenden erforderlichen Schritte zum Veröffentlichen dieses Angebots und der erwarteten Dauer ihrer Ausführung auf.

Diese Seite enthält außerdem Links, die Ihnen bei der Arbeit mit dem Angebot helfen, je nach Status:

- Wenn das Angebot ein Entwurf ist: [Delete draft offer](update-existing-offer.md#delete-a-draft-offer) (Angebotsentwurf löschen)
- Wenn das Angebot live geschaltet wurde: [Stop selling the offer](update-existing-offer.md#stop-selling-an-offer-or-plan) (Verkauf des Angebots beenden)
- Wenn sich das Angebot in der Vorschau befindet: [Live schalten](publishing-status.md#publisher-approval)
- Wenn keine Bestätigung durch den Herausgeber erfolgt ist: [Veröffentlichung abbrechen](update-existing-offer.md#cancel-publishing)

## <a name="marketplace-examples"></a>Marketplace-Beispiele

Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen im Azure Marketplace angezeigt werden:

:::image type="content" source="media/example-azure-marketplace-virtual-machine-offer.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Großes Logo
2. Preis
3. Kategorien
4. Geschäftsbedingungen
5. Adresse der Datenschutzrichtlinie (Link)
6. Angebotsname
7. BESCHREIBUNG
8. Nützliche Links
9. Screenshots/Videos

<br>Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen in Azure Marketplace-Suchergebnissen angezeigt werden:

:::image type="content" source="media/example-azure-marketplace-virtual-machine-search-results.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Kleines Logo
2. Angebotsname
3. Zusammenfassung der Suchergebnisse
4. Testversion

<br>Hier sehen Sie ein Beispiel für Plandetails im Azure Marketplace:

:::image type="content" source="media/example-azure-marketplace-virtual-machine-plan-details.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Planname und Zusammenfassung
2. Empfohlene VM-Größen
3. Preise für den Plan

<br>Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen im Azure-Portal angezeigt werden:

:::image type="content" source="media/example-azure-portal-virtual-machine-offer.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Name
2. BESCHREIBUNG
3. Nützliche Links
4. Screenshots/Videos

<br>Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen in den Suchergebnissen im Azure-Portal angezeigt werden:

:::image type="content" source="media/example-azure-portal-virtual-machine-search-results.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Kleines Logo
2. Angebotsname
3. Zusammenfassung der Suchergebnisse

<br>Hier sehen Sie ein Beispiel für die Plandetails im Azure-Portal:

:::image type="content" source="media/example-azure-portal-virtual-machine-plan-details.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Planname
2. Planbeschreibung

## <a name="next-step"></a>Nächster Schritt

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](update-existing-offer.md)
