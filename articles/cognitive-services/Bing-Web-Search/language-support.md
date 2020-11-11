---
title: Sprachunterstützung – Bing-Websuche-API
titleSuffix: Azure Cognitive Services
description: Eine Liste der von der Bing-Websuche-API unterstützten natürlichen Sprachen, Länder und Regionen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: b3ce1fdc0b52db2706f81950fff735cf7f95e52b
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94379679"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Sprach- und Regionsunterstützung für die Bing-Websuche-API

> [!WARNING]
> Die APIs der Bing-Suche werden von Cognitive Services auf Bing-Suchdienste umgestellt. Ab dem **30. Oktober 2020** müssen alle neuen Instanzen der Bing-Suche mit dem [hier](https://aka.ms/cogsvcs/bingmove) dokumentierten Prozess bereitgestellt werden.
> APIs der Bing-Suche, die mit Cognitive Services bereitgestellt wurden, werden noch drei Jahre lang bzw. bis zum Ablauf Ihres Enterprise Agreement unterstützt (je nachdem, was zuerst geschieht).
> Eine Anleitung zur Migration finden Sie unter [Bing-Suchdienste](https://aka.ms/cogsvcs/bingmigration).

Die Bing-Websuche-API unterstützt über drei Dutzend Länder oder Regionen, viele davon mit mehreren Sprachen. Die Angabe eines Lands oder einer Region mit einer Abfrage dient dazu, Suchergebnisse auf Grundlage der Interessen des Lands oder der Region zu verfeinern. Die Ergebnisse können Links zu Bing enthalten, und diese Links können die Bing-Benutzeroberfläche entsprechend dem angegebenen Land bzw. der angegebenen Region oder der angegebenen Sprache lokalisieren.

Sie können mit dem Abfrageparameter `cc` ein Land oder eine Region angeben. Bei der Angabe eines Lands oder einer Region müssen Sie mithilfe des [`Accept-Language`-Headers](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#headers) auch mindestens einen Sprachcode angeben. Unter [Märkte](#markets) finden Sie eine Aufstellung der in jedem Markt unterstützten Sprachen.

Alternativ können Sie den Markt mit dem Abfrageparameter `mkt` und einem Code aus der Tabelle **Märkte** angeben. Bei der Angabe eines Markts werden gleichzeitig ein Land oder eine Region und die bevorzugte Sprache angegeben. Mit dem Abfrageparameter `setLang` können Sie die Sprache ausdrücklich festlegen.

## <a name="countriesregions"></a>Länder/Regionen

|Land/Region|Code|
|-------|----|
|Argentinien|AR|
|Australien|AU|
|Österreich|AT|
|Belgien|BE|
|Brasilien|BR|
|Canada|CA|
|Chile|CL|
|Dänemark|DK|
|Finnland|FI|
|Frankreich|BV|
|Deutschland|DE|
|Hongkong (SAR)|HK|
|Indien|IN|
|Indonesien|id|
|Italien|IT|
|Japan|JP|
|Korea|KR|
|Malaysia|MY|
|Mexiko|MX|
|Niederlande|NL|
|Neuseeland|NZ|
|Norwegen|Nein|
|China|CN|
|Polen|PL|
|Portugal|PT|
|Philippinen|PH|
|Russland|RU|
|Saudi-Arabien|SA|
|Südafrika|ZA|
|Spanien|ES|
|Schweden|SE|
|Schweiz|CH|
|Taiwan|TW|
|Türkei|TR|
|United Kingdom|GB|
|USA|US|

## <a name="markets"></a>Märkte

|Land/Region|Sprache|Marktcode|
|-------|--------|-----------|
|Argentinien|Spanisch|es-AR|
|Australien|Englisch|en-AU|
|Österreich|Deutsch|de-AT|
|Belgien|Niederländisch|nl-BE|
|Belgien|Französisch|fr-BE|
|Brasilien|Portugiesisch|pt-BR|
|Canada|Englisch|en-CA|
|Canada|Französisch|fr-CA|
|Chile|Spanisch|es-CL|
|Dänemark|Dänisch|da-DK|
|Finnland|Finnisch|fi-FI|
|Frankreich|Französisch|fr-FR|
|Deutschland|Deutsch|de-DE|
|Hongkong (SAR)|Chinesisch (traditionell)|zh-HK|
|Indien|Englisch|en-IN|
|Indonesien|Englisch|en-ID|
|Italien|Italienisch|it-IT|
|Japan|Japanisch|ja-JP|
|Korea|Koreanisch|ko-KR|
|Malaysia|Englisch|en-MY|
|Mexiko|Spanisch|es-MX|
|Niederlande|Niederländisch|nl-NL|
|Neuseeland|Englisch|en-NZ|
|Norwegen|Norwegisch|no-NO|
|China|Chinesisch|zh-CN|
|Polen|Polnisch|pl-PL|
|Portugal|Portugiesisch|pt-PT|
|Philippinen|Englisch|en-PH|
|Russland|Russisch|ru-RU|
|Saudi-Arabien|Arabisch|ar-SA|
|Südafrika|Englisch|en-ZA|
|Spanien|Spanisch|es-ES|
|Schweden|Schwedisch|sv-SE|
|Schweiz|Französisch|fr-CH|
|Schweiz|Deutsch|de-CH|
|Taiwan|Chinesisch (traditionell)|zh-TW|
|Türkei|Türkisch|tr-TR|
|United Kingdom|Englisch|en-GB|
|USA|Englisch|de-DE|
|USA|Spanisch|es-US|

## <a name="next-steps"></a>Nächste Schritte

* [Referenz zur Bing-Bildersuche-API](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)