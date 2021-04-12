---
Titel: Azure Media Services v3 overview : Azure Media Services description: Eine allgemeine Übersicht über Azure Media Services v3 mit Links zu Schnellstarts, Tutorials und Codebeispielen.
services: media-services documentationcenter: na author: IngridAtMicrosoft manager: femila editor: '' tags: '' keywords: azure media services, stream, broadcast, live, offline

ms.service: media-services ms.devlang: multiple ms.topic: overview ms.tgt_pltfrm: multiple ms.workload: media ms.date: 3/10/2021 ms.author: inhenkel ms.custom: mvc
#<a name="customer-intent-as-a-developer-or-a-content-provider-i-want-to-encode-stream-on-demand-or-live-analyze-my-media-content-so-that-my-customers-can-view-the-content-on-a-wide-variety-of-browsers-and-devices-gain-valuable-insights-from-recorded-content"></a>Kundenabsicht: Als Entwickler oder Inhaltsanbieter möchte ich meine Medieninhalte codieren, (bedarfsgesteuert oder live) streamen und analysieren, damit meine Kunden die Inhalte in verschiedenen Browsern und auf zahlreichen Geräten anzeigen und wertvolle Erkenntnisse aus aufgezeichneten Inhalten gewinnen können.
---

# <a name="azure-media-services-v3-overview"></a>Azure Media Services v3: Übersicht

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure Media Services ist eine cloudbasierte Plattform für die Erstellung von Lösungen, die Videostreaming in Broadcastqualität ermöglichen, Barrierefreiheit und Verteilung optimieren, Inhalte analysieren und vieles mehr. Ganz gleich, ob Sie ein Anwendungsentwickler sind oder in einem Callcenter, bei einer Regierungsbehörde oder in einem Unterhaltungsunternehmen arbeiten – Media Services unterstützt Sie bei der Erstellung von Apps, die Medienerlebnisse von herausragender Qualität für ein breites Publikum auf den beliebtesten mobilen Geräten und Browsern von heute bieten.

Die Media Services v3 SDKs basieren auf der [OpenAPI-Spezifikation von Media Services v3 (Swagger)](https://aka.ms/ams-v3-rest-sdk).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="compliance-privacy-and-security"></a>Compliance, Datenschutz und Sicherheit

Wichtig: Ihre Nutzung von Azure Media Services darf nicht gegen geltende Gesetze verstoßen, und weder Media Services noch ein anderer Azure-Dienst darf auf eine Weise verwendet werden, die die Rechte Dritter verletzt oder für Dritte schädlich ist.

Vor dem Upload eines Videos oder Bilds in Media Services müssen Sie über alle entsprechenden Berechtigungen zur Verwendung des Videos/Bilds verfügen, einschließlich – sofern gesetzlich erforderlich – alle notwendigen Zustimmungen von Einzelpersonen (falls vorhanden), die auf dem Video/Bild zu sehen sind, zur Nutzung, Verarbeitung und Speicherung ihrer Daten in Media Services und Azure. Je nach Rechtsprechung können besondere rechtliche Anforderungen für die Sammlung, Onlineverarbeitung und Speicherung bestimmter Datenkategorien, z. B. biometrische Daten, gelten. Stellen Sie Konformität mit allen rechtlichen Anforderungen sicher, die möglicherweise für Sie gelten, bevor Sie Media Services und Azure für die Verarbeitung und Speicherung von Daten verwenden, die bestimmten rechtlichen Anforderungen unterliegen.

Informationen zu Compliance, Datenschutz und Sicherheit in Media Services finden Sie im [Trust Center](https://www.microsoft.com/trust-center/?rtc=1) von Microsoft. Informationen zu den Datenschutzauflagen und zur Behandlung Ihrer Daten sowie Datenaufbewahrungsmethoden durch Microsoft, einschließlich Informationen zur Löschung Ihrer Daten, finden Sie in den [Datenschutzbestimmungen](https://privacy.microsoft.com/PrivacyStatement), in den [Lizenzbedingungen für Onlinedienste](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) (Online Services Terms, OST) und im [Nachtrag zur Datenverarbeitung](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) (Data Processing Addendum, DPA) von Microsoft. Wenn Sie Media Services nutzen, erklären Sie sich damit einverstanden, dass Sie OST, DPA und den Datenschutzbestimmungen unterliegen.
 
## <a name="what-can-i-do-with-media-services"></a>Wie kann ich Media Services nutzen?

Mit Media Services können Sie eine Vielzahl von Medienworkflows in der Cloud erstellen. Einige Beispiele für die Verwendungsmöglichkeiten von Media Services:

* Übermitteln Sie Videos in verschiedenen Formaten, damit sie auf einer Vielzahl von Browsern und Geräten wiedergegeben werden können. Für die bedarfsgesteuerte und Livestreamingbereitstellung für verschiedene Clients (mobile Geräte, TV, PC usw.) müssen die Video- und Audioinhalte ordnungsgemäß codiert und verpackt werden. Informationen zum Bereitstellen und Streamen solcher Inhalte finden Sie unter [Schnellstart: Codieren und Streamen von Dateien](stream-files-dotnet-quickstart.md).
* Streamen Sie Livesportereignisse an ein großes Onlinepublikum, z. B. Fußball, Baseball, Uni- und Schulsport und vieles mehr.
* Übertragen Sie öffentliche Versammlungen und Veranstaltungen, z. B. in Rathäusern, bei Stadtratssitzungen oder von Gesetzgebungsorganen.
* Analysieren Sie aufgezeichnete Videos oder Audioinhalte. Um beispielsweise eine höhere Kundenzufriedenheit zu erreichen, können Unternehmen Sprache-in-Text extrahieren und Suchindizes und Dashboards erstellen. Sie können dann Daten zu häufigen Beschwerden und ihren Ursachen sowie andere relevante Daten extrahieren.
* Erstellen Sie einen Abonnementvideodienst, und streamen Sie durch DRM geschützte Inhalte, wenn ein Kunde (z.B. ein Filmstudio) den Zugriff und die Nutzung von urheberrechtlich geschützten Werken einschränken muss.
* Stellen Sie Offlineinhalte für die Wiedergabe in Flugzeugen, Zügen und Autos bereit. Möglicherweise muss ein Kunde Inhalte zur Wiedergabe auf sein Smartphone oder Tablet herunterladen, wenn er eine Trennung vom Netzwerk erwartet.
* Implementieren Sie eine E-Learning-Videoplattform mit Azure Media Services und [Azure Cognitive Services-APIs](../../index.yml?pivot=products&panel=ai) für Sprache-in-Text-Untertitel, Übersetzung in mehrere Sprachen usw.
* Verwenden Sie Azure Media Services zusammen mit [Azure Cognitive Services-APIs](../../index.yml?pivot=products&panel=ai), um Untertitel zu Videos hinzuzufügen und so ein breiteres Publikum anzusprechen (z.B. Menschen mit Hörbehinderungen oder Menschen, die in einer anderen Sprache mitlesen möchten).
* Aktivieren Sie Azure CDN, um eine umfassende Skalierung zu erreichen und hohe Auslastungen besser zu bewältigen (z.B. den Beginn einer Produkteinführung).

## <a name="how-can-i-get-started-with-v3"></a>Wie kann ich erste Schritte mit v3 ausführen?

Hier erfahren Sie, wie Sie mit Media Services Videos v3 Inhalte codieren und packen sowie Videos on Demand streamen, live übertragen und analysieren. Anhand von Tutorials, API-Referenzen und anderem Dokumentationsmaterial wird gezeigt, wie Sie Video- oder Audiostreams (bedarfsgesteuert oder live) sicher und mit Skalierung für Millionen von Benutzern übermitteln.

> [!TIP]
> Machen Sie sich zunächst mit Folgendem vertraut, bevor Sie mit der Entwicklung beginnen: Mit dem Artikel [Grundlegende Konzepte](concepts-overview.md), der wichtige Konzepte wie Packen, Codierung und Schutz enthält, und mit dem Artikel [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md), der Informationen zum Zugreifen auf APIs, zu Benennungskonventionen und vielem mehr enthält.

### <a name="sdks"></a>SDKs

Beginnen Sie mit den [Azure Media Services v3-Client-SDKs](media-services-apis-overview.md#sdks) mit dem Entwickeln.

### <a name="quickstarts"></a>Schnellstarts  

Die Schnellstartanleitungen enthalten grundlegende Informationen für Einsteiger, mit denen neue Kunden Media Services schnell ausprobieren können.

* [Streamen von Videodateien: .NET](stream-files-dotnet-quickstart.md)
* [Streamen von Videodateien: CLI](stream-files-cli-quickstart.md)
* [Streamen von Videodateien: Node.js](stream-files-nodejs-quickstart.md)

### <a name="tutorials"></a>Tutorials

Die Tutorials enthalten szenariobasierte Verfahren für einige der wichtigsten Media Services-Aufgaben.

* [Codieren von Remotedateien und Streamen von Videos: REST](stream-files-tutorial-with-rest.md)
* [Codieren von hochgeladenen Dateien und Streamen von Videos: .NET](stream-files-tutorial-with-api.md)
* [Livestreaming: .NET](stream-live-tutorial-with-api.md)
* [Analysieren Ihres Videos: .NET](analyze-videos-tutorial.md)
* [Dynamische AES-128-Verschlüsselung: .NET](drm-playready-license-template-concept.md)

### <a name="samples"></a>Beispiele

Verwenden Sie diesen [Beispielbrowser](/samples/browse/?products=azure-media-services), um Azure Media Services-Codebeispiele zu durchsuchen.

### <a name="how-to-guides"></a>Anleitungen

Die Anleitungen enthalten Codebeispiele, um die Ausführung einer Aufgabe zu veranschaulichen. In diesem Abschnitt finden Sie viele Beispiele. Einige davon sind:

* [Erstellen eines Azure Media Services-Kontos](./account-create-how-to.md)
* [Zugriff auf Azure Media Services-API mit Azure CLI](./access-api-howto.md)
* [Codieren mit HTTPS als Auftragseingabe: .NET](job-input-from-http-how-to.md)  
* [Überwachen von Ereignissen: Portal](monitoring/monitor-events-portal-how-to.md)
* [Dynamische Verschlüsselung mit Multi-DRM: .NET](drm-protect-with-drm-tutorial.md) 
* [Codieren mit einer benutzerdefinierten Transformation: CLI](transform-custom-preset-cli-how-to.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

[Kenntnisse grundlegender Konzepte](concepts-overview.md)
