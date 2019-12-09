---
title: Verhindern der Inhaltsübersetzung – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: Verhindern Sie mit der Textübersetzungs-API die Übersetzung von Inhalten. Die Textübersetzungs-API ermöglicht es, Inhalte so zu kennzeichnen, dass sie nicht übersetzt werden.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: swmachan
ms.openlocfilehash: 15a36451c18d65df6667f24284f3f69f3d1c06b8
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326758"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Verhindern der Inhaltsübersetzung mit der Textübersetzungs-API

Die Textübersetzungs-API ermöglicht es, Inhalte so zu kennzeichnen, dass sie nicht übersetzt werden. Sie können beispielsweise Code, einen Markennamen oder einzelne Wörter/Ausdrücke markieren, deren Übersetzung nicht sinnvoll ist.

## <a name="methods-for-preventing-translation"></a>Methoden zum Verhindern der Übersetzung
1. Verwenden Sie als Escapezeichen ein Twitter-Tag wie @somethingtopassthrough oder #somethingtopassthrough. Entfernen Sie das Escapezeichen nach der Übersetzung. Dies ist der reguläre Ausdruck für gültige Twitter-Tags: `\B@[A-Za-z]+[A-Za-z0-9_]+)`. Ein Tag sollte mit einem @-Zeichen beginnen, gefolgt von einem Buchstaben, dem wiederum weitere Buchstaben, Ziffern oder Unterstriche folgen. Es empfiehlt sich, Tags kurz zu halten; dem führenden Tag muss ein Leerzeichen vorangestellt sein.

2. Kennzeichnen Sie Ihren Inhalt mit `notranslate`. Dies funktioniert prinzipbedingt nur, wenn der Eingabetexttyp auf HTML festgelegt ist.

   Beispiel:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Verwenden Sie das [dynamische Wörterbuch](dynamic-dictionary.md), um eine bestimmte Übersetzung vorzuschreiben.

4. Übergeben Sie die Zeichenfolge nicht zur Übersetzung an die Textübersetzungs-API.

5. Benutzerdefinierter Translator: Verwenden Sie ein [Wörterbuch in „Benutzerdefinierter Translator“](custom-translator/what-is-dictionary.md), um die Übersetzung eines Ausdrucks mit einer Wahrscheinlichkeit von 100% vorzuschreiben.


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Vermeiden einer Übersetzung in Ihrem Textübersetzungs-API-Aufruf](reference/v3-0-translate.md)
