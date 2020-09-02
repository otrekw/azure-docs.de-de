---
title: 'Kommerzieller Microsoft-Marketplace: Erstellen eines Azure-Anwendungsangebots'
description: Hier lernen Sie die Schritte und Aspekte des Erstellens eines neuen Azure-Anwendungsangebots im kommerziellen Marketplace-Portal in Partner Center kennen. Sie können Ihr Azure-Anwendungsangebot im Azure Marketplace oder über das CSP-Programm (Cloud Solution Provider) anbieten oder verkaufen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: AarathiN
ms.author: aarathin
ms.date: 07/14/2020
ms.openlocfilehash: 071d3fc972e99d61a226492350b7eee146584df6
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88815663"
---
# <a name="create-an-azure-application-offer"></a>Erstellen eines Azure-Anwendungsangebots

In diesem Artikel werden die Schritte und Aspekte des Erstellens eines neuen Azure-Anwendungsangebots im kommerziellen Marketplace erläutert. Sie sollten mit diesen Konzepten vertraut sein, bevor Sie ein neues Azure-Anwendungsangebot erstellen.

Bevor Sie ein neues Azure-Anwendungsangebot veröffentlichen können, [erstellen Sie ein Konto für den kommerziellen Marketplace in Partner Center](create-account.md), und stellen Sie sicher, dass Ihr Konto im kommerziellen Marketplace-Programm registriert ist.

## <a name="before-you-begin"></a>Voraussetzungen

Das Entwerfen, Erstellen und Testen dieser Azure-Anwendungsangebote erfordert technische Kenntnisse in Bezug auf die Azure-Plattform und -Technologien, um das Angebot zu erstellen. Ihr Entwicklerteam sollte sich mit den folgenden Microsoft-Technologien auskennen:

* Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/).
* [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/).
* Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage#storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking#networking).
* Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
* Fundierte Kenntnisse zu [JSON](https://www.json.org/).

### <a name="technical-documentation-and-resources"></a>Technische Dokumentation und Ressourcen

Bereiten Sie Ihr Azure-Anwendungsangebot mithilfe der folgenden Ressourcen für den kommerziellen Marketplace vor.

* [Verstehen von Azure Resource Manager-Vorlagen](../../azure-resource-manager/resource-group-authoring-templates.md)

* Schnellstartanleitungen:

    * [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/)
    * [Leitfaden zu bewährten Methoden für Azure-Vorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md)
    * [Veröffentlichen der Anwendungsdefinition](../../managed-applications/publish-service-catalog-app.md)
    * [Bereitstellen einer Dienstkatalog-App](../../managed-applications/deploy-service-catalog-quickstart.md)

* Tutorials:

    * [Erstellen von Definitionsdateien](../../managed-applications/publish-service-catalog-app.md)
    * [Veröffentlichen der Marketplace-Anwendung](../../managed-applications/publish-marketplace-app.md)

* Beispiele:

    * [Azure-Befehlszeilenschnittstelle](../../managed-applications/cli-samples.md)
    * [Azure PowerShell](../../managed-applications/powershell-samples.md)
    * [Verwaltete Anwendungslösungen](../../managed-applications/sample-projects.md)

Das Video [Erstellen von Lösungsvorlagen und verwalteten Anwendungen für den Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) enthält eine ausführliche Einführung in den Angebotstyp „Azure-Anwendung“:

* Welche Angebotstypen sind verfügbar?
* Welche technischen Ressourcen sind erforderlich?
* Erstellen einer Azure Resource Manager-Vorlage
* Entwickeln und Testen der App-Benutzeroberfläche
* Veröffentlichen des App-Angebots
* Der Anwendungsüberprüfungsprozess

### <a name="suggested-tools"></a>Vorgeschlagene Tools

Wählen Sie eine oder beide der folgenden Skriptumgebungen als Unterstützung bei der Verwaltung Ihrer Azure-Anwendung aus:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)

Es empfiehlt sich, dass Sie Ihrer Entwicklungsumgebung die folgenden Tools hinzufügen:

* [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen:
    * Erweiterung: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    * Erweiterung: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    * Erweiterung: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Die verfügbaren Tools finden Sie auf der Seite [Azure-Entwicklertools](https://azure.microsoft.com/tools/). Und wenn Sie Visual Studio verwenden, auch auf dem [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="types-of-azure-application-plans"></a>Typen von Azure-Anwendungsplänen

Es gibt zwei Arten von Azure-Anwendungsplänen: Lösungsvorlagen und verwaltete Anwendungen.

* **Lösungsvorlagen** zählen zu den wichtigsten Optionen für die Veröffentlichung von Lösungen im Marketplace. Verwenden Sie diesen Plantyp, wenn für eine Lösung neben einem einzelnen virtuellen Computer noch eine zusätzliche Bereitstellungs- und Konfigurationsautomatisierung erforderlich ist. Mit einer Lösungsvorlage können Sie die Bereitstellung mehrerer Ressourcen automatisieren, beispielsweise VMs, Netzwerke und Speicherressourcen, um komplexe IaaS-Lösungen bereitzustellen.  Weitere Informationen zum Erstellen von Lösungsvorlagen finden Sie unter [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

* **Verwaltete Anwendungen** und Lösungsvorlagen sind bis auf einen wichtigen Unterschied ähnlich. Bei einer verwalteten Anwendung werden die Ressourcen in einer Ressourcengruppe bereitgestellt, die vom Herausgeber der App verwaltet wird. Die Ressourcengruppe befindet sich zwar im Abonnement des Verbrauchers, eine Identität im Mandanten des Herausgebers hat jedoch Zugriff auf die Ressourcengruppe. Als Herausgeber geben Sie die Kosten für den kontinuierlichen Support der Lösung an. Verwenden Sie verwaltete Anwendungen, um mühelos vollständig verwaltete, schlüsselfertige Anwendungen für Ihre Kunden zu erstellen und bereitzustellen.  Weitere Informationen über die Vorteile und Typen von verwalteten Anwendungen finden Sie unter [Übersicht über verwaltete Azure-Anwendungen](../../managed-applications/overview.md).

## <a name="technical-requirements"></a>Technische Anforderungen

Alle Azure-Anwendungen enthalten mindestens zwei Dateien im Stammordner eines `.zip`-Archivs:

* Eine Resource Manager-Vorlagendatei mit dem Namen [mainTemplate.json](../../azure-resource-manager/resource-group-overview.md).  In dieser Vorlage werden die Ressourcen definiert, die im Azure-Abonnement des Kunden bereitgestellt werden sollen. Beispiele für Resource Manager-Vorlagen finden Sie im [Katalog der Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) oder im entsprechenden Repository [GitHub: Azure Resource Manager Quickstart Templates](https://github.com/azure/azure-quickstart-templates) (Azure Resource Manager-Schnellstartvorlagen).

* Eine Benutzeroberflächendefinition für die Oberfläche zum Erstellen von Azure-Anwendungen mit dem Namen [createUiDefinition.json](../../managed-applications/create-uidefinition-overview.md).  Auf der Benutzeroberfläche geben Sie Elemente an, die Kunden die Angabe von Parameterwerten ermöglichen.

Alle neuen Azure-Anwendungsangebote müssen eine [GUID zum Zuordnen der Nutzung durch Kunden von Azure-Partnern](../azure-partner-customer-usage-attribution.md) enthalten. 

Weitere Informationen über Veröffentlichungsanforderungen für Anwendungspläne finden Sie unter [Anforderungen für die Veröffentlichung von Lösungsvorlagenangeboten](../marketplace-solution-templates.md) und [Anforderungen für die Veröffentlichung von Angeboten verwalteter Anwendungen](../marketplace-managed-apps.md).

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

>[!NOTE]
>Nach dem Veröffentlichen eines Angebots werden Änderungen, die Sie in Partner Center vornehmen, erst in den Onlineshops angezeigt, nachdem Sie das Angebot erneut veröffentlicht haben. Achten Sie darauf, dass Sie die Veröffentlichung immer erneut durchführen, nachdem Sie Änderungen vorgenommen haben.

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.

1. Klicken Sie im linken Menü auf **Kommerzieller Marketplace** > **Übersicht**.

1. Klicken Sie auf der Seite „Übersicht“ auf **+ Neues Angebot** > **Azure-Anwendung**.

    ![Darstellung des Navigationsmenüs auf der linken Seite.](./media/new-offer-azure-app.png)

1. Geben Sie auf der Seite **Neues Angebot** eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

     * Diese ID wird für Kunden unter der Webadresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt.
     * Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
     * Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

1. Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird.

     * Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden.
     * Nachdem Sie **Erstellen** ausgewählt haben, kann der Angebotsalias nicht mehr geändert werden.

1. Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="offer-setup"></a>Angebotseinrichtung

Auf der Seite **Angebotseinrichtung** können Sie eine Testversion und die Leadverwaltung für Ihr Angebot konfigurieren.

### <a name="test-drive"></a>Testversion

Die Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, vor dem Kauf einen Test durchzuführen. Dies führt zu einer höheren Konversionsrate und zur Generierung von aussichtsreichen Leads. Weitere Informationen zu Testversionen finden Sie [hier](../what-is-test-drive.md).

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion für einen festen Zeitraum zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen. Konfigurieren Sie die Umgebung für die Testversion wie im Abschnitt [Technische Konfiguration der Testversion](#test-drive-technical-configuration) weiter unten in diesem Artikel beschrieben.

Weitere Informationen finden Sie unter [Ermöglichen Sie Ihren Kunden das Testen Ihres Angebots](test-drive.md). Sie können auch mehr über die [bewährten Methoden für Testversionen](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) erfahren und die [PDF-Datei mit einer Übersicht über Testversionen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) herunterladen (stellen Sie hierzu sicher, dass Ihr Popupblocker deaktiviert ist).

>[!Note]
>Da alle Azure-Anwendungen mit einer Azure Resource Manager-Vorlage implementiert werden, ist die einzige Art von Testversion, die für Azure-Anwendungen verfügbar ist, eine [Azure Resource Manager-Testversion](../azure-resource-manager-test-drive.md).

### <a name="customer-leads"></a>Kundenleads

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="properties"></a>Eigenschaften

Auf der Seite **Eigenschaften** können Sie die Kategorien, die zum Gruppieren Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren.

### <a name="category"></a>Category

Wählen Sie Kategorien und Unterkategorien aus, um Ihr Angebot in den geeigneten Marketplace-Suchbereichen zu platzieren. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, inwiefern Ihr Angebot diesen Kategorien entspricht. Wählen Sie Folgendes:

- Mindestens eine und bis zu zwei Kategorien, einschließlich einer primären und einer sekundären Kategorie (optional).
- Bis zu zwei Unterkategorien für jede primäre und/oder sekundäre Kategorie. Wenn keine Unterkategorie auf Ihr Angebot anwendbar ist, wählen Sie **Nicht zutreffend** aus.

Eine vollständige Liste der Kategorien und Unterkategorien finden Sie unter [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md).

### <a name="legal"></a>Rechtliche Hinweise

[!INCLUDE [Commercial marketplace Standard Contract](./includes/marketplace-contract-text.md)]

## <a name="offer-listing"></a>Angebotsliste

Auf dieser Seite verwalten Sie die Kopie und Bilder für Ihr Angebot im kommerziellen Marketplace.

### <a name="marketplace-details"></a>Marketplace-Details

> [!NOTE]
> Die Inhalte der Angebotsliste (z. B. Beschreibung, Dokumente, Screenshots und Nutzungsbedingungen) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf [nicht englische Sprache] verfügbar.“ Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen im Azure Marketplace angezeigt werden (die aufgelisteten Preise dienen lediglich als Beispiele und stellen keine tatsächlichen Kosten dar):

:::image type="content" source="media/example-azure-marketplace-app.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure Marketplace angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Logo
2. Kategorien
3. Supportadresse (Link)
4. Nutzungsbedingungen
5. Adresse der Datenschutzrichtlinie (Link)
6. Angebotsname
7. Zusammenfassung
8. BESCHREIBUNG
9. Screenshots/Videos

<br>Hier sehen Sie ein Beispiel dafür, wie Angebotsinformationen im Azure-Portal angezeigt werden:

:::image type="content" source="media/example-virtual-machine-container-iot-edge-saas.png" alt-text="Veranschaulicht, wie dieses Angebot im Azure-Portal angezeigt wird.":::

#### <a name="call-out-descriptions"></a>Beschreibungen zu den Nummern

1. Titel
2. BESCHREIBUNG
3. Nützliche Links
4. Screenshots

#### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text gefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

#### <a name="search-results-summary"></a>Zusammenfassung der Suchergebnisse

Geben Sie eine kurze Beschreibung Ihres Angebots an (bis zu 100 Zeichen). Diese Beschreibung kann in Suchergebnissen verwendet werden.

#### <a name="long-summary"></a>Lange Zusammenfassung

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 256 Zeichen). Diese Beschreibung kann in Suchergebnissen verwendet werden.

#### <a name="description"></a>BESCHREIBUNG

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

#### <a name="search-keywords"></a>Suchbegriffe

Sie können optional bis zu drei Suchbegriffe eingeben, die Kunden helfen, Ihr Angebot im Marketplace zu finden. Um optimale Ergebnisse zu erzielen, sollten Sie diese Schlüsselwörter auch in ihrer Beschreibung verwenden.

#### <a name="privacy-policy-link"></a>Link zur Datenschutzrichtlinie

Geben Sie die URL zur Datenschutzrichtlinie Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="useful-links"></a>Nützliche Links

Fügen Sie Links zu zusätzlichen optionalen Onlinedokumentationen zu Ihrer Lösung bereit, indem Sie auf **+ Link hinzufügen** klicken.

### <a name="contact-information"></a>Kontaktinformationen

Geben Sie den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Supportkontakt**, **Engineering-Kontakt** und einen **CSP-Programm-Kontakt** an. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und werden möglicherweise an CSP-Partner weitergegeben. Für einige Kontakte können weitere Informationen erforderlich sein.

### <a name="marketplace-media"></a>Marketplace-Medien

Stellen Sie Logos und Bilder zur Verwendung mit Ihrem Angebot bereit. Alle Bilder müssen das PNG-Format aufweisen. Bei unscharfen Bildern wird Ihre Übermittlung abgelehnt.

[!INCLUDE [logotips](../includes/graphics-suggestions.md)]

>[!Note]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

#### <a name="store-logos"></a>Store-Logos

Geben Sie eine PNG-Datei für das **große** Logo an. Dies wird in Partner Center verwendet, um ein **kleines** und ein **mittleres** Logo zu erstellen. Sie können diese Logos später optional durch andere Bilder ersetzen.

- **Groß** (von 216 x 216 bis 350 x 350 Pixel, erforderlich)
- **Mittel** (90 x 90 Pixel, optional)
- **Klein** (48 x 48 Pixel, optional)

Diese Logos werden an unterschiedlichen Stellen in der Auflistung verwendet:

[!INCLUDE [logos-azure-marketplace-only](../includes/logos-azure-marketplace-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots"></a>Screenshots

Fügen Sie bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Jeder Screenshot muss 1.280 x 720 Pixel groß sein und im PNG-Format vorliegen. Jeder Screenshot muss einen Titel enthalten.

#### <a name="videos"></a>Videos

Fügen Sie optional bis zu fünf Videos hinzu, die Ihr Angebot vorstellen. Diese sollten von einem externen Videodienst gehostet werden. Geben Sie für alle Videos den Namen, die Webadresse und ein Miniaturbild im PNG-Format mit einer Auflösung von 1280 × 720 Pixel an.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](../gtm-offer-listing-best-practices.md)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="preview-audience"></a>Vorschauzielgruppe

Auf der Registerkarte „Vorschau“ können Sie eine eingeschränkte **Vorschauzielgruppe** zur Überprüfung Ihres Angebots auswählen, bevor Sie es live für die breitere Zielgruppe der Marketplacebenutzer veröffentlichen.

> [!IMPORTANT]
> Nachdem Sie Ihr Angebot in der Vorschau überprüft haben, wählen Sie **Live schalten** aus, um es für die öffentliche Zielgruppe des kommerziellen Marketplace zu veröffentlichen.

Ihre Vorschauzielgruppe wird durch Azure-Abonnement-ID-GUIDs zusammen mit einer optionalen Beschreibung für jede Gruppe identifiziert. Keines dieser Felder ist für Kunden sichtbar. Sie finden Ihre Azure-Abonnement-ID auf der Seite **Abonnements** im Azure-Portal.

Fügen Sie mindestens eine Azure-Abonnement-ID hinzu, entweder einzeln (bis zu 10) oder durch Hochladen einer CSV-Datei (bis zu 100). Durch Hinzufügen dieser Abonnement-IDs definieren Sie, wer Ihr Angebot als Vorschau anzeigen kann, bevor es live veröffentlicht wird. Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

> [!NOTE]
> Eine Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, *bevor* es in den Marketplaces live veröffentlicht wird. Sie kann alle Pläne sehen und überprüfen, einschließlich der Pläne, die nach der vollständigen Veröffentlichung Ihres Angebots im Marketplace nur für eine private Zielgruppe verfügbar sind. Eine private Zielgruppe (definiert in der Registerkarte **Preise und Verfügbarkeit**) hat exklusiven Zugriff auf einen bestimmten Plan.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="technical-configuration"></a>Technische Konfiguration

Vervollständigen Sie diesen Abschnitt nur, wenn Ihr Angebot eine verwaltete Anwendung umfasst, die Messereignisse mithilfe der Marketplace-Messungsdienst-API ausgibt. Geben Sie die **Azure Active Directory-Mandanten-ID** und die **Azure Active Directory-Anwendungs-ID** ein, die Ihr Dienst beim Ausgeben von Messereignissen verwendet.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="technical-configuration-offer-level"></a>Technische Konfiguration (Angebotsebene)

>[!Note]
>Technische Details auf Angebotsebene sind optional.  Sie müssen diese Details nur konfigurieren, wenn Sie eine verwaltete Anwendung mit getakteter Abrechnung veröffentlichen und über einen Dienst verfügen, der mit einem Azure AD-Sicherheitstoken authentifiziert wird. Weitere Informationen über die verschiedenen Authentifizierungsoptionen finden Sie unter [Authentifizierungsstrategien](./marketplace-metering-service-authentication.md).

Mit der technischen Konfiguration werden die Details (Mandanten-ID und App-ID) definiert, die zur Identifikation Ihres Diensts dienen, der Messereignisse für eine verwaltete Anwendung mit den [Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md) ausgibt.  Geben Sie die Identität an, die Ihr Dienst beim Ausgeben von Messereignissen verwendet.

* **ID des Azure AD-Mandanten** (erforderlich): Im Azure-Portal müssen Sie eine [Azure AD-App (Active Directory) erstellen](../../active-directory/develop/howto-create-service-principal-portal.md), damit überprüft werden kann, ob die Verbindung zwischen unseren beiden Diensten über eine authentifizierte Kommunikation erfolgt. Rufen Sie Ihre Azure Active Directory-Instanz auf, klicken Sie auf **Eigenschaften**, und suchen Sie dann nach der **Verzeichnis-ID** (z. B. „50c464d3-4930-494c-963c-1e951d15360e“), um die [Mandanten-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) zu finden.
* **Azure AD-App-ID** (erforderlich): Sie benötigen auch die [Anwendungs-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) und einen Authentifizierungsschlüssel. Rufen Sie Ihre Azure Active Directory-Instanz auf, klicken Sie auf **App-Registrierungen**, und suchen Sie dann nach der **Anwendungs-ID** (z. B. „50c464d3-4930-494c-963c-1e951d15360e“), um diese Werte abzurufen. Um den Authentifizierungsschlüssel zu suchen, wechseln Sie zu **Einstellungen**, und wählen Sie **Schlüssel** aus. Sie müssen eine Beschreibung und eine Dauer angeben und erhalten dann einen Zahlenwert.

>[!Note]
>Die Azure-Anwendungs-ID wird Ihrer Herausgeber-ID zugeordnet und kann nur mit diesem Herausgeberkonto wiederverwendet werden.

>[!Note]
>Diese Konfiguration ist erforderlich, wenn Sie das [Batchnutzungsereignis](marketplace-metering-service-apis.md#metered-billing-batch-usage-event) verwenden möchten.  Wenn Sie [Nutzungsereignisse](marketplace-metering-service-apis.md#metered-billing-single-usage-event) übermitteln möchten, können Sie [Instance Metadata Service](../../active-directory/managed-identities-azure-resources/overview.md) verwenden, um das [JWT-Bearertoken (JSON Web Token)](pc-saas-registration.md#how-to-get-the-publishers-authorization-token) abzurufen.

## <a name="plan-overview"></a>Planübersicht

Auf dieser Registerkarte können Sie verschiedene Planoptionen innerhalb desselben Angebots bereitstellen. Diese Pläne (vorher als SKUs bezeichnet) können sich in Bezug auf den Plantyp (Lösungsvorlage oder verwaltete Anwendung), die Monetarisierung oder Zielgruppe unterscheiden. Konfigurieren Sie mindestens einen Plan, um Ihr Angebot im Marketplace zu listen.

Sie können bis zu 100 Pläne für jedes Angebot erstellen. Bis zu 45 davon können privat sein. Erfahren Sie unter [Private Angebote im kommerziellen Microsoft-Marketplace](../private-offers.md) mehr über private Pläne.

Nach der Erstellung werden die Plannamen, die IDs und Plantypen, die Verfügbarkeit (öffentlich oder privat), der aktuelle Veröffentlichungsstatus und alle verfügbaren Aktionen auf dieser Registerkarte angezeigt.

Die unter **Planübersicht** verfügbaren **Aktionen** variieren je nach dem aktuellen Status des Plans und umfassen möglicherweise:

* Wenn der Planstatus **Entwurf** lautet: Entwurf löschen.
* Wenn der Planstatus **Live** lautet: Verkauf des Plans einstellen oder private Zielgruppe synchronisieren.

### <a name="create-new-plan"></a>Erstellen eines neuen Plans

***Plan-ID:*** Erstellen Sie für jeden Plan im Angebot eine eindeutige Plan-ID. Diese ID ist für Kunden in der Produkt-URL sichtbar.  Verwenden Sie nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche. Für die Plan-ID sind maximal 50 Zeichen zulässig. Die Plan-ID kann nach der Erstellung nicht geändert werden.

***Planname***: Kunden sehen diesen Namen, wenn sie einen Plan in Ihrem Angebot auswählen. Erstellen Sie für jeden Plan im Angebot einen eindeutigen Angebotsnamen. Der Planname wird verwendet, um Softwarepläne zu unterscheiden, die möglicherweise Teil desselben Angebots sind (z. B. Angebotsname: Windows Server, Pläne: Windows Server 2016, Windows Server 2019).

### <a name="plan-setup"></a>Plansetup

Mithilfe dieser Registerkarte können Sie die allgemeine Konfiguration für den Plantyp festlegen sowie angeben, ob Pakete von einem anderen Plan wiederverwendet werden und in welchen Clouds der Plan verfügbar sein soll. Ihre Antworten auf dieser Registerkarte beeinflussen, welche Felder auf anderen Registerkarten desselben Plans angezeigt werden.

#### <a name="plan-type"></a>Plantyp
Wählen Sie den Plantyp für Ihr Angebot aus. Ein Plan mit **Lösungsvorlage** wird vollständig vom Kunden verwaltet. Ein Plan mit **verwalteter Anwendung** ermöglicht Herausgebern, die Anwendung für den Kunden zu verwalten. Ausführliche Informationen finden Sie unter [Typen von Azure-Anwendungsplänen](#types-of-azure-application-plans).

#### <a name="re-use-technical-configuration"></a>Wiederverwenden von technischen Konfigurationen

Wenn Sie über mehrere Pläne desselben Typs verfügen und deren Pakete identisch sind, können Sie auswählen, dass **dieser Plan Pakete aus einem anderen Plan wiederverwendet**.  Wenn Sie diese Option aktivieren, können Sie einen der anderen Pläne desselben Typs für dieses Angebot auswählen, dessen Pakete wiederverwendet werden sollen.

>[!Note]
>Wenn Sie Pakete aus einem anderen Plan wiederverwenden, wird die gesamte Registerkarte für die technische Konfiguration in diesem Plan nicht mehr angezeigt. Die technischen Konfigurationsdetails aus dem anderen Plan, einschließlich aller Updates, die Sie in Zukunft vornehmen, werden für diesen Plan ebenfalls verwendet.<br><br>Diese Einstellung kann nicht geändert werden, sobald der Plan veröffentlicht wurde.

#### <a name="azure-regions-cloud"></a>Azure-Regionen (Cloud)

Ihr Plan muss in mindestens einer Azure-Region verfügbar gemacht werden.

Wählen Sie die Option **Azure Global** aus, um Ihren Plan für Kunden in allen globalen Azure-Regionen mit Integration des kommerziellen Marketplace verfügbar zu machen. Details finden Sie unter [Geografische Verfügbarkeit und Unterstützung von Währungen](../marketplace-geo-availability-currencies.md).

Wählen Sie die Option **Azure Government** aus, um Ihren Plan in der [Azure Government](../../azure-government/documentation-government-welcome.md)-Region verfügbar zu machen. Diese Region bietet kontrolliertem Zugriff für Kunden der US-Behörden auf Bundes-, Bundesstaats-, Gemeinde- oder Stammesgebietsebene sowie für Partner, die diese beliefern können. Als Herausgeber sind Sie für alle Kompatibilitätskontrollen, Sicherheitsmaßnahmen und bewährten Methoden verantwortlich. Azure Government verwendet physisch isolierte Rechenzentren und Netzwerke (die sich ausschließlich in den USA befinden).

Testen und validieren Sie Ihren Plan vor dem Veröffentlichen für [Azure Government](../../azure-government/documentation-government-manage-marketplace-partners.md) in der Umgebung, da sich bestimmte Endpunkte möglicherweise unterscheiden. Fordern Sie für Einrichten und Testen Ihres Plans ein Testkonto bei [Microsoft Azure Government-Test](https://azure.microsoft.com/global-infrastructure/government/request/) an.

>[!NOTE]
>Nachdem Ihr Plan veröffentlicht und in einer bestimmten Azure-Region verfügbar gemacht wurde, können Sie diese Region nicht mehr entfernen.

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

### <a name="availability-solution-template-plans-only"></a>Verfügbarkeit (nur Pläne mit Lösungsvorlage)

Sie können den Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) sichtbar machen oder festlegen, dass der Plan ausgeblendet werden und nur für die Verwendung durch andere Lösungsvorlagen oder verwaltete Anwendungen verwendbar sein soll.

#### <a name="plan-visibility"></a>Sichtbarkeit des Plans

Sie können festlegen, dass jeder Plan für alle Benutzer oder nur für eine von Ihnen ausgewählte Zielgruppe angezeigt wird. Sie können mithilfe von Azure Abonnement-IDs die Mitgliedschaft in dieser eingeschränkten Zielgruppe zuweisen.

Aktivieren Sie die Option **Dies ist ein privater Plan**, um den Plan als privaten Plan festzulegen und nur für die von Ihnen ausgewählte eingeschränkte Zielgruppe anzuzeigen. Nachdem Sie den Plan als privaten Plan veröffentlicht haben, können Sie die Zielgruppe aktualisieren oder den Plan für alle Benutzer verfügbar machen. Sobald ein Plan als für alle Benutzer als sichtbar veröffentlicht wurde, muss er für alle Benutzer sichtbar bleiben. Der Plan kann nicht in einen privaten Plan umkonfiguriert werden.

Wenn Sie den Plan als privat konfigurieren, geben Sie **Azure-Abonnement-IDs** und eine dazugehörige Beschreibung ein. Jede entspricht einer Zielgruppe, die auf diesen privaten Plan zugreifen kann. Der Zugriff wird mit Azure-Abonnement-IDs zugewiesen. Dabei kann eine Beschreibung jeder zugewiesenen Azure-Abonnement-ID angegeben werden. Fügen Sie bis zu 10 Abonnement-IDs von Kunden einzeln hinzu, oder fügen Sie durch Importieren einer CSV-Datei 20.000 hinzu. Azure-Abonnement-IDs werden als GUIDs dargestellt. Buchstaben müssen in Kleinbuchstaben angegeben werden.

>[!Note]
>Eine private oder eingeschränkte Zielgruppe unterscheidet sich von der auf der Registerkarte **Vorschau** definierten Vorschauzielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, _bevor_ es in den Marketplaces live veröffentlicht wird. Die Auswahl der privaten Zielgruppe gilt nur für einen bestimmten Plan, aber die private Zielgruppe kann alle Pläne (private und nicht private Pläne) zu Validierungszwecken anzeigen.

#### <a name="hide-plan"></a>Plan ausblenden

Soll die Lösungsvorlage nur indirekt, also nur bei Verweisen über eine andere Lösungsvorlage oder eine verwaltete Anwendung bereitgestellt werden, aktivieren Sie das Kontrollkästchen, um Ihre Lösungsvorlage zu veröffentlichen aber für Kunden, die danach direkt suchen oder blättern, auszublenden.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="pricing-and-availability-managed-application-plans-only"></a>Preise und Verfügbarkeit (nur für Pläne mit verwalteter Anwendung)

Sie können diese Registerkarte zum Konfigurieren der **Märkte**, in denen der Plan verfügbar ist, der monatlichen **Tarife** für die Verwaltung der Lösung und der **Sichtbarkeit des Plans** verwenden, wenn er nur für spezifische Kunden angezeigt werden soll (d. h. eine private Zielgruppe).

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

#### <a name="markets"></a>Märkte

Jeder Plan muss in mindestens einem Markt verfügbar sein. Aktivieren Sie das Kontrollkästchen für jeden Marktstandort, in dem Sie diesen Plan zur Verfügung stellen möchten. Zur Unterstützung sind ein Suchfeld und eine Schaltfläche zum Auswählen von Ländern/Regionen vorhanden, in denen die Umsatz- und Gebrauchssteuer von Microsoft für Sie überwiesen wird.

Wenn Sie bereits Preise in US-Dollar für Ihren Plan festgelegt haben und einen anderen Marktstandort hinzufügen, wird der Preis für den neuen Markt entsprechend dem aktuellen Wechselkurs berechnet. Überprüfen Sie vor dem Veröffentlichen immer den Preis für jeden Markt. Die Preise können nach dem Speichern der Änderungen mithilfe des Links „Export prices (xlsx)“ (Preise exportieren (.xlsx)) überprüft werden.

#### <a name="pricing"></a>Preise

Geben Sie den Preis pro Monat für diesen Plan an.  Dieser Preis gilt zusätzlich zu allen Azure-Infrastruktur- oder nutzungsbasierten Softwarekosten, die durch die von dieser Lösung bereitgestellten Ressourcen entstehen.

Zusätzlich zu den monatlichen Preisen können Sie die Preise für die Nutzung von nicht standardmäßigen Einheiten mithilfe der [getakteten Abrechnung](./azure-app-metered-billing.md) festlegen.  Wenn Sie möchten, können Sie den monatlichen Preis auf null (0) festlegen und ausschließlich die getaktete Abrechnung verwenden.

Preise in USD (USD = US-Dollar) werden beim Speichern gemäß dem aktuellen Wechselkurs in die lokale Währung aller ausgewählten Märkte konvertiert. Überprüfen Sie vor der Veröffentlichung die Preise in jedem Markt, indem Sie die Preistabellenkalkulation exportieren. Wenn Sie benutzerdefinierte Preise in einem einzelnen Markt festlegen möchten, ändern und importieren Sie die Preistabellenkalkulation.

>[!Note]
>Sie müssen Ihre Preisänderungen speichern, damit die Preisdaten exportiert werden können.

Überprüfen Sie Ihre Preise vor der Veröffentlichung sorgfältig, da nicht alle Elemente eines Plans nach seiner Veröffentlichung geändert werden können.  

>[!Note]
>Nachdem ein Preis für einen Markt in Ihrem Plan veröffentlicht wurde, kann er nicht mehr geändert werden.

#### <a name="plan-visibility"></a>Sichtbarkeit des Plans

Sie können festlegen, dass jeder Plan für alle Benutzer oder nur für eine von Ihnen ausgewählte Zielgruppe angezeigt wird. Sie können mithilfe von Azure Abonnement-IDs die Mitgliedschaft in dieser eingeschränkten Zielgruppe zuweisen.

Aktivieren Sie die Option **Dies ist ein privater Plan**, um den Plan als privaten Plan festzulegen und nur für die von Ihnen ausgewählte eingeschränkte Zielgruppe anzuzeigen. Nachdem Sie den Plan als privaten Plan veröffentlicht haben, können Sie die Zielgruppe aktualisieren oder den Plan für alle Benutzer verfügbar machen. Sobald ein Plan als für alle Benutzer als sichtbar veröffentlicht wurde, muss er für alle Benutzer sichtbar bleiben. Der Plan kann nicht in einen privaten Plan umkonfiguriert werden.

>[!Note]
>Eine private oder eingeschränkte Zielgruppe unterscheidet sich von der auf der Registerkarte **Vorschau** definierten Vorschauzielgruppe. Eine Vorschauzielgruppe kann auf das Angebot zugreifen, _bevor_ es in den Marketplaces live veröffentlicht wird. Die Auswahl der privaten Zielgruppe gilt nur für einen bestimmten Plan, aber die private Zielgruppe kann alle Pläne (private und nicht private Pläne) zu Validierungszwecken anzeigen.

Wenn Sie den Plan als privat konfigurieren, geben Sie **Azure-Abonnement-IDs** und eine dazugehörige Beschreibung ein. Jede entspricht einer Zielgruppe, die auf diesen privaten Plan zugreifen kann. Der Zugriff wird mit Azure-Abonnement-IDs zugewiesen. Dabei kann eine Beschreibung jeder zugewiesenen Azure-Abonnement-ID angegeben werden. Fügen Sie bis zu 10 Abonnement-IDs von Kunden einzeln hinzu, oder fügen Sie durch Importieren einer CSV-Datei 20.000 hinzu. Azure-Abonnement-IDs werden als GUIDs dargestellt. Buchstaben müssen in Kleinbuchstaben angegeben werden.

>[!Note]
>Private Angebote werden bei Azure-Abonnements, die über einen Handelspartner des Cloud Solution Provider-Programms (CSP) eingerichtet wurden, nicht unterstützt.

### <a name="technical-configuration"></a>Technische Konfiguration

Auf dieser Registerkarte können Sie das Bereitstellungspaket hochladen, das Kunden erlaubt, Ihren Plan bereitzustellen.

>[!Note]
>Diese Registerkarte wird nicht angezeigt, wenn Sie den Plan auf der Registerkarte **Plansetup** so konfiguriert haben, dass Pakete aus einem anderen Plan wiederverwendet werden.

#### <a name="package-details"></a>Paketdetails

Auf dieser Registerkarte können Sie die Entwurfsversion Ihrer technischen Konfiguration bearbeiten.

**Version:** Weisen Sie die aktuelle Version der technischen Konfiguration zu.  Erhöhen Sie diese Version jedes Mal, wenn Sie eine Änderung auf dieser Seite veröffentlichen. Die Version muss im Format `{integer}.{integer}.{integer}` angegeben werden.

**Paketdatei** (.zip): Dieses Paket enthält alle für diesen Plan erforderlichen Vorlagendateien und alle zusätzlichen Ressourcen in einer `.zip`-Datei.

Alle Azure-Anwendungsplanpakete müssen diese zwei Dateien im Stammordner eines `.zip`-Archivs enthalten:

* Eine Resource Manager-Vorlagendatei mit dem Namen [mainTemplate.json](../../azure-resource-manager/resource-group-overview.md). Diese Vorlage automatisiert die Bereitstellung von Ressourcen im Azure-Abonnement des Kunden.  Beispiele für Resource Manager-Vorlagen finden Sie im [Katalog der Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) oder im entsprechenden Repository [GitHub: Azure Resource Manager Quickstart Templates](https://github.com/azure/azure-quickstart-templates) (Azure Resource Manager-Schnellstartvorlagen).
* Eine Benutzeroberflächendefinition für die Oberfläche zum Erstellen von Azure-Anwendungen mit dem Namen [createUiDefinition.json](../../azure-resource-manager/managed-application-createuidefinition-overview.md).

Unterstützte maximale Dateigrößen:

* Bis zu 1 GB komprimierte Gesamtgröße des `.zip`-Archivs
* Bis zu 1 GB für jede einzelne nicht komprimierte Datei innerhalb des `.zip`-Archivs  

Alle neuen Azure-Anwendungsangebote müssen auch eine [GUID zum Zuordnen der Nutzung durch Kunden von Azure-Partnern](../azure-partner-customer-usage-attribution.md) enthalten.

>[!Note]
>Wenn beim Hochladen von Dateien ein Problem auftritt, vergewissern Sie sich, dass das lokale Netzwerk den von Partner Center verwendeten Dienst https://upload.xboxlive.com nicht blockiert.

Pläne mit verwalteter Anwendung erfordern zusätzliche Informationen auf dieser Registerkarte.

#### <a name="previously-published-packages"></a>Zuvor veröffentlichte Pakete

Auf der Unterregisterkarte **Zuvor veröffentlichte Pakete** können Sie alle veröffentlichten Versionen der technischen Konfiguration anzeigen.

#### <a name="enable-just-in-time-jit-access"></a>Just-in-Time-Zugriff (JIT) aktivieren

Wählen Sie diese Option, um den Just-in-time-Zugriff (JIT) für diesen Plan zu aktivieren.  Der JIT-Zugriff ermöglicht es Ihnen, erhöhte Zugriffsrechte auf die Ressourcen einer verwalteten Anwendung zur Problembehandlung oder Wartung anzufordern. Sie verfügen immer über Lesezugriff auf die Ressourcen, aber für einen bestimmten Zeitraum können Sie über erhöhte Zugriffsrechte verfügen.  Weitere Informationen finden Sie unter [Aktivieren und Anfordern des Just-In-time-Zugriffs für Azure Managed Applications](../../managed-applications/request-just-in-time-access.md).  Lassen Sie diese Option deaktiviert, um festzulegen, dass Ihnen von den Consumern Ihrer verwalteten Anwendung dauerhaft Zugriff gewährt werden muss.

>[!Note]
>Stellen Sie sicher, dass Sie Ihre `createUiDefinition.json`-Datei aktualisiert wurde, damit dieses Feature unterstützt wird.  

#### <a name="deployment-mode"></a>Bereitstellungsmodus

Wählen Sie aus, ob Sie den **vollständigen** oder **inkrementellen Bereitstellungsmodus** bei der Bereitstellung dieses Plans konfigurieren möchten: 

* Im **vollständigen Modus** führt eine erneute Bereitstellung der Anwendung durch den Kunden zur Entfernung von Ressourcen aus der verwalteten Ressourcengruppe, wenn die Ressourcen nicht in der Datei `mainTemplate.json` definiert sind. 
* Im **inkrementellen Modus** bleiben bestehende Ressourcen bei einer erneuten Bereitstellung der Anwendung unverändert.

Weitere Informationen zu den Bereitstellungsmodi finden Sie unter [Azure Resource Manager-Bereitstellungsmodi](../../azure-resource-manager/deployment-modes.md).

#### <a name="notification-endpoint-url"></a>Benachrichtigungsendpunkt-URL

Geben Sie einen HTTPS-Webhookendpunkt an, um Benachrichtigungen zu allen CRUD-Vorgängen für verwaltete Anwendungsinstanzen dieser Planversion zu erhalten.

#### <a name="customize-allowed-customer-actions"></a>Zulässige Kundenaktionen anpassen

Wählen Sie diese Option aus, um festzulegen, welche Aktionen Kunden neben den standardmäßig verfügbaren „`*/read`“-Aktionen für verwaltete Ressourcen ausführen können.

Listen Sie hier die zusätzlichen Aktionen auf, durch Semikolons getrennt, die Sie Ihren Kunden ermöglichen möchten.  Weitere Informationen finden Sie unter Weitere Informationen finden Sie unter [Grundlegendes zu Ablehnungszuweisungen für Azure-Ressourcen](../../role-based-access-control/deny-assignments.md). Die verfügbaren Aktionen finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../../role-based-access-control/resource-provider-operations.md). Beispiel: Wenn Sie Kunden das Neustarten virtueller Computer erlauben möchten, fügen Sie den zulässigen Aktionen `Microsoft.Compute/virtualMachines/restart/action` hinzu.

#### <a name="global-azure--azure-government-cloud"></a>Global Azure / Azure Government Cloud

Geben Sie an, wer in jeder unterstützten Cloud über Verwaltungszugriff auf diese verwaltete Anwendung verfügen soll. Benutzer, Gruppen oder Anwendungen, denen Berechtigungen für die verwaltete Ressourcengruppe erteilt werden sollen, werden mithilfe von Azure Active Directory-Identitäten (AD) identifiziert.

**Azure Active Directory-Mandanten-ID**: Hierbei handelt es sich um die Azure AD-Mandanten-ID (auch als Verzeichnis-ID bezeichnet), die die Identitäten der Benutzer, Gruppen oder Anwendungen enthält, denen Sie Berechtigungen erteilen möchten. Sie finden Ihre Azure AD-Mandanten-ID im Azure-Portal unter [Eigenschaften für Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties).

**Autorisierungen:** Fügen Sie die Azure Active Directory-Objekt-ID des Benutzers, der Gruppe oder der Anwendung hinzu, dem oder der die Berechtigung für die verwaltete Ressourcengruppe erteilt werden soll. Sie können den Benutzer anhand seiner Prinzipal-ID auf dem [Blatt „Azure Active Directory Benutzer“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) identifizieren.

Wählen Sie für jeden Prinzipal eine der in Azure AD integrierten Rollen aus der Liste aus (Besitzer oder Mitwirkender). Die von Ihnen ausgewählte Rolle beschreibt die Berechtigungen, die der Prinzipal für die Ressourcen im Kundenabonnement erhält. Weitere Informationen finden Sie unter [Integrierte Azure-Rollen](../../role-based-access-control/built-in-roles.md). Weitere Informationen zur rollenbasierten Zugriffssteuerung (RBAC) finden Sie unter [Erste Schritte mit RBAC im Azure-Portal](../../role-based-access-control/overview.md).

>[!Note]
>Obwohl Sie bis zu 100 Autorisierungen pro Cloud hinzufügen können, ist es im Allgemeinen einfacher, eine Active Directory-Benutzergruppe zu erstellen und ihre ID in der „Prinzipal-ID“ anzugeben. Dies ermöglicht Ihnen, nach der Bereitstellung des Plans weitere Benutzer zur Verwaltungsgruppe hinzuzufügen, und verringert die Notwendigkeit, den Plan zu aktualisieren, nur um weitere Autorisierungen hinzuzufügen.

#### <a name="policy-settings"></a>Richtlinieneinstellungen

Wenden Sie [Azure-Richtlinien](../../governance/policy/overview.md) auf Ihre verwaltete Anwendung an, um Complianceanforderungen für die bereitgestellte Lösung anzugeben. Informationen zu Richtliniendefinitionen und zum Format der Parameterwerte finden Sie unter [Azure Policy-Beispiele](../../governance/policy/samples/index.md). Sie können maximal fünf Richtlinien und nur eine Instanz jeder einzelnen Richtlinienoption konfigurieren. Einige Richtlinien erfordern zusätzliche Parameter. Der Standard-SKU ist für Überwachungsrichtlinien erforderlich. Richtlinienname darf maximal 50 Zeichen lang sein.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="co-sell-with-microsoft"></a>Co-Selling mit Microsoft

Die Bereitstellung von Informationen auf der Registerkarte „Co-Selling“ ist für die Veröffentlichung Ihres Angebots völlig optional. Es ist erforderlich, um den Status „Bereit für Co-Selling“ (Co-sell Ready) und „Bereit für IP-Co-Selling“ (IP Co-sell Ready) zu erreichen. Die von Ihnen angegebenen Informationen werden von den Microsoft-Vertriebsteams verwendet, um mehr über Ihre Lösung zu erfahren, wenn sie deren Eignung für die Anforderungen Ihrer Kunden auswerten. Sie sind nicht direkt für Kunden verfügbar.

Weitere Informationen über diese Registerkarte finden Sie unter [Co-Selling-Option in Partner Center](commercial-marketplace-co-sell.md).

## <a name="resell-through-csps"></a>Verkaufen über CSPs

Erweitern Sie die Reichweite Ihres Angebots, indem Sie es für Partner im [CSP](https://azure.microsoft.com/offers/ms-azr-0145p/)-Programm (Cloud Solution Providers) verfügbar machen. Dadurch können Handelspartner Ihr Angebot an ihre Kunden verkaufen und gebündelte Lösungen erstellen.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="test-drive"></a>Testversion

Richten Sie eine Demonstration (Testversion) ein, mit der Kunden Ihr Angebot vor dem Erwerb ausprobieren können. Informationen zum Erstellen einer Demonstrationsumgebung, mit der Kunden Ihr Angebot für einen bestimmten Zeitraum testen können, finden Sie unter [Ermöglichen Sie Ihren Kunden das Testen Ihres Angebots](test-drive.md).

Aktivieren Sie auf der Registerkarte [Angebotseinrichtung](#test-drive) das Kontrollkästchen **Testversion aktivieren**, um eine Testversion zu aktivieren. Deaktivieren Sie dieses Kontrollkästchen, um die Testversion aus Ihrem Angebot zu entfernen.

### <a name="test-drive-technical-configuration"></a>Technische Konfiguration der Testversion

- **Azure AD-App-ID** (erforderlich): Geben Sie die [Anwendungs-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) von Azure Active Directory (AD) ein. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **App-Registrierungen**, und suchen Sie dann die **Anwendungs-ID** (z. B. „50c464d3-4930-494c-963c-1e951d15360e“), um diese ID zu finden.

#### <a name="deployment-subscription-details"></a>Abonnementdetails für Bereitstellung der Testversion

Erstellen Sie ein separates eindeutiges Azure-Abonnement, und stellen Sie es bereit (für Power BI-Testversionen nicht erforderlich), damit die Testversion in Ihrem Auftrag bereitstellen kann.

* **Azure-Abonnement-ID** (erforderlich für den Azure Resource Manager und Logic Apps): Geben Sie die Abonnement-ID ein, um für Ressourcennutzungsberichte und Abrechnungszwecke Zugriff auf die Dienste Ihres Azure-Kontos zu gewähren. Es wird empfohlen, [ein eigenes Azure-Abonnement zu erstellen](../../billing/billing-create-subscription.md), das für Testversionen verwendet wird, falls Sie es noch nicht getan haben. Sie finden die Azure-Abonnement-IDs, indem Sie sich beim [Azure-Portal](https://portal.azure.com/) anmelden und im Menü auf der linken Seite zu den **Abonnements** navigieren. Durch Auswahl der Registerkarte wird Ihre Abonnement-ID angezeigt (z. B. „a83645ac-1234-5ab6-6789-1h234g764ghty“).
* **Azure AD-Mandanten-ID** (erforderlich): Geben Sie Ihre [Mandanten-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) für Azure Active Directory (AD) ein. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **Eigenschaften**, und suchen Sie dann die **Verzeichnis-ID** (z. B. „50c464d3-4930-494c-963c-1e951d15360e“), um diese ID zu finden. Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe einer Domänennamen-URL nachschlagen.
* **Azure AD tenant name** (Name des Azure AD-Mandanten) (erforderlich für Dynamics 365): Geben Sie den Namen Ihrer Azure Active Directory-Instanz (AD) ein. Um diesen Namen zu suchen, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Oben rechts wird der Mandantenname unter Ihrem Kontonamen angegeben.
* **Azure AD-App-ID** (erforderlich): Geben Sie Ihre [Anwendungs-ID](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) für Azure Active Directory (AD) ein. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Navigationsmenü die Registerkarte „Active Directory“ aus, klicken Sie auf **App-Registrierungen**, und suchen Sie dann die **Anwendungs-ID** (z. B. „50c464d3-4930-494c-963c-1e951d15360e“), um diese ID zu finden.
* **Clientgeheimnis für Azure Active Directory-Anwendung** (erforderlich): Geben Sie den [geheimen Clientschlüssel](../../active-directory/develop/howto-create-service-principal-portal.md#create-a-new-application-secret) Ihrer Azure AD-Anwendung ein. Um diesen Wert zu finden, melden Sie sich beim [Azur-Portal](https://portal.azure.com/) an. Wählen Sie im linken Navigationsmenü die Registerkarte **Azure Active Directory** aus, klicken Sie auf **App-Registrierungen**, und wählen Sie dann die Testversion Ihrer App aus. Wählen Sie als Nächstes **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, geben Sie eine Beschreibung ein, wählen Sie **Nie** unter **Läuft ab** aus, und wählen Sie dann **Hinzufügen** aus. (Stellen Sie sicher, dass Sie sich den Wert notieren, bevor Sie diese Seite schließen.)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

### <a name="marketplace-listing-optional"></a>Marketplace-Angebot (optional)

Hier beschreiben Sie die Testversion.

<!-- The **Test Drive listings** option found under the **Test drive** tab displays the languages (and markets) where your test drive is available (currently English (United States) is the only location available). Additionally, this page displays the status of the language-specific listing and the date/time that it was added. You will need to define the test drive details (description, user manual, videos, etc.) for each language/market. -->

* **Beschreibung** (erforderlich): Beschreiben Sie Ihre Testversion. Geben Sie an, was in der Demonstration enthalten ist, nennen Sie die Ziele, mit denen der Benutzer experimentieren soll, sowie die zu erkundenden Features, und geben Sie relevante Informationen an, die dem Benutzer helfen, zu entscheiden, ob er Ihr Angebot erwerben soll. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 
* **Zugriffsinformationen** (erforderlich für Azure Resource Manager- und Logik-Testversionen): Erläutern Sie, was ein Kunde wissen muss, um auf diese Testversion zuzugreifen und diese zu verwenden. Beschreiben Sie ein Szenario für die Verwendung Ihres Angebots, und teilen Sie genau mit, was der Kunde wissen sollte, um auf die Funktionen in der gesamten Testversion zuzugreifen. In diesem Feld können bis zu 10.000 Zeichen Text eingegeben werden.
* **Benutzerhandbuch** (erforderlich): Hierbei handelt es sich um eine ausführliche Beschreibung der Verwendung Ihrer Testversion. Das Benutzerhandbuch sollte genau beschreiben, was der Kunde durch die Erkundung der Testversion erlangen soll, und als Referenz für die Fragen des Kunden dienen. Die Datei muss das PDF-Format aufweisen und nach dem Hochladen mit einem Namen (max. 255 Zeichen) versehen werden.
* **Videos: Add videos** (Videos: Videos hinzufügen) (optional): Videos können auf YouTube oder Vimeo hochgeladen werden, und hier kann mit einem Link und einer Miniaturansicht (533 × 324 Pixel) auf sie verwiesen werden. So erhalten Kunden Informationen zu einzelnen Schritten, um sich besser mit der Testversion vertraut zu machen. Unter anderem können Sie ihnen zeigen, wie sie die Funktionen Ihres Angebots erfolgreich verwenden, und Szenarios erläutern, in denen die Vorteile hervorgehoben werden.
  * **Name** (erforderlich)
  * **Adresse** (nur YouTube oder Vimeo, erforderlich)
  * **Miniaturansicht** (Bilddatei muss im PNG-Format und 533 x 324 Pixel groß sein)

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="publish"></a>Veröffentlichen

Nachdem Sie alle erforderlichen Abschnitte des Angebots ausgefüllt haben, klicken Sie rechts oben im Portal auf **Überprüfen und veröffentlichen**.

Überprüfen Sie den Abschlussstatus der einzelnen Abschnitte des Angebots.
    - *Nicht gestartet*: Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
    - *Unvollständig*: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - *Abgeschlossen*: Der Abschnitt ist abgeschlossen. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.

Wenn Sie dieses Angebot zum ersten Mal veröffentlichen, können Sie Testanweisungen für das Zertifizierungsteam bereitstellen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird. Außerdem können Sie ergänzende Hinweise bereitstellen, die das Verständnis Ihrer App erleichtern.

Klicken Sie auf **Senden**, um Ihr Angebot zur Veröffentlichung zu übermitteln. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können.

Kehren Sie zu Partner Center zurück, und wählen Sie **Live schalten** aus, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

### <a name="errors-and-review-feedback"></a>Fehler und Prüfungsfeedback

Der Schritt **Manuelle Validierung** im Veröffentlichungsvorgang stellt eine umfassende Überprüfung Ihres Angebots und der damit verbundenen technischen Ressourcen (insbesondere der Azure Resource Manager-Vorlage) dar, und Probleme werden in der Regel als Pull Request-Links dargestellt. Eine Erklärung, wie Sie diese Pull Requests anzeigen und auf sie reagieren, finden Sie unter [Verarbeiten von Prüfungsfeedback](./azure-apps-review-feedback.md).

Wenn Fehler bei einem oder mehreren Veröffentlichungsschritten aufgetreten sind, beheben Sie diese, bevor Sie Ihr Angebot erneut veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

* [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
