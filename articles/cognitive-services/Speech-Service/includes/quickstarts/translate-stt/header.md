---
title: 'Schnellstart: Übersetzen von Sprache in Text – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: cd45657beacd04eb2376af3e4297eae051157778
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816937"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv von einer Sprache in Text einer anderen Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die Übersetzung von Sprache in Text lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen Sie ein Objekt vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region.
> * Aktualisieren Sie das ````SpeechConfig````-Objekt, um die Quell- und Zielsprachen anzugeben.
> * Erstellen Sie ein Objekt vom Typ ````TranslationRecognizer```` unter Verwendung des obigen ````SpeechConfig````-Objekts.
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````TranslationRecognitionResult````
