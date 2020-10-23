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
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: ec39b3692a90f22409e85b5502d3ea874e3282d6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91742059"
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

2. Kennzeichnen Sie Ihren Inhalt mit `translate="no"`. Dies funktioniert nur, wenn textType für die Eingabe auf HTML festgelegt ist.

   Beispiel:

   ```html
   <span translate="no">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div translate="no">This will not be translated.</div>
   <div>This will be translated. </div>

3. Use the [dynamic dictionary](dynamic-dictionary.md) to prescribe a specific translation.

4. Don't pass the string to the Translator for translation.

5. Custom Translator: Use a [dictionary in Custom Translator](custom-translator/what-is-dictionary.md) to prescribe the translation of a phrase with 100% probability.


## Next steps
> [!div class="nextstepaction"]
> [Use the Translate operation to translate text](reference/v3-0-translate.md)
