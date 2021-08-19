---
title: Verbessern der Antwortqualität mit Synonymen
description: In diesem Tutorial erfahren Sie, wie Sie Antworten mit Synonymen und alternativen Wörtern verbessern.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/29/2021
ms.openlocfilehash: 9830eab526ad1cad4f7eb45d010958545ccec81b
ms.sourcegitcommit: 285d5c48a03fcda7c27828236edb079f39aaaebf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113235240"
---
# <a name="improve-quality-of-response-with-synonyms"></a>Verbessern der Antwortqualität mit Synonymen

In diesem Tutorial erfahren Sie, wie Sie die Qualität Ihrer Antworten mithilfe von Synonymen verbessern können. Angenommen, Benutzer erhalten keine genaue Antwort auf ihre Fragen, wenn sie Varianten, Synonyme oder Akronyme eines Worts verwenden. Über die [Änderungs-API](/rest/api/cognitiveservices-qnamaker/QnAMaker4.0/Alterations) sollen nun Synonyme für Schlüsselwörter hinzugefügt werden, um die Antwortqualität zu verbessern.

## <a name="add-synonyms-using-alterations-api"></a>Hinzufügen von Synonymen über die Änderungs-API

Fügen wir die folgenden Wörter und alternative Begriffe hinzu, um die Ergebnisse zu verbessern:

|Word | Alternative Begriffe|
|--------------|--------------------------------|
| fix problems (Probleme behandeln) | `Troubleshoot`, `trouble-shoot`|
| whiteboard   | `white-board`, `white board`   |
| Bluetooth    | `blue-tooth`, `blue tooth`     |

```json
{
    "wordAlterations": [
        {
            "alterations": [
                "fix problems",
                "trouble shoot",
                "trouble-shoot",
                ]
        },
        {
            "alterations": [
                "whiteboard",
                "white-board",
                "white board"
            ]
        },
        {
            "alterations": [
                "bluetooth",
                "blue-tooth",
                "blue tooth"
            ]
        }
    ]
}

```

## <a name="response-after-adding-synonyms"></a>Antwort nach dem Hinzufügen von Synonymen

Für das unten gezeigte Frage-Antwort-Paar „Fix problems with Surface Pen“ (Probleme mit Surface Pen behandeln) werden nun die Antworten verglichen, wenn anstelle von „fix problems“ die englische Alternative „trouble shoot“ verwendet wird.

> [!div class="mx-imgBorder"]
> [ ![Screenshot: „Fix problems with Surface Pen“ (Behandeln von Problemen mit Surface Pen) rot hervorgehoben]( ../media/adding-synonyms/fix-problems.png) ]( ../media/adding-synonyms/fix-problems.png#lightbox)

## <a name="response-before-addition-of-synonym"></a>Antwort vor dem Hinzufügen des Synonyms

> [!div class="mx-imgBorder"]
> [ ![Screenshot mit rot hervorgehobener Zuverlässigkeitsbewertung von 71,82]( ../media/adding-synonyms/confidence-score.png) ]( ../media/adding-synonyms/confidence-score.png#lightbox)

## <a name="response-after-addition-of-synonym"></a>Antwort nach dem Hinzufügen des Synonyms 

> [!div class="mx-imgBorder"]
> [ ![Screenshot mit rot hervorgehobener Zuverlässigkeitsbewertung von 97]( ../media/adding-synonyms/increase-score.png) ]( ../media/adding-synonyms/increase-score.png#lightbox)

Wie Sie sehen, haben wir, bevor `trouble shoot` als Synonym hinzugefügt wurde, eine Antwort mit geringer Zuverlässigkeitsbewertung auf die Frage „How to troubleshoot your surface pen“ erhalten. Nachdem `trouble shoot` als Synonym für „fix problems“ hinzugefügt wurde, wurde die richtige Antwort auf die Frage mit einer höheren Zuverlässigkeitsbewertung zurückgegeben. Nach dem Hinzufügen dieser alternativen Begriffe verbesserte sich die Relevanz der Ergebnisse und damit die Benutzerfreundlichkeit. 

> [!NOTE]
> Bei Synonymen wird die Groß-/Kleinschreibung nicht berücksichtigt. Synonyme funktionieren möglicherweise nicht wie erwartet, wenn Sie Stoppwörter als Synonyme hinzufügen. Die Liste der Stoppwörter finden Sie hier: [Liste der Stoppwörter](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/STOPWORDS.md).

Beispiel: In der englischen Sprache ist **it** (es) ein Stoppwort. Wenn in dieser Sprache die Abkürzung **IT** für Informationstechnologie hinzugefügt wird, erkennt das System den Begriff möglicherweise nicht, da „it“ ein Stoppwort ist, das bei der Verarbeitung der Abfrage herausgefiltert wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen von Wissensdatenbanken in mehreren Sprachen](multiple-languages.md)