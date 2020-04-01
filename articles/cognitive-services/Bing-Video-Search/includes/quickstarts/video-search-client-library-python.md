---
title: 'Schnellstart: Python-Clientbibliothek für die Bing-Videosuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: 7a9fab8ba8bb9d21c9284cbf14bc67226d2ef9d3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80289859"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Videosuche-Clientbibliothek für Python mit der Suche nach Nachrichten zu beginnen. Die Bing-Videosuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Der Quellcode für dieses Beispiel steht mit zusätzlichen Anmerkungen und Features auf [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) bereit.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Python](https://www.python.org/) 2.x oder 3.x
- Die Bing-Videosuche-Clientbibliothek für Python

Es wird empfohlen, eine [virtuelle Python-Umgebung](https://docs.python.org/3/tutorial/venv.html) zu verwenden. Sie können eine virtuelle Umgebung mit dem [venv-Modul](https://pypi.python.org/pypi/virtualenv) installieren und initialisieren. Installieren Sie virtualenv für Python 2.7 mit:

```console
python -m venv mytestenv
```

Installieren Sie die Clientbibliothek für die Bing-Videosuche mit:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor eine neue Python-Datei, und fügen Sie die folgenden Importanweisungen hinzu. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchClient
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Erstellen Sie eine Variable für Ihren Abonnementschlüssel. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    ```

## <a name="create-the-search-client"></a>Erstellen des Suchclients

Erstellen Sie eine Instanz von `CognitiveServicesCredentials`, und instanziieren Sie den Client:

```python
client = VideoSearchAPI(endpoint, CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Senden einer Suchanforderung und Erhalten einer Antwort

1. Verwenden Sie `client.videos.search()` mit Ihrer Suchabfrage, um eine Anforderung an die Bing-Videosuche-API zu senden und eine Antwort zu erhalten.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Wenn die Antwort Suchergebnisse enthält, können Sie das erste Ergebnis abrufen und dafür die ID, den Namen und die URL ausgeben.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Webanwendung](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen 

- [Worum handelt es sich bei der Bing-Videosuche-API?](../../overview.md)
- [Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
