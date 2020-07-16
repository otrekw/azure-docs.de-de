---
title: Verwenden des Blobindex zum Verwalten und Suchen von Daten in Azure Blob Storage
description: Hier finden Sie Beispiele für die Verwendung von Blobindextags zum Kategorisieren, Verwalten und Abfragen von Daten, um Blobobjekte zu ermitteln.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2020
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.reviewer: hux
ms.openlocfilehash: 6e3ce99211da35105fd9e118a850110dfd48ece1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84986281"
---
# <a name="utilize-blob-index-tags-preview-to-manage-and-find-data-on-azure-blob-storage"></a>Verwenden von Blobindextags (Vorschauversion) zum Verwalten und Suchen von Daten in Azure Blob Storage

Blobindextags kategorisieren Daten in Ihrem Speicherkonto mithilfe von Schlüssel-Wert-Tagattributen. Diese Tags werden automatisch indiziert und als abfragbarer mehrdimensionaler Index verfügbar gemacht, um Daten einfach finden zu können. In diesem Artikel wird erörtert, wie Sie Daten mithilfe von Blobindextags festlegen, abrufen und suchen.

Weitere Informationen zum Blobindex finden Sie unter [Verwalten und Suchen von Daten in Azure Blob Storage mit dem Blobindex (Vorschau)](storage-manage-find-blobs.md).

> [!NOTE]
> Der Blobindex befindet sich in der öffentlichen Vorschauphase und ist in den Regionen **Kanada, Mitte**, **Kanada, Osten**, **Frankreich, Mitte** und **Frankreich, Süden** verfügbar. Weitere Informationen zu dieser Funktion sowie zu bekannten Problemen und Einschränkungen finden Sie unter [Verwalten und Suchen von Daten in Azure Blob Storage mit dem Blobindex (Vorschau)](storage-manage-find-blobs.md).

## <a name="prerequisites"></a>Voraussetzungen
# <a name="portal"></a>[Portal](#tab/azure-portal)
- Das Abonnement ist registriert und wurde für den Zugriff auf die Blobindex-Vorschauversion genehmigt.
- Zugriff auf das [Azure-Portal](https://portal.azure.com/)

# <a name="net"></a>[.NET](#tab/net)
Da sich der Blobindex in der öffentlichen Vorschauphase befindet, wird das .NET-Speicherpaket im NuGet-Vorschaufeed veröffentlicht. Diese Bibliothek kann bis zu ihrer offiziellen Veröffentlichung noch geändert werden. 

1. Fügen Sie in Visual Studio die URL-`https://azuresdkartifacts.blob.core.windows.net/azure-sdk-for-net/index.json` zu den NuGet-Paketquellen hinzu. 

   Entsprechende Informationen finden Sie unter [Paketquellen](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources).

2. Suchen Sie im NuGet-Paket-Manager nach dem Paket **Azure.Storage.Blobs**, und installieren Sie Version **12.5.0-dev.20200422.2** in Ihrem Projekt. Sie können auch den Befehl ```Install-Package Azure.Storage.Blobs -Version 12.5.0-dev.20200422.2``` ausführen.

   Weitere Informationen finden Sie unter [Suchen und Installieren eines Pakets](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#find-and-install-a-package).

3. Fügen Sie die folgenden using-Anweisungen am Anfang Ihrer Codedatei ein.
```csharp
using Azure;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using Azure.Storage.Blobs.Specialized;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
```
---

## <a name="upload-a-new-blob-with-index-tags"></a>Hochladen eines neuen Blobs mit Indextags
# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Speicherkonto aus. 

2. Navigieren Sie unter **Blob-Dienst** zur Option **Container**, und wählen Sie Ihren Container aus.

3. Wählen Sie die Schaltfläche **Hochladen** aus, um das Uploadblatt zu öffnen, und navigieren Sie in Ihrem lokalen Dateisystem zu einer Datei, um sie als Blockblob hochzuladen.

4. Erweitern Sie das Dropdownmenü **Erweitert**, und wechseln Sie zum Abschnitt **Blobindextags**.

5. Geben Sie die Schlüssel-Wert-Blobindextags ein, die auf Ihre Daten angewendet werden sollen.

6. Wählen Sie die Schaltfläche **Hochladen** aus, um das Blob hochzuladen.

![Hochladen von Daten mit Blobindextags](media/storage-blob-index-concepts/blob-index-upload-data-with-tags.png)

# <a name="net"></a>[.NET](#tab/net)
Im folgenden Beispiel wird das Erstellen eines Anfügeblobs gezeigt, bei dem Tags während der Erstellung festgelegt wurden.
```csharp
static async Task BlobIndexTagsOnCreate()
   {
      BlobServiceClient serviceClient = new BlobServiceClient(ConnectionString);
      BlobContainerClient container = serviceClient.GetBlobContainerClient("mycontainer");

      try
      {
          // Create a container
          await container.CreateIfNotExistsAsync();

          // Create an append blob
          AppendBlobClient appendBlobWithTags = container.GetAppendBlobClient("myAppendBlob0.logs");
         
          // Blob Index tags to upload
          CreateAppendBlobOptions appendOptions = new CreateAppendBlobOptions();
          appendOptions.Tags = new Dictionary<string, string>
          {
              { "Sealed", "false" },
              { "Content", "logs" },
              { "Date", "2020-04-20" }
          };

          // Upload data with tags set on creation
          await appendBlobWithTags.CreateAsync(appendOptions);
      }
      finally
      {
      }
   }
```

---

## <a name="get-set-and-update-blob-index-tags"></a>Abrufen, Festlegen und Aktualisieren von Blobindextags
# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Speicherkonto aus. 

2. Navigieren Sie unter **Blob-Dienst** zur Option **Container**, und wählen Sie Ihren Container aus.

3. Wählen Sie das gewünschte Blob aus der Liste der Blobs innerhalb des ausgewählten Containers aus.

4. Auf der Registerkarte „Übersicht“ für das Blob werden die Eigenschaften Ihres Blobs einschließlich der **Blobindextags** angezeigt.

5. Sie können alle Schlüssel-Wert-Indextags für Ihr Blob abrufen, festlegen, ändern oder löschen.

6. Wählen Sie die Schaltfläche **Speichern** aus, um die an Ihrem Blob vorgenommenen Aktualisierungen zu bestätigen.

![Abrufen, Festlegen, Aktualisieren und Löschen von Blobindextags für Objekte](media/storage-blob-index-concepts/blob-index-get-set-tags.png)

# <a name="net"></a>[.NET](#tab/net)
```csharp
static async Task BlobIndexTagsExample()
   {
      BlobServiceClient serviceClient = new BlobServiceClient(ConnectionString);
      BlobContainerClient container = serviceClient.GetBlobContainerClient("mycontainer");

      try
      {
          // Create a container
          await container.CreateIfNotExistsAsync();

          // Create a new append blob
          AppendBlobClient appendBlob = container.GetAppendBlobClient("myAppendBlob1.logs");
          await appendBlob.CreateAsync();

          // Set or Update Blob Index tags on existing blob
          Dictionary<string, string> tags = new Dictionary<string, string>
          {
              { "Project", "Contoso" },
              { "Status", "Unprocessed" },
              { "Sealed", "true" }
          };
          await appendBlob.SetTagsAsync(tags);

          // Get Blob Index tags
          Response<IDictionary<string, string>> tagsResponse = await appendBlob.GetTagsAsync();
          Console.WriteLine(appendBlob.Name);
          foreach (KeyValuePair<string, string> tag in tagsResponse.Value)
          {
              Console.WriteLine($"{tag.Key}={tag.Value}");
          }

          // List Blobs with all options returned including Blob Index tags
          await foreach (BlobItem blobItem in container.GetBlobsAsync(BlobTraits.All))
          {
              Console.WriteLine(Environment.NewLine + blobItem.Name);
              foreach (KeyValuePair<string, string> tag in blobItem.Tags)
              {
                  Console.WriteLine($"{tag.Key}={tag.Value}");
              }
          }

          // Delete existing Blob Index tags by replacing all tags
          Dictionary<string, string> noTags = new Dictionary<string, string>();
          await appendBlob.SetTagsAsync(noTags);

      }
      finally
      {
      }
   }
```

---

## <a name="filter-and-find-data-with-blob-index-tags"></a>Filtern und Suchen von Daten mit Blobindextags

# <a name="portal"></a>[Portal](#tab/azure-portal)

Im Azure-Portal wendet der Filter für Blobindextags den `@container`-Parameter automatisch an, um den Bereich auf Ihren ausgewählten Container festzulegen. Wenn Sie mit Tags versehene Daten in Ihrem gesamten Speicherkonto filtern und suchen möchten, verwenden Sie unsere REST-API, SDKs oder Tools.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Speicherkonto aus. 

2. Navigieren Sie unter **Blob-Dienst** zur Option **Container**, und wählen Sie Ihren Container aus.

3. Wählen Sie die Schaltfläche **Filter für Blobindextags** aus, um innerhalb des ausgewählten Containers zu filtern.

4. Geben Sie einen Tagschlüssel und einen Tagwert für den Blobindex ein.

5. Wählen Sie die Schaltfläche **Filter für Blobindextags** aus, um weitere (bis zu 10) Tagfilter hinzuzufügen.

![Filtern und Suchen von mit Tags versehenen Objekten mithilfe von Blobindextags](media/storage-blob-index-concepts/blob-index-tag-filter-within-container.png)

# <a name="net"></a>[.NET](#tab/net)
```csharp
static async Task FindBlobsByTagsExample()
   {
      BlobServiceClient serviceClient = new BlobServiceClient(ConnectionString);
      BlobContainerClient container1 = serviceClient.GetBlobContainerClient("mycontainer");
      BlobContainerClient container2 = serviceClient.GetBlobContainerClient("mycontainer2");

      // Blob Index queries and selection
      String singleEqualityQuery = @"""Archive"" = 'false'";
      String andQuery = @"""Archive"" = 'false' AND ""Priority"" = '01'";
      String rangeQuery = @"""Date"" >= '2020-04-20' AND ""Date"" <= '2020-04-30'";
      String containerScopedQuery = @"@container = 'mycontainer' AND ""Archive"" = 'false'";

      String queryToUse = containerScopedQuery;

      try
      {
          // Create a container
          await container1.CreateIfNotExistsAsync();
          await container2.CreateIfNotExistsAsync();

          // Create append blobs
          AppendBlobClient appendBlobWithTags0 = container1.GetAppendBlobClient("myAppendBlob00.logs");
          AppendBlobClient appendBlobWithTags1 = container1.GetAppendBlobClient("myAppendBlob01.logs");
          AppendBlobClient appendBlobWithTags2 = container1.GetAppendBlobClient("myAppendBlob02.logs");
          AppendBlobClient appendBlobWithTags3 = container2.GetAppendBlobClient("myAppendBlob03.logs");
          AppendBlobClient appendBlobWithTags4 = container2.GetAppendBlobClient("myAppendBlob04.logs");
          AppendBlobClient appendBlobWithTags5 = container2.GetAppendBlobClient("myAppendBlob05.logs");
           
          // Blob Index tags to upload
          CreateAppendBlobOptions appendOptions = new CreateAppendBlobOptions();
          appendOptions.Tags = new Dictionary<string, string>
          {
              { "Archive", "false" },
              { "Priority", "01" },
              { "Date", "2020-04-20" }
          };
          
          CreateAppendBlobOptions appendOptions2 = new CreateAppendBlobOptions();
          appendOptions2.Tags = new Dictionary<string, string>
          {
              { "Archive", "true" },
              { "Priority", "02" },
              { "Date", "2020-04-24" }
          };

          // Upload data with tags set on creation
          await appendBlobWithTags0.CreateAsync(appendOptions);
          await appendBlobWithTags1.CreateAsync(appendOptions);
          await appendBlobWithTags2.CreateAsync(appendOptions2);
          await appendBlobWithTags3.CreateAsync(appendOptions);
          await appendBlobWithTags4.CreateAsync(appendOptions2);
          await appendBlobWithTags5.CreateAsync(appendOptions2);

          // Find Blobs given a tags query
          Console.WriteLine("Find Blob by Tags query: " + queryToUse + Environment.NewLine);

          List<FilterBlobItem> blobs = new List<FilterBlobItem>();
          await foreach (FilterBlobItem filterBlobItem in serviceClient.FindBlobsByTagsAsync(queryToUse))
          {
              blobs.Add(filterBlobItem);
          }

          foreach (var filteredBlob in blobs)
          {
              Console.WriteLine($"BlobIndex result: ContainerName= {filteredBlob.ContainerName}, " +
                  $"BlobName= {filteredBlob.Name}");
          }

      }
      finally
      {
      }
   }
```

---

## <a name="lifecycle-management-with-blob-index-tag-filters"></a>Lebenszyklusverwaltung mit Filtern für Blobindextags

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Speicherkonto aus. 

2. Navigieren Sie unter **Blob-Dienst** zur Option **Lebenszyklusverwaltung**.

3. Wählen Sie *Regel hinzufügen* aus, und füllen Sie dann die Felder des Formulars „Aktionssatz“ aus.

4. Wählen Sie „Filtersatz“ aus, um optionale Filter für „Präfixübereinstimmung“ und „Blobindexübereinstimmung“ hinzuzufügen. ![Hinzufügen von Filtern für Blobindextags für die Lebenszyklusverwaltung](media/storage-blob-index-concepts/blob-index-match-lifecycle-filter-set.png)

5. Wählen Sie **Überprüfen + hinzufügen** aus, um die Regeleinstellungen zu überprüfen. ![Beispiel für eine Regel zur Lebenszyklusverwaltung mit dem Filter für Blobindextags](media/storage-blob-index-concepts/blob-index-lifecycle-management-example.png)

6. Wählen Sie **Hinzufügen** aus, um die neue Regel auf die Richtlinie für die Lebenszyklusverwaltung anzuwenden.

# <a name="net"></a>[.NET](#tab/net)
Richtlinien für die [Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md) werden für jedes Speicherkonto auf der Steuerungsebene angewendet. Installieren Sie für .NET [Microsoft Azure Management Storage Library, Version 16.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/) oder höher, um den Filter „Blobindexübereinstimmung“ in einer Regel zur Lebenszyklusverwaltung nutzen zu können.

---

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den Blobindex. Sehen Sie sich hierzu [Verwalten und Ermitteln von Daten in Azure Blob Storage mit dem Blobindex (Vorschau)](storage-manage-find-blobs.md ) an.

Erfahren Sie mehr über die Lebenszyklusverwaltung. Sehen Sie sich hierzu [Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md) an.
