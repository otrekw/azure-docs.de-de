---
title: 'Integration in andere Anwendungen: QnA Maker'
description: QnA Maker lässt sich in Clientanwendungen wie etwa Chatbots sowie in andere Dienste zur natürlichsprachlichen Verarbeitung wie LUIS (Language Understanding) integrieren.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 38b275aa2b8cf5768a2bc95634a7ff4892893eda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300518"
---
# <a name="design-knowledge-base-for-client-applications"></a>Entwerfen einer Wissensdatenbank für Clientanwendungen

QnA Maker lässt sich in Clientanwendungen wie etwa Chatbots sowie in andere Dienste zur natürlichsprachlichen Verarbeitung wie LUIS (Language Understanding) integrieren.

## <a name="integration-with-a-conversational-client"></a>Integration in einen Unterhaltungsclient

QnA Maker kann in unterhaltungsbasierte Clientanwendungen wie [Microsoft Bot Framework](https://dev.botframework.com/) integriert werden. Der an QnA Maker gesendete Text muss nicht bereinigt oder transformiert werden. QnA Maker akzeptiert natürliche Sprachen und gibt die beste Antwort zurück.

## <a name="create-a-bot-without-writing-any-code"></a>Erstellen Sie einen Bot, ohne Code zu schreiben

Erstellen Sie nach dem Veröffentlichen Ihrer Wissensdatenbank auf der Seite **Veröffentlichen** einen Bot, indem Sie die Schaltfläche **Bot erstellen** auswählen. Verwenden Sie das [Bot-Tutorial](../Quickstarts/create-publish-knowledge-base.md), um zu erfahren, was nach dem Auswählen der Schaltfläche geschieht.

## <a name="providing-multi-turn-conversations"></a>Bereitstellen von Unterhaltungen mit mehreren Durchgängen

Ein Bot stellt die optimal ausgewählte Antwort aus Ihrer Wissensdatenbank zur Verfügung und kann Folgeaufforderungen bereitstellen, falls es sich bei der Antwort um einen QnA-Satz in mehreren Durchgängen handelt. Erfahren Sie, [wie](../how-to/multiturn-conversation.md) Sie Ihrer Wissensdatenbank Frage-Antwort-Sätze mit mehreren Durchgängen hinzufügen.

## <a name="natural-language-processing"></a>Verarbeitung natürlicher Sprache

Während QnA Maker Fragen verarbeitet, die die Verarbeitung natürlicher Sprache verwenden, kann es auch als Teil eines größeren Systems verwendet werden, das Fragen aus mehreren Wissensdatenbanken beantwortet. Sie können QnA Maker mit einem anderen Cognitive Service kombinieren, Language Understanding (LUIS), um natürlichsprachliche Verarbeitung bereitzustellen, noch bevor eine bestimmte Wissensdatenbank erreicht wurde. Erfahren Sie mehr darüber, wann und wie Sie [LUIS und QnA Maker](../../luis/choose-natural-language-processing-service.md?toc=/azure/cognitive-services/qnamaker/toc.json) zusammen verwenden können.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie [Konzepte](development-lifecycle-knowledge-base.md) für den Entwicklungszyklus für QnA Maker kennen.