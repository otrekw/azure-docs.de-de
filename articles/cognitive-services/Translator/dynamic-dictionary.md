---
title: Dynamisches Wörterbuch – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Sie die dynamische Wörterbuchfunktion der Textübersetzungs-API von Azure Cognitive Services verwenden.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 5f007660b98a22ff9ffad072991a39cf59518b6a
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73837355"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Verwenden eines dynamischen Wörterbuchs

Wenn Ihnen die Übersetzung eines Worts oder eines Ausdrucks bereits bekannt ist, können Sie diese als Markup in der Anforderung angeben. Das dynamische Wörterbuch ist nur für zusammengesetzte Substantive wie Eigennamen und Produktnamen sicher.

**Syntax:**

<mstrans:dictionary translation="translation of phrase">phrase</mstrans:dictionary>

**Anforderungen:**

* Die Sprachen `From` und `To` müssen unterschiedlich sein. 
* Sie müssen den Parameter `From` in Ihre API-Übersetzungsanforderung einfügen, anstatt die Funktion zur automatischen Erkennung zu verwenden. 

**Beispiel: en-de:**

Quelleingabe: The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

Zielausgabe: Das Wort „wordomatic“ ist ein Wörterbucheintrag.

Diese Funktion lässt sich gleichermaßen mit und ohne HTML-Modus ausführen.

Sie sollte in Maßen eingesetzt werden. Für die Anpassung einer Übersetzung ist Custom Translator deutlich besser geeignet. Custom Translator nutzt den Kontext und statistische Wahrscheinlichkeiten in vollem Umfang. Sie erhalten viel bessere Ergebnisse, wenn Sie über Trainingsdaten verfügen oder Trainingsdaten erstellen können, die den Kontext des Worts oder des Ausdrucks zeigen. Weitere Informationen zu Custom Translator finden Sie unter [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).
