---
title: 'Python-Tutorial: Hinzufügen von Suchfunktionen zu Web-Apps'
titleSuffix: Azure Cognitive Search
description: Sie können mit Python unter Verwendung des PYPI-Paket-SDK „azure-search-documents“ einen Index erstellen und CSV-Daten in den Suchindex importieren.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 05/25/2021
ms.custom: devx-track-python
ms.devlang: python
ms.openlocfilehash: a9678e542e577c13141f18e59fe2e628ed465321
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110580625"
---
# <a name="2---create-and-load-search-index-with-python"></a>2\. Erstellen und Laden des Suchindexes mit Python

Setzen Sie die Erstellung der Website mit Suchunterstützung fort, indem Sie folgende Schritte ausführen:
* Erstellen einer Suchressource mit der VS Code-Erweiterung
* Sie können mit Python unter Verwendung des Beispielskripts und Azure SDK [azure-search-documents](https://pypi.org/project/azure-search-documents/) einen neuen Index erstellen und Daten importieren.

## <a name="create-an-azure-cognitive-search-resource"></a>Erstellen einer Ressource für Azure Cognitive Search 

Erstellen Sie mit der [Azure Cognitive Search](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurecognitivesearch)-Erweiterung für Visual Studio Code eine neue Suchressource.

1. Öffnen Sie in Visual Studio Code die [Aktivitätsleiste](https://code.visualstudio.com/docs/getstarted/userinterface), und wählen Sie das Azure-Symbol aus. 

1. Klicken Sie auf der Seitenleiste im Bereich `Azure: Cognitive Search` **mit der rechten Maustaste auf Ihr Azure-Abonnement**, und wählen Sie **Create new search service** (Neuen Suchdienst erstellen) aus.

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-create-search-resource.png" alt-text="Rechtsklicken auf das Azure-Abonnement auf der Seitenleiste im Bereich **Azure: Cognitive Search** und Auswählen von **Create new search service** (Neuen Suchdienst erstellen)":::

1. Geben Sie gemäß der Eingabeaufforderungen die folgenden Informationen an:

    |Prompt|EINGABETASTE|
    |--|--|
    |Geben Sie einen global eindeutigen Namen für den neuen Suchdienst ein.|**Notieren Sie sich diesen Namen.** Dieser Ressourcenname wird Teil des Ressourcenendpunkts.|
    |Auswählen einer Ressourcengruppe für neue Ressourcen|Verwenden Sie die Ressourcengruppe, die Sie für dieses Tutorial erstellt haben.|
    |Wählen Sie die SKU für Ihren Suchdienst aus.|Wählen Sie in diesem Tutorial die Option **Free** aus. Sie können einen Tarif nicht mehr ändern, nachdem der Dienst erstellt wurde.|
    |„Select a location for new resources.“ (Wählen Sie einen Standort für neue Ressourcen aus.)|Wählen Sie eine Region in Ihrer Nähe aus.|

1. Nach Ihrer Eingabe wird die neue Suchressource erstellt. 

## <a name="get-your-search-resource-admin-key"></a>Abrufen des Administratorschlüssels für die Suchressource

Rufen Sie den Administratorschlüssel für die Suchressource mit der Visual Studio Code-Erweiterung ab. 

1. Klicken Sie in Visual Studio Code auf der Seitenleiste mit der rechten Maustaste auf die Suchressource, und wählen Sie **Copy Admin Key** (Administratorschlüssel kopieren) aus.

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-copy-admin-key.png" alt-text="Rechtsklicken auf die Suchressource auf der Seitenleiste und Auswählen von **Copy Admin Key** (Administratorschlüssel kopieren)":::

1. Notieren Sie sich diesen Administratorschlüssel. Sie benötigen ihn zum Erstellen von Objekten in [einem späteren Abschnitt](#prepare-the-bulk-import-script-for-search). 

## <a name="prepare-the-bulk-import-script-for-search"></a>Vorbereiten des Massenimportskripts für die Suche

Das Skript verwendet das Azure SDK für Cognitive Search:

* [PYPI-Paket „azure-search-documents“](https://pypi.org/project/azure-search-documents/)
* [Referenzdokumentation](/python/api/azure-search-documents)

1. Öffnen Sie in Visual Studio Code die Datei `bulk_upload.py` im Unterverzeichnis `search-website/bulk-upload`, und ersetzen Sie die folgenden Variablen durch Ihre eigenen Werte, um sich mit dem Azure Search SDK zu authentifizieren:

    * YOUR-SEARCH-RESOURCE-NAME
    * YOUR-SEARCH-ADMIN-KEY

    :::code language="python" source="~/azure-search-python-samples/search-website/bulk-upload/bulk-upload.py" highlight="12,13, 117" :::

1. Öffnen Sie in Visual Studio ein integriertes Terminal für das Unterverzeichnis `search-website/bulk-upload` des Projektverzeichnisses, und führen Sie den folgenden Befehl aus, um die Abhängigkeiten zu installieren: 

    # <a name="macoslinux"></a>[macOS/Linux](#tab/linux-install)
    
    ```bash
    python3 -m pip install -r requirements.txt 
    ```
    
    # <a name="windows"></a>[Windows](#tab/windows-install)

    ```bash
    py -m pip install -r requirements.txt 
    ```

## <a name="run-the-bulk-import-script-for-search"></a>Ausführen des Massenimportskripts für die Suche

1. Verwenden Sie weiterhin das integrierte Terminal in Visual Studio für das Unterverzeichnis `search-website/bulk-upload` des Projektverzeichnisses, um den folgenden Bash-Befehl zum Ausführen des Skripts `bulk_upload.py` auszuführen:

    # <a name="macoslinux"></a>[macOS/Linux](#tab/linux-run)
    
    ```bash
    python3 bulk-upload.py
    ```
    
    # <a name="windows"></a>[Windows](#tab/windows-run)

    ```bash
    py bulk-upload.py
    ```


1. Während der Codeausführung wird in der Konsole der Status angezeigt. 
1. Nach Abschluss des Uploads lautet die letzte in der Konsole ausgegebene Meldung „Fertig. Betätigen Sie eine beliebige Taste, um das Terminal zu schließen.“

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

[Bereitstellen Ihrer statischen Web-App](tutorial-python-deploy-static-web-app.md)