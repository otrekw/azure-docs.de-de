---
title: include file
ms.topic: include
ms.custom: include file
ms.date: 5/17/2020
ms.openlocfilehash: 79a45825be66b44d869ef741917436de87a1f5d0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "83653216"
---
Informieren Sie sich über [Konzepte](../luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) der Normalisierung, und erfahren Sie, wie Sie [Versions](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings)-APIs verwenden, um diese Einstellungen zu aktualisieren, oder verwenden Sie im LUIS-Portal den Abschnitt **Verwalten** auf der Seite **Einstellungen**.


|Einstellung auf der Benutzeroberfläche|API-Einstellung|Information|
|--|--|--|
|„Use non-deterministic training“ (Nicht deterministisches Training verwenden)|`UseAllTrainingData`|Das Training verwendet einen kleinen Prozentsatz negativer Stichproben. Wenn Sie anstelle der kleinen negativen Stichprobenentnahme alle Daten verwenden möchten, legen Sie diese Option auf `true` fest. |
|„Normalize diacritics“ (Diakritische Zeichen normalisieren)|`NormalizeDiacritics`|Bei der Normalisierung diakritischer Zeichen werden diakritische Zeichen in Äußerungen durch reguläre Zeichen ersetzt. Diese Einstellung ist nur für [Sprachen](../luis-reference-application-settings.md#diacritics-normalization) verfügbar, die diakritische Zeichen unterstützen.|
|„Normalize punctuation“ (Interpunktion normalisieren)|`NormalizePunctuation`|Bei der Normalisierung der Interpunktion wird die Interpunktion aus den Äußerungen entfernt, bevor Ihre Modelle trainiert und Ihre Endpunktabfragen vorhergesagt werden.|
|„Normalize word forms“ (Wortformen normalisieren)|`NormalizeWordForm`|Bei Wortformen wird nur der Wortstamm berücksichtigt.|
