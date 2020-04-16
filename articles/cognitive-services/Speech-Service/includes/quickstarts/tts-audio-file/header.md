---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 870dce55e79bf0169f19d31dfec6689c65fce9cd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400359"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache in einer Audiodatei umgewandelt. Der Sprachsynthesedienst bietet zahlreiche Optionen für synthetisierte Stimmen. Weitere Informationen finden Sie unter [Text-zu-Sprache](../../../language-support.md#text-to-speech). Nach der Erfüllung einiger Voraussetzungen sind für die Sprachsynthese in eine Audiodatei lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Audiokonfigurationsobjekts zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ `SpeechSynthesizer` unter Verwendung der obigen Konfigurationsobjekte
> * Konvertieren Ihres Texts unter Verwendung des Objekts `SpeechSynthesizer` in synthetisierte Sprache und Speichern der synthetisierten Sprache in der angegebenen Audiodatei
> * Überprüfen des zurückgegebenen Objekts `SpeechSynthesizer` auf Fehler
