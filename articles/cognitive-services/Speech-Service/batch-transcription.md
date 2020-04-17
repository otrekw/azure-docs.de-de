---
title: 'Speech-Dienst: Was ist die Batch-Transkription?'
titleSuffix: Azure Cognitive Services
description: Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: wolfma
ms.openlocfilehash: 1f88df186526c2f9903337bb3331940be0989c3d
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892460"
---
# <a name="what-is-batch-transcription"></a>Was ist die Batch-Transkription?

Bei der Batch-Transkription handelt es sich um eine Reihe von Rest-API-Vorgängen, mit denen Sie große Mengen von Audiodaten im Speicher transkribieren können. Sie können per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionsergebnisse asynchron empfangen.

Die asynchrone Transkription für die Spracherkennung ist nur eine der verfügbaren Funktionen. Mit den REST-APIs für die Batch-Transkription können Sie die folgenden Methoden aufrufen:



|    Batch-Transkriptionsvorgang                                             |    Methode    |    REST-API-Aufruf                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    Erstellt eine neue Transkription.                                              |    POST      |    api/speechtotext/v2.0/transcriptions            |
|    Ruft eine Liste von Transkriptionen für das authentifizierte Abonnement ab.    |    GET       |    api/speechtotext/v2.0/transcriptions            |
|    Ruft eine Liste der unterstützten Gebietsschemas für Offlinetranskriptionen ab.              |    GET       |    api/speechtotext/v2.0/transcriptions/locales    |
|    Aktualisiert die änderbaren Details der durch die ID angegebenen Transkription.    |    PATCH     |    api/speechtotext/v2.0/transcriptions/{id}       |
|    Löscht die angegebene Transkriptionsaufgabe.                                 |    Delete    |    api/speechtotext/v2.0/transcriptions/{id}       |
|    Ruft die durch die ID angegebene Transkription ab.                        |    GET       |    api/speechtotext/v2.0/transcriptions/{id}       |




Sie können die ausführliche API prüfen und testen. Sie steht als [Swagger-Dokument](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A) unter der Überschrift `Custom Speech transcriptions` zur Verfügung.

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

| Format | Codec | Bitrate | Samplingrate                     |
|--------|-------|---------|---------------------------------|
| WAV    | PCM   | 16 Bit  | 8 kHz oder 16 kHz, Mono oder Stereo |
| MP3    | PCM   | 16 Bit  | 8 kHz oder 16 kHz, Mono oder Stereo |
| OGG    | OPUS  | 16 Bit  | 8 kHz oder 16 kHz, Mono oder Stereo |

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
    "TranscriptionResultsContainerUrl" : "<service SAS URI to Azure container to store results into (write permission required)>"
  }
}
```

### <a name="configuration-properties"></a>Konfigurationseigenschaften

Verwenden Sie diese optionalen Eigenschaften zum Konfigurieren der Transkription:

:::row:::
   :::column span="1":::
      **Parameter**
   :::column-end:::
   :::column span="2":::
      **Beschreibung**
:::row-end:::
:::row:::
   :::column span="1":::
      `ProfanityFilterMode`
   :::column-end:::
   :::column span="2":::
      Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind: `None` (deaktiviert den Obszönitätenfilter), `Masked` (Obszönitäten werden durch Sternchen ersetzt), `Removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `Tags` (fügt „Obszönität“-Tags ein). Die Standardeinstellung ist `Masked`.
:::row-end:::
:::row:::
   :::column span="1":::
      `PunctuationMode`
   :::column-end:::
   :::column span="2":::
      Gibt den Umgang mit Satzzeichen in Erkennungsergebnissen an. Zulässige Werte sind: `None` (deaktiviert die Interpunktion), `Dictated` (impliziert explizite (gesprochene) Interpunktion), `Automatic` (überlässt dem Decoder die Interpunktion) oder `DictatedAndAutomatic` (verwendet diktierte und automatische Interpunktion). Die Standardeinstellung ist `DictatedAndAutomatic`.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddWordLevelTimestamps`
   :::column-end:::
   :::column span="2":::
      Gibt an, ob der Ausgabe Zeitstempel auf Wortebene hinzugefügt werden sollen. Zulässige Werte sind `true` zum Aktivieren und `false` (Standardwert) zum Deaktivieren von Zeitstempeln auf Wortebene.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddSentiment`
   :::column-end:::
   :::column span="2":::
      Gibt an, ob die Standpunktanalyse auf die Äußerung angewendet werden soll. Zulässige Werte sind `true` zum Aktivieren und `false` (Standardwert) zum Deaktivieren. Weitere Informationen finden Sie unter [Stimmungsanalyse](#sentiment-analysis).
:::row-end:::
:::row:::
   :::column span="1":::
      `AddDiarization`
   :::column-end:::
   :::column span="2":::
      Gibt an, dass die Diarisierungsanalyse für die Eingabe durchgeführt werden soll. Es wird erwartet, dass diese Eingabe ein Monokanal mit zwei Stimmen ist. Zulässige Werte sind `true` zum Aktivieren und `false` (Standardwert) zum Deaktivieren der Diarisierung. Außerdem muss `AddWordLevelTimestamps` auf „true“ festgelegt werden.
:::row-end:::
:::row:::
   :::column span="1":::
      `TranscriptionResultsContainerUrl`
   :::column-end:::
   :::column span="2":::
      Optionale URL mit [Dienst-SAS](../../storage/common/storage-sas-overview.md) zu einem beschreibbaren Container in Azure. Das Ergebnis wird in diesem Container gespeichert.
:::row-end:::

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
      SegmentResults:[                                      'for each individual segment'
        {
          "RecognitionStatus": "Success | Failure"
          "ChannelNumber": null
          "SpeakerId": null | "1 | 2"                       'null if no diarization
                                                             or stereo input file, the
                                                             speakerId as a string if
                                                             diarization requested for
                                                             mono audio file'
          "Offset": number                                  'time in ticks (1 tick is 100 nanosec)'
          "Duration": number                                'time in ticks (1 tick is 100 nanosec)'
          "OffsetInSeconds" : number                        'Real number. Two decimal places'
          "DurationInSeconds" : number                      'Real number. Two decimal places'
          "NBest": [
            {
              "Confidence": number                          'between 0 and 1'
              "Lexical": string
              "ITN": string
              "MaskedITN": string
              "Display": string
              "Sentiment":
                {                                           'this is omitted if sentiment is
                                                             not requested'
                  "Negative": number                        'between 0 and 1'
                  "Neutral": number                         'between 0 and 1'
                  "Positive": number                        'between 0 and 1'
                }
              "Words": [
                {
                  "Word": string
                  "Offset": number                          'time in ticks (1 tick is 100 nanosec)'
                  "Duration": number                        'time in ticks (1 tick is 100 nanosec)'
                  "OffsetInSeconds": number                 'Real number. Two decimal places'
                  "DurationInSeconds": number               'Real number. Two decimal places'
                  "Confidence": number                      'between 0 and 1'
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

:::row:::
   :::column span="1":::
      **Form**
   :::column-end:::
   :::column span="2":::
      **Inhalt**
:::row-end:::
:::row:::
   :::column span="1":::
      `Lexical`
   :::column-end:::
   :::column span="2":::
      Die tatsächlich erkannten Wörter
:::row-end:::
:::row:::
   :::column span="1":::
      `ITN`
   :::column-end:::
   :::column span="2":::
      Der erkannte Text in Form von inverser Textnormalisierung. Abkürzungen („Doktor Schmidt“ zu „Dr. Schmidt“), Telefonnummern und weitere Transformationen werden angewendet.
:::row-end:::
:::row:::
   :::column span="1":::
      `MaskedITN`
   :::column-end:::
   :::column span="2":::
      Die ITN-Form mit angewendeter Maskierung von Obszönitäten
:::row-end:::
:::row:::
   :::column span="1":::
      `Display`
   :::column-end:::
   :::column span="2":::
      Die Anzeigeform des erkannten Texts. Hinzugefügte Satzzeichen und Groß-/Kleinschreibung sind enthalten.
:::row-end:::

## <a name="speaker-separation-diarization"></a>Sprechertrennung (Diarisierung)

Diarisierung ist der Prozess, bei dem Sprecher in einem Audioelement getrennt werden. Unsere Batch-Pipeline unterstützt die Diarisierung und kann zwei Sprecher in Monokanalaufnahmen erkennen. Die Funktion ist für Stereoaufzeichnungen nicht verfügbar.

Alle Transkriptionsausgaben enthalten eine `SpeakerId`. Wenn die Diarisierung nicht verwendet wird, wird `"SpeakerId": null` in der JSON-Ausgabe angezeigt. Bei der Diarisierung werden zwei Stimmen unterstützt, sodass die Sprecher als `"1"` oder `"2"` identifiziert werden.

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

Die Stimmung wird pro Audiosegment auf der Grundlage der lexikalischen Form bewertet. Zum Berechnen der Stimmung wird der gesamte Text im betreffenden Audiosegment bewertet. Es wird keine aggregierte Stimmung für die gesamte Transkription berechnet. Die Standpunktanalyse ist zurzeit nur für Englisch verfügbar.

> [!NOTE]
> Es wird empfohlen, stattdessen die Textanalyse-API von Microsoft zu verwenden. Diese bietet erweiterte Features, die über die Stimmungsanalyse hinausgehen, zum Beispiel die Schlüsselbegriffserkennung, die automatische Spracherkennung usw. Weitere Informationen und Beispiele finden Sie in der [Dokumentation zur Textanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/).
>

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

Der Transkriptionsdienst kann eine große Anzahl an übermittelten Transkriptionen verarbeiten. Sie können den Status Ihrer Transkriptionen über ein `GET` in der [Transkriptionsmethode](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions) abfragen. Halten Sie die zurückgegebenen Informationen in einem vernünftigen Größenrahmen, indem Sie den Parameter `take` (einige Hundert) angeben. [Löschen Sie Transkriptionen](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription) regelmäßig aus dem Dienst, nachdem Sie die Ergebnisse abgerufen haben. Dadurch stellen Sie schnelle Antworten auf die Aufrufe der Transkriptionsverwaltung sicher.

## <a name="sample-code"></a>Beispielcode

Vollständige Beispiele stehen im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) innerhalb des Unterverzeichnisses `samples/batch` zur Verfügung.

Sie müssen den Beispielcode mit Ihren Abonnementinformationen, der Dienstregion, dem SAS-URI mit Verweis auf die zu transkribierende Audiodatei und die Modell-IDs anpassen, falls Sie ein benutzerdefiniertes Audio- oder Sprachmodell verwenden möchten.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Der Beispielcode richtet den Client ein und sendet die Transkriptionsanforderung. Anschließend fragt er die Statusinformationen ab und gibt Details zum Fortschritt der Transkription aus.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Vollständige Informationen zu den vorherigen Aufrufen finden Sie in unserer [Swagger-Dokumentation](https://westus.cris.ai/swagger/ui/index) (in englischer Sprache). Das vollständige hier gezeigte Beispiel finden Sie auf [GitHub](https://aka.ms/csspeech/samples) im Unterverzeichnis `samples/batch`.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der von Ihnen erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode ist kein benutzerdefiniertes Modell angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei bzw. Dateien. Zum Angeben der Modelle können Sie dieselbe Methode wie bei den Modell-IDs für das Akustikmodell und das Sprachmodell übergeben.

> [!NOTE]
> Für Basistranskriptionen müssen Sie die ID der Basismodelle nicht deklarieren. Wenn Sie nur eine Sprachmodell-ID (und keine Akustikmodell-ID) angeben, wird automatisch ein entsprechendes Akustikmodell ausgewählt. Wenn Sie nur eine Akustikmodell-ID angeben, wird automatisch ein entsprechendes Sprachmodell ausgewählt.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Sie finden das Beispiel im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) im Verzeichnis `samples/batch`.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
