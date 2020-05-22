---
title: include file
ms.topic: include
ms.custom: include file
ms.date: 5/04/2020
ms.openlocfilehash: 819dfa3127eb91a2f08687a76ea0586439b650a4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83590984"
---
|Ebene|Einstellung auf der Benutzeroberfläche|API-Einstellung|Information|
|--|--|--|--|
|App|„Make endpoints public“ (Endpunkte öffentlich machen)|`Public`|Auf Ihre öffentliche App kann von beliebigen Benutzern zugegriffen werden, sofern sie über einen Vorhersageschlüssel und über die ID Ihrer App verfügen. |
|Version|„Use non-deterministic training“ (Nicht deterministisches Training verwenden)|`UseAllTrainingData`|Das Training verwendet einen kleinen Prozentsatz negativer Stichproben. Wenn Sie anstelle der kleinen negativen Stichprobenentnahme alle Daten verwenden möchten, legen Sie diese Option auf `true` fest. |
|Version|„Normalize diacritics“ (Diakritische Zeichen normalisieren)|`NormalizeDiacritics`|Bei der Normalisierung diakritischer Zeichen werden diakritische Zeichen in Äußerungen durch reguläre Zeichen ersetzt.|
|Version|„Normalize punctuation“ (Interpunktion normalisieren)|`NormalizePunctuation`|Bei der Normalisierung der Interpunktion wird die Interpunktion aus den Äußerungen entfernt, bevor Ihre Modelle trainiert und Ihre Endpunktabfragen vorhergesagt werden.|
|Version|„Normalize word forms“ (Wortformen normalisieren)|`NormalizeWordForm`|Bei Wortformen wird nur der Wortstamm berücksichtigt.|

Informieren Sie sich über [Konzepte](../luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) der Normalisierung, und erfahren Sie, wie Sie [App](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58aeface39e2bb03dcd5909e)- und [Versions](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings)-APIs verwenden, um diese Einstellungen zu aktualisieren, oder verwenden Sie im LUIS-Portal den Abschnitt **Verwalten** auf der Seite **Anwendungseinstellungen**.