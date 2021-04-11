---
title: Entwickeln und Veröffentlichen von .NET 5-Funktionen mit Azure Functions
description: Erfahren Sie, wie Sie C#-Funktionen mithilfe von .NET 5.0 erstellen und debuggen und dann das lokale Projekt in Azure Functions für serverloses Hosting bereitstellen.
ms.date: 03/03/2021
ms.topic: how-to
zone_pivot_groups: development-environment-functions
ms.openlocfilehash: 6403e5d898b76d459a9712f7847e81e5442deeda
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075327"
---
# <a name="develop-and-publish-net-5-functions-using-azure-functions"></a>Entwickeln und Veröffentlichen von .NET 5-Funktionen mit Azure Functions 

In diesem Artikel erfahren Sie, wie Sie unter Verwendung von .NET 5.0 mit C#-Funktionen arbeiten, die prozessextern außerhalb der Azure Functions-Laufzeit ausgeführt werden. Sie erfahren, wie Sie diese .NET-Funktionen für einen isolierten Prozess in Azure erstellen, lokal debuggen und veröffentlichen. In Azure werden diese Funktionen in einem isolierten Prozess ausgeführt, der .NET 5.0 unterstützt. Weitere Informationen finden Sie im [Handbuch zum Ausführen von Funktionen unter .NET 5.0 in Azure](dotnet-isolated-process-guide.md).

Wenn Sie .NET 5.0 nicht unterstützen müssen oder Ihre Funktionen prozessextern ausführen, können Sie stattdessen [eine C#-Klassenbibliotheksfunktion erstellen](functions-create-your-first-function-visual-studio.md).

>[!NOTE]
>Das Entwickeln von .NET-Funktionen für isolierte Prozesse im Azure-Portal wird derzeit nicht unterstützt. Sie müssen entweder die Azure CLI- oder Visual Studio Code-Veröffentlichung verwenden, um eine Funktions-App in Azure zu erstellen, die die prozessexterne Ausführung von .NET 5.0-Apps unterstützt.   

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [.NET SDK 5.0](https://www.microsoft.com/net/download)

+ [Azure Functions Core Tools](functions-run-local.md#v2) ab Version 3.0.3381.

+ [Azure CLI](/cli/azure/install-azure-cli) ab Version 2.20.  
::: zone pivot="development-environment-vscode"
+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)  

+ [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code  

+ Die [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code ab Version 1.3.0.
::: zone-end
::: zone pivot="development-environment-vs"
+ [Visual Studio 2019](https://azure.microsoft.com/downloads/) einschließlich der Workload **Azure-Entwicklung**.  
Projektvorlagen und Veröffentlichung von isolierten .NET-Funktionen sind zurzeit in Visual Studio nicht verfügbar.
::: zone-end

## <a name="create-a-local-function-project"></a>Erstellen eines lokalen Funktionsprojekts

In Azure Functions handelt es sich bei einem Funktionsprojekt um einen Container für eine oder mehrere individuelle Funktionen, die jeweils auf einen bestimmten Trigger reagieren. Für alle Funktionen eines Projekts werden die gleichen lokalen Konfigurationen und Hostkonfigurationen gemeinsam genutzt. In diesem Abschnitt erstellen Sie ein Funktionsprojekt, das nur eine Funktion enthält.

::: zone pivot="development-environment-vs"

>[!NOTE]  
> Zurzeit gibt es keine Visual Studio-Projektvorlagen, die das Erstellen von Projekten für isolierte .NET-Funktionen unterstützen. In diesem Artikel erfahren Sie, wie Sie mit Core Tools Ihr C#-Projekt erstellen, das Sie dann in Visual Studio lokal ausführen und debuggen können.  

::: zone-end

::: zone pivot="development-environment-vscode"  
1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** das Symbol **Neues Projekt erstellen** aus.

    ![Auswählen von „Neues Projekt erstellen“](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Select a language for your function project** (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.): Wählen Sie die Option `C#`.

    + **Wählen Sie eine .NET-Laufzeit aus**: Wählen Sie `.NET 5 isolated` aus.

    + **Select a template for your project's first function** (Wählen Sie die Vorlage für die erste Funktion Ihres Projekts aus.): Wählen Sie die Option `HTTP trigger`.

    + **Provide a function name** (Geben Sie einen Funktionsnamen an.): Geben Sie `HttpExample`ein.

    + **Provide a namespace** (Geben Sie einen Namespace an.): Geben Sie `My.Functions`ein.

    + **Autorisierungsstufe:** Wählen Sie `Anonymous` aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann. Weitere Informationen zur Autorisierungsstufe finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Select how you would like to open your project** (Wählen Sie aus, wie Sie Ihr Projekt öffnen möchten.): Wählen Sie die Option `Add to workspace`.

1. Auf der Grundlage dieser Informationen generiert Visual Studio Code ein Azure Functions-Projekt mit einem HTTP-Trigger. Die lokalen Projektdateien können im Explorer angezeigt werden. Weitere Informationen zu den erstellten Dateien finden Sie unter [Generierte Projektdateien](functions-develop-vs-code.md#generated-project-files).
::: zone-end  
::: zone pivot="development-environment-cli,development-environment-vs"  

1. Führen Sie den Befehl `func init` wie folgt aus, um in einem Ordner mit dem Namen *LocalFunctionProj* ein Funktionsprojekt zu erstellen:  

    ```console
    func init LocalFunctionProj --worker-runtime dotnetisolated
    ```
    
    Durch Angabe von `dotnetisolated` wird ein Projekt erstellt, das unter .NET 5.0 ausgeführt wird.


1. Navigieren Sie zum Projektordner:

    ```console
    cd LocalFunctionProj
    ```

    Dieser Ordner enthält verschiedene Dateien für das Projekt, z. B. die Konfigurationsdateien [local.settings.json](functions-run-local.md#local-settings-file) und [host.json](functions-host-json.md). Da *local.settings.json* aus Azure heruntergeladene Geheimnisse enthalten kann, wird die Datei in der *GITIGNORE*-Datei standardmäßig aus der Quellcodeverwaltung ausgeschlossen.

1. Fügen Sie dem Projekt über den unten gezeigten Befehl eine Funktion hinzu. Hierbei ist das `--name`-Argument der eindeutige Name Ihrer Funktion (HttpExample), mit dem `--template`-Argument wird der Trigger der Funktion (HTTP) angegeben.

    ```console
    func new --name HttpExample --template "HTTP trigger" --authlevel "anonymous"
    ``` 

    `func new` erstellt die Codedatei „HttpExample.cs“.
::: zone-end  

::: zone pivot="development-environment-vscode"  

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end  
::: zone pivot="development-environment-cli" 

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

::: zone-end

::: zone pivot="development-environment-vs"

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

An diesem Punkt können Sie den `func start`-Befehl aus dem Stammverzeichnis des Projektordners ausführen, um das Projekt für isolierte C#-Funktionen zu kompilieren und auszuführen. Wenn Sie den Code Ihrer prozessexternen Funktion in Visual Studio debuggen möchten, müssen Sie derzeit dem laufenden Functions-Laufzeitprozess mit den folgenden Schritten manuell einen Debugger an anfügen:  

1. Öffnen Sie die Projektdatei (.csproj) in Visual Studio. Sie können den Projektcode überprüfen und ändern und alle gewünschten Haltepunkte im Code festlegen. 

1. Verwenden Sie im Stammprojektordner den folgenden Befehl im Terminal oder in einer Eingabeaufforderung, um den Laufzeithost zu starten:

    ```console
    func start --dotnet-isolated-debug
    ```

    Die `--dotnet-isolated-debug`-Option weist den Prozess an, vor dem Fortfahren auf das Anfügen eines Debuggers zu warten. Am Ende der Ausgabe sollten Sie etwa die folgenden Zeilen sehen: 
    
    <pre>
    ...
    
    Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample

    For detailed output, run func with --verbose flag.
    [2021-03-09T08:41:41.904Z] Azure Functions .NET Worker (PID: 81720) initialized in debug mode. Waiting for debugger to attach...
    ...
    
    </pre> 

    `PID: XXXXXX` gibt die Prozess-ID (PID) des dotnet.exe-Prozesses an, der ausführender Functions-Host ist.
 
1. Notieren Sie sich in der Azure Functions-Laufzeitausgabe die Prozess-ID des Hostprozesses, dem Sie einen Debugger anfügen. Beachten Sie auch die URL Ihrer lokalen Funktion.

1. Wählen Sie in Visual Studio im Menü **Debuggen** die Option **An Prozess anhängen...** aus, suchen Sie den Prozess, der mit der Prozess-ID übereinstimmt, und wählen Sie **Anfügen** aus. 
    
    :::image type="content" source="media/dotnet-isolated-process-developer-howtos/attach-to-process.png" alt-text="Anfügen des Debuggers an den Functions-Hostprozess":::    

    Nachdem der Debugger angefügt wurde, können Sie den Funktionscode wie gewohnt debuggen.

1. Geben Sie in die Adressleiste Ihres Browsers Ihre lokale Funktions-URL ein, die wie folgt aussieht, und führen Sie die Anforderung aus. 

    `http://localhost:7071/api/HttpExample`

    Die Ausgabe der Ablaufverfolgung sollte in der in das ausgegebene Terminal geschriebenen Anforderung angezeigt werden. Die Codeausführung wird an allen Breakpoints angehalten, die Sie in Ihrem Funktionscode festlegen.

1. Wenn Sie fertig sind, wechseln Sie zum Terminal, und drücken Sie STRG+C, um den Hostprozess anzuhalten.
 
Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

> [!NOTE]  
> Die Visual Studio-Veröffentlichung ist zurzeit für .NET-Apps für isolierte Prozesse nicht verfügbar. Nachdem Sie die Entwicklung Ihres Projekts in Visual Studio abgeschlossen haben, müssen Sie die Azure CLI zum Erstellen der Azure-Remoteressourcen verwenden. Anschließend können Sie Azure Functions Core Tools wieder in der Befehlszeile verwenden, um Ihr Projekt in Azure zu veröffentlichen.
::: zone-end

::: zone pivot="development-environment-cli,development-environment-vs" 
## <a name="create-supporting-azure-resources-for-your-function"></a>Erstellen von unterstützenden Azure-Ressourcen für Ihre Funktion

Zum Bereitstellen Ihres Funktionscodes in Azure müssen Sie drei Ressourcen erstellen:

- Eine [Ressourcengruppe](../azure-resource-manager/management/overview.md), bei der es sich um einen logischen Container für verwandte Ressourcen handelt.
- Ein [Speicherkonto](../storage/common/storage-account-create.md), das verwendet wird, um den Status und andere Informationen zu Ihren Funktionen zu verwalten.
- Eine Funktions-App, die als Umgebung zum Ausführen Ihres Funktionscodes dient. Eine Funktions-App ist Ihrem lokalen Funktionsprojekt zugeordnet und ermöglicht Ihnen das Gruppieren von Funktionen als logische Einheit, um die Verwaltung, Bereitstellung und Freigabe von Ressourcen zu vereinfachen.

Verwenden Sie die folgenden Befehle, um diese Elemente zu erstellen.

1. Melden Sie sich bei Azure an, falls dies noch nicht geschehen ist:

    ```azurecli
    az login
    ```

    Mit dem Befehl [az login](/cli/azure/reference-index#az_login) werden Sie bei Ihrem Azure-Konto angemeldet.

1. Erstellen Sie eine Ressourcengruppe mit dem Namen `AzureFunctionsQuickstart-rg` in der Region `westeurope`:

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
 
    Mit dem Befehl [az group create](/cli/azure/group#az_group_create) wird eine Ressourcengruppe erstellt. Im Allgemeinen erstellen Sie Ihre Ressourcengruppe und die Ressourcen in einer Region in Ihrer Nähe, indem Sie eine verfügbare Region verwenden, die vom Befehl `az account list-locations` zurückgegeben wird.

1. Erstellen Sie in Ihrer Ressourcengruppe und Region ein universelles Speicherkonto:

    ```azurecli
    az storage account create --name <STORAGE_NAME> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```

    Der Befehl [az storage account create](/cli/azure/storage/account#az_storage_account_create) erstellt ein Speicherkonto. 

    Ersetzen Sie im vorherigen Beispiel `<STORAGE_NAME>` durch einen Namen, der für Sie geeignet und eindeutig in Azure Storage ist. Namen dürfen nur 3 bis 24 Zeichen und ausschließlich Kleinbuchstaben enthalten. Mit `Standard_LRS` wird ein universelles Konto angegeben, das [von Functions unterstützt](storage-considerations.md#storage-account-requirements) wird.

1. Erstellen Sie die Funktions-App in Azure:
   
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location westeurope --runtime dotnet-isolated --runtime-version 5.0 --functions-version 3 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```

    Der Befehl [az functionapp create](/cli/azure/functionapp#az_functionapp_create) erstellt die Funktions-App in Azure. 
    
    Ersetzen Sie im vorherigen Beispiel `<STORAGE_NAME>` durch den Namen des Kontos, das Sie im vorherigen Schritt verwendet haben, und `<APP_NAME>` durch einen global eindeutigen Namen, der für Sie geeignet ist. `<APP_NAME>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. 
    
    Dieser Befehl erstellt eine Funktions-App, die .NET 5.0 unter dem [Azure Functions-Verbrauchsplans](consumption-plan.md) ausführt. Dieser Plan sollte für die Nutzungsmenge, die Sie in diesem Artikel beanspruchen, kostenlos sein. Außerdem stellt der Befehl in derselben Ressourcengruppe eine zugeordnete Azure Application Insights-Instanz bereit. Überwachen Sie mit dieser Instanz Ihre Funktions-App, und zeigen Sie Protokolle an. Weitere Informationen finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md). Für die Instanz fallen erst Kosten an, wenn Sie sie aktivieren.

[!INCLUDE [functions-publish-project-cli](../../includes/functions-publish-project-cli.md)]

::: zone-end

::: zone pivot="development-environment-vscode"

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

In diesem Abschnitt erstellen Sie eine Funktions-App sowie zugehörige Ressourcen in Ihrem Azure-Abonnement und stellen anschließend Ihren Code bereit.

> [!IMPORTANT]
> Beim Veröffentlichen in einer vorhandenen Funktions-App wird der Inhalt dieser App in Azure überschrieben.


1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Schaltfläche **Deploy to function app...** (In Funktions-App bereitstellen...) aus.

    ![Veröffentlichen Ihres Projekts in Azure](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    - **Wählen Sie einen Ordner aus:** Wählen Sie einen Ordner in Ihrem Arbeitsbereich aus, oder navigieren Sie zu einem Ordner, der Ihre Funktions-App enthält. Diese Eingabeaufforderung wird nicht angezeigt, wenn Sie bereits eine gültige Funktions-App geöffnet haben.

    - **Wählen Sie das Abonnement aus:** Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur über ein Abonnement verfügen, wird diese Eingabeaufforderung nicht angezeigt.

    - **Select Function App in Azure:** (Wählen Sie die Funktions-App in Azure aus:) Wählen Sie die Option `- Create new Function App`. (Wählen Sie nicht die Option `Advanced` aus, diese wird im vorliegenden Artikel nicht behandelt.)
      
    - **Enter a globally unique name for the function app:** (Geben Sie einen global eindeutigen Namen für die Funktions-App ein:) Geben Sie einen Namen ein, der in einem URL-Pfad gültig ist. Der eingegebene Name wird überprüft, um sicherzustellen, dass er in Azure Functions eindeutig ist.
    
    - **Wählen Sie einen Runtimestapel aus**: Wählen Sie die Option `.NET 5 (non-LTS)`. 
    
    - **Wählen Sie einen Speicherort für neue Ressourcen aus:**  Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus, um eine bessere Leistung zu erzielen. 
    
    Im Benachrichtigungsbereich sehen Sie den Status einzelner Ressourcen, die in Azure erstellt werden.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Benachrichtigung über die Erstellung von Azure-Ressourcen":::
    
1.  Nach Abschluss des Vorgangs werden in Ihrem Abonnement die folgenden Azure-Ressourcen erstellt, deren Namen auf dem Namen Ihrer Funktions-App basieren:
    
    [!INCLUDE [functions-vs-code-created-resources](../../includes/functions-vs-code-created-resources.md)]

    Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. 

    [!INCLUDE [functions-vs-code-create-tip](../../includes/functions-vs-code-create-tip.md)]

4. Wählen Sie in dieser Benachrichtigungen **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen erstellten Azure-Ressourcen) anzuzeigen. Wenn Sie die Benachrichtigung übersehen haben, wählen Sie das Glockensymbol in der unteren rechten Ecke aus, um sie erneut anzuzeigen.

    ![Benachrichtigung nach Abschluss der Erstellung](../../includes/media/functions-publish-project-vscode/function-create-notifications.png)

::: zone-end

::: zone pivot="development-environment-cli"  
[!INCLUDE [functions-run-remote-azure-cli](../../includes/functions-run-remote-azure-cli.md)]  
::: zone-end  

::: zone pivot="development-environment-vscode"  
[!INCLUDE [functions-vs-code-run-remote](../../includes/functions-vs-code-run-remote.md)]  
::: zone-end  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie haben Ressourcen erstellt, um diesen Artikel abzuschließen. Für diese Ressourcen fallen je nach [Kontostatus](https://azure.microsoft.com/account/) und [Dienstpreisen](https://azure.microsoft.com/pricing/) unter Umständen Kosten an. 

::: zone pivot="development-environment-cli"  
Löschen Sie mit dem unten angegebenen Befehl die Ressourcengruppe und alle darin enthaltenen Ressourcen, damit keine weiteren Kosten anfallen.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```
::: zone-end  

::: zone pivot="development-environment-vscode"  
Löschen Sie mit den folgenden Schritten die Funktions-App und die zugehörigen Ressourcen, um weitere Kosten zu vermeiden.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]  
::: zone-end  
::: zone pivot="development-environment-vs"   
Löschen Sie mit den folgenden Schritten die Funktions-App und die zugehörigen Ressourcen, um weitere Kosten zu vermeiden.

1. Erweitern Sie im Cloud-Explorer Ihr Abonnement > **App Services**, klicken Sie mit der rechten Maustaste auf ihre Funktions-App, und wählen Sie **Im Portal öffnen** aus. 

1. Wählen Sie auf der Seite „Funktions-App“ die Registerkarte **Übersicht** und anschließend unter **Ressourcengruppe** den Link aus.

   :::image type="content" source="media/functions-create-your-first-function-visual-studio/functions-app-delete-resource-group.png" alt-text="Auswählen der zu löschenden Ressourcengruppe auf der Seite „Funktions-App“":::

2. Prüfen Sie auf der Seite **Ressourcengruppe** die Liste mit den enthaltenen Ressourcen, und vergewissern Sie sich, dass es sich dabei um die Ressourcen handelt, die Sie löschen möchten.
 
3. Klicken Sie auf **Ressourcengruppe löschen**, und folgen Sie den Anweisungen.

   Der Löschvorgang kann einige Minuten dauern. Nach Abschluss des Vorgangs wird kurz eine Benachrichtigung angezeigt. Sie können auch am oberen Seitenrand auf das Glockensymbol klicken, um die Benachrichtigung anzuzeigen.
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu isolierten .NET-Funktionen](dotnet-isolated-process-guide.md)

