---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/18/2020
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: cba789b6a40dd23309bb94289b187209893908e3
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92116692"
---
Der Container bietet websocketbasierte Abfrageendpunkt-APIs, auf die über das [Speech SDK](../index.yml) zugegriffen wird. Standardmäßig verwendet das Speech SDK Online-Speech-Dienste. Um den Container verwenden zu können, müssen Sie die Initialisierungsmethode ändern.

> [!TIP]
> Wenn Sie das Sprach-SDK mit Containern verwenden, müssen Sie nicht die Azure Speech-Ressource [Abonnementschlüssel oder Authentifizierungsbearertoken](../rest-speech-to-text.md#authentication) bereitstellen.

Beispiele hierzu finden Sie weiter unten.

# <a name="c"></a>[C#](#tab/csharp)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

in den folgenden Aufruf mit dem [Containerhost](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet):

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```

# <a name="python"></a>[Python](#tab/python)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

in den folgenden Aufruf mit dem [Containerendpunkt](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python):

```python
speech_config = speechsdk.SpeechConfig(
    endpoint="ws://localhost:5000/speech/recognition/conversation/cognitiveservices/v1"
```

---
