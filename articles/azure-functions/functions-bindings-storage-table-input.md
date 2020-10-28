---
title: Azure Table Storage-Eingabebindungen für Azure Functions
description: Hier erfahren Sie, wie Sie Azure Table Storage-Eingabebindungen in Azure Functions verwenden.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 7f5db2a2df7314c89f2ebba8e7e54ebe24126386
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92098153"
---
# <a name="azure-table-storage-input-bindings-for-azure-functions"></a>Azure Table Storage-Eingabebindungen für Azure Functions

Verwenden Sie die Azure Table Storage-Eingabebindung, um eine Tabelle in einem Azure Storage-Konto zu lesen.

## <a name="example"></a>Beispiel

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

### <a name="cloudtable"></a>CloudTable

`CloudTable` wird nur in der [Functions v2 Runtime oder höher](functions-versions.md) unterstützt.

Verwenden Sie einen `CloudTable`-Methodenparameter, um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Hier finden Sie ein Beispiel einer Funktion, die eine Azure Functions-Protokolltabelle abfragt:

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

Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x) vorliegt.

### <a name="iqueryable"></a>IQueryable

`IQueryable` wird nur in der [Functions v1 Runtime](functions-versions.md) unterstützt.

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

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

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
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
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

Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x) vorliegt.

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

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

Der C#-Skriptcode fügt einen Verweis auf das Azure Storage SDK hinzu, sodass der Entitätstyp von `TableEntity` abgeleitet werden kann:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
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

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

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

## <a name="attributes-and-annotations"></a>Attribute und Anmerkungen

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

  Ein vollständiges Beispiel finden Sie unter „C#-Beispiel“.

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

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `Table` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf `table` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen.|
|**direction** | – | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie die Bindung im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die die Tabelle oder Entität im Funktionscode darstellt. | 
|**tableName** | **TableName** | Der Name der Tabelle.| 
|**partitionKey** | **PartitionKey** |Optional. Der Partitionsschlüssel der zu lesenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#usage).| 
|**rowKey** |**Zeilenschlüssel** | Optional. Der Zeilenschlüssel der zu lesenden Tabellenentität. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#usage).| 
|**take** |**Take** | Optional. Die maximale Anzahl von Entitäten, die in JavaScript gelesen werden sollen. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#usage).| 
|**filter** |**Filter** | Optional. Ein OData-Filterausdruck für die Tabelleneingabe in JavaScript. Informationen zur Verwendung dieser Eigenschaft finden Sie im Abschnitt [Verwendung](#usage).| 
|**connection** |**Connection** | Der Name einer App-Einstellung, die die Storage-Verbindungszeichenfolge für diese Bindung enthält. Bei dieser Einstellung kann es sich um den Namen einer App-Einstellung mit dem Präfix „AzureWebJobs“ oder den Namen einer Verbindungszeichenfolge handeln. Wenn der Name Ihrer Einstellung beispielsweise „AzureWebJobsMyStorage“ lautet, können Sie hier „MyStorage“ angeben. Die Functions-Laufzeit sucht automatisch nach einer App-Einstellung namens „AzureWebJobsMyStorage“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Storage-Verbindungszeichenfolge aus der App-Einstellung `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Verwendung

# <a name="c"></a>[C#](#tab/csharp)

* **Lesen einer einzelnen Zeile in**

  Legen Sie `partitionKey` und `rowKey` fest. Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `T <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` ist üblicherweise ein Typ, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.

* **Eine oder mehrere Zeilenlesen**

  Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `IQueryable<T> <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Bei `T` muss es sich um einen Typ handeln, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Für ggf. erforderliche Filtervorgänge können Sie `IQueryable`-Methoden verwenden. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.  

  > [!NOTE]
  > `IQueryable` wird in der [Laufzeit von Functions v2](functions-versions.md) nicht unterstützt. Als Alternative kann beispielsweise ein [CloudTable paramName-Methodenparameter verwendet werden](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable), um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

* **Lesen einer einzelnen Zeile in**

  Legen Sie `partitionKey` und `rowKey` fest. Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `T <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. `T` ist üblicherweise ein Typ, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Die Eigenschaften `filter` und `take` werden in diesem Szenario nicht verwendet.

* **Eine oder mehrere Zeilenlesen**

  Verwenden Sie für den Zugriff auf die Tabellendaten einen Methodenparameter vom Typ `IQueryable<T> <paramName>`. In C#-Skripts ist `paramName` der Wert, der in der Eigenschaft `name` von *function.json* angegeben ist. Bei `T` muss es sich um einen Typ handeln, der `ITableEntity` implementiert oder von `TableEntity` abgeleitet wird. Für ggf. erforderliche Filtervorgänge können Sie `IQueryable`-Methoden verwenden. Die Eigenschaften `partitionKey`, `rowKey`, `filter` und `take` werden in diesem Szenario nicht verwendet.  

  > [!NOTE]
  > `IQueryable` wird in der [Laufzeit von Functions v2](functions-versions.md) nicht unterstützt. Als Alternative kann beispielsweise ein [CloudTable paramName-Methodenparameter verwendet werden](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable), um die Tabelle unter Verwendung des Azure Storage SDKs zu lesen. Wenn Sie versuchen, eine Bindung an `CloudTable` herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x) vorliegt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Legen Sie die Eigenschaften `filter` und `take` fest. Legen Sie `partitionKey` oder `rowKey` nicht fest. Verwenden Sie `context.bindings.<BINDING_NAME>`, um auf die Eingabetabellenentität(en) zuzugreifen. Die deserialisierten Objekte verfügen über die Eigenschaften `RowKey` und `PartitionKey`.

# <a name="python"></a>[Python](#tab/python)

Tabellendaten werden als JSON-Zeichenfolge an die Funktion übergeben. Deserialisieren Sie die Nachricht durch Aufrufen von `json.loads`, wie im Eingabe[beispiel](#example) gezeigt.

# <a name="java"></a>[Java](#tab/java)

Das [TableInput](/java/api/com.microsoft.azure.functions.annotation.tableinput)-Attribut gewährt Ihnen Zugriff auf die Tabellenzeile, die die Funktion ausgelöst hat.

---

## <a name="next-steps"></a>Nächste Schritte

* [Schreiben von Tabellenspeicherdaten aus einer Funktion](./functions-bindings-storage-table-output.md)
