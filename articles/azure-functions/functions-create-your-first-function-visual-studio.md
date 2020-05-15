---
title: 'Schnellstart: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Visual Studio eine Azure-Funktion mit HTTP-Trigger erstellen und veröffentlichen.
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.topic: quickstart
ms.date: 03/06/2020
ms.custom: mvc, devcenter, vs-azure, 23113853-34f2-4f
ms.openlocfilehash: a4549bd2947332d7140f4f440a5344f417430554
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122747"
---
# <a name="quickstart-create-your-first-function-in-azure-using-visual-studio"></a>Schnellstart: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio

Mit Azure Functions können Sie Ihren Code in einer serverlosen Umgebung ausführen, ohne vorher einen virtuellen Computer zu erstellen oder eine Webanwendung zu veröffentlichen.

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Visual Studio 2019 eine C#-Funktions-App vom Typ „Hallo Welt“ mit HTTP-Trigger lokal erstellen und testen und anschließend in Azure veröffentlichen. 

![localhost-Antwort der Funktion im Browser](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local-final.png)

Diese Schnellstartanleitung gilt für Visual Studio 2019. 

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie für dieses Tutorial zunächst [Visual Studio 2019](https://azure.microsoft.com/downloads/). Wählen Sie bei der Installation die Workload **Azure-Entwicklung** aus. Falls Sie stattdessen ein Azure Functions-Projekt mit Visual Studio 2017 erstellen möchten, müssen Sie zuerst die [aktuellen Azure Functions-Tools](functions-develop-vs.md#check-your-tools-version) installieren.

![Installieren von Visual Studio mit der Workload „Azure-Entwicklung“](media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Wenn Sie kein [Azure-Abonnement](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/dotnet/), bevor Sie beginnen.

## <a name="create-a-function-app-project"></a>Erstellen eines Funktionen-App-Projekts

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio erstellt ein Projekt und eine Klasse mit Codebausteinen für den Funktionstyp „HTTP-Trigger“. Das Methodenattribut `FunctionName` dient zum Festlegen des Namens der Funktion. Der Standardname lautet `Function1`. Mit dem Attribut `HttpTrigger` wird angegeben, dass die Funktion von einer HTTP-Anforderung ausgelöst wird. Mit Code in den Codebausteinen wird eine HTTP-Antwort gesendet, die einen Wert aus dem Anforderungstext oder der Abfragezeichenfolge enthält.

Sie können den Funktionsumfang Ihrer Funktion mit Eingabe- und Ausgabebindungen erweitern, indem Sie die geeigneten Attribute auf die Methode anwenden. Weitere Informationen finden Sie im Abschnitt [Trigger und Bindungen](functions-dotnet-class-library.md#triggers-and-bindings) in der [C#-Entwicklerreferenz zu Azure Functions](functions-dotnet-class-library.md).

Nachdem Sie nun Ihr Funktionsprojekt und eine Funktion mit HTTP-Trigger erstellt haben, können Sie sie auf Ihrem lokalen Computer testen.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Visual Studio wird in Azure Functions Core Tools integriert, sodass Sie Ihre Funktionen mithilfe der vollständigen Azure Functions-Runtime lokal testen können.  

[!INCLUDE [functions-run-function-test-local-vs](../../includes/functions-run-function-test-local-vs.md)]

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

Sie müssen in Ihrem Azure-Abonnement über eine Funktions-App verfügen, um Ihr Projekt veröffentlichen zu können. Bei der Veröffentlichung in Visual Studio wird eine Funktions-App für Sie erstellt, wenn Sie das Projekt zum ersten Mal veröffentlichen.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testen der Funktion in Azure

1. Kopieren Sie die auf der Profilseite **Veröffentlichen** angegebene Basis-URL der Funktions-App. Ersetzen Sie den Teil `localhost:port` der URL, die Sie zum lokalen Testen der Funktion verwendet haben, durch die neue Basis-URL. Hängen Sie anschließend die Abfragezeichenfolge `?name=<YOUR_NAME>` an diese URL an, und führen Sie die Anforderung aus.

    Die URL, über die Ihre Funktion mit HTTP-Trigger aufgerufen wird, hat das folgende Format:

    `http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?name=<YOUR_NAME>`

2. Fügen Sie diese neue URL für die HTTP-Anforderung in die Adresszeile des Browsers ein. Die folgende Abbildung zeigt die Antwort des Browsers auf die von der Funktion zurückgegebene GET-Remoteanforderung:

    ![Funktionsantwort im Browser](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe von Visual Studio eine C#-Funktions-App in Azure mit einer einfachen HTTP-Triggerfunktion erstellt und veröffentlicht. 

Im nächsten Artikel erfahren Sie, wie Sie Ihrer Funktion eine Azure Storage-Warteschlangenbindung zu hinzufügen:
> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Funktion](functions-add-output-binding-storage-queue-vs.md)

