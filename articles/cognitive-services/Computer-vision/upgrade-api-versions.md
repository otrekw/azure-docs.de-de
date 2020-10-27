---
title: Upgraden auf Read 3.0 der Maschinelles Sehen-API
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie ein Upgrade von v2.0/v2.1 auf v3.0 der Lese-API für maschinelles Sehen ausführen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 08/11/2020
ms.author: pafarley
ROBOTS: NOINDEX
ms.openlocfilehash: cfc9745fc4684a7b0d8f7da7e63149a6fe50f6d2
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331837"
---
# <a name="upgrade-from-read-v2x-to-read-v3x"></a>Upgraden von Read 2.x auf Read 3.x

In diesem Leitfaden erfahren Sie, wie Sie Ihren vorhandenen Container- oder Cloud-API-Code von Read 2.x auf Read 3.0 und 3.1 (Vorschauversion) upgraden.

## <a name="determine-your-api-path"></a>Bestimmen Ihres API-Pfads
Verwenden Sie die folgende Tabelle, um die **Versionszeichenfolge** im API-Pfad auf der Grundlage der Read-Version 3.x zu ermitteln, zu der Sie migrieren:

|Produkttyp| Version | Versionszeichenfolge im API-Pfad der Version 3.x |
|:-----|:----|:----|
|Dienst | Read 3.0 oder 3.1 | **v3.0** bzw. **v3.1** |
|Dienst | Read 3.2 (Vorschau) | **v3.2-preview.1** |
|Container | Read 3.0 (Vorschau) oder Read 3.1 (Vorschau) | **v3.0** bzw. **v3.1-preview.2** |


Verwenden Sie als Nächstes die folgenden Abschnitte, um Ihre Vorgänge einzugrenzen, und ersetzen Sie die **Versionszeichenfolge** in Ihrem API-Pfad durch den Wert aus der Tabelle. Aktualisieren Sie also beispielsweise den API-Pfad für Cloud- und Containerversionen vom Typ **Read v3.2 (Vorschau)** wie folgt: **https://{endpoint}/vision/v3.2-preview.1/read/analyze[?language]** .

## <a name="servicecontainer"></a>Dienst/Container

### `Batch Read File`

|Read 2.x |Read 3.x  |
|----------|-----------|
|https://{endpoint}/vision/ **v2.0/read/core/asyncBatchAnalyze**     |https://{endpoint}/vision/< **Versionszeichenfolge** >/read/analyze[?language]|
    
Ein neuer optionaler _Sprachparameter_ ist verfügbar. Wenn Sie die Sprache Ihres Dokuments nicht kennen oder es möglicherweise mehrsprachig ist, schließen Sie diesen Parameter nicht ein. 

### `Get Read Results`

|Read 2.x |Read 3.x  |
|----------|-----------|
|https://{endpoint}/vision/ **v2.0/read/operations** /{operationId}     |https://{endpoint}/vision/< **Versionszeichenfolge** >/read/analyzeResults/{operationId}|

### <a name="get-read-operation-result-status-flag"></a>Statusflag `Get Read Operation Result`

Wenn der Aufruf von `Get Read Operation Result` erfolgreich ist, wird im JSON-Text ein Feld für die Statuszeichenfolge zurückgegeben.
 
|Read 2.x |Read 3.x  |
|----------|-----------|
|`"NotStarted"` |    `"notStarted"`|
|`"Running"` | `"running"`|
|`"Failed"` | `"failed"`|
|`"Succeeded"` | `"succeeded"`|
    
### <a name="api-response-json"></a>API-Antwort (JSON) 

Beachten Sie die folgenden Änderungen in der JSON-Datei:
* In v2.x gibt `Get Read Operation Result` die JSON-Datei der OCR-Erkennung zurück, wenn der Status `Succeeded"` ist. In v3.0 ist dieses Feld `succeeded`.
* Zum Abrufen des Stamms für das Seitenarray ändern Sie die JSON-Hierarchie von `recognitionResults` in `analyzeResult`/`readResults`. Die JSON-Hierarchie pro Zeile und Seite bleibt unverändert, sodass keine Codeänderungen erforderlich sind.
* Der Seitenwinkel `clockwiseOrientation` wurde in `angle` umbenannt, und der Bereich wurde von 0 bis 360 Grad in -180 bis 180 Grad geändert. Je nach Ihrem Code müssen Sie möglicherweise Änderungen vornehmen, da die meisten mathematischen Funktionen beide Bereiche verarbeiten können.

Mit der API 3.0 werden außerdem die folgenden Verbesserungen eingeführt, die Sie optional nutzen können:
* `createdDateTime` und `lastUpdatedDateTime` werden hinzugefügt, um die Nachverfolgung der Verarbeitungsdauer zu ermöglichen. Weitere Informationen finden Sie in der Dokumentation. 
* `version` gibt Aufschluss über die Version der API, die zum Generieren von Ergebnissen verwendet wurde.
* Eine wortbasierte `confidence` wurde hinzugefügt. Dieser Wert wird so kalibriert, dass der Wert 0,95 bedeutet, dass die Erkennung mit einer Wahrscheinlichkeit von 95 % richtig ist. Anhand des Konfidenz-Score kann ausgewählt werden, welcher Text zur Überprüfung durch Personen gesendet werden soll. 
    
    
In 2.X lautet das Ausgabeformat wie folgt: 
    
```json
    {
        {
                "status": "Succeeded",
                "recognitionResults": [
                    {
                    "page": 1,
                    "language": "en",
                    "clockwiseOrientation": 349.59,
                    "width": 2661,
                    "height": 1901,
                    "unit": "pixel",
                    "lines": [
                        {
                        "boundingBox": [
                            67,
                            646,
                            2582,
                            713,
                            2580,
                            876,
                            67,
                            821
                        ],
                        "text": "The quick brown fox jumps",
                        "words": [
                            {
                            "boundingBox": [
                                143,
                                650,
                                435,
                                661,
                                436,
                                823,
                                144,
                                824
                            ],
                            "text": "The",
                            },
            // The rest of result is omitted for brevity 
            
}
```
    
In v3.0 wurde es angepasst:
    
```json
    {
        {
            "status": "succeeded",
            "createdDateTime": "2020-05-28T05:13:21Z",
            "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
            "analyzeResult": {
            "version": "3.0.0",
            "readResults": [
                {
                "page": 1,
                "language": "en",
                "angle": 0.8551,
                "width": 2661,
                "height": 1901,
                "unit": "pixel",
                "lines": [
                    {
                    "boundingBox": [
                        67,
                        646,
                        2582,
                        713,
                        2580,
                        876,
                        67,
                        821
                    ],
                    "text": "The quick brown fox jumps",
                    "words": [
                        {
                        "boundingBox": [
                            143,
                            650,
                            435,
                            661,
                            436,
                            823,
                            144,
                            824
                        ],
                        "text": "The",
                        "confidence": 0.958
                        },
        // The rest of result is omitted for brevity 
        
    }
```

## <a name="service-only"></a>Nur Dienst

### `Recognize Text`
`Recognize Text` ist ein *Vorschauvorgang* , der in allen Versionen der *Maschinelles Sehen-API* als veraltet markiert wird. Sie müssen von `Recognize Text` zu `Read` (v3.0) oder `Batch Read File` (v2.0, v2.1) migrieren. v3.0 von `Read` enthält neuere, bessere Modelle für die Texterkennung und zusätzliche Features, daher wird empfohlen. Durchführen des Upgrades von `Recognize Text` auf `Read`:

|Texterkennung 2.x |Read 3.x  |
|----------|-----------|
|https://{endpoint}/vision/ **v2.0/recognizeText[?mode]**|https://{endpoint}/vision/< **Versionszeichenfolge** >/read/analyze[?language]|
    
Der _mode_ -Parameter wird in `Read` nicht unterstützt. Sowohl handgeschriebener als auch gedruckter Text werden automatisch unterstützt.
    
In v3.0 ist neuer optionaler _Sprachparameter_ verfügbar. Wenn Sie die Sprache Ihres Dokuments nicht kennen oder es möglicherweise mehrsprachig ist, schließen Sie diesen Parameter nicht ein. 

### `Get Recognize Text Operation Result`

|Texterkennung 2.x |Read 3.x  |
|----------|-----------|
|https://{endpoint}/vision/ **v2.0/textOperations/** {operationId}|https://{endpoint}/vision/< **Versionszeichenfolge** >/read/analyzeResults/{operationId}|

### <a name="get-recognize-text-operation-result-status-flags"></a>Statusflags vom Typ `Get Recognize Text Operation Result`
Wenn der Aufruf von `Get Recognize Text Operation Result` erfolgreich ist, wird im JSON-Text ein Feld für die Statuszeichenfolge zurückgegeben. 
 
|Texterkennung 2.x |Read 3.x  |
|----------|-----------|
|`"NotStarted"` |    `"notStarted"`|
|`"Running"` | `"running"`|
|`"Failed"` | `"failed"`|
|`"Succeeded"` | `"succeeded"`|

### <a name="api-response-json"></a>API-Antwort (JSON)

Beachten Sie die folgenden Änderungen in der JSON-Datei:    
* In v2.x gibt `Get Read Operation Result` die JSON-Datei der OCR-Erkennung zurück, wenn der Status `Succeeded` ist. In v3.x ist dieses Feld `succeeded`.
* Zum Abrufen des Stamms für das Seitenarray ändern Sie die JSON-Hierarchie von `recognitionResult` in `analyzeResult`/`readResults`. Die JSON-Hierarchie pro Zeile und Seite bleibt unverändert, sodass keine Codeänderungen erforderlich sind.

Mit der v3.0-API werden außerdem die folgenden Verbesserungen eingeführt, die Sie optional nutzen können. Ausführlichere Informationen finden Sie in der API-Referenz:
* `createdDateTime` und `lastUpdatedDateTime` werden hinzugefügt, um die Nachverfolgung der Verarbeitungsdauer zu ermöglichen. Weitere Informationen finden Sie in der Dokumentation. 
* `version` gibt Aufschluss über die Version der API, die zum Generieren von Ergebnissen verwendet wurde.
* Eine wortbasierte `confidence` wurde hinzugefügt. Dieser Wert wird so kalibriert, dass der Wert 0,95 bedeutet, dass die Erkennung mit einer Wahrscheinlichkeit von 95 % richtig ist. Anhand des Konfidenz-Score kann ausgewählt werden, welcher Text zur Überprüfung durch Personen gesendet werden soll. 
* `angle`: Die allgemeine Ausrichtung des Texts im Uhrzeigersinn, gemessen in Grad zwischen (-180, 180).
* `width` und `"height"` geben Ihnen die Dimensionen Ihres Dokuments, und `"unit"` stellt die Einheit dieser Dimensionen (Pixel oder Zoll, je nach Dokumenttyp) bereit.
* `page`: Mehrseitige Dokumente werden unterstützt.
* `language`: Die Eingabesprache des Dokuments (aus dem optionalen _Sprachparameter_ ).


In 2.X lautet das Ausgabeformat wie folgt: 
    
```json
    {
        {
                "status": "Succeeded",
                "recognitionResult": [
                    {
                    "lines": [
                        {
                        "boundingBox": [
                            67,
                            646,
                            2582,
                            713,
                            2580,
                            876,
                            67,
                            821
                        ],
                        "text": "The quick brown fox jumps",
                        "words": [
                            {
                            "boundingBox": [
                                143,
                                650,
                                435,
                                661,
                                436,
                                823,
                                144,
                                824
                            ],
                            "text": "The",
                            },
            // The rest of result is omitted for brevity 
            
    }
```
    
In v3.x wurde es angepasst:
    
```json
    {
        {
            "status": "succeeded",
            "createdDateTime": "2020-05-28T05:13:21Z",
            "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
            "analyzeResult": {
            "version": "3.0.0",
            "readResults": [
                {
                "page": 1,
                "angle": 0.8551,
                "width": 2661,
                "height": 1901,
                "unit": "pixel",
                "lines": [
                    {
                    "boundingBox": [
                        67,
                        646,
                        2582,
                        713,
                        2580,
                        876,
                        67,
                        821
                    ],
                    "text": "The quick brown fox jumps",
                    "words": [
                        {
                        "boundingBox": [
                            143,
                            650,
                            435,
                            661,
                            436,
                            823,
                            144,
                            824
                        ],
                        "text": "The",
                        "confidence": 0.958
                        },
        // The rest of result is omitted for brevity 
        
    }
```

## <a name="container-only"></a>Nur Container

### `Synchronous Read`

|Read 2.0 |Read 3.x  |
|----------|-----------|
|https://{endpoint}/vision/ **v2.0/read/core/Analyze**     |https://{endpoint}/vision/< **Versionszeichenfolge** >/read/syncAnalyze[?language]|
