---
title: Anforderungslimits – Textübersetzungs-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel werden die Anforderungslimits für die Textübersetzungs-API aufgeführt. Die Gebühren werden basierend auf der Anzahl der Zeichen berechnet, nicht nach der Anforderungshäufigkeit, mit einem Grenzwert von 5.000 Zeichen pro Anforderung. Die Zeichengrenzwerte sind abonnementbasiert, wobei F0 auf 2 Millionen Zeichen pro Stunde beschränkt ist.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: swmachan
ms.openlocfilehash: 8d26efec2783d6f121c319e46b1b505b6e1b1e09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79498935"
---
# <a name="request-limits-for-translator-text"></a>Anforderungslimits für die Textübersetzungs-API

In diesem Artikel werden die Drosselungslimits für die Textübersetzungs-API aufgeführt. Zu den Diensten gehören Übersetzung, Transliteration, Satzlängenerkennung, Spracherkennung und alternative Übersetzungen.

## <a name="character-and-array-limits-per-request"></a>Zeichen- und Arraygrenzwerte pro Anforderung

Jede Übersetzungsanforderung ist auf 5.000 Zeichen in allen von Ihnen verwendeten Zielsprachen beschränkt. Beispiel: Das Senden einer Übersetzungsanforderung von 1.500 Zeichen für die Übersetzung in drei verschiedene Sprachen ergibt eine Anforderungsgröße von 1.500 · 3 = 4.500 Zeichen. Dies entspricht dem Anforderungsgrenzwert. Die Abrechnung erfolgt nach der Anzahl der Zeichen, nicht nach der Anzahl der Anforderungen. Es wird empfohlen, kürzere Anforderungen zu senden.

In der folgenden Tabelle sind Arrayelement- und Zeichengrenzwerte für jeden Vorgang der Textübersetzungs-API aufgeführt.

| Vorgang | Maximale Größe des Arrayelements |   Maximale Anzahl von Arrayelementen |  Maximale Anforderungsgröße (Zeichen) |
|:----|:----|:----|:----|
| Translate | 5\.000 | 100   | 5\.000 |
| Transliterate | 5\.000 | 10    | 5\.000 |
| Detect | 10.000 | 100 |   50.000 |
| BreakSentence | 10.000    | 100 | 50.000 |
| Wörterbuchsuche| 100 |  10  | 1\.000 |
| Wörterbuchbeispiele | 100 für Text und 100 für Übersetzung (200 insgesamt)| 10|   2\.000 |

## <a name="character-limits-per-hour"></a>Zeichengrenzwerte pro Stunde

Ihre Zeichengrenzwerte pro Stunde basieren auf der Abonnementebene der Textübersetzungs-API. 

Das Stundenkontingent sollte gleichmäßig über die gesamte Stunde verbraucht werden. Beispielsweise sollten Zeichen beim Grenzwert für den F0-Tarif von 2 Millionen Zeichen pro Stunde nicht schneller als mit ungefähr 33.300 Zeichen pro Minute gleitendes Fenster (2 Millionen, dividiert durch 60 Minuten) verbraucht werden.

Wenn Sie diese Grenzwerte erreichen oder überschreiten oder einen zu großen Teil des Kontingents in kurzer Zeit senden, werden Sie wahrscheinlich eine Antwort zur Kontingentüberschreitung erhalten. Es gibt keine Beschränkungen für gleichzeitige Anforderungen.

| Tarif | Zeichengrenzwert |
|------|-----------------|
| F0 | 2 Millionen Zeichen pro Stunde |
| S1 | 40 Millionen Zeichen pro Stunde |
| S2/C2 | 40 Millionen Zeichen pro Stunde |
| S3/C3 | 120 Millionen Zeichen pro Stunde |
| S4/C4 | 200 Millionen Zeichen pro Stunde |

Die Grenzwerte für [Abonnements mit mehreren Diensten](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) sind dieselben wie für den S1-Tarif.

Diese Grenzwerte sind auf die Standardübersetzungsmodelle von Microsoft beschränkt. Für benutzerdefinierte Übersetzungsmodelle, die den benutzerdefinierten Translator verwenden, gilt ein Grenzwert von 1.800 Zeichen pro Sekunde.

## <a name="latency"></a>Latency

Die maximale Latenz der Textübersetzungs-API beträgt bei Verwendung von Standardmodellen 15 Sekunden und bei Verwendung von benutzerdefinierten Modellen 120 Sekunden. In der Regel werden die Antworten *für Text unter 100 Zeichen* innerhalb von 150 bis 300 Millisekunden zurückgegeben. Die benutzerdefinierten Übersetzungsmodelle weisen ähnliche Latenzmerkmale für dauerhafte Anforderungsraten auf. Die Latenz kann aber höher sein, wenn Ihre Anforderungsrate unterbrochen wird. Die Antwortzeiten variieren je nach der Größe der Anforderung und dem Sprachpaar. Wenn Sie innerhalb dieses Zeitrahmens keine Übersetzung oder eine [Fehlerantwort](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) erhalten, überprüfen Sie Ihren Code und die Netzwerkverbindung, und versuchen Sie es nochmal. 

## <a name="sentence-length-limits"></a>Grenzwerte bei der Satzlänge

Bei Verwendung der [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence)-Funktion ist die Satzlänge auf 275 Zeichen beschränkt. Es gelten Ausnahmen für diese Sprachen:

| Sprache | Code | Zeichengrenzwert |
|----------|------|-----------------|
| Chinesisch | zh | 132 |
| Deutsch | de | 290 |
| Italienisch | it | 280 |
| Japanisch | ja | 150 |
| Portugiesisch | pt | 290 |
| Spanisch | es | 280 |
| Italienisch | it | 280 |
| Thailändisch | th | 258 |

> [!NOTE]
> Dieser Grenzwert gilt nicht für Übersetzungen.

## <a name="next-steps"></a>Nächste Schritte

* [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Regionale Verfügbarkeit](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [Referenz für Version 3 der Textübersetzungs-API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
