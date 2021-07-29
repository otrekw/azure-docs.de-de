---
title: Entwickeln und Veröffentlichen von .NET 5-Funktionen mit Azure Functions
description: Erfahren Sie, wie Sie C#-Funktionen mithilfe von .NET 5.0 erstellen und debuggen und dann das lokale Projekt in Azure Functions für serverloses Hosting bereitstellen.
ms.date: 05/03/2021
ms.topic: how-to
recommendations: false
zone_pivot_groups: development-environment-functions
ms.openlocfilehash: 6521c02686da55142d9a9d9f1faf569d584ef593
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111589992"
---
# <a name="develop-and-publish-net-5-functions-using-azure-functions"></a>Entwickeln und Veröffentlichen von .NET 5-Funktionen mit Azure Functions 

In diesem Artikel erfahren Sie, wie Sie unter Verwendung von .NET 5.0 mit C#-Funktionen arbeiten, die prozessextern außerhalb der Azure Functions-Laufzeit ausgeführt werden. Sie erfahren, wie Sie diese .NET-Funktionen für einen isolierten Prozess in Azure erstellen, lokal debuggen und veröffentlichen. In Azure werden diese Funktionen in einem isolierten Prozess ausgeführt, der .NET 5.0 unterstützt. Weitere Informationen finden Sie im [Handbuch zum Ausführen von Funktionen unter .NET 5.0 in Azure](dotnet-isolated-process-guide.md).

Wenn Sie .NET 5.0 nicht unterstützen müssen oder Ihre Funktionen prozessextern ausführen, können Sie stattdessen [eine C#-Klassenbibliotheksfunktion erstellen](functions-create-your-first-function-visual-studio.md).

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="development-environment-vscode,development-environment-cli"  
+ [.NET 5.0 SDK](https://dotnet.microsoft.com/download)

+ [Azure Functions Core Tools](functions-run-local.md#v2) ab Version 3.0.3381.

+ [Azure CLI](/cli/azure/install-azure-cli) ab Version 2.20.  
::: zone-end  
::: zone pivot="development-environment-vscode"  
+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)  

+ [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code  

+ Die [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code ab Version 1.3.0.
::: zone-end  
::: zone pivot="development-environment-vs"  
+ [Visual Studio 2019](https://azure.microsoft.com/downloads/) Version 16.10 oder höher. Ihre Installation muss entweder die Workload **Azure-Entwicklung** oder die Workload **ASP.NET und Webentwicklung** enthalten.  
::: zone-end  

## <a name="create-a-local-function-project"></a>Erstellen eines lokalen Funktionsprojekts

In Azure Functions handelt es sich bei einem Funktionsprojekt um einen Container für eine oder mehrere individuelle Funktionen, die jeweils auf einen bestimmten Trigger reagieren. Für alle Funktionen eines Projekts werden die gleichen lokalen Konfigurationen und Hostkonfigurationen gemeinsam genutzt. In diesem Abschnitt erstellen Sie ein Funktionsprojekt, das nur eine Funktion enthält.

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
::: zone pivot="development-environment-cli"  

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

::: zone pivot="development-environment-vs"

1. Wählen Sie im Visual Studio-Menü **Datei** > **Neu** > **Projekt** aus.

1. Geben Sie unter **Neues Projekt erstellen** den Suchbegriff *functions* in das Suchfeld ein, und wählen Sie die Vorlage **Azure Functions** und anschließend **Weiter** aus.

1. Geben Sie unter **Neues Projekt konfigurieren** im Feld **Projektname** einen Namen für Ihr Projekt ein, und wählen Sie anschließend **Erstellen** aus. Der Name der Funktions-App muss als C#-Namespace gültig sein, verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.

1. Verwenden Sie für die Einstellungen zum **Erstellen einer neuen Azure Functions-Anwendung** die Werte in der folgenden Tabelle:

    | Einstellung      | Wert  | Beschreibung                      |
    | ------------ |  ------- |----------------------------------------- |
    | **.NET-Version** | **.NET 5 (isoliert)** | Dieser Wert erstellt ein Funktionsprojekt, das in .NET 5.0 in einem isolierten Prozess ausgeführt wird.   |
    | **Funktionsvorlage** | **HTTP-Trigger** | Mit diesem Wert wird eine Funktion erstellt, die durch eine HTTP-Anforderung ausgelöst wird. |
    | **Speicherkonto (AzureWebJobsStorage)**  | **Speicheremulator** | Da für eine Funktions-App in Azure ein Speicherkonto erforderlich ist, wird ein Speicherkonto zugewiesen oder erstellt, wenn Sie Ihr Projekt in Azure veröffentlichen. Von HTTP-Triggern wird keine Azure Storage-Kontoverbindungszeichenfolge verwendet. Für alle anderen Triggertypen ist dagegen eine gültige Azure Storage-Kontoverbindungszeichenfolge erforderlich.  |
    | **Autorisierungsstufe** | **Anonym** | Die erstellte Funktion kann von jedem Client ausgelöst werden, ohne dass ein Schlüssel angegeben werden muss. Diese Autorisierungseinstellung erleichtert Ihnen das Testen Ihrer neuen Funktion. Weitere Informationen zu Schlüsseln und zur Autorisierung finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys) sowie in der [Übersicht über HTTP-Trigger und -Bindungen in Azure Functions](functions-bindings-http-webhook.md). |
    
    
    ![Azure Functions-Projekteinstellungen](./media/dotnet-isolated-process-developer-howtos/functions-project-settings.png)

    Stellen Sie sicher, dass **Autorisierungsstufe** auf **Anonym** festgelegt ist. Bei Verwendung der Standardebene **Funktion** muss in Anforderungen für den Zugriff auf den Funktionsendpunkt der [Funktionsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys) angegeben werden.

1. Wählen Sie **Erstellen** aus, um das Funktionsprojekt und die HTTP-Triggerfunktion zu erstellen.

Visual Studio erstellt ein Projekt und eine Klasse mit Codebausteinen für den Funktionstyp „HTTP-Trigger“. Der Codebaustein sendet ein „Willkommen bei Azure Functions!“ HTTP-Antwort. Mit dem Attribut `HttpTrigger` wird angegeben, dass die Funktion von einer HTTP-Anforderung ausgelöst wird. 

## <a name="rename-the-function"></a>Umbenennen der Funktion

Das Methodenattribut `FunctionName` dient zum Festlegen des Namens der Funktion, der standardmäßig als `Function1` generiert wird. Da die Tools Sie den Standardfunktionsnamen nicht überschreiben lassen, wenn Sie Ihr Projekt erstellen, nehmen Sie sich eine Minute Zeit, um einen besseren Namen für die Funktionsklasse, Datei und die Metadaten zu erstellen.

1. Klicken Sie im **Datei-Explorer** mit der rechten Maustaste auf die Datei „Function1.cs“, und benennen Sie sie in `HttpExample.cs` um.

1. Benennen Sie die Klasse „Function1“ im Code in `HttpExample` um.

1. Benennen Sie in der Methode `HttpTrigger` mit dem Namen `Run` das Methodenattribut `FunctionName` in `HttpExample` und den Wert um, der an die Methode `GetLogger` übergeben wird.

Die Definition der Funktion sollte nun wie folgt aussehen:

:::code language="csharp" source="~/functions-docs-csharp/http-trigger-isolated/HttpExample.cs" range="9-15":::
 
Nachdem Sie die Funktion nun umbenannt haben, können Sie sie auf Ihrem lokalen Computer testen.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Visual Studio wird in Azure Functions Core Tools integriert, sodass Sie Ihre Funktionen mithilfe der vollständigen Azure Functions-Runtime lokal testen können.  

1. Drücken Sie in Visual Studio die Taste <kbd>F5</kbd>, um Ihre Funktion auszuführen. Sie müssen unter Umständen eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können. Autorisierungsebenen werden niemals erzwungen, wenn Sie eine Funktion lokal ausführen.

1. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe, und führen Sie die Anforderung aus. Eine Willkommensmeldung zu Functions wird angezeigt, wenn die Funktion erfolgreich ausgeführt wird und Protokolle in die Laufzeitausgabe geschrieben werden. 

1. Drücken Sie in Visual Studio <kbd>UMSCHALTTASTE</kbd>+<kbd>F5</kbd>, um das Debuggen zu beenden.

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

::: zone-end

::: zone pivot="development-environment-vscode"  

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end  
::: zone pivot="development-environment-cli" 

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

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

:::zone pivot="development-environment-vs"

## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

Sie müssen in Ihrem Azure-Abonnement über eine Funktions-App verfügen, um Ihr Projekt veröffentlichen zu können. Bei der Veröffentlichung in Visual Studio wird eine Funktions-App für Sie erstellt, wenn Sie das Projekt zum ersten Mal veröffentlichen.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="verify-your-function-in-azure"></a>Überprüfen Ihrer Funktion in Azure

1. In Cloud-Explorer sollte Ihre neue Funktions-App ausgewählt sein. Wenn nicht, erweitern Sie Ihr Abonnement > **App Services**, und wählen Sie Ihre neue Funktions-App aus.

1. Klicken Sie mit der rechten Maustaste auf die Funktions-App, und wählen Sie **In Browser öffnen** aus. Dadurch wird der Stamm ihrer Funktions-App in Ihrem Standardwebbrowser geöffnet und die Seite angezeigt, die anzeigt, dass ihre Funktions-App ausgeführt wird. 

    :::image type="content" source="media/functions-create-your-first-function-visual-studio/function-app-running-azure.png" alt-text="Ausgeführte Funktions-App":::

1. Fügen Sie in der Adressleiste des Browsers den Pfad `/api/HttpExample` an die Basis-URL an, und führen Sie die Anforderung aus.

1. Wechseln Sie zu dieser URL, und im Browser wird Ihnen dieselbe Antwort angezeigt wie bei der lokalen Ausführung.

:::zone-end

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
[!INCLUDE [functions-vstools-cleanup](../../includes/functions-vstools-cleanup.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zu isolierten .NET-Funktionen](dotnet-isolated-process-guide.md)

