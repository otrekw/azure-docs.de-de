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
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: d4781808ce8e80f62e86ce1d0c6db9c38b2636d0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "74981077"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv von einer Sprache in Text einer anderen Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die Übersetzung von Sprache in Text lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Aktualisieren des Objekts ````SpeechConfig````, um die Quell- und Zielsprache anzugeben
> * Erstellen eines Objekts vom Typ ````TranslationRecognizer```` unter Verwendung des obigen Objekts ````SpeechConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````TranslationRecognitionResult````
