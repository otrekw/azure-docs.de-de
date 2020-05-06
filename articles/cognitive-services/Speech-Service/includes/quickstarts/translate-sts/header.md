---
title: 'Schnellstart: Übersetzen von Sprache in Sprache – Speech-Dienst'
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
ms.openlocfilehash: c5f0a0fe032d18cd4f01aebe9a5c736d6d511a74
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "74981488"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv in eine andere Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die Übersetzung von Sprache in Sprache lediglich sechs Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechTranslationConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Aktualisieren des Objekts ````SpeechTranslationConfig````, um die Quell- und Zielsprache anzugeben
> * Aktualisieren des Objekts ````SpeechTranslationConfig````, um den Namen der Stimme für die Sprachausgabe anzugeben
> * Erstellen eines Objekts vom Typ ````TranslationRecognizer```` unter Verwendung des obigen Objekts ````SpeechTranslationConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````TranslationRecognitionResult````
