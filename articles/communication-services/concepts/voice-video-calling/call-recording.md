---
title: 'Azure Communication Services: Übersicht über die Anrufaufzeichnung'
titleSuffix: An Azure Communication Services concept document
description: Hier finden Sie eine Übersicht über das Feature und die APIs für die Anrufaufzeichnung.
author: joseys
manager: anvalent
services: azure-communication-services
ms.author: joseys
ms.date: 04/13/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: df9638588b4d677a7ceb80bafbe7157bb5c2df42
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107730328"
---
# <a name="calling-recording-overview"></a>Übersicht über die Anrufaufzeichnung

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

> [!NOTE]
> In vielen Ländern und Staaten gibt es Gesetze und Vorschriften im Zusammenhang der Aufzeichnung von Telefon-, Sprach- und Videoanrufen. Diese schreiben häufig die Einwilligung des Benutzers in die Aufzeichnung seiner Kommunikation vor. Es liegt in Ihrer Verantwortung, die Anrufaufzeichnungsfunktionen im Einklang mit dem Gesetz zu verwenden. Die Einwilligung der an aufgezeichneter Kommunikation beteiligten Parteien muss auf eine Weise eingeholt werden, die den für den jeweiligen Teilnehmer geltenden Gesetzen entspricht.

> [!NOTE]
> Bestimmungen im Zusammenhang mit der Pflege personenbezogener Daten erfordern die Möglichkeit zum Exportieren von Benutzerdaten. Zur Erfüllung dieser Anforderungen enthalten Aufzeichnungsmetadatendateien die Teilnehmer-ID für jeden Aufrufteilnehmer im Array `participants`. Sie können die MRIs im Array `participants` mit Ihren internen Benutzeridentitäten abgleichen, um Aufrufteilnehmer zu identifizieren. Weiter unten finden Sie ein Beispiel für eine Aufzeichnungsmetadatendatei als Referenz.

Von der Anrufaufzeichnung wird eine Reihe von APIs zum Starten, Beenden, Anhalten und Fortsetzen der Aufzeichnung bereitgestellt. Auf diese APIs kann über serverseitige Geschäftslogik oder über durch Benutzeraktionen ausgelöste Ereignisse zugegriffen werden. Aufgezeichnete Medien werden im Format `MP4 Audio+Video` ausgegeben. Dieses Format wird auch von Teams zum Aufzeichnen von Medien verwendet. Benachrichtigungen im Zusammenhang mit Medien und Metadaten werden über Event Grid ausgegeben. Aufzeichnungen werden 48 Stunden lang in einem integrierten temporären Speicher gespeichert, wo sie abgerufen und in eine langfristige Speicherlösung Ihrer Wahl verschoben werden können. 

## <a name="run-time-control-apis"></a>Runtimesteuerungs-APIs
Runtimesteuerungs-APIs können verwendet werden, um die Aufzeichnung über interne Geschäftslogiktrigger (beispielsweise eine Anwendung, die einen Gruppenanruf erstellt und das Gespräch aufzeichnet) oder über eine vom Benutzer ausgelöste Aktion zu verwalten, die die Serveranwendung anweist, mit der Aufzeichnung zu beginnen. In beiden Szenarien wird die Konversations-ID (`<conversation-id>`) benötigt, um eine bestimmte Besprechung oder einen bestimmten Anruf aufzuzeichnen. 

#### <a name="getting-conversation-id-from-a-server-initiated-call"></a>Ermitteln der Konversations-ID im Falle eines serverseitig initiierten Anrufs

Eine Konversations-ID (`ConversationId`) wird über das Ereignis `Microsoft.Communication.CallLegStateChanged` zurückgegeben. Diese Ereignisbenachrichtigung wird ausgegeben, nachdem eine Verbindung für den Anruf hergestellt wurde. Sie befindet sich im Feld `data.ConversationId`. Dieser Wert kann direkt als Parameter `{conversationId}` in Runtimesteuerungs-APIs verwendet werden:
```
      {
        "id": null,
        "topic": null,
        "subject": "callLeg/<callLegId>/callState",
        "data": {
---->       "ConversationId": "<conversation-id>",    <----
            "CallLegId": "<callLegId>",
            "CallState": "Established"
        },
        "eventType": "Microsoft.Communication.CallLegStateChanged",
        "eventTime": "2021-04-14T16:32:34.1115003Z",
        "metadataVersion": null,
        "dataVersion": null
    }
```
                                                            
#### <a name="getting-the-conversation-id-from-a-user-triggered-event-on-the-client"></a>Ermitteln der Konversations-ID im Falle eines von Benutzer ausgelösten Ereignisses im Client

Rufen Sie nach dem Herstellen einer Anrufverbindung `let result = call.info.getConversationUrl()` über die JavaScript-Bibliothek `@azure/communication-calling` auf, um die Konversations-URL (`conversationUrl`) zu erhalten, und **codieren Sie die Konversations-URL (`conversationUrl`) mit „Base64Url“, um die Konversations-ID (`{conversationId}`) für die Runtimesteuerungs-APIs zu erhalten**. Die Codierung kann entweder clientseitig (vor dem Senden des Ereignisses an den Server) oder serverseitig durchgeführt werden.

Beachten Sie, dass die Konversations-URL (`conversationUrl`) mit „Base64Url“ codiert werden *muss*, nicht nur mit „Base64“ (btoa).                                                            

### <a name="start-recording"></a>Starten der Aufzeichnung

#### <a name="request"></a>Anforderung

**HTTP**
<!-- {
  "blockType": "request",
  "name": "start-recording"
}-->
```
POST /conversations/{conversationId}/Recordings
Content-Type: application/json

{
  "operationContext": "string", // developer provided string for correlation context on each operation
  "recordingStateCallbackUri": "string"
}
```
**C# SDK**
<!-- {
  "blockType": "request",
  "name": "start-recording"
}-->
```C#
string connectionString = "YOUR_CONNECTION_STRING";
ConversationClient conversationClient = new ConversationClient(connectionString);

/// start call recording
StartRecordingResponse startRecordingResponse = await conversationClient.StartRecordingAsync(
    conversationId: "<conversation-id>"
    operationContext: "<operation-context>", /// developer provided string for correlation context on each operation
    recordingStateCallbackUri: "<recording-state-callback-uri>").ConfigureAwait(false);

string recordingId = startRecordingResponse.RecordingId;
```

#### <a name="response"></a>Antwort

**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 200 Success
Content-Type: application/json

{
  "recordingId": "string"
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 404 Not found
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```

### <a name="get-call-recording-state"></a>Abrufen des Zustands der Anrufaufzeichnung

#### <a name="request"></a>Anforderung

**HTTP**
<!-- {
  "blockType": "request",
  "name": "get-recording-state"
}-->
```http
GET /conversations/{conversationId}/recordings/{recordingId}
Content-Type: application/json

{
}
```
**C# SDK**
<!-- {
  "blockType": "request",
  "name": "start-recording"
}-->
```C#
string connectionString = "YOUR_CONNECTION_STRING";
ConversationClient conversationClient = new ConversationClient(connectionString);

/// get recording state
GetCallRecordingStateResponse recordingState = await conversationClient.GetRecordingStateAsync(
    conversationId: "<conversation-id>",
    recordingId: <recordingId>).ConfigureAwait(false);
```
#### <a name="response"></a>Antwort

**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 200 Success
Content-Type: application/json

{
  "recordingState": "active"
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```

### <a name="stop-recording"></a>Aufzeichnung beenden
#### <a name="request"></a>Anforderung
**HTTP**
<!-- {
  "blockType": "request",
  "name": "stop-recording"
}-->
```
DELETE /conversations/{conversationId}/recordings/{recordingId}
Content-Type: application/json

{
  "operationContext": "string" // developer provided string for correlation context on each operation
}
```
**C# SDK**
<!-- {
  "blockType": "request",
  "name": "start-recording"
}-->
```C#
string connectionString = "YOUR_CONNECTION_STRING";
ConversationClient conversationClient = new ConversationClient(connectionString);

/// stop recording
StopRecordingResponse response = conversationClient.StopRecordingAsync(
    conversationId: "<conversation-id>",
    recordingId: <recordingId>,
    operationContext: "<operation-context>").ConfigureAwait(false);
```
#### <a name="response"></a>Antwort
**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 200 Success
Content-Type: application/json

{
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```

### <a name="pause-recording"></a>Aufzeichnung anhalten
Durch Anhalten und Fortsetzen der Anrufaufzeichnung können Sie die Aufzeichnung eines Teils eines Anrufs oder einer Besprechung überspringen und die Aufzeichnung in einer einzelnen Datei fortsetzen. 
#### <a name="request"></a>Anforderung
**HTTP**
<!-- {
  "blockType": "request",
  "name": "pause-recording"
}-->
```
POST /conversations/{conversationId}/recordings/{recordingId}/Pause
Content-Type: application/json

{
  "operationContext": "string" // developer provided string for correlation context on each operation
}
```
**C# SDK**
<!-- {
  "blockType": "request",
  "name": "start-recording"
}-->
```C#
string connectionString = "YOUR_CONNECTION_STRING";
ConversationClient conversationClient = new ConversationClient(connectionString);

/// pause recording
PauseRecordingResponse response = conversationClient.PauseRecordingAsync(
    conversationId: "<conversation-id>",
    recordingId: <recordingId>,
    operationContext: "<operation-context>").ConfigureAwait(false);
```
#### <a name="response"></a>Antwort
**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 200 Success
Content-Type: application/json

{
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```

### <a name="resume-recording"></a>Fortsetzen der Aufzeichnung
#### <a name="request"></a>Anforderung
**HTTP**
<!-- {
  "blockType": "request",
  "name": "resume-recording"
}-->
```
POST /conversations/{conversationId}/recordings/{recordingId}/Resume
Content-Type: application/json

{
  "operationContext": "string" // developer provided string for correlation context on each operation
}
```
**C# SDK**
<!-- {
  "blockType": "request",
  "name": "start-recording"
}-->
```C#
string connectionString = "YOUR_CONNECTION_STRING";
ConversationClient conversationClient = new ConversationClient(connectionString);

/// resume recording
ResumeRecordingResponse response = conversationClient.ResumeRecordingAsync(
    conversationId: "<conversation-id>",
    recordingId: <recordingId>,
    operationContext: "<operation-context>").ConfigureAwait(false);
```
#### <a name="response"></a>Antwort
**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 200 Success
Content-Type: application/json

{
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```

## <a name="media-output-types"></a>Medienausgabetypen
Von der Anrufaufzeichnung wird derzeit das gemischte MP4-Ausgabeformat für Audio und Video unterstützt. Das Ausgabemedium entspricht Besprechungsaufzeichnungen, die durch die Aufzeichnungsfunktion von Microsoft Teams generiert werden.

| Channeltyp | Inhaltsformat | Video | Audio |
| :----------- | :------------- | :---- | :--------------------------- |
| audioVideo | MP4 | Video (1.920 × 1.080, 8 FPS) aller Teilnehmer in Standardkachelanordnung | Gemischtes Audio (16 kHz, MP4A) aller Teilnehmer |

## <a name="event-grid-notifications"></a>Event Grid-Benachrichtigungen
Eine Event Grid-Benachrichtigung (`Microsoft.Communication.RecordingFileStatusUpdated`) wird veröffentlicht, wenn eine Aufzeichnung abrufbereit ist. Dies ist in der Regel ein bis zwei Minuten nach Abschluss des Aufzeichnungsprozesses der Fall (also beispielweise nach dem Ende der Besprechung oder nach dem Beenden der Aufzeichnung). Aufzeichnungsereignisbenachrichtigungen enthalten eine Dokument-ID, mit der sowohl aufgezeichnete Medien als auch eine Aufzeichnungsmetadatendatei abgerufen werden können:

- <Azure Communication Services-Endpunkt>/recording/download/{documentId}
- <Azure Communication Services-Endpunkt>/recording/download/{documentId}/metadata

Beispielcode für die Behandlung von Event Grid-Benachrichtigungen und das Herunterladen von Aufzeichnungs- und Metadatendateien finden Sie [hier](../../quickstarts/voice-video-calling/download-recording-file-sample.md). 

### <a name="notification-schema"></a>Benachrichtigungsschema
```
{
    "id": string, // Unique guid for event
    "topic": string, // Azure Communication Services resource id
    "subject": string, // /recording/call/{call-id}
    "data": {
        "recordingStorageInfo": {
            "recordingChunks": [
                {
                    "documentId": string, // Document id for retrieving from storage
                    "index": int, // Index providing ordering for this chunk in the entire recording
                    "endReason": string, // Reason for chunk ending: "SessionEnded", "ChunkMaximumSizeExceeded”, etc.
                }
            ]
        },
        "recordingStartTime": string, // ISO 8601 date time for the start of the recording
        "recordingDurationMs": int, // Duration of recording in milliseconds
        "sessionEndReason": string // Reason for call ending: "CallEnded", "InitiatorLeft", etc.
    },
    "eventType": string, // "Microsoft.Communication.RecordingFileStatusUpdated"
    "dataVersion": string, // "1.0"
    "metadataVersion": string, // "1"
    "eventTime": string // ISO 8601 date time for when the event was created
}
```
## <a name="file-download"></a>Dateidownload

> Azure Communication Services bietet eine kurzfristige Medienspeicherung für Aufzeichnungen. **Aufgezeichnete Inhalte, die Sie behalten möchten, müssen innerhalb von 48 Stunden exportiert werden.** Nach 48 Stunden sind die Aufzeichnungen nicht mehr verfügbar.

### <a name="download-recording"></a>Herunterladen von Aufzeichnungen
#### <a name="request"></a>Anforderung
**HTTP**
<!-- {
  "blockType": "request",
  "name": "download-recording"
}-->
```http
GET /recording/download/{documentId}
Content-Type: application/json

{
}
```
#### <a name="response"></a>Antwort
**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```
HTTP/1.1 200 Success
Content-Type: video/mp4

{
string // Recording file bytes
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
### <a name="download-recording-metadata"></a>Herunterladen von Aufzeichnungsmetadaten
#### <a name="request"></a>Anforderung
**HTTP**
<!-- {
  "blockType": "request",
  "name": "download-recording-metadata"
}-->
```http
GET /recording/download/{documentId}/metadata
Content-Type: application/json

{
}
```
#### <a name="response"></a>Antwort
**HTTP**
<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 200 Success
Content-Type: application/json

{
  "resourceId": "string",
  "callId": "string",
  "chunkDocumentId": "string",
  "chunkIndex": 0,
  "chunkStartTime": "string",
  "chunkDuration": 0,
  "pauseResumeIntervals": [
    {
      "startTime": "string",
      "duration": 0
    }
  ],
  "recordingInfo": {
    "contentType": "string",
    "channelType": "string",
    "format": "string",
    "audioConfiguration": {
      "sampleRate": "string",
      "bitRate": 0,
      "channels": 0
    },
    "videoConfiguration": {
      "longerSideLength": 0,
      "shorterSideLength": 0,
      "framerate": 0,
      "bitRate": 0
    }
  },
  "participants": [
    {
      "participantId": "string"
    }
  ]
}
```
```
HTTP/1.1 400 Bad request
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
```
HTTP/1.1 500    Internal server error
Content-Type: application/json

{
  "code": "string",
  "message": "string",
  "target": "string",
  "details": [
    null
  ]
}
```
