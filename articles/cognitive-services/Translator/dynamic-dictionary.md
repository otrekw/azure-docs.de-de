---
title: 'Dynamisches Wörterbuch: Translator'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie die dynamische Wörterbuchfunktion von Azure Cognitive Services Translator verwenden.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: ebfccb6fed46db65f6a70937516552f5280c8016
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "98897763"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Verwenden eines dynamischen Wörterbuchs

Wenn Ihnen die Übersetzung eines Worts oder eines Ausdrucks bereits bekannt ist, können Sie diese als Markup in der Anforderung angeben. Das dynamische Wörterbuch ist nur für zusammengesetzte Substantive wie Eigennamen und Produktnamen sicher.

**Syntax:**

<mstrans:dictionary translation="translation of phrase">phrase</mstrans:dictionary>

**Anforderungen:**

* Die `From`- und `To`-Sprachen müssen Englisch und eine andere unterstützte Sprache beinhalten. 
* Sie müssen den Parameter `From` in Ihre API-Übersetzungsanforderung einfügen, anstatt die Funktion zur automatischen Erkennung zu verwenden. 

**Beispiel: en-de:**

Quelleingabe: `The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

Zielausgabe: `Das Wort "wordomatic" ist ein Wörterbucheintrag.`

Diese Funktion lässt sich gleichermaßen mit und ohne HTML-Modus ausführen.

Verwenden Sie die Funktion sparsam. Eine bessere Methode zum Anpassen der Übersetzung ist die Verwendung von Benutzerdefinierter Translator. Custom Translator nutzt den Kontext und statistische Wahrscheinlichkeiten in vollem Umfang. Sie erhalten viel bessere Ergebnisse, wenn Sie über Trainingsdaten verfügen oder Trainingsdaten erstellen können, die den Kontext des Worts oder des Ausdrucks zeigen. Weitere Informationen zu Custom Translator finden Sie unter [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).