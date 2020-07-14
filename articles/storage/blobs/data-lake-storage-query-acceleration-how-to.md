---
title: Filtern von Daten mithilfe der Abfragebeschleunigung für Azure Data Lake Storage (Vorschau) | Microsoft-Dokumentation
description: Verwenden Sie die Abfragebeschleunigung (Vorschau), um eine Teilmenge von Daten aus Ihrem Speicherkonto abzurufen.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 04/21/2020
ms.author: normesta
ms.reviewer: jamsbak
ms.openlocfilehash: cc9235f07c0829abfb8be42e83d05d8428bc1806
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84465863"
---
# <a name="filter-data-by-using-azure-data-lake-storage-query-acceleration-preview"></a>Filtern von Daten mithilfe der Abfragebeschleunigung für Azure Data Lake Storage (Vorschau)

In diesem Artikel wird gezeigt, wie Sie mithilfe der Abfragebeschleunigung (Vorschau) eine Teilmenge von Daten aus Ihrem Speicherkonto abrufen. 

Die Abfragebeschleunigung (Vorschau) ist eine neue Funktion für Azure Data Lake Storage, die es Anwendungen und Analyseframeworks ermöglicht, die Datenverarbeitung drastisch zu optimieren. Dabei werden nur die Daten abgerufen, die für einen bestimmten Vorgang erforderlich sind. Weitere Informationen finden Sie unter [Abfragebeschleunigung für Azure Data Lake Storage (Vorschau)](data-lake-storage-query-acceleration.md).

> [!NOTE]
> Die Abfragebeschleunigung ist als öffentliche Vorschau in den Regionen „Kanada, Mitte“ und „Frankreich, Mitte“ verfügbar. Informationen zu den Einschränkungen finden Sie im Artikel [Bekannte Probleme](data-lake-storage-known-issues.md). Wie Sie sich für die Vorschau registrieren, erfahren Sie in [diesem Formular](https://aka.ms/adls/qa-preview-signup).  

## <a name="prerequisites"></a>Voraussetzungen

### <a name="net"></a>[.NET](#tab/dotnet)

- Sie benötigen ein Azure-Abonnement, um auf Azure Storage zuzugreifen. Wenn Sie noch kein Abonnement haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Ein Speicherkonto vom Typ **Allgemein V2**. Informationen finden Sie unter [Erstellen eines Speicherkontos](../common/storage-quickstart-create-account.md).

- [.NET SDK](https://dotnet.microsoft.com/download). 

### <a name="java"></a>[Java](#tab/java)

- Sie benötigen ein Azure-Abonnement, um auf Azure Storage zuzugreifen. Wenn Sie noch kein Abonnement haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Ein Speicherkonto vom Typ **Allgemein V2**. Informationen finden Sie unter [Erstellen eines Speicherkontos](../common/storage-quickstart-create-account.md).

- [Java Development Kit (JDK)](/java/azure/jdk/?view=azure-java-stable), Version 8 oder höher.

- [Apache Maven](https://maven.apache.org/download.cgi). 

  > [!NOTE] 
  > In diesem Artikel wird davon ausgegangen, dass Sie ein Java-Projekt mithilfe von Apache Maven erstellt haben. Ein Beispiel zum Erstellen eines Projekts mithilfe von Apache Maven finden Sie unter [Einrichten](storage-quickstart-blobs-java.md#setting-up).
  
---

## <a name="install-packages"></a>Installieren von Paketen 

### <a name="net"></a>[.NET](#tab/dotnet)

1. Laden Sie die Pakete für die Abfragebeschleunigung herunter. Sie können eine komprimierte ZIP-Datei mit diesen Paketen über den folgenden Link abrufen: [https://aka.ms/adls/qqsdk/.net](https://aka.ms/adls/qqsdk/.net) 

2. Extrahieren Sie den Inhalt dieser Datei in Ihr Projektverzeichnis.

3. Öffnen Sie die Projektdatei ( *.csproj*) in einem Text-Editor, und fügen Sie die folgenden Paketverweise innerhalb des Elements \<Project\> hinzu.

   ```xml
   <ItemGroup>
       <PackageReference Include="Azure.Storage.Blobs" Version="12.5.0-preview.1" />
       <PackageReference Include="Azure.Storage.Common" Version="12.4.0-preview.1" />
       <PackageReference Include="Azure.Storage.QuickQuery" Version="12.0.0-preview.1" />
   </ItemGroup>
   ```

4. Stellen Sie die SDK-Vorschaupakete wieder her. In diesem Beispiel werden die SDK-Vorschaupakete mit dem Befehl `dotnet restore` wiederhergestellt. 

   ```console
   dotnet restore --source C:\Users\contoso\myProject
   ```

5. Stellen Sie alle anderen Abhängigkeiten aus dem öffentlichen NuGet-Repository wieder her.

   ```console
   dotnet restore
   ```

### <a name="java"></a>[Java](#tab/java)

1. Erstellen Sie ein Verzeichnis im Stammverzeichnis Ihres Projekts. Das Stammverzeichnis ist das Verzeichnis, das die Datei **pom.xml** enthält.

   > [!NOTE]
   > Bei den Beispielen in diesem Artikel wird davon ausgegangen, dass der Name des Verzeichnisses **lib** lautet.

2. Laden Sie die Pakete für die Abfragebeschleunigung herunter. Sie können eine komprimierte ZIP-Datei mit diesen Paketen über den folgenden Link abrufen: [https://aka.ms/adls/qqsdk/java](https://aka.ms/adls/qqsdk/java) 

3. Extrahieren Sie die Dateien in dieser ZIP-Datei in das von Ihnen erstellte Verzeichnis. In diesem Beispiel hat das Verzeichnis den Namen **lib**. 

4. Öffnen Sie die Datei *pom.xml* in Ihrem Text-Editor. Fügen Sie der Gruppe der Abhängigkeiten das folgende Abhängigkeitselement hinzu: 

   ```xml
   <!-- Request static dependencies from Maven -->
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-core</artifactId>
       <version>1.3.0</version>
   </dependency>
   <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-core-http-netty</artifactId>
      <version>1.3.0</version>
   </dependency>
   <dependency>
      <groupId>org.apache.avro</groupId>
      <artifactId>avro</artifactId>
      <version>1.9.2</version>
   </dependency>
   <dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.8</version>
   </dependency>
   <!-- Local dependencies -->
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-storage-blob</artifactId>
       <version>12.5.0-beta.1</version>
       <scope>system</scope>
       <systemPath>${project.basedir}/lib/azure-storage-blob-12.5.0-beta.1.jar</systemPath>
   </dependency>
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-storage-common</artifactId>
       <version>12.5.0-beta.1</version>
       <scope>system</scope>
       <systemPath>${project.basedir}/lib/azure-storage-common-12.5.0-beta.1.jar</systemPath>
   </dependency>
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-storage-quickquery</artifactId>
       <version>12.0.0-beta.1</version>
       <scope>system</scope>
       <systemPath>${project.basedir}/lib/azure-storage-quickquery-12.0.0-beta.1.jar</systemPath>
   </dependency>
   ```

---

## <a name="add-statements"></a>Hinzufügen von Anweisungen


### <a name="net"></a>[.NET](#tab/dotnet)

Fügen Sie die folgenden `using`-Anweisungen am Anfang Ihrer Codedatei hinzu.

```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using Azure.Storage.Blobs.Specialized;
using Azure.Storage.QuickQuery;
using Azure.Storage.QuickQuery.Models;
```

Bei der Abfragebeschleunigung werden CSV- und JSON-formatierte Daten abgerufen. Stellen Sie deshalb sicher, dass Sie die using-Anweisungen für alle CSV- oder JSON-Analysebibliotheken hinzufügen, die Sie verwenden möchten. In den Beispielen in diesem Artikel wird eine CSV-Datei mithilfe der Bibliothek [CsvHelper](https://www.nuget.org/packages/CsvHelper/) analysiert, die auf NuGet verfügbar ist. Daher werden die folgenden `using`-Anweisungen am Anfang der Codedatei hinzugefügt.

```csharp
using CsvHelper;
using CsvHelper.Configuration;
```

Zum Kompilieren der Beispiele, die in diesem Artikel gezeigt werden, müssen Sie auch die folgenden `using`-Anweisungen hinzufügen.

```csharp
using System.Threading.Tasks;
using System.IO;
using System.Globalization;
using System.Threading;
using System.Linq;
```

### <a name="java"></a>[Java](#tab/java)

Fügen Sie die folgenden `import`-Anweisungen am Anfang Ihrer Codedatei hinzu.

```java
import com.azure.storage.blob.*;
import com.azure.storage.blob.models.*;
import com.azure.storage.common.*;
import com.azure.storage.quickquery.*;
import com.azure.storage.quickquery.models.*;
import java.io.*;
import org.apache.commons.csv.*;
```

---

## <a name="retrieve-data-by-using-a-filter"></a>Abrufen von Daten mithilfe eines Filters

Sie können SQL verwenden, um die Zeilenfilterprädikate und Spaltenprojektionen in einer Abfragebeschleunigungsanforderung anzugeben. Der folgende Code fragt eine CSV-Datei im Speicher ab und gibt alle Datenzeilen zurück, bei denen die dritte Spalte mit dem Wert `Hemingway, Ernest` übereinstimmt. 

- In der SQL-Abfrage wird das Schlüsselwort `BlobStorage` verwendet, um die abzufragende Datei anzugeben.

- Spaltenverweise werden als `_N` angegeben, wobei die erste Spalte `_1` ist. Wenn die Quelldatei eine Kopfzeile enthält, können Sie mit dem Namen, der in der Kopfzeile angegeben ist, auf Spalten verweisen. 

### <a name="net"></a>[.NET](#tab/dotnet)

Die asynchrone Methode `BlobQuickQueryClient.QueryAsync` sendet die Abfrage an die Abfragebeschleunigungs-API und streamt dann die Ergebnisse als [Stream](https://docs.microsoft.com/dotnet/api/system.io.stream?view=netframework-4.8)-Objekt an die Anwendung zurück.

```cs
static async Task QueryHemingway(BlockBlobClient blob)
{
    string query = @"SELECT * FROM BlobStorage WHERE _3 = 'Hemingway, Ernest'";
    await DumpQueryCsv(blob, query, false);
}

private static async Task DumpQueryCsv(BlockBlobClient blob, string query, bool headers)
{
    try
    {
        using (var reader = new StreamReader((await blob.GetQuickQueryClient().QueryAsync(query,
                new CsvTextConfiguration() { HasHeaders = headers }, 
                new CsvTextConfiguration() { HasHeaders = false }, 
                new ErrorHandler(),
                new BlobRequestConditions(), 
                new ProgressHandler(),
                CancellationToken.None)).Value.Content))
        {
            using (var parser = new CsvReader(reader, new CsvConfiguration(CultureInfo.CurrentCulture) 
            { HasHeaderRecord = false }))
            {
                while (await parser.ReadAsync())
                {
                    parser.Context.Record.All(cell =>
                    {
                        Console.Out.Write(cell + "  ");
                        return true;
                    });
                    Console.Out.WriteLine();
                }
            }
        }
    }
    catch (Exception ex)
    {
        Console.Error.WriteLine("Exception: " + ex.ToString());
    }
}

class ErrorHandler : IBlobQueryErrorReceiver
{
    public void ReportError(BlobQueryError err)
    {
        Console.Error.WriteLine($"Error: {err.Name}:{ err.Description }");
    }
}

class ProgressHandler : IProgress<long>
{
    public void Report(long value)
    {
        Console.Error.WriteLine("Bytes scanned: " + value.ToString());
    }
}

```

### <a name="java"></a>[Java](#tab/java)

Die Methode `BlobQuickQueryClient.openInputStream()` sendet die Abfrage an die Abfragebeschleunigungs-API und streamt dann die Ergebnisse als `InputStream`-Objekt, das wie jedes andere InputStream-Objekt gelesen werden kann, an die Anwendung zurück.

```java
static void QueryHemingway(BlobClient blobClient) {
    String expression = "SELECT * FROM BlobStorage WHERE _3 = 'Hemingway, Ernest'";
    DumpQueryCsv(blobClient, expression, false);
}

static void DumpQueryCsv(BlobClient blobClient, String query, Boolean headers) {
    try {
    
        BlobQuickQueryDelimitedSerialization input = new BlobQuickQueryDelimitedSerialization()
            .setRecordSeparator('\n')
            .setColumnSeparator(',')
            .setHeadersPresent(headers)
            .setFieldQuote('\0')
            .setEscapeChar('\\');

        BlobQuickQueryDelimitedSerialization output = new BlobQuickQueryDelimitedSerialization()
            .setRecordSeparator('\n')
            .setColumnSeparator(',')
            .setHeadersPresent(false)
            .setFieldQuote('\0')
            .setEscapeChar('\n');
                
        BlobRequestConditions requestConditions = null;
        /* ErrorReceiver determines what to do on errors. */
        ErrorReceiver<BlobQuickQueryError> errorReceiver = System.out::println;

        /* ProgressReceiver details how to log progress*/
        com.azure.storage.common.ProgressReceiver progressReceiver = System.out::println;
    
        /* Create a query acceleration client to the blob. */
        BlobQuickQueryClient qqClient = new BlobQuickQueryClientBuilder(blobClient)
            .buildClient();
        /* Open the query input stream. */
        InputStream stream = qqClient.openInputStream(query, input, output, requestConditions, errorReceiver, progressReceiver);
            
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(stream))) {
            /* Read from stream like you normally would. */
            for (CSVRecord record : CSVParser.parse(reader, CSVFormat.EXCEL)) {
                System.out.println(record.toString());
            }
        }
    } catch (Exception e) {
        System.err.println("Exception: " + e.toString());
    }
}
```

---

## <a name="retrieve-specific-columns"></a>Abrufen bestimmter Spalten

Sie können die Ergebnisse auf eine Teilmenge der Spalten beschränken. Auf diese Weise rufen Sie nur die Spalten ab, die zum Ausführen einer bestimmten Berechnung benötigt werden. Dadurch werden die Anwendungsleistung verbessert und Kosten reduziert, da weniger Daten über das Netzwerk übertragen werden. 

Dieser Code ruft nur die Spalte `PublicationYear` für alle Bücher im Dataset ab. Außerdem werden die Informationen aus der Kopfzeile in der Quelldatei verwendet, um auf Spalten in der Abfrage zu verweisen.


### <a name="net"></a>[.NET](#tab/dotnet)

```cs
static async Task QueryPublishDates(BlockBlobClient blob)
{
    string query = @"SELECT PublicationYear FROM BlobStorage";
    await DumpQueryCsv(blob, query, true);
}
```

### <a name="java"></a>[Java](#tab/java)

```java
static void QueryPublishDates(BlobClient blobClient)
{
    String expression = "SELECT PublicationYear FROM BlobStorage";
    DumpQueryCsv(blobClient, expression, true);
}
```

---

Im folgenden Code werden Zeilenfilterung und Spaltenprojektionen in derselben Abfrage kombiniert. 

### <a name="net"></a>[.NET](#tab/dotnet)

```cs
static async Task QueryMysteryBooks(BlockBlobClient blob)
{
    string query = @"SELECT BibNum, Title, Author, ISBN, Publisher FROM BlobStorage WHERE Subjects LIKE '%Mystery%'";
    await DumpQueryCsv(blob, query, true);
}
```

### <a name="java"></a>[Java](#tab/java)

```java
static void QueryMysteryBooks(BlobClient blobClient)
{
    String expression = "SELECT BibNum, Title, Author, ISBN, Publisher FROM BlobStorage WHERE Subjects LIKE '%Mystery%'";
    DumpQueryCsv(blobClient, expression, true);
}
```

---

## <a name="next-steps"></a>Nächste Schritte

- [Registrierungsformular für die Abfragebeschleunigung](https://aka.ms/adls/qa-preview-signup)    
- [Abfragebeschleunigung für Azure Data Lake Storage (Vorschau)](data-lake-storage-query-acceleration.md)
- [Abfragebeschleunigung – SQL-Sprachreferenz (Vorschau)](query-acceleration-sql-reference.md)
