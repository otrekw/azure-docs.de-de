---
title: 'Sovereign Clouds: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Sovereign Clouds verwenden.
services: cognitive-services
author: cbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 1/14/2020
ms.author: cbasoglu
ms.openlocfilehash: 3b96ded52aa2148df9e96d6f1e878d50b821abab
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170456"
---
# <a name="speech-services-with-sovereign-clouds"></a>Speech-Dienste mit Sovereign Clouds

## <a name="azure-government-united-states"></a>Azure Government (USA)

Nur US-Behörden des Bundes und von Bundesstaaten, Kommunen und Stämmen sowie ihre Partner haben Zugriff auf diese dedizierte Instanz mit Vorgängen, die von überwachten US-Bürgern gesteuert werden.
- Regionen: US Government, Virginia
- SR im Speech SDK: *config.FromHost("wss://virginia.stt.speech.azure.us", "\<IhrSchlüssel\>");*
- TTS im Speech SDK: *config.FromHost("https[]()://virginia.tts.speech.azure.us", "\<IhrSchlüssel\>");*
- Authentifizierungstoken: https[]()://virginia.api.cognitive.microsoft.us/sts/v1.0/issueToken
- Azure-Portal: https://portal.azure.us  
- Custom Speech-Portal: https://virginia.cris.azure.us/Home/CustomSpeech
- Verfügbare SKUs: S0
- Unterstützte Features:
  - Spracherkennung
  - Custom Speech (akustische/Sprachanpassung)
  - Sprachsynthese
  - Sprachübersetzung
- Nicht unterstützte Funktionen
  - Custom Voice
  - Neuronale Stimmen für Text-zu-Sprache
- Unterstützte Gebietsschemas: Gebietsschemas für die folgenden Sprachen werden unterstützt.
  - Arabisch (ar-*)
  - Chinesisch (zh-*)
  - Englisch (en-*)
  - Französisch (fr-*)
  - Deutsch (de-*)
  - Hindi
  - Koreanisch
  - Russisch
  - Spanisch (es-*)

## <a name="microsoft-azure-china"></a>Microsoft Azure China

Ein in China befindliches Azure-Rechenzentrum mit direktem Zugang zu China Mobile, China Telecom, China Unicom und anderen wichtigen Backbone-Netzwerken von Netzbetreibern für chinesische Benutzer, das Hochgeschwindigkeitszugriff und stabile Verbindungen mit dem lokalen Netzwerk bietet.
- Regionen: China, Osten 2 (Shanghai)
- SR im Speech SDK: *config.FromHost("wss://chinaeast2.stt.speech.azure.cn", "\<IhrSchlüssel\>");*
- TTS im Speech SDK: *config.FromHost("https[]()://chinaeast2.tts.speech.azure.cn", "\<IhrSchlüssel\>");*
- Authentifizierungstoken: https[]()://chinaeast2.api.cognitive.microsoft.cn/sts/v1.0/issueToken
- Azure-Portal: https://portal.azure.cn
- Custom Speech-Portal: https://chinaeast2.cris.azure.cn/Home/CustomSpeech
- Verfügbare SKUs: S0
- Unterstützte Features:
  - Spracherkennung
  - Custom Speech (akustische/Sprachanpassung)
  - Sprachsynthese
  - Sprachübersetzung
- Nicht unterstützte Funktionen
  - Custom Voice
  - Neuronale Stimmen für Text-zu-Sprache
- Unterstützte Gebietsschemas: Gebietsschemas für die folgenden Sprachen werden unterstützt.
  - Arabisch (ar-*)
  - Chinesisch (zh-*)
  - Englisch (en-*)
  - Französisch (fr-*)
  - Deutsch (de-*)
  - Hindi
  - Koreanisch
  - Russisch
  - Spanisch (es-*)

