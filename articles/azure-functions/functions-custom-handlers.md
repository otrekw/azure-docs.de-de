---
title: Benutzerdefinierte Azure Functions-Handler (Vorschau)
description: Erfahren Sie, wie Sie Azure Functions mit einer beliebigen Sprache oder Laufzeitversion verwenden.
author: craigshoemaker
ms.author: cshoe
ms.date: 3/18/2020
ms.topic: article
ms.openlocfilehash: 5abc216e182d7becd9d6f42e0f566ee96d09c2a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475136"
---
# <a name="azure-functions-custom-handlers-preview"></a>Benutzerdefinierte Azure Functions-Handler (Vorschau)

Jede Functions-App wird von einem sprachspezifischen Handler ausgeführt. Auch wenn Azure Functions zahlreiche [Sprachhandler](./supported-languages.md) standardmäßig unterstützt, gibt es Fälle, in denen Sie möglicherweise zusätzliche Kontrolle über die App-Ausführungsumgebung haben möchten. Benutzerdefinierte Handler geben Ihnen diese zusätzliche Kontrolle.

Benutzerdefinierte Handler sind schlanke Webserver, die Ereignisse vom Functions-Host empfangen. Jede Sprache, die HTTP-Primitive unterstützt, kann einen benutzerdefinierten Handler implementieren.

Benutzerdefinierte Handler eignen sich am besten für Situationen, in denen Sie Folgendes ausführen möchten:

- Implementieren einer Functions-App in einer Sprache, die über die offiziell unterstützten Sprachen hinausgeht.
- Implementieren einer Functions-App in einer Sprachversion oder Laufzeit, die standardmäßig nicht unterstützt wird.
- Ausüben präziser Kontrolle über die App-Ausführungsumgebung.

Bei benutzerdefinierten Handlern werden alle [Trigger und Eingabe- und Ausgabebindungen](./functions-triggers-bindings.md) über [Erweiterungspakete](./functions-bindings-register.md) unterstützt.

## <a name="overview"></a>Übersicht

Die folgende Abbildung zeigt die Beziehung zwischen dem Functions-Host und einem Webserver, der als benutzerdefinierter Handler implementiert ist.

![Benutzerdefinierte Azure Functions-Handler: Übersicht](./media/functions-custom-handlers/azure-functions-custom-handlers-overview.png)

- Ereignisse löst eine Anforderung aus, die an den Functions-Host gesendet wird. Das Ereignis enthält entweder eine unformatierte HTTP-Nutzlast (für durch HTTP ausgelöste Funktionen ohne Bindungen) oder eine Nutzlast, die Eingabebindungsdaten für die Funktion enthält.
- Der Functions-Host leitet die Anforderung dann über einen Proxy an den Webserver weiter, indem er eine [Anforderungsnutzlast](#request-payload) ausgibt.
- Der Webserver führt die einzelne Funktion aus und gibt eine [Antwortnutzlast](#response-payload) an den Functions-Host zurück.
- Der Functions-Host leitet die Antwort über einen Proxy als Ausgabebindungsnutzlast an das Ziel weiter.

Eine Azure Functions-App, die als benutzerdefinierter Handler implementiert ist, muss die Dateien *host.json* und *function.json* bestimmten Konventionen entsprechend konfigurieren.

## <a name="application-structure"></a>Anwendungsstruktur

Um einen benutzerdefinierten Handler zu implementieren, benötigen Sie die folgenden Komponenten für Ihre Anwendung:

- Eine Datei *host.json* im Stammverzeichnis Ihrer App.
- Eine Datei *function.json* für jede Funktion (in einem Ordner, der mit dem Funktionsnamen übereinstimmt).
- Einen Befehl, ein Skript oder eine ausführbare Datei zum Ausführen eines Webservers.

Die folgende Abbildung zeigt, wie diese Dateien im Dateisystem nach einer Funktion mit dem Namen „order“ suchen.

```bash
| /order
|   function.json
|
| host.json
```

### <a name="configuration"></a>Konfiguration

Die Anwendung wird über die Datei *host.json* konfiguriert. Diese Datei informiert den Functions-Host, wohin Anforderungen gesendet werden sollen, indem auf einen Webserver verwiesen wird, der HTTP-Ereignisse verarbeiten kann.

Ein benutzerdefinierter Handler wird definiert, indem die Datei *host.json* mit Details zum Ausführen des Webservers über den Abschnitt `httpWorker` konfiguriert wird.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "server.exe"
        }
    }
}
```

Der Abschnitt `httpWorker` verweist auf ein Ziel (wie durch `defaultExecutablePath`definiert). Das Ausführungsziel kann entweder ein Befehl, eine ausführbare Datei oder eine Datei sein, in der der Webserver implementiert wird.

Bei Skript-Apps verweist `defaultExecutablePath` auf die Laufzeit der Skriptsprache und `defaultWorkerPath` auf den Speicherort der Skriptdatei. Im folgenden Beispiel wird gezeigt, wie eine JavaScript-App in Node.js als benutzerdefinierter Handler konfiguriert wird.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

Auch mithilfe des `arguments`-Arrays können Sie Argumente übergeben:

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js",
            "arguments": [ "--argument1", "--argument2" ]
        }
    }
}
```

Argumente sind für viele Debugkonfigurationen erforderlich. Weitere Informationen finden Sie im Abschnitt [Debuggen](#debugging).

> [!NOTE]
> Die Datei *host.json* muss sich auf derselben Ebene in der Verzeichnisstruktur befinden wie der ausgeführte Webserver. Einige Sprachen und Toolketten platzieren diese Datei möglicherweise nicht standardmäßig im Anwendungsstamm.

#### <a name="bindings-support"></a>Bindungsunterstützung

Standardtrigger sind zusammen mit Eingabe- und Ausgabebindungen verfügbar, indem Sie in der Datei *host.json* auf [Erweiterungspakete](./functions-bindings-register.md) verweisen.

### <a name="function-metadata"></a>Funktionsmetadaten

Bei Verwendung mit einem benutzerdefinierten Handler unterscheiden sich die Inhalte von *function.json* nicht von der Definition einer Funktion in einem beliebigen anderen Kontext. Die einzige Voraussetzung besteht darin, dass sich *function.json*-Dateien in einem Ordner befinden müssen, dessen Name mit dem Funktionsnamen übereinstimmt.

### <a name="request-payload"></a>Anforderungsnutzlast

Die Anforderungsnutzlast für reine HTTP-Funktionen ist die unformatierte HTTP-Anforderungsnutzlast. Reine HTTP-Funktionen werden als Funktionen ohne Eingabe- oder Ausgabebindungen definiert, die eine HTTP-Antwort zurückgeben.

Jeder andere Funktionstyp, der entweder Eingabe- oder Ausgabebindungen enthält oder über eine andere Ereignisquelle als HTTP ausgelöst wird, verfügt über eine benutzerdefinierte Anforderungsnutzlast.

Der folgende Code stellt eine Anforderungsbeispielnutzlast dar. Die Nutzlast umfasst eine JSON-Struktur mit zwei Elementen: `Data` und `Metadata`.

Das `Data`-Element enthält Schlüssel, die Eingabe- und Triggernamen entsprechen, wie im Bindungsarray in der Datei *function.json* definiert.

Das `Metadata`-Element umfasst [Metadaten, die aus der Ereignisquelle generiert](./functions-bindings-expressions-patterns.md#trigger-metadata) werden.

Wenn die Bindungen in der folgenden Datei *function.json* definiert sind:

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages-incoming",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "type": "queue",
      "direction": "out",
      "queueName": "messages-outgoing",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

Wird eine Anforderungsnutzlast zurückgegeben, die dem folgenden Beispiel ähnelt:

```json
{
    "Data": {
        "myQueueItem": "{ message: \"Message sent\" }"
    },
    "Metadata": {
        "DequeueCount": 1,
        "ExpirationTime": "2019-10-16T17:58:31+00:00",
        "Id": "800ae4b3-bdd2-4c08-badd-f08e5a34b865",
        "InsertionTime": "2019-10-09T17:58:31+00:00",
        "NextVisibleTime": "2019-10-09T18:08:32+00:00",
        "PopReceipt": "AgAAAAMAAAAAAAAAAgtnj8x+1QE=",
        "sys": {
            "MethodName": "QueueTrigger",
            "UtcNow": "2019-10-09T17:58:32.2205399Z",
            "RandGuid": "24ad4c06-24ad-4e5b-8294-3da9714877e9"
        }
    }
}
```

### <a name="response-payload"></a>Antwortnutzlast

Gemäß der Konvention werden Funktionsantworten als Schlüssel-Wert-Paare formatiert. Unterstützte Schlüssel sind:

| <nobr>Nutzlastschlüssel</nobr>   | Datentyp | Bemerkungen                                                      |
| ------------- | --------- | ------------------------------------------------------------ |
| `Outputs`     | JSON      | Enthält Antwortwerte, wie vom `bindings`-Array der Datei *function.json* definiert.<br /><br />Wenn eine Funktion beispielsweise mit einer Blobspeicher-Ausgabebindung namens „blob“ konfiguriert ist, enthält `Outputs` einen Schlüssel mit dem Namen `blob`, der auf den Wert des Blobs festgelegt ist. |
| `Logs`        | array     | Meldungen des Funktionsprotokolls werden in den Functions-Aufrufprotokollen angezeigt.<br /><br />Bei Ausführung in Azure werden Meldungen in Application Insights angezeigt. |
| `ReturnValue` | Zeichenfolge    | Wird verwendet, um eine Antwort bereitzustellen, wenn eine Ausgabe als `$return` in der Datei *function.json* konfiguriert wird. |

Weitere Informationen finden sie im [Beispiel für eine Beispielnutzlast](#bindings-implementation).

## <a name="examples"></a>Beispiele

Benutzerdefinierte Handler können in jeder Sprache implementiert werden, die HTTP-Ereignisse unterstützt. Azure Functions [unterstützt JavaScript und Node.js vollständig](./functions-reference-node.md). Die folgenden Beispiele zeigen jedoch, wie ein benutzerdefinierter Handler mithilfe von JavaScript in Node.js für Anweisungszwecke implementiert wird.

> [!TIP]
> Die hier gezeigten auf Node.js basierenden Beispiele bieten eine Anleitung zum Implementieren eines benutzerdefinierten Handlers in anderen Sprachen. Sie können jedoch auch nützlich sein, wenn Sie eine Functions-App in einer nicht unterstützten Version von Node.js ausführen möchten.

## <a name="http-only-function"></a>Reine HTTP-Funktion

Im folgenden Beispiel wird veranschaulicht, wie eine durch HTTP ausgelöste Funktion ohne zusätzliche Bindungen oder Ausgaben konfiguriert wird. Das in diesem Beispiel implementierte Szenario enthält eine Funktion namens `http`, die eine `GET`- oder `POST`-Anforderungen annimmt.

Der folgende Codeausschnitt zeigt, wie eine Anforderung an die Funktion zusammengestellt wird.

```http
POST http://127.0.0.1:7071/api/hello HTTP/1.1
content-type: application/json

{
  "message": "Hello World!"
}
```

<a id="hello-implementation" name="hello-implementation"></a>

### <a name="implementation"></a>Implementierung

In einem Ordner mit dem Namen *http* konfiguriert die Datei *function.json* die durch HTTP ausgelöste Funktion.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get", "post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

Die Funktion ist so konfiguriert, dass sowohl `GET`- als auch `POST`-Anforderungen akzeptiert werden, und der Ergebniswert wird über ein Argument namens `res` bereitgestellt.

Im Stammverzeichnis der App ist die Datei *host.json* Datei zum Ausführen von Node.js und zum Verweisen auf die Datei `server.js` konfiguriert.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

Die Datei *server.js* implementiert einen Webserver und eine HTTP-Funktion.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

const PORT = process.env.FUNCTIONS_HTTPWORKER_PORT;

const server = app.listen(PORT, "localhost", () => {
  console.log(`Your port is ${PORT}`);
  const { address: host, port } = server.address();
  console.log(`Example app listening at http://${host}:${port}`);
});

app.get("/hello", (req, res) => {
  res.json("Hello World!");
});

app.post("/hello", (req, res) => {
  res.json({ value: req.body });
});
```

In diesem Beispiel wird Express zum Erstellen eines Webservers verwendet, um HTTP-Ereignisse zu verarbeiten. Es wird festgelegt, dass über `FUNCTIONS_HTTPWORKER_PORT` auf Anforderungen gelauscht wird.

Die Funktion wird im Pfad von `/hello` definiert. `GET`-Anforderungen werden durch Rückgabe eines einfachen JSON-Objekts verarbeitet, und `POST`-Anforderungen haben über `req.body` Zugriff auf den Anforderungstext.

Die Route für die order-Funktion ist `/hello` und nicht `/api/hello`, da der Functions-Host die Anforderung über einen Proxy an den benutzerdefinierten Handler weiterleitet.

>[!NOTE]
>`FUNCTIONS_HTTPWORKER_PORT` ist nicht der öffentliche Port, mit dem die Funktion aufgerufen wird. Dieser Port wird vom Functions-Host verwendet, um den benutzerdefinierten Handler aufzurufen.

## <a name="function-with-bindings"></a>Funktion mit Bindungen

Das in diesem Beispiel implementierte Szenario enthält eine Funktion namens `order`, die eine `POST`-Anforderung mit einer Nutzlast annimmt, die eine Produktbestellung darstellt. Wenn eine Bestellung an die Funktion gesendet wird, wird eine Queue Storage-Nachricht erstellt und eine HTTP-Antwort zurückgegeben.

```http
POST http://127.0.0.1:7071/api/order HTTP/1.1
content-type: application/json

{
  "id": 1005,
  "quantity": 2,
  "color": "black"
}
```

<a id="bindings-implementation" name="bindings-implementation"></a>

### <a name="implementation"></a>Implementierung

In einem Ordner mit dem Namen *order* konfiguriert die Datei *function.json* die durch HTTP ausgelöste Funktion.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "authLevel": "function",
      "direction": "in",
      "name": "req",
      "methods": ["post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "queue",
      "name": "message",
      "direction": "out",
      "queueName": "orders",
      "connection": "AzureWebJobsStorage"
    }
  ]
}

```

Diese Funktion wird als eine [durch HTTP ausgelöste Funktion](./functions-bindings-http-webhook-trigger.md) definiert, die eine [HTTP-Antwort](./functions-bindings-http-webhook-output.md) zurückgibt und eine [Queue Storage](./functions-bindings-storage-queue-output.md)-Nachricht ausgibt.

Im Stammverzeichnis der App ist die Datei *host.json* Datei zum Ausführen von Node.js und zum Verweisen auf die Datei `server.js` konfiguriert.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

Die Datei *server.js* implementiert einen Webserver und eine HTTP-Funktion.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

const PORT = process.env.FUNCTIONS_HTTPWORKER_PORT;

const server = app.listen(PORT, "localhost", () => {
  console.log(`Your port is ${PORT}`);
  const { address: host, port } = server.address();
  console.log(`Example app listening at http://${host}:${port}`);
});

app.post("/order", (req, res) => {
  const message = req.body.Data.req.Body;
  const response = {
    Outputs: {
      message: message,
      res: {
        statusCode: 200,
        body: "Order complete"
      }
    },
    Logs: ["order processed"]
  };
  res.json(response);
});
```

In diesem Beispiel wird Express zum Erstellen eines Webservers verwendet, um HTTP-Ereignisse zu verarbeiten. Es wird festgelegt, dass über `FUNCTIONS_HTTPWORKER_PORT` auf Anforderungen gelauscht wird.

Die Funktion wird im Pfad von `/order` definiert.  Die Route für die order-Funktion ist `/order` und nicht `/api/order`, da der Functions-Host die Anforderung über einen Proxy an den benutzerdefinierten Handler weiterleitet.

Wenn `POST`-Anforderungen an diese Funktion gesendet werden, werden die Daten über wenige Punkte verfügbar gemacht:

- Der Anforderungstext ist über `req.body` verfügbar.
- Die Daten, die an die Funktion gesendet werden, sind über `req.body.Data.req.Body` verfügbar.

Die Antwort der Funktion wird in ein Schlüssel-Wert-Paar formatiert, wobei das `Outputs`-Element einen JSON-Wert enthält, in dem die Schlüssel den Ausgaben entsprechen, die in der Datei *function.json* definiert sind.

Durch Festlegen von `message` auf die Nachricht, die von der Anforderung gesendet wurde, und `res` auf die erwartete HTTP-Antwort, gibt diese Funktion eine Nachricht an Queue Storage aus und eine HTTP-Antwort zurück.

## <a name="debugging"></a>Debuggen

Zum Debuggen der benutzerdefinierten Functions-Handler-App müssen Sie für die Sprache und Laufzeit geeignete Argumente zum Aktivieren des Debuggens hinzufügen.

Um beispielsweise eine Node.js-Anwendung zu debuggen, wird das `--inspect`-Flag als Argument in der Datei *host.json* übergeben.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js",
            "arguments": [ "--inspect" ]
        }
    }
}
```

> [!NOTE]
> Die Debugkonfiguration ist Teil der Datei *Host.json*, was bedeutet, dass Sie vor der Bereitstellung in der Produktion ggf. einige Argumente entfernen müssen.

Mit dieser Konfiguration können Sie den Hostprozess der Funktion mit dem folgenden Befehl starten:

```bash
func host start
```

Nachdem der Prozess gestartet wurde, können Sie einen Debugger anfügen und Breakpoints ansteuern.

### <a name="visual-studio-code"></a>Visual Studio Code

Die folgende Beispielkonfiguration zeigt, wie Sie Ihre Datei *launch.json* einrichten können, um eine Verbindung zwischen Ihrer App und dem Visual Studio Code-Debugger herzustellen.

Dieses Beispiel gilt für Node.js. Sie müssen dieses Beispiel daher möglicherweise für andere Sprachen oder Laufzeiten ändern.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Attach to Node Functions",
      "type": "node",
      "request": "attach",
      "port": 9229,
      "preLaunchTask": "func: host start"
    }
  ]
}
```

## <a name="deploying"></a>Wird bereitgestellt

Ein benutzerdefinierter Handler kann für nahezu jede Azure Functions-Hostingoption bereitgestellt werden (siehe [Einschränkungen](#restrictions)). Wenn Ihr Handler benutzerdefinierte Abhängigkeiten erfordert (z. B. eine Sprachlaufzeit), müssen Sie möglicherweise einen [benutzerdefinierten Container](./functions-create-function-linux-custom-image.md) verwenden.

## <a name="restrictions"></a>Beschränkungen

- Benutzerdefinierte Handler werden in Linux-Nutzungsplänen nicht unterstützt.
- Der Webserver muss innerhalb von 60 Sekunden gestartet werden.

## <a name="samples"></a>Beispiele

Beispiele für die Implementierung von Funktionen in einer Vielzahl verschiedener Sprachen finden Sie im [GitHub-Repository mit Beispielen für benutzerdefinierte Handler](https://github.com/Azure-Samples/functions-custom-handlers).
