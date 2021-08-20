---
title: Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte mit Azure Video Analyzer for Media (früher Video Indexer)
titleSuffix: Azure Video Analyzer for Media
description: In diesem Thema wird gezeigt, wie mehrsprachige Inhalte mit Azure Video Analyzer for Media (früher Video Indexer) automatisches erkannt und transkribiert werden.
services: azure-video-analyzer
author: Juliako
manager: femila
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 09/01/2019
ms.author: juliako
ms.openlocfilehash: a0466a8123a3732e78cd35826290b3e29e3c8c50
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112119208"
---
# <a name="automatically-identify-and-transcribe-multi-language-content"></a>Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte

Azure Video Analyzer for Media (früher Video Indexer) unterstützt die automatische Spracherkennung und die Transkription in mehrsprachigen Inhalten. Dieser Prozess beinhaltet das automatische Erkennen der gesprochenen Sprache in verschiedenen Audiosegmenten, das Versenden der einzelnen Segmente der zu transkribierenden Mediendatei und das Zusammenführen der Transkription in einer einzelnen Transkription. 

## <a name="choosing-multilingual-identification-on-indexing-with-portal"></a>Auswählen der mehrsprachigen Erkennung bei der Indizierung mit dem Portal

Wenn Sie Ihr Video hochladen und indizieren, können Sie die **mehrsprachige Erkennung** auswählen. Alternativ können Sie die **mehrsprachige Erkennung** auch bei der Neuindizierung Ihres Videos auswählen. Die folgenden Schritte beschreiben, wie Sie eine Neuindizierung durchführen:

1. Navigieren Sie zur [Video Analyzer for Media](https://vi.microsoft.com/)-Website und melden Sie sich an.
1. Navigieren Sie zur Seite **Bibliothek**, und zeigen Sie auf den Namen des Videos, das Sie erneut indizieren möchten. 
1. Klicken Sie unten rechts auf die Schaltfläche **Video erneut indizieren**. 
1. Wählen Sie im Dialogfeld **Video erneut indizieren** im Dropdownfeld **Ausgangssprache des Videos** die Option **multi-language detection** (Mehrsprachige Erkennung) aus.

    * Wenn ein Video als mehrsprachig indiziert ist, enthält die Seite „Insight“ diese Option, und ein zusätzlicher Insighttyp erscheint, mit dem der Benutzer anzeigen kann, welches Segment in welcher Sprache als „gesprochene Sprache“ transkribiert wird.
    * Die Übersetzung in alle Sprachen ist über das mehrsprachige Transkript vollständig verfügbar.
    * Alle weiteren Insights erscheinen in der erkannten Hauptsprache – das ist die Sprache, die in der Audiodatei zum größten Teil enthalten war.
    * Untertitel sind im Player ebenfalls in mehreren Sprachen verfügbar.

![Portalfunktion](./media/multi-language-identification-transcription/portal-experience.png)

## <a name="choosing-multilingual-identification-on-indexing-with-api"></a>Auswählen der mehrsprachigen Erkennung bei der Indizierung mit der API

Wählen Sie bei der Indizierung bzw. der [Neuindizierung](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Re-Index-Video) eines Videos mithilfe der API die Option `multi-language detection` im Parameter `sourceLanguage` aus.

### <a name="model-output"></a>Ausgabe des Modells

Das Modell ruft alle im Video erkannten Sprachen in einer Liste ab.

```json
"sourceLanguage": null,
"sourceLanguages": [
    "es-ES",
    "en-US"
],
```

Darüber hinaus enthält jede Instanz im Transkriptionsabschnitt die Sprache, in der sie transkribiert wurde.

```json
{
  "id": 136,
  "text": "I remember well when my youth Minister took me to hear Doctor King I was a teenager.",
  "confidence": 0.9343,
  "speakerId": 1,
  "language": "en-US",
  "instances": [
    {
       "adjustedStart": "0:21:10.42",
       "adjustedEnd": "0:21:17.48",
       "start": "0:21:10.42",
       "end": "0:21:17.48"
    }
  ]
},
```

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen

* Unterstützte Sprachen: Englisch, Französisch, Deutsch, Spanisch.
* Unterstützung für mehrsprachige Inhalte mit bis zu drei unterstützten Sprachen.
* Falls die Audiodaten andere Sprachen enthalten, die nicht in der obigen Liste mit den unterstützten Sprachen aufgeführt sind, kommt es zu einem unerwarteten Ergebnis.
* Die Mindestsegmentlänge für die Erkennung der einzelnen Sprachen beträgt 15 Sekunden.
* Der Offset der Spracherkennung beträgt im Durchschnitt drei Sekunden.
* Die Sprache sollte fortlaufend gleich sein. Häufige Wechsel zwischen Sprachen wirken sich unter Umständen auf die Leistung des Modells aus.
* Die Leistung des Modells wird ggf. auch beeinträchtigt, wenn der Sprecher kein Muttersprachler ist (wenn ein Sprecher beispielsweise zuerst in seiner Muttersprache spricht und dann zu einer anderen Sprache wechselt).
* Das Modell ist so konzipiert, dass eine spontane Unterhaltung mit einer angemessenen Audioakustik erkannt werden kann (keine Sprachbefehle, Gesang usw.).
* Die Projekterstellung und -bearbeitung ist für mehrsprachige Videos derzeit nicht verfügbar.
* Benutzerdefinierte Sprachmodelle sind bei Verwendung der mehrsprachigen Erkennung nicht verfügbar.
* Das Hinzufügen von Schlüsselwörtern wird nicht unterstützt.
* Beim Exportieren von Untertiteldateien erscheint keine Sprachanzeige.
* Die API zum Aktualisieren von Transkripts unterstützt keine mehrsprachigen Dateien.

## <a name="next-steps"></a>Nächste Schritte

[Video Analyzer for Media: Übersicht](video-indexer-overview.md)
