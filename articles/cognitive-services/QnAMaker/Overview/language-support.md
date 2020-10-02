---
title: 'Sprachunterstützung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Eine Liste der von QnA Maker für Ihre Wissensdatenbank unterstützten Kulturen und natürlichen Sprachen. Mischen Sie Sprachen nicht in derselben Wissensdatenbank.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 09/24/2019
ms.openlocfilehash: c990b6980dea871679b0b301e293e4fb94748db7
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89650897"
---
# <a name="language-support-for-a-qna-maker-resource-and-knowledge-bases"></a>Sprachunterstützung für QnA Maker-Ressourcen und Wissensdatenbanken

Die Sprache für den Dienst ist ausgewählt, wenn Sie die erste Wissensdatenbank in der Ressource erstellen. Alle weiteren Wissensdatenbanken in der Ressource müssen die gleiche Sprache aufweisen. 

Die Sprache bestimmt die Relevanz der Ergebnisse, die QnA Maker als Antwort auf Benutzerabfragen bereitstellt. Die QnA Maker-Ressource und alle Wissensdatenbanken innerhalb dieser Ressource unterstützen nur eine einzelne Sprache. Diese einzelne Sprache ist erforderlich, um bestmögliche Antwortergebnisse für eine Abfrage zu erzielen.

## <a name="single-language-per-resource"></a>Einzelne Sprache pro Ressource

Beachten Sie Folgendes:

* Ein QnA Maker-Dienst und alle seine Wissensdatenbanken unterstützen nur eine Sprache
* Die Sprache wird explizit festgelegt, wenn die erste Wissensdatenbank des Diensts erstellt wird
* Die Sprache wird durch die Dateien und URLs bestimmt, die beim Erstellen der Wissensdatenbank hinzugefügt werden
* Die Sprache kann für andere Wissensdatenbanken im Dienst nicht geändert werden
* Die Sprache wird vom Cognitive Search-Dienst (Bewerter Nr. 1) und dem QnA Maker-Dienst (Bewerter Nr. 2) verwendet, um die beste Antwort auf eine Abfrage zu generieren

## <a name="supporting-multiple-languages"></a>Unterstützung mehrerer Sprachen

Wenn Sie ein Wissensdatenbank-System unterstützen müssen, das mehrere Sprachen umfasst, stehen die folgenden Methoden zur Wahl:

* Verwenden Sie den [Translator-Diensts](../../translator/translator-info-overview.md), um eine Frage in eine einzelne Sprache zu übersetzen, bevor die Frage an Ihre Wissensdatenbank gesendet wird. Dies erlaubt es Ihnen, sich auf die Qualität einer einzelnen Sprache und die Qualität der alternativen Fragen und Antworten zu konzentrieren.
* Erstellen Sie für jede Sprache eine QnA Maker-Ressource und eine Wissensdatenbank innerhalb dieser Ressource. Auf diese Weise können Sie für jede Sprache separate alternative Frage- und Antworttexte mit stärkerer Nuancierung verwenden. Dadurch erhalten Sie eine viel größere Flexibilität, allerdings um den Preis eines deutlich höheren Verwaltungsaufwands, wenn sich die Fragen oder Antworten über die Sprachen hinweg unterscheiden.

Mehr dazu finden Sie bei den [unterstützten Sprachen](../overview/language-support.md) für QnA Maker.

### <a name="support-each-language-with-a-qna-maker-resource"></a>Unterstützen Sie jede Sprache mit einer QnA Maker-Ressource

* Erstellen Sie für jede Sprache eine QnA Maker-Ressource
* Fügen Sie nur Dateien und URLs für diese Sprache hinzu
* Verwenden Sie eine Benennungskonvention zum Identifizieren der Sprache. Ein Beispiel ist `qna-maker-fr` für alle Wissensdatenbanken für französische Dokumente


## <a name="languages-supported"></a>Unterstützte Sprachen

Die folgende Liste enthält die Sprachen, die für QnA Maker-Ressourcen unterstützt werden. 

|Sprache|
|--|
|Arabisch|
|Armenisch|
|Bengalisch|
|Baskisch|
|Bulgarisch|
|Katalanisch|
|Chinesisch (vereinfacht)|
|Chinesisch (traditionell)|
|Kroatisch|
|Tschechisch|
|Dänisch|
|Niederländisch|
|Englisch|
|Estnisch|
|Finnisch|
|Französisch|
|Galizisch|
|Deutsch|
|Griechisch|
|Gujarati|
|Hebräisch|
|Hindi|
|Ungarisch|
|Isländisch|
|Indonesisch|
|Irisch|
|Italienisch|
|Japanisch|
|Kannada|
|Koreanisch|
|Lettisch|
|Litauisch|
|Malayalam|
|Malaiisch|
|Norwegisch|
|Polnisch|
|Portugiesisch|
|Pandschabi|
|Rumänisch|
|Russisch|
|Serbisch (Kyrillisch)|
|Serbisch (Lateinisch)|
|Slowakisch|
|Slowenisch|
|Spanisch|
|Schwedisch|
|Tamilisch|
|Telugu|
|Thailändisch|
|Türkisch|
|Ukrainisch|
|Urdu|
|Vietnamesisch|

## <a name="query-matching-and-relevance"></a>Abfrageabgleich und Relevanz
QnA Maker nutzt [Sprachanalysefunktionen von Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/language-support), um Ergebnisse bereitzustellen.

Während die Azure Cognitive Search-Funktionen für unterstützte Sprachen ebenbürtig sind, verfügt QnA Maker über ein zusätzliches Rangfolgemodul, das oberhalb der Azure-Suchergebnisse ansetzt. In diesem Rangfolgemodul verwenden wir einige besondere semantische und wortbasierte Funktionen in den folgenden Sprachen.

|Sprachen mit zusätzlichem Rangfolgemodul|
|--|
|Chinesisch|
|Tschechisch|
|Niederländisch|
|Englisch|
|Französisch|
|Deutsch|
|Ungarisch|
|Italienisch|
|Japanisch|
|Koreanisch|
|Polnisch|
|Portugiesisch|
|Spanisch|
|Schwedisch|

Diese zusätzliche Rangbewertung ist zählt zu den internen Mechanismen des Rangfolgemoduls von QnA Maker.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Sprachauswahl](../how-to/language-knowledge-base.md)
