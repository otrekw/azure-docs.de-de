---
title: Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure
description: Es wird beschrieben, wie Sie serverlosen Python-Code mit Azure Functions in der Cloud erstellen und bereitstellen.
ms.date: 01/15/2020
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 452c5aeab5d2a1092cb7d338d37e26a82d92396e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845497"
---
# <a name="quickstart-create-an-http-triggered-python-function-in-azure"></a>Schnellstart: Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure

In diesem Artikel verwenden Sie Befehlszeilentools zum Erstellen einer Python-Funktion, die auf HTTP-Anforderungen antwortet. Der Code wird lokal getestet und anschließend in der serverlosen Umgebung von Azure Functions bereitgestellt. Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

Es gibt auch eine [Visual Studio Code-basierte Version](/azure/python/tutorial-vs-code-serverless-python-01) dieses Artikels.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.7.1846 oder höher.
- [Azure CLI](/cli/azure/install-azure-cli), Version 2.0.76 oder höher. 
- [Python 3.7.4 (64 Bit)](https://www.python.org/downloads/release/python-374/). (Python 3.7.4 wurde in Bezug auf Azure Functions überprüft, und Python 3.8 und höhere Versionen werden noch nicht unterstützt.)

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

1. Führen Sie in einem Terminal oder Befehlsfenster `func --version` aus, um zu überprüfen, ob mindestens Version 2.7.1846 der Azure Functions Core Tools verwendet wird.
1. Führen Sie `az --version` aus, um zu überprüfen, ob die Azure CLI-Version 2.0.76 oder höher verwendet wird.
1. Führen Sie `az login` aus, um sich bei Azure anzumelden und zu überprüfen, ob ein aktives Abonnement vorhanden ist.
1. Führen Sie `python --version` (Linux/Mac OS) oder `py --version` (Windows) aus, um zu überprüfen, ob Python-Version 3.7.x verwendet wird.

## <a name="create-and-activate-a-virtual-environment"></a>Erstellen und Aktivieren einer virtuellen Umgebung

Führen Sie die folgenden Befehle in einem geeigneten Ordner aus, um eine virtuelle Umgebung mit dem Namen `.venv` zu erstellen und zu aktivieren. Achten Sie darauf, dass Sie Python 3.7 verwenden. Diese Version wird von Azure Functions unterstützt.


# <a name="bashtabbash"></a>[Bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Führen Sie den folgenden Befehl aus, wenn über Python das venv-Paket auf Ihrer Linux-Distribution nicht installiert wurde:

```bash
sudo apt-get install python3-venv
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Sie führen alle nachfolgenden Befehle in dieser aktivierten virtuellen Umgebung aus. (Führen Sie `deactivate` aus, um die virtuelle Umgebung zu beenden.)

## <a name="create-a-local-function-project"></a>Erstellen eines lokalen Funktionsprojekts

In Azure Functions handelt es sich bei einem Funktionsprojekt um einen Container für eine oder mehrere individuelle Funktionen, die jeweils auf einen bestimmten Trigger reagieren. Für alle Funktionen eines Projekts werden die gleichen lokalen Konfigurationen und Hostkonfigurationen gemeinsam genutzt. In diesem Abschnitt erstellen Sie ein Funktionsprojekt, das nur eine Funktion enthält.

1. Führen Sie in der virtuellen Umgebung den Befehl `func init` aus, um in einem Ordner mit dem Namen *LocalFunctionProj* ein Funktionsprojekt mit der angegebenen Runtime zu erstellen:

    ```
    func init LocalFunctionProj --python
    ```
    
    Dieser Ordner enthält verschiedene Dateien für das Projekt, z. B. die Konfigurationsdateien [local.settings.json](functions-run-local.md#local-settings-file) und [host.json](functions-host-json.md). Da *local.settings.json* aus Azure heruntergeladene Geheimnisse enthalten kann, wird die Datei in der *GITIGNORE*-Datei standardmäßig aus der Quellcodeverwaltung ausgeschlossen.

    > [!TIP]
    > Da ein Funktionsprojekt an eine bestimmte Runtime gebunden ist, müssen alle Funktionen des Projekts in derselben Sprache geschrieben werden.

1. Navigieren Sie zum Projektordner:

    ```
    cd LocalFunctionProj
    ```
    
1. Fügen Sie dem Projekt eine Funktion hinzu, indem Sie den unten angegebenen Befehl verwenden. Hierbei ist das `--name`-Argument der eindeutige Name Ihrer Funktion, und mit dem `--template`-Argument wird der Trigger der Funktion angegeben. Mit `func new` wird ein Unterordner passend zum Funktionsnamen erstellt. Er enthält eine geeignete Codedatei für die gewählte Sprache des Projekts und eine Konfigurationsdatei mit dem Namen *function.json*.

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

### <a name="optional-examine-the-file-contents"></a>(Optional) Untersuchen des Dateiinhalts

Bei Bedarf können Sie [Lokales Ausführen der Funktion](#run-the-function-locally) überspringen und den Dateiinhalt später untersuchen.

### <a name="__init__py"></a>\_\_init\_\_.py

*\_\_init\_\_.py* enthält eine Python-Funktion vom Typ `main()`, die gemäß der Konfiguration in *function.json* ausgelöst wird.

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

Für den HTTP-Trigger empfängt die Funktion Anforderungsdaten in der Variablen `req`, wie dies in *function.json* definiert ist. `req` ist eine Instanz der [azure.functions.HttpRequest-Klasse](/python/api/azure-functions/azure.functions.httprequest). Das Rückgabeobjekt, das in *function.json* als `$return` definiert ist, ist eine Instanz der [azure.functions.HttpResponse-Klasse](/python/api/azure-functions/azure.functions.httpresponse). Weitere Informationen finden Sie unter [HTTP-Trigger und -Bindungen in Azure Functions](functions-bindings-http-webhook.md).

### <a name="functionjson"></a>function.json

*function.json* ist eine Konfigurationsdatei, in der die Eingabe- und Ausgabebindungen (`bindings`) für die Funktion, einschließlich Triggertyp, definiert sind. Falls erforderlich, können Sie `scriptFile` auch so ändern, dass eine andere Python-Datei aufgerufen wird.

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

Für jede Bindung sind eine Richtung, ein Typ und ein eindeutiger Name erforderlich. Der HTTP-Trigger weist eine Eingabebindung vom Typ [`httpTrigger`](functions-bindings-http-webhook.md#trigger) und eine Ausgabebindung vom Typ [`http`](functions-bindings-http-webhook.md#output) auf.


## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Starten Sie die Funktion, indem Sie den lokalen Azure Functions-Runtimehost im Ordner *LocalFunctionProj* starten:

```
func start
```

Die folgende Ausgabe sollte angezeigt werden. (Falls HttpExample nicht wie unten dargestellt angezeigt wird, haben Sie den Host wahrscheinlich aus dem Ordner *HttpExample* gestartet. Verwenden Sie in diesem Fall **STRG**+**C**, um den Host zu beenden. Navigieren Sie anschließend zum übergeordneten Ordner *LocalFunctionProj*, und führen Sie `func start` erneut aus.)

```output
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
```

Kopieren Sie die URL Ihrer `HttpExample`-Funktion aus dieser Ausgabe in einen Browser, und fügen Sie die Abfragezeichenfolge `?name=<your-name>` an. Die vollständige URL lautet dann wie folgt: `http://localhost:7071/api/HttpExample?name=Functions`. Im Browser sollte eine Meldung wie `Hello Functions` angezeigt werden:

![Ergebnis der lokal im Browser ausgeführten Funktion](./media/functions-create-first-function-python/function-test-local-browser.png)

Im Terminal, in dem Sie `func start` ausgeführt haben, wird beim Senden von Anforderungen auch die Protokollausgabe angezeigt.

Drücken Sie nach Abschluss des Vorgangs **STRG**+**C** um den Funktionshost zu beenden.

## <a name="create-supporting-azure-resources-for-your-function"></a>Erstellen von unterstützenden Azure-Ressourcen für Ihre Funktion

Zum Bereitstellen Ihres Funktionscodes in Azure müssen Sie drei Ressourcen erstellen:

- Eine Ressourcengruppe als logischen Container für verwandte Ressourcen.
- Ein Azure Storage-Konto, unter dem Status- und andere Informationen zu Ihren Projekten verwaltet werden.
- Eine Azure-Funktions-App, die als Umgebung zum Ausführen Ihres Funktionscodes dient. Eine Funktions-App ist Ihrem lokalen Funktionsprojekt zugeordnet und ermöglicht Ihnen das Gruppieren von Funktionen als logische Einheit, um die Verwaltung, Bereitstellung und Freigabe von Ressourcen zu vereinfachen.

Sie verwenden Azure CLI-Befehle, um diese Elemente zu erstellen. Für jeden Befehl wird nach Abschluss der Ausführung eine JSON-Ausgabe bereitgestellt.

1. Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az-login) bei Azure an, falls Sie dies noch nicht getan haben:

    ```azurecli
    az login
    ```
    
1. Erstellen Sie mithilfe des Befehls [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `AzureFunctionsQuickstart-rg` in der Region `westeurope` erstellt. (Im Allgemeinen erstellen Sie Ihre Ressourcengruppe und die Ressourcen in einer Region in Ihrer Nähe, indem Sie über den Befehl `az account list-locations` eine verfügbare Region nutzen.)

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
    
    > [!NOTE]
    > In derselben Ressourcengruppe können nicht gleichzeitig Linux- und Windows-Apps gehostet werden. Wenn Sie über eine bestehende Ressourcengruppe mit dem Namen `AzureFunctionsQuickstart-rg` und einer Windows-Funktions-App oder -Web-App verfügen, müssen Sie eine andere Ressourcengruppe verwenden.
    
1. Erstellen Sie mit dem Befehl [az storage account create](/cli/azure/storage/account#az-storage-account-create) in Ihrer Ressourcengruppe und Region ein universelles Speicherkonto. Ersetzen Sie im folgenden Beispiel `<storage_name>` durch einen global eindeutigen Namen, der für Sie geeignet ist. Namen dürfen nur 3 bis 24 Zeichen und ausschließlich Kleinbuchstaben enthalten. Mit `Standard_LRS` wird ein typisches universelles Konto angegeben.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    Mit diesem Speicherkonto fallen für diese Schnellstartanleitung nur Kosten in Höhe von wenigen US-Cent an.
    
1. Erstellen Sie die Funktions-App mit dem Befehl [az functionapp create](/cli/azure/functionapp#az-functionapp-create). Ersetzen Sie im folgenden Beispiel `<storage_name>` durch den Namen des Kontos, das Sie im vorherigen Schritt verwendet haben, und `<app_name>` durch einen global eindeutigen Namen, der für Sie geeignet ist. `<app_name>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App.

    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --name <app_name> --storage-account <storage_name>
    ```
    
    Mit diesem Befehl wird eine Funktions-App erstellt, für die die angegebene Language Runtime unter dem [Azure Functions-Verbrauchstarif](functions-scale.md#consumption-plan) ausgeführt wird. Dies ist für die Nutzungsmenge, die in diesem Fall anfällt, kostenlos. Darüber hinaus wird mit dem Befehl auch eine zugeordnete Azure Application Insights-Instanz in derselben Ressourcengruppe bereitgestellt, mit der Sie Ihre Funktions-App überwachen und Protokolle anzeigen können. Weitere Informationen finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md). Für die Instanz fallen erst Kosten an, wenn Sie sie aktivieren.
    
## <a name="deploy-the-function-project-to-azure"></a>Bereitstellen des Funktionsprojekts in Azure

Da nun die benötigten Ressourcen vorhanden sind, können Sie Ihr lokales Funktionsprojekt für die Funktions-App in Azure bereitstellen, indem Sie den Befehl [func azure functionapp publish](functions-run-local.md#project-file-deployment) verwenden. Ersetzen Sie im folgenden Beispiel `<app_name>` durch den Namen Ihrer App.

```
func azure functionapp publish <app_name>
```

Wenn ein Fehler der Art „App mit dem Namen ... kann nicht gefunden werden“ angezeigt wird, sollten Sie einige Sekunden warten und es dann erneut versuchen. Es kann sein, dass Azure die App nach dem vorherigen `az functionapp create`-Befehl nicht richtig initialisiert hat.

Mit dem Befehl zum Veröffentlichen (publish) werden Ergebnisse wie in der folgenden (gekürzten) Ausgabe angezeigt:

```output
Getting site publishing info...
Creating archive for current directory...
Performing remote build for functions project.

...

Deployment successful.
Remote build succeeded!
Syncing triggers...
Functions in msdocs-azurefunctions-qs:
    HttpExample - [httpTrigger]
        Invoke url: https://msdocs-azurefunctions-qs.azurewebsites.net/api/httpexample?code=KYHrydo4GFe9y0000000qRgRJ8NdLFKpkakGJQfC3izYVidzzDN4gQ==
```

## <a name="invoke-the-function-on-azure"></a>Aufrufen der Funktion in Azure

Da für Ihre Funktion ein HTTP-Trigger verwendet wird, führen Sie das Aufrufen durch, indem Sie per Browser eine HTTP-Anforderung an die entsprechende URL senden oder ein Tool wie curl verwenden. In beiden Fällen ist der URL-Parameter `code` Ihr eindeutiger Funktionsschlüssel, über den der Aufruf mit Ihrem Funktionsendpunkt autorisiert wird.

# <a name="browsertabbrowser"></a>[Browser](#tab/browser)

Kopieren Sie die vollständige **Aufruf-URL**, die in der Ausgabe des Befehls zum Veröffentlichen (publish) angezeigt wird, in eine Browseradressleiste, und fügen Sie den Abfrageparameter `&name=Azure` an. Im Browser sollte eine ähnliche Ausgabe wie bei der lokalen Ausführung der Funktion angezeigt werden.

![Ausgabe der in Azure ausgeführten Funktion in einem Browser](./media/functions-create-first-function-python/function-test-cloud-browser.png)


# <a name="curltabcurl"></a>[curl](#tab/curl)

Führen Sie [curl](https://curl.haxx.se/) mit der **Aufruf-URL** aus, und fügen Sie den Parameter `&name=Azure` an. Die Ausgabe des Befehls sollte der Text „Hello Azure“ sein.

![Ausgabe der in Azure per curl ausgeführten Funktion](./media/functions-create-first-function-python/function-test-cloud-curl.png)

---

> [!TIP]
> Verwenden Sie die [Live Metrics Stream-Features von Application Insights](functions-monitoring.md#streaming-logs), um Protokolle für eine veröffentlichte Python-App nahezu in Echtzeit anzuzeigen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Gehen Sie wie folgt vor, falls Sie mit dem nächsten Schritt fortfahren möchten: [Fügen Sie eine Ausgabebindung für die Azure Storage-Warteschlange hinzu](functions-add-output-binding-storage-queue-python.md), und behalten Sie alle Ressourcen bei, weil der nächste Schritt auf der bisherigen Arbeit aufbaut.

Verwenden Sie andernfalls den unten angegebenen Befehl, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, damit keine weiteren Kosten anfallen.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Python-Funktion](functions-add-output-binding-storage-queue-python.md)
