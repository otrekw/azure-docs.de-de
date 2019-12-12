---
title: 'Schnellstart: Übersetzen von Sprache in mehrere Sprachen – Speech-Dienst'
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
ms.openlocfilehash: 8c8cbc4e4f531d7a06ae3a33c33df9264c2cc6f2
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74980761"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv in eine andere Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die mehrsprachige Übersetzung von Sprache in Text lediglich sechs Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechTranslationConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Aktualisieren des Objekts ````SpeechTranslationConfig````, um die Ausgangssprache für die Spracherkennung anzugeben
> * Aktualisieren des Objekts ````SpeechTranslationConfig````, um mehrere Zielsprachen für die Übersetzung anzugeben
> * Erstellen eines Objekts vom Typ ````TranslationRecognizer```` unter Verwendung des obigen Objekts ````SpeechTranslationConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````TranslationRecognitionResult````
