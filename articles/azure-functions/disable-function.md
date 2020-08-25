---
title: Deaktivieren von Funktionen in Azure Functions
description: Erfahren Sie, wie Sie Funktionen in Azure Functions deaktivieren und aktivieren.
ms.topic: conceptual
ms.date: 04/08/2020
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 761a78f050aa25a62075dd7a53836afb48f89cd7
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88213142"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Deaktivieren von Funktionen in Azure Functions

In diesem Artikel wird erläutert, wie Sie eine Funktion in Azure Functions deaktivieren. Durch das *Deaktivieren* einer Funktion wird die Runtime angewiesen, den für die Funktion definierten automatischen Trigger zu ignorieren. Dadurch können Sie das Ausführen einer bestimmten Funktion verhindern, ohne die gesamte Funktions-App zu beenden.

Es wird empfohlen, eine Funktion mithilfe einer App-Einstellung im Format `AzureWebJobs.<FUNCTION_NAME>.Disabled` zu deaktivieren. Sie können diese Anwendungseinstellung auf verschiedene Weise erstellen und ändern, z. B. über die [Azure-Befehlszeilenschnittstelle](/cli/azure/) und im [Azure-Portal](https://portal.azure.com) auf der Registerkarte **Verwalten** der Funktion. 

> [!NOTE]  
> Wenn Sie eine durch HTTP ausgelöste Funktion mithilfe der in diesem Artikel beschriebenen Methoden deaktivieren, kann der Zugriff auf den Endpunkt bei Ausführung auf dem lokalen Computer weiterhin möglich sein.  

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

Verwenden Sie zum Erstellen und Ändern der App-Einstellung den Azure CLI-Befehl [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). Der folgende Befehl deaktiviert eine Funktion namens `QueueTrigger`. Hierzu wird eine App-Einstellung namens `AzureWebJobs.QueueTrigger.Disabled` erstellt und auf `true` festgelegt. 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

Wenn Sie die Funktion wieder aktivieren möchten, führen Sie den gleichen Befehl mit dem Wert `false` aus.

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

## <a name="use-the-portal"></a>Verwenden des Portals

Sie können auch die Schaltflächen **Aktivieren** und **Deaktivieren** auf der Seite **Übersicht** der Funktion verwenden. Diese Schaltflächen erstellen und löschen die App-Einstellung `AzureWebJobs.<FUNCTION_NAME>.Disabled`.

![Option „Funktionszustand“](media/disable-function/function-state-switch.png)

> [!NOTE]  
> Die im Portal integrierte Testfunktion ignoriert die Einstellung `Disabled`. Dies bedeutet, dass eine deaktivierte Funktion immer noch ausgeführt wird, wenn sie über das Fenster **Test** im Portal gestartet wird. 

## <a name="localsettingsjson"></a>local.settings.json

Funktionen können bei lokaler Ausführung auf die gleiche Weise deaktiviert werden. Um eine Funktion mit dem Namen `HttpExample` zu deaktivieren, fügen Sie der Values-Sammlung in der Datei „local.settings.json“ einen Eintrag wie folgt hinzu:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true", 
    "AzureWebJobs.HttpExample.Disabled": "true"
  }
}
``` 

## <a name="other-methods"></a>Andere Methoden

Obwohl die Methode der Anwendungseinstellung für alle Sprachen und alle Runtimeversionen empfohlen wird, gibt es verschiedene weitere Möglichkeiten, Funktionen zu deaktivieren. Diese Methoden, die je nach Sprache und Runtimeversion variieren, werden aus Gründen der Abwärtskompatibilität beibehalten. 

### <a name="c-class-libraries"></a>C#-Klassenbibliotheken

In einer Klassenbibliotheksfunktion können Sie auch das `Disable`-Attribut verwenden, um das Auslösen der Funktion zu verhindern. Sie können das Attribut wie im folgenden Beispiel gezeigt ohne einen Konstruktorparameter verwenden:

```csharp
public static class QueueFunctions
{
    [Disable]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Wenn Sie das Attribut ohne Konstruktorparameter verwenden, müssen Sie das Projekt erneut kompilieren und erneut bereitstellen, um den Zustand „Deaktiviert“ der Funktion zu ändern. Eine flexiblere Möglichkeit zur Verwendung des Attributs besteht darin, wie im folgenden Beispiel gezeigt einen Konstruktorparameter hinzuzufügen, der auf eine boolesche App-Einstellung verweist:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Mit dieser Methode können Sie die Funktion durch Ändern der App-Einstellung ohne erneutes Kompilieren oder erneutes Bereitstellen aktivieren und deaktivieren. Das Ändern einer App-Einstellung bewirkt einen Neustart der Funktions-App, sodass die Änderung des Zustands „Deaktiviert“ sofort erkannt wird.

> [!IMPORTANT]
> Das `Disabled`-Attribut ist die einzige Möglichkeit zum Deaktivieren einer Klassenbibliotheksfunktion. Die generierte Datei *function.json* für eine Klassenbibliotheksfunktion ist nicht zur direkten Bearbeitung vorgesehen. Wenn Sie diese Datei bearbeiten, haben Änderungen, die Sie an der `disabled`-Eigenschaft vornehmen, keinerlei Auswirkungen.
>
> Das Gleiche gilt für die Option **Funktionszustand** auf der Registerkarte **Verwalten**, da diese die Datei *function.json* ändert.
>
> Beachten Sie außerdem, dass die Funktion im Portal als deaktiviert angezeigt werden kann, wenn dies nicht der Fall ist.

### <a name="functions-1x---scripting-languages"></a>Functions 1.x – Skriptsprachen

In Version 1.x können Sie auch die `disabled`-Eigenschaft in der Datei *function.json* verwenden, um der Runtime mitzuteilen, dass keine Funktion ausgelöst werden soll. Diese Methode funktioniert nur bei Skriptsprachen wie C#-Skripten und JavaScript. Die `disabled`-Eigenschaft kann auf `true` oder auf den Namen einer App-Einstellung festgelegt werden:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
oder 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

Im zweiten Beispiel wird die Funktion deaktiviert, wenn eine App-Einstellung mit dem Namen „IS_DISABLED“ vorhanden und auf `true` oder 1 festgelegt ist.

>[!IMPORTANT]  
>Das Portal verwendet nun Anwendungseinstellungen, um v1.x-Funktionen zu deaktivieren. Wenn eine Anwendungseinstellung mit der Datei „function.json“ in Konflikt steht, kann ein Fehler auftreten. Sie sollten die Eigenschaft `disabled` aus der Datei „function.json“ entfernen, um Fehler zu verhindern. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird das Deaktivieren von automatischen Triggern behandelt. Weitere Informationen zu Triggern finden Sie unter [Trigger und Bindungen](functions-triggers-bindings.md).
