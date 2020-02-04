---
title: Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Python-Funktion
description: Es wird beschrieben, wie Sie eine Azure Storage-Warteschlange per Ausgabebindung in eine Python-Funktion integrieren.
ms.date: 01/15/2020
ms.topic: quickstart
ms.openlocfilehash: 14a381d13da052fd67679ed17bbb6b6711f7a0e6
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715374"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Python-Funktion

In diesem Artikel integrieren Sie eine Azure Storage-Warteschlange in die Funktion und das Speicherkonto, die bzw. das Sie unter [Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure](functions-create-first-function-python.md) erstellt haben. Sie erzielen diese Integration mit einer *Ausgabebindung*, mit der Daten aus einer HTTP-Anforderung in eine Nachricht der Warteschlange geschrieben werden. Beim Durcharbeiten dieses Artikels fallen keine zusätzlichen Kosten an, die über die wenigen US-Cent aus der vorherigen Schnellstartanleitung hinausgehen.

## <a name="prerequisites"></a>Voraussetzungen

- Arbeiten Sie die Schnellstartanleitung zum [Erstellen einer durch HTTP ausgelösten Python-Funktion](functions-create-first-function-python.md) durch. Falls Sie die Ressourcen am Ende dieses Artikels bereits bereinigt haben, sollten Sie die Schritte erneut ausführen, um die Funktions-App in Azure neu zu erstellen. Behalten Sie die Ressourcen am Ende dann bei.

## <a name="retrieve-the-azure-storage-connection-string"></a>Abrufen der Azure Storage-Verbindungszeichenfolge

Als Sie im Rahmen der vorherigen Schnellstartanleitung eine Funktions-App in Azure erstellt haben, haben Sie auch ein Storage-Konto erstellt. Die Verbindungszeichenfolge für dieses Konto wird sicher in App-Einstellungen in Azure gespeichert. Indem Sie die Einstellung in die Datei *local.settings.json* herunterladen, können Sie diese Verbindung zum Schreiben in eine Storage-Warteschlange unter demselben Konto verwenden, wenn Sie die Funktion lokal ausführen. 

1. Führen Sie im Stammverzeichnis des Projekts den folgenden Befehl aus, indem Sie `<app_name>` durch den Namen Ihrer Funktions-App aus der vorherigen Schnellstartanleitung ersetzen. Mit diesem Befehl werden alle vorhandenen Werte in der Datei überschrieben.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. Öffnen Sie *local.settings.json*, und suchen Sie nach dem Wert mit dem Namen `AzureWebJobsStorage`. Dies ist die Verbindungszeichenfolge des Storage-Kontos. Sie verwenden den Namen `AzureWebJobsStorage` und die Verbindungszeichenfolge noch in anderen Abschnitten dieses Artikels.

> [!IMPORTANT]
> Da *local.settings.json* aus Azure heruntergeladene Geheimnisse enthält, sollten Sie diese Datei immer aus der Quellcodeverwaltung ausschließen. In der *GITIGNORE*-Datei, die für ein lokales Funktionsprojekt erstellt wird, ist die Datei standardmäßig ausgeschlossen.

## <a name="add-an-output-binding-to-functionjson"></a>Hinzufügen einer Ausgabebindung zu „function.json“

Eine Funktion kann zwar nur über einen Trigger verfügen, aber über mehrere Eingabe- und Ausgabebindungen. Hiermit können Sie eine Verbindung mit anderen Azure-Diensten und -Ressourcen herstellen, ohne benutzerdefinierten Integrationscode zu schreiben. Sie deklarieren diese Bindungen in der Datei *function.json* in Ihrem Funktionsordner. Dies richtet sich nach der Sprache, die Sie für die Funktion verwenden.

In der vorherigen Schnellstartanleitung hat die Datei *function.json* im Ordner *HttpExample* zwei Bindungen in der Sammlung `bindings` enthalten:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Jede Bindung verfügt mindestens über einen Typ, eine Richtung und einen Namen. Im obigen Beispiel hat die erste Bindung den Typ `httpTrigger` und die Richtung `in`. Für die Richtung `in` wird mit `name` der Name eines Eingabeparameters angegeben, der an die Funktion gesendet wird, wenn sie vom Trigger aufgerufen wird.

Die zweite Bindung hat den Typ `http` mit der Richtung `out`. In diesem Fall wird mit dem speziellen `name`-Element von `$return` angegeben, dass für diese Bindung der Rückgabewert der Funktion verwendet wird, anstatt einen Eingabeparameter bereitzustellen.

Fügen Sie zum Schreiben in eine Azure Storage-Warteschlange aus dieser Funktion eine `out`-Bindung vom Typ `queue` mit dem Namen `msg` hinzu. Dies ist im Code unten veranschaulicht:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

In diesem Fall wird `msg` als Ausgabeargument für die Funktion bereitgestellt. Für einen `queue`-Typ müssen Sie auch den Namen der Warteschlange in `queueName` und den Namen (*name*) der Azure Storage-Verbindung (aus *local.settings.json*) in `connection` angeben.

Weitere Informationen zu den Details von Bindungen finden Sie unter [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md) und [Ausgabe: Konfiguration](functions-bindings-storage-queue.md#output---configuration).

## <a name="add-code-to-use-the-output-binding"></a>Hinzufügen von Code für die Verwendung der Ausgabebindung

Nachdem die Ausgabebindung in *function.json* angegeben wurde, können Sie Ihre Funktion nun so aktualisieren, dass sie den Ausgabeparameter `msg` empfängt und Nachrichten in die Warteschlange geschrieben werden.

Aktualisieren Sie *HttpExample\\\_\_init\_\_.py* gemäß dem folgenden Code, und fügen Sie den Parameter `msg` der Funktionsdefinition und `msg.set(name)` unter der `if name:`-Anweisung hinzu.

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> str:

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        msg.set(name)
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

Der Parameter `msg` ist eine Instanz der [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest). Die zugehörige `set`-Methode schreibt eine Zeichenfolgennachricht in die Warteschlange. In diesem Fall ist dies der Name, der in der URL-Abfragezeichenfolge an die Funktion übergeben wird.

Beachten Sie, dass Sie den Code für das Durchführen der Authentifizierung, das Abrufen eines Warteschlangenverweises oder das Schreiben von Daten *nicht* erstellen müssen. Diese Integrationsaufgaben werden allesamt bequem über die Azure Functions-Runtime und die Warteschlangen-Ausgabebindung verarbeitet.

## <a name="run-and-test-the-function-locally"></a>Lokales Ausführen und Testen der Funktion

1. Navigieren Sie in einem Terminal oder über die Eingabeaufforderung zu Ihrem Funktionsprojektordner *LocalFunctionProj*.

1. Starten Sie den Host der Azure Functions-Runtime mit dem folgenden Befehl.

    ```
    func host start
    ```

1. Nachdem der Startvorgang abgeschlossen ist und die URL für den `HttpExample`-Endpunkt angezeigt wird, können Sie die zugehörige URL in einem Browser kopieren und die Abfragezeichenfolge `?name=<your-name>` anfügen, um diese vollständige URL zu erhalten: `http://localhost:7071/api/HttpExample?name=Guido`. Im Browser sollte wie im vorherigen Artikel eine Meldung wie `Hello Guido` angezeigt werden.

    Gehen Sie wie folgt vor, falls der `HttpExample`-Endpunkt nicht angezeigt wird: Beenden Sie den Host mit **STRG**+**C**, und untersuchen Sie die Ausgabe auf Fehler. Der Endpunkt wird vom Host beispielsweise nicht aktiviert, wenn *function.json* einen Fehler enthält. Vergewissern Sie sich auch, dass Sie `func host start` aus dem Funktionsprojektordner ausführen, und nicht aus dem Ordner *HttpExample*.

1. Beenden Sie den Host nach Abschluss des Vorgangs mit **STRG**+**C**.

> [!TIP]
> Beim Starten führt der Host den Download und die Installation der [Storage-Bindungserweiterung](functions-bindings-storage-blob.md#packages---functions-2x-and-higher) und anderer Microsoft-Bindungserweiterungen durch. Diese Installation erfolgt, weil Bindungserweiterungen in der Datei *host.json* mit den folgenden Eigenschaften standardmäßig aktiviert sind:
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> Falls bei Ihnen Fehler in Bezug auf Bindungserweiterungen auftreten, sollten Sie sich vergewissern, dass die obigen Eigenschaften in *host.json* enthalten sind.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Anzeigen der Nachricht in der Azure Storage-Warteschlange

Wenn Ihre Funktion eine HTTP-Antwort für den Webbrowser generiert, ruft sie auch `msg.set(name)` auf. Hiermit wird eine Nachricht in eine Azure Storage-Warteschlange mit dem Namen `outqueue` geschrieben, wie dies ist in der Warteschlangenbindung angegeben ist. Sie können die Warteschlange im [Azure-Portal](../storage/queues/storage-quickstart-queues-portal.md) oder im [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) anzeigen. Sie haben auch die Möglichkeit, die Warteschlange per Azure CLI anzuzeigen. Dies ist in den folgenden Schritten beschrieben:

1. Öffnen Sie die Datei *local.setting.json* des Funktionsprojekts, und kopieren Sie den Wert der Verbindungszeichenfolge. Führen Sie in einem Terminal oder Befehlsfenster den folgenden Befehl aus, um eine Umgebungsvariable mit dem Namen `AZURE_STORAGE_CONNECTION_STRING` zu erstellen, und fügen Sie anstelle von `<connection_string>` Ihre spezifische Verbindungszeichenfolge ein. (Die Verwendung dieser Umgebungsvariablen bewirkt, dass Sie die Verbindungszeichenfolge nicht für jeden weiteren Befehl per `--connection-string`-Argument angeben müssen.)

    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<connection_string>"
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    ---
    
1. (Optional) Verwenden Sie den Befehl [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list), um die Storage-Warteschlangen in Ihrem Konto anzuzeigen. Die Ausgabe dieses Befehls sollte eine Warteschlange mit dem Namen `outqueue` enthalten. Sie wurde erstellt, als die Funktion ihre erste Nachricht in diese Warteschlange geschrieben hat.
    
    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    az storage queue list --output tsv
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    az storage queue list --output tsv
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    az storage queue list --output tsv
    ```
    
    ---


1. Verwenden Sie den Befehl [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek), um die Nachrichten in dieser Warteschlange anzuzeigen. Dies sollte der erste Name sein, den Sie beim zuvor durchgeführten Testen der Funktion genutzt haben. Mit dem Befehl wird die erste Nachricht in der Warteschlange in [Base64-Codierung](functions-bindings-storage-queue.md#encoding) abgerufen. Sie müssen die Nachricht also auch decodieren, um sie als Text anzeigen zu können.

    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    Da Sie die Nachrichtenauflistung dereferenzieren und die Decodierung aus Base64 durchführen müssen, sollten Sie PowerShell ausführen und den PowerShell-Befehl verwenden.

    ---
    
## <a name="redeploy-the-project-to-azure"></a>Erneutes Bereitstellen des Projekts in Azure

Nachdem Sie nun die Funktion lokal getestet und sichergestellt haben, dass eine Nachricht in die Azure Storage-Warteschlange geschrieben wurde, können Sie Ihr Projekt erneut bereitstellen, um den in Azure ausgeführten Endpunkt zu aktualisieren.

1. Verwenden Sie im Ordner *LocalFunctionsProj* den Befehl [`func azure functionapp publish`](functions-run-local.md#project-file-deployment), um das Projekt erneut bereitzustellen, indem Sie `<app_name>` durch den Namen Ihrer App ersetzen.

    ```
    func azure functionapp publish <app_name>
    ```
    
1. Verwenden Sie wie in der vorherigen Schnellstartanleitung einen Browser oder curl, um die erneut bereitgestellte Funktion zu testen.

    # <a name="browsertabbrowser"></a>[Browser](#tab/browser)
    
    Kopieren Sie die vollständige **Aufruf-URL**, die in der Ausgabe des Befehls zum Veröffentlichen (publish) angezeigt wird, in eine Browseradressleiste, und fügen Sie den Abfrageparameter `&name=Azure` an. Im Browser sollte eine ähnliche Ausgabe wie bei der lokalen Ausführung der Funktion angezeigt werden.

    ![Ausgabe der in Azure ausgeführten Funktion in einem Browser](./media/functions-create-first-function-python/function-test-cloud-browser.png)

    # <a name="curltabcurl"></a>[curl](#tab/curl)
    
    Führen Sie [curl](https://curl.haxx.se/) mit der **Aufruf-URL** aus, und fügen Sie den Parameter `&name=Azure` an. Die Ausgabe des Befehls sollte der Text „Hello Azure“ sein.
    
    ![Ausgabe der in Azure per curl ausgeführten Funktion](./media/functions-create-first-function-python/function-test-cloud-curl.png)

    --- 

1. Untersuchen Sie die Storage-Warteschlange erneut (wie im vorherigen Abschnitt beschrieben), um zu überprüfen, ob sie die neu geschriebene Nachricht enthält.

    Falls Sie für die Untersuchung der Warteschlange die Azure CLI verwenden, wird mit dem Befehl `az storage message peek` nur die erste Nachricht in der Warteschlange angezeigt. Verwenden Sie zum Simulieren der Nachrichtenverarbeitung stattdessen `az storage message get` mit den gleichen Argumenten. Der Befehl `get` gibt die Nachricht zurück und entfernt sie aus der Warteschlange. Sie können den Befehl dann wiederholen, bis die Warteschlange leer ist (und für den Befehl ein Fehler angezeigt wird).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Schritt [Aktivieren der Application Insights-Integration](functions-monitoring.md#manually-connect-an-app-insights-resource) fortfahren möchten, sollten Sie alle Ressourcen beibehalten, weil er auf der bisherigen Arbeit aufbaut.

Verwenden Sie andernfalls den unten angegebenen Befehl, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, damit keine weiteren Kosten anfallen.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aktivieren der Application Insights-Integration mit einer Azure Functions-App](functions-monitoring.md#manually-connect-an-app-insights-resource)

Weitere Ressourcen:

- [Beispiele für vollständige Functions-Projekte in Python](/samples/browse/?products=azure-functions&languages=python).
- [Python-Entwicklerhandbuch für Azure Functions](functions-reference-python.md)
- [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
- [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/)
- [Abschätzen der Kosten des Verbrauchstarifs](functions-consumption-costs.md)
