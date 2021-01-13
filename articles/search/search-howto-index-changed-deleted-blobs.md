---
title: Geänderte und gelöschte Blobs
titleSuffix: Azure Cognitive Search
description: Nach einem ersten Suchindexbuild, der aus Azure Blob Storage importiert wird, kann die nachfolgende Indizierung nur die Blobs übernehmen, die geändert oder gelöscht werden. In diesem Artikel werden die Details erläutert.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 2e73039418233c97fc20242ed7af7df14c5b47ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91534776"
---
# <a name="how-to-set-up-change-and-deletion-detection-for-blobs-in-azure-cognitive-search-indexing"></a>Einrichten der Erkennung von Änderungen und Löschungen für Blobs in der Azure Cognitive Search-Indizierung

Nachdem ein ursprünglicher Suchindex erstellt wurde, sollten Sie nachfolgende Indexeraufträge so konfigurieren, dass nur die Dokumente ausgewählt werden, die seit der ersten Ausführung erstellt oder gelöscht wurden. Wenn Sie einen Zeitplan für die Triggerindizierung verwenden, wird die Änderungserkennung bei Suchinhalten aus Azure Blob Storage automatisch durchgeführt. Standardmäßig werden wie vom `LastModified`-Zeitstempel des Blobs angegeben nur die geänderten Blobs vom Dienst neu indiziert. Im Gegensatz zu anderen von Suchindexern unterstützten Datenquellen verfügen Blobs immer über einen Zeitstempel, wodurch die Notwendigkeit entfällt, eine Richtlinie für die Änderungserkennung manuell einzurichten.

Die Änderungserkennung ist eine Standardeinstellung, die Erkennung von Löschungen hingegen nicht. Wenn Sie gelöschte Dokumente erkennen möchten, stellen Sie sicher, dass Sie einen Ansatz für „vorläufiges Löschen“ verwenden. Wenn Sie die Blobs direkt löschen, werden die entsprechenden Dokumente nicht aus dem Suchindex entfernt.

Es gibt zwei Möglichkeiten, den Ansatz des vorläufigen Löschens zu implementieren. Beide werden unten beschrieben.

## <a name="native-blob-soft-delete-preview"></a>Natives vorläufiges Löschen von Blobs (Vorschau)

> [!IMPORTANT]
> Die Unterstützung für das native vorläufige Löschen von Blobs ist als Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2020-06-30-Preview](./search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

> [!NOTE]
> Bei Verwendung der Richtlinie zum nativen vorläufigen Löschen von Blobs müssen die Dokumentschlüssel für die Dokumente in Ihrem Index entweder eine Blobeigenschaft oder Blobmetadaten sein.

Bei dieser Methode verwenden Sie das Feature zum [nativen vorläufigen Löschen von Blobs](../storage/blobs/soft-delete-blob-overview.md), das Azure Blob Storage bietet. Wenn natives vorläufiges Löschen von Blobs für Ihr Speicherkonto aktiviert ist, wird für die Datenquelle eine Richtlinie für natives vorläufiges Löschen festgelegt, und der Indexer sucht ein Blob, das in einen vorläufig gelöschten Zustand versetzt wurde. Der Indexer entfernt dieses Dokument aus dem Index. Die Richtlinie zum nativen vorläufigen Löschen von Blobs wird beim Indizieren von Blobs aus Azure Data Lake Storage Gen2 nicht unterstützt.

Führen Sie die folgenden Schritte durch:

1. Aktivieren Sie [natives vorläufiges Löschen für Azure Blob Storage](../storage/blobs/soft-delete-blob-overview.md). Sie sollten für die Aufbewahrungsrichtlinie einen Wert festlegen, der wesentlich höher ist als der Intervallzeitplan Ihres Indexers. Wenn ein Problem beim Ausführen des Indexers vorliegt oder eine große Anzahl von Dokumenten indiziert werden muss, steht dem Indexer viel Zeit für die endgültige Verarbeitung der vorläufig gelöschten Blobs zur Verfügung. Indexer von Azure Cognitive Suche löschen nur dann ein Dokument aus dem Index, wenn sie das Blob verarbeiten, während es sich in einem vorläufig gelöschten Zustand befindet.

1. Konfigurieren Sie für die Datenquelle eine Richtlinie zur Erkennung des nativen vorläufigen Löschens von Blobs. Ein entsprechendes Beispiel ist nachfolgend dargestellt. Da dieses Feature in der Vorschauversion verfügbar ist, müssen Sie die Vorschau-REST-API verwenden.

1. Führen Sie den Indexer aus, oder legen Sie fest, dass der Indexer gemäß einem Zeitplan ausgeführt wird. Wenn der Indexer das Blob ausführt und verarbeitet, wird das Dokument aus dem Index entfernt.

    ```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

### <a name="reindexing-un-deleted-blobs-using-native-soft-delete-policies"></a>Erneutes Indizieren nicht gelöschter Blobs (mithilfe von nativen Richtlinien für vorläufiges Löschen)

Wenn Sie mit aktiviertem nativem vorläufigem Löschen ein Blob aus Azure Blob Storage in Ihrem Speicherkonto löschen, wechselt das Blob in einen vorläufig gelöschten Zustand, sodass Sie die Möglichkeit haben, das Blob innerhalb des Aufbewahrungszeitraums wiederherzustellen. Wenn Sie einen Löschvorgang rückgängig machen, nachdem der Indexer ihn verarbeitet hat, indiziert der Indexer nicht immer das wiederhergestellte Blob. Dies liegt daran, dass der Indexer basierend auf dem `LastModified`-Zeitstempel des Blobs bestimmt, welche Blobs indiziert werden sollen. Wenn ein vorläufig gelöschtes Blob wiederhergestellt wird, wird sein `LastModified`-Zeitstempel nicht aktualisiert. Falls der Indexer bereits Blobs mit aktuelleren `LastModified`-Zeitstempeln verarbeitet hat, wird das wiederhergestellte Blob nicht neu indiziert. 

Sie müssen den `LastModified`-Zeitstempel des Blobs aktualisieren, um sicherzustellen, dass ein wiederhergestelltes Blob neu indiziert wird. Eine Möglichkeit hierfür besteht darin, die Metadaten des Blobs erneut zu speichern. Sie müssen die Metadaten nicht ändern, aber durch das erneute Speichern der Metadaten wird der `LastModified`-Zeitstempel des Blobs aktualisiert, sodass der Indexer weiß, dass er dieses Blob neu indizieren muss.

## <a name="soft-delete-using-custom-metadata"></a>Vorläufiges Löschen mithilfe benutzerdefinierter Metadaten

In dieser Methode verwenden Sie die Metadaten eines Blobs, um anzugeben, wann ein Dokument aus dem Suchindex entfernt werden sollte. Diese Methode erfordert zwei separate Aktionen: Zunächst wird das Suchdokument aus dem Index und dann das Blob in Azure Storage gelöscht.

Führen Sie die folgenden Schritte durch:

1. Fügen Sie dem Blob ein Metadaten-Schlüssel-Wert-Paar hinzu, um in Azure Cognitive Search anzugeben, dass das Blob logisch gelöscht ist.

1. Konfigurieren Sie für die Datenquelle eine Richtlinie zur Erkennung von Spalten für vorläufiges Löschen. Ein entsprechendes Beispiel ist nachfolgend dargestellt.

1. Nachdem der Indexer das Blob verarbeitet und das Dokument aus dem Index gelöscht hat, können Sie das Blob in Azure Blob Storage löschen.

Bei der folgenden Richtlinie wird ein Blob beispielsweise als gelöscht angesehen, wenn es über die Metadateneigenschaft `IsDeleted` mit dem Wert `true` verfügt:

```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }
```

### <a name="reindexing-un-deleted-blobs-using-custom-metadata"></a>Erneutes Indizieren wiederhergestellter Blobs (mithilfe von benutzerdefinierten Metadaten)

Nachdem ein Indexer ein gelöschtes Blob verarbeitet und das entsprechende Suchdokument aus dem Index gelöscht hat, wird das Blob nicht erneut überprüft, wenn Sie es später wiederherstellen und der `LastModified`-Zeitstempel des Blobs älter als die letzte Indexerausführung ist.

Wenn Sie dieses Dokument neu indizieren möchten, ändern Sie `"softDeleteMarkerValue" : "false"` für das Blob, und führen Sie den Indexer erneut aus.

## <a name="help-us-make-azure-cognitive-search-better"></a>Helfen Sie uns bei der Verbesserung der kognitiven Azure-Suche

Wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search/) dafür ab. Wenn Sie Hilfe bei der Verwendung des vorhandenen Features benötigen, veröffentlichen Sie Ihre Frage in [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870).

## <a name="next-steps"></a>Nächste Schritte

* [Indexer in der kognitiven Azure-Suche](search-indexer-overview.md)
* [Indizieren von Dokumenten in Azure Blob Storage mit der kognitiven Azure-Suche](search-howto-indexing-azure-blob-storage.md)
* [Hinzufügen der Volltextsuche zu Azure-Blobdaten mithilfe von Azure Cognitive Search](search-blob-storage-integration.md)