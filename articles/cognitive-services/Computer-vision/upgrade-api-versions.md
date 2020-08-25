---
title: Upgrade auf v3.0 der Maschinelles Sehen-API
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
ms.openlocfilehash: 6e695fcfacac19ca82273d84d049bdb2afe14b54
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88214193"
---
# <a name="upgrade-to-computer-vision-v30-read-api-from-v20v21"></a>Upgrade von v2.0/v2.1 auf v3.0 der Lese-API für maschinelles Sehen

In dieser Anleitung wird gezeigt, wie Sie ein Upgrade des vorhandenen v2.0- oder v2.1-REST-API-Codes für maschinelles Sehen auf v3.0-Lesevorgänge ausführen. 

## <a name="upgrade-batch-read-file-to-read"></a>Upgrade von `Batch Read File` auf `Read`


1. Ändern Sie den API-Pfad für `Batch Read File` 2.x wie folgt:


    |Read 2.x |Read 3.0  |
    |----------|-----------|
    |https://{endpoint}/vision/**v2.0/read/core/asyncBatchAnalyze**     |https://{endpoint}/vision/**v3.0/read/analyze**[?language]|
    
    Ein neuer optionaler _Sprachparameter_ ist verfügbar. Wenn Sie die Sprache Ihres Dokuments nicht kennen oder es möglicherweise mehrsprachig ist, schließen Sie diesen Parameter nicht ein. 

2. Ändern Sie den API-Pfad für `Get Read Results` in 2.x wie folgt:

    |Read 2.x |Read 3.0  |
    |----------|-----------|
    |https://{endpoint}/vision/**v2.0**/read/**operations**/{operationId}     |https://{endpoint}/vision/**v3.0**/read/**analyzeResults**/{operationId}|

3. Ändern Sie den Code zum Überprüfen der JSON-Ergebnisse in `Get Read Operation Result`. Wenn der Aufruf von `Get Read Operation Result` erfolgreich ist, wird im JSON-Text ein Feld für die Statuszeichenfolge zurückgegeben. Die folgenden Werte von v2.0 wurden geändert, damit Sie besser an den anderen Cognitive Service-REST-APIs ausgerichtet sind. 
 
    |Read 2.x |Read 3.0  |
    |----------|-----------|
    |`"NotStarted"` |   `"notStarted"`|
    |`"Running"` | `"running"`|
    |`"Failed"` | `"failed"`|
    |`"Succeeded"` | `"succeeded"`|
    
4. Ändern Sie den Code, um die JSON-Datei mit dem endgültigen Erkennungsergebnis aus `Get Read Operation Result` zu interpretieren. 

    Beachten Sie die folgenden Änderungen in der JSON-Datei:
    
    - In v2.x gibt `"Get Read Operation Result"` die JSON-Datei der OCR-Erkennung zurück, wenn der Status `"Succeeded"` ist. In v3.0 ist dieses Feld `"succeeded"`.
    - Zum Abrufen des Stamms für das Seitenarray ändern Sie die JSON-Hierarchie von `"recognitionResults"` in `"analyzeResult"`/`"readResults"`. Die JSON-Hierarchie pro Zeile und Seite bleibt unverändert, sodass keine Codeänderungen erforderlich sind.
    -   Der Seitenwinkel `"clockwiseOrientation"` wurde in `"angle"` umbenannt, und der Bereich wurde von 0 bis 360 Grad in -180 bis 180 Grad geändert. Je nach Ihrem Code müssen Sie möglicherweise Änderungen vornehmen, da die meisten mathematischen Funktionen beide Bereiche verarbeiten können.
    -   Mit der v3.0-API werden auch die folgenden Verbesserungen eingeführt, die Sie optional nutzen können: – `"createdDateTime"` und `"lastUpdatedDateTime"` werden hinzugefügt, damit Sie die Verarbeitungsdauer nachverfolgen können. Weitere Informationen finden Sie in der Dokumentation. 
        - `"version"` gibt Aufschluss über die Version der API, die zum Generieren von Ergebnissen verwendet wurde.
        - Eine wortbasierte `"confidence"` wurde hinzugefügt. Dieser Wert wird so kalibriert, dass der Wert 0,95 bedeutet, dass die Erkennung mit einer Wahrscheinlichkeit von 95 % richtig ist. Anhand des Konfidenz-Score kann ausgewählt werden, welcher Text zur Überprüfung durch Personen gesendet werden soll. 
    
    
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

## <a name="upgrade-from-recognize-text-to-read"></a>Upgrade von `Recognize Text` auf `Read`
`Recognize Text` ist ein *Vorschauvorgang*, der in allen Versionen der *Maschinelles Sehen-API* als veraltet markiert wird. Sie müssen von `Recognize Text` zu `Read` (v3.0) oder `Batch Read File` (v2.0, v2.1) migrieren. v3.0 von `Read` enthält neuere, bessere Modelle für die Texterkennung und zusätzliche Features, daher wird empfohlen. Durchführen des Upgrades von `Recognize Text` auf `Read`:

1. Ändern Sie den API-Pfad für `Recognize Text` v2.x wie folgt:


    |Texterkennung 2.x |Read 3.0  |
    |----------|-----------|
    |https://{endpoint}/vision/**v2.0/recognizeText[?mode]**|https://{endpoint}/vision/**v3.0/read/analyze**[?language]|
    
    Der _mode_-Parameter wird in `Read` nicht unterstützt. Sowohl handgeschriebener als auch gedruckter Text werden automatisch unterstützt.
    
    In v3.0 ist neuer optionaler _Sprachparameter_ verfügbar. Wenn Sie die Sprache Ihres Dokuments nicht kennen oder es möglicherweise mehrsprachig ist, schließen Sie diesen Parameter nicht ein. 

2. Ändern Sie den API-Pfad für `Get Recognize Text Operation Result` v2.x wie folgt:

    |Texterkennung 2.x |Read 3.0  |
    |----------|-----------|
    |https://{endpoint}/vision/**v2.0/textOperations/** {operationId}|https://{endpoint}/vision/**v3.0/read/analyzeResults**/{operationId}|

3. Ändern Sie den Code zum Überprüfen der JSON-Ergebnisse in `Get Recognize Text Operation Result`. Wenn der Aufruf von `Get Read Operation Result` erfolgreich ist, wird im JSON-Text ein Feld für die Statuszeichenfolge zurückgegeben. 
 
    |Texterkennung 2.x |Read 3.0  |
    |----------|-----------|
    |`"NotStarted"` |   `"notStarted"`|
    |`"Running"` | `"running"`|
    |`"Failed"` | `"failed"`|
    |`"Succeeded"` | `"succeeded"`|


4. Ändern Sie den Code, um die JSON-Datei mit dem endgültigen Erkennungsergebnis aus `Get Recognize Text Operation Result` zu interpretieren, um `Get Read Operation Result` zu unterstützen

    Beachten Sie die folgenden Änderungen in der JSON-Datei:    

    - In v2.x gibt `"Get Read Operation Result"` die JSON-Datei der OCR-Erkennung zurück, wenn der Status `"Succeeded"` ist. In v3.0 ist dieses Feld `"succeeded"`.
    - Zum Abrufen des Stamms für das Seitenarray ändern Sie die JSON-Hierarchie von `"recognitionResult"` in `"analyzeResult"`/`"readResults"`. Die JSON-Hierarchie pro Zeile und Seite bleibt unverändert, sodass keine Codeänderungen erforderlich sind.
    -   Mit der v3.0-API werden außerdem die folgenden Verbesserungen eingeführt, die Sie optional nutzen können. Weitere Informationen finden Sie in der API-Referenz: – `"createdDateTime"` und `"lastUpdatedDateTime"` werden hinzugefügt, damit Sie die Verarbeitungsdauer nachverfolgen können. Weitere Informationen finden Sie in der Dokumentation. 
        - `"version"` gibt Aufschluss über die Version der API, die zum Generieren von Ergebnissen verwendet wurde.
        - Eine wortbasierte `"confidence"` wurde hinzugefügt. Dieser Wert wird so kalibriert, dass der Wert 0,95 bedeutet, dass die Erkennung mit einer Wahrscheinlichkeit von 95 % richtig ist. Anhand des Konfidenz-Score kann ausgewählt werden, welcher Text zur Überprüfung durch Personen gesendet werden soll. 
        - `"angle"`: Die allgemeine Ausrichtung des Texts im Uhrzeigersinn, gemessen in Grad zwischen (-180, 180).
        -  `"width"` und `"height"` geben Ihnen die Dimensionen Ihres Dokuments, und `"unit"` stellt die Einheit dieser Dimensionen (Pixel oder Zoll, je nach Dokumenttyp) bereit.
        - `"page"`: Mehrseitige Dokumente werden unterstützt.
        - `"language"`: Die Eingabesprache des Dokuments (aus dem optionalen _Sprachparameter_).


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
    
## <a name="all-other-operations"></a>Alle anderen Vorgänge

Es gibt keine weiteren Breaking Changes zwischen v2.X und v3.0 der Maschinelles Sehen-API. Sie können einfach den API-Pfad ändern, um `v2.0` durch `v3.0` zu ersetzen.
