---
title: Azure Functions mit Event Grid Blob-Trigger
description: Erfahren Sie, wie Sie mit dem Event Grid Blob-Trigger einrichten und debuggen.
author: cachai2
ms.topic: conceptual
ms.date: 3/1/2021
ms.author: cachai
ms.openlocfilehash: 259928cbfc046930d82bc3462db6d8aad4cd6b06
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108124993"
---
# <a name="azure-function-event-grid-blob-trigger"></a>Azure Function mit Event Grid Blob-Trigger

In diesem Artikel wird veranschaulicht, wie Sie eine lokale Event Grid durch Blob ausgelöste Funktion debuggen und bereitstellen, welche Ereignisse behandelt, die von einem Speicherkonto ausgelöst werden.

> [!NOTE]
> Der Event Grid Blob-Trigger befindet sich in der Vorschau.

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen oder Verwenden einer vorhandenen Funktions-App
- Erstellen oder Verwenden eines vorhandenen Speicherkontos
- Version 5.0 und höher der [Erweiterung Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/5.0.0-beta.2) muss installiert sein.
- Herunterladen von [ngrok](https://ngrok.com/) zum Ermöglichen des Aufrufs Ihrer lokalen Funktion in Azure

## <a name="create-a-new-function"></a>Erstellen einer neuen Funktion

1. Öffnen Sie Ihre Funktions-App in Visual Studio Code.

1. **Drücken Sie F1**, um eine neue Blob-Triggerfunktion zu erstellen. Verwenden Sie die Verbindungszeichenfolge für Ihr Speicherkonto.

1. Die Standard-URL für Ihren Event Grid Blob-Trigger lautet:

    # <a name="c"></a>[C#](#tab/csharp)

    ```http
    http://localhost:7071/runtime/webhooks/blobs?functionName={functionname}
    ```

    # <a name="python"></a>[Python](#tab/python)

    ```http
    http://localhost:7071/runtime/webhooks/blobs?functionName=Host.Functions.{functionname}
    ```

    # <a name="java"></a>[Java](#tab/java)

    ```http
    http://localhost:7071/runtime/webhooks/blobs?functionName=Host.Functions.{functionname}
    ```

    ---

    Beachten Sie den Namen Ihrer Funktions-App und dass der Triggertyp ein Blob-Trigger ist, der in der URL als `blobs` angegeben wird. Dies ist erforderlich, wenn Sie Endpunkte später in der Anleitung einrichten.

1. Nachdem die Funktion erstellt wurde, fügen Sie den Event Grid Quellparameter hinzu.

    # <a name="c"></a>[C#](#tab/csharp)
    Fügen Sie den Funktionsparametern **Source = BlobTriggerSource.EventGrid** hinzu.
    
    ```csharp
    [FunctionName("BlobTriggerCSharp")]
    public static void Run([BlobTrigger("samples-workitems/{name}", Source = BlobTriggerSource.EventGrid, Connection = "connection")]Stream myBlob, string name, ILogger log)
    {
        log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
    }
    ```

    # <a name="python"></a>[Python](#tab/python)
    Fügen Sie dem function.json für Bindungsdaten **"source": "EventGrid"** hinzu.
    
    ```json
    {
      "scriptFile": "__init__.py",
      "bindings": [
        {
          "name": "myblob",
          "type": "blobTrigger",
          "direction": "in",
          "path": "samples-workitems/{name}",
          "source": "EventGrid",
          "connection": "MyStorageAccountConnectionString"
        }
      ]
    }
    ```

    # <a name="java"></a>[Java](#tab/java)
    **Drücken Sie F5**, um die Funktion zu erstellen. Fügen Sie nach Abschluss des Builds dem **function.js** für Bindungsdaten **"source": "EventGrid"** hinzu.
    
    ```json
    {
      "scriptFile" : "../java-1.0-SNAPSHOT.jar",
      "entryPoint" : "com.function.{MyFunctionName}.run",
      "bindings" : [ {
        "type" : "blobTrigger",
        "direction" : "in",
        "name" : "content",
        "path" : "samples-workitems/{name}",
        "dataType" : "binary",
        "source": "EventGrid",
        "connection" : "MyStorageAccountConnectionString"
       } ]
    }
    ```

    ---

1. Legen Sie einen Haltepunkt in Ihrer Funktion in der Zeile fest, welche die Protokollierung verarbeitet.

1. Starten einer Debugsitzung.

    # <a name="c"></a>[C#](#tab/csharp)
    Drücken Sie anschließend **F5**, um eine Debugsitzung zu starten.

    # <a name="python"></a>[Python](#tab/python)
    Drücken Sie anschließend **F5**, um eine Debugsitzung zu starten.

    # <a name="java"></a>[Java](#tab/java)
    Öffnen Sie ein neues Terminal und führen Sie den folgenden mvn-Befehl aus, um die Debugsitzung zu starten.

    ```bash
    mvn azure-functions:run
    ```

    ---

[!INCLUDE [functions-event-grid-local-dev](../../includes/functions-event-grid-local-dev.md)]

## <a name="debug-the-function"></a>Debuggen der Funktion
Sobald der Blob-Trigger erkennt, dass eine neue Datei in den Speichercontainer hochgeladen wurde, wird der Haltepunkt in der lokalen Funktion erreicht.

## <a name="deployment"></a>Bereitstellung

Wenn Sie die Funktions-App in Azure bereitstellen, aktualisieren Sie den Webhook-Endpunkt von Ihrem lokalen Endpunkt auf ihrem bereitgestellten App-Endpunkt. Führen Sie zum Aktualisieren eines Endpunkts die Schritte unter [Hinzufügen eines Speicherereignisses](#add-a-storage-event) aus und verwenden Sie den folgenden Schritt für die Webhook-URL in Schritt 5. Die `<BLOB-EXTENSION-KEY>` finden Sie im Abschnitt **App-Schlüssel** im Menü links in Ihrer **Funktions-App**.

# <a name="c"></a>[C#](#tab/csharp)

```http
https://<FUNCTION-APP-NAME>.azurewebsites.net/runtime/webhooks/blobs?functionName=<FUNCTION-NAME>&code=<BLOB-EXTENSION-KEY>
```

# <a name="python"></a>[Python](#tab/python)

```http
https://<FUNCTION-APP-NAME>.azurewebsites.net/runtime/webhooks/blobs?functionName=Host.Functions.<FUNCTION-NAME>&code=<BLOB-EXTENSION-KEY>
```

# <a name="java"></a>[Java](#tab/java)

```http
https://<FUNCTION-APP-NAME>.azurewebsites.net/runtime/webhooks/blobs?functionName=Host.Functions.<FUNCTION-NAME>&code=<BLOB-EXTENSION-KEY>
```

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die in diesem Artikel erstellten Ressourcen zu bereinigen, löschen Sie das Event Grid-Abonnement, das Sie in diesem Tutorial erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

- [Automatisieren der Größenänderung von hochgeladenen Images per Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md)
- [Event Grid-Trigger für Azure Functions](./functions-bindings-event-grid.md)