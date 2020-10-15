---
title: 'Speech-Dienst: Verbessern eines Modells für Custom Speech'
titleSuffix: Azure Cognitive Services
description: Mit bestimmten Arten von menschenmarkierten Transkriptionen und zugehörigem Text kann die Erkennungsgenauigkeit für ein Spracherkennungsmodell anhand des Sprechszenarios verbessert werden.
services: cognitive-services
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: v-demjoh
ms.openlocfilehash: bb904482f3cb5900b724803816269f1b10ab3720
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83727866"
---
# <a name="improve-custom-speech-accuracy"></a>Verbessern der Custom Speech-Genauigkeit

In diesem Artikel wird beschrieben, wie Sie die Qualität Ihres benutzerdefinierten Modells verbessern, indem Sie Audiodaten, menschenmarkierte Transkripts und zugehörigen Text hinzufügen.

## <a name="accuracy-in-different-scenarios"></a>Genauigkeit in verschiedenen Szenarien

Spracherkennungsszenarien unterscheiden sich in Bezug auf die Audioqualität und die Sprache (Vokabular und Sprechweise). Die folgende Tabelle enthält Informationen zu vier gängigen Szenarien:

| Szenario | Audioqualität | Vokabular | Sprechweise |
|----------|---------------|------------|----------------|
| Callcenter | Niedrig, 8 kHz, ggf. zwei Menschen über einen Audiokanal, ggf. komprimiert | Eingeschränkt, auf jeweiliges Thema und Produkte bezogen | Interaktiv, locker strukturiert |
| Sprachassistent (z. B. Cortana oder ein Drive-Through-Fenster) | Hoch, 16 kHz | Viele Entitäten (Songtitel, Produkte, Orte) | Klar gesprochene Wörter und Ausdrücke |
| Diktat (Sofortnachricht, Notizen, Suche) | Hoch, 16 kHz | Verschiedene | Festhalten von Notizen |
| Untertitelung für Videos | Variabel, z. B. vielfältige Mikrofonnutzung, hinzugefügte Musik | Variabel, aus Besprechungen, Rezitationen, Songtexten | Vorgelesen, vorbereitet oder locker strukturiert |

Unterschiedliche Szenarien führen zu Ergebnissen unterschiedlicher Qualität. Die folgende Tabelle enthält Informationen dazu, wie die Inhalte dieser vier Szenarien in Bezug auf die [Wort-Fehler-Rate (Word Error Rate, WER)](how-to-custom-speech-evaluate-data.md) abschneiden. In der Tabelle ist angegeben, welche Fehlertypen in den einzelnen Szenarien am häufigsten vorkommen.

| Szenario | Spracherkennungsqualität | Fehler beim Einfügen | Fehler beim Löschen | Fehler beim Ersetzen |
|----------|----------------------------|------------------|-----------------|---------------------|
| Callcenter | Mittel (WER < 30 %) | Niedrig, sofern keine anderen Personen im Hintergrund sprechen | Ggf. hoch. In Callcentern kann es laut sein, und für das Modell kommt es durch sich überlappende Sprecher ggf. zu Verwirrung | Mittel. Namen von Produkten und Personen können diese Fehler verursachen |
| Sprachassistent | Hoch (WER kann < 10 % sein) | Niedrig | Niedrig | Mittel, aufgrund von Songtiteln, Produktnamen oder Orten |
| Diktieren | Hoch (WER kann < 10 % sein) | Niedrig | Niedrig | High |
| Untertitelung für Videos | Je nach Videotyp (WER kann < 50 % sein) | Niedrig | Kann aufgrund von Musik, Nebengeräuschen, Mikrofonqualität hoch sein | Fachjargon kann zu diesen Fehlern führen |

Durch die Bestimmung der WER-Komponenten (Anzahl von Fehlern beim Einfügen, Löschen und Ersetzen) können Sie ermitteln, welche Art von Daten zur Verbesserung des Modells hinzugefügt werden sollten. Verwenden Sie das [Custom Speech-Portal](https://speech.microsoft.com/customspeech), um die Qualität für ein Basismodell anzuzeigen. Im Portal werden die Raten für Fehler beim Einfügen, Ersetzen und Löschen gemeldet, die in der WER-Qualitätsrate zusammengefasst sind.

## <a name="improve-model-recognition"></a>Verbessern der Modellerkennung

Sie können die Zahl von Erkennungsfehlern reduzieren, indem Sie im [Custom Speech-Portal](https://speech.microsoft.com/customspeech) Trainingsdaten hinzufügen. 

Planen Sie die Wartung Ihres benutzerdefinierten Modells durch das regelmäßige Hinzufügen von Quellmaterial. Sie müssen für Ihr benutzerdefiniertes Modell zusätzliche Trainingsläufe durchführen, damit Sie über Änderungen in Bezug auf Ihre Entitäten informiert sind. Es kann beispielsweise sein, dass Aktualisierungen von Produktnamen, Songtiteln oder neuen Dienstidentifizierungen erforderlich sind.

In den folgenden Abschnitten wird beschrieben, wie mit den einzelnen Arten von zusätzlichen Trainingsdaten Fehler reduziert werden können.

### <a name="add-related-text-sentences"></a>Hinzufügen von passenden Textsätzen

Mit zusätzlichen passenden Textsätzen können hauptsächlich Ersetzungsfehler reduziert werden, die sich auf die fehlerhafte Erkennung von gängigen und themenspezifischen Wörtern beziehen, indem diese im Kontext angezeigt werden. Bei themenspezifischen Wörtern kann es sich auch um ungewöhnliche oder ausgedachte Wörter handeln, aber ihre Aussprache muss eindeutig sein, damit sie erkannt werden können.

> [!NOTE]
> Vermeiden Sie Textsätze, die „Rauschen“ enthalten, z. B. nicht zu erkennende Buchstaben oder Wörter.

### <a name="add-audio-with-human-labeled-transcripts"></a>Hinzufügen von Audiodaten mit menschenmarkierten Transkriptionen

Audiodaten mit menschenmarkierten Transkripts ermöglichen die größten Verbesserungen bei der Genauigkeit, sofern die Audiodaten aus dem Zielanwendungsfall stammen. Beispiele müssen den vollständigen Sprachbereich abdecken. Ein Callcenter für ein Einzelhandelsgeschäft erhält die meisten Anrufe zu Badebekleidung und Sonnenbrillen in den Sommermonaten. Stellen Sie sicher, dass Ihr Beispiel den vollständigen Sprachbereich abdeckt, der erkannt werden soll.

Berücksichtigen Sie Folgendes:

* Custom Speech kann den Wortkontext nur erfassen, um Ersetzungsfehler zu reduzieren (keine Einfüge- oder Löschfehler).
* Vermeiden Sie Beispiele, die Transkriptionsfehler enthalten, aber verwenden Sie unterschiedliche Audioqualitäten.
* Vermeiden Sie Sätze, die sich nicht auf Ihre Problembestellung beziehen. Sätze dieser Art können das Modell beeinträchtigen.
* Wenn die Qualität von Transkripts variiert, können Sie besonders gute Sätze (z. B. hervorragende Transkriptionen mit wichtigen Ausdrücken) duplizieren, um ihre Gewichtung zu erhöhen.

### <a name="add-new-words-with-pronunciation"></a>Hinzufügen von neuen Wörtern mit besonderer Aussprache

Wörter, die ausgedacht oder sehr speziell sind, verfügen unter Umständen über eine besondere Aussprache. Ein Wort dieser Art kann erkannt werden, wenn es zum Aussprechen in kleinere Wörter unterteilt werden kann. Sprechen Sie **Xbox** beispielsweise deutlich als **X box** aus, damit das Wort erkannt wird. Diese Vorgehensweise führt nicht zu einer Erhöhung der Gesamtgenauigkeit, aber die Erkennung dieser Schlüsselwörter kann verbessert werden.

> [!NOTE]
> Diese Methode ist derzeit nur für einige Sprachen verfügbar. Informationen zur Anpassung für die Aussprache finden Sie in der [Tabelle zur Spracherkennung](language-support.md).

## <a name="sources-by-scenario"></a>Quellen nach Szenario

Die folgende Tabelle enthält Szenarien zur Spracherkennung und Quellmaterial für die drei obigen Kategorien für Trainingsinhalte.

| Szenario | Passende Textsätze | Audio + menschenmarkierte Transkripte | Neue Wörter mit besonderer Aussprache |
|----------|------------------------|------------------------------|------------------------------|
| Callcenter             | Marketingdokumente, Website, Produktbewertungen in Bezug auf Callcenteraktivitäten | Von Menschen transkribierte Callcenteranrufe | Begriffe mit mehrdeutiger Aussprache (siehe „Xbox“ oben) |
| Sprachassistent         | Auflisten von Sätzen mit allen Kombinationen von Befehlen und Entitäten | Aufzeichnen von Stimmen, die Befehle in das Gerät sprechen, und Transkribieren in Text | Namen (Filme, Songs, Produkte) mit besonderer Aussprache |
| Diktieren               | Eingaben in Schriftform, z. B. Sofortnachrichten oder E-Mails | Ähnlich wie oben | Ähnlich wie oben |
| Untertitelung für Videos | Skripts von TV-Sendungen, Filme, Marketinginhalte, Videozusammenfassungen | Genaue Transkripts von Videos | Ähnlich wie oben |

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren Ihres Modells](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Vorbereiten und Testen Ihrer Daten](how-to-custom-speech-test-data.md)
- [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)