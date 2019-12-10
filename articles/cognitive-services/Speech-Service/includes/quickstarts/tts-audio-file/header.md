---
title: 'Schnellstart: Synthetisieren von Sprache in eine Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 7d9a03f5a57473ce651560b261a4cf84b4ca4824
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74817865"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache in einer Audiodatei umgewandelt. Nach der Erfüllung einiger Voraussetzungen sind für die Sprachsynthese in eine Audiodatei lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Audiokonfigurationsobjekts zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ ````SpeechSynthesizer```` unter Verwendung der obigen Konfigurationsobjekte
> * Konvertieren Ihres Texts unter Verwendung des Objekts ````SpeechSynthesizer```` in synthetisierte Sprache und Speichern der synthetisierten Sprache in der angegebenen Audiodatei
> * Überprüfen des zurückgegebenen Objekts ````SpeechSynthesizer```` auf Fehler
