---
title: Erstellen eines neuen Angebots für verwaltete Dienste im kommerziellen Marketplace
description: Erstellen eines neuen Angebots für verwaltete Dienste, um das Angebot über das Portal des kommerziellen Marketplace im Partner Center in Azure Marketplace aufzulisten
author: JnHs
ms.author: jenhayes
manager: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: 6baf5d77b70fa6e2717b492163826f27d80fbb88
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991619"
---
# <a name="create-a-new-managed-service-offer"></a>Erstellen eines neuen Angebots für verwaltete Dienste

> [!IMPORTANT]
> Wir verlagern die Verwaltung Ihrer Angebote für verwaltete Dienste aus dem Cloud-Partnerportal in Partner Center. Folgen Sie den Anweisungen unter [Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace](../../lighthouse/how-to/publish-managed-services-offers.md), um Ihre Angebote im Cloud-Partnerportal zu verwalten, bis Ihre Angebote migriert wurden.

Angebote für verwaltete Dienste unterstützen [Azure Lighthouse](../../lighthouse/overview.md)-Szenarien. Wenn ein Kunde ein Angebot für verwaltete Dienste annimmt, kann er Ressourcen für die [delegierte Azure-Ressourcenverwaltung](../../lighthouse/concepts/azure-delegated-resource-management.md) integrieren.

Um mit dem Erstellen von Angeboten für verwaltete Dienste zu beginnen, müssen Sie zunächst [ein Partner Center-Konto erstellen](./create-account.md), das [Dashboard „Kommerzieller Marketplace“](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) öffnen und darin die Seite **Übersicht** auswählen. Sie müssen über eine [Silver oder Gold Cloud Platform-Kompetenzebene](https://partner.microsoft.com/membership/cloud-platform-competency) verfügen oder ein [Azure Expert MSP](https://partner.microsoft.com/membership/azure-expert-msp) sein, um ein Angebot für verwaltete Dienste zu veröffentlichen.

![Dashboard „Kommerzieller Marketplace“ im Partner Center](./media/new-offer-overview.png)

>[!Note]
> Nach der Veröffentlichung eines Angebots werden in Partner Center am Angebot vorgenommene Änderungen erst nach einer erneuten Veröffentlichung im System und den Store Fronts aktualisiert. Sie müssen das Angebot nach dem Vornehmen von Änderungen für die Veröffentlichung einreichen.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

Wählen Sie die Schaltfläche **+ Neues Angebot** und dann das Menüelement **Verwalteter Dienst** aus. Das Dialogfeld **Neues Angebot** wird angezeigt.

### <a name="offer-id-and-alias"></a>Angebots-ID und Angebotsalias

- **Angebots-ID:** Eindeutiger Bezeichner für jedes Angebot in Ihrem Konto. Diese ID ist für Kunden in der URL-Adresse des Marketplace-Angebots sichtbar. Die ID darf nur alphanumerische Zeichen ohne Großbuchstaben enthalten (Bindestriche und Unterstriche sind zulässig, Leerzeichen jedoch nicht). Sie ist auf 50 Zeichen beschränkt und kann nach dem Auswählen von **Erstellen** nicht mehr geändert werden.  Wenn Sie hier z.B. *test-offer-1* eingeben, lautet die Angebots-URL `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Angebotsalias**: Der Name, der zum Verweisen auf das Angebot im Partner Center verwendet wird. Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden. Dieser Wert kann nach der Auswahl von **Erstellen** nicht mehr geändert werden.

Nachdem Sie Ihre **Angebots-ID** und ihren **Angebotsalias** eingegeben haben, wählen Sie **Erstellen** aus. Sie können dann alle anderen Teile Ihres Angebots bearbeiten.

## <a name="offer-setup"></a>Angebotseinrichtung

Auf der Seite **Angebotseinrichtung** wird um die folgenden Informationen gebeten. Vergessen Sie nicht, **Speichern** auszuwählen, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

## <a name="connect-lead-management"></a>Einbinden der Leadverwaltung

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Beachten Sie, dass gemäß den [Zertifizierungsrichtlinien für verwaltete Dienste](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services) eine **Leadzielgruppe** erforderlich ist. Dadurch wird jedes Mal, wenn ein Kunde Ihr Angebot bereitstellt, ein Eintrag in Ihrem CRM-System erstellt.

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Sie sollten die Seite **speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="properties"></a>Eigenschaften

Auf der Seite **Eigenschaften** können Sie die Kategorien, die zum Gruppieren Ihres Angebots im Marketplace verwendet werden, und die Verträge definieren, die Ihr Angebot unterstützen. Wählen Sie **Speichern** aus, nachdem Sie diese Seite ausgefüllt haben.

### <a name="category"></a>Category

Wählen Sie mindestens eine und höchstens fünf Kategorien aus, die zum Platzieren Ihres Angebots in den entsprechenden Marketplace-Suchbereichen verwendet werden. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, wie Ihr Angebot diesen Kategorien entspricht.

### <a name="terms-and-conditions"></a>Geschäftsbedingungen

Geben Sie im Feld **Geschäftsbedingungen** Ihre eigenen rechtlichen Bestimmungen an. Sie können auch die URL angeben, unter der Ihre Geschäftsbedingungen zu finden sind. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihr Angebot testen können.

## <a name="offer-listing"></a>Angebotsliste

Auf der Seite **Angebotsliste** können Sie Marketplace-Informationen (Angebotsname, Beschreibung, Bilder usw.) für Ihr Angebot definieren.

> [!NOTE]
> Die Inhalte der Angebotsliste (z. B. Beschreibung, Dokumente, Screenshots, Nutzungsbedingungen usw.) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur auf [nicht englische Sprache] verfügbar.“. Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="name"></a>Name

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text aufgefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

### <a name="search-results-summary"></a>Zusammenfassung der Suchergebnisse

Geben Sie eine kurze Beschreibung Ihres Angebots ein (bis zu 100 Zeichen), die in den Marketplace-Suchergebnissen verwendet werden kann.

### <a name="long-summary"></a>Lange Zusammenfassung

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 256 Zeichen). Diese lange Zusammenfassung kann auch in Marketplace-Suchergebnissen verwendet werden.

### <a name="description"></a>BESCHREIBUNG

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 3.000 Zeichen). Diese Beschreibung wird Kunden in der Übersicht der Marketplace-Auflistung angezeigt. Geben Sie z.B ein Wertversprechen für Ihr Angebot, wichtige Vorteile, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe sowie alle erforderlichen Veröffentlichungen ein.

Einige Tipps zum Verfassen Ihrer Beschreibung:  

- Schildern Sie in den ersten Sätzen Ihrer Beschreibung deutlich das Nutzenversprechen Ihres Angebots. Fügen Sie folgende Elemente in Ihr Wertversprechen ein:
  - Beschreibung des Angebots
  - Benutzertyp, der von dem Produkt profitiert
  - Kundenbedürfnisse oder -probleme, an die sich das Angebot richtet
- Beachten Sie, dass die ersten Sätze in Suchmaschinenergebnissen angezeigt werden können.  
- Setzen Sie sich nicht nur auf Features und Funktionen, um Ihr Produkt zu verkaufen. Konzentrieren Sie stattdessen auf den Nutzen, den Sie bieten.  
- Verwenden Sie möglichst viel branchenspezifisches Vokabular oder eine nutzenorientierte Formulierung.

Um Ihre Angebotsbeschreibung ansprechender zu gestalten, formatieren Sie die Beschreibung mithilfe des Rich-Text-Editors.

![Verwenden des Rich-Text-Editors](./media/text-editor2.png)

Gehen Sie bei der Verwendung des Rich-Text-Editors folgendermaßen vor:

- Um das Format der Inhalte zu ändern, markieren Sie den zu formatierenden Text und wählen ein Textformat aus, wie unten dargestellt:

     ![Verwenden des Rich-Text-Editors zum Ändern des Textformats](./media/text-editor3.png)

- Um dem Text eine nummerierte Liste oder eine Liste mit Aufzählungszeichen hinzuzufügen, verwenden Sie die folgenden Optionen:

     ![Verwenden des Rich-Text-Editors zum Hinzufügen von Listen](./media/text-editor4.png)

- Verwenden Sie zum Hinzufügen oder Entfernen von Texteinzügen die folgenden Optionen:

     ![Verwenden des Rich-Text-Editors für Einzüge](./media/text-editor5.png)

### <a name="privacy-policy-link"></a>Link zur Datenschutzrichtlinie

Geben Sie die URL zu der (an Ihrem Standort gehosteten) Datenschutzrichtlinie Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="useful-links"></a>Nützliche Links

Stellen Sie optionale Onlinedokumente zu Ihrer Lösung bereit. Fügen Sie weitere nützliche Links hinzu, indem Sie auf **+ Link hinzufügen** klicken.

### <a name="contact-information"></a>Kontaktinformationen

In diesem Abschnitt müssen Sie den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Supportkontakt** und einen **Engineering-Kontakt** angeben. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und können CSP-Partnern bereitgestellt werden.

### <a name="support-urls"></a>Support-URLs

Wenn Sie Supportwebsites für **weltweite Azure-Kunden** und/oder **Azure Government-Kunden** anbieten, geben Sie deren URLs hier an.

### <a name="marketplace-images"></a>Marketplace-Bilder

In diesem Abschnitt können Sie Logos und Bilder bereitstellen, die beim Anzeigen Ihres Angebots für Kunden verwendet werden. Alle Bilder müssen das PNG-Format aufweisen.

#### <a name="marketplace-logos"></a>Marketplace-Logos

Vier Logogrößen sind erforderlich: **Klein (40 x 40)** , **Mittel (90 x 90)** , **Groß (115 x 115)** und **Breit (255 x 115)** . Befolgen Sie die folgenden Richtlinien für Ihre Logos:

- Die Farbpalette des Azure-Designs ist einfach und geradlinig. Beschränken Sie die Anzahl der Primär- und Sekundärfarben auf Ihrem Logo.
- Die Designfarben des Azure-Portals sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihr Logo. Verwenden Sie eine Farbe, die Ihr Logo im Portal ideal zur Geltung bringt. Sie sollten einfache Primärfarben verwenden.
- Wenn Sie einen transparenten Hintergrund verwenden, stellen Sie sicher, dass das Logo und der Text nicht weiß, schwarz oder blau sind.
- Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein. Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.
- Platzieren Sie keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen.
- Stellen Sie sicher, dass das Logo nicht verzerrt wird.

#### <a name="screenshots"></a>Screenshots

Fügen Sie bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Alle Screenshots müssen in der Größe 1280 x 720 Pixel vorliegen.

#### <a name="videos"></a>Videos

Optional können Sie bis zu fünf Videos hinzufügen, die Ihr Angebot veranschaulichen. Diese Videos sollten auf YouTube und/oder Vimeo gehostet werden. Geben Sie für jedes Video den Namen, seine URL und ein Miniaturbild (1280 x 720 Pixel) ein.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Vorschau

Bevor Sie Ihr Angebot im Marketplace für die breite Öffentlichkeit live schalten, müssen Sie es zunächst für eine begrenzte Vorschauzielgruppe verfügbar machen. Auf diese Weise können Sie ermitteln, wie Ihr Angebot im Azure Marketplace dargestellt wird, bevor Sie es Kunden zur Verfügung stellen. Die Support- und Entwicklerteams von Microsoft können Ihr Angebot während dieses Vorschauzeitraums ebenfalls sehen.

Sie können die Vorschauzielgruppe definieren, indem Sie im Abschnitt **Vorschauzielgruppe** Azure-Abonnement-IDs eingeben. Sie können bis zu 10 Abonnement-IDs manuell eingeben oder eine CSV-Datei mit bis zu 100 Abonnement-IDs hochladen.

Alle Kunden, die mit diesen Abonnements verknüpft sind, können das Angebot im Azure Marketplace sehen, bevor es live geschaltet wird. Achten Sie darauf, hier Ihre eigenen Abonnements anzugeben, damit Sie das Angebot in der Vorschau betrachten können.

## <a name="plan-overview"></a>Planübersicht

Jedes Angebot muss über einen oder mehrere Pläne (manchmal auch als SKUs bezeichnet) verfügen. Sie können mehrere Pläne hinzufügen, um verschiedene Featuregruppen zu unterschiedlichen Preisen zu unterstützen oder um einen bestimmten Plan für eine begrenzte Zielgruppe spezifischer Kunden anzupassen. Kunden können die Pläne, die Ihnen zur Verfügung stehen, unter dem übergeordneten Angebot anzeigen.

Wählen Sie auf der Seite **Planübersicht** die Option **+ Neuen Plan erstellen** aus. Geben Sie dann eine **Plan-ID** und einen **Plannamen** ein. Beide Werte dürfen nur kleingeschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und höchstens 50 Zeichen lang sein. Diese Werte können für Kunden sichtbar sein und nach der Veröffentlichung des Angebots nicht mehr geändert werden.

Nachdem Sie diese Werte eingegeben haben, wählen Sie **Erstellen** aus, um mit dem Plan fortzufahren. Sie müssen drei Abschnitte bearbeiten: **Planlisting**, **Preise und Verfügbarkeit** und **Technische Konfiguration**.

### <a name="plan-listing"></a>Planlisting

Geben Sie zunächst eine **Zusammenfassung der Suchergebnisse** für den Plan an. Dies ist eine kurze Beschreibung Ihres Plans (bis zu 100 Zeichen), die in den Marketplace-Suchergebnissen verwendet werden kann.

Geben Sie als Nächstes eine **Beschreibung** ein, in der der Plan ausführlicher erläutert wird.

### <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Zurzeit kann für Angebote für verwaltete Dienste nur ein Preismodell verwendet werden: **BYOL** (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz). Dies bedeutet, dass Sie Ihren Kunden Kosten im Zusammenhang mit diesem Angebot direkt berechnen, und dass Microsoft Ihnen keine Gebühren berechnet.

Im Abschnitt zur **Sichtbarkeit des Plans** können Sie angeben, ob dieser Plan [privat](../../marketplace/private-offers.md) sein soll. Wenn Sie das Kontrollkästchen **Dies ist ein privater Plan** deaktiviert lassen, wird Ihr Plan nicht auf bestimmte Kunden (oder auf eine bestimmte Anzahl von Kunden) beschränkt.

Um diesen Plan nur für bestimmte Kunden verfügbar zu machen, wählen Sie **Ja** aus. Wenn Sie dies tun, müssen Sie die Kunden identifizieren, indem Sie ihre Abonnement-IDs angeben. Sie können diese einzeln (für bis zu 10 Abonnements) eingeben oder eine CSV-Datei (planübergreifend für bis zu 10.000 Abonnements) hochladen. Stellen Sie sicher, dass Sie hier Ihre eigenen Abonnements einschließen, damit Sie das Angebot testen und validieren können.

> [!IMPORTANT]
> Nachdem ein Plan als öffentlich veröffentlicht wurde, kann er nicht mehr in privat geändert werden. Um zu steuern, welche Kunden Ihr Angebot annehmen und Ressourcen delegieren können, verwenden Sie einen privaten Plan. Mit einem öffentlichen Plan können Sie die Verfügbarkeit nicht auf bestimmte Kunden oder auf eine bestimmte Anzahl von Kunden beschränken, obgleich Sie den Verkauf des Plans vollständig beenden können, wenn Sie sich dafür entscheiden. Sie können den [Zugriff auf eine Delegierung nur entfernen](../../lighthouse/how-to/onboard-customer.md#remove-access-to-a-delegation), nachdem ein Kunde ein Angebot akzeptiert hat, wenn Sie beim Veröffentlichen des Angebots eine **Autorisierung** mit der auf [Rolle zum Löschen der Registrierungszuweisung für verwaltete Dienste](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) festgelegten **Rollendefinition** eingeschlossen haben. Sie können sich auch an den Kunden wenden und diesen bitten, [Ihren Zugriff zu entfernen](../../lighthouse/how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

### <a name="technical-configuration"></a>Technische Konfiguration

In diesem Abschnitt Ihres Plans wird ein Manifest mit Autorisierungsinformationen zum Verwalten von Kundenressourcen erstellt. Diese Informationen werden benötigt, um die [delegierte Azure-Ressourcenverwaltung](../../lighthouse/concepts/azure-delegated-resource-management.md) zu aktivieren.

Unter [Mandanten, Rollen und Benutzer in Azure Lighthouse-Szenarien](../../lighthouse/concepts/tenants-users-roles.md#best-practices-for-defining-users-and-roles) erfahren Sie, welche Rollen unterstützt werden und welche bewährten Methoden für das Definieren von Autorisierungen gelten.

> [!NOTE]
> Wie oben erwähnt, werden die Benutzer und Rollen in Ihren **Autorisierungseinträgen** auf jeden Kunden angewendet, der den Plan erwirbt. Wenn Sie den Zugriff auf einen bestimmten Kunden eingrenzen möchten, müssen Sie einen privaten Plan für dessen ausschließliche Nutzung veröffentlichen.

#### <a name="manifest"></a>Manifest

Geben Sie als Erstes eine **Version** für das Manifest an. Verwenden Sie das Format *n.n.n* (z. B. 1.2.5).

Geben Sie als Nächstes Ihre **Mandanten-ID** an. Dabei handelt es sich um eine GUID, die der Azure Active Directory (Azure AD)-Mandanten-ID Ihrer Organisation zugeordnet ist. Dies ist der Verwaltungsmandant, über den Sie auf Ressourcen Ihrer Kunden zugreifen. Wenn Sie diese nicht zur Hand haben, können Sie sie anzeigen, indem Sie rechts oben im Azure-Portal auf Ihren Kontonamen zeigen, oder indem Sie **Verzeichnis wechseln** auswählen.

Wenn Sie eine neue Version Ihres Angebots veröffentlichen und ein aktualisiertes Manifest erstellen müssen, wählen Sie **+ Neues Manifest** aus. Achten Sie darauf, dass die Versionsnummer einen höheren Wert als die vorherige Manifestversion aufweist.

#### <a name="authorization"></a>Authorization

Durch Autorisierungen werden in Ihrem Verwaltungsmandanten die Entitäten definiert, die auf Ressourcen und Abonnements für Kunden zugreifen können, die den Plan kaufen. Jeder dieser Entitäten wird eine integrierte Rolle zugewiesen, mit der bestimmte Zugriffsebenen gewährt werden.

Sie können für jeden Plan bis zu zwanzig Autorisierungen erstellen.

> [!TIP]
> In den meisten Fällen sollten Sie Rollen einer Azure AD-Benutzergruppe oder einem -Dienstprinzipal zuweisen, anstatt einer Reihe einzelner Benutzerkonten. Auf diese Weise können Sie den Zugriff für einzelne Benutzer hinzufügen oder entfernen, ohne den Plan aktualisieren und erneut veröffentlichen zu müssen, wenn sich ihre Zugriffsanforderungen ändern. Wenn Sie Azure AD-Gruppen Rollen zuweisen, [sollten Sie darauf achten, dass der **Gruppentyp** auf **Sicherheit** und nicht auf **Office 365**](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) festgelegt ist. Zusätzliche Empfehlungen finden Sie unter [Mandanten, Rollen und Benutzer in Azure Lighthouse-Szenarien](../../lighthouse/concepts/tenants-users-roles.md).

Geben Sie für jede **Autorisierung** die folgenden Informationen an. Sie können dann so oft wie nötig **+ Autorisierung hinzufügen** auswählen, um weitere Benutzer und Rollendefinitionen hinzuzufügen.

- **Azure AD-Objekt-ID**: Der Azure AD-Bezeichner eines Benutzers, einer Benutzergruppe oder Anwendung, denen bestimmte Berechtigungen (wie in der Rollendefinition festgelegt) für die Ressourcen Ihrer Kunden gewährt werden.
- **Azure AD-Objektanzeigename**: Ein Anzeigename, der dem Kunden helfen soll, den Zweck dieser Autorisierung zu verstehen. Dem Kunde wird dieser Name beim Delegieren von Ressourcen angezeigt.
- **Rollendefinition:** Wählen Sie eine der verfügbaren integrierten Azure AD-Rollen aus der Liste aus. Diese Rolle legt die Berechtigungen fest, über die der Benutzer im Feld **Azure AD-Objekt-ID** für Ressourcen Ihrer Kunden verfügt. Beschreibungen dieser Rollen finden Sie unter [Integrierte Rollen](../../role-based-access-control/built-in-roles.md) und [Rollenunterstützung für die delegierte Azure-Ressourcenverwaltung](../../lighthouse/concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management).
  > [!NOTE]
  > Wenn Azure geeignete neue integrierte Rollen hinzugefügt werden, sind diese hier verfügbar. Es kann jedoch zu Verzögerungen kommen, bevor sie angezeigt werden.
- **Zuweisbare Rollen**: Diese Option wird nur angezeigt, wenn Sie in der **Rollendefinition** für diese Autorisierung „Benutzerzugriffsadministrator“ ausgewählt haben. Wenn dies der Fall ist, müssen Sie hier mindestens eine zuweisbare Rollen hinzufügen. Der Benutzer im Feld **Objekt-ID von Azure AD** kann diese Rollen [verwalteten Identitäten](../../active-directory/managed-identities-azure-resources/overview.md) zuweisen. Dies ist erforderlich, um [Richtlinien bereitzustellen, die korrigiert werden können](../../lighthouse/how-to/deploy-policy-remediation.md). Beachten Sie, dass für diesen Benutzer keine anderen Berechtigungen gelten, die normalerweise der Rolle „Benutzerzugriffsadministrator“ zugeordnet sind.

> [!TIP]
> Um sicherzustellen, dass Sie den [Zugriff auf eine Delegierung im Bedarfsfall entfernen](../../lighthouse/how-to/onboard-customer.md#remove-access-to-a-delegation) können, schließen Sie eine **Autorisierung** mit der auf [Rolle zum Löschen der Registrierungszuweisung für verwaltete Dienste](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) festgelegten **Rollendefinition** ein. Wenn diese Rolle nicht zugewiesen wird, können delegierte Ressourcen nur durch einen Benutzer im Kundenmandanten entfernt werden.

Nachdem Sie alle Abschnitte für Ihren Plan bearbeitet haben, können Sie so häufig wie nötig **+ Neuen Plan erstellen** auswählen, um zusätzliche Pläne zu erstellen. Klicken Sie auf **Speichern**, wenn Sie fertig sind.

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

### <a name="customer-experience-and-offer-management"></a>Benutzerfreundlichkeit und Angebotsverwaltung

Wenn ein Kunde Ihr Angebot bereitstellt, kann er Abonnements oder Ressourcengruppen für die [delegierte Azure-Ressourcenverwaltung](../../lighthouse/concepts/azure-delegated-resource-management.md) delegieren. Weitere Informationen zu diesem Verfahren finden Sie unter [Kundenonboarding](../../lighthouse/how-to/publish-managed-services-offers.md#the-customer-onboarding-process).

Sie können jederzeit [eine aktualisierte Version Ihres Angebots veröffentlichen](update-existing-offer.md). Beispielsweise können Sie einem zuvor veröffentlichten Angebot eine neue Rollendefinition hinzufügen. Kunden, die das Angebot bereits hinzugefügt haben, wird dann auf der Seite [**Dienstanbieter**](../../lighthouse/how-to/view-manage-service-providers.md) im Azure-Portal ein Symbol angezeigt, das darauf hinweist, dass eine Aktualisierung verfügbar ist. Jeder Kunde kann die Änderungen überprüfen und entscheiden, ob er auf die neue Version aktualisieren möchte.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
- [Informationen zu Azure Lighthouse](../../lighthouse/overview.md)