---
title: Sprachunterstützung – Textanalyse-API
titleSuffix: Azure Cognitive Services
description: Eine Liste der von der Textanalyse-API unterstützten natürlichen Sprachen. In diesem Artikel wird beschrieben, welche Sprachen für die einzelnen Vorgänge unterstützt werden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: aahi
ms.openlocfilehash: 4fd893691f68c8b51fa3ef234d7828b1bd70cea5
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2020
ms.locfileid: "84465591"
---
# <a name="text-analytics-api-v3-language-support"></a>Textanalyse-API v3: Sprachunterstützung 

> [!IMPORTANT]
> Version 3.x der Textanalyse-API ist derzeit in den folgenden Regionen nicht verfügbar: Indien, Mitte; VAE, Norden; China, Norden 2; China, Osten.


#### <a name="sentiment-analysis"></a>[Standpunktanalyse](#tab/sentiment-analysis)

| Sprache              | Sprachcode | v2-Unterstützung | v3-Unterstützung | Ab Modellversion 3: |              Notizen |
|:----------------------|:-------------:|:----------:|:----------:|:--------------------------:|-------------------:|
| Chinesisch (vereinfacht)    |   `zh-hans`   |     ✓      |     ✓      |         2019-10-01         | `zh` wird ebenfalls akzeptiert. |
| Chinesisch (traditionell)   |   `zh-hant`   |            |     ✓      |         2019-10-01         |                    |
| Dänisch               |     `da`      |     ✓      |            |                            |                    |
| Niederländisch                 |     `nl`      |     ✓      |            |                            |                    |
| Englisch               |     `en`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Finnisch               |     `fi`      |     ✓      |            |                            |                    |
| Französisch                |     `fr`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Deutsch                |     `de`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Griechisch                 |     `el`      |     ✓      |            |                            |                    |
| Italienisch               |     `it`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Japanisch              |     `ja`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Koreanisch                |     `ko`      |            |     ✓      |         2019-10-01         |                    |
| Norwegisch (Bokmål)   |     `no`      |     ✓      |            |                            |                    |
| Polnisch                |     `pl`      |     ✓      |            |                            |                    |
| Portugiesisch (Portugal) |    `pt-PT`    |     ✓      |     ✓      |         2019-10-01         | `pt` wird ebenfalls akzeptiert. |
| Russisch               |     `ru`      |     ✓      |            |                            |                    |
| Spanisch               |     `es`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Schwedisch               |     `sv`      |     ✓      |            |                            |                    |
| Türkisch               |     `tr`      |     ✓      |            |                            |                    |

### <a name="opinion-mining-v31-preview-only"></a>Opinion Mining (nur v3.1-preview)

| Sprache              | Sprachcode | Ab Modellversion 3: |              Notizen |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| Englisch               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Erkennung benannter Entitäten (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * Für NER v3 wird derzeit nur Englisch unterstützt. Wenn Sie NER v3 mit einer anderen Sprache aufrufen, gibt die API Ergebnisse für v2.1 zurück, sofern die Sprache in Version 2.1 unterstützt wird.
> * Bei v2.1 wird der vollständige Satz mit den verfügbaren Entitäten nur für die Sprachen Englisch, Chinesisch (Vereinfacht), Französisch, Deutsch und Spanisch zurückgegeben.  Für die anderen unterstützten Sprachen werden die Entitäten für „Person“, „Standort“ und „Organisation“ zurückgegeben.

| Sprache               | Sprachcode | v2.1-Unterstützung | v3-Unterstützung | Ab Modellversion 3: |       Notizen        |
|:-----------------------|:-------------:|:----------:|:----------:|:-------------------------------:|:------------------:|
| Arabisch                |     `ar`      |     ✓      |            |                                 |                    |
| Tschechisch                 |     `cs`      |     ✓      |            |                                 |                    |
| Chinesisch (vereinfacht)     |   `zh-hans`   |     ✓      |            |                                 | `zh` wird ebenfalls akzeptiert. |
| Chinesisch (traditionell)   |   `zh-hant`   |     ✓      |            |                                 |                    |
| Dänisch                |     `da`      |     ✓      |            |                                 |                    |
| Niederländisch                 |     `nl`      |     ✓      |            |                                 |                    |
| Englisch                |     `en`      |     ✓      |     ✓      |           2019-10-01            |                    |
| Finnisch               |     `fi`      |     ✓      |            |                                 |                    |
| Französisch                 |     `fr`      |     ✓      |            |                                 |                    |
| Deutsch                 |     `de`      |     ✓      |            |                                 |                    |
| Hebräisch                |     `he`      |     ✓      |            |                                 |                    |
| Ungarisch             |     `hu`      |     ✓      |            |                                 |                    |
| Italienisch               |     `it`      |     ✓      |            |                                 |                    |
| Japanisch              |     `ja`      |     ✓      |            |                                 |                    |
| Koreanisch                |     `ko`      |     ✓      |            |                                 |                    |
| Norwegisch (Bokmål)   |     `no`      |     ✓      |            |                                 | `nb` wird ebenfalls akzeptiert. |
| Polnisch                |     `pl`      |     ✓      |            |                                 |                    |
| Portugiesisch (Portugal) |    `pt-PT`    |     ✓      |            |                                 | `pt` wird ebenfalls akzeptiert. |
| Portugiesisch (Brasilien)   |    `pt-BR`    |     ✓      |            |                                 |                    |
| Russisch              |     `ru`      |     ✓      |            |                                 |                    |
| Spanisch               |     `es`      |     ✓      |            |                                 |                    |
| Schwedisch               |     `sv`      |     ✓      |            |                                 |                    |
| Türkisch               |     `tr`      |     ✓      |            |                                 |                    |

#### <a name="key-phrase-extraction"></a>[Schlüsselbegriffserkennung](#tab/key-phrase-extraction)

| Sprache              | Sprachcode | v2-Unterstützung | v3-Unterstützung | Ab Modellversion 3 verfügbar: |       Notizen        |
|:----------------------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:------------------:|
| Niederländisch                 |     `nl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Englisch               |     `en`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Finnisch               |     `fi`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Französisch                |     `fr`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Deutsch                |     `de`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Italienisch               |     `it`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Japanisch              |     `ja`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Koreanisch                |     `ko`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Norwegisch (Bokmål)   |     `no`      |     ✓      |     ✓      |                2019-10-01                 | `nb` wird ebenfalls akzeptiert. |
| Polnisch                |     `pl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Portugiesisch (Portugal) |    `pt-PT`    |     ✓      |     ✓      |                2019-10-01                 | `pt` wird ebenfalls akzeptiert. |
| Portugiesisch (Brasilien)   |    `pt-BR`    |     ✓      |     ✓      |                2019-10-01                 |                    |
| Russisch               |     `ru`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Spanisch               |     `es`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Schwedisch               |     `sv`      |     ✓      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[Entitätsverknüpfung](#tab/entity-linking)

| Sprache | Sprachcode | v2-Unterstützung | v3-Unterstützung | Ab Modellversion 3 verfügbar: | Notizen |
|:---------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:-----:|
| Englisch  |     `en`      |     ✓      |     ✓      |                2019-10-01                 |       |
| Spanisch  |     `es`      |     ✓      |     ✓      |                2019-10-01                 |       |

#### <a name="language-detection"></a>[Sprachenerkennung](#tab/language-detection)

Die Textanalyse-API kann eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen erkennen.  Die Spracherkennung gibt das „Skript“ einer Sprache zurück. Für den englischen Satz „I have a dog“ wird beispielsweise `en` anstelle von `en-US` zurückgegeben. Der einzige Sonderfall tritt im Chinesischen auf. Für diese Sprache gibt die Spracherkennungsfunktion `zh_CHS` oder `zh_CHT` zurück, wenn sie das Skript anhand des verfügbaren Texts ermitteln kann. In Situationen, in denen kein bestimmtes Skript für ein Dokument auf Chinesisch ermittelt werden kann, wird nur `zh` zurückgegeben.

Für dieses Feature wird keine genaue Liste mit Sprachen veröffentlicht. Es kann jedoch eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen erkennen. 

Bei Inhalten in einer seltener verwendeten Sprache können Sie die Sprachenerkennung ausprobieren, um zu sehen, ob sie einen Code zurückgibt. Die Antwort bei Sprachen, die nicht erkannt werden können, lautet `unknown`.

---

## <a name="see-also"></a>Weitere Informationen

* [Worum handelt es sich bei der Textanalyse-API?](overview.md)   
