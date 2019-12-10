---
title: 'Schnellstart: Erkennen von im Blobspeicher gespeicherter Sprache – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: ef61139056ad194d89e2a67cb4ac1b1414a3380b
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74828737"
---
In dieser Schnellstartanleitung verwenden Sie die [REST-API zur Batchtranskription](../../../batch-transcription.md), um die in einem [SAS-Blob](https://aka.ms/ignite2019/speech/placeholder) gespeicherte Sprache zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über eine REST-API lediglich wenige Schritte erforderlich:
> [!div class="checklist"]
> * Senden Sie die JSON-Anforderung an den Speech-Dienst, um die Sprache zu transkribieren.
> * Überprüfen des Status der Transkription.
> * Laden Sie die Transkriptionsergebnisse herunter, wenn Sie fertig sind.