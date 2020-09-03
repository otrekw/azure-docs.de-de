---
title: Worum handelt es sich bei Language Understanding (LUIS)?
description: Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der maschinelles Lernen auf Konversationstext in natürlicher Sprache anwendet, um die Bedeutung vorherzusagen und Informationen zu extrahieren.
keywords: Azure, künstliche Intelligenz, KI, Verarbeitung natürlicher Sprache, NLP, Verstehen natürlicher Sprache, NLU, KI-Konversation, Konversations-KI, KI Chatbot, Chatbot Maker, LUIS, NLP KI, LUIS KI, Azure LUIS, natürliche Sprache verstehen
ms.topic: overview
ms.date: 08/07/2020
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 98d801f6a34feb40d56215f8b6257a68ec628e10
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89320051"
---
# <a name="what-is-language-understanding-luis"></a>Worum handelt es sich bei Language Understanding (LUIS)?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der benutzerdefinierte Machine Learning-Intelligenz auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen.

Bei einer Clientanwendung für LUIS handelt es sich um eine beliebige Konversationsanwendung, die zum Durchführen einer Aufgabe mit einem Benutzer in natürlicher Sprache kommuniziert. Beispiele für Clientanwendungen sind Social Media Apps, Chatbots und sprachfähige Desktopanwendungen.

![Konzeptionelle Abbildung dreier Clientanwendungen, die mit Cognitive Services Language Understanding (LUIS) arbeiten](./media/luis-overview/luis-entry-point.png "Konzeptionelle Abbildung dreier Clientanwendungen, die mit Cognitive Services Language Understanding (LUIS) arbeiten")

## <a name="use-luis-in-a-chat-bot"></a>Verwendung von LUIS in einen Chatbot

<a name="Accessing-LUIS"></a>

Sobald die LUIS-App veröffentlicht ist, sendet eine Clientanwendung Äußerungen (Text) an die LUIS-Endpunkt-[API][endpoint-apis] für die Verarbeitung von natürlicher Sprache und empfängt die Ergebnisse als JSON-Antworten. Eine gängige Clientanwendung für LUIS ist ein Chatbot.


![Konzeptionelle Abbildungen von LUIS in Zusammenarbeit mit dem Chatbot zur Vorhersage von Benutzertext mithilfe des Verstehens natürlicher Sprache](./media/luis-overview/LUIS-chat-bot-request-response.svg "Konzeptionelle Abbildungen von LUIS in Zusammenarbeit mit dem Chatbot zur Vorhersage von Benutzertext mithilfe des Verstehens natürlicher Sprache")

|Schritt|Aktion|
|:--|:--|
|1|Die Clientanwendung sendet die _Benutzeräußerung_ (Text in den Worten des Benutzers) „Ich möchte meinen Personalverantwortlichen anrufen“ als HTTP-Anforderung an den LUIS-Endpunkt.|
|2|LUIS ermöglicht Ihnen das Erstellen eigener Sprachmodelle, um Ihrer Anwendung Intelligenz zu verleihen. Maschinell erlernte Sprachmodelle verwenden den unstrukturierten Eingabetext des Benutzers und geben eine Antwort im JSON-Format mit der wichtigsten Absicht, `HRContact`, zurück. Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. Sie kann auch Daten wie die Entität _Kontakttyp_ extrahieren.|
|3|Die Clientanwendung verwendet die JSON-Antwort für Entscheidungen darüber, wie die Anforderungen des Benutzer erfüllt werden. Diese Entscheidungen können eine Entscheidungsstruktur im Frameworkcode des Bots und Aufrufe anderer Dienste umfassen. |

Die LUIS-App stellt Intelligenz bereit, damit die Clientanwendung intelligente Entscheidungen treffen kann. Die Auswahlmöglichkeiten werden nicht von LUIS bereitgestellt.

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-understanding-nlu"></a>Verstehen natürlicher Sprache (Natural Language Understanding, NLU)

[LUIS bietet künstliche Intelligenz (KI)](artificial-intelligence.md) in Form von NLU, einem Teil der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP).

Ihre LUIS-App enthält ein domänenspezifisches Modell für natürliche Sprache. Sie können die LUIS-App mit einem vordefinierten Domänenmodell beginnen, ein eigenes Modell erstellen oder Teile einer vordefinierten Domäne mit Ihren eigenen benutzerdefinierten Informationen mischen.

* **Vordefiniertes Modell**: LUIS verfügt über eine Vielzahl vordefinierter Domänenmodelle, einschließlich Absichten, Äußerungen und vordefinierter Entitäten. Sie können die vordefinierten Entitäten nutzen, ohne die Absichten und Äußerungen des vordefinierten Modells verwenden zu müssen. [Vordefinierte Domänenmodelle](luis-how-to-use-prebuilt-domains.md) enthalten den gesamten Entwurf und bieten eine hervorragende Möglichkeit für ein schnelle Verwendung von LUIS.

* **Benutzerdefiniertes Modell** LUIS bietet Ihnen mehrere Möglichkeiten, eigene benutzerdefinierte Modelle zu definieren, einschließlich Absichten und Entitäten. Entitäten umfassen Machine Learning-Entitäten, bestimmte oder literale Entitäten sowie eine Kombination von literalen und Machine Learning-Entitäten.

Informieren Sie sich ausführlicher über [NLP](artificial-intelligence.md) und den LUIS-spezifischen Bereich NLU.

## <a name="step-1-design-and-build-your-model"></a>Schritt 1: Entwerfen und Erstellen des Modells

Entwerfen Sie Ihr Modell mit Kategorien von Benutzerabsichten, die als **[Absichten](luis-concept-intent.md)** bezeichnet werden. Jede Absicht erfordert Beispiele für **[Äußerungen](luis-concept-utterance.md)** von Benutzern. Jede Äußerung kann Daten enthalten, die mit [Machine Learning-Entitäten](luis-concept-entity-types.md#effective-machine-learned-entities) extrahiert werden müssen.

|Beispieläußerung eines Benutzers|Intent|Extrahierte Daten|
|-----------|-----------|-----------|
|`Book a flight to Seattle?`|FlugBuchen|Seattle|
|`When does your store open?`|GeschäftszeitenUndStandorte|öffnen|
|`Schedule a meeting at 1pm with Bob in Distribution`|BesprechungPlanen|13 Uhr, Robert|

Erstellen Sie das Modell mit den [Dokumenterstellungs](https://go.microsoft.com/fwlink/?linkid=2092087)-APIs, dem [**LUIS-Portal**](https://www.luis.ai) oder mit beidem. Erfahren Sie mehr über das Erstellen mit dem [Portal](get-started-portal-build-app.md) und den [SDK-Clientbibliotheken](azure-sdk-quickstart.md).

## <a name="step-2-get-the-query-prediction"></a>Schritt 2: Abrufen der Abfragevorhersage

Nachdem Ihr App-Modell trainiert und für den Endpunkt veröffentlicht wurde, sendet eine Clientanwendung (etwa ein Chatbot) Äußerungen an die API des [Vorhersageendpunkts](https://go.microsoft.com/fwlink/?linkid=2092356). Die API wendet zu Analysezwecken das Modell auf die Äußerung an und antwortet mit Vorhersageergebnissen im JSON-Format.

Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. Sie kann auch Daten wie die folgende Entität **Kontakttyp** und die allgemeine Stimmung extrahieren.

```JSON
{
    "query": "I want to call my HR rep",
    "prediction": {
        "topIntent": "HRContact",
        "intents": {
            "HRContact": {
                "score": 0.8582669
            }
        },
        "entities": {
            "Contact Type": [
                "call"
            ]
        },
        "sentiment": {
            "label": "neutral",
            "score": 0.5
        }
    }
}
```

## <a name="step-3-improve-model-prediction"></a>Schritt 3: Verbessern der Modellvorhersage

Nachdem die LUIS-App veröffentlicht wurde und echte Benutzeräußerungen empfängt, ermöglicht LUIS zum Verbessern der Vorhersagegenauigkeit [aktives Lernen](luis-concept-review-endpoint-utterances.md) anhand von Endpunktäußerungen. Überprüfen Sie diese Vorschläge im Rahmen der regelmäßigen Wartungsarbeiten in Ihrem Entwicklungslebenszyklus.

<a name="using-luis"></a>

## <a name="development-lifecycle-and-tools"></a>Entwicklungslebenszyklus und -tools
LUIS bietet Tools, eine Versionsverwaltung und die Zusammenarbeit mit anderen LUIS-Autoren für die Integration in den vollständigen [Entwicklungslebenszyklus](luis-concept-app-iteration.md).

LUIS (Language Understanding ) kann als REST-API mithilfe einer HTTP-Anforderung mit allen Produkten, Diensten oder Frameworks eingesetzt werden. LUIS bietet außerdem Clientbibliotheken (SDKs) für verschiedene Programmiersprachen. Informieren Sie sich ausführlicher über die bereitgestellten [Entwicklerressourcen](developer-reference-resource.md).

Tools zum schnellen und einfachen Nutzen von LUIS mit einem Bot:
* [LUIS-CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) Mit dem NPM-Paket sind Erstellungen und Vorhersagen entweder mit einem eigenständigen Befehlszeilentool oder als Import möglich.
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen ist ein Tool zum Generieren von stark typisiertem C#- und TypeScript-Quellcode aus einem exportierten LUIS-Modell.
* [Dispatch](https://aka.ms/dispatch-tool) ermöglicht die Verwendung mehrerer LUIS- und QnA Maker-Apps aus einer übergeordneten App mithilfe eines Verteilermodells.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown ist ein Befehlszeilentool, mit dem Sprachmodelle für Ihren Bot verwaltet werden können.

## <a name="integrate-with-a-bot"></a>Integrieren in einen Bot

Verwenden Sie [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0) mit [Microsoft Bot Framework](https://dev.botframework.com/), um einen Chatbot zu erstellen und bereitzustellen. Verwenden Sie zum Entwerfen und Entwickeln das GUI-Tool [Composer](https://docs.microsoft.com/composer/) oder [funktionierende Botbeispiele](https://github.com/microsoft/BotBuilder-Samples) für die wichtigsten Botszenarien.

## <a name="integrate-with-other-cognitive-services"></a>Integrieren in andere Cognitive Services-Dienste

Weitere Cognitive Services, die mit LUIS verwendet werden:
* [QnA Maker][qnamaker] ermöglicht die Kombination mehrerer Texttypen in einer Wissensdatenbank für Fragen und Antworten.
* Der [Spracherkennungsdienst](../Speech-Service/overview.md) konvertiert Anforderungen in gesprochener Sprache in Text.

LUIS stellt Funktionen der Textanalyse als Teil Ihrer vorhandenen LUIS-Ressourcen bereit. Diese Funktionen umfassen [Stimmungsanalyse](luis-how-to-publish-app.md#configuring-publish-settings) und [Schlüsselbegriffserkennung](luis-reference-prebuilt-keyphrase.md) mit der vorgefertigten Entität „keyPhrase“.

## <a name="learn-with-the-quickstarts"></a>Lernen mit den Schnellstartanleitungen

Erfahren Sie mit praktischen Schnellstarts, in denen das [Portal](get-started-portal-build-app.md) und die [SDK-Clientbibliotheken](azure-sdk-quickstart.md) verwendet werden, mehr über LUIS.


## <a name="next-steps"></a>Nächste Schritte

* [Neuerungen](whats-new.md) beim Dienst und bei der Dokumentation
* [Planen Ihrer App](luis-how-plan-your-app.md) mit [Absichten](luis-concept-intent.md) und [Entitäten](luis-concept-entity-types.md)
* [Abfragen des Vorhersageendpunkts](luis-get-started-get-intent-from-browser.md)
* [Entwicklerressourcen](developer-reference-resource.md) für LUIS.

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
