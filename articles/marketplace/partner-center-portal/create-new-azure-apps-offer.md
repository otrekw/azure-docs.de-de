---
title: Erstellen eines neuen Angebots für Azure-Apps im kommerziellen Marketplace
description: Erstellen eines neuen Angebots für Azure-Apps zum Auflisten oder Verkaufen in Azure Marketplace, AppSource oder über das CSP-Programm (Cloud Solution Provider) mit dem kommerziellen Marketplace-Portal im Microsoft Partner Center.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: d7e05f12c04136c8394dbcb27b7a950fc5ce85d9
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74281044"
---
# <a name="create-an-azure-application-offer"></a>Erstellen eines Azure-Anwendungsangebots

Hier werden die Schritte zum Veröffentlichen eines Azure-Anwendungsangebots im kommerziellen Marketplace beschrieben.

## <a name="azure-application-offer-type"></a>Angebotstyp „Azure-Anwendung“

In diesem Thema werden die Grundlagen zu Azure-Anwendungsangeboten beschrieben.  Sie sollten sich mit diesen Konzepten vertraut machen, bevor Sie mit dem Veröffentlichen eines neuen Azure-Anwendungsangebots im Marketplace beginnen.

### <a name="publishing-overview"></a>Übersicht über die Veröffentlichung

Das Video [Erstellen von Lösungsvorlagen und verwalteten Anwendungen für den Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) enthält eine Einführung in den Angebotstyp „Azure-Anwendung“:

* Welche Angebotstypen sind verfügbar?
* Welche technischen Ressourcen sind erforderlich?
* Erstellen einer Azure Resource Manager-Vorlage.
* Entwickeln und Testen der App-Benutzeroberfläche.
* Veröffentlichen des App-Angebots.
* Der Anwendungsüberprüfungsprozess.

### <a name="types-of-azure-application-plans"></a>Typen von Azure-Anwendungsplänen

Es gibt zwei Typen von Azure-Anwendungsplänen: verwaltete Anwendungen und Lösungsvorlagen.

* **Lösungsvorlagen** zählen zu den wichtigsten Optionen für die Veröffentlichung von Lösungen im Marketplace. Dieser Plantyp wird verwendet, wenn für eine Lösung neben einem einzelnen virtuellen Computer noch eine zusätzliche Bereitstellungs- und Konfigurationsautomatisierung erforderlich ist.  Mit einer Lösungsvorlage können Sie die Bereitstellung mehrerer Ressourcen automatisieren, beispielsweise VMs, Netzwerke und Speicherressourcen, um komplexe IaaS-Lösungen bereitzustellen.  Weitere Informationen zum Erstellen von Lösungsvorlagen finden Sie in der [Dokumentation zu Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

* **Verwaltete Anwendungen** und Lösungsvorlagen sind bis auf einen wichtigen Unterschied ähnlich. Bei einer verwalteten Anwendung werden die Ressourcen in einer Ressourcengruppe bereitgestellt, die vom Herausgeber der App verwaltet wird. Die Ressourcengruppe befindet sich zwar im Abonnement des Verbrauchers, eine Identität im Mandanten des Herausgebers hat jedoch Zugriff auf die Ressourcengruppe. Als Herausgeber geben Sie die Kosten für den kontinuierlichen Support der Lösung an. Verwenden Sie verwaltete Anwendungen, um mühelos vollständig verwaltete, schlüsselfertige Anwendungen für Ihre Kunden zu erstellen und bereitzustellen.  Weitere Informationen über die Vorteile und Typen von verwalteten Anwendungen finden Sie unter [Übersicht über verwaltete Azure-Anwendungen](https://docs.microsoft.com/azure/managed-applications/overview).

Alle Azure-Anwendungen enthalten mindestens zwei Dateien im Stammordner eines `.zip`-Archivs:

* Eine Resource Manager-Vorlagendatei mit dem Namen [mainTemplate.json](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).  Dies ist die Vorlage, die die Ressourcen definiert, die im Azure-Abonnement des Kunden bereitgestellt werden sollen.  Beispiele für Resource Manager-Vorlagen finden Sie im [Katalog der Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) oder im entsprechenden Repository [GitHub: Azure Resource Manager Quickstart Templates](https://github.com/azure/azure-quickstart-templates) (Azure Resource Manager-Schnellstartvorlagen).

* Eine Benutzeroberflächendefinition für die Oberfläche zum Erstellen von Azure-Anwendungen mit dem Namen [createUiDefinition.json](https://docs.microsoft.com/azure/managed-applications/create-uidefinition-overview).  Auf der Benutzeroberfläche geben Sie Elemente an, die Kunden die Angabe von Parameterwerten ermöglichen.

Alle neuen Azure-Anwendungsangebote müssen eine [GUID zum Zuordnen der Nutzung durch Kunden von Azure-Partnern](??) enthalten.

### <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie die folgende Dokumentation zu Azure-Anwendungen. Diese Dokumentation enthält Schnellstarts, Tutorials und Beispiele.

* [Verstehen von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
* Schnellstartanleitungen:

    * [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/)
    * [Azure-Schnellstartvorlagen auf GitHub](https://github.com/azure/azure-quickstart-templates)
    * [Veröffentlichen der Anwendungsdefinition](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
    * [Bereitstellen einer Dienstkatalog-App](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

* Tutorials:

    * [Erstellen von Definitionsdateien](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Veröffentlichen der Marketplace-Anwendung](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

* Beispiele:

    * [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    * [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    * [Verwaltete Anwendungslösungen](https://docs.microsoft.com/azure/managed-applications/sample-projects)

### <a name="fundamentals-in-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure Platform und die Technologien erforderlich, um das Angebot zu erstellen.

Ihr Entwicklerteam sollte sich mit den folgenden Microsoft-Technologien auskennen:

* Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/).
* [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/).
* Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage#storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking#networking).
* Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
* Fundierte Kenntnisse zu [JSON](https://www.json.org/).

### <a name="suggested-tools"></a>Vorgeschlagene Tools

Wählen Sie eine oder beide der folgenden Skriptumgebungen als Unterstützung bei der Verwaltung Ihrer Azure-Anwendung aus:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)

Es empfiehlt sich, dass Sie Ihrer Entwicklungsumgebung die folgenden Tools hinzufügen:

* [Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen:
    * Erweiterung: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    * Erweiterung: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    * Erweiterung: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Die verfügbaren Tools finden Sie auf der Seite [Azure-Entwicklertools](https://azure.microsoft.com/tools/).  Und wenn Sie Visual Studio verwenden, auch auf dem [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-an-azure-application-offer"></a>Erstellen eines Azure-Anwendungsangebots

Bevor Sie ein Azure-Anwendungsangebot erstellen können, müssen Sie zunächst [ein Partner Center-Konto erstellen](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), das [Dashboard „Kommerzieller Marketplace“](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) öffnen, und die Registerkarte **Übersicht** auswählen.

>[!Note]
>Nach der Veröffentlichung eines Angebots werden in Partner Center am Angebot vorgenommene Änderungen erst nach einer erneuten Veröffentlichung im System und den Storefronts aktualisiert.  Sie müssen das Angebot nach dem Vornehmen von Änderungen für die Veröffentlichung einreichen.

### <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

Wählen Sie die Schaltfläche **+ Neues Angebot** und dann das Menüelement **Azure-Anwendung** aus. Das Dialogfeld **Neues Angebot** wird angezeigt.

### <a name="offer-id-and-alias"></a>Angebots-ID und Angebotsalias

* **Angebots-ID:** Ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto. Diese ID wird für Kunden in der URL-Adresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt. <br> <br> Die Angebots-ID muss aus alphanumerischen Zeichen bestehen und darf keine Großbuchstaben enthalten (Bindestriche und Unterstriche sind zulässig, Leerzeichen jedoch nicht). Sie ist auf 50 Zeichen beschränkt und kann nach der Auswahl von „Erstellen“ nicht mehr geändert werden. <br> <br> Wenn Sie hier z.B. `test-offer-1` eingeben, lautet die Angebots-URL `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`. 

* **Angebotsalias**: Der Name, der zum Verweisen auf das Angebot im Partner Center verwendet wird. Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden. Dieser Wert kann nach der Auswahl von **Erstellen** nicht mehr geändert werden.

Nachdem Sie Ihre **Angebots-ID** und ihren **Angebotsalias** eingegeben haben, wählen Sie **Erstellen** aus. Sie können dann alle anderen Teile Ihres Angebots bearbeiten.

## <a name="offer-setup"></a>Angebotseinrichtung

Auf der Seite **Angebotseinrichtung** wird um die folgenden Informationen gebeten. Vergessen Sie nicht, **Speichern** auszuwählen, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

### <a name="test-drive"></a>Testversion

Die Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, vor dem Kauf einen Test durchzuführen, was zu einer höheren Konvertierung und der Generierung von aussichtsreichen Leads führt. [Erfahren Sie mehr über Testversionen.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion zu aktivieren. Anschließend müssen Sie in der [technischen Konfiguration der Testversion](#types-of-azure-application-plans) eine Demo-Umgebung konfigurieren, damit Kunden Ihr Angebot für einen bestimmten Zeitraum testen können. 

>[!Note]
>Da alle Azure-Anwendungen mit einer Azure Resource Manager-Vorlage implementiert werden, ist die einzige Art von Testversion, die für eine Azure-Anwendung konfiguriert werden kann, eine [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

#### <a name="additional-test-drive-resources"></a>Zusätzliche Ressourcen zu Testversionen

- [Test Drive Technical Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte technische Methoden für Testversionen, in englischer Sprache)
- [Testversion: Marketing und Best Practices](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Drives](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (Testversionen, Informationsblatt in englischer Sprache)

## <a name="connect-lead-management"></a>Einbinden der Leadverwaltung

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

## <a name="properties"></a>Properties

Auf der **Eigenschaftenseite** können Sie die Kategorien und Branchen, die zum Gruppieren Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren. Wählen Sie **Speichern** aus, nachdem Sie diese Seite ausgefüllt haben.

### <a name="category"></a>Category

Wählen Sie mindestens eine und höchstens drei Kategorien aus, die zum Platzieren Ihres Angebots in den entsprechenden Marketplace-Suchbereichen verwendet werden. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, wie Ihr Angebot diesen Kategorien entspricht. 

### <a name="standard-marketplace-terms-and-conditions"></a>Standardnutzungsbedingungen von Marketplace

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Verringerung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft die Vorlage „Standardvertrag“ bereit, um eine Transaktion im Marketplace zu erleichtern.

Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Sie Ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen.

Der Standardvertrag ist hier zu finden: https://go.microsoft.com/fwlink/?linkid=2041178

Aktivieren Sie das Kontrollkästchen **Standardvertrag verwenden?** , um den Standardvertrag zu verwenden.

#### <a name="terms-of-use"></a>Nutzungsbedingungen

Wenn Sie das Kontrollkästchen **Standardvertrag verwenden?** nicht aktivieren, müssen Sie Ihre eigenen rechtlichen Bedingungen im Feld **Nutzungsbedingungen** angeben. Geben Sie bis zu 10.000 Textzeichen oder, wenn Ihre Nutzungsbedingungen eine längere Beschreibung erfordern, eine URL ein, über den Ihre zusätzlichen Lizenzbedingungen aufgerufen werden können. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihre App testen können.

## <a name="offer-listing"></a>Angebotsliste

Auf der Seite „Angebotsliste“ werden die Sprachen angezeigt, in denen Ihr Angebot aufgeführt wird. Derzeit ist **Englisch (Vereinigte Staaten)** die einzige verfügbare Option.

Sie müssen für jede Sprache/jeden Markt Marketplace-Informationen (Angebotsname, Beschreibung, Bilder usw.) definieren. Wählen Sie die Sprache bzw. den Namen des Markts aus, um diese Informationen anzugeben.

> [!NOTE]
> Die Inhalte der Angebotsliste (z. B. Beschreibung, Dokumente, Screenshots, Nutzungsbedingungen usw.) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf [nicht englische Sprache] verfügbar.“. Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="name"></a>NAME

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text aufgefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

### <a name="summary"></a>Zusammenfassung

Geben Sie eine kurze Beschreibung Ihres Angebots ein (bis zu 100 Zeichen), die in den Marketplace-Suchergebnissen verwendet werden kann.

### <a name="long-summary"></a>Lange Zusammenfassung

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 256 Zeichen). Diese Beschreibung kann in Marketplace-Suchergebnissen verwendet werden.

### <a name="description"></a>BESCHREIBUNG

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 3.000 Zeichen). Diese Beschreibung wird Kunden in der Übersicht der Marketplace-Auflistung angezeigt. Geben Sie z.B ein Wertversprechen für Ihr Angebot, wichtige Vorteile, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe sowie alle erforderlichen Veröffentlichungen ein. 

Einige Tipps zum Verfassen Ihrer Beschreibung:  

- Schildern Sie in den ersten Sätzen Ihrer Beschreibung deutlich das Nutzenversprechen Ihres Angebots. Fügen Sie folgende Elemente in Ihr Wertversprechen ein:
  - Beschreibung des Produkts
  - Benutzertyp, der von dem Produkt profitiert
  - Anforderungen oder Probleme der Kunden, die das Produkt erfüllt bzw. behebt
- Beachten Sie, dass die ersten Sätze in Suchmaschinenergebnissen angezeigt werden können.  
- Setzen Sie sich nicht nur auf Features und Funktionen, um Ihr Produkt zu verkaufen. Konzentrieren Sie stattdessen auf den Nutzen, den Sie bieten.  
- Verwenden Sie möglichst viel branchenspezifisches Vokabular oder eine nutzenorientierte Formulierung. 
- Verwenden Sie ggf. HTML-Tags, um Ihre Beschreibung zu formatieren und Sie ansprechender zu gestalten.

### <a name="search-keywords"></a>Suchbegriffe

Sie können optional bis zu drei Suchbegriffe eingeben, die Kunden helfen, Ihr Angebot im Marketplace zu finden. Um optimale Ergebnisse zu erzielen, sollten Sie diese Schlüsselwörter auch in ihrer Beschreibung verwenden.

### <a name="support-urls"></a>Support-URLs

In diesem Abschnitt können Sie Links bereitstellen, die Kunden dabei helfen, mehr über Ihr Angebot zu erfahren.

#### <a name="privacy-policy-url"></a>URL zur Datenschutzrichtlinie

Geben Sie die URL zur Datenschutzrichtlinie Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

#### <a name="useful-links"></a>Nützliche Links

Stellen Sie optionale Onlinedokumente zu Ihrer Lösung bereit.  Fügen Sie weitere nützliche Links hinzu, indem Sie auf **+ Link hinzufügen** klicken.

### <a name="contacts"></a>Kontakte

In diesem Abschnitt müssen Sie den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Supportkontakt** und einen **Engineering-Kontakt** angeben. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und können CSP-Partnern bereitgestellt werden.

Im Abschnitt **Supportkontakt** müssen Sie auch die **Support-URL** angeben, unter der CSP-Partner Support für Ihr Angebot erhalten.

### <a name="marketplace-images"></a>Marketplace-Bilder

In diesem Abschnitt können Sie Logos und Bilder bereitstellen, die beim Anzeigen Ihres Angebots für Kunden verwendet werden. Alle Bilder müssen das PNG-Format aufweisen.

#### <a name="store-logos"></a>Store-Logos

Geben Sie das Logo Ihres Angebots in drei Größen an: **Klein (48 x 48)** , **Mittel (90 X 90)** und **Groß (216 x 216)** .

#### <a name="hero"></a>Hero

Das Herobild ist optional. Wenn Sie ein Herobild angeben, muss es in der Größe 815 x 290 Pixel vorliegen.

#### <a name="screenshots"></a>Screenshots

Fügen Sie Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Sie können bis zu fünf Screenshots hinzufügen. Alle Screenshots müssen in der Größe 1280 x 720 Pixel vorliegen.

#### <a name="videos"></a>Videos

Optional können Sie bis zu fünf Videos hinzufügen, die Ihr Angebot veranschaulichen. Diese Videos sollten auf YouTube und/oder Vimeo gehostet werden. Geben Sie für jedes Video den Namen, seine URL und ein Miniaturbild (1280 x 720 Pixel) ein.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Vorschau

Sie können auf der Registerkarte **Vorschau** eine eingeschränkte **Vorschauzielgruppe** für das Validieren Ihres Angebots definieren, bevor Sie das Angebot für die größere Marketplace-Zielgruppe live veröffentlichen.

> [!IMPORTANT]
> Sie müssen **Live schalten** auswählen, bevor Ihr Angebot live für die öffentliche Marketplace-Zielgruppe veröffentlicht wird, nachdem Sie es in der Vorschau überprüft haben.

Ihre Vorschauzielgruppe wird durch Azure-Abonnement-ID-GUIDs, kombiniert mit einer optionalen Beschreibung für jede Gruppe, identifiziert.  Keines dieser Felder ist für Kunden sichtbar.

Fügen Sie bis zu 10 Azure-Abonnement-IDs manuell oder bis zu 100 durch Hochladen einer CSV-Datei hinzu.  Durch das Hinzufügen dieser Abonnements definieren Sie eine Zielgruppe, die Zugriff auf die Vorschau Ihres Angebot erhält, bevor es vollständig veröffentlicht wird.  Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

>[!Note]
>Die Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, *bevor* es in den Marketplaces live veröffentlicht wird. Sie können auch einen Plan erstellen und nur für eine private Zielgruppe verfügbar machen (über die Registerkarte „Verfügbarkeit“ des Plans).  Ihre Vorschauzielgruppe kann alle Pläne sehen und validieren, einschließlich der Pläne, die nach der vollständigen Veröffentlichung Ihres Angebots im Marketplace nur für eine private Zielgruppe verfügbar sind.

## <a name="plan-overview"></a>Planübersicht

Auf der Registerkarte **Planübersicht** können Sie verschiedene Planoptionen innerhalb desselben Angebots bereitstellen. Diese Pläne (Im Cloud-Partnerportal als SKUs bezeichnet) können sich in Bezug auf den Plantyp (Lösungsvorlage oder verwaltete Anwendung), Monetarisierung oder Zielgruppe unterscheiden.  Konfigurieren Sie mindestens einen Plan, um Ihr Angebot im Marketplace zu listen.

Nach der Erstellung werden die Plannamen, die IDs und Plantypen, die Verfügbarkeit (öffentlich oder privat), der aktuelle Veröffentlichungsstatus und alle verfügbaren Aktionen auf dieser Registerkarte angezeigt.

Die unter **Planübersicht** verfügbaren **Aktionen** variieren je nach dem aktuellen Status des Plans und umfassen möglicherweise:

* Wenn der Planstatus **Entwurf** lautet: Entwurf löschen.
* Wenn der Planstatus **Live** lautet: Verkauf des Plans einstellen oder private Zielgruppe synchronisieren.

### <a name="create-new-plan"></a>Erstellen eines neuen Plans

***Plan-ID***: Erstellen Sie für jeden Plan im Angebot eine eindeutige Plan-ID. Diese ID ist für Kunden in der Produkt-URL sichtbar.  Verwenden Sie nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche. Für die Plan-ID sind maximal 50 Zeichen zulässig. Die Plan-ID kann nach der Erstellung nicht geändert werden.

***Planname***: Für Kunden wird dieser Name angezeigt, wenn sie einen Plan in Ihrem Angebot auswählen. Erstellen Sie für jeden Plan im Angebot einen eindeutigen Angebotsnamen. Der Planname wird verwendet, um Softwarepläne zu unterscheiden, die möglicherweise Teil desselben Angebots sind (Beispiel: Angebotsname: Windows Server, Pläne: Windows Server 2016, Windows Server 2019).

### <a name="plan-setup"></a>Plansetup

Mithilfe der Registerkarte **Plansetup** können Sie die allgemeine Konfiguration für den Plantyp festlegen sowie angeben, ob Pakete von einem anderen Plan wiederverwendet werden und in welchen Clouds der Plan verfügbar sein soll.  Ihre Antworten auf dieser Registerkarte beeinflussen, welche Felder auf anderen Registerkarten desselben Plans angezeigt werden.

#### <a name="plan-type"></a>Plantyp

Wählen Sie, wie unter [Typen von Azure-Anwendungsplänen](#types-of-azure-application-plans) beschrieben, aus, ob Ihr Plan eine Lösungsvorlage oder eine verwaltete Anwendung enthält.

#### <a name="this-plan-reuses-packages"></a>In diesem Plan werden Pakete wiederverwendet

Wenn Sie über mehrere Pläne desselben Typs verfügen und deren Pakete identisch sind, können Sie auswählen, dass **dieser Plan Pakete aus einem anderen Plan wiederverwendet**.  Wenn Sie diese Option aktivieren, können Sie einen der anderen Pläne desselben Typs für dieses Angebot auswählen, dessen Pakete wiederverwendet werden sollen. 

>[!Note]
>Wenn Sie Pakete aus einem anderen Plan wiederverwenden, wird die gesamte Registerkarte für die technische Konfiguration in diesem Plan nicht mehr angezeigt.  Die technischen Konfigurationsdetails aus dem anderen Plan, einschließlich aller Updates, die Sie in Zukunft vornehmen, werden für diesen Plan ebenfalls verwendet. <br> <br> Außerdem kann diese Einstellung nicht geändert werden, nachdem der Plan veröffentlicht wurde.

#### <a name="cloud-availability"></a>Cloudverfügbarkeit

Jeder Plan muss in mindestens einer Cloud verfügbar gemacht werden. 

Wählen Sie die Option **Öffentliches Azure** aus, damit Ihre Lösung für Kunden in allen öffentlichen Azure-Regionen mit Marketplace-Integration bereitgestellt werden können.  Erfahren Sie mehr über [geografische Verfügbarkeit](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Wählen Sie die Option **Azure Government Cloud** aus, damit Ihre Lösung in [Azure Government Cloud](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) bereitgestellt werden kann, einer Government-Community-Cloud mit kontrolliertem Zugriff für Kunden der US-Behörden auf Bundes-, Bundesstaats-, Gemeinde- und Stammesgebietsebene und für Partner, die Dienste für diese Entitäten erbringen können.  Als Herausgeber sind Sie für alle Kompatibilitätskontrollen, Sicherheitsmaßnahmen und bewährten Methoden für diese Cloudcommunity verantwortlich.  Azure Government verwendet physisch isolierte Rechenzentren und Netzwerke (die sich nur in den USA befinden).  Microsoft empfiehlt, Ihre Lösung vor dem Veröffentlichen für [Azure Government](https://aka.ms/azuregovpublish) in der Umgebung zu testen und zu validieren, da sich bestimmte Endpunkte möglicherweise unterscheiden. Fordern Sie für Staging und Test Ihrer Lösung ein Testkonto über diesen [Link](https://azure.microsoft.com/global-infrastructure/government/request/) an.

>[!Note]
>Nachdem ein Plan in einer bestimmten Cloud als verfügbar veröffentlicht wurde, kann diese Cloud nicht mehr entfernt werden.

**Azure Government Cloud-Zertifizierungen**

Diese Option ist nur sichtbar, wenn **Azure Government Cloud** unter **Cloudverfügbarkeit** ausgewählt ist.

Azure Government-Dienste verarbeiten Daten, die bestimmten behördlichen Vorschriften und Anforderungen unterliegen, z.B. FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 und CJIS.  Um bei Ihren Zertifizierungen ein Bewusstsein für diese Programme zu schaffen, können Sie bis zu 100 Links bereitstellen, in denen Ihre Zertifizierungen beschrieben werden.  Diese Links können entweder direkt mit ihrem Listing im Programm verknüpft sein oder zu Beschreibungen der Konformität mit diesen Programmen auf Ihren eigenen Websites weiterleiten.  Die Links sind nur für Azure Government Cloud-Kunden sichtbar.

## <a name="plan-listing"></a>Planlisting

Die Registerkarte **Planlisting** zeigt die planspezifischen Listinginformationen an, die sich bei verschiedenen Plänen für dasselbe Angebot unterscheiden können.

### <a name="name"></a>NAME

Wird basierend auf dem Namen, den Sie Ihrem Plan bei der Erstellung zugewiesen haben, vorab aufgefüllt.  Dieser Name wird als Titel des „Softwareplans“ im Marketplace angezeigt.  Er darf bis zu 100 Zeichen enthalten.

### <a name="summary"></a>Zusammenfassung

Geben Sie eine kurze Zusammenfassung Ihres Softwareplans an.  Darf bis zu 100 Zeichen enthalten.

### <a name="description"></a>BESCHREIBUNG

Diese Beschreibung bietet die Möglichkeit zu erklären, was diesen Softwareplan einzigartig macht und von anderen Softwareplänen in Ihrem Angebot unterscheidet. Darf bis zu 2.000 Zeichen enthalten.

Klicken Sie auf **Speichern**, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

## <a name="availability"></a>Verfügbarkeit

Die Registerkarte **Verfügbarkeit** wird nur für Pläne vom Typ „Lösungsvorlage“ angezeigt.  Sie können den Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) sichtbar machen oder festlegen, dass der Plan ausgeblendet werden und nur für die Verwendung durch andere Lösungsvorlagen oder verwaltete Anwendungen verwendbar sein soll.

### <a name="plan-audience"></a>Planzielgruppe

Sie können festlegen, dass jeder Plan für alle Benutzer oder nur für eine von Ihnen ausgewählte Zielgruppe angezeigt wird. Sie können mithilfe von Azure Abonnement-IDs die Mitgliedschaft in dieser eingeschränkten Zielgruppe zuweisen.

**Datenschutz/Dies ist ein privater Plan** (optionales Kontrollkästchen): Aktivieren Sie dieses Kontrollkästchen, um den Plan als privaten Plan festzulegen und nur für die von Ihnen ausgewählte eingeschränkte Zielgruppe anzuzeigen. Nachdem Sie den Plan als privaten Plan veröffentlicht haben, können Sie die Zielgruppe aktualisieren oder den Plan für alle Benutzer verfügbar machen. Nachdem ein Plan als für alle Benutzer sichtbar veröffentlicht wurde, muss er für alle Benutzer sichtbar bleiben. (Der Plan kann nicht wieder als privater Plan konfiguriert werden).

**Eingeschränkte Zielgruppe (Azure-Abonnement-IDs)** : Weisen Sie die Zielgruppe zu, die auf diesen privaten Plan zugreifen darf. Der Zugriff wird mit Azure-Abonnement-IDs zugewiesen. Dabei kann eine Beschreibung jeder zugewiesenen Azure-Abonnement-ID angegeben werden. Es können maximal 10 Abonnement-IDs oder, wenn eine CSV-Tabellenkalkulationsdatei importiert wird, 20.000 Abonnement-IDs von Kunden hinzugefügt werden.  Azure-Abonnement-IDs werden als GUIDs dargestellt. Buchstaben müssen in Kleinbuchstaben angegeben werden.

>[!Note]
>Eine private Zielgruppe (oder eingeschränkte Zielgruppe) unterscheidet sich von der auf der Registerkarte [**Vorschau**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview) definierten Vorschauzielgruppe.  Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, *bevor* es im Marketplace live veröffentlicht wird. Die Festlegung als private Zielgruppe gilt nur für einen bestimmten Plan. Für die private Zielgruppe können alle Pläne (private und nicht private Pläne) zu Validierungszwecken angezeigt werden.

### <a name="hide-plan"></a>Plan ausblenden

Soll die Lösungsvorlage nur indirekt, also nur bei Verweisen über eine andere Lösungsvorlage oder eine verwaltete Anwendung bereitgestellt werden, aktivieren Sie das Kontrollkästchen, um Ihre Lösungsvorlage zu veröffentlichen aber für Kunden, die danach direkt suchen oder blättern, auszublenden.

## <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Die Registerkarte **Preise und Verfügbarkeit** wird nur für verwaltete Anwendungspläne angezeigt.  Sie können die Märkte, in denen dieser Plan verfügbar sein soll, und den Preis pro Monat für die Verwaltung der Lösung konfigurieren und festlegen, ob der Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) sichtbar gemacht werden soll.

### <a name="markets"></a>Märkte

Jeder Plan muss in mindestens einem Markt verfügbar sein. Aktivieren Sie das Kontrollkästchen für jeden Marktstandort, in dem Sie diesen Plan zur Verfügung stellen möchten. Zur Unterstützung sind ein Suchfeld und eine Schaltfläche zum Auswählen von Ländern vorhanden, in denen die Umsatz- und Gebrauchssteuer von Microsoft für Sie überwiesen wird.

Wenn Sie bereits Preise in US-Dollar für Ihren Plan festgelegt haben und einen anderen Marktstandort hinzufügen, wird der Preis für den neuen Markt entsprechend dem aktuellen Wechselkurs berechnet. Überprüfen Sie vor dem Veröffentlichen immer den Preis für jeden Markt. Die Preise können nach dem Speichern der Änderungen mithilfe des Links „Export prices (xlsx)“ (Preise exportieren (.xlsx)) überprüft werden.

### <a name="pricing"></a>Preise

Geben Sie den Preis pro Monat für diesen Plan an.  Dieser Preis gilt zusätzlich zu allen Azure-Infrastruktur- oder nutzungsbasierten Softwarekosten, die durch die von dieser Lösung bereitgestellten Ressourcen entstehen.

Preise in der lokalen Währung (USD = US-Dollar) werden während der Einrichtung gemäß dem aktuellen Wechselkurs in die lokale Währung aller ausgewählten Märkte konvertiert. Überprüfen Sie vor der Veröffentlichung die Preise in jedem Markt, indem Sie die Preistabellenkalkulation exportieren. Wenn Sie benutzerdefinierte Preise in einem einzelnen Markt festlegen möchten, ändern und importieren Sie die Preistabellenkalkulation. 

>[!Note]
>Sie müssen Ihre Preisänderungen speichern, damit die Preisdaten exportiert werden können.

Überprüfen Sie Ihre Preise vor der Veröffentlichung sorgfältig, da nicht alle Elemente eines Plans nach seiner Veröffentlichung geändert werden können.  

>[!Note]
>Nachdem ein Preis für einen Markt in Ihrem Plan veröffentlicht wurde, kann er nicht mehr geändert werden.

### <a name="plan-audience"></a>Plan Audience (Planzielgruppe)

Sie können festlegen, dass jeder Plan für alle Benutzer oder nur für eine von Ihnen ausgewählte Zielgruppe angezeigt wird. Sie können mithilfe von Azure Abonnement-IDs die Mitgliedschaft in dieser eingeschränkten Zielgruppe zuweisen.

**Datenschutz/Dies ist ein privater Plan** (optionales Kontrollkästchen): Aktivieren Sie dieses Kontrollkästchen, um den Plan als privaten Plan festzulegen und nur für die von Ihnen ausgewählte eingeschränkte Zielgruppe anzuzeigen. Nachdem Sie den Plan als privaten Plan veröffentlicht haben, können Sie die Zielgruppe aktualisieren oder den Plan für alle Benutzer verfügbar machen. Nachdem ein Plan als für alle Benutzer sichtbar veröffentlicht wurde, muss er für alle Benutzer sichtbar bleiben. (Der Plan kann nicht wieder als privater Plan konfiguriert werden).

**Eingeschränkte Zielgruppe (Azure-Abonnement-IDs)** : Weisen Sie die Zielgruppe zu, die auf diesen privaten Plan zugreifen darf. Der Zugriff wird mit Azure-Abonnement-IDs zugewiesen. Dabei kann eine Beschreibung jeder zugewiesenen Azure-Abonnement-ID angegeben werden. Es können maximal 10 Abonnement-IDs oder, wenn eine CSV-Tabellenkalkulationsdatei importiert wird, 20.000 Abonnement-IDs von Kunden hinzugefügt werden.  Azure-Abonnement-IDs werden als GUIDs dargestellt. Buchstaben müssen in Kleinbuchstaben angegeben werden.

>[!Note]
>Eine private Zielgruppe (oder eingeschränkte Zielgruppe) unterscheidet sich von der auf der Registerkarte [**Vorschau**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview) definierten Vorschauzielgruppe.  Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, *bevor* es im Marketplace live veröffentlicht wird. Die Festlegung als private Zielgruppe gilt nur für einen bestimmten Plan. Für die private Zielgruppe können alle Pläne (private und nicht private Pläne) zu Validierungszwecken angezeigt werden.

## <a name="technical-configuration"></a>Technische Konfiguration 

Über die Registerkarte **Technische Konfiguration** können Sie das Bereitstellungspaket hochladen, das Kunden die Bereitstellung Ihres Plans ermöglicht.

>[!Note]
>Diese Registerkarte wird nicht angezeigt, wenn Sie den Plan auf der Registerkarte **Plansetup** so konfiguriert haben, dass Pakete aus einem anderen Plan wiederverwendet werden.

### <a name="package-details"></a>Paketdetails

Auf der Unterregisterkarte **Paketdetails** können Sie die Entwurfsversion Ihrer technischen Konfiguration bearbeiten.

***Version***: Weisen Sie die aktuelle Version der technischen Konfiguration zu.  Erhöhen Sie diese Version jedes Mal, wenn Sie eine Änderung auf dieser Seite veröffentlichen. Die Version muss im Format `{integer}.{integer}.{integer}` angegeben werden.

***Paketdatei*** (`.zip`): Dieses Paket enthält alle für diesen Plan erforderlichen Vorlagendateien und alle zusätzlichen Ressourcen in einer `.zip`-Datei.

Alle Azure-Anwendungsplanpakete müssen diese zwei Dateien im Stammordner eines `.zip`-Archivs enthalten:

* Eine Resource Manager-Vorlagendatei mit dem Namen [mainTemplate.json](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).  Diese Vorlage automatisiert die Bereitstellung von Ressourcen im Azure-Abonnement des Kunden.  Beispiele für Resource Manager-Vorlagen finden Sie im [Katalog der Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) oder im entsprechenden Repository [GitHub: Azure Resource Manager Quickstart Templates](https://github.com/azure/azure-quickstart-templates) (Azure Resource Manager-Schnellstartvorlagen).

* Eine Benutzeroberflächendefinition für die Oberfläche zum Erstellen von Azure-Anwendungen mit dem Namen [createUiDefinition.json](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

Alle neuen Azure-Anwendungsangebote müssen auch eine [GUID zum Zuordnen der Nutzung durch Kunden von Azure-Partnern](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution) enthalten.

### <a name="previously-published-packages"></a>Zuvor veröffentlichte Pakete 

Auf der Unterregisterkarte**Zuvor veröffentlichte Pakete** können Sie alle veröffentlichten Versionen der technischen Konfiguration anzeigen.

## <a name="technical-configuration-managed-application-plans-only"></a>Technische Konfiguration (nur verwaltete Anwendungspläne)

Verwaltete Anwendungspläne enthalten auf der Registerkarte **Technische Konfiguration** neben den oben beschriebenen Feldern **Version** und **Paketdatei** zusätzliche Felder. 

### <a name="enable-just-in-time-jit-access"></a>Just-in-Time-Zugriff (JIT) aktivieren

Wählen Sie diese Option, um den Just-in-time-Zugriff (JIT) für diesen Plan zu aktivieren.  Der JIT-Zugriff ermöglicht es Ihnen, erhöhte Zugriffsrechte auf die Ressourcen einer verwalteten Anwendung zur Problembehandlung oder Wartung anzufordern. Sie verfügen immer über Lesezugriff auf die Ressourcen, aber für einen bestimmten Zeitraum können Sie über erhöhte Zugriffsrechte verfügen.  Weitere Informationen finden Sie unter [Aktivieren und Anfordern des Just-In-time-Zugriffs für Azure Managed Applications](https://docs.microsoft.com/azure/managed-applications/request-just-in-time-access).  Lassen Sie diese Option deaktiviert, um festzulegen, dass Ihnen von den Consumern Ihrer verwalteten Anwendung dauerhaft Zugriff gewährt werden muss.

>[!Note]
>Stellen Sie sicher, dass Sie Ihre `createUiDefinition.json`-Datei aktualisiert wurde, damit dieses Feature unterstützt wird.  

### <a name="deployment-mode"></a>Bereitstellungsmodus

Wählen Sie aus, ob Sie den **vollständigen** oder **inkrementellen Bereitstellungsmodus** bei der Bereitstellung dieses Plans konfigurieren möchten: 

* Im **vollständigen Modus** führt eine erneute Bereitstellung der Anwendung durch den Kunden zur Entfernung von Ressourcen aus der verwalteten Ressourcengruppe, wenn die Ressourcen nicht in der Datei `mainTemplate.json` definiert sind. 
* Im **inkrementellen Modus** bleiben bestehende Ressourcen bei einer erneuten Bereitstellung der Anwendung unverändert.

Weitere Informationen zu den Bereitstellungsmodi finden Sie unter [Azure Resource Manager-Bereitstellungsmodi](https://docs.microsoft.com/azure/azure-resource-manager/deployment-modes).

### <a name="notification-endpoint-url"></a>Benachrichtigungsendpunkt-URL

Geben Sie einen HTTPS-Webhookendpunkt an, um Benachrichtigungen zu allen CRUD-Vorgängen für verwaltete Anwendungsinstanzen dieser Planversion zu erhalten.

### <a name="customize-allowed-customer-actions"></a>Zulässige Kundenaktionen anpassen

Wählen Sie diese Option aus, um festzulegen, welche Aktionen Kunden neben den standardmäßig verfügbaren „`*/read`“-Aktionen für verwaltete Ressourcen ausführen können. 

Listen Sie hier die zusätzlichen Aktionen auf, durch Semikolons getrennt, die Sie Ihren Kunden ermöglichen möchten.  Weitere Informationen finden Sie unter Weitere Informationen finden Sie unter [Grundlegendes zu Ablehnungszuweisungen für Azure-Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments).  Die verfügbaren Aktionen finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations). Beispiel: Wenn Sie Kunden das Neustarten virtueller Computer erlauben möchten, fügen Sie den zulässigen Aktionen `Microsoft.Compute/virtualMachines/restart/action` hinzu.

### <a name="global-azure--azure-government-cloud"></a>Global Azure / Azure Government Cloud

Geben Sie an, wer in jeder unterstützten Cloud über Verwaltungszugriff auf diese verwaltete Anwendung verfügen soll.  Benutzer, Gruppen oder Anwendungen, denen Berechtigungen für die verwaltete Ressourcengruppe erteilt werden sollen, werden mithilfe von Azure Active Directory-Identitäten (AAD) identifiziert.

***Azure Active Directory-Mandanten-ID***: Die AAD-Mandanten-ID (auch als Verzeichnis-ID bezeichnet), die die Identitäten der Benutzer, Gruppen oder Anwendungen enthält, denen Sie Berechtigungen erteilen möchten.  Sie finden Ihre AAD-Mandanten-ID im Azure-Portal unter [Eigenschaften für Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties).

***Autorisierungen***: Fügen Sie die Azure Active Directory-Objekt-ID des Benutzers, der Gruppe oder der Anwendung hinzu, dem oder der die Berechtigung für die verwaltete Ressourcengruppe erteilt werden soll. Sie können den Benutzer anhand seiner Prinzipal-ID auf dem [Blatt „Azure Active Directory Benutzer“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) identifizieren.

Wählen Sie für jeden Prinzipal eine der in Azure AD integrierten Rollen aus der Liste aus (Besitzer oder Mitwirkender). Die von Ihnen ausgewählte Rolle beschreibt die Berechtigungen, die der Prinzipal für die Ressourcen im Kundenabonnement erhält. Weitere Informationen finden Sie unter [Integrierte Rollen für Azure-Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).  Weitere Informationen zur rollenbasierten Zugriffssteuerung (RBAC) finden Sie unter [Erste Schritte mit RBAC im Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/overview).

>[!Note]
>Obwohl Sie bis zu 100 Autorisierungen pro Cloud hinzufügen können, ist es im Allgemeinen einfacher, eine Active Directory-Benutzergruppe zu erstellen und ihre ID in der „Prinzipal-ID“ anzugeben.  Dies ermöglicht Ihnen, nach der Bereitstellung des Plans weitere Benutzer zur Verwaltungsgruppe hinzuzufügen und verringern die Notwendigkeit, den Plan zu aktualisieren, nur um weitere Autorisierungen hinzuzufügen.

### <a name="policy-settings"></a>Richtlinieneinstellungen

Wenden Sie [Azure-Richtlinien](https://docs.microsoft.com/azure/governance/policy/overview) auf Ihre verwaltete Anwendung an, um Complianceanforderungen für die bereitgestellte Lösung anzugeben.  Informationen zu Richtliniendefinitionen und zum Format der Parameterwerte finden Sie unter [Azure Policy-Beispiele](https://docs.microsoft.com/azure/governance/policy/samples/index).  Sie können maximal fünf Richtlinien und nur eine Instanz jeder einzelnen Richtlinienoption konfigurieren.  Einige Richtlinien erfordern zusätzliche Parameter.  Der Standard-SKU ist für Überwachungsrichtlinien erforderlich.  Richtlinienname darf maximal 50 Zeichen lang sein.

## <a name="co-sell"></a>Co-Selling

Die Bereitstellung von Informationen auf der Registerkarte „Co-Selling“ ist für die Veröffentlichung Ihres Angebots völlig optional. Es ist erforderlich, um den Status „Bereit für Co-Selling“ (Co-sell Ready) und „Bereit für IP-Co-Selling“ (IP Co-sell Ready) zu erreichen. Die von Ihnen angegebenen Informationen werden von den Microsoft-Vertriebsteams verwendet, um mehr über Ihre Lösung zu erfahren, wenn sie deren Eignung für die Anforderungen Ihrer Kunden auswerten. Sie sind nicht direkt für Kunden verfügbar.

Weitere Informationen zum Ausfüllen dieser Registerkarte finden Sie unter [Co-Selling-Option im Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-co-sell).

## <a name="test-drive"></a>Testversion

Auf der Registerkarte **Testversion** können Sie eine Demonstration (oder „Testversion“) einrichten, sodass Kunden Ihr Angebot testen können, bevor sie sich zum Kauf entschließen. Erfahren Sie mehr im Artikel [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)  Wenn Sie keine Testversion für Ihr Angebot mehr bereitstellen möchten, kehren Sie zur Seite **Angebotseinrichtung** zurück, und deaktivieren Sie **Testversion aktivieren**.

### <a name="technical-configuration"></a>Technische Konfiguration

Azure-Anwendungen verwenden von Natur aus den Typ „Azure Resource Manager-Testversion“.  Weitere Informationen finden Sie unter [Technische Konfiguration für die Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-customer-engagement-offer#technical-configuration-for-azure-resource-manager-test-drive).

### <a name="deployment-subscription-details"></a>Abonnementdetails für Bereitstellung der Testversion

Um die Testversion in Ihrem Auftrag bereitzustellen, erstellen Sie ein eigenes eindeutiges Azure-Abonnement, und stellen Sie es bereit. (Power BI-Testversionen nicht erforderlich).

- **Azure-Abonnement-ID** (erforderlich für Azure Resource Manager und Logik-Apps): Geben Sie die Abonnement-ID ein, um für Ressourcennutzungsberichte und Abrechnungszwecke Zugriff auf die Dienste Ihres Azure-Kontos zu gewähren. Es wird empfohlen, [ein eigenes Azure-Abonnement zu erstellen](https://docs.microsoft.com/azure/billing/billing-create-subscription), das für Testversionen verwendet wird, falls Sie es noch nicht getan haben. Sie finden die Azure-Abonnement-IDs, indem Sie sich beim [Azure-Portal](https://portal.azure.com/) anmelden und im Menü auf der linken Seite zu den **Abonnements** navigieren. Durch Auswahl der Registerkarte wird Ihre Abonnement-ID angezeigt (z.B. a83645ac-1234-5ab6-6789-1h234g764ghty).

- **ID des Azure AD-Mandanten** (erforderlich): Geben Sie Ihre [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) von Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **Eigenschaften, und suchen Sie dann die aufgeführte Nummer der **Verzeichnis-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe einer Domänennamen-URL nachschlagen.

- **Name des Azure AD-Mandanten** (erforderlich für Dynamics 365): Geben Sie den Namen Ihres Azure Active Directory (AD) ein. Um diesen Namen zu suchen, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Oben rechts wird der Mandantenname unter Ihrem Kontonamen angegeben.

- **Azure AD-App-ID** (erforderlich): Geben Sie die [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) von Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **App-Registrierungen**, und suchen Sie dann die aufgeführte Nummer der **Anwendungs-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Geheimer Azure AD-App-Clientschlüssel** (erforderlich): Geben Sie Ihren [geheimen Clientschlüssel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) für die Azure AD-Anwendung ein. Um diesen Wert zu finden, melden Sie sich beim [Azur-Portal](https://portal.azure.com/) an. Wählen Sie im linken Menü die Registerkarte **Azure Active Directory** aus, wählen Sie **App-Registrierungen** aus und dann Ihre Testversion-App. Wählen Sie als Nächstes **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, geben Sie eine Beschreibung ein, wählen Sie **Nie** unter **Läuft ab** aus, und wählen Sie dann **Hinzufügen** aus. Stellen Sie sicher, dass Sie den Wert kopieren. (Verlassen Sie die Seite nicht, bevor Sie den Wert kopiert haben, sonst haben Sie keinen Zugriff auf den Wert.)

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

### <a name="test-drive-listings-optional"></a>Test drive listings (Testversionslisten) (optional)

Auf der Registerkarte **Test drive listings** (Testversionslisten) unter **Testversion** werden die Sprachen (und Märkte) angezeigt, in denen Ihre Testversion verfügbar ist. Derzeit ist nur der Standort „Englisch (USA)“ verfügbar. Auf dieser Seite werden zudem der Status der sprachspezifischen Liste und das Datum/die Uhrzeit der Hinzufügung angezeigt. Sie müssen für jede Sprache/jeden Markt die Testversionsdetails (Beschreibung, Benutzerhandbuch, Videos usw.) definieren.

- **Beschreibung:** (erforderlich): Beschreiben Sie Ihre Testversion. Geben Sie an, was vorgeführt wird, nennen Sie die Ziele, mit denen der Benutzer experimentieren soll, sowie die zu erkundenden Funktionen, und geben Sie relevante Informationen an, die dem Benutzer helfen zu entscheiden, ob er Ihr Angebot erwerben soll. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 

- **Zugriffsinformationen** (für Azure Resource Manager und Logik-Testversionen erforderlich): Erläutern Sie, was ein Kunde wissen muss, um auf die Testversion zuzugreifen und sie zu verwenden. Beschreiben Sie ein Szenario für die Verwendung Ihres Angebots, und teilen Sie genau mit, was der Kunde wissen sollte, um auf die Funktionen in der gesamten Testversion zuzugreifen. In diesem Feld können bis zu 10.000 Zeichen Text eingegeben werden.

- **Benutzerhandbuch** (erforderlich): Eine ausführliche Beschreibung der Verwendung Ihrer Testversion. Das Benutzerhandbuch sollte genau beschreiben, was der Kunde durch die Erkundung der Testversion erlangen soll, und als Referenz für die Fragen des Kunden dienen. Die Datei muss das PDF-Format aufweisen und nach dem Hochladen mit einem Namen (max. 255 Zeichen) versehen werden.

- **Videos: Videos hinzufügen** (optional): Videos können auf YouTube oder Vimeo hochgeladen werden, und hier kann mit einem Link und einer Miniaturansicht (533 × 324 Pixel) auf sie verwiesen werden. So erhalten Kunden Informationen zu einzelnen Schritten, um sich besser mit der Testversion vertraut zu machen. Unter anderem können Sie ihnen zeigen, wie sie die Funktionen Ihres Angebots erfolgreich verwenden, und Szenarien erläutern, in denen die Vorteile hervorgehoben werden.
  - **Name** (erforderlich)
  - **URL (nur YouTube oder Vimeo)** (erforderlich)
  - **Miniaturansicht (533 × 324 px):** Die Bilddatei muss im PNG-Format vorliegen.

Klicken Sie auf **Speichern**, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

## <a name="publish"></a>Veröffentlichen

### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Nachdem Sie alle erforderlichen Abschnitte des Angebots abgeschlossen haben, klicken Sie rechts oben im Portal auf **Veröffentlichen**. Sie werden zur Seite **Review and publish** (Überprüfen und veröffentlichen) weitergeleitet. 

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
    - *Nicht gestartet:* Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
    - *Unvollständig:* Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu dem/den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - *Abgeschlossen:* Der Abschnitt ist abgeschlossen. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Kehren Sie zu Partner Center zurück, und wählen Sie **Live schalten** aus, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

### <a name="errors-and-review-feedback"></a>Fehler und Prüfungsfeedback

Der Schritt **Manuelle Validierung** im Veröffentlichungsvorgang stellt eine umfassende Überprüfung Ihres Angebots und der damit verbundenen technischen Ressourcen (insbesondere der Azure Resource Manager-Vorlage) dar, und Probleme werden in der Regel als Pull Request-Links dargestellt. Eine Erklärung, wie Sie diese Pull Requests anzeigen und auf sie reagieren, finden Sie unter [Verarbeiten von Prüfungsfeedback](./azure-apps-review-feedback.md).

Wenn Sie in einem oder mehreren der Veröffentlichungsschritte Fehler festgestellt haben, müssen Sie diese korrigieren und Ihr Angebot erneut veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
