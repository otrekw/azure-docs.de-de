---
title: '.NET-Tutorial: Highlights der Suchintegration'
titleSuffix: Azure Cognitive Search
description: In diesem Cheatsheet finden Sie grundlegende Informationen zu den .NET SDK-Suchintegrationsabfragen, die auf Website mit Suchunterstützung verwendet werden.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 04/23/2021
ms.custom: devx-track-csharp
ms.devlang: dotnet
ms.openlocfilehash: d548822bb7140c25884a44a45725ff09f16ac0e5
ms.sourcegitcommit: 42ac9d148cc3e9a1c0d771bc5eea632d8c70b92a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2021
ms.locfileid: "109848121"
---
# <a name="4---net-search-integration-cheat-sheet"></a>4\. Cheatsheet für die .NET-Suchintegration

In den vorherigen Lektionen haben Sie einer statischen Web-App eine Suchfunktion hinzugefügt. In dieser Lektion werden die wesentlichen Schritte zum Einrichten der Integration hervorgehoben. Dieser Artikel kann als Cheatsheet für die Integration von Suchfunktionen in Ihre Web-App herangezogen werden.

Die Anwendung ist hier verfügbar: 
* [Beispiel](https://github.com/azure-samples/azure-search-dotnet-samples/tree/master/search-website)
* [Demowebsite: aka.ms/azs-good-books](https://aka.ms/azs-good-books)

## <a name="azure-sdk-azuresearchdocuments"></a>Azure SDK Azure.Search.Documents

Von der Funktions-App wird das Azure SDK für Cognitive Search verwendet:

* NuGet: [Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents/)
* Referenzdokumentation: [Clientbibliothek](/dotnet/api/overview/azure/search)

Die Funktions-App authentifiziert sich über das SDK gegenüber der cloudbasierten Cognitive Search-API unter Verwendung Ihres Ressourcennamens, Ihres Ressourcenschlüssels und Ihres Indexnamens. Die Geheimnisse sind in den Einstellungen der statischen Web-App gespeichert und werden als Umgebungsvariablen in die Funktion gepullt. 

## <a name="configure-secrets-in-a-localsettingsjson-file"></a>Konfigurieren von Geheimnissen in der Datei „local.settings.json“

1. Erstellen Sie unter `./api/` eine neue Datei mit dem Namen `local.settings.json`, und kopieren Sie das folgende JSON-Objekt in die Datei:

    ```json
    {
      "IsEncrypted": false,
      "Values": {
        "AzureWebJobsStorage": "",
        "FUNCTIONS_WORKER_RUNTIME": "dotnet",
        "SearchApiKey": "YOUR_SEARCH_QUERY_KEY",
        "SearchServiceName": "YOUR_SEARCH_RESOURCE_NAME",
        "SearchIndexName": "good-books"
      }
    }
    ```

1. Ändern Sie Folgendes in Ihre eigenen Suchressourcenwerte: 
    * YOUR_SEARCH_RESOURCE_NAME
    * YOUR_SEARCH_QUERY_KEY

## <a name="azure-function-search-the-catalog"></a>Azure-Funktion: Durchsuchen des Katalogs

Die [API](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/search-website/api/Search.cs) `Search` akzeptiert einen Suchbegriff, durchsucht die Dokumente im Suchindex und gibt eine Liste mit Treffern zurück. 

Die Azure-Funktion pullt die Informationen der Suchkonfiguration und führt die Abfrage aus.

:::code language="csharp" source="~/azure-search-dotnet-samples/search-website/api/Search.cs" highlight="22-24, 55" :::

## <a name="client-search-from-the-catalog"></a>Client: Suchen über den Katalog

Rufen Sie mithilfe des folgenden Codes die Azure-Funktion im React-Client auf: 

:::code language="javascript" source="~/azure-search-dotnet-samples/search-website/src/pages/Search/Search.js" highlight="40-51" :::

## <a name="azure-function-suggestions-from-the-catalog"></a>Azure-Funktion: Vorschläge aus dem Katalog

Die [API](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/search-website/api/Suggest.cs) `Suggest` nimmt einen Suchbegriff während der Eingabe eines Benutzers entgegen und schlägt Suchbegriffe wie Buchtitel und Autoren aus den Dokumenten im Suchindex vor. Dabei wird eine kurze Liste mit Treffern zurückgegeben. 

Der Suchvorschlagsfunktion (`sg`) wird in der bei Massenuploadvorgängen verwendeten [Schemadatei](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/search-website/bulk-insert/BookSearchIndex.cs) definiert.

:::code language="csharp" source="~/azure-search-dotnet-samples/search-website/api/Suggest.cs" highlight="21-23, 50-52" :::

## <a name="client-suggestions-from-the-catalog"></a>Client: Vorschläge aus dem Katalog

Die Vorschlagsfunktions-API wird in der React-App unter `\src\components\SearchBar\SearchBar.js` im Rahmen der Komponenteninitialisierung aufgerufen:

:::code language="javascript" source="~/azure-search-dotnet-samples/search-website/src/components/SearchBar/SearchBar.js" highlight="52-60" :::

## <a name="azure-function-get-specific-document"></a>Azure-Funktion: Abrufen eines bestimmten Dokuments 

Die [API](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/search-website/api/Lookup.cs) `Lookup` akzeptiert eine ID und gibt das Dokumentobjekt aus dem Suchindex zurück. 

:::code language="csharp" source="~/azure-search-dotnet-samples/search-website/api/Lookup.cs" highlight="19-21, 42" :::

## <a name="client-get-specific-document"></a>Client: Abrufen eines bestimmten Dokuments 

Diese Funktions-API wird in der React-App unter `\src\pages\Details\Detail.js` im Rahmen der Komponenteninitialisierung aufgerufen:

:::code language="javascript" source="~/azure-search-dotnet-samples/search-website/src/pages/Details/Details.js" highlight="19-29" :::

## <a name="c-models-to-support-function-app"></a>C#-Modelle zur Unterstützung der Funktions-App

Die folgenden Modelle werden verwendet, um die Funktionen in dieser App zu unterstützen:

:::code language="csharp" source="~/azure-search-dotnet-samples/search-website/api/Models.cs" :::

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Indizieren von Azure SQL-Daten mithilfe des .NET SDK](search-indexer-tutorial.md)
