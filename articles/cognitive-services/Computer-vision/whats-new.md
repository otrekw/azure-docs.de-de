---
title: Neuerungen beim maschinellen Sehen
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Neuigkeiten zum maschinellen Sehen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 09/11/2020
ms.author: pafarley
ms.openlocfilehash: f007ddf2625bf9e8b9d4365902ac1511ea95fa81
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90904104"
---
# <a name="whats-new-in-computer-vision"></a>Neuerungen beim maschinellen Sehen

Informieren Sie sich über die Neuerungen im Dienst. Dabei kann es sich um Versionshinweise, Videos, Blogbeiträge und andere Informationen handeln. Legen Sie ein Lesezeichen für diese Seite an, um über den Dienst auf dem Laufenden zu bleiben.

## <a name="september-2020"></a>September 2020

### <a name="spatial-analysis-container-preview"></a>Container für räumliche Analyse (Vorschauversion)

Der [Container für räumliche Analyse](spatial-analysis-container.md) befindet sich nun in der Vorschauphase. Mithilfe des Features der räumlichen Analyse für maschinelles Sehen können Sie in Echtzeit gestreamte Videodaten analysieren, um zu ermitteln, wie Personen räumlich zueinander in Beziehung stehen und wie sie sich durch physische Umgebungen bewegen. Bei der räumlichen Analyse handelt es sich um einen lokal verwendbaren Docker-Container. 

### <a name="read-api-v31-public-preview-adds-ocr-for-japanese"></a>Lese-API v3.1 (Public Preview) jetzt mit OCR für Japanisch
Der Lese-API v3.1 (Public Preview) für maschinelles Sehen wurden folgende Funktionen hinzugefügt:
* OCR für Japanisch
* Für jede Textzeile wird angegeben, ob es sich um Handschrift oder um gedruckten Text handelt, und es wird eine Zuverlässigkeitsbewertung bereitgestellt (nur lateinische Sprachen).
* Bei mehrseitigen Dokumenten wird Text nur für ausgewählte Seiten oder für einen ausgewählten Seitenbereich extrahiert.

* Diese Vorschauversion der Lese-API unterstützt die Sprachen Englisch, Niederländisch, Französisch, Deutsch, Italienisch, Japanisch, Portugiesisch, Chinesisch (vereinfacht) und Spanisch.

Weitere Informationen finden Sie in der [Übersicht zur Lese-API](concept-recognizing-text.md).

> [!div class="nextstepaction"]
> [Weitere Informationen zur Lese-API v3.1 (Public Preview 2)](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005)

## <a name="july-2020"></a>Juli 2020

### <a name="read-api-v31-public-preview-with-ocr-for-simplified-chinese"></a>Lese-API v3.1 (Public Preview) mit OCR für vereinfachtes Chinesisch
In der Lese-API v3.1 (Public Preview) für maschinelles Sehen wurde Unterstützung für vereinfachtes Chinesisch hinzugefügt.

* Diese Vorschauversion der Lese-API unterstützt die Sprachen Englisch, Niederländisch, Französisch, Deutsch, Italienisch, Portugiesisch, Chinesisch (vereinfacht) und Spanisch.

Weitere Informationen finden Sie in der [Übersicht zur Lese-API](concept-recognizing-text.md).

> [!div class="nextstepaction"]
> [Weitere Informationen zur Lese-API v3.1 (Public Preview 1)](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005)

## <a name="may-2020"></a>Mai 2020
Maschinelles Sehen-API v3.0 ist nun allgemein verfügbar und beinhaltet Aktualisierungen der [Lese-API](concept-recognizing-text.md):

* Unterstützung für Englisch, Niederländisch, Französisch, Deutsch, Italienisch, Portugiesisch und Spanisch
* Höhere Genauigkeit
* Konfidenzbewertung für jedes extrahierte Wort
* Neues Ausgabeformat

## <a name="march-2020"></a>März 2020

* TLS 1.2 wird nun für alle HTTP-Anforderungen erzwungen, die an diesen Dienst gerichtet werden. Weitere Informationen finden Sie unter [Sicherheit von Azure Cognitive Services](../cognitive-services-security.md).

## <a name="january-2020"></a>Januar 2020

### <a name="read-api-30-public-preview"></a>Lese-API 3.0 Public Preview

Sie haben nun die Möglichkeit, die Version 3.0 der Lese-API zu verwenden, um gedruckten oder handschriftlichen Text aus Bildern zu extrahieren. Verglichen mit früheren Versionen bietet die Version 3.0 Folgendes:
* Höhere Genauigkeit
* Neues Ausgabeformat
* Konfidenzbewertung für jedes extrahierte Wort
* Unterstützung der Sprachen Spanisch und Englisch mit dem zusätzlichen Sprachparameter

Informationen zu den ersten Schritten mit der API-Version 3.0 finden Sie in einer [Schnellstartanleitung zur Textextraktion](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/csharp-hand-text?tabs=version-3).

## <a name="cognitive-service-updates"></a>Cognitive Services-Updates

[Azure-Updateankündigungen für Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)
