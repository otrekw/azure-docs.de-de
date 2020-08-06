---
title: 'TSV-Format für Batchtests: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Grundlagen zum TSV-Format für Batchtests
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 10/24/2019
ms.openlocfilehash: 0ed948b4bf69fc672a59a7825279a12868d10521
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132125"
---
# <a name="batch-testing-tsv-format"></a>TSV-Format für Batchtests

Batchtests sind als [Quellcode](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting) oder als [gezippte ausführbare Datei zum Herunterladen](https://aka.ms/qna_btzip) verfügbar. Das Format des Befehls zum Ausführen des Batchtests sieht wie folgt aus:

```console
batchtesting.exe input.tsv https://YOUR-HOST.azurewebsites.net ENDPOINT-KEY out.tsv
```

|Parameter|Erwarteter Wert|
|--|--|
|1|Name der TSV-Datei, mit [TSV-Eingabefeldern](#tsv-input-fields) formatiert|
|2|URI für den Endpunkt, mit YOUR-HOST (IHR HOST) von der Seite „Veröffentlichen“ des QnA Maker-Portals.|
|3|ENDPOINT-KEY (ENDPUNKTSCHLÜSSEL), zu finden auf der Seite „Veröffentlichen“ des QnA Maker-Portals.|
|4|Der Name der von den Batchtests für Ergebnisse erstellten TSV-Datei.|

Verwenden Sie die folgenden Informationen, um das TSV-Format für Batchtests zu verstehen und zu implementieren. 

## <a name="tsv-input-fields"></a>TSV-Eingabefelder

|Felder der TSV-Eingabedatei|Notizen|
|--|--|
|KBID|Ihre KB-ID, wie sie sich auf der Seite „Veröffentlichen“ findet.|
|Frage|Die Frage, die ein Benutzer eingeben würde.|
|Metadatentags|optional|
|Oberster Parameter|optional| 
|Erwartete Antwort-ID|optional|

![Eingabeformat für die TSV-Datei für Batch Tests.](media/batch-test/input-tsv-format-batch-test.png)

## <a name="tsv-output-fields"></a>TSV-Ausgabefelder 

|Parameter der TSV-Ausgabedatei|Notizen|
|--|--|
|KBID|Ihre KB-ID, wie sie sich auf der Seite „Veröffentlichen“ findet.|
|Frage|Die Frage, wie sie aus der Eingabedatei eingegeben wird.|
|Antwort|Oberste Antwort aus Ihrer Wissensdatenbank.|
|Antwort-ID|Antwort-ID|
|Ergebnis|Vorhersagepunktzahl für die Antwort. |
|Metadatentags|Der zurückgegebenen Antwort zugeordnet|
|Erwartete Antwort-ID|optional (nur wenn die erwartete Antwort-ID angegeben ist)|
|Urteilsbezeichnung|Optional, es können diese Werte angenommen werden: „richtig“ oder „falsch“ (nur, wenn die erwartete Antwort angegeben ist)|
