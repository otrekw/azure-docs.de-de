---
title: 'Verhindern der Übersetzung von Inhalten: Translator'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit Translator die Übersetzung von Inhalten verhindern. Mit Translator können Sie Inhalte kennzeichnen, die nicht übersetzt werden sollen.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: swmachan
ms.openlocfilehash: 33939976a0824ce8afeb2e6f6fb19e7033098683
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592694"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator"></a>Verhindern der Übersetzung von Inhalten mit Translator

Mit Translator können Sie Inhalte kennzeichnen, die nicht übersetzt werden sollen. Sie können beispielsweise Code, einen Markennamen oder einzelne Wörter/Ausdrücke markieren, deren Übersetzung nicht sinnvoll ist.

## <a name="methods-for-preventing-translation"></a>Methoden zum Verhindern der Übersetzung

1. Kennzeichnen Sie Ihren Inhalt mit `notranslate`. Dies funktioniert prinzipbedingt nur, wenn der Eingabetexttyp auf HTML festgelegt ist.

   Beispiel:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

2. Verwenden Sie das [dynamische Wörterbuch](dynamic-dictionary.md), um eine bestimmte Übersetzung vorzuschreiben.

3. Übergeben Sie die Zeichenfolge nicht zur Übersetzung an Translator.

4. Benutzerdefinierter Translator: Verwenden Sie ein [Wörterbuch in „Benutzerdefinierter Translator“](custom-translator/what-is-dictionary.md), um die Übersetzung eines Ausdrucks mit einer Wahrscheinlichkeit von 100% vorzuschreiben.


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Vermeiden einer Übersetzung in Ihrem Translator-Aufruf](reference/v3-0-translate.md)
