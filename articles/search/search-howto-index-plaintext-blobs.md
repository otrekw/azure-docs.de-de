---
title: Durchsuchen von Nur-Text-Blobs
titleSuffix: Azure Cognitive Search
description: Konfigurieren Sie einen Suchindexer, um Nur-Text aus Azure-Blobs für eine Volltextsuche in Azure Cognitive Search zu extrahieren.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 417bdacc3ce8b619d5ec9618e6060ac071882471
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91533924"
---
# <a name="how-to-index-plain-text-blobs-in-azure-cognitive-search"></a>Indizieren von Nur-Text-Blobs in Azure Cognitive Search

Wenn Sie einen [Blob-Indexer](search-howto-indexing-azure-blob-storage.md) verwenden, um durchsuchbaren Text für die Volltextsuche zu extrahieren, können Sie verschiedene Verarbeitungsmodi aufrufen, um bessere Indizierungsergebnisse zu erzielen. In der Standardeinstellung analysiert der Indexer durch Trennzeichen getrennte Blobs als ein einzelnes Textsegment. Wenn jedoch alle Ihre Blobs Nur-Text in derselben Codierung enthalten, können Sie die Indizierungsleistung signifikant verbessern, indem Sie den **Textanalysemodus** nutzen.

## <a name="set-up-plain-text-indexing"></a>Einrichten der Nur-Text-Indizierung

Erstellen oder aktualisieren Sie zum Indizieren von Nur-Text-Blobs eine Indexerdefinition mit der für `text` festgelegten `parsingMode`-Konfigurierungseigenschaft in einer [Indexer erstellen](/rest/api/searchservice/create-indexer)-Anforderung:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }
```

Standardmäßig wird von der `UTF-8`-Codierung ausgegangen. Um eine andere Codierung anzugeben, verwenden Sie die `encoding`-Konfigurationseigenschaft. 

```http
    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }
```

## <a name="request-example"></a>Anforderungsbeispiel

In der Indexerdefinition werden die Definitionsmodi angegeben.

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-plaintext-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }
```

## <a name="help-us-make-azure-cognitive-search-better"></a>Helfen Sie uns bei der Verbesserung der kognitiven Azure-Suche

Wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search/) dafür ab. Wenn Sie Hilfe bei der Verwendung des vorhandenen Features benötigen, veröffentlichen Sie Ihre Frage in [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870).

## <a name="next-steps"></a>Nächste Schritte

* [Indexer in der kognitiven Azure-Suche](search-indexer-overview.md)
* [Indizieren von Dokumenten in Azure Blob Storage mit der kognitiven Azure-Suche](search-howto-indexing-azure-blob-storage.md)
* [Hinzufügen der Volltextsuche zu Azure-Blobdaten mithilfe von Azure Cognitive Search](search-blob-storage-integration.md)