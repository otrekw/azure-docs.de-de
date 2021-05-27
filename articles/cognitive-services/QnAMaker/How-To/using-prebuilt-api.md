---
title: 'QnA Maker: vordefinierte API'
titleSuffix: Azure Cognitive Services
description: Verwenden der vordefinierten API zum Abrufen von Antworten zu Text
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/05/2021
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: 0aa26cc0d78d6ca20dccb97b3936bad840c989b7
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110382545"
---
# <a name="prebuilt-question-answering"></a>Vordefinierte Fragen und Antworten

Das Feature „Vordefinierte Fragen und Antworten“ bietet dem Benutzer die Möglichkeit, Fragen zu einer Textpassage zu beantworten, ohne Wissensdatenbanken erstellen, Frage-Antwort-Paare pflegen oder Gebühren für nicht ausgelastete Infrastruktur zahlen zu müssen. Diese Funktionalität wird als API zur Verfügung gestellt und lässt sich zur Erfüllung von Anforderungen an die Beantwortung von Fragen nutzen, ohne dass Sie sich um die Details von QnA Maker oder zusätzlichem Speicherplatz kümmern müssen.

Bei einer Abfrage des Benutzers und einem Block bzw. einer Passage mit Text gibt die API eine Antwort und (falls verfügbar) eine präzise Antwort zurück. 

<a name="qna-entity"></a>


## <a name="example-usage-of-prebuilt-question-answering"></a>Beispiel für die Verwendung von „Vordefinierte Fragen und Antworten“

Angenommen, Sie haben einen oder mehrere Textblöcke, denen Sie Antworten auf eine bestimmte Frage entnehmen möchten. Üblicherweise müssten Sie so viele Quellen erstellen, wie es Textblöcke gibt. Doch jetzt können Sie mit dem Feature „Vordefinierte Fragen und Antworten“ die Textblöcke abfragen, ohne dass Sie Inhaltsquellen in einer Wissensdatenbank definieren müssen. 

Es folgen einige andere Szenarien für die vordefinierte API:

* Sie entwickeln eine E-Book-Reader-App für Endbenutzer, mit der sie Text hervorheben, eine Frage eingeben und Antworten zu markiertem Text finden können 
* Eine Browsererweiterung, die es Benutzern ermöglicht, eine Frage zum aktuell auf der Browserseite gezeigten Inhalt zu stellen
* Ein Chatbot im Gesundheitswesen, der Abfragen von Benutzern entgegennimmt und Antworten auf Grundlage der medizinischen Inhalte liefert, die der Bot als am relevantesten für die Abfrage des Benutzers ausmacht 

Im Folgenden finden Sie ein Beispiel einer Anforderung:

## <a name="sample-request"></a>Beispiel für eine Anforderung
```
POST https://{Endpoint}/qnamaker/v5.0-preview.2/generateanswer
```

### <a name="sample-query-over-a-single-block-of-text"></a>Beispielabfrage eines einzelnen Textblocks

Anforderungstext

```json
{
    "question": "How long it takes to charge surface pro 4?",
    "documents": [
        {
            "text": "### The basics #### Power and charging It takes two to four hours to charge the Surface Pro 4 battery fully from an empty state. It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it. You can use the USB port on your Surface Pro 4 power supply to charge other devices, like a phone, while your Surface charges.",
            "id": "doc1"
        }
    ],
    "Language": "en"
}
```
## <a name="sample-response"></a>Beispiel für eine Antwort

Im obigen Anforderungstext fragen wir einen einzelnen Textblock ab. Eine Beispielantwort der obigen Abfrage ist unten dargestellt:

```json
{
    "answers": [
        {
            "answer": "### The basics #### Power and charging It takes two to four hours to charge the Surface Pro 4 battery fully from an empty state. It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it. You can use the USB port on your Surface Pro 4 power supply to charge other devices, like a phone, while your Surface charges.",
            "answerSpan": {
                "text": "two to four hours",
                "score": 0.0,
                "startIndex": 47,
                "endIndex": 64
            },
            "score": 0.9599020481109619,
            "id": "doc1",
            "answerStartIndex": 0,
            "answerEndIndex": 390
        },
        {
            "answer": "It can take longer if you’re using your Surface for power-intensive activities like gaming or video streaming while you’re charging it. You can use the USB port on your Surface Pro 4 power supply to charge other devices, like a phone, while your Surface charges.",
            "score": 0.06749606877565384,
            "id": "doc1",
            "answerStartIndex": 129,
            "answerEndIndex": 390
        },
        {
            "answer": "You can use the USB port on your Surface Pro 4 power supply to charge other devices, like a phone, while your Surface charges.",
            "score": 0.011389964260160923,
            "id": "doc1",
            "answerStartIndex": 265,
            "answerEndIndex": 390
        }
    ]
}
```
Wir sehen, dass mehrere Antworten als Teil der API-Antwort empfangen werden. Jede Antwort hat eine bestimmte Zuverlässigkeitsbewertung, die ihnen hilft, die Gesamtrelevanz der Antwort zu verstehen. Benutzer können diese Zuverlässigkeitsbewertung nutzen, um die Antworten auf die Abfrage zu zeigen.

## <a name="prebuilt-api-limits"></a>Grenzwerte für die vordefinierte API 

Konsultieren Sie die Dokumentation zu [Grenzwerten der vordefinierten API](https://docs.microsoft.com/azure/cognitive-services/qnamaker/limits?#prebuilt-question-answering-limits). 

## <a name="prebuilt-api-reference"></a>Referenz zur vordefinierten API
Ziehen Sie die [Referenz zur vordefinierten API](https://docs.microsoft.com/rest/api/cognitiveservices-qnamaker/qnamaker5.0preview2/prebuilt/generateanswer) zu Rate, um die für das Aufrufen der API erforderlichen Ein- und Ausgabeparameter zu verstehen.
