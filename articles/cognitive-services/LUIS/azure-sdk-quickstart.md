---
title: 'Schnellstart: LUIS-SDK-Clientbibliotheken (Language Understanding)'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit den LUIS-SDK-Clientbibiliotheken eine LUIS-App mithilfe von C#, Python oder JavaScript abfragen.
ms.topic: quickstart
ms.date: 09/01/2020
keywords: Azure, künstliche Intelligenz, KI, Verarbeitung natürlicher Sprache, NLP, Verstehen natürlicher Sprache, NLU, KI-Konversation, Konversations-KI, KI Chatbot, Chatbot Maker, LUIS, NLP KI, LUIS KI, Azure LUIS, natürliche Sprache verstehen
ms.custom: devx-track-python, devx-track-javascript, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-set-diberry-3core
ms.openlocfilehash: 6bcdca85125d44475fadfd195c1dfda88f761f88
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89323087"
---
# <a name="quickstart-language-understanding-luis-sdk-client-libraries-to-create-and-query-your-luis-app"></a>Schnellstart: LUIS-SDK-Clientbibliotheken (Language Understanding) zum Erstellen und Abfragen Ihrer LUIS-App

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit den LUIS-SDK-Clientbibiliotheken eine LUIS-App mithilfe von C#, Python oder JavaScript abfragen.

Language Understanding (LUIS) ermöglicht das Anwenden benutzerdefinierter Machine Learning-Intelligenz auf Benutzertext mit natürlicher Sprache, um die allgemeine Bedeutung vorherzusagen sowie relevante, detaillierte Informationen zu extrahieren.

* Mit der Clientbibliothek des **Authoring SDK** können Sie Ihre LUIS-App erstellen, bearbeiten, trainieren und veröffentlichen. * Mit der Clientbibliothek des **Prediction Runtime SDK** können Sie die veröffentlichte App abfragen.

::: zone pivot="programming-language-csharp"
[!INCLUDE [LUIS development with C# SDK](./includes/sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [LUIS development with Node.js SDK](./includes/sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [LUIS development with Python SDK](./includes/sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die App über das [LUIS-Portal](https://www.luis.ai) und die Azure-Ressourcen über das [Azure-Portal](https://portal.azure.com/) löschen.

## <a name="troubleshooting"></a>Problembehandlung

* Authentifizieren bei der Clientbibliothek: Authentifizierungsfehler weisen in der Regel darauf hin, dass der falsche Schlüssel und Endpunkt verwendet wurden. In dieser Schnellstartanleitung werden zur Vereinfachung der Erstellungsschlüssel und der Endpunkt für die Vorhersagelaufzeit verwendet. Dies ist jedoch nur möglich, wenn Sie das monatliche Kontingent noch nicht aufgebraucht haben. Wenn Sie den Erstellungsschlüssel und den Endpunkt nicht verwenden können, müssen Sie beim Zugriff auf die Clientbibliothek des Prediction Runtime SDK den Schlüssel und den Endpunkt der Vorhersagelaufzeit verwenden.
* Erstellen von Entitäten: Wenn beim Erstellen der in diesem Tutorial verwendeten geschachtelten Machine Learning-Entität ein Fehler auftritt, stellen Sie sicher, dass Sie den Code kopiert und nicht zum Erstellen einer anderen Entität geändert haben.
* Erstellen von Beispieläußerungen: Wenn beim Erstellen der in diesem Tutorial verwendeten bezeichneten Beispieläußerung ein Fehler auftritt, stellen Sie sicher, dass Sie den Code kopiert und nicht zum Erstellen eines anderen bezeichneten Beispiels geändert haben.
* Trainieren: Wenn ein Trainingsfehler angezeigt wird, weist dies normalerweise auf eine leere App (keine Absichten mit Beispieläußerungen) oder eine App mit falsch formatierten Absichten oder Entitäten hin.
* Sonstige Fehler: Da der Code die Clientbibliotheken mit Text- und JSON-Objekten aufruft, vergewissern Sie sich, dass der Code nicht geändert wurde.

Andere Fehler: Wenn ein Fehler auftritt, der in der obigen Liste nicht behandelt wird, teilen Sie uns dies unten auf dieser Seite über den Feedbackbereich mit. Geben Sie die Programmiersprache und die Version der Clientbibliotheken an, die Sie installiert haben. 

## <a name="next-steps"></a>Nächste Schritte

* [Worum handelt es sich bei Language Understanding (LUIS)?](what-is-luis.md)
* [Neuigkeiten](whats-new.md)
* [Absichten](luis-concept-intent.md), [Entitäten](luis-concept-entity-types.md), [Beispieläußerungen](luis-concept-utterance.md) und [vordefinierte Entitäten](luis-reference-prebuilt-entities.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code).
