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
ms.openlocfilehash: 3e7f310f37bd016a73c589db3c9a23e197465427
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88053915"
---
# <a name="what-is-batch-transcription"></a>Was ist die Batch-Transkription?

Bei der Batch-Transkription handelt es sich um eine Reihe von Rest-API-Vorgängen, mit denen Sie große Mengen von Audiodaten im Speicher transkribieren können. Sie können per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionsergebnisse asynchron empfangen. Mit der neuen API der Version 3.0 haben Sie die Möglichkeit, eine oder mehrere Audiodateien zu transkribieren oder einen ganzen Speichercontainer zu verarbeiten.

Die asynchrone Transkription für die Spracherkennung ist nur eine der verfügbaren Funktionen. Mit den REST-APIs für die Batch-Transkription können Sie die folgenden Methoden aufrufen:



|    Batch-Transkriptionsvorgang                                             |    Methode    |    REST-API-Aufruf                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    Erstellt eine neue Transkription.                                              |    POST      |    speechtotext/v3.0/transcriptions            |
|    Ruft eine Liste von Transkriptionen für das authentifizierte Abonnement ab.    |    GET       |    speechtotext/v3.0/transcriptions            |
|    Ruft eine Liste der unterstützten Gebietsschemas für Offlinetranskriptionen ab.              |    GET       |    speechtotext/v3.0/transcriptions/locales    |
|    Aktualisiert die änderbaren Details der durch die ID angegebenen Transkription.    |    PATCH     |    speechtotext/v3.0/transcriptions/{id}       |
|    Löscht die angegebene Transkriptionsaufgabe.                                 |    Delete    |    speechtotext/v3.0/transcriptions/{id}       |
|    Ruft die durch die ID angegebene Transkription ab.                        |    GET       |    speechtotext/v3.0/transcriptions/{id}       |
|    Ruft die Ergebnisdateien der durch die ID angegebenen Transkription ab.    |    GET       |    speechtotext/v3.0/transcriptions/{id}/files |




Sie können die ausführliche API prüfen und testen. Sie steht als [Swagger-Dokument](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) zur Verfügung.

Batch-Transkriptionsaufträge werden auf der Grundlage des optimalen Arbeitsaufwands geplant. Zurzeit lässt sich nicht einschätzen, wann ein Auftrag in den Ausführungsstatus wechselt. Bei normaler Systemlast sollte dies innerhalb von Minuten erfolgen. Sobald er ausgeführt wird, wird die aktuelle Transkription schneller als in Audioechtzeit verarbeitet.

Über die benutzerfreundliche API hinaus ist keine Bereitstellung benutzerdefinierter Endpunkte erforderlich, und es sind keine Einschränkungen wegen paralleler Ausführung zu beachten.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="subscription-key"></a>Abonnementschlüssel

Wie bei allen Features des Speech-Diensts erstellen Sie mithilfe der Anleitung unter [Erste Schritte](get-started.md) einen Abonnementschlüssel im [Azure-Portal](https://portal.azure.com).

>[!NOTE]
> Für die Verwendung der Batch-Transkription ist ein Standardabonnement (S0) für den Speech-Dienst erforderlich. Kostenlose Abonnementschlüssel (F0) funktionieren nicht. Weitere Informationen finden Sie unter [Preise und Einschränkungen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Benutzerdefinierte Modelle

Wenn Sie planen, Modelle anzupassen, befolgen Sie die Schritte unter [Akustische Anpassung](how-to-customize-acoustic-models.md) und [Sprachanpassung](how-to-customize-language-model.md). Um die erstellten Modelle in der Batch-Transkription zu verwenden, benötigen Sie ihren Modellspeicherort. Der Modellspeicherort lässt sich durch Untersuchen der Details des Modells ermitteln (`self`-Eigenschaft). Für den Batch-Transkriptionsdienst ist *kein* bereitgestellter benutzerdefinierter Endpunkt erforderlich.

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

Konfigurationsparameter werden als JSON-Code (eine oder mehrere Einzeldateien) bereitgestellt:

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

Konfigurationsparameter werden als JSON-Code (Verarbeitung eines ganzen Speichercontainers) bereitgestellt:

```json
{
  "contentContainerUrl": "<SAS URL to the Azure blob container to transcribe>",
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of container using default model for en-US"
}
```

Zum Verwenden benutzerdefinierter, trainierter Modelle in Batch-Transkriptionen kann auf diese (wie unten gezeigt) verwiesen werden:

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "model": {
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}"
  },
  "displayName": "Transcription of file using default model for en-US"
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
      `profanityFilterMode`
   :::column-end:::
   :::column span="2":::
      Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind: `None` (deaktiviert den Obszönitätenfilter), `Masked` (Obszönitäten werden durch Sternchen ersetzt), `Removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `Tags` (fügt „Obszönität“-Tags ein). Die Standardeinstellung ist `Masked`.
:::row-end:::
:::row:::
   :::column span="1":::
      `punctuationMode`
   :::column-end:::
   :::column span="2":::
      Gibt den Umgang mit Satzzeichen in Erkennungsergebnissen an. Zulässige Werte sind: `None` (deaktiviert die Interpunktion), `Dictated` (impliziert explizite (gesprochene) Interpunktion), `Automatic` (überlässt dem Decoder die Interpunktion) oder `DictatedAndAutomatic` (verwendet diktierte und automatische Interpunktion). Die Standardeinstellung ist `DictatedAndAutomatic`.
:::row-end:::
:::row:::
   :::column span="1":::
      `wordLevelTimestampsEnabled`
   :::column-end:::
   :::column span="2":::
      Gibt an, ob der Ausgabe Zeitstempel auf Wortebene hinzugefügt werden sollen. Zulässige Werte sind `true` zum Aktivieren und `false` (Standardwert) zum Deaktivieren von Zeitstempeln auf Wortebene.
:::row-end:::
:::row:::
   :::column span="1":::
      `diarizationEnabled`
   :::column-end:::
   :::column span="2":::
      Gibt an, dass die Diarisierungsanalyse für die Eingabe durchgeführt werden soll. Es wird erwartet, dass diese Eingabe ein Monokanal mit zwei Stimmen ist. Zulässige Werte sind `true` zum Aktivieren und `false` (Standardwert) zum Deaktivieren der Diarisierung. Außerdem muss `wordLevelTimestampsEnabled` auf „true“ festgelegt werden.
:::row-end:::
:::row:::
   :::column span="1":::
      `channels`
   :::column-end:::
   :::column span="2":::
      Ein optionales Array von zu verarbeitenden Kanalnummern. Hier kann eine Teilmenge der verfügbaren Kanäle in der Audiodatei zur Verarbeitung angegeben werden (z. B. nur `0`). Wenn keine Angabe erfolgt, werden die Kanäle `0` und `1` standardmäßig transkribiert.
:::row-end:::
:::row:::
   :::column span="1":::
      `timeToLive`
   :::column-end:::
   :::column span="2":::
      Eine optionale Dauer zum automatischen Löschen von Transkriptionen nach Abschluss der Transkription. `timeToLive` ist bei der Massenverarbeitung von Transkriptionen nützlich, um sicherzustellen, dass sie schließlich gelöscht werden (z. B. `PT12H`). Wenn keine Angabe erfolgt oder die Option auf `PT0H` festgelegt ist, wird die Transkription nicht automatisch gelöscht.
:::row-end:::
:::row:::
   :::column span="1":::
      `destinationContainerUrl`
   :::column-end:::
   :::column span="2":::
      Optionale URL mit [Dienst-SAS](../../storage/common/storage-sas-overview.md) zu einem beschreibbaren Container in Azure. Das Ergebnis wird in diesem Container gespeichert. Wenn keine Angabe erfolgt, speichert Microsoft die Ergebnisse in einem von Microsoft verwalteten Speichercontainer. Wenn die Transkription durch Aufruf von [Transkription löschen](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription) gelöscht wird, werden die Ergebnisdaten ebenfalls gelöscht.
:::row-end:::

### <a name="storage"></a>Storage

Die Batchtranskription unterstützt [Azure Blob-Speicher](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) zum Lesen von Audio und zum Schreiben von Transkriptionen in den Speicher.

## <a name="the-batch-transcription-result"></a>Das Ergebnis der Batch-Transkription

Für jeden Audioeingang wird eine Transkriptions-Ergebnisdatei erstellt. Sie können die Liste der Ergebnisdateien abrufen, indem Sie [Transkriptionsdateien abrufen](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptionFiles) aufrufen. Diese Methode gibt eine Liste der Ergebnisdateien für diese Transkription zurück. Filtern Sie alle zurückgegebenen Dateien mit `kind` == `Transcription` und `name` == `{originalInputName.suffix}.json`, um die Transkriptionsdatei für eine bestimmte Eingabedatei zu finden.

Jedes Transkriptionsergebnis hat das folgende Format:

```json
{
  "source": "...",                                                 // the sas url of a given contentUrl or the path relative to the root of a given container
  "timestamp": "2020-06-16T09:30:21Z",                             // creation time of the transcription, ISO 8601 encoded timestamp, combined date and time
  "durationInTicks": 41200000,                                     // total audio duration in ticks (1 tick is 100 nanoseconds)
  "duration": "PT4.12S",                                           // total audio duration, ISO 8601 encoded duration
  "combinedRecognizedPhrases": [                                   // concatenated results for simple access in single string for each channel
    {
      "channel": 0,                                                // channel number of the concatenated results
      "lexical": "hello world",
      "itn": "hello world",
      "maskedITN": "hello world",
      "display": "Hello world."
    }
  ],
  "recognizedPhrases": [                                           // results for each phrase and each channel individually
    {
      "recognitionStatus": "Success",                              // recognition state, e.g. "Success", "Failure"
      "channel": 0,                                                // channel number of the result
      "offset": "PT0.07S",                                         // offset in audio of this phrase, ISO 8601 encoded duration 
      "duration": "PT1.59S",                                       // audio duration of this phrase, ISO 8601 encoded duration
      "offsetInTicks": 700000.0,                                   // offset in audio of this phrase in ticks (1 tick is 100 nanoseconds)
      "durationInTicks": 15900000.0,                               // audio duration of this phrase in ticks (1 tick is 100 nanoseconds)
      
      // possible transcriptions of the current phrase with confidences
      "nBest": [
        {
          "confidence": 0.898652852,                               // confidence value for the recognition of the whole phrase
          "speaker": 1,                                            // if `diarizationEnabled` is `true`, this is the identified speaker (1 or 2), otherwise this property is not present
          "lexical": "hello world",
          "itn": "hello world",
          "maskedITN": "hello world",
          "display": "Hello world.",
          
          // if wordLevelTimestampsEnabled is `true`, there will be a result for each word of the phrase, otherwise this property is not present
          "words": [
            {
              "word": "hello",
              "offset": "PT0.09S",
              "duration": "PT0.48S",
              "offsetInTicks": 900000.0,
              "durationInTicks": 4800000.0,
              "confidence": 0.987572
            },
            {
              "word": "world",
              "offset": "PT0.59S",
              "duration": "PT0.16S",
              "offsetInTicks": 5900000.0,
              "durationInTicks": 1600000.0,
              "confidence": 0.906032
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
      `lexical`
   :::column-end:::
   :::column span="2":::
      Die tatsächlich erkannten Wörter
:::row-end:::
:::row:::
   :::column span="1":::
      `itn`
   :::column-end:::
   :::column span="2":::
      Der erkannte Text in Form von inverser Textnormalisierung. Abkürzungen („Doktor Schmidt“ zu „Dr. Schmidt“), Telefonnummern und weitere Transformationen werden angewendet.
:::row-end:::
:::row:::
   :::column span="1":::
      `maskedITN`
   :::column-end:::
   :::column span="2":::
      Die ITN-Form mit angewendeter Maskierung von Obszönitäten
:::row-end:::
:::row:::
   :::column span="1":::
      `display`
   :::column-end:::
   :::column span="2":::
      Die Anzeigeform des erkannten Texts. Hinzugefügte Satzzeichen und Groß-/Kleinschreibung sind enthalten.
:::row-end:::

## <a name="speaker-separation-diarization"></a>Sprechertrennung (Diarisierung)

Diarisierung ist der Prozess, bei dem Sprecher in einem Audioelement getrennt werden. Unsere Batch-Pipeline unterstützt die Diarisierung und kann zwei Sprecher in Monokanalaufnahmen erkennen. Die Funktion ist für Stereoaufzeichnungen nicht verfügbar.

Die Ausgabe der Transkription mit aktivierter Diarisierung enthält einen `Speaker`-Eintrag für jeden transkribierten Ausdruck. Wenn die Diarisierung nicht verwendet wird, ist die Eigenschaft `Speaker` in der JSON-Ausgabe nicht vorhanden. Bei der Diarisierung werden zwei Stimmen unterstützt, sodass die Sprecher als `1` oder `2` identifiziert werden.

Wenn Sie Diarisierung anfordern möchten, müssen Sie einfach der HTTP-Anforderung den relevanten Parameter wie unten gezeigt hinzufügen.

 ```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "diarizationEnabled": true,
    "wordLevelTimestampsEnabled": true,
    "punctuationMode": "DictatedAndAutomatic",
    "profanityFilterMode": "Masked"
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

Zeitstempel auf Wortebene müssen aktiviert werden, wie die Parameter in der obigen Anforderung angeben.

## <a name="best-practices"></a>Bewährte Methoden

Der Transkriptionsdienst kann eine große Anzahl an übermittelten Transkriptionen verarbeiten. Sie können den Status Ihrer Transkriptionen über ein `GET` unter [Transkriptionen abrufen](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptions) abfragen. Rufen Sie [Transkription löschen](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription) regelmäßig aus dem Dienst auf, nachdem Sie die Ergebnisse abgerufen haben. Alternativ kann die Eigenschaft `timeToLive` auf einen angemessenen Wert festgelegt werden, um eine eventuelle Löschung der Ergebnisse sicherzustellen.

## <a name="sample-code"></a>Beispielcode

Vollständige Beispiele stehen im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) innerhalb des Unterverzeichnisses `samples/batch` zur Verfügung.

Aktualisieren Sie den Beispielcode mit Ihren Abonnementinformationen, der Dienstregion, dem SAS-URI mit Verweis auf die zu transkribierende Audiodatei und den Modellspeicherort, falls Sie ein benutzerdefiniertes Modell verwenden möchten.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptiondefinition)]

Der Beispielcode richtet den Client ein und sendet die Transkriptionsanforderung. Anschließend fragt er die Statusinformationen ab und gibt Details zum Fortschritt der Transkription aus.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptionstatus)]

Vollständige Informationen zu den vorherigen Aufrufen finden Sie in unserer [Swagger-Dokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) (in englischer Sprache). Das vollständige hier gezeigte Beispiel finden Sie auf [GitHub](https://aka.ms/csspeech/samples) im Unterverzeichnis `samples/batch`.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der von Ihnen erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Zustände. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode ist kein benutzerdefiniertes Modell angegeben. Der Dienst verwendet das Basismodell zum Transkribieren der Datei bzw. Dateien. Zur Angabe des Modells können Sie derselben Methode die Modellreferenz für das benutzerdefinierte Modell übergeben.

> [!NOTE]
> Für Basistranskriptionen müssen Sie die ID des Basismodells nicht deklarieren.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Sie finden das Beispiel im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) im Verzeichnis `samples/batch`.

## <a name="next-steps"></a>Nächste Schritte

- [API-Referenz für Spracherkennung v3](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CopyModelToSubscription)
