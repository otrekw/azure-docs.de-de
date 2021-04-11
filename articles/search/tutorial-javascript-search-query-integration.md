---
title: 'JavaScript-Tutorial: Highlights der Suchintegration'
titleSuffix: Azure Cognitive Search
description: Hier finden Sie grundlegende Informationen zu den JavaScript SDK-Suchabfragen, die auf Website mit Suchunterstützung verwendet werden.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 03/09/2021
ms.custom: devx-track-js
ms.devlang: javascript
ms.openlocfilehash: cf4e1b1ecf209b587a45ca4c43607bfa95155aee
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104723499"
---
# <a name="4---search-integration-highlights"></a>4: Highlights der Suchintegration

In den vorherigen Lektionen haben Sie einer statischen Web-App eine Suchfunktion hinzugefügt. In dieser Lektion werden die wesentlichen Schritte zum Einrichten der Integration hervorgehoben. Dieser Artikel kann als Cheat Sheet für die Integration von Suchfunktionen in Ihre JavaScript-App herangezogen werden.

## <a name="azure-sdk-azuresearch-documents"></a>Azure SDK @azure/search-documents 

Von der Funktions-App wird das Azure SDK für Cognitive Search verwendet:

* NPM: [@azure/search-documents](https://www.npmjs.com/package/@azure/search-documents)
* Referenzdokumentation: [Clientbibliothek](/javascript/api/overview/azure/search-documents-readme)

Die Funktions-App authentifiziert sich über das SDK gegenüber der cloudbasierten Cognitive Search-API unter Verwendung Ihres Ressourcennamens, Ihres Ressourcenschlüssels und Ihres Indexnamens. Die Geheimnisse sind in den Einstellungen der statischen Web-App gespeichert und werden als Umgebungsvariablen in die Funktion gepullt. 

## <a name="configure-secrets-in-a-configuration-file"></a>Konfigurieren von Geheimnissen in einer Konfigurationsdatei

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/api/config.js" highlight="3,4" :::

## <a name="azure-function-search-the-catalog"></a>Azure-Funktion: Durchsuchen des Katalogs

Die [API](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/api/Search/index.js) `Search` akzeptiert einen Suchbegriff, durchsucht die Dokumente im Suchindex und gibt eine Liste mit Treffern zurück. 

Das Routing für die Such-API ist in den Bindungen vom Typ [function.json](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/api/Search/function.json) enthalten.

Die Azure-Funktion pullt die Informationen der Suchkonfiguration und führt die Abfrage aus.

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/api/Search/index.js" highlight="4-9, 75" :::

## <a name="client-search-from-the-catalog"></a>Client: Suchen über den Katalog

Rufen Sie mithilfe des folgenden Codes die Azure-Funktion im React-Client auf: 

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/src/pages/Search/Search.js" highlight="40-51" :::

## <a name="azure-function-suggestions-from-the-catalog"></a>Azure-Funktion: Vorschläge aus dem Katalog

Die [API](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/api/Suggest/index.js) `Suggest` nimmt einen Suchbegriff während der Eingabe eines Benutzers entgegen und schlägt Suchbegriffe wie Buchtitel und Autoren aus den Dokumenten im Suchindex vor. Dabei wird eine kurze Liste mit Treffern zurückgegeben. 

Der Suchvorschlagsfunktion (`sg`) wird in der bei Massenuploadvorgängen verwendeten [Schemadatei](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/bulk-insert/good-books-index.json) definiert.

Das Routing für die Vorschlags-API ist in den Bindungen vom Typ [function.json](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/api/Suggest/function.json) enthalten.

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/api/Suggest/index.js" highlight="4-9, 21" :::

## <a name="client-suggestions-from-the-catalog"></a>Client: Vorschläge aus dem Katalog

Die Vorschlagsfunktions-API wird in der React-App unter `\src\components\SearchBar\SearchBar.js` im Rahmen der Komponenteninitialisierung aufgerufen:

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/src/components/SearchBar/SearchBar.js" highlight="52-60" :::

## <a name="azure-function-get-specific-document"></a>Azure-Funktion: Abrufen eines bestimmten Dokuments 

Die [API](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/api/Lookup/index.js) `Lookup` akzeptiert eine ID und gibt das Dokumentobjekt aus dem Suchindex zurück. 

Das Routing für die Lookup-API ist in den Bindungen vom Typ [function.json](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/search-website/api/Lookup/function.json) enthalten.

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/api/Lookup/index.js" highlight="4-9, 17" :::

## <a name="client-get-specific-document"></a>Client: Abrufen eines bestimmten Dokuments 

Diese Funktions-API wird in der React-App unter `\src\pages\Details\Detail.js` im Rahmen der Komponenteninitialisierung aufgerufen:

:::code language="javascript" source="~/azure-search-javascript-samples/search-website/src/pages/Details/Details.js" highlight="19-29" :::

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Indizieren von Azure SQL-Daten mithilfe des .NET SDK](search-indexer-tutorial.md)
