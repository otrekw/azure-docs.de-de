---
title: 'Schnellstart: Serverlose Anwendungen mit Azure SignalR Service – JavaScript'
description: Eine Schnellstartanleitung für die Verwendung des Azure SignalR-Diensts und von Azure Functions zum Erstellen eines Chatraums.
author: sffamily
ms.service: signalr
ms.devlang: javascript
ms.topic: quickstart
ms.date: 12/14/2019
ms.author: zhshang
ms.openlocfilehash: eadeb0f0203868c2a1a37190fdd46e47bf26e8f7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450264"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-javascript"></a>Schnellstart: Erstellen eines Chatraums mit Azure Functions und dem SignalR-Dienst mithilfe von JavaScript

Mit dem Azure SignalR-Dienst können Sie Ihrer Anwendung ganz einfach Echtzeitfunktionen hinzufügen. Azure Functions ist eine serverlose Plattform, mit der Sie Ihren Code ohne Verwaltung von Infrastruktur ausführen können. Erfahren Sie in diesem Schnellstart, wie Sie den SignalR-Dienst und Azure Functions verwenden, um eine serverlose Chatanwendung in Echtzeit zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Dieser Schnellstart kann unter MacOS, Windows oder Linux ausgeführt werden.

Stellen Sie sicher, dass Sie einen Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/) installiert haben.

Installieren Sie die [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing) (Version 2 oder höher), um Azure Functions-Apps lokal auszuführen.

In diesem Schnellstart wird [Node.js](https://nodejs.org/en/download/) 10.x verwendet, es sollte aber auch mit anderen Versionen funktionieren. Weitere Informationen zu unterstützten Node.js-Versionen finden Sie in der [Übersicht über die Runtimeversionen von Azure Functions](../azure-functions/functions-versions.md#languages).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter <https://portal.azure.com/> mit Ihrem Azure-Konto beim Azure-Portal an.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

## <a name="configure-and-run-the-azure-function-app"></a>Konfigurieren und Ausführen der Azure Functions-App

1. Bestätigen Sie im Browser, in dem das Azure-Portal geöffnet ist, dass die von Ihnen zuvor bereitgestellte SignalR-Dienstinstanz erfolgreich erstellt wurde, indem Sie nach ihrem Namen im Suchfeld oben im Portal suchen. Wählen Sie die Instanz aus, um sie zu öffnen.

    ![Suchen nach der SignalR-Dienstinstanz](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. Wählen Sie **Schlüssel** aus, um die Verbindungszeichenfolgen für die SignalR-Dienstinstanz anzuzeigen.

1. Wählen Sie die primäre Verbindungszeichenfolge aus, und kopieren Sie diese.

    ![Erstellen des SignalR-Diensts](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-keys.png)

1. Öffnen Sie in Ihrem Code-Editor den Ordner *src/chat/javascript* im geklonten Repository.

1. Benennen Sie *local.settings.sample.json* in *local.settings.json* um.

1. Fügen Sie in **local.settings.json** die Verbindungszeichenfolge in den Wert der **AzureSignalRConnectionString**-Einstellung ein. Speichern Sie die Datei .

1. JavaScript-Funktionen werden in Ordnern organisiert. In jedem Ordner befinden sich zwei Dateien: *function.json* definiert die Bindungen, die in der Funktion verwendet werden, und *index.js* ist der Textkörper der Funktion. Es gibt zwei durch HTTP ausgelöste Funktionen in dieser Funktions-App:

    - **negotiate**: Verwendet die *SignalRConnectionInfo*-Eingabebindung, um gültige Verbindungsinformationen zu generieren und zurückzugeben.
    - **messages**: Empfängt eine Chatnachricht im Anforderungstext und verwendet die *SignalR*-Ausgabebindung, um die Nachricht an alle verbundenen Clientanwendungen zu senden.

1. Stellen Sie im Terminal sicher, dass Sie sich im Ordner *src/chat/javascript* befinden. Führen Sie die Funktions-App aus.

    ```bash
    func start
    ```

    ![Erstellen des SignalR-Diensts](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-run-application.png)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine serverlose Echtzeitanwendung in VS Code erstellt und ausgeführt. Im nächsten Schritt erfahren Sie mehr darüber, wie Sie Azure Functions aus VS Code bereitstellen.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure Functions mit VS Code](/azure/javascript/tutorial-vscode-serverless-node-01)
