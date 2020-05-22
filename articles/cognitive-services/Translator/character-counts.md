---
title: 'Zählen von Zeichen: Translator'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Zeichen von Azure Cognitive Services Translator gezählt werden, um besser zu verstehen, wie Inhalte erfasst werden.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 0219940a4ac60e4a6187d13802c36e8bff1925b3
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587322"
---
# <a name="how-the-translator-counts-characters"></a>Zählen von Zeichen durch Translator

Von Translator wird jeder Unicode-Codepunkt des Eingabetexts als Zeichen gezählt. Jede Übersetzung eines Texts in eine Sprache gilt als separate Übersetzung, auch wenn die Anforderung in einem einzigen API-Aufruf zur Übersetzung in mehrere Sprachen gestellt wurde. Die Länge der Antwort spielt keine Rolle.

Folgendes wird gezählt:

* Text, der im Hauptteil der Anforderung an Translator übergeben wird
   * `Text` bei Verwendung der Methoden „Übersetzen“, „Transkribieren“ und „Wörterbuchsuche“
   * `Text` und `Translation` bei Verwendung der Methode „Wörterbuchbeispiele“
* Das gesamte Markup: HTML-, XML-Tags usw. im Textfeld des Anforderungstexts. Die JSON-Notation, die für die Erstellung der Anforderung verwendet wird (z.B. „Text:“), wird nicht gezählt.
* Ein einzelner Buchstabe
* Interpunktion
* Ein Leerzeichen, Tabulator, Markup und alle anderen Arten von Leerzeichen
* Jeder in Unicode definierte Codepunkt
* Eine Übersetzungswiederholung, selbst wenn Sie zuvor den gleichen Text übersetzt haben

Bei Schriftzeichen, die auf Ideogrammen basieren (etwa chinesische und japanische Kanji), wird von Translator ebenfalls die Anzahl von Unicode-Codepunkten gezählt (also jeweils ein Zeichen pro Ideogramm). Ausnahme: Unicode-Ersatzzeichen gelten als zwei Zeichen.

Die Anzahl der Anforderungen, Wörtern, Bytes oder Sätze ist für die Anzahl der Zeichen nicht relevant.

Aufrufe der Methoden Detect und BreakSentence werden nicht in den Zeichenverbrauch eingerechnet. Es wird aber erwartet, dass die Aufrufe der Methoden Detect und BreakSentence in einem angemessenen Verhältnis zur Verwendung anderer zu zählender Funktionen stehen. Wenn die Anzahl der erfolgten Aufrufe von „Detect“ oder „BreakSentence“ die Anzahl der anderen gezählten Methoden um das 100-fache übersteigt, behält sich Microsoft das Recht vor, Ihre Verwendung dieser Methoden einzuschränken.

Weitere Informationen zur Zeichenzählung finden Sie in den [häufig gestellten Fragen zu Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
