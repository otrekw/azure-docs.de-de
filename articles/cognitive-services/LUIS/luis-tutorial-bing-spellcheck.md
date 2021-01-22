---
title: 'Korrigieren falsch geschriebener Wörter: LUIS'
titleSuffix: Azure Cognitive Services
description: Korrigieren Sie falsch geschriebene Wörter in Äußerungen, indem Sie LUIS-Endpunktabfragen die Bing-Rechtschreibprüfungs-API V7 hinzufügen.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 01/12/2021
ms.openlocfilehash: f416fe8ef4f6e89d07e6065d4c9435642d9bacb9
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98179638"
---
# <a name="correct-misspelled-words-with-bing-search-resource"></a>Korrigieren falsch geschriebener Wörter mit einer Bing-Suche-Ressource

Sie können Ihre LUIS-App mit der [Bing-Suche](https://ms.portal.azure.com/#create/Microsoft.BingSearch) integrieren, um falsch geschriebene Wörter in Äußerungen zu korrigieren, bevor der LUIS den Score und die Entitäten dieser Äußerung vorhersagt.

## <a name="create-endpoint-key"></a>Erstellen eines Endpunktschlüssels

Gehen Sie wie folgt vor, um im Azure-Portal eine Bing-Suche-Ressource zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie in der linken oberen Ecke **Ressource erstellen** aus.

3. Geben Sie in das Suchfeld `Bing Search V7` ein, und wählen Sie den Dienst aus.

4. Auf der rechten Seite wird ein Informationsbereich angezeigt, der Informationen sowie die rechtlichen Hinweise enthält. Wählen Sie **Erstellen** aus, um mit der Abonnementerstellung zu beginnen.

    :::image type="content" source="./media/luis-tutorial-bing-spellcheck/bing-search-resource-portal.png" alt-text="Ressource Bing-Rechtschreibprüfungs-API, Version 7":::

5. Geben Sie im nächsten Bereich Ihre Diensteinstellungen an. Warten Sie, bis die Diensterstellung abgeschlossen wurde.

6. Navigieren Sie nach der Erstellung der Ressource zum Blatt **Keys and Endpoint** (Schlüssel und Endpunkt) auf der linken Seite. 

7. Kopieren Sie einen der Schlüssel, die dem Header Ihrer Vorhersageanforderung hinzugefügt werden sollen. Sie benötigen nur einen der beiden Schlüssel.

8. Fügen Sie den Schlüssel im Vorhersageanforderungsheader zu `mkt-bing-spell-check-key` hinzu.

<!--
## Using the key in LUIS test panel
There are two places in LUIS to use the key. The first is in the [test panel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key isn't saved into LUIS but instead is a session variable. You need to set the key every time you want the test panel to apply the Bing Spell Check API v7 service to the utterance. See [instructions](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) in the test panel for setting the key.
-->
## <a name="adding-the-key-to-the-endpoint-url"></a>Hinzufügen des Schlüssels zur Endpunkt-URL
Für jede Abfrage, auf die Sie die Rechtschreibprüfung anwenden möchten, muss der Endpunktabfrage der Schlüssel für die Bing-Rechtschreibprüfungs-Ressource im Abfrageheaderparameter übergeben werden. Ihr Chatbot könnte beispielsweise LUIS aufrufen, oder Sie rufen die LUIS-Endpunkt-API direkt auf. Unabhängig davon, wie der Endpunkt aufgerufen wird, muss jeder Aufruf die erforderlichen Informationen im Anforderungsheader enthalten, damit die Rechtschreibprüfung ordnungsgemäß funktioniert. Sie müssen den Wert mit **mkt-bing-spell-check-key** auf den Schlüsselwert festlegen.


## <a name="send-misspelled-utterance-to-luis"></a>Übermitteln falsch geschriebener Äußerungen an LUIS
1. Fügen Sie in der Vorhersageabfrage, die Sie senden, eine falsch geschriebene Äußerung hinzu, z. B. „How far is the mountainn?“. Im Englischen ist `mountain` mit einem `n` die richtige Schreibweise.

2. LUIS antwortet mit einem JSON-Ergebnis für `How far is the mountain?`. Wenn die Bing-Rechtschreibprüfungs-API v7 einen Rechtschreibfehler erkennt, enthält das Feld `query` in der JSON-Antwort der LUIS-App die ursprüngliche Abfrage und das Feld `alteredQuery` die korrigierte, an LUIS gesendete Abfrage.

```json
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Ignorieren von Rechtschreibfehlern

Wenn Sie den Dienst Bing-Suche-API, Version 7, nicht nutzen möchten, müssen Sie die richtige und falsche Schreibung hinzufügen.

Zwei Lösungen sind:

* Kennzeichnen Sie Beispieläußerungen, welche die verschiedenen Schreibweisen enthalten, damit LUIS sowohl die richtige Rechtschreibung als auch Tippfehler lernen kann. Diese Option erfordert einen höheren Aufwand für die Bezeichnung als die Verwendung einer Rechtschreibprüfung.
* Erstellen Sie eine Phrasenliste mit allen Wortvariationen. Mit dieser Lösung müssen Sie die Wortvariationen in den Beispieläußerungen nicht kennzeichnen.


> [!div class="nextstepaction"]
> [Weitere Informationen zu Beispieläußerungen](./luis-how-to-add-entities.md)
