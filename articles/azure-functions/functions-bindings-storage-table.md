---
title: Azure Table Storage-Bindungen für Azure Functions
description: Hier erfahren Sie, wie Sie Azure Table Storage-Bindungen in Azure Functions verwenden.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: fd50e649257ba8849f49cd3aae85e3228b9eb94c
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88612213"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Table Storage-Bindungen für Azure Functions

In diesem Artikel erfahren Sie, wie Sie Azure Table Storage-Bindungen in Azure Functions verwenden. Azure Functions unterstützt Eingabe- und Ausgabebindungen für Azure-Tabellenspeicher.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Die Tabellenspeicherbindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), Version 2.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table).

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Pakete: Functions 2.x oder höher

Die Tabellenspeicherbindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage), Version 3.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Eingabe

Verwenden Sie die Azure Table Storage-Eingabebindung, um eine Tabelle in einem Azure Storage-Konto zu lesen.

# <a name="c"></a>[C#](#tab/csharp)

### <a name="one-entity"></a>Eine Entität

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md) zum Lesen einer einzelnen Tabellenzeile. Die Funktion wird für jede an die Warteschlange gesendete Nachricht ausgelöst.

Der Zeilenschlüsselwert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="iqueryable"></a>IQueryable

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die mehrere Tabellenzeilen liest, wobei die `MyPoco`-Klasse von `TableEntity` abgeleitet wird.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="cloudtable"></a>CloudTable

`IQueryable` wird in der [Laufzeit von Functions v2](functions-versions.md) nicht unterstützt. Als Alternative kann beispielsweise ein `CloudTable`-Methodenparameter verwendet werden, um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Hier finden Sie ein Beispiel einer Funktion, die eine Azure Functions-Protokolltabelle abfragt:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Cosmos.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

Weitere Informationen zur Verwendung von „CloudTable“ finden Sie unter [Erste Schritte mit Azure Table Storage](../cosmos-db/tutorial-develop-table-dotnet.md).

Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

### <a name="one-entity"></a>Eine Entität

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile. 

Die Datei *function.json* gibt jeweils einen Wert für `partitionKey` und `rowKey` zurück. Der `rowKey`-Wert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="iqueryable"></a>IQueryable

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion liest Entitäten für einen Partitionsschlüssel, der in einer Warteschlangenmeldung angegeben ist.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der C#-Skriptcode fügt einen Verweis auf das Azure Storage SDK hinzu, sodass der Entitätstyp von `TableEntity` abgeleitet werden kann:

```csharp
#r "Microsoft.Azure.Cosmos"
using Microsoft.Azure.Cosmos.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="cloudtable"></a>CloudTable

`IQueryable` wird in der Functions-Runtime für [Version 2.x und höhere Versionen](functions-versions.md) nicht unterstützt. Als Alternative kann beispielsweise ein `CloudTable`-Methodenparameter verwendet werden, um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Hier finden Sie ein Beispiel einer Funktion, die eine Azure Functions-Protokolltabelle abfragt:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.Azure.Cosmos"
using Microsoft.Azure.Cosmos.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

Weitere Informationen zur Verwendung von „CloudTable“ finden Sie unter [Erste Schritte mit Azure Table Storage](../cosmos-db/tutorial-develop-table-dotnet.md).

Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.


# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine Tabelleneingabebindung in einer Datei vom Typ *function.json* sowie [JavaScript-Code](functions-reference-node.md), der die Bindung verwendet. Die Funktion verwendet einen Warteschlangentrigger zum Lesen einer einzelnen Tabellenzeile. 

Die Datei *function.json* gibt jeweils einen Wert für `partitionKey` und `rowKey` zurück. Der `rowKey`-Wert „{queueTrigger}“ gibt an, dass der Zeilenschlüssel aus der Zeichenfolge der Warteschlangennachricht stammt.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Einzelne Tabellenzeile 

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "messageJSON",
      "type": "table",
      "tableName": "messages",
      "partitionKey": "message",
      "rowKey": "{id}",
      "connection": "AzureWebJobsStorage",
      "direction": "in"
    },
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ],
      "route": "messages/{id}"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ],
  "disabled": false
}
```

```python
import json

import azure.functions as func

def main(req: func.HttpRequest, messageJSON) -> func.HttpResponse:

    message = json.loads(messageJSON)
    return func.HttpResponse(f"Table row: {messageJSON}")
```

# <a name="java"></a>[Java](#tab/java)

Das folgende Beispiel zeigt eine durch HTTP ausgelöste Funktion, die eine Liste mit Personenobjekte in einer angegebenen Partition in Table Storage zurückgibt. Bei diesem Beispiel wird der Partitionsschlüssel aus der HTTP-Route extrahiert, und „tableName“ und „connection“ stammen aus den Funktionseinstellungen. 

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() { return this.PartitionKey; }
    public void setPartitionKey(String key) { this.PartitionKey = key; }
    public String getRowKey() { return this.RowKey; }
    public void setRowKey(String key) { this.RowKey = key; }
    public String getName() { return this.Name; }
    public void setName(String name) { this.Name = name; }
}

@FunctionName("getPersonsByPartitionKey")
public Person[] get(
        @HttpTrigger(name = "getPersons", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="persons/{partitionKey}") HttpRequestMessage<Optional<String>> request,
        @BindingName("partitionKey") String partitionKey,
        @TableInput(name="persons", partitionKey="{partitionKey}", tableName="%MyTableName%", connection="MyConnectionString") Person[] persons,
        final ExecutionContext context) {

    context.getLogger().info("Got query for person related to persons with partition key: " + partitionKey);

    return persons;
}
```

Die Anmerkung TableInput kann auch, wie im folgenden Beispiel gezeigt, die Bindungen aus dem JSON-Text der Anforderung extrahieren.

```java
@FunctionName("GetPersonsByKeysFromRequest")
public HttpResponseMessage get(
        @HttpTrigger(name = "getPerson", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="query") HttpRequestMessage<Optional<String>> request,
        @TableInput(name="persons", partitionKey="{partitionKey}", rowKey = "{rowKey}", tableName="%MyTableName%", connection="MyConnectionString") Person person,
        final ExecutionContext context) {

    if (person == null) {
        return request.createResponseBuilder(HttpStatus.NOT_FOUND)
                    .body("Person not found.")
                    .build();
    }

    return request.createResponseBuilder(HttpStatus.OK)
                    .header("Content-Type", "application/json")
                    .body(person)
                    .build();
}
```

Das folgende Beispiel verwendet den Filter zum Abfragen von Personen mit einem bestimmten Namen in einer Azure-Tabelle und begrenzt die Anzahl der möglichen Treffer auf 10 Ergebnisse.

```java
@FunctionName("getPersonsByName")
public Person[] get(
        @HttpTrigger(name = "getPersons", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="filter/{name}") HttpRequestMessage<Optional<String>> request,
        @BindingName("name") String name,
        @TableInput(name="persons", filter="Name eq '{name}'", take = "10", tableName="%MyTableName%", connection="MyConnectionString") Person[] persons,
        final ExecutionContext context) {

    context.getLogger().info("Got query for person related to persons with name: " + name);

    return persons;
}
```

---

## <a name="input---attributes-and-annotations"></a>Eingabe – Attribute und Anmerkungen

# <a name="c"></a>[C#](#tab/csharp)

 Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) die folgenden Attribute, um eine Tabelleneingabebindung zu konfigurieren:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  Der Konstruktor des Attributs akzeptiert den Tabellennamen, Partitionsschlüssel und Zeilenschlüssel. Das Attribut kann für einen `out`-Parameter oder für den Rückgabewert der Funktion verwendet werden, wie im folgenden Beispiel zu sehen:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Ein vollständiges Beispiel finden Sie unter „Eingabe: C#-Beispiel“.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Eine weitere Möglichkeit zum Angeben des zu verwendenden Speicherkontos. Der Konstruktor akzeptiert den Namen einer App-Einstellung mit einer Speicherverbindungszeichenfolge. Das Attribut kann auf Parameter-, Methoden- oder Klassenebene angewendet werden. Das folgende Beispiel zeigt die Anwendung auf Klassen- und Methodenebene:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Das zu verwendende Speicherkonto wird anhand von Folgendem bestimmt (in der angegebenen Reihenfolge):

* Die Eigenschaft `Connection` des Attributs `Table`.
* Das Attribut `StorageAccount`, das auf den gleichen Parameter angewendet wird wie das Attribut `Table`.
* Das Attribut `StorageAccount`, das auf die Funktion angewendet wird.
* Das Attribut `StorageAccount`, das auf die Klasse angewendet wird.
* Das Standardspeicherkonto für die Funktions-App (App-Einstellung „AzureWebJobsStorage“).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

Verwenden Sie die `@TableInput`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Parameter, deren Wert von Table Storage empfangen wird.  Diese Anmerkung kann mit nativen Java-Typen, POJOs oder Werten mit `Optional<T>`, die NULL-Werte annehmen können, verwendet werden.

---

## <a name="input---configuration"></a>Eingabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Table` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf `table` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen.|
|**direction** | – | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die die Tabelle oder Entität im Funktionscode darstellt. | 
|**tableName** | **TableName** | Der Name der Tabelle.| 
|**partitionKey** | **PartitionKey** |Optional. Der Partitionsschlüssel der zu lesenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**rowKey** |**Zeilenschlüssel** | Optional. Der Zeilenschlüssel der zu lesenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**take** |**Take** | Optional. Die maximale Anzahl von Entitäten, die in JavaScript gelesen werden sollen. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**filter** |**Filter** | Optional. Ein OData-Filterausdruck für die Tabelleneingabe in JavaScript. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#input---usage).| 
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Bei dieser Einstellung kann es sich um den Namen einer App-Einstellung mit dem Präfix „AzureWebJobs“ oder den Namen einer Verbindungszeichenfolge handeln. Wenn der Name Ihrer Einstellung beispielsweise „AzureWebJobsMyStorage“ lautet, können Sie hier „MyStorage“ angeben. Die Functions-Laufzeit sucht automatisch nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Eingabe: Verwendung

# <a name="c"></a>[C#](#tab/csharp)

* **Lesen einer einzelnen Zeile in**

  Legen Sie `partitionKey` und `rowKey` fest. Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `T <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` ist üblicherweise ein Typ, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.

* **Eine oder mehrere Zeilenlesen**

  Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `IQueryable<T> <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Bei `T` muss es sich um einen Typ handeln, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Für ggf. erforderliche Filtervorgänge können Sie `IQueryable`-Methoden verwenden. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.  

  > [!NOTE]
  > `IQueryable` wird in der [Laufzeit von Functions v2](functions-versions.md) nicht unterstützt. Als Alternative kann beispielsweise ein [CloudTable paramName-Methodenparameter verwendet werden](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable), um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

* **Lesen einer einzelnen Zeile in**

  Legen Sie `partitionKey` und `rowKey` fest. Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `T <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` ist üblicherweise ein Typ, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.

* **Eine oder mehrere Zeilenlesen**

  Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `IQueryable<T> <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Bei `T` muss es sich um einen Typ handeln, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Für ggf. erforderliche Filtervorgänge können Sie `IQueryable`-Methoden verwenden. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.  

  > [!NOTE]
  > `IQueryable` wird in der [Laufzeit von Functions v2](functions-versions.md) nicht unterstützt. Als Alternative kann beispielsweise ein [CloudTable paramName-Methodenparameter verwendet werden](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable), um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Legen Sie die Eigenschaften `filter` und `take` fest. Legen Sie `partitionKey` oder `rowKey` nicht fest. Verwenden Sie `context.bindings.<BINDING_NAME>`, um auf die Eingabetabellenentität(en) zuzugreifen. Die deserialisierten Objekte verfügen über die Eigenschaften `RowKey` und `PartitionKey`.

# <a name="python"></a>[Python](#tab/python)

Tabellendaten werden als JSON-Zeichenfolge an die Funktion übergeben. Deserialisieren Sie die Nachricht durch Aufrufen von `json.loads`, wie im Eingabe[beispiel](#input) gezeigt.

# <a name="java"></a>[Java](#tab/java)

Das [TableInput](/java/api/com.microsoft.azure.functions.annotation.tableinput)-Attribut gewährt Ihnen Zugriff auf die Tabellenzeile, die die Funktion ausgelöst hat.

---

## <a name="output"></a>Output

Verwenden Sie eine Azure Table Storage-Ausgabebindung, um Entitäten in eine Tabelle in einem Azure Storage-Konto zu schreiben.

> [!NOTE]
> Das Aktualisieren vorhandener Entitäten wird von dieser Ausgabebindung nicht unterstützt. Verwenden Sie den `TableOperation.Replace`-Vorgang [aus dem Azure Storage-SDK](../cosmos-db/tutorial-develop-table-dotnet.md#delete-an-entity) zum Aktualisieren einer vorhandenen Entität.

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md) mit einem HTTP-Trigger zum Schreiben einer einzelnen Tabellenzeile. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine Tabellenausgabebindung in einer Datei vom Typ *function.json* sowie [C#-Skriptcode](functions-reference-csharp.md), der die Bindung verwendet. Die Funktion schreibt mehrere Tabellenentitäten.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine Tabellenausgabebindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion schreibt mehrere Tabellenentitäten.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Das folgende Beispiel zeigt, wie Sie die Tabellenspeicherausgabe-Bindung verwenden. Die `table`-Bindung ist in *function.json* konfiguriert, indem `name`, `tableName`, `partitionKey` und `connection` Werte zugewiesen werden:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "message",
      "type": "table",
      "tableName": "messages",
      "partitionKey": "message",
      "connection": "AzureWebJobsStorage",
      "direction": "out"
    },
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

Die folgende Funktion generiert eine eindeutige UUI für den `rowKey`-Wert und speichert die Nachricht dauerhaft im Tabellenspeicher.

```python
import logging
import uuid
import json

import azure.functions as func

def main(req: func.HttpRequest, message: func.Out[str]) -> func.HttpResponse:

    rowKey = str(uuid.uuid4())

    data = {
        "Name": "Output binding message",
        "PartitionKey": "message",
        "RowKey": rowKey
    }

    message.set(json.dumps(data))

    return func.HttpResponse(f"Message created with the rowKey: {rowKey}")
```

# <a name="java"></a>[Java](#tab/java)

Das folgende Beispiel zeigt eine Java-Funktion, die einen HTTP-Trigger zum Schreiben einer einzelnen Tabellenzeile verwendet.

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() {return this.PartitionKey;}
    public void setPartitionKey(String key) {this.PartitionKey = key; }
    public String getRowKey() {return this.RowKey;}
    public void setRowKey(String key) {this.RowKey = key; }
    public String getName() {return this.Name;}
    public void setName(String name) {this.Name = name; }
}

public class AddPerson {

    @FunctionName("addPerson")
    public HttpResponseMessage get(
            @HttpTrigger(name = "postPerson", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION, route="persons/{partitionKey}/{rowKey}") HttpRequestMessage<Optional<Person>> request,
            @BindingName("partitionKey") String partitionKey,
            @BindingName("rowKey") String rowKey,
            @TableOutput(name="person", partitionKey="{partitionKey}", rowKey = "{rowKey}", tableName="%MyTableName%", connection="MyConnectionString") OutputBinding<Person> person,
            final ExecutionContext context) {

        Person outPerson = new Person();
        outPerson.setPartitionKey(partitionKey);
        outPerson.setRowKey(rowKey);
        outPerson.setName(request.getBody().get().getName());

        person.setValue(outPerson);

        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(outPerson)
                        .build();
    }
}
```

Das folgende Beispiel zeigt eine Java-Funktion, die einen HTTP-Trigger zum Schreiben mehrerer Tabellenzeilen verwendet.

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() {return this.PartitionKey;}
    public void setPartitionKey(String key) {this.PartitionKey = key; }
    public String getRowKey() {return this.RowKey;}
    public void setRowKey(String key) {this.RowKey = key; }
    public String getName() {return this.Name;}
    public void setName(String name) {this.Name = name; }
}

public class AddPersons {

    @FunctionName("addPersons")
    public HttpResponseMessage get(
            @HttpTrigger(name = "postPersons", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION, route="persons/") HttpRequestMessage<Optional<Person[]>> request,
            @TableOutput(name="person", tableName="%MyTableName%", connection="MyConnectionString") OutputBinding<Person[]> persons,
            final ExecutionContext context) {

        persons.setValue(request.getBody().get());

        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(request.getBody().get())
                        .build();
    }
}
```

---

## <a name="output---attributes-and-annotations"></a>Ausgabe – Attribute und Anmerkungen

# <a name="c"></a>[C#](#tab/csharp)

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie die [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs).

Der Konstruktor des Attributs akzeptiert den Tabellennamen. Das Attribut kann für einen `out`-Parameter oder für den Rückgabewert der Funktion verwendet werden, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Durch Festlegen der Eigenschaft `Connection` können Sie das zu verwendende Speicherkonto angeben, wie im folgenden Beispiel zu sehen:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Ausgabe: C#-Beispiel](#output).

Mit dem Attribut `StorageAccount` können Sie das Speicherkonto auf Klassen-, Methoden- oder Parameterebene angeben. Weitere Informationen finden Sie unter [Eingabe: Attribute](#input---attributes-and-annotations).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

Verwenden Sie in der [Java-Funktionslaufzeitbibliothek](/java/api/overview/azure/functions/runtime) die [TableOutput](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/TableOutput.java/)-Anmerkung bei Parametern, um Werte in den Tabellenspeicher zu schreiben.

Weitere Details finden Sie in dem [Beispiel](#output).

---

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Table` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf `table` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen.|
|**direction** | – | Muss auf `out` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Variablenname, der in Funktionscode zur Darstellung der Tabelle oder Entität verwendet wird. Legen Sie diesen Wert auf `$return` fest, um auf den Rückgabewert der Funktion zu verweisen.| 
|**tableName** |**TableName** | Der Name der Tabelle.| 
|**partitionKey** |**PartitionKey** | Der Partitionsschlüssel der zu schreibenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#output---usage).| 
|**rowKey** |**Zeilenschlüssel** | Der Zeilenschlüssel der zu schreibenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#output---usage).| 
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie hier nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyStorage“ festlegen, sucht die Functions-Runtime nach einer App-Einstellung namens „MyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Ausgabe: Verwendung

# <a name="c"></a>[C#](#tab/csharp)

Greifen Sie mithilfe eines der Methodenparameter `ICollector<T> paramName` oder `IAsyncCollector<T> paramName` auf die Ausgabetabellenentität zu, wobei `T` die Eigenschaften `PartitionKey` und `RowKey` umfasst. Diese Eigenschaften gehen häufig mit der Implementierung von `ITableEntity` oder der Vererbung von `TableEntity` einher.

Alternativ können Sie einen `CloudTable`-Methodenparameter verwenden, um unter Verwendung des Azure Storage SDKs in die Tabelle zu schreiben. Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Greifen Sie mithilfe eines der Methodenparameter `ICollector<T> paramName` oder `IAsyncCollector<T> paramName` auf die Ausgabetabellenentität zu, wobei `T` die Eigenschaften `PartitionKey` und `RowKey` umfasst. Diese Eigenschaften gehen häufig mit der Implementierung von `ITableEntity` oder der Vererbung von `TableEntity` einher. Der Wert `paramName` wird in der Eigenschaft `name` von *function.json* angegeben.

Alternativ können Sie einen `CloudTable`-Methodenparameter verwenden, um unter Verwendung des Azure Storage SDKs in die Tabelle zu schreiben. Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Greifen Sie mithilfe von `context.bindings.<name>` auf das Ausgabeereignis zu, wobei `<name>` der in der Eigenschaft `name` von *function.json* angegebene Wert ist.

# <a name="python"></a>[Python](#tab/python)

Es gibt zwei Optionen für das Ausgeben einer Tabellenspeicherzeilen-Nachricht aus einer Funktion:

- **Rückgabewert**: Legen Sie die Eigenschaft `name` in *function.json* auf `$return` fest. Mit dieser Konfiguration wird der Rückgabewert der Funktion als Tabellenspeicherzeile beibehalten.

- **Imperativ**: Übergeben Sie einen Wert an die [set](/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none)-Methode des Parameters, der als [Out](/python/api/azure-functions/azure.functions.out?view=azure-python)-Typ deklariert ist. Der an `set` übergebene Wert wird als Event Hub-Nachricht beibehalten.

# <a name="java"></a>[Java](#tab/java)

Es gibt zwei Optionen für das Ausgeben einer Tabellenspeicherzeile aus einer Funktion mittels der [TableStorageOutput](/java/api/com.microsoft.azure.functions.annotation.tableoutput?view=azure-java-stablet)-Anmerkung:

- **Rückgabewert**: Wenn Sie die Anmerkung auf die Funktion selbst anwenden, wird der Rückgabewert der Funktion als Tabellenspeicherzeile beibehalten.

- **Imperativ**: Um den Nachrichtenwert explizit festzulegen, wenden Sie die Anmerkung auf einen bestimmten Parameter des Typs [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) an, wobei `T` die Eigenschaften `PartitionKey` und `RowKey` enthält. Diese Eigenschaften gehen häufig mit der Implementierung von `ITableEntity` oder der Vererbung von `TableEntity` einher.

---

## <a name="exceptions-and-return-codes"></a>Ausnahmen und Rückgabecodes

| Bindung | Verweis |
|---|---|
| Tabelle | [Tabellenfehlercodes](/rest/api/storageservices/fileservices/table-service-error-codes) |
| Blob, Tabelle, Warteschlange | [Speicherfehlercodes](/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, Tabelle, Warteschlange | [Problembehandlung](/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
