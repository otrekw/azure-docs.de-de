---
title: 'Schlüsselbegriffe: benutzerdefinierter Translator'
titleSuffix: Azure Cognitive Services
description: Liste der Schlüsselbegriffe, die in den Artikeln zu Benutzerdefinierter Translator verwendet werden.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 04/02/2021
ms.author: lajanuar
ms.topic: reference
ms.openlocfilehash: 4a0308f2b961ce9b1321fab3e202e038eb199674
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286418"
---
# <a name="custom-translator-key-terms"></a>Schlüsselbegriffe für benutzerdefinierte Translators

In der folgenden Tabelle finden Sie eine Liste der Begriffe, die bei der Verwendung von [Benutzerdefinierten Translators](https://portal.customtranslator.azure.ai) vorkommen können.

| Wort oder Ausdruck|Definition|
|------------------|-----------|
| Ausgangssprache | Die Ausgangssprache ist die Sprache, die in eine andere Sprache konvertiert werden soll („Zielsprache“).|
| Zielsprache| Die Zielsprache ist die Sprache, die die maschinelle Übersetzung basierend auf der Ausgangssprache ausgeben soll. |
| Einsprachige Datei | Eine einsprachige Datei wurde in einer einzigen Sprache verfasst, die keiner anderen Datei in einer anderen Sprache zugeordnet ist. |
| Parallele Dateien | Eine parallele Datei ist eine Kombination aus zwei Dateien mit übereinstimmendem Text. Eine Datei enthält die Ausgangssprache. Die andere die Zielsprache.|
| Satzzuordnung| In einem parallelen Dataset sind Sätze einander zugeordnet, die in der Ausgangs- und Zielsprache den gleichen Text abbilden. So sollte beispielsweise der erste Satz einer parallelen Quelldatei dem ersten Satz der parallelen Zieldatei entsprechen.|
| Zugeordneter Text | Einer der wichtigsten Schritte bei der Dateiüberprüfung ist die Zuordnung der Sätze in den parallelen Dokumenten. Die Ausdrucksweise unterscheidet sich von Sprache zu Sprache. Auch verfügen verschiedene Sprachen über unterschiedliche Wortstellung. Beim Zuordnen von Text werden Sätze mit gleichem Inhalt so ausgerichtet, dass sie zum Trainieren verwendet werden können. Wenn nur wenige Sätze zugeordnet werden können, stellt möglicherweise mindestens eine der beiden Dateien ein Problem dar. |
| Worttrennung/-zusammenführung | Beim Trennen von Wörtern werden die Grenzen zwischen Wörtern markiert. Viele Zeichensysteme kennzeichnen Abgrenzungen zwischen Wörtern mit Leerzeichen. Beim Zusammenführen von Wörtern werden diese Abgrenzungen entfernt, die möglicherweise in einem vorherigen Schritt zwischen den Wörtern eingefügt wurden. |
| Trennzeichen   | Mit Trennzeichen wird ein Satz in Segmente unterteilt oder von anderen Sätzen abgegrenzt. Zum Beispiel trennen im Englischen Leerzeichen Wörter, Doppelpunkte und Semikolons Nebensätze und Punkte ganze Sätze. |
| Trainingsdateien | Anhand einer Trainingsdatei lernt das maschinelle Übersetzungssystem, wie eine Sprache (Quelle) auf eine Zielsprache (Ziel) abgebildet wird. Je mehr Daten Sie zur Verfügung stellen, umso besser sind die Ergebnisse des Systems. |
| Optimierungsdateien | Diese Dateien werden oft zufällig aus dem Trainingsset abgeleitet (wenn Sie kein Optimierungsset auswählen). Die Sätze werden automatisch ausgewählt und verwendet, um das System zu optimieren und sicherzustellen, dass es ordnungsgemäß funktioniert. Wenn Sie ein allgemeines Übersetzungsmodell und eigene Optimierungsdateien erstellen möchten, achten Sie darauf, dass es sich um eine zufällige Menge von Sätzen aus unterschiedlichen Domänen handelt. |
| Testdateien| Hierbei handelt es sich oft um abgeleitete Dateien, die zufällig aus dem Trainingsset ausgewählt wurden (wenn Sie kein Testset auswählen). Mithilfe dieser Sätze soll die Genauigkeit des Übersetzungsmodells bewertet werden. Da das System diese Sätze genau übersetzen soll, empfiehlt es sich, ein Testset von Sätzen zu erstellen und in den Translator hochzuladen. Dadurch wird sichergestellt, dass diese Sätze in der Auswertung des Systems verwendet werden (Generierung einer BLEU-Bewertung).   |
| Kombinationsdatei   | Dieser Dateityp enthält sowohl die ausgangssprachlichen als auch die übersetzten Sätze in einer Datei. Diese Dateiformate werden unterstützt: TMX, XLIFF, XLF, ICI, XLSX. |
| Archivdatei | Eine Datei, die andere Dateien enthält. Diese Dateiformate werden unterstützt: ZIP, GZ, TGZ.  |
| BLEU-Bewertung   | [BLEU](what-is-bleu-score.md) ist die branchenübliche Methode zur Bewertung der Richtigkeit bzw. Genauigkeit des Übersetzungsmodells. Es gibt zwar auch andere Bewertungsmethoden, doch Microsoft Translator meldet Projektbesitzern die Genauigkeit basierend auf der BLEU-Methode.
