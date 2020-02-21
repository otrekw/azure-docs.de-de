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
ms.openlocfilehash: 00617fc09f471eaf3dc13a5aa691c4aab2e1e2ec
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77424914"
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
- Authentication Tokens: https[]()://chinaeast2.api.cognitive.azure.cn/sts/v1.0/issueToken
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

