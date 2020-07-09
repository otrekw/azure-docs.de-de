---
title: 'Spracherkennungs-API-Referenz (REST): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Spracherkennungs-REST-API verwenden. In diesem Artikel erfahren Sie mehr über Autorisierungs- und Abfrageoptionen sowie darüber, wie Sie eine Anforderung strukturieren und eine Antwort erhalten.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/13/2020
ms.author: yinhew
ms.openlocfilehash: c4eb1419859d4a87e53371a266dcef52e632b6c8
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636086"
---
# <a name="speech-to-text-rest-api"></a>Spracherkennungs-REST-API

Als Alternative zum [Speech SDK](speech-sdk.md) ermöglicht der Speech-Dienst das Konvertieren von Sprache in Text mithilfe einer REST-API. Jeder zugängliche Endpunkt ist einer Region zugeordnet. Ihre Anwendung benötigt einen Abonnementschlüssel für den Endpunkt, den Sie verwenden möchten. Die Möglichkeiten der REST-API sind stark eingeschränkt. Daher sollte die REST-API nur verwendet werden, wenn das [Speech SDK](speech-sdk.md) nicht verwendet werden kann.

Vor der Verwendung der Spracherkennungs-REST-API müssen Sie Folgendes verstanden haben:

* Anforderungen, die die REST-API verwenden und Audiodaten direkt übertragen, dürfen nur bis zu 60 Sekunden Audiodaten enthalten.
* Die Spracherkennung-REST-API gibt nur Endergebnisse zurück. Teilergebnisse werden nicht bereitgestellt.

Wenn das Senden von längerem Audio eine Anforderung für Ihre Anwendung ist, verwenden Sie das [Speech SDK](speech-sdk.md) oder eine dateibasierte REST-API wie die [Batchtranskription](batch-transcription.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Regionen und Endpunkte

Der Endpunkt für die REST-API weist das folgende Format auf:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

Ersetzen Sie `<REGION_IDENTIFIER>` durch den Bezeichner aus der folgenden Tabelle, der mit der Region Ihres Abonnements übereinstimmt:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> Der Sprachparameter muss an die URL angefügt werden, um HTTP Fehler des Typs „4xx“ zu vermeiden. Das folgende Beispiel zeigt die Spracheinstellung „Englisch (USA)“ bei Verwendung des Endpunkt „USA, Westen“: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

## <a name="query-parameters"></a>Abfrageparameter

Diese Parameter können in der Abfragezeichenfolge der REST-Anforderung enthalten sein.

| Parameter | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| `language` | Identifiziert die gesprochene Sprache, die erkannt wird. Siehe [Unterstützte Sprachen](language-support.md#speech-to-text). | Erforderlich |
| `format` | Gibt das Ergebnisformat an. Zulässige Werte sind `simple` und `detailed`. Einfache Ergebnisse enthalten `RecognitionStatus`, `DisplayText`, `Offset` und `Duration`. Ausführliche Antworten enthalten vier verschiedene Darstellungen des Anzeigetexts. Die Standardeinstellung ist `simple`. | Optional |
| `profanity` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `masked` (Obszönitäten werden durch Sternchen ersetzt), `removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `raw` (Obszönitäten sind im Ergebnis enthalten). Die Standardeinstellung ist `masked`. | Optional |
| `cid` | Wenn Sie das [Custom Speech-Portal](how-to-custom-speech.md) zum Erstellen von benutzerdefinierten Modellen verwenden, können Sie benutzerdefinierte Modelle über ihre **Endpunkt-ID** verwenden, die Sie auf der Seite **Bereitstellung** finden. Verwenden Sie die **Endpunkt-ID** als Argument für den Parameter `cid` der Abfragezeichenfolge. | Optional |

## <a name="request-headers"></a>Anforderungsheader

Diese Tabelle führt die erforderlichen und optionalen Header für Spracherkennungsanforderungen auf.

|Header| BESCHREIBUNG | Erforderlich/optional |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Ihr Abonnementschlüssel des Spracherkennungsdiensts. | Entweder dieser Header oder `Authorization` ist erforderlich. |
| `Authorization` | Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Weitere Informationen finden Sie unter [Authentifizierung](#authentication). | Entweder dieser Header oder `Ocp-Apim-Subscription-Key` ist erforderlich. |
| `Pronunciation-Assessment` | Gibt die Parameter für das Anzeigen von Aussprachebewertungen in Erkennungsergebnissen an, mit denen die Aussprachequalität der Spracheingabe mit Indikatoren für Genauigkeit, Flüssigkeit, Vollständigkeit usw. bewertet wird. Dieser Parameter ist base64-codierter JSON-Code, der viele ausführliche Parameter enthält. Informationen zum Erstellen dieses Headers finden Sie unter [Parameter für die Aussprachebewertung](#pronunciation-assessment-parameters). | Optional |
| `Content-type` | Beschreibt das Format und den Codec der bereitgestellten Audiodaten. Zulässige Werte sind `audio/wav; codecs=audio/pcm; samplerate=16000` und `audio/ogg; codecs=opus`. | Erforderlich |
| `Transfer-Encoding` | Gibt an, dass segmentierte Audiodaten anstatt einer einzelnen Datei gesendet werden. Verwenden Sie diesen Header nur, wenn Sie Audiodaten segmentieren. | Optional |
| `Expect` | Wenn Sie segmentierte Übertragung verwenden, senden Sie `Expect: 100-continue`. Der Spracherkennungsdienst bestätigt die ursprüngliche Anforderung und wartet auf weitere Daten.| Erforderlich, wenn segmentierte Audiodaten gesendet werden. |
| `Accept` | Wenn angegeben, muss der Wert `application/json` entsprechen. Der Speech-Dienst übermittelt Ergebnisse im JSON-Format. Einige Anforderungsframeworks bieten einen inkompatiblen Standardwert. Es ist eine bewährte Methode, `Accept` immer einzubeziehen. | Optional, wird jedoch empfohlen. |

## <a name="audio-formats"></a>Audioformate

Audiodaten werden im Text der HTTP-`POST`-Anforderung gesendet. Sie müssen in einem der in der folgenden Tabelle aufgeführten Formate vorliegen:

| Format | Codec | Bitrate | Samplingrate  |
|--------|-------|----------|--------------|
| WAV    | PCM   | 256 KBit/s | 16 kHz, mono |
| OGG    | OPUS  | 256 KBit/s | 16 kHz, mono |

>[!NOTE]
>Die oben genannten Formate werden durch die REST-API und WebSocket im Speech-Dienst unterstützt. Das [Speech-SDK](speech-sdk.md) unterstützt gegenwärtig das WAV-Format mit dem PCM-Codec sowie [weitere Formate](how-to-use-codec-compressed-audio-input-streams.md).

## <a name="pronunciation-assessment-parameters"></a>Parameter für die Aussprachebewertung

In dieser Tabelle sind die erforderlichen und optionalen Parameter für die Aussprachebewertung aufgeführt.

| Parameter | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| ReferenceText | Der Text, für den die Aussprache ausgewertet wird. | Erforderlich |
| GradingSystem | Das Punktesystem zur Kalibrierung der Bewertung. Zulässige Werte sind `FivePoint` und `HundredMark`. Die Standardeinstellung ist `FivePoint`. | Optional |
| Granularität | Die Granularität der Auswertung. Akzeptierte Werte sind `Phoneme`, für das die Bewertung auf Volltext-, Wort- und Phonemebene, `Word`, für das die Bewertung auf Volltext und Wortebene und `FullText`, für das nur die Bewertung auf der Volltextebene angezeigt wird. Die Standardeinstellung ist `Phoneme`. | Optional |
| Dimension | Definiert die Ausgabekriterien. Die akzeptierten Werte sind `Basic`, das nur die Genauigkeitsbewertung anzeigt, und `Comprehensive`, für das Bewertungen in weiteren Dimensionen angezeigt wird (z. B. Flüssigkeitsbewertung und Vollständigkeitsbewertung auf der Volltextebene, Fehlertyp auf der Wortebene). In den [Antwortparametern](#response-parameters) finden Sie Definitionen verschiedener Bewertungsdimensionen und Wortfehlertypen. Die Standardeinstellung ist `Basic`. | Optional |
| EnableMiscue | Aktiviert die Fehlschlagsberechnung. Wenn diese Option aktiviert ist, werden die ausgesprochenen Wörter mit dem Referenztext verglichen und auf der Grundlage des Vergleichs mit Auslassung/Einfügung gekennzeichnet. Zulässige Werte sind `False` und `True`. Die Standardeinstellung ist `False`. | Optional |
| ScenarioId | Eine GUID, die ein benutzerdefiniertes Punktesystem angibt. | Optional |

Unten finden Sie ein JSON-Beispiel mit den Parametern für die Aussprachebewertung:

```json
{
  "ReferenceText": "Good morning.",
  "GradingSystem": "HundredMark",
  "Granularity": "FullText",
  "Dimension": "Comprehensive"
}
```

Der folgende Beispielcode zeigt, wie die Parameter für die Aussprachebewertung in den Header `Pronunciation-Assessment` integriert werden:

```csharp
var pronAssessmentParamsJson = $"{{\"ReferenceText\":\"Good morning.\",\"GradingSystem\":\"HundredMark\",\"Granularity\":\"FullText\",\"Dimension\":\"Comprehensive\"}}";
var pronAssessmentParamsBytes = Encoding.UTF8.GetBytes(pronAssessmentParamsJson);
var pronAssessmentHeader = Convert.ToBase64String(pronAssessmentParamsBytes);
```

Für die Veröffentlichung von Audiodaten wird dringend das Hochladen per Streaming (segmentiert) empfohlen, da damit die Latenz deutlich reduziert werden kann. Weitere Informationen zum Aktivieren des Streamings finden Sie im [Beispielcode in verschiedenen Programmiersprachen](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/PronunciationAssessment).

>[!NOTE]
>Das Feature für die Aussprachebewertung ist derzeit nur in den Regionen `westus`, `eastasia` und `centralindia` verfügbar. Es steht außerdem zurzeit nur für die Sprache `en-US` zur Verfügung.

## <a name="sample-request"></a>Beispiel für eine Anforderung

Das folgende Beispiel enthält den Hostnamen und die erforderlichen Header. Beachten Sie, dass der Dienst auch Audiodaten erwartet, die in diesem Beispiel nicht enthalten sind. Wie bereits erwähnt, wird die Segmentierung empfohlen, ist aber nicht erforderlich.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

Sie können den unten angegebenen Header hinzufügen, um die Aussprachebewertung zu aktivieren. Informationen zum Erstellen dieses Headers finden Sie unter [Parameter für die Aussprachebewertung](#pronunciation-assessment-parameters).

```HTTP
Pronunciation-Assessment: eyJSZWZlcm...
```

## <a name="http-status-codes"></a>HTTP-Statuscodes

Der HTTP-Statuscode jeder Antwort zeigt den Erfolg oder allgemeine Fehler an.

| HTTP-Statuscode | BESCHREIBUNG | Mögliche Ursache |
|------------------|-------------|-----------------|
| `100` | Continue | Die ursprüngliche Anforderung wurde akzeptiert. Mit dem Senden der restlichen Daten fortfahren. (Wird bei segmentierter Übertragung verwendet) |
| `200` | OK | Die Anforderung war erfolgreich. Der Antworttext ist ein JSON-Objekt. |
| `400` | Ungültige Anforderung | Der Sprachcode wurde nicht bereitgestellt, ist keine unterstützte Sprache, eine ungültige Audiodatei usw. |
| `401` | Nicht autorisiert | Der Abonnementschlüssel oder das Autorisierungstoken ist in der angegebenen Region ungültig oder ungültiger Endpunkt. |
| `403` | Verboten | Fehlender Abonnementschlüssel oder fehlendes Autorisierungstoken. |

## <a name="chunked-transfer"></a>Segmentierte Übertragung

Mithilfe der segmentierten Übertragung (`Transfer-Encoding: chunked`) kann die Erkennungslatenz verringert werden. Es ermöglicht dem Speech-Dienst, mit der Verarbeitung der Audiodatei zu beginnen, während sie übertragen wird. Der REST-API bietet keine Teil- oder Zwischenergebnisse.

Dieses Codebeispiel zeigt, wie Sie Audio in Blöcken senden. Nur der erste Block sollte den Header der Audiodatei enthalten. `request` ist ein `HttpWebRequest`-Objekt, das mit dem entsprechenden REST-Endpunkt verbunden ist. `audioFile` ist der Pfad zu einer Audiodatei auf dem Datenträger.

```csharp
var request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = host;
request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";
request.AllowWriteStreamBuffering = false;

using (var fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    // Open a request stream and write 1024 byte chunks in the stream one at a time.
    byte[] buffer = null;
    int bytesRead = 0;
    using (var requestStream = request.GetRequestStream())
    {
        // Read 1024 raw bytes from the input audio file.
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>Antwortparameter

Ergebnisse werden im JSON-Format bereitgestellt. Das `simple`-Format schließt diese Felder auf oberster Ebene ein.

| Parameter | BESCHREIBUNG  |
|-----------|--------------|
|`RecognitionStatus`|Status, z.B. `Success` für erfolgreiche Erkennung. Siehe nächste Tabelle.|
|`DisplayText`|Der erkannte Text nach Großschreibung, Interpunktion, inverser Textnormalisierung (Umwandlung von gesprochenem Text in kürzere Formen, z.B. 200 für „zweihundert“ oder „Dr. Smith“ für „doctor smith“) und Obszönitätenmaskierung. Nur bei Erfolg vorhanden.|
|`Offset`|Die Zeit (in Einheiten von 100 Nanosekunden), zu der die erkannte Sprache im Audiostream beginnt.|
|`Duration`|Die Dauer (in Einheiten von 100 Nanosekunden) der erkannten Sprache im Audiostream.|

Das `RecognitionStatus`-Feld kann diese Werte enthalten:

| Status | BESCHREIBUNG |
|--------|-------------|
| `Success` | Die Erkennung war erfolgreich, und das `DisplayText`-Feld ist vorhanden. |
| `NoMatch` | Im Audiodatenstrom wurde Sprache erkannt, aber es wurde keine Übereinstimmung mit Wörtern aus der Zielsprache festgestellt. Normalerweise bedeutet dies, dass die Erkennungssprache eine andere Sprache ist als die, die der Benutzer spricht. |
| `InitialSilenceTimeout` | Der Anfang des Audiodatenstroms enthielt nur Stille, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `BabbleTimeout` | Der Anfang des Audiodatenstroms enthielt nur Rauschen, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `Error` | Der Erkennungsdienst hat einen internen Fehler erkannt und konnte nicht fortgesetzt werden. Versuchen Sie es noch mal, wenn möglich. |

> [!NOTE]
> Wenn die Audiodaten nur aus Obszönitäten bestehen und der `profanity`-Abfrageparameter auf `remove` festgelegt ist, gibt der Dienst kein Sprachergebnis zurück.

Das Format `detailed` enthält zusätzliche Formen erkannter Ergebnisse.
Bei Verwendung des Formats `detailed` wird `DisplayText` als `Display` für jedes Ergebnis in der `NBest`-Liste angegeben.

Das Objekt in der `NBest`-Liste kann Folgendes enthalten:

| Parameter | BESCHREIBUNG |
|-----------|-------------|
| `Confidence` | Die Zuverlässigkeitsbewertung des Eintrags von 0,0 (keine Zuverlässigkeit) bis 1,0 (volle Zuverlässigkeit) |
| `Lexical` | Die lexikalische Form des erkannten Texts: die tatsächlich erkannten Wörter. |
| `ITN` | Die inverse Textnormalisierung („kanonische Form“) des erkannten Texts mit Telefonnummern, Zahlen, Abkürzungen („doctor smith“ in „dr. smith“) und anderen angewendeten Transformationen. |
| `MaskedITN` | Die ITN-Form mit angewendeter Obszönitätenmaskierung, wenn angefordert. |
| `Display` | Die Anzeigeform des erkannten Texts mit hinzugefügten Satzzeichen und Großschreibung. Dieser Parameter ist derselbe wie `DisplayText`, wenn das Format `simple` entspricht. |
| `AccuracyScore` | Der Score, der die Aussprachegenauigkeit des übergebenen Sprachtexts angibt. |
| `FluencyScore` | Der Score, der die Flüssigkeit des übergebenen Sprachtexts angibt. |
| `CompletenessScore` | Der Score, der die Vollständigkeit des übergebenen Sprachtexts durch Berechnen des Verhältnisses ausgesprochener Wörter zur Gesamteingabe angibt. |
| `PronScore` | Der Gesamtscore, der die Aussprachequalität des übergebenen Sprachtexts angibt. Dieser wird aus `AccuracyScore`, `FluencyScore` und `CompletenessScore` mit Gewichtung berechnet. |
| `ErrorType` | Dieser Wert gibt an, ob ein Wort im Vergleich zu `ReferenceText`ausgelassen, eingefügt oder schlecht ausgesprochen wird. Die möglichen Werte sind `None` (d. h. kein Fehler in diesem Wort), `Omission`, `Insertion` und `Mispronunciation`. |

## <a name="sample-responses"></a>Beispielantworten

Eine typische Antwort für die `simple` Erkennung:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Eine typische Antwort für die `detailed` Erkennung:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      }
  ]
}
```

Eine typische Antwort für Erkennung mit Bewertung der Aussprache:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "400000",
  "Duration": "11000000",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "good morning",
        "ITN" : "good morning",
        "MaskedITN" : "good morning",
        "Display" : "Good morning.",
        "PronScore" : 84.4,
        "AccuracyScore" : 100.0,
        "FluencyScore" : 74.0,
        "CompletenessScore" : 100.0,
        "Words": [
            {
              "Word" : "Good",
              "AccuracyScore" : 100.0,
              "ErrorType" : "None",
              "Offset" : 500000,
              "Duration" : 2700000
            },
            {
              "Word" : "morning",
              "AccuracyScore" : 100.0,
              "ErrorType" : "None",
              "Offset" : 5300000,
              "Duration" : 900000
            }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
