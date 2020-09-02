---
title: 'Schnellstart: C#-Clientbibliothek für die Bing-Entitätssuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 27951a6da2c5e4a9d17e8b332d8ee9706701649d
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88931885"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Entitätssuche-Clientbibliothek für C# mit der Suche nach Entitäten zu beginnen. Die Bing-Entitätssuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch).


## <a name="prerequisites"></a>Voraussetzungen

* Eine beliebige Edition von [Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/).
* Das [Json.NET](https://www.newtonsoft.com/json)-Framework, das als NuGet-Paket verfügbar ist
* Unter Linux/macOS kann diese Anwendung mit [Mono](https://www.mono-project.com/) ausgeführt werden
* Das [NuGet-Paket mit dem SDK für die Bing-News-Suche](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). Bei der Installation dieses Pakets wird auch Folgendes installiert:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Verwenden Sie die Option **NuGet-Pakete verwalten** im **Projektmappen-Explorer**, und fügen Sie das Paket `Microsoft.Azure.CognitiveServices.Search.EntitySearch` hinzu, um Ihrem Visual Studio-Projekt die Clientbibliothek für die Bing-Entitätssuche hinzuzufügen.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Erstellen und Initialisieren einer Anwendung

1. Erstellen Sie eine neue C#-Konsolenprojektmappe in Visual Studio. Fügen Sie dann Folgendes in die Hauptcodedatei ein:

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>Erstellen eines Clients und Senden einer Suchanforderung

1. Erstellen Sie einen neuen Suchclient. Erstellen Sie ein neues Element vom Typ `ApiKeyServiceClientCredentials`, um Ihren Abonnementschlüssel hinzuzufügen.

    ```csharp
    var client = new EntitySearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

1. Verwenden Sie die Funktion `Entities.Search()` des Clients, um nach Ihrer Abfrage zu suchen:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Abrufen und Ausgeben einer Entitätsbeschreibung

1. Falls die API Suchergebnisse zurückgegeben hat, rufen Sie die Hauptentität aus `entityData` ab.

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. Ausgeben der Beschreibung der Hauptentität 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](../../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../../overview.md )
