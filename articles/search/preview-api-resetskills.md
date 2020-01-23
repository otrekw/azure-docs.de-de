---
title: Qualifikationen zurücksetzen (api-version=2019-050-06-Preview)
titleSuffix: Azure Cognitive Search
description: Die Vorschau-REST-API wird für die inkrementelle Anreicherung verwendet, wenn Sie eine vollständige oder partielle Wiederverarbeitung eines Skillsets benötigen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 4557e966c431eca31aa7577bebc75caae9365dc9
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75832155"
---
# <a name="reset-skills-api-version2019-05-06-preview"></a>Qualifikationen zurücksetzen (api-version=2019-05-06-Preview)

> [!IMPORTANT] 
> Die inkrementelle Anreicherung ist derzeit als öffentliche Vorschauversion verfügbar. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Die Anforderung **Qualifikationen zurücksetzen** gibt an, welche Qualifikationen bei der nächsten Indexerausführung verarbeitet werden sollen. Bei Indexern, für die das Zwischenspeichern aktiviert ist, können Sie explizit die Verarbeitung für Qualifikationsupdates anfordern, die der Indexer nicht erkennen kann. Wenn Sie z. B. externe Änderungen wie Überarbeitungen an einer benutzerdefinierten Qualifikation vornehmen, können Sie diese API verwenden, um die Qualifikation erneut auszuführen. Ausgaben, wie z. B. ein Wissensspeicher oder ein Suchindex, werden mit wiederverwendbaren Daten aus dem Cache und neuen Inhalten gemäß der aktualisierten Qualifikation aktualisiert.

Sie können ein vorhandenes [Skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) mithilfe einer HTTP-PUT-Methode zurücksetzen, indem Sie den Namen des Skillsets, das aktualisiert werden soll, im Anforderungs-URI angeben. Sie müssen **api-version=2019-05-06-Preview** in der Anforderung verwenden.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]/resetskills?api-version=2019-05-06-Preview
api-key: [admin key]
Content-Type: application/json
```  

Verwenden Sie alternativ POST, und platzieren Sie den Indexernamen im Text der Anforderung:

```http
POST https://[service name].search.windows.net/indexers/resetskills?api-version=[api-version]  
api-key: [admin key]  
```  

## <a name="request-headers"></a>Anforderungsheader  

 Die folgende Tabelle beschreibt die erforderlichen und optionalen Anforderungsheader.  

|Anforderungsheader|Beschreibung|  
|--------------------|-----------------|  
|*Content-Type:*|Erforderlich. Auf `application/json`|  
|*api-key:*|Erforderlich. `api-key` wird zum Authentifizieren der Anforderung beim Search-Dienst verwendet. Es handelt sich um einen für Ihren Dienst eindeutigen Zeichenfolgewert. Die Anforderung **Skillset zurücksetzen** muss einen `api-key`-Header enthalten, der auf Ihren Verwaltungsschlüssel festgelegt ist (im Gegensatz zu einem Abfrageschlüssel).|  

Sie benötigen auch den Dienstnamen, um die Anforderungs-URL zu erstellen. Sie können sowohl den Dienstnamen als auch `api-key` auf Ihrer Dienstübersichtsseite im Azure-Portal abrufen. Hilfe bei der Seitennavigation finden Sie unter [Schnellstart: Erstellen eines Azure Cognitive Search-Diensts im Portal](https://docs.microsoft.com/azure/search/search-create-service-portal).  

## <a name="request-body-syntax"></a>Syntax des Anforderungstexts  

Bei dem Anforderungstext handelt es sich um ein Array von Qualifikationsnamen.

```json
{   
    "skillNames" : ["<SKILL-1>", "SKILL-n"]
}  
```

## <a name="response"></a>Antwort  

Statuscode: „204 No Content“ für eine erfolgreiche Antwort. 

## <a name="see-also"></a>Weitere Informationen

+ [Azure Cognitive Search-Dienst-REST](https://docs.microsoft.com/rest/api/searchservice)
+ [HTTP-Statuscodes (Azure Cognitive Search)](https://docs.microsoft.com/rest/api/searchservice/http-status-codes)  
+ [Einführung in die KI in Azure Cognitive Search](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro)
+ [Konfigurieren der Zwischenspeicherung und inkrementelle Anreicherung](search-howto-incremental-index.md)
+ [Inkrementelle Anreicherung](cognitive-search-incremental-indexing-conceptual.md)