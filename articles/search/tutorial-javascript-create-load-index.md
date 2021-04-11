---
title: 'JavaScript-Tutorial: Hinzufügen von Suchfunktionen zu Web-Apps'
titleSuffix: Azure Cognitive Search
description: Erstellen Sie einen Index, und importieren Sie CSV-Dateien mit JavaScript mithilfe des npm SDK @azure/search-documents in den Suchindex.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 03/18/2021
ms.custom: devx-track-js
ms.devlang: javascript
ms.openlocfilehash: 0fd28262f4a4b852386fa354037e69c5097109c5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104723538"
---
# <a name="2---create-and-load-search-index-with-javascript"></a>2: Erstellen und Laden des Suchindexes mit JavaScript

Setzen Sie die Erstellung der Website mit Suchunterstützung fort, indem Sie folgende Schritte ausführen:
* Erstellen einer Suchressource mit der VS Code-Erweiterung
* Erstellen eines neuen Indexes und Importieren von Daten mit JavaScript mithilfe des Beispielskripts und des Azure SDK [@azure/search-documents](https://www.npmjs.com/package/@azure/search-documents)

## <a name="create-an-azure-search-resource"></a>Erstellen einer Azure Search-Ressource 

Erstellen Sie mit der [Azure Cognitive Search](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurecognitivesearch)-Erweiterung für Visual Studio Code eine neue Suchressource.

1. Öffnen Sie in Visual Studio Code die [Aktivitätsleiste](https://code.visualstudio.com/docs/getstarted/userinterface), und wählen Sie das Azure-Symbol aus. 

1. Klicken Sie auf der Seitenleiste im Bereich `Azure: Cognitive Search` **mit der rechten Maustaste auf Ihr Azure-Abonnement**, und wählen Sie **Create new search service** (Neuen Suchdienst erstellen) aus.

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-create-search-resource.png" alt-text="Rechtsklicken auf das Azure-Abonnement auf der Seitenleiste im Bereich **Azure: Cognitive Search** und Auswählen von **Create new search service** (Neuen Suchdienst erstellen)":::

1. Geben Sie gemäß der Eingabeaufforderungen die folgenden Informationen an:

    |Prompt|EINGABETASTE|
    |--|--|
    |Geben Sie einen global eindeutigen Namen für den neuen Suchdienst ein.|**Notieren Sie sich diesen Namen.** Dieser Ressourcenname wird Teil des Ressourcenendpunkts.|
    |Auswählen einer Ressourcengruppe für neue Ressourcen|Verwenden Sie die Ressourcengruppe, die Sie für dieses Tutorial erstellt haben.|
    |Wählen Sie die SKU für Ihren Suchdienst aus.|Wählen Sie in diesem Tutorial die Option **Free** aus. Sie können einen SKU-Tarif nicht mehr ändern, nachdem der Dienst erstellt wurde.|
    |Wählen Sie einen Standort für neue Ressourcen aus.|Wählen Sie eine Region in Ihrer Nähe aus.|

1. Nach Ihrer Eingabe wird die neue Suchressource erstellt. 

## <a name="get-your-search-resource-admin-key"></a>Abrufen des Administratorschlüssels für die Suchressource

Rufen Sie den Administratorschlüssel für die Suchressource mit der Visual Studio Code-Erweiterung ab. 

1. Klicken Sie in Visual Studio Code auf der Seitenleiste mit der rechten Maustaste auf die Suchressource, und wählen Sie **Copy Admin Key** (Administratorschlüssel kopieren) aus.

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-copy-admin-key.png" alt-text="Rechtsklicken auf die Suchressource auf der Seitenleiste und Auswählen von **Copy Admin Key** (Administratorschlüssel kopieren)":::

1. Notieren Sie sich diesen Administratorschlüssel. Sie benötigen ihn in [einem späteren Abschnitt](#prepare-the-bulk-import-script-for-search). 

## <a name="prepare-the-bulk-import-script-for-search"></a>Vorbereiten des Massenimportskripts für die Suche

Das Skript verwendet das Azure SDK für Cognitive Search:

* [npm-Paket @azure/search-documents](https://www.npmjs.com/package/@azure/search-documents)
* [Referenzdokumentation](/javascript/api/overview/azure/search-documents-readme)

1. Öffnen Sie in Visual Studio Code die Datei `bulk_insert_books.js` im Unterverzeichnis `search-web/bulk-insert`, und ersetzen Sie die folgenden Variablen durch Ihre eigenen Werte, um sich mit dem Azure Search SDK zu authentifizieren:

    * YOUR-SEARCH-RESOURCE-NAME
    * YOUR-SEARCH-ADMIN-KEY

    :::code language="javascript" source="~/azure-search-javascript-samples/search-website/bulk-insert/bulk_insert_books.js" highlight="16,17" :::

1. Öffnen Sie in Visual Studio ein integriertes Terminal für das Unterverzeichnis `search-web/bulk-insert` des Projektverzeichnisses, und führen Sie den folgenden Befehl aus, um die Abhängigkeiten zu installieren: 

    ```bash
    npm install 
    ```

## <a name="run-the-bulk-import-script-for-search"></a>Ausführen des Massenimportskripts für die Suche

1. Verwenden Sie weiterhin das integrierte Terminal in Visual Studio für das Unterverzeichnis `search-web/bulk-insert` des Projektverzeichnisses, um den folgenden Bash-Befehl zum Ausführen des Skripts `bulk_insert_books.js` auszuführen:

    ```javascript
    npm start
    ```

1. Während der Codeausführung wird in der Konsole der Status angezeigt. 
1. Nach Abschluss des Uploads lautet die letzte in der Konsole ausgegebene Meldung „Done“ (Fertig).

## <a name="review-the-new-search-index"></a>Überprüfen des neuen Suchindexes

Nach Abschluss des Uploads kann der Suchindex verwendet werden. Überprüfen Sie den neuen Suchindex.

1. Öffnen Sie in Visual Studio Code die Azure Cognitive Search-Erweiterung, und wählen Sie Ihre Suchressource aus.  

    :::image type="content" source="media/tutorial-javascript-create-load-index/visual-studio-code-search-extension-view-resource.png" alt-text="Öffnen der Azure Cognitive Search-Erweiterung in Visual Studio Code und Auswählen der Suchressource":::

1. Erweitern Sie „Indizes“ > „Dokumente“ > `good-books`, und wählen Sie dann ein Dokument aus, um alle dokumentspezifischen Daten anzuzeigen.
 
    :::image type="content" source="media/tutorial-javascript-create-load-index/visual-studio-code-search-extension-view-docs.png" lightbox="media/tutorial-javascript-create-load-index/visual-studio-code-search-extension-view-docs.png" alt-text="Erweitern von „Indizes“ und „good-books“ und Auswählen eines Dokuments":::

## <a name="copy-your-search-resource-name"></a>Kopieren des Namens Ihrer Suchressource

Notieren Sie sich den **Namen der Suchressource**. Sie benötigen ihn, um die Azure Functions-App mit Ihrer Suchressource zu verbinden. 

> [!CAUTION]
> Möglicherweise möchten Sie den Administratorschlüssel für die Suche in der Azure Functions-Instanz verwenden. Dadurch wird jedoch nicht das Prinzip der geringsten Berechtigungen befolgt. Die Azure Functions-Instanz verwendet den Abfrageschlüssel, um das Prinzip der geringsten Berechtigungen einzuhalten. 

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen Ihrer statischen Web-App](tutorial-javascript-deploy-static-web-app.md)