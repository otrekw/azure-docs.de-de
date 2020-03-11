---
title: Verbinden von Azure Functions mit Azure Storage über Befehlszeilentools
description: Es wird beschrieben, wie Sie Azure Functions mit einer Azure Storage-Warteschlange verbinden, indem Sie Ihrem Befehlszeilenprojekt eine Ausgabebindung hinzufügen.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: e3c37b368b723cc95302949baa8e85e2a8b621be
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78201001"
---
# <a name="connect-azure-functions-to-azure-storage-using-command-line-tools"></a>Verbinden von Azure Functions mit Azure Storage über Befehlszeilentools

In diesem Artikel integrieren Sie eine Azure Storage-Warteschlange in die Funktion und das Speicherkonto, die bzw. das Sie in der [vorherigen Schnellstartanleitung](functions-create-first-azure-function-azure-cli.md) erstellt haben. Sie erzielen diese Integration mit einer *Ausgabebindung*, mit der Daten aus einer HTTP-Anforderung in eine Nachricht der Warteschlange geschrieben werden. Beim Durcharbeiten dieses Artikels fallen keine zusätzlichen Kosten an, die über die wenigen US-Cent aus der vorherigen Schnellstartanleitung hinausgehen. Weitere Informationen zu Bindungen finden Sie unter [Azure Functions-Trigger und Bindungskonzepte](functions-triggers-bindings.md).

## <a name="configure-your-local-environment"></a>Konfigurieren Ihrer lokalen Umgebung

Zunächst müssen Sie den Artikel [Schnellstart: Erstellen Ihrer ersten Funktion über die Befehlszeile mithilfe der Azure-Befehlszeilenschnittstelle](functions-create-first-azure-function-azure-cli.md) durcharbeiten. Falls Sie die Ressourcen am Ende dieses Artikels bereits bereinigt haben, sollten Sie die Schritte erneut ausführen, um die Funktions-App und die zugehörigen Ressourcen neu zu erstellen.

## <a name="retrieve-the-azure-storage-connection-string"></a>Abrufen der Azure Storage-Verbindungszeichenfolge

Als Sie im Rahmen der vorherigen Schnellstartanleitung eine Funktions-App in Azure erstellt haben, haben Sie auch ein Storage-Konto erstellt. Die Verbindungszeichenfolge für dieses Konto wird sicher in App-Einstellungen in Azure gespeichert. Indem Sie die Einstellung in die Datei *local.settings.json* herunterladen, können Sie diese Verbindung zum Schreiben in eine Storage-Warteschlange unter demselben Konto verwenden, wenn Sie die Funktion lokal ausführen. 

1. Führen Sie im Stammverzeichnis des Projekts den folgenden Befehl aus, indem Sie `<APP_NAME>` durch den Namen Ihrer Funktions-App aus der vorherigen Schnellstartanleitung ersetzen. Mit diesem Befehl werden alle vorhandenen Werte in der Datei überschrieben.

    ```
    func azure functionapp fetch-app-settings <APP_NAME>
    ```
    
1. Öffnen Sie *local.settings.json*, und suchen Sie nach dem Wert mit dem Namen `AzureWebJobsStorage`. Dies ist die Verbindungszeichenfolge des Storage-Kontos. Sie verwenden den Namen `AzureWebJobsStorage` und die Verbindungszeichenfolge noch in anderen Abschnitten dieses Artikels.

> [!IMPORTANT]
> Da *local.settings.json* aus Azure heruntergeladene Geheimnisse enthält, sollten Sie diese Datei immer aus der Quellcodeverwaltung ausschließen. In der *GITIGNORE*-Datei, die für ein lokales Funktionsprojekt erstellt wird, ist die Datei standardmäßig ausgeschlossen.

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

## <a name="add-an-output-binding-definition-to-the-function"></a>Hinzufügen einer Ausgabebindungsdefinition zur Funktion

Eine Funktion kann zwar nur über einen Trigger verfügen, aber über mehrere Eingabe- und Ausgabebindungen. Hiermit können Sie eine Verbindung mit anderen Azure-Diensten und -Ressourcen herstellen, ohne benutzerdefinierten Integrationscode zu schreiben. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Sie deklarieren diese Bindungen in der Datei *function.json* in Ihrem Funktionsordner. In der vorherigen Schnellstartanleitung hat die Datei *function.json* im Ordner *HttpExample* zwei Bindungen in der Sammlung `bindings` enthalten:  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-python"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-powershell"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json" range="2-18":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript, programming-language-powershell, programming-language-typescript"  
Jede Bindung verfügt mindestens über einen Typ, eine Richtung und einen Namen. Im obigen Beispiel hat die erste Bindung den Typ `httpTrigger` und die Richtung `in`. Für die Richtung `in` wird mit `name` der Name eines Eingabeparameters angegeben, der an die Funktion gesendet wird, wenn sie vom Trigger aufgerufen wird.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Die zweite Bindung in der Sammlung hat den Namen `res`. Diese `http`-Bindung ist eine Ausgabebindung (`out`), die zum Schreiben der HTTP-Antwort verwendet wird. 

Fügen Sie zum Schreiben in eine Azure Storage-Warteschlange aus dieser Funktion eine `out`-Bindung vom Typ `queue` mit dem Namen `msg` hinzu. Dies ist im Code unten veranschaulicht:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
Die zweite Bindung der Sammlung hat den Typ `http` mit der Richtung `out`. In diesem Fall wird mit dem speziellen `name`-Element von `$return` angegeben, dass für diese Bindung der Rückgabewert der Funktion verwendet wird, anstatt einen Eingabeparameter bereitzustellen.

Fügen Sie zum Schreiben in eine Azure Storage-Warteschlange aus dieser Funktion eine `out`-Bindung vom Typ `queue` mit dem Namen `msg` hinzu. Dies ist im Code unten veranschaulicht:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
Die zweite Bindung in der Sammlung hat den Namen `res`. Diese `http`-Bindung ist eine Ausgabebindung (`out`), die zum Schreiben der HTTP-Antwort verwendet wird. 

Fügen Sie zum Schreiben in eine Azure Storage-Warteschlange aus dieser Funktion eine `out`-Bindung vom Typ `queue` mit dem Namen `msg` hinzu. Dies ist im Code unten veranschaulicht:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
In diesem Fall wird `msg` als Ausgabeargument für die Funktion bereitgestellt. Für einen `queue`-Typ müssen Sie auch den Namen der Warteschlange in `queueName` und den Namen (*name*) der Azure Storage-Verbindung (aus *local.settings.json*) in `connection` angeben. 
::: zone-end  

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  

Weitere Informationen zu den Details von Bindungen finden Sie unter [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md) und [Ausgabe: Konfiguration](functions-bindings-storage-queue-output.md#configuration).

## <a name="add-code-to-use-the-output-binding"></a>Hinzufügen von Code für die Verwendung der Ausgabebindung

Nachdem die Ausgabebindung in *function.json* angegeben wurde, können Sie Ihre Funktion nun so aktualisieren, dass sie den Ausgabeparameter `msg` empfängt und Nachrichten in die Warteschlange geschrieben werden.

::: zone pivot="programming-language-python"     
[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]
::: zone-end  

::: zone pivot="programming-language-javascript"  
[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]
::: zone-end  

::: zone pivot="programming-language-typescript"  
[!INCLUDE [functions-add-output-binding-ts](../../includes/functions-add-output-binding-ts.md)]
::: zone-end  

::: zone pivot="programming-language-powershell"  
[!INCLUDE [functions-add-output-binding-powershell](../../includes/functions-add-output-binding-powershell.md)]  
::: zone-end

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]
::: zone-end 

Beachten Sie, dass Sie den Code für das Durchführen der Authentifizierung, das Abrufen eines Warteschlangenverweises oder das Schreiben von Daten *nicht* erstellen müssen. Diese Integrationsaufgaben werden allesamt bequem über die Azure Functions-Runtime und die Warteschlangen-Ausgabebindung verarbeitet.

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

[!INCLUDE [functions-extension-bundles-info](../../includes/functions-extension-bundles-info.md)]

## <a name="view-the-message-in-the-azure-storage-queue"></a>Anzeigen der Nachricht in der Azure Storage-Warteschlange

Sie können die Warteschlange im [Azure-Portal](../storage/queues/storage-quickstart-queues-portal.md) oder im [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) anzeigen. Sie haben auch die Möglichkeit, die Warteschlange per Azure CLI anzuzeigen. Dies ist in den folgenden Schritten beschrieben:

1. Öffnen Sie die Datei *local.setting.json* des Funktionsprojekts, und kopieren Sie den Wert der Verbindungszeichenfolge. Führen Sie in einem Terminal oder Befehlsfenster den folgenden Befehl aus, um eine Umgebungsvariable mit dem Namen `AZURE_STORAGE_CONNECTION_STRING` zu erstellen, und fügen Sie anstelle von `<MY_CONNECTION_STRING>` Ihre spezifische Verbindungszeichenfolge ein. (Die Verwendung dieser Umgebungsvariablen bewirkt, dass Sie die Verbindungszeichenfolge nicht für jeden weiteren Befehl per `--connection-string`-Argument angeben müssen.)

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<MY_CONNECTION_STRING>"
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    ---
    
1. (Optional) Verwenden Sie den Befehl [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list), um die Storage-Warteschlangen in Ihrem Konto anzuzeigen. Die Ausgabe dieses Befehls sollte eine Warteschlange mit dem Namen `outqueue` enthalten. Sie wurde erstellt, als die Funktion ihre erste Nachricht in diese Warteschlange geschrieben hat.
    
    ```azure-cli
    az storage queue list --output tsv
    ```

1. Verwenden Sie den Befehl [`az storage message get`](/cli/azure/storage/message#az-storage-message-get), um die Nachricht aus dieser Warteschlange zu lesen. Dies sollte der erste Name sein, den Sie beim zuvor durchgeführten Testen der Funktion genutzt haben. Mit dem Befehl wird die erste Nachricht der Warteschlange gelesen und daraus entfernt. 

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    az storage message get --queue-name outqueue -o tsv --query [].{Message:content} > %TEMP%out.b64 && certutil -decode -f %TEMP%out.b64 %TEMP%out.txt > NUL && type %TEMP%out.txt && del %TEMP%out.b64 %TEMP%out.txt /q
    ```

    In diesem Skript wird certutil verwendet, um die Base64-codierte Nachrichtensammlung aus einer lokalen temporären Datei zu decodieren. Versuchen Sie bei einer fehlenden Ausgabe, `> NUL` aus dem Skript zu entfernen, um die Unterdrückung der certutil-Ausgabe zu beenden, falls ein Fehler auftritt. 
    
    ---
    
    Da der Nachrichtentext im [Base64-codierten](functions-bindings-storage-queue-trigger.md#encoding) Format gespeichert ist, muss die Nachricht vor dem Anzeigen decodiert werden. Nachdem Sie `az storage message get` ausgeführt haben, wird die Nachricht aus der Warteschlange entfernt. Falls in `outqueue` nur eine Nachricht enthalten war, rufen Sie keine Nachricht ab, wenn Sie diesen Befehl zum zweiten Mal ausführen, sondern erhalten einen Fehler.

## <a name="redeploy-the-project-to-azure"></a>Erneutes Bereitstellen des Projekts in Azure

Nachdem Sie nun lokal sichergestellt haben, dass von der Funktion eine Nachricht in die Azure Storage-Warteschlange geschrieben wurde, können Sie Ihr Projekt erneut bereitstellen, um den in Azure ausgeführten Endpunkt zu aktualisieren.

1. Verwenden Sie im Ordner *LocalFunctionsProj* den Befehl [`func azure functionapp publish`](functions-run-local.md#project-file-deployment), um das Projekt erneut bereitzustellen, indem Sie `<APP_NAME>` durch den Namen Ihrer App ersetzen.

    ```
    func azure functionapp publish <APP_NAME>
    ```
    
1. Verwenden Sie wie in der vorherigen Schnellstartanleitung einen Browser oder curl, um die erneut bereitgestellte Funktion zu testen.

    # <a name="browser"></a>[Browser](#tab/browser)
    
    Kopieren Sie die vollständige **Aufruf-URL**, die in der Ausgabe des Befehls zum Veröffentlichen (publish) angezeigt wird, in eine Browseradressleiste, und fügen Sie den Abfrageparameter `&name=Functions` an. Im Browser sollte eine ähnliche Ausgabe wie bei der lokalen Ausführung der Funktion angezeigt werden.

    ![Ausgabe der in Azure ausgeführten Funktion in einem Browser](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-browser.png)

    # <a name="curl"></a>[curl](#tab/curl)
    
    Führen Sie [`curl`](https://curl.haxx.se/) mit der **Aufruf-URL** aus, und fügen Sie den Parameter `&name=Functions` an. Die Ausgabe des Befehls sollte der Text „Hello Functions“ sein.
    
    ![Ausgabe der in Azure per CURL ausgeführten Funktion](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-curl.png)

    --- 

1. Untersuchen Sie die Storage-Warteschlange erneut (wie im vorherigen Abschnitt beschrieben), um zu überprüfen, ob sie die neu geschriebene Nachricht enthält.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie nach Abschluss des Vorgangs den unten angegebenen Befehl, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, damit keine weiteren Kosten anfallen.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihre mittels HTTP ausgelöste Funktion so aktualisiert, dass sie Daten in eine Speicherwarteschlange schreibt. Nun können Sie sich weiter über die Entwicklung von Functions über die Befehlszeile mit Core Tools und der Azure CLI informieren:

+ [Arbeiten mit Azure Functions Core Tools](functions-run-local.md)  

::: zone pivot="programming-language-csharp"  
+ [Beispiele für vollständige Functions-Projekte in C#](/samples/browse/?products=azure-functions&languages=csharp).

+ [C#-Entwicklerreferenz zu Azure Functions](functions-dotnet-class-library.md)  
::: zone-end 
::: zone pivot="programming-language-javascript"  
+ [Beispiele für vollständige Functions-Projekte in JavaScript](/samples/browse/?products=azure-functions&languages=javascript).

+ [JavaScript-Entwicklerhandbuch für Azure Functions](functions-reference-node.md)  
::: zone-end  
::: zone pivot="programming-language-typescript"  
+ [Beispiele für vollständige Functions-Projekte in TypeScript](/samples/browse/?products=azure-functions&languages=typescript).

+ [TypeScript-Entwicklerhandbuch für Azure Functions](functions-reference-node.md#typescript)  
::: zone-end  
::: zone pivot="programming-language-python"  
+ [Beispiele für vollständige Functions-Projekte in Python](/samples/browse/?products=azure-functions&languages=python).

+ [Python-Entwicklerhandbuch für Azure Functions](functions-reference-python.md)  
::: zone-end  
::: zone pivot="programming-language-powershell"  
+ [Beispiele für vollständige Functions-Projekte in PowerShell](/samples/browse/?products=azure-functions&languages=azurepowershell).

+ [PowerShell-Entwicklerhandbuch für Azure Functions](functions-reference-powershell.md) 
::: zone-end
+ [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

+ [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/)

+ [Abschätzen der Kosten des Verbrauchstarifs](functions-consumption-costs.md) 
