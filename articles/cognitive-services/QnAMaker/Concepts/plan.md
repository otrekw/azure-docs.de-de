---
title: Planen Ihrer App – QnA Maker
description: Die Planung Ihrer QnA Maker-App erfordert Kenntnisse über die Funktionsweise von QnA Maker und interagiert mit anderen Azure-Diensten sowie einigen Wissensdatenbank-Konzepten.
ms.topic: conceptual
ms.date: 07/2/2020
ms.openlocfilehash: d19ec51aec7e71b6f040a03543f72af3aed09556
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85875384"
---
# <a name="plan-your-qna-maker-app"></a>Planen Ihrer QnA Maker-App

Die Planung Ihrer QnA Maker-App erfordert Kenntnisse über die Funktionsweise von QnA Maker und interagiert mit anderen Azure-Diensten sowie einigen Wissensdatenbank-Konzepten.

## <a name="azure-resources"></a>Azure-Ressourcen

Jede [Azure-Ressource](azure-resources.md#resource-purposes), die mit QnA Maker erstellt wird, hat einen bestimmten Zweck. Da jede Ressource eigene Zwecke, Grenzwerte und [Tarife](azure-resources.md#pricing-tier-considerations) aufweist, müssen Sie wissen, wie diese Ressourcen im Rahmen Ihres Planungsprozesses funktionieren.

|Resource|Zweck|
|--|--|
| [QnA Maker](azure-resources.md#qna-maker-resource)-Ressource|Erstellung und Abfragevorhersage|
| [Cognitive Search](azure-resources.md#cognitive-search-resource)-Ressource|Datenspeicherung und -suche|
| [App Service-Ressource und App Plan Service](azure-resources.md#app-service-and-app-service-plan)-Ressource|Abfragevorhersage-Endpunkt|
| [Application Insights](azure-resources.md#application-insights)-Ressource|Abfragevorhersagetelemetrie|

### <a name="resource-planning"></a>Ressourcenplanung

Während Sie Erstellen und Abfragevorhersage lernen, funktioniert jede Ressource unter Verwendung des Free-Tarifs `F0` und vermittelt sowohl die Erstellungs- als auch Abfragevorhersageerfahrung. Wenn Sie zu einem Produktions- oder Liveszenario wechseln, sollten Sie Ihre Ressourcenauswahl neu auswerten.

#### <a name="qna-maker-resource"></a>QnA Maker-Ressource

Eine einzelne QnA Maker-Ressource kann mehr hosten als eine Wissensdatenbank. Die Anzahl der Wissensdatenbanken wird durch die Menge der Indizes bestimmt, die der Cognitive Search-Tarif unterstützt. Erfahren Sie mehr über die [Beziehung zwischen Indizes und Wissensdatenbanken](azure-resources.md#index-usage).

#### <a name="knowledge-base-size-and-throughput"></a>Größe und Durchsatz der Wissensdatenbank

Wenn Sie beabsichtigen, eine echte App zu erstellen, planen Sie Ihre Ressourcen für die Größe Ihrer Wissensdatenbank und die erwarteten Abfragevorhersageanforderungen.

Die Größe einer Wissensdatenbank wird gesteuert durch:
* Tarifgrenzwerte der [Cognitive Search-Ressource](../../../search/search-limits-quotas-capacity.md)
* [QnA Maker-Grenzwerte](../limits.md)

Die Abfragevorhersageanforderung der Wissensdatenbank wird durch den Web-App-Plan und die Web-App gesteuert. Informationen zum Planen Ihres Tarifs finden Sie unter [Empfohlene Einstellungen](azure-resources.md#recommended-settings).

### <a name="resource-sharing"></a>Gemeinsame Nutzung von Ressourcen

Wenn Sie bereits einige dieser Ressourcen verwenden, können Sie die Freigabe von Ressourcen in Erwägung ziehen. Obwohl einige Ressourcen [freigegeben werden können](azure-resources.md#share-services-with-qna-maker), handelt es sich hierbei um ein erweitertes Szenario.

Alle Wissensdatenbanken, die in derselben QnA Maker-Ressource erstellt wurden, verwenden denselben **Test**-Abfragevorhersage-Endpunkt.

### <a name="understanding-impact-of-resource-selection"></a>Grundlegendes zu den Auswirkungen der Ressourcenauswahl

Die richtige Ressourcenauswahl bedeutet, dass Ihre Wissensdatenbank die Abfragevorhersagen erfolgreich beantwortet.

Wenn Ihre Wissensdatenbank nicht ordnungsgemäß funktioniert, liegt das Problem in der Regel an einer nicht ordnungsgemäßen Ressourcenverwaltung.

Bei nicht ordnungsgemäßer Ressourcenauswahl müssen Sie untersuchen, welche [Ressource geändert werden muss](azure-resources.md#when-to-change-a-pricing-tier).

## <a name="knowledge-bases"></a>Wissensdatenbanken

Eine Wissensdatenbank ist direkt mit der QnA Maker-Ressource verknüpft und enthält die Frage-/Antwort-Paare (QnA), die zum Beantworten von Abfragevorhersageanforderungen verwendet werden.

### <a name="language-considerations"></a>Sprachbezogene Überlegungen

Die erste in Ihrer QnA Maker-Ressource erstellte Wissensdatenbank legt die Sprache für die Ressource fest. Sie können nur eine Sprache für eine QnA Maker-Ressource festlegen.

Strukturieren Sie Ihre QnA Maker-Ressourcen nach Sprache, oder verwenden Sie [Translator](../../translator/translator-info-overview.md), um eine Abfrage von einer anderen Sprache in die Sprache der Wissensdatenbank zu übersetzen, bevor Sie die Abfrage an den Abfragevorhersage-Endpunkt senden.

### <a name="ingesting-data-sources"></a>Erfassen von Datenquellen

Als erfasste [Datenquellen](knowledge-base.md), die zum Erstellen einer Wissensdatenbank verwendet werden, kommen folgende infrage:

* Öffentliche URL
* Private SharePoint-URL
* Datei

Beim Erfassungsprozess werden [unterstützte Inhaltstypen](content-types.md) in Markdown konvertiert. Die weitere Bearbeitung der *Antwort* erfolgt mit Markdown. Nachdem Sie Ihre Wissensdatenbank erstellt haben, können Sie [QnA-Paare](question-answer-set.md) im QnA Maker-Portal mit [Rich Text Authoring](../how-to/edit-knowledge-base.md#rich-text-editing-for-answer) bearbeiten.

### <a name="data-format-considerations"></a>Überlegungen zum Datenformat

Da das endgültige Format eines QnA-Paars Markdown ist, müssen Sie die [Markdown-Unterstützung](../reference-markdown-format.md) kennen.

Verknüpfte Bilder müssen über eine öffentliche URL verfügbar sein, damit sie im Testbereich des QnA Maker-Portals sowie in jeder beliebigen Clientanwendung angezeigt werden, da QnA Maker keine Authentifizierung für Inhalte einschließlich Bilder bietet.

### <a name="bot-personality"></a>Botpersönlichkeit

Fügen Sie Ihrer Wissensdatenbank mit [Geplauder](../how-to/chit-chat-knowledge-base.md) eine Botpersönlichkeit hinzu. Diese Persönlichkeit bietet vorgefertigte Antworten, die in einem bestimmten Konversationston bereitgestellt werden, z. B. *geschäftlich* und *freundlich*. Dieses Geplauder wird als Konversationsgruppe bereitgestellt, die Sie die mit Hinzufügen, Bearbeiten und Entfernen vollständig kontrollieren können.

Eine Botpersönlichkeit sollten Sie verwenden, wenn Ihr Bot mit Ihrer Wissensdatenbank verbunden ist. Wenn Sie eine Verbindung mit mehreren Diensten herstellen, von denen die Wissensdatenbank einer ist, können Sie Geplauder weiterhin in Ihrer Wissensdatenbank verwenden, sollten jedoch überprüfen, wie der Botdienst interagiert, um zu wissen, ob dies der richtige Architekturentwurf für ihre Verwendung ist.

### <a name="conversation-flow-with-a-knowledge-base"></a>Konversationsfluss mit einer Wissensdatenbank

Der Konversationsfluss beginnt in der Regel mit einer Begrüßung eines Benutzers, z. B. `Hi` oder `Hello`. Ihre Wissensdatenbank kann mit einer allgemeinen Antwort (z. B. `Hi, how can I help you`) antworten, und Sie kann auch eine Auswahl von Folgeaufforderungen zum Fortsetzen der Konversation bereitstellen.

Sie sollten beim Entwurf Ihres Konversationsflusses an eine Schleife denken, damit ein Benutzer weiß, wie er Ihren Bot verwenden kann, und nicht vom Bot in der Konversation stehen gelassen wird. [Folgeaufforderungen](../how-to/multiturn-conversation.md) bieten die Verknüpfung zwischen QnA-Paaren, die den Konversationsfluss ermöglichen.

### <a name="authoring-with-collaborators"></a>Erstellung mit Projektmitarbeitern

Projektmitarbeiter sind möglicherweise andere Entwickler, für die der vollständige Entwicklungsstapel der Wissensdatenbankanwendung freigeben ist, oder die nur auf die Erstellung der Wissensdatenbank beschränkt sind.

Die Erstellung der Wissensdatenbank unterstützt mehrere [rollenbasierte Zugriffsberechtigungen](../how-to/collaborate-knowledge-base.md), die Sie im Azure-Portal anwenden, um den Handlungsspielraum eines Projektmitarbeiters einzuschränken.

## <a name="integration-with-client-applications"></a>Integration in Clientanwendungen

Integration in [Clientanwendungen](integration-with-other-applications.md) bedeutet, dass eine Abfrage an den Vorhersage-Runtimeendpunkt gesendet wird. Eine Abfrage wird mit einem SDK oder einer REST-basierten Anforderung an den Web-App-Endpunkt Ihres QnA Maker an Ihre bestimmte Wissensdatenbank gesendet.

Damit eine Clientanforderung ordnungsgemäß authentifiziert werden kann, muss die Clientanwendung die richtigen Anmeldeinformationen und die ID der Wissensdatenbank senden. Wenn Sie einen Azure Bot Service verwenden, konfigurieren Sie die Einstellung als Teil der Botkonfiguration im Azure-Portal.

### <a name="conversation-flow-in-a-client-application"></a>Konversationsfluss in einer Clientanwendung

Der Konversationsfluss in einer [Clientanwendungs](integration-with-other-applications.md) wie einem Azure-Bot erfordert möglicherweise vor und nach der Interaktion mit der Wissensdatenbank Funktionalität.

Wenn Ihre Clientanwendung den Konversationsfluss unterstützt, indem sie entweder Alternativmöglichkeiten zur Behandlung von Folgeaufforderungen oder Geplauder bietet, sollten Sie diese früh entwerfen und sicherstellen, dass die Verwendung der Abfrage in der Clientanwendung ordnungsgemäß verarbeitet wird, entweder durch einen anderen Dienst, oder an Ihre Wissensdatenbank gesendet wird.

### <a name="dispatching-between-qna-maker-and-language-understanding-luis"></a>Disponieren zwischen QnA Maker und Language Understanding (Luis)

Eine Clientanwendung kann mehrere Features bereitstellen, von denen nur eines durch eine Wissensdatenbank beantwortet wird. Andere Features müssten weiterhin den Konversationstext verstehen und die Bedeutung daraus extrahieren.

Eine gängige Clientanwendungsarchitektur ist die gemeinsame Verwendung von QnA Maker und [Language Understanding (Luis)](../../LUIS/what-is-luis.md). LUIS stellt einschließlich anderer Dienste die Textklassifizierung und -extrahierung für jede Abfrage bereit, während QnA Maker Antworten aus Ihrer Wissensdatenbank bereitstellt.

In einer [freigegebenen Architektur](../choose-natural-language-processing-service.md) erfolgt die Verteilung zwischen den beiden Diensten mit dem [Dispatch](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch)-Tool aus Bot Framework.

### <a name="active-learning-from-a-client-application"></a>Aktives Lernen über eine Clientanwendung

QnA Maker nutzt _aktives Lernen_, um Ihre Wissensdatenbank durch Vorschlagen alternativer Fragen zu einer Antwort zu verbessern. Die Clientanwendung ist für einen Teil dieses [aktiven Lernens](active-learning-suggestions.md) verantwortlich. Die Clientanwendung kann mithilfe von Konversationsaufforderungen feststellen, dass die von der Wissensdatenbank zurückgegebene Antwort nicht die Antwort war, die der Benutzer suchte, und die bessere Antwort ermitteln. Die Clientanwendung muss [diese Informationen an die Wissensdatenbank zurücksenden](active-learning-suggestions.md#how-you-give-explicit-feedback-with-the-train-api), um die Vorhersagequalität zu verbessern.

### <a name="providing-a-default-answer"></a>Bereitstellen einer Standardantwort

Wenn Ihre Wissensdatenbank keine Antwort findet, wird die _Standardantwort_ zurückgegeben. Diese Antwort kann im QnA Maker-Portal auf der Seite **Einstellungen** oder in den [APIs](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#request-body) konfiguriert werden.

Diese Standardantwort unterscheidet sich von der Standardantwort des Azure-Bots. Die Standardantwort des Azure-Bots wird im Azure-Portal als Teil der Konfigurationseinstellungen für Ihren Bot konfiguriert und zurückgegeben, wenn der Bewertungsschwellenwert nicht erreicht wird.

## <a name="prediction"></a>Vorhersage

Die Vorhersage ist die Antwort aus Ihrer Wissensdatenbank und enthält mehr Informationen als nur die Antwort. Um eine Abfragevorhersageantwort zu erhalten, verwenden Sie die [GenerateAnswer-API](query-knowledge-base.md).

### <a name="prediction-score-fluctuations"></a>Fluktuationen des Vorhersageergebnisses

Ein Ergebnis kann sich auf der Grundlage verschiedener Faktoren ändern:

* Anzahl der Antworten, die Sie als Reaktion auf [GenerateAnswer](query-knowledge-base.md) mit `top`-Eigenschaft angefordert haben
* Vielzahl verfügbarer alternativer Fragen
* Filtern nach Metadaten
* An `test` oder `production` Wissensdatenbank gesendete Abfrage

Es gibt eine [zweiphasige Antwortrangfolge](query-knowledge-base.md#how-qna-maker-processes-a-user-query-to-select-the-best-answer):
* Cognitive Search – erster Rang: Damit eine Antwort von Cognitive Search zurückgegeben werden kann, muss die Anzahl der _zulässigen Antworten_ so hoch sein, dass die besten Antworten von Cognitive Search zurückgegeben werden, damit sie an die Rangfolgefunktion von QnA Maker übergeben werden können.
* QnA Maker – zweiter Rang: Ermitteln der besten Antwort durch Featurisierung und Machine Learning.

### <a name="service-updates"></a>Dienstupdates

Dienstupdates werden automatisch durch Anwenden der [aktuellen Runtimeupdates](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) verwaltet.

### <a name="scaling-throughput-and-resiliency"></a>Skalierung, Durchsatz und Resilienz

Skalierung, Durchsatz und Resilienz werden durch die [Azure-Ressourcen](../how-to/set-up-qnamaker-service-azure.md), ihre Tarife und alle umgebenden Architekturen, wie [Traffic Manager](../how-to/set-up-qnamaker-service-azure.md#business-continuity-with-traffic-manager) bestimmt.

### <a name="analytics-with-application-insights"></a>Analyse mit Application Insights

Alle Abfragen Ihrer Wissensdatenbank werden in Application Insights gespeichert. Verwenden Sie unsere [wichtigsten Abfragen](../how-to/get-analytics-knowledge-base.md), um Ihre Metriken zu verstehen.

## <a name="development-lifecycle"></a>Lebenszyklus der Entwicklung

Der [Entwicklungslebenszyklus](development-lifecycle-knowledge-base.md) einer Wissensdatenbank ist fortlaufend: Bearbeiten, Testen und Veröffentlichen Ihrer Wissensdatenbank.

### <a name="knowledge-base-development-of-qna-maker-pairs"></a>Entwicklung von QnA Maker-Paaren in der Wissensdatenbank

Ihre [QnA-Paare](question-answer-set.md) sollten auf Basis der Nutzung Ihrer Clientanwendung entworfen und entwickelt werden.

Jedes Paar kann Folgendes enthalten:
* Metadaten – bei der Abfrage filterbar. So können Sie Ihre QnA-Paare mit zusätzlichen Informationen über Quelle, Inhalt, Format und Zweck Ihrer Daten markieren.
* Folgeaufforderungen – bahnen einen Pfad durch Ihre Wissensdatenbank, damit der Benutzer die richtige Antwort findet.
* Alternative Fragen – alternative Fragen sind wichtig, damit die Übereinstimmung Ihrer Antwort mit der Suche aus einer Vielzahl von Formen der Frage ermittelt werden kann. [Vorschläge für aktives Lernen](active-learning-suggestions.md) werden in alternative Fragen umgewandelt.

### <a name="devops-development"></a>DevOps-Entwicklung

Zum Entwickeln einer Wissensdatenbank, die in eine DevOps-Pipeline eingefügt werden soll, muss die Wissensdatenbank in [Batchtests](../quickstarts/batch-testing.md) isoliert werden.

Eine Wissensdatenbank teilt den Cognitive Search-Index mit allen anderen Wissensdatenbanken der QnA Maker-Ressource. Obwohl die Wissensdatenbank durch Partition isoliert ist, kann die Freigabe des Indexes im Vergleich zur veröffentlichten Wissensdatenbank einen Unterschied im Ergebnis verursachen.

Um mit der `test`- und `production`-Wissensdatenbank das _gleiche Ergebnis_ zu erhalten, isolieren Sie eine QnA Maker-Ressource in eine einzelne Wissensdatenbank. In dieser Architektur muss die Ressource nur für die Dauer des isolierten Batchtests existieren.

## <a name="next-step"></a>Nächster Schritt

* [Azure-Ressourcen](../how-to/set-up-qnamaker-service-azure.md)
* [Frage-Antwort-Paare](question-answer-set.md)