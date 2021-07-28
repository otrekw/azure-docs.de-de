---
title: 'Python-Tutorial: Highlights der Suchintegration'
titleSuffix: Azure Cognitive Search
description: In diesem Cheatsheet finden Sie grundlegende Informationen zu den Python SDK-Suchintegrationsabfragen, die auf Website mit Suchunterstützung verwendet werden.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 05/25/2021
ms.custom: devx-track-python
ms.devlang: python
ms.openlocfilehash: a8f1078e714bd88d2b2e9911a9e1708c21aa91b2
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110580599"
---
# <a name="4---python-search-integration-cheat-sheet"></a>4\. Cheatsheet für die Python-Suchintegration

In den vorherigen Lektionen haben Sie einer statischen Web-App eine Suchfunktion hinzugefügt. In dieser Lektion werden die wesentlichen Schritte zum Einrichten der Integration hervorgehoben. Dieser Artikel kann als Cheatsheet für die Integration von Suchfunktionen in Ihre Python-App herangezogen werden.

Die Anwendung ist hier verfügbar: 
* [Beispiel](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/search-website)
* [Demowebsite: aka.ms/azs-good-books](https://aka.ms/azs-good-books)

## <a name="azure-sdk-azure-search-documents"></a>Azure SDK „azure-search-documents“

Von der Funktions-App wird das Azure SDK für Cognitive Search verwendet:

* [PYPI-Paket „azure-search-documents“](https://pypi.org/project/azure-search-documents/)
* [Referenzdokumentation](/python/api/azure-search-documents)

Azure Function-App authentifiziert sich über das SDK gegenüber der cloudbasierten Cognitive Search-API unter Verwendung Ihres API-Namens, Ihres Ressourcenschlüssels und Ihres Indexnamens. Die Geheimnisse sind in den Einstellungen der statischen Web-App gespeichert und werden als Umgebungsvariablen in die Funktion gepullt. 

## <a name="configure-secrets-in-a-configuration-file"></a>Konfigurieren von Geheimnissen in einer Konfigurationsdatei

Die Umgebungsvariablen der Azure Function-App-Einstellungen werden per Pull aus einer Datei (`__init__.py`) abgerufen, die von den drei API-Funktionen gemeinsam genutzt wird. 

:::code language="python" source="~/azure-search-python-samples/search-website/api/shared_code/__init__.py" highlight="6-9" :::

## <a name="azure-function-search-the-catalog"></a>Azure-Funktion: Durchsuchen des Katalogs

Die Search-[API](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Search/__init__.py) akzeptiert einen Suchbegriff, durchsucht die Dokumente im Suchindex und gibt eine Liste mit Treffern zurück. 

Das Routing für die Such-API ist in den Bindungen vom Typ [function.json](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Search/function.json) enthalten.

Azure Function ruft die Informationen der Suchkonfiguration per Pull ab und führt die Abfrage aus.

:::code language="python" source="~/azure-search-python-samples/search-website/api/Search/__init__.py" highlight="8-18, 122" :::

## <a name="client-search-from-the-catalog"></a>Client: Suchen über den Katalog

Rufen Sie mithilfe des folgenden Codes die Azure-Funktion im React-Client auf: 

:::code language="javascript" source="~/azure-search-python-samples/search-website/src/pages/Search/Search.js" highlight="42-55" :::

## <a name="azure-function-suggestions-from-the-catalog"></a>Azure-Funktion: Vorschläge aus dem Katalog

Die [API](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Suggest/__init__.py) `Suggest` nimmt einen Suchbegriff während der Eingabe eines Benutzers entgegen und schlägt Suchbegriffe wie Buchtitel und Autoren aus den Dokumenten im Suchindex vor. Dabei wird eine kurze Liste mit Treffern zurückgegeben. 

Der Suchvorschlagsfunktion (`sg`) wird in der bei Massenuploadvorgängen verwendeten [Schemadatei](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/bulk-upload/good-books-index.json) definiert.

Das Routing für die Vorschlags-API ist in den Bindungen vom Typ [function.json](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Suggest/function.json) enthalten.

:::code language="python" source="~/azure-search-python-samples/search-website/api/Suggest/__init__.py" highlight="8-23, 35" :::

## <a name="client-suggestions-from-the-catalog"></a>Client: Vorschläge aus dem Katalog

Die Vorschlagsfunktions-API wird in der React-App unter `\src\components\SearchBar\SearchBar.js` im Rahmen der Komponenteninitialisierung aufgerufen:

:::code language="javascript" source="~/azure-search-python-samples/search-website/src/components/SearchBar/SearchBar.js" highlight="52-60" :::

## <a name="azure-function-get-specific-document"></a>Azure-Funktion: Abrufen eines bestimmten Dokuments 

Die [API](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Lookup/__init__.py) `Lookup` akzeptiert eine ID und gibt das Dokumentobjekt aus dem Suchindex zurück. 

Das Routing für die Lookup-API ist in den Bindungen vom Typ [function.json](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Lookup/function.json) enthalten.

:::code language="python" source="~/azure-search-python-samples/search-website/api/Lookup/__init__.py" highlight="8-18, 27" :::

## <a name="client-get-specific-document"></a>Client: Abrufen eines bestimmten Dokuments 

Diese Funktions-API wird in der React-App unter `\src\pages\Details\Detail.js` im Rahmen der Komponenteninitialisierung aufgerufen:

:::code language="javascript" source="~/azure-search-python-samples/search-website/src/pages/Details/Details.js" highlight="19-29" :::

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Indizieren von Azure SQL-Daten mithilfe des .NET SDK](search-indexer-tutorial.md)
