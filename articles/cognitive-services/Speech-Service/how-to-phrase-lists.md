---
title: Begriffslisten – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Informieren Sie sich, wie Sie dem Speech-Dienst mithilfe des `PhraseListGrammar`-Objekts eine Begriffsliste zur Verfügung stellen, um die Ergebnisse der Spracherkennung zu verbessern.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 5a21358edae4c61f35993770c22634da9ac83633
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81401966"
---
# <a name="phrase-lists-for-speech-to-text"></a>Begriffslisten für Spracherkennung

Indem Sie dem Speech-Dienst eine Begriffsliste zur Verfügung stellen, können Sie die Genauigkeit der Spracherkennung verbessern. Begriffslisten dienen zur Identifizierung bekannter Begriffe in Audiodaten, wie beispielsweise den Namen einer Person oder einen bestimmten Ort.

Wenn Sie beispielsweise den Befehl „Move to“ und „Ward“ als mögliches Ziel haben, das gesprochen werden kann, können Sie den Eintrag „Move to Ward“ hinzufügen. Das Hinzufügen eines Begriffs erhöht die Wahrscheinlichkeit, dass bei der Audioerkennung „Move to Ward“ anstelle von „Move toward“ erkannt wird.

Einer Begriffsliste können einzelne Wörter oder ganze Phrasen hinzugefügt werden. Bei der Erkennung wird ein Eintrag in einer Begriffsliste verwendet, wenn in der Audiodatei eine genaue Übereinstimmung für den gesamten Ausdruck als gesonderter Ausdruck enthalten ist. Wenn keine genaue Übereinstimmung für den Ausdruck gefunden wird, wird die Erkennung nicht unterstützt.

>[!Note]
> Derzeit unterstützen Begriffslisten nur Englisch für die Spracherkennung.

## <a name="how-to-use-phrase-lists"></a>Verwenden von Begriffslisten

Die folgenden Beispiele veranschaulichen das Erstellen einer Begriffsliste mithilfe des `PhraseListGrammar`-Objekts.

::: zone pivot="programming-language-csharp"

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

::: zone-end

::: zone pivot="programming-language-more"

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

::: zone-end

>[!Note]
> Die maximale Anzahl von Begriffslisten, die der Speech-Dienst zum Abgleich verwenden kann, beträgt 1024 Begriffe.

Sie können die dem `PhraseListGrammar`-Objekt zugeordneten Begriffe auch löschen, indem Sie „clear()“ aufrufen.

::: zone pivot="programming-language-csharp"

```cs
phraseList.Clear();
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
phraselist->Clear();
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
phraseListGrammar.clear();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
phrase_list_grammar.clear()
```

::: zone-end

::: zone pivot="programming-language-more"

```JavaScript
phraseListGrammar.clear();
```

::: zone-end

> [!NOTE]
> Änderungen an einem `PhraseListGrammar`-Objekt werden bei der nächsten Erkennung oder nach einer erneuten Verbindung zum Speech-Dienst wirksam.

## <a name="next-steps"></a>Nächste Schritte

* [Speech SDK-Referenzdokumentation](speech-sdk.md)
