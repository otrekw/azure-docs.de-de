---
title: Verwenden von Batch-Transkription – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: panosper
ms.openlocfilehash: 4c2985f35621ff3120217cbe38705ad2c228d6f7
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122099"
---
# <a name="how-to-use-batch-transcription"></a>Verwenden von Batch-Transkription

Die Batch-Transkription eignet sich optimal für die Transkription großer Mengen an Audiomaterial im Speicher. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionsergebnisse asynchron empfangen.

Die API bietet asynchrone Transkription von Sprache in Text und weitere Funktionen. Sie können die REST-API verwenden, um Methoden für folgende Zwecke verfügbar zu machen:

- Erstellen von Anforderungen für die Batchverarbeitung
- Abfragen des Status
- Herunterladen von Transkriptionsergebnissen
- Löschen von Transkriptionsinformationen aus dem Dienst

Die ausführliche API steht als [Swagger-Dokument](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A) unter der Überschrift `Custom Speech transcriptions` zur Verfügung.

Batch-Transkriptionsaufträge werden auf der Grundlage des optimalen Arbeitsaufwands geplant. Zurzeit lässt sich nicht einschätzen, wann ein Auftrag in den Ausführungsstatus wechselt. Bei normaler Systemlast sollte dies innerhalb von Minuten erfolgen. Sobald er ausgeführt wird, wird die aktuelle Transkription schneller als in Audioechtzeit verarbeitet.

Über die benutzerfreundliche API hinaus ist keine Bereitstellung benutzerdefinierter Endpunkte erforderlich, und es sind keine Einschränkungen wegen paralleler Ausführung zu beachten.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="subscription-key"></a>Abonnementschlüssel

Wie bei allen Features des Speech-Diensts erstellen Sie mithilfe der Anleitung unter [Erste Schritte](get-started.md) einen Abonnementschlüssel im [Azure-Portal](https://portal.azure.com).

>[!NOTE]
> Für die Verwendung der Batch-Transkription ist ein Standardabonnement (S0) für den Speech-Dienst erforderlich. Kostenlose Abonnementschlüssel (F0) funktionieren nicht. Weitere Informationen finden Sie unter [Preise und Einschränkungen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Benutzerdefinierte Modelle

Wenn Sie die Anpassung von Audio- oder Sprachmodellen planen, befolgen Sie die Schritte unter [Anpassen von Akustikmodellen](how-to-customize-acoustic-models.md) und [Entwerfen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md). Um die erstellten Modelle in der Batch-Transkription zu verwenden, benötigen Sie ihre Modell-IDs. Die Modell-ID lässt sich durch Untersuchen der Details des Modells ermitteln. Für den Batch-Transkriptionsdienst ist kein bereitgestellter benutzerdefinierter Endpunkt erforderlich.

## <a name="the-batch-transcription-api"></a>Die Batch-Transkriptions-API

### <a name="supported-formats"></a>Unterstützte Formate

Die Batch-Transkriptions-API unterstützt die folgenden Formate:

| Format | Codec | Bitrate | Samplingrate |
|--------|-------|---------|-------------|
| WAV | PCM | 16 Bit | 8 kHz oder 16 kHz, Mono oder Stereo |
| MP3 | PCM | 16 Bit | 8 kHz oder 16 kHz, Mono oder Stereo |
| OGG | OPUS | 16 Bit | 8 kHz oder 16 kHz, Mono oder Stereo |

Bei Audiostreams in Stereo sind linker und rechter Kanal während des Transkriptionsvorgangs geteilt. Für jeden Kanal wird eine JSON-Ergebnisdatei erstellt. Die pro Äußerung erstellten Zeitstempel ermöglichen es dem Entwickler, eine geordnete endgültige Transkription zu erstellen.

### <a name="configuration"></a>Konfiguration

Die Konfigurationsparameter werden als JSON angegeben:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to use, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "None | Removed | Tags | Masked",
    "PunctuationMode": "None | Dictated | Automatic | DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True | False",
    "AddSentiment" : "True | False",
    "AddDiarization" : "True | False",
    "TranscriptionResultsContainerUrl" : "<SAS to Azure container to store results into (write permission required)>"
  }
}
```

### <a name="configuration-properties"></a>Konfigurationseigenschaften

Verwenden Sie diese optionalen Eigenschaften zum Konfigurieren der Transkription:

| Parameter | Beschreibung |
|-----------|-------------|
| `ProfanityFilterMode` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `None` (deaktiviert den Obszönitätenfilter), `Masked` (Obszönitäten werden durch Sternchen ersetzt), `Removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `Tags` (fügt „Obszönität“-Tags ein). Die Standardeinstellung ist `Masked`. |
| `PunctuationMode` | Gibt den Umgang mit Satzzeichen in Erkennungsergebnissen an. Gültige Werte sind `None` (deaktiviert Satzzeichen), `Dictated` (explizite Satzzeichen), `Automatic` (der Decoder verwaltet die Satzzeichen), `DictatedAndAutomatic` (vorgeschriebene Satzzeichen) oder „automatic“. |
| `AddWordLevelTimestamps` | Gibt an, ob der Ausgabe Zeitstempel auf Wortebene hinzugefügt werden sollen. Gültige Werte sind `true` zum Aktivieren und `false` zum Deaktivieren von Zeitstempeln auf Wortebene. |
| `AddSentiment` | Gibt an, dass die Stimmung der Äußerung hinzugefügt werden soll. Gültige Werte sind `true` zum Aktivieren der Stimmung pro Äußerung und `false` (Standardwert) zum Deaktivieren. |
| `AddDiarization` | Gibt an, dass die Diarisierungsanalyse bei der Eingabe durchgeführt werden sollte. Es wird erwartet, dass diese Eingabe ein Monokanal mit zwei Stimmen ist. Gültige Werte sind `true` zum Aktivieren der Diarisierung und `false` (der Standardwert) zu deren Deaktivierung. Außerdem muss `AddWordLevelTimestamps` auf „true“ festgelegt werden.|
|`TranscriptionResultsContainerUrl`|Optionales SAS-Token für einen Container mit Schreibzugriff in Azure. Das Ergebnis wird in diesem Container gespeichert.

### <a name="storage"></a>Storage

Die Batchtranskription unterstützt [Azure Blob-Speicher](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) zum Lesen von Audio und zum Schreiben von Transkriptionen in den Speicher.

## <a name="the-batch-transcription-result"></a>Das Ergebnis der Batch-Transkription

Für Mono-Audioeingangssignale wird eine Transkriptions-Ergebnisdatei erstellt. Für Stereo-Audioeingangssignale werden zwei Transkriptions-Ergebnisdateien erstellt. Jede wiest diese Struktur auf:

```json
{
  "AudioFileResults":[ 
    {
      "AudioFileName": "Channel.0.wav | Channel.1.wav"      'maximum of 2 channels supported'
      "AudioFileUrl": null                                  'always null'
      "AudioLengthInSeconds": number                        'Real number. Two decimal places'
      "CombinedResults": [
        {
          "ChannelNumber": null                             'always null'
          "Lexical": string
          "ITN": string
          "MaskedITN": string
          "Display": string
        }
      ]
      SegmentResults:[                                     'for each individual segment'
        {
          "RecognitionStatus": Success | Failure
          "ChannelNumber": null
          "SpeakerId": null | "1 | 2"                     'null if no diarization
                                                            or stereo input file, the
                                                            speakerId as a string if
                                                            diarization requested for
                                                            mono audio file'
          "Offset": number                                'time in milliseconds'
          "Duration": number                              'time in milliseconds'
          "OffsetInSeconds" : number                      'Real number. Two decimal places'
          "DurationInSeconds" : number                    'Real number. Two decimal places'
          "NBest": [
            {
              "Confidence": number                        'between 0 and 1'
              "Lexical": string
              "ITN": string
              "MaskedITN": string
              "Display": string
              "Sentiment":
                {                                          'this is omitted if sentiment is
                                                            not requested'
                  "Negative": number                        'between 0 and 1'
                  "Neutral": number                         'between 0 and 1'
                  "Positive": number                        'between 0 and 1'
                }
              "Words": [
                {
                  "Word": string
                  "Offset": number                         'time in milliseconds'
                  "Duration": number                       'time in milliseconds'
                  "OffsetInSeconds": number                'Real number. Two decimal places'
                  "DurationInSeconds": number              'Real number. Two decimal places'
                  "Confidence": number                     'between 0 and 1'
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

Das Ergebnis enthält diese Formen:

|Form|Inhalt|
|-|-|
|`Lexical`|Die tatsächlich erkannten Wörter
|`ITN`|Der erkannte Text in Form von inverser Textnormalisierung. Abkürzungen („Doktor Schmidt“ zu „Dr. Schmidt“), Telefonnummern und weitere Transformationen werden angewendet.
|`MaskedITN`|Die ITN-Form mit angewendeter Maskierung von Obszönitäten
|`Display`|Die Anzeigeform des erkannten Texts. Diese beinhaltet hinzugefügte Satzzeichen und Groß-/Kleinschreibung.

## <a name="speaker-separation-diarization"></a>Sprechertrennung (Diarisierung)

Diarisierung ist der Prozess, bei dem Sprecher in einem Audioelement getrennt werden. Unsere Batch-Pipeline unterstützt die Diarisierung und kann zwei Sprecher in Monokanalaufnahmen erkennen. Die Funktion ist für Stereoaufzeichnungen nicht verfügbar.

Alle Transkriptionsausgaben enthalten eine `SpeakerId`. Wenn keine Diarisierung verwendet wird, tritt `"SpeakerId": null` in der JSON-Ausgabe auf. Bei der Diarisierung werden zwei Stimmen unterstützt, sodass die Sprecher als `"1"` oder `"2"` identifiziert werden.

Wenn Sie Diarisierung anfordern möchten, müssen Sie einfach der HTTP-Anforderung den relevanten Parameter wie unten gezeigt hinzufügen.

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

Zeitstempel auf Wortebene müssten außerdem in der oben angegebenen Anforderung als Parameter ‚aktiviert‘ werden.

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Die Funktion zur Stimmungsanalyse schätzt die im Audiosignal ausgedrückte Stimmung ein. Die Stimmung wird durch einen Wert zwischen 0 und 1 für die Stimmungen `Negative`, `Neutral` und `Positive` ausgedrückt. Beispielsweise kann die Stimmungsanalyse in Callcenterszenarien verwendet werden:

- Einblicke in die Kundenzufriedenheit gewinnen
- Einblicke in die Leistung der Mitarbeiter erhalten (das Team, das Anrufe entgegennimmt)
- Den exakten Zeitpunkt ermitteln, zu dem ein Anruf ins Negative umschlug
- Was funktionierte gut, wenn es gelingt, einem negativen Anruf eine positive Wendung zu geben
- herausfinden, was Kunden an einem Produkt oder Dienst gefallen oder missfallen hat

Die Stimmung wird pro Audiosegment auf der Grundlage der lexikalischen Form bewertet. Zum Berechnen der Stimmung wird der gesamte Text im betreffenden Audiosegment bewertet. Es wird keine aggregierte Stimmung für die gesamte Transkription berechnet.

Nachfolgend sehen Sie ein Beispiel für eine JSON-Ausgabe:

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="best-practices"></a>Bewährte Methoden

Der Transkriptionsdienst kann eine große Anzahl an übermittelten Transkriptionen verarbeiten. Sie können den Status Ihrer Transkriptionen über ein `GET` in der [Transkriptionsmethode](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions) abfragen. Halten Sie die zurückgegebenen Informationen in einem vernünftigen Größenrahmen, indem Sie den Parameter `take` (einige Hundert) angeben. [Löschen Sie Transkriptionen](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription) regelmäßig aus dem Dienst, nachdem Sie die Ergebnisse abgerufen haben. Dies stellt schnelle Antworten auf die Aufrufe der Transkriptionsverwaltung sicher.

## <a name="sample-code"></a>Beispielcode

Vollständige Beispiele stehen im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) innerhalb des Unterverzeichnisses `samples/batch` zur Verfügung.

Sie müssen den Beispielcode mit Ihren Abonnementinformationen, der Dienstregion, dem SAS-URI mit Verweis auf die zu transkribierende Audiodatei und die Modell-IDs anpassen, falls Sie ein benutzerdefiniertes Audio- oder Sprachmodell verwenden möchten.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Der Beispielcode richtet den Client ein und sendet die Transkriptionsanforderung. Anschließend fragt er die Statusinformationen ab und druckt Details über den Fortschritt der Transkription.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Vollständige Informationen zu den vorherigen Aufrufen finden Sie in unserer [Swagger-Dokumentation](https://westus.cris.ai/swagger/ui/index) (in englischer Sprache). Das vollständige hier gezeigte Beispiel finden Sie auf [GitHub](https://aka.ms/csspeech/samples) im Unterverzeichnis `samples/batch`.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der von Ihnen erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode ist kein benutzerdefiniertes Modell angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei bzw. Dateien. Zum Angeben der Modelle können Sie dieselbe Methode wie bei den Modell-IDs für das Akustikmodell und das Sprachmodell übergeben.

> [!NOTE]
> Für Basistranskriptionen müssen Sie die ID der Basismodelle nicht deklarieren. Wenn Sie nur eine Sprachmodell-ID (und keine Akustikmodell-ID) angeben, wird automatisch ein entsprechendes Akustikmodell ausgewählt. Wenn Sie nur eine Akustikmodell-ID angeben, wird automatisch ein entsprechendes Sprachmodell ausgewählt.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Sie finden das Beispiel im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) im Verzeichnis `samples/batch`.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
