---
title: Python-Entwicklerreferenz für Azure Functions
description: Entwickeln von Funktionen mit Python
ms.topic: article
ms.date: 11/4/2020
ms.custom: devx-track-python
ms.openlocfilehash: b856c68d9393b08fd6e17e48fc77026faf9c4ab2
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110457405"
---
# <a name="azure-functions-python-developer-guide"></a>Python-Entwicklerhandbuch für Azure Functions

Dieser Artikel ist eine Einführung in die Entwicklung von Azure Functions mithilfe von Python. Der folgende Inhalt geht davon aus, dass Sie das [Azure Functions: Entwicklerhandbuch](functions-reference.md) bereits gelesen haben.

Für Python-Entwickler sind möglicherweise auch folgende Artikel interessant:

| Erste Schritte | Konzepte| Szenarien/Beispiele |
| -- | -- | -- | 
| <ul><li>[Python-Funktion unter Verwendung von Visual Studio Code](./create-first-function-vs-code-csharp.md?pivots=programming-language-python)</li><li>[Python-Funktion mit Terminal/Eingabeaufforderung](./create-first-function-cli-csharp.md?pivots=programming-language-python)</li></ul> | <ul><li>[Entwicklerhandbuch](functions-reference.md)</li><li>[Hostingoptionen](functions-scale.md)</li><li>[Überlegungen&nbsp;zur Leistung](functions-best-practices.md)</li></ul> | <ul><li>[Imageklassifizierung mit PyTorch](machine-learning-pytorch.md)</li><li>[Azure-Automatisierungsbeispiel](/samples/azure-samples/azure-functions-python-list-resource-groups/azure-functions-python-sample-list-resource-groups/)</li><li>[Maschinelles Lernen mit TensorFlow](functions-machine-learning-tensorflow.md)</li><li>[Durchsuchen von Python-Beispielen](/samples/browse/?products=azure-functions&languages=python)</li></ul> |

> [!NOTE]
> Obwohl Sie Ihre [Python-basierten Azure-Funktionen lokal unter Windows entwickeln](create-first-function-vs-code-python.md#run-the-function-locally) können, wird Python bei der Ausführung in Azure nur in einem Linux-basierten Hostingplan unterstützt. Sehen Sie sich die Liste der unterstützten [Betriebssystem- und Runtimekombinationen](functions-scale.md#operating-systemruntime) an.

## <a name="programming-model"></a>Programmiermodell

Azure Functions geht davon aus, dass eine Funktion eine zustandslose Methode in Ihrem Python-Skript ist, die Eingaben verarbeitet und Ausgaben erzeugt. Standardmäßig erwartet die Runtime, dass die Methode als globale Methode namens `main()` in der `__init__.py`-Datei implementiert ist. Sie können auch [einen alternativen Einstiegspunkt angeben](#alternate-entry-point).

Daten von Triggern und Bindungen werden mittels Methodenattributen an die Funktion gebunden, die die in der Konfigurationsdatei *function.json* definierte Eigenschaft `name` verwenden. Beispielsweise beschreibt die unten stehende _function.json_ eine einfache Funktion, die von einer HTTP-Anforderung namens `req` ausgelöst wird:

:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json":::

Basierend auf dieser Definition könnte die Datei `__init__.py`, die den Funktionscode enthält, wie im folgenden Beispiel aussehen:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Sie können auch Python-Typanmerkungen verwenden, um die Attributtypen und den Rückgabetyp in der Funktion explizit zu deklarieren. Auf diese Weise können Sie die IntelliSense-und AutoVervollständigen-Funktionen nutzen, die von vielen Python-Code-Editoren bereitgestellt werden.

```python
import azure.functions


def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Verwenden Sie die Python-Anmerkungen im Paket [azure.functions.*](/python/api/azure-functions/azure.functions), um Eingaben und Ausgaben an Ihre Methoden zu binden.

## <a name="alternate-entry-point"></a>Alternativer Einstiegspunkt

Sie können das Standardverhalten einer Funktion ändern, indem Sie die Eigenschaften `scriptFile` und `entryPoint` in der *function.json*-Datei angeben. Beispielsweise weist die unten stehende _function.json_ die Runtime an, die Methode `customentry()` in der _main.py_-Datei als Einstiegspunkt für Ihre Azure-Funktion zu verwenden.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  "bindings": [
      ...
  ]
}
```

## <a name="folder-structure"></a>Ordnerstruktur

Die empfohlene Ordnerstruktur für ein Python Functions-Projekt sieht wie im folgenden Beispiel aus:

```
 <project_root>/
 | - .venv/
 | - .vscode/
 | - my_first_function/
 | | - __init__.py
 | | - function.json
 | | - example.py
 | - my_second_function/
 | | - __init__.py
 | | - function.json
 | - shared_code/
 | | - __init__.py
 | | - my_first_helper_function.py
 | | - my_second_helper_function.py
 | - tests/
 | | - test_my_second_function.py
 | - .funcignore
 | - host.json
 | - local.settings.json
 | - requirements.txt
 | - Dockerfile
```
Der Hauptprojektordner (<project_root>) kann die folgenden Dateien enthalten:

* *local.settings.json*: Wird verwendet, um bei der lokalen Ausführung App-Einstellungen und Verbindungszeichenfolgen zu speichern. Diese Datei wird nicht in Azure veröffentlicht. Weitere Informationen finden Sie unter [local.settings.file](functions-run-local.md#local-settings-file).
* *requirements.txt*: Diese Datei enthält die Liste der bei der Veröffentlichung in Azure vom System installierten Python-Pakete.
* *host.json*: Enthält globale Konfigurationsoptionen, die sich auf alle Funktionen in einer Funktions-App auswirken. Diese Datei wird in Azure veröffentlicht. Nicht alle Optionen werden bei lokaler Ausführung unterstützt. Weitere Informationen finden Sie unter [host.json](functions-host-json.md).
* *.vscode/:* (optional) Diese Datei enthält die VSCode-Konfiguration des Speichers. Weitere Informationen finden Sie unter [VSCode-Einstellung](https://code.visualstudio.com/docs/getstarted/settings).
* *.venv/:* (optional) Diese Datei enthält eine virtuelle Python-Umgebung, die von der lokalen Entwicklung verwendet wird.
* *Dockerfile:* (optional) Diese Datei wird verwendet, wenn Sie Ihr Projekt in einem [benutzerdefinierten Container](functions-create-function-linux-custom-image.md) veröffentlichen.
* *tests/:* (optional) Diese Datei enthält die Testfälle ihrer Funktions-App.
* *.funcignore:* (optional) In dieser Datei werden Dateien deklariert, die nicht in Azure veröffentlicht werden sollen. Normalerweise enthält diese Datei `.vscode/`, um die Editoreinstellung zu ignorieren, `.venv/`, um die lokale virtuelle Python-Umgebungen zu ignorieren, `tests/`, um Testfälle zu ignorieren, und `local.settings.json`, um zu verhindern, dass lokale App-Einstellungen veröffentlicht werden.

Jede Funktion verfügt über eine eigene Codedatei sowie über eine eigene Bindungskonfigurationsdatei (function.json).

Wenn Sie Ihr Projekt in einer Funktions-App in Azure bereitstellen, sollte der gesamte Inhalt des Hauptprojektordners ( *<project_root>* ) in das Paket aufgenommen werden, jedoch nicht der Ordner selbst. `host.json` sollte sich also im Paketstammverzeichnis befinden. Es wird empfohlen, die Tests in einem Ordner zusammen mit anderen Funktionen aufzubewahren, in diesem Beispiel `tests/`. Weitere Informationen finden Sie unter [Komponententests](#unit-testing).

## <a name="import-behavior"></a>Importverhalten

Sie können Module sowohl über relative und als auch über absolute Verweise in Ihren Funktionscode importieren. Auf Basis der oben gezeigten Ordnerstruktur funktionieren die folgenden Importe innerhalb der Funktionsdatei *<project_root>\my\_first\_function\\_\_init\_\_.py*:

```python
from shared_code import my_first_helper_function #(absolute)
```

```python
import shared_code.my_second_helper_function #(absolute)
```

```python
from . import example #(relative)
```

> [!NOTE]
>  Der Ordner *shared_code/* muss eine \_\_init\_\_.py-Datei enthalten, durch die er bei Verwendung der absoluten Importsyntax als Python-Paket gekennzeichnet wird.

Der \_\_app\_\_-Import und der relative Import jenseits der obersten Ebene in den folgenden Beispielen sind veraltet, da sie weder von der statischen Typüberprüfung noch von Python-Testframeworks unterstützt werden:

```python
from __app__.shared_code import my_first_helper_function #(deprecated __app__ import)
```

```python
from ..shared_code import my_first_helper_function #(deprecated beyond top-level relative import)
```

## <a name="triggers-and-inputs"></a>Trigger und Eingaben

Eingaben werden in Azure Functions in zwei Kategorien unterteilt: Triggereingaben und zusätzliche Eingaben. Obwohl sie sich in der Datei `function.json` unterscheiden, ist ihre Verwendung im Python-Code identisch.  Verbindungszeichenfolgen oder Geheimnisse für Trigger- und Eingabequellen werden bei lokaler Ausführung Werten in der Datei `local.settings.json` und bei der Ausführung in Azure den Anwendungseinstellungen zugeordnet.

Im folgenden Code wird beispielsweise der Unterschied zwischen beiden dargestellt:

```json
// function.json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Bei Aufruf der Funktion wird die HTTP-Anforderung als `req` an die Funktion übergeben. Es wird ein Eintrag, der auf der _ID_ in der Routen-URL basiert, aus Azure Blob Storage abgerufen und als `obj` im Funktionstext verfügbar gemacht.  Hier ist das angegebene Speicherkonto die Verbindungszeichenfolge in der App-Einstellung „AzureWebJobsStorage“. Dabei handelt es sich um dasselbe Speicherkonto, das von der Funktions-App verwendet wird.


## <a name="outputs"></a>Ausgaben

Ausgaben können sowohl im Rückgabewert als auch in Ausgabeparametern angegeben werden. Wenn es nur eine Ausgabe gibt, empfehlen wir, den Rückgabewert zu verwenden. Bei mehreren Ausgaben müssen Sie Ausgabeparameter verwenden.

Um den Rückgabewert einer Funktion als Wert für eine Ausgabebindung zu verwenden, sollte die `name`-Eigenschaft der Bindung in `function.json` auf `$return` festgelegt werden.

Um mehrere Ausgaben zu erzeugen, verwenden Sie die `set()`-Methode, die von der [`azure.functions.Out`](/python/api/azure-functions/azure.functions.out)-Schnittstelle bereitgestellt wird, um der Bindung einen Wert zuzuweisen. Die folgende Funktion kann z. B. mithilfe von Push eine Nachricht an eine Warteschlange übertragen und auch eine HTTP-Antwort zurückgeben.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func


def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Protokollierung

Zugriff auf die Azure Functions-Runtimeprotokollierung ist über einen Stamm-[`logging`](https://docs.python.org/3/library/logging.html#module-logging)-Handler in Ihrer Funktions-App verfügbar. Diese Protokollierung ist an Application Insights gebunden und ermöglicht es Ihnen, während der Funktionsausführung aufgetretene Warnungen und Fehler zu kennzeichnen.

Im folgenden Beispiel wird eine Informationsmeldung protokolliert, wenn die Funktion über einen HTTP-Trigger aufgerufen wird.

```python
import logging


def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Es sind zusätzliche Protokollierungsmethoden verfügbar, mit denen Sie auf anderen Ablaufverfolgungsebenen in die Konsole schreiben können:

| Methode                 | BESCHREIBUNG                                |
| ---------------------- | ------------------------------------------ |
| **`critical(_message_)`**   | Schreibt eine Meldung mit der Stufe KRITISCH in die Stammprotokollierung.  |
| **`error(_message_)`**   | Schreibt eine Meldung mit der Stufe ERROR in die Stammprotokollierung.    |
| **`warning(_message_)`**    | Schreibt eine Meldung mit der Stufe WARNUNG in die Stammprotokollierung.  |
| **`info(_message_)`**    | Schreibt eine Meldung mit der Stufe INFO in die Stammprotokollierung.  |
| **`debug(_message_)`** | Schreibt eine Meldung mit der Stufe DEBUG in die Stammprotokollierung.  |

Weitere Informationen über Protokollierung finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

## <a name="http-trigger-and-bindings"></a>HTTP-Trigger und -Bindungen

Der HTTP-Trigger ist in der Datei „function.json“ definiert. Der `name` der Bindung muss mit dem benannten Parameter in der Funktion identisch sein.
In den vorherigen Beispielen wird der Bindungsname `req` verwendet. Dieser Parameter ist ein [HttpRequest]-Objekt, und es wird ein [HttpResponse]-Objekt zurückgegeben.

Aus dem [HttpRequest]-Objekt können Sie Anforderungsheader, Abfrageparameter, Routenparameter und den Nachrichtentext extrahieren.

Das folgende Beispiel stammt aus der [HTTP-Trigger-Vorlage für Python](https://github.com/Azure/azure-functions-templates/tree/dev/Functions.Templates/Templates/HttpTrigger-Python).

```python
def main(req: func.HttpRequest) -> func.HttpResponse:
    headers = {"my-http-header": "some-value"}

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!", headers=headers)
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             headers=headers, status_code=400
        )
```

In dieser Funktion wird der Wert des `name`-Abfrageparameters aus dem `params`-Parameter des [HttpRequest]-Objekts ermittelt. Der JSON-codierte Nachrichtentext wird mit der `get_json`-Methode gelesen.

Außerdem können Sie `status_code` und `headers` für die Antwortnachricht im zurückgegebenen [HttpResponse]-Objekt festlegen.

## <a name="scaling-and-performance"></a>Skalierung und Leistung

Informationen zu den bewährten Methoden für Skalierung und Leistung für Python-Funktions-Apps finden Sie im Artikel [Verbessern der Durchsatzleistung von Python-Apps in Azure Functions](python-scale-performance-reference.md).

## <a name="context"></a>Kontext

Um den Aufrufkontext einer Funktion während der Ausführung abzurufen, nehmen Sie das [`context`](/python/api/azure-functions/azure.functions.context)-Argument in ihre Signatur auf.

Beispiel:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

Die [**Context**](/python/api/azure-functions/azure.functions.context)-Klasse weist die folgenden Zeichenfolgenattribute auf:

`function_directory`: Das Verzeichnis, in dem die Funktion ausgeführt wird.

`function_name`: Der Name der Funktion.

`invocation_id`: Die ID des aktuellen Funktionsaufrufs.

## <a name="global-variables"></a>Globale Variablen

Es besteht keine Garantie, dass der Status Ihrer App für zukünftige Ausführungen beibehalten wird. Jedoch verwendet die Azure Functions-Runtime oft wieder den gleichen Prozess für mehrere Ausführungen derselben App. Zum Zwischenspeichern der Ergebnisse einer teuren Berechnung deklarieren Sie diese als globale Variable.

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="environment-variables"></a>Umgebungsvariablen

In Functions werden [Anwendungseinstellung](functions-app-settings.md), z.B. Dienstverbindungszeichenfolgen, während der Ausführung als Umgebungsvariablen verfügbar gemacht. Sie können auf diese Einstellungen zugreifen, indem Sie `import os` deklarieren und dann `setting = os.environ["setting-name"]` verwenden.

Im folgenden Beispiel wird die [Anwendungseinstellung](functions-how-to-use-azure-function-app-settings.md#settings) mit dem Schlüssel `myAppSetting` ermittelt:

```python
import logging
import os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    # Get the setting named 'myAppSetting'
    my_app_setting_value = os.environ["myAppSetting"]
    logging.info(f'My app setting value:{my_app_setting_value}')
```

Für die lokale Entwicklung werden Anwendungseinstellungen [in der Datei „local.settings.json“ verwaltet](functions-run-local.md#local-settings-file).

## <a name="python-version"></a>Python-Version

Azure Functions unterstützt die folgenden Python-Versionen:

| Functions-Version | Python-Versionen<sup>*</sup> |
| ----- | ----- |
| 3.x | 3.9 (Vorschau) <br/> 3.8<br/>3,7<br/>3.6 |
| 2.x | 3,7<br/>3.6 |

<sup>*</sup>Offizielle CPython-Distributionen

Wenn Sie beim Erstellen der Funktions-App in Azure eine bestimmte Python-Version anfordern möchten, verwenden Sie die `--runtime-version`-Option des Befehls [`az functionapp create`](/cli/azure/functionapp#az_functionapp_create). Die Functions-Runtimeversion wird mit der Option `--functions-version` festgelegt. Die Python-Version wird bei der Erstellung der Funktions-App festgelegt und kann nicht geändert werden.

Bei lokaler Ausführung verwendet die Runtime die verfügbare Python-Version.

## <a name="package-management"></a>Paketverwaltung

Bei der lokalen Entwicklung mithilfe der Azure Functions Core Tools oder von Visual Studio Code fügen Sie die Namen und Versionen der erforderlichen Pakete in der `requirements.txt`-Datei hinzu, und installieren Sie sie mithilfe von `pip`.

Beispielsweise können die folgende Datei mit Anforderungen und der pip-Befehl verwendet werden, um das `requests`-Paket aus PyPI zu installieren.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Veröffentlichen in Azure

Wenn Sie zur Veröffentlichung bereit sind, stellen Sie sicher, dass alle Ihre öffentlich verfügbaren Abhängigkeiten in der Datei „requirements.txt“ aufgeführt sind, die sich im Stamm Ihres Projektverzeichnisses befindet.

Projektdateien und -ordner, die von der Veröffentlichung ausgeschlossen sind, einschließlich des Ordners der virtuellen Umgebung, werden in der .funcignore-Datei aufgelistet.

Für die Veröffentlichung Ihres Python-Projekts in Azure werden drei Buildaktionen unterstützt: Remotebuild, lokaler Build und Builds mit benutzerdefinierten Abhängigkeiten.

Sie können auch Azure Pipelines verwenden, um Ihre Abhängigkeiten zu erstellen und mit Continuous Delivery (CD) Veröffentlichungen durchzuführen. Weitere Informationen finden Sie unter [Continuous Delivery mit Azure DevOps](functions-how-to-azure-devops.md).

### <a name="remote-build"></a>Remotebuild

Bei Verwendung des Remotebuilds stimmen die auf dem Server wiederhergestellten Abhängigkeiten und die nativen Abhängigkeiten mit der Produktionsumgebung überein. Dies führt zu einem kleineren Bereitstellungspaket, das hochgeladen werden muss. Verwenden Sie den Remotebuild, wenn Sie Python-Apps unter Windows entwickeln. Wenn Ihr Projekt über benutzerdefinierte Abhängigkeiten verfügt, können Sie den [Remotebuild mit einer zusätzlichen Index-URL verwenden](#remote-build-with-extra-index-url).

Abhängigkeiten werden entsprechend dem Inhalt der Datei „requirements.txt“ remote abgerufen. [Remotebuild](functions-deployment-technologies.md#remote-build) ist die empfohlene Buildmethode. Standardmäßig fordert Azure Functions Core Tools einen Remotebuild an, wenn Sie den folgenden [func azure functionapp publish](functions-run-local.md#publish)-Befehl zum Veröffentlichen Ihres Python-Projekts in Azure verwenden.

```bash
func azure functionapp publish <APP_NAME>
```

Denken Sie daran, `<APP_NAME>` durch den Namen Ihrer Funktions-App in Azure zu ersetzen.

Die [Azure Functions-Erweiterung für Visual Studio Code](./create-first-function-vs-code-csharp.md#publish-the-project-to-azure) fordert ebenfalls standardmäßig einen Remotebuild an.

### <a name="local-build"></a>Lokaler Build

Abhängigkeiten werden entsprechend dem Inhalt der Datei „requirements.txt“ lokal abgerufen. Sie können das Ausführen eines Remotebuilds verhindern, indem Sie mit dem folgenden [func azure functionapp publish](functions-run-local.md#publish)-Befehl mit einem lokalen Build veröffentlichen.

```command
func azure functionapp publish <APP_NAME> --build local
```

Denken Sie daran, `<APP_NAME>` durch den Namen Ihrer Funktions-App in Azure zu ersetzen.

Mit der `--build local`-Option werden Projektabhängigkeiten aus der Datei „requirements.txt“ gelesen, und die betreffenden abhängigen Pakete werden lokal heruntergeladen und installiert. Projektdateien und Abhängigkeiten werden von Ihrem lokalen Computer in Azure bereitgestellt. Dadurch wird ein größeres Bereitstellungspaket in Azure hochgeladen. Wenn, gleich aus welchen Gründen, Abhängigkeiten in der Datei „requirements.txt “ nicht von Core Tools abgerufen werden können, müssen Sie die benutzerdefinierte Option für Abhängigkeiten für die Veröffentlichung verwenden.

Es ist nicht zu empfehlen, bei der lokalen Entwicklungsarbeit unter Windows lokale Builds zu nutzen.

### <a name="custom-dependencies"></a>Benutzerdefinierte Abhängigkeiten

Wenn Ihr Projekt über Abhängigkeiten verfügt, die nicht im [Index für Python-Pakete](https://pypi.org/) enthalten sind, haben Sie zwei Möglichkeiten für die Erstellung des Projekts. Die Buildmethode hängt davon ab, wie Sie das Projekt erstellen.

#### <a name="remote-build-with-extra-index-url"></a>Remotebuild mit zusätzlicher Index-URL

Verwenden Sie einen Remotebuild, wenn Ihre Pakete über einen zugänglichen benutzerdefinierten Paketindex verfügbar sind. Stellen Sie vor der Veröffentlichung sicher, dass Sie eine [App-Einstellung erstellen](functions-how-to-use-azure-function-app-settings.md#settings), die den Namen `PIP_EXTRA_INDEX_URL` hat. Der Wert für diese Einstellung ist die URL Ihres benutzerdefinierten Paketindexes. Mit der Verwendung dieser Einstellung wird der Remotebuild angewiesen, `pip install` mit der Option `--extra-index-url` auszuführen. Weitere Informationen finden Sie in der [Dokumentation zur Python-PIP-Installation](https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format).

Sie können auch die grundlegenden Anmeldeinformationen für die Authentifizierung mit Ihren zusätzlichen Paketindex-URLs verwenden. Weitere Informationen finden Sie im Abschnitt zu den [grundlegenden Anmeldeinformationen für die Authentifizierung](https://pip.pypa.io/en/stable/user_guide/#basic-authentication-credentials) in der Python-Dokumentation.

#### <a name="install-local-packages"></a>Installieren von lokalen Paketen

Wenn in Ihrem Projekt Pakete verwendet werden, die für unsere Tools nicht öffentlich verfügbar sind, können Sie sie für die App verfügbar machen, indem Sie sie im Verzeichnis „\_\_app\_\_/.python_packages“ ablegen. Führen Sie vor dem Veröffentlichen den folgenden Befehl aus, um die Abhängigkeiten lokal zu installieren:

```command
pip install  --target="<PROJECT_DIR>/.python_packages/lib/site-packages"  -r requirements.txt
```

Wenn Sie benutzerdefinierte Abhängigkeiten verwenden, müssen Sie die `--no-build`-Veröffentlichungsoption nutzen, da Sie die Abhängigkeiten bereits im Projektordner installiert haben.

```command
func azure functionapp publish <APP_NAME> --no-build
```

Denken Sie daran, `<APP_NAME>` durch den Namen Ihrer Funktions-App in Azure zu ersetzen.

## <a name="unit-testing"></a>Komponententests

In Python geschriebene Funktionen können wie anderer Python-Code mithilfe von Standardtestframeworks getestet werden. Bei den meisten Bindungen können Sie ein Pseudoeingabeobjekt erstellen, indem Sie eine Instanz einer geeigneten Klasse aus dem `azure.functions`-Paket erstellen. Da das [`azure.functions`](https://pypi.org/project/azure-functions/)-Paket nicht sofort verfügbar ist, müssen Sie es über Ihre Datei `requirements.txt` installieren. Die Vorgehensweise wird oben im Abschnitt zur [Paketverwaltung](#package-management) beschrieben.

Als Beispiel fungiert *my_second_function* im folgenden Modelltest einer durch HTTP ausgelösten Funktion:

Zunächst müssen Sie die Datei *<project_root>/my_second_function/function.json* erstellen und diese Funktion als HTTP-Triggern definieren.

```json
{
  "scriptFile": "__init__.py",
  "entryPoint": "main",
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

Nun können Sie die Funktionen *my_second_function* und *shared_code.my_second_helper_function* implementieren.

```python
# <project_root>/my_second_function/__init__.py
import azure.functions as func
import logging

# Use absolute import to resolve shared_code modules
from shared_code import my_second_helper_function

# Define an http trigger which accepts ?value=<int> query parameter
# Double the value and return the result in HttpResponse
def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Executing my_second_function.')

    initial_value: int = int(req.params.get('value'))
    doubled_value: int = my_second_helper_function.double(initial_value)

    return func.HttpResponse(
      body=f"{initial_value} * 2 = {doubled_value}",
      status_code=200
    )
```

```python
# <project_root>/shared_code/__init__.py
# Empty __init__.py file marks shared_code folder as a Python package
```

```python
# <project_root>/shared_code/my_second_helper_function.py

def double(value: int) -> int:
  return value * 2
```

Jetzt können Sie mit dem Schreiben von Testfällen für den HTTP-Trigger beginnen.

```python
# <project_root>/tests/test_my_second_function.py
import unittest

import azure.functions as func
from my_second_function import main

class TestFunction(unittest.TestCase):
    def test_my_second_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/my_second_function',
            params={'value': '21'})

        # Call the function.
        resp = main(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'21 * 2 = 42',
        )
```

Installieren Sie in der virtuellen Python-Umgebung `.venv` Ihr bevorzugtes Python-Testframework (z. B. `pip install pytest`). Führen Sie einfach `pytest tests` aus, um das Testergebnis zu überprüfen.

## <a name="temporary-files"></a>Temporäre Dateien

Die `tempfile.gettempdir()`-Methode gibt einen temporären Ordner zurück, unter Linux `/tmp`. Die Anwendung kann dieses Verzeichnis zum Speichern von temporären Dateien verwenden, die von ihren Funktionen während der Ausführung generiert und verwendet werden.

> [!IMPORTANT]
> Für Dateien, die in das temporäre Verzeichnis geschrieben werden, wird nicht garantiert, dass sie über Aufrufe hinweg beibehalten werden. Beim Aufskalieren werden temporäre Dateien nicht von Instanzen gemeinsam verwendet.

Im folgenden Beispiel wird eine benannte temporäre Datei im temporären Verzeichnis (`/tmp`) erstellt:

```python
import logging
import azure.functions as func
import tempfile
from os import listdir

#---
   tempFilePath = tempfile.gettempdir()
   fp = tempfile.NamedTemporaryFile()
   fp.write(b'Hello world!')
   filesDirListInTemp = listdir(tempFilePath)
```

Es wird empfohlen, dass Sie Ihre Tests in einem Ordner außerhalb des Projektordners speichern. Dadurch wird die Bereitstellung von Testcode mit der App verhindert.

## <a name="preinstalled-libraries"></a>Vorinstallierte Bibliotheken

Es gibt einige Bibliotheken, die über die Python-Functions-Runtime verfügen.

### <a name="python-standard-library"></a>Python-Standardbibliothek

Die Python-Standardbibliothek enthält eine Liste mit integrierten Python-Modulen, die in jeder Python-Distribution enthalten sind. Die meisten dieser Bibliotheken dienen Ihnen als Hilfe beim Zugreifen auf die Systemfunktionalität, z. B. Datei-E/A. Auf Windows-Systemen werden diese Bibliotheken mit Python installiert. Auf den UNIX-basierten Systemen werden sie über Paketsammlungen bereitgestellt.

Die vollständigen Details zur Liste mit diesen Bibliotheken finden Sie unter den folgenden Links:

* [Python 3.6-Standardbibliothek](https://docs.python.org/3.6/library/)
* [Python 3.7-Standardbibliothek](https://docs.python.org/3.7/library/)
* [Python 3.8-Standardbibliothek](https://docs.python.org/3.8/library/)
* [Python 3.9-Standardbibliothek](https://docs.python.org/3.9/library/)

### <a name="azure-functions-python-worker-dependencies"></a>Azure Functions: Python-Workerabhängigkeiten

Für den Functions-Python-Worker wird ein bestimmter Satz mit Bibliotheken benötigt. Sie können diese Bibliotheken auch in Ihren Funktionen verwenden, aber sie sind nicht Teil des Python-Standards. Sofern Ihre Funktionen auf einer dieser Bibliotheken basieren, sind sie für Ihren Code ggf. nicht verfügbar, wenn die Ausführung außerhalb von Azure Functions erfolgt. Eine detaillierte Liste mit den Abhängigkeiten finden Sie im Abschnitt **install\_requires** in der Datei [setup.py](https://github.com/Azure/azure-functions-python-worker/blob/dev/setup.py#L282).

> [!NOTE]
> Wenn die Datei „requirements.txt“ Ihrer Funktions-App einen Eintrag `azure-functions-worker` enthält, entfernen Sie ihn. Der Funktionsworker wird automatisch von der Azure Functions-Plattform verwaltet, und wir aktualisieren ihn regelmäßig mit neuen Features und Fehlerbehebungen. Die manuelle Installation einer alten Version des Workers in „requirements.txt“ kann zu unerwarteten Problemen führen.

> [!NOTE]
>  Wenn Ihr Paket bestimmte Bibliotheken enthält, die mit den Abhängigkeiten des Workers in Konflikt stehen können (z. B. protobuf, tensorflow, grpcio), konfigurieren Sie `PYTHON_ISOLATE_WORKER_DEPENDENCIES` in den App-Einstellungen mit `1`, um zu verhindern, dass Ihre Anwendung auf die Abhängigkeiten des Workers verweist.

### <a name="azure-functions-python-library"></a>Azure Functions-Python-Bibliothek

Jedes Update eines Python-Workers enthält eine neue Version der [Azure Functions-Python-Bibliothek (azure.functions)](https://github.com/Azure/azure-functions-python-library). Dieser Ansatz vereinfacht es, Ihre Python-Funktions-Apps fortlaufend zu aktualisieren, weil jedes Update abwärtskompatibel ist. Eine Liste mit den Releases dieser Bibliothek finden Sie unter [azure-functions PyPi](https://pypi.org/project/azure-functions/#history).

Die Version der Runtimebibliothek ist in Azure festgelegt und kann mit „requirements.txt“ nicht überschrieben werden. Der Eintrag `azure-functions` in der Datei „requirements.txt“ dient nur zum Linten und für das Kundenbewusstsein.

Verwenden Sie den folgenden Code, um die tatsächliche Version der Functions-Python-Bibliothek in Ihrer Runtime nachzuverfolgen:

```python
getattr(azure.functions, '__version__', '< 1.2.1')
```

### <a name="runtime-system-libraries"></a>Runtimesystembibliotheken

Eine Liste mit den vorinstallierten Systembibliotheken in Docker-Images von Python-Workern finden Sie unter den folgenden Links:

|  Functions-Runtime  | Debian-Version | Python-Versionen |
|------------|------------|------------|
| Version 2.x | Stretch  | [Python 3.6](https://github.com/Azure/azure-functions-docker/blob/master/host/2.0/stretch/amd64/python/python36/python36.Dockerfile)<br/>[Python 3.7](https://github.com/Azure/azure-functions-docker/blob/master/host/2.0/stretch/amd64/python/python37/python37.Dockerfile) |
| Version 3.x | Buster | [Python 3.6](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python36/python36.Dockerfile)<br/>[Python 3.7](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python37/python37.Dockerfile)<br />[Python 3.8](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python38/python38.Dockerfile)<br/> [Python 3.9](https://github.com/Azure/azure-functions-docker/blob/master/host/3.0/buster/amd64/python/python39/python39.Dockerfile)|

## <a name="cross-origin-resource-sharing"></a>Cross-Origin Resource Sharing

[!INCLUDE [functions-cors](../../includes/functions-cors.md)]

CORS wird für Python-Funktions-Apps vollständig unterstützt.

## <a name="known-issues-and-faq"></a>Bekannte Probleme und FAQ

Hier ist eine Liste mit Leitfäden zur Problembehandlung für häufige Probleme angegeben:

* [ModuleNotFoundError und ImportError](recover-python-functions.md#troubleshoot-modulenotfounderror)
* [Importieren von „cygrpc“ nicht möglich](recover-python-functions.md#troubleshoot-cannot-import-cygrpc)

Alle bekannten Probleme und Funktionsanfragen werden mithilfe der [GitHub-Probleme](https://github.com/Azure/azure-functions-python-worker/issues)liste nachverfolgt. Wenn Sie auf ein Problem stoßen, das in GitHub nicht zu finden ist, öffnen Sie ein neues Problem mit einer ausführlichen Problembeschreibung.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Dokumentation zur Azure Functions-Paket-API](/python/api/azure-functions/azure.functions)
* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Blobspeicherbindungen](functions-bindings-storage-blob.md)
* [HTTP- und Webhook-Bindungen](functions-bindings-http-webhook.md)
* [Warteschlangenspeicherbindungen](functions-bindings-storage-queue.md)
* [Trigger mit Timer](functions-bindings-timer.md)

[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/python-functions-ref-survey)


[HttpRequest]: /python/api/azure-functions/azure.functions.httprequest
[HttpResponse]: /python/api/azure-functions/azure.functions.httpresponse