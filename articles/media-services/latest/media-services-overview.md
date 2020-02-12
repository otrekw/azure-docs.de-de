---
title: 'Azure Media Services v3: Übersicht'
titleSuffix: Azure Media Services
description: Eine allgemeine Übersicht über Azure Media Services v3 mit Links zu Schnellstarts, Tutorials und Codebeispielen.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, streamen, übertragen, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 02/03/2020
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 559050d11b6b85a9842c679b94e0e5b2fd5dd0f8
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76987712"
---
# <a name="azure-media-services-v3-overview"></a>Azure Media Services v3: Übersicht

Azure Media Services ist eine cloudbasierte Plattform für die Erstellung von Lösungen, die Videostreaming in Broadcastqualität ermöglichen, Barrierefreiheit und Verteilung optimieren, Inhalte analysieren und vieles mehr. Ganz gleich, ob Sie ein Anwendungsentwickler sind oder in einem Callcenter, bei einer Regierungsbehörde oder in einem Unterhaltungsunternehmen arbeiten, Media Services unterstützt Sie bei der Erstellung von Apps, die Medienerlebnisse von herausragender Qualität für ein breites Publikum auf den beliebtesten mobilen Geräten und Browsern von heute bieten.

Die Media Services v3 SDKs basieren auf der [OpenAPI-Spezifikation von Media Services v3 (Swagger)](https://aka.ms/ams-v3-rest-sdk).

> [!NOTE]
> Derzeit können Sie das Azure-Portal nicht für die Verwaltung von v3-Ressourcen verwenden. Verwenden Sie die [REST-API](https://aka.ms/ams-v3-rest-ref), die [Befehlszeilenschnittstelle](https://aka.ms/ams-v3-cli-ref) oder eines der unterstützten [SDKs](media-services-apis-overview.md#sdks).

## <a name="compliance-privacy-and-security"></a>Compliance, Datenschutz und Sicherheit
 
Wichtig: Ihre Nutzung von Azure Media Services darf nicht gegen geltende Gesetze verstoßen, und weder Azure Media Services noch ein anderer Azure-Dienst darf auf eine Weise verwendet werden, die die Rechte Dritter verletzt oder für Dritte schädlich ist. 

Bevor Sie Videos/Bilder, einschließlich biometrischer Daten, zur Verarbeitung und Speicherung in Media Services hochladen, müssen Sie (1) über alle entsprechenden Rechte für die Videos/Bilder und (2) über alle zugehörigen Einwilligungen von den Personen in den Videos verfügen, mit denen die Nutzung und Speicherung ihrer biometrischen Daten in Media Services zugelassen wird. 

Besuchen Sie das [Azure Trust Center](https://www.microsoft.com/trust-center/?rtc=1), um sich über die Konformität, den Datenschutz und den Schutz Ihrer Daten in Azure Media Services durch Microsoft zu informieren. Informationen zu den Datenschutzauflagen und zur Behandlung Ihrer Daten durch Microsoft finden Sie in den [Datenschutzbestimmungen](https://privacy.microsoft.com/PrivacyStatement), in den [Lizenzbedingungen für Onlinedienste](https://www.microsoft.com/licensing/product-licensing/products) (Online Services Terms, OST) und im [Nachtrag zur Datenverarbeitung](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) (Data Processing Addendum, DPA). Informationen zur Aufbewahrung und Löschung von Kundendaten sind im Nachtrag zur Datenverarbeitung enthalten. Wenn Sie Azure Media Services nutzen, erklären Sie sich damit einverstanden, dass Sie OST, DPA und den Datenschutzbestimmungen unterliegen.

## <a name="what-can-i-do-with-media-services"></a>Wie kann ich Media Services nutzen?

Mit Media Services können Sie eine Vielzahl von Medienworkflows in der Cloud erstellen. Einige Beispiele für die Verwendungsmöglichkeiten von Media Services:

* Übermitteln Sie Videos in verschiedenen Formaten, damit sie auf einer Vielzahl von Browsern und Geräten wiedergegeben werden können. Für die bedarfsgesteuerte und Livestreamingbereitstellung für verschiedene Clients (mobile Geräte, TV, PC usw.) müssen die Video- und Audioinhalte ordnungsgemäß codiert und verpackt werden. Informationen zum Bereitstellen und Streamen solcher Inhalte finden Sie unter [Schnellstart: Codieren und Streamen von Dateien](stream-files-dotnet-quickstart.md).
* Streamen Sie Livesportereignisse an ein großes Onlinepublikum, z. B. Fußball, Baseball, Uni- und Schulsport und vieles mehr.
* Übertragen Sie öffentliche Versammlungen und Veranstaltungen, z. B. in Rathäusern, bei Stadtratssitzungen oder von Gesetzgebungsorganen.
* Analysieren Sie aufgezeichnete Videos oder Audioinhalte. Um beispielsweise eine höhere Kundenzufriedenheit zu erreichen, können Unternehmen Sprache-in-Text extrahieren und Suchindizes und Dashboards erstellen. Sie können dann Daten zu häufigen Beschwerden und ihren Ursachen sowie andere relevante Daten extrahieren.
* Erstellen Sie einen Abonnementvideodienst, und streamen Sie durch DRM geschützte Inhalte, wenn ein Kunde (z.B. ein Filmstudio) den Zugriff und die Nutzung von urheberrechtlich geschützten Werken einschränken muss.
* Stellen Sie Offlineinhalte für die Wiedergabe in Flugzeugen, Zügen und Autos bereit. Möglicherweise muss ein Kunde Inhalte zur Wiedergabe auf sein Smartphone oder Tablet herunterladen, wenn er eine Trennung vom Netzwerk erwartet.
* Implementieren Sie eine E-Learning-Videoplattform mit Azure Media Services und [Azure Cognitive Services-APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) für Sprache-in-Text-Untertitel, Übersetzung in mehrere Sprachen usw.
* Verwenden Sie Azure Media Services zusammen mit [Azure Cognitive Services-APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai), um Untertitel zu Videos hinzuzufügen und so ein breiteres Publikum anzusprechen (z.B. Menschen mit Hörbehinderungen oder Menschen, die in einer anderen Sprache mitlesen möchten).
* Aktivieren Sie Azure CDN, um eine umfassende Skalierung zu erreichen und hohe Auslastungen besser zu bewältigen (z.B. den Beginn einer Produkteinführung).

## <a name="how-can-i-get-started-with-v3"></a>Wie kann ich erste Schritte mit v3 ausführen? 

Hier erfahren Sie, wie Sie mit Media Services Videos v3 Inhalte codieren und packen sowie Videos on Demand streamen, live übertragen und analysieren. Anhand von Tutorials, API-Referenzen und anderem Dokumentationsmaterial wird gezeigt, wie Sie Video- oder Audiostreams (bedarfsgesteuert oder live) sicher und mit Skalierung für Millionen von Benutzern übermitteln.

> [!TIP]
> Machen Sie sich zunächst mit Folgendem vertraut, bevor Sie mit der Entwicklung beginnen:<br/>* [Grundlegende Konzepte](concepts-overview.md) (beinhaltet wichtige Konzepte wie Verpacken, Codierung und Schutz)<br/>* [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md) (Informationen zum Zugreifen auf APIs, Namenskonventionen usw.)

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
* [Analysieren Ihres Videos: .NET](analyze-videos-tutorial-with-api.md)
* [Dynamische AES-128-Verschlüsselung: .NET](protect-with-aes128.md)

### <a name="samples"></a>Beispiele

Verwenden Sie diesen [Beispielbrowser](https://docs.microsoft.com/samples/browse/?products=azure-media-services), um Azure Media Services-Codebeispiele zu durchsuchen.

### <a name="how-to-guides"></a>Anleitungen

Die Anleitungen enthalten Codebeispiele, um die Ausführung einer Aufgabe zu veranschaulichen. In diesem Abschnitt finden Sie viele Beispiele. Einige davon sind:

* [Erstellen eines Azure Media Services-Kontos](create-account-cli-how-to.md)
* [Zugriff auf Azure Media Services-API mit Azure CLI](access-api-cli-how-to.md)
* [Codieren mit HTTPS als Auftragseingabe: .NET](job-input-from-http-how-to.md)  
* [Überwachen von Ereignissen: Portal](monitor-events-portal-how-to.md)
* [Dynamische Verschlüsselung mit Multi-DRM: .NET](protect-with-drm.md) 
* [Codieren mit einer benutzerdefinierten Transformation: CLI](custom-preset-cli-howto.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

[Kenntnisse grundlegender Konzepte](concepts-overview.md)

