---
title: Pullmodell für Änderungsfeed
description: Erfahren Sie, wie Sie das Pullmodell für den Azure Cosmos DB-Änderungsfeed verwenden, um den Änderungsfeed zu lesen. Erfahren Sie außerdem etwas über die Unterschiede zwischen dem Pullmodell und dem Änderungsfeedprozessor.
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/19/2020
ms.reviewer: sngun
ms.openlocfilehash: c47016d0b82a4e4ed084f5d82394d91fd2b46be1
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697717"
---
# <a name="change-feed-pull-model-in-azure-cosmos-db"></a>Pullmodell für den Änderungsfeed in Azure Cosmos DB

Mit dem Pullmodell für den Änderungsfeed können Sie den Azure Cosmos DB-Änderungsfeed in Ihrem eigenen Tempo verwenden. Wie Sie bereits beim [Änderungsfeedprozessor](change-feed-processor.md) können Sie das Pullmodell für den Änderungsfeed verwenden, um die Verarbeitung von Änderungen über mehrere Änderungsfeedconsumer zu parallelisieren.

> [!NOTE]
> Das Pullmodell für den Änderungsfeed ist derzeit ausschließlich als [Vorschauversion im Azure Cosmos DB .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview) verfügbar. Für andere SDK-Versionen ist noch keine Vorschauversion verfügbar.

## <a name="consuming-an-entire-containers-changes"></a>Verwenden der Änderungen eines vollständigen Containers

Sie können einen `FeedIterator` erstellen, um den Änderungsfeed mithilfe des Pullmodells zu verarbeiten. Wenn Sie erstmalig einen `FeedIterator` erstellen, können Sie in den `ChangeFeedRequestOptions` eine optionale `StartTime` angeben. Wenn diese nicht angegeben wird, ist die `StartTime` die aktuelle Uhrzeit.

Der `FeedIterator` ist in zwei Varianten verfügbar. Zusätzlich zu den unten aufgeführten Beispielen, die Entitätsobjekte zurückgeben, können Sie die Antwort auch mit `Stream`-Unterstützung abrufen. Datenströme ermöglichen Ihnen, Daten zu lesen, ohne sie zuvor deserialisieren und auf Clientressourcen speichern zu müssen.

Im Folgenden finden Sie ein Beispiel für das Abrufen eines `FeedIterator`, der Entitätsobjekte zurückgibt (in diesem Fall ein `User`-Objekt):

```csharp
FeedIterator<User> iteratorWithPOCOS = container.GetChangeFeedIterator<User>();
```

Im Folgenden finden Sie ein Beispiel für das Abrufen eines `FeedIterator`, der einen `Stream` zurückgibt:

```csharp
FeedIterator iteratorWithStreams = container.GetChangeFeedStreamIterator();
```

Mithilfe eines `FeedIterator` können Sie den Änderungsfeed eines vollständigen Containers problemlos in Ihrem eigenen Tempo verarbeiten. Hier sehen Sie ein Beispiel:

```csharp
FeedIterator<User> iteratorForTheEntireContainer= container.GetChangeFeedIterator<User>();

while (iteratorForTheEntireContainer.HasMoreResults)
{
   FeedResponse<User> users = await iteratorForTheEntireContainer.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="consuming-a-partition-keys-changes"></a>Verwenden der Änderungen eines Partitionsschlüssels

In einigen Fällen möchten Sie möglicherweise nur die Änderungen eines bestimmten Partitionsschlüssels verarbeiten. Sie können einen `FeedIterator` für einen bestimmten Partitionsschlüssel abrufen und die Änderungen auf die gleiche Weise wie für einen vollständigen Container verarbeiten.

```csharp
FeedIterator<User> iteratorForThePartitionKey = container.GetChangeFeedIterator<User>(new PartitionKey("myPartitionKeyValueToRead"));

while (iteratorForThePartitionKey.HasMoreResults)
{
   FeedResponse<User> users = await iteratorForThePartitionKey.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="using-feedrange-for-parallelization"></a>Verwenden von FeedRange für die Parallelisierung

Beim [Änderungsfeedprozessor](change-feed-processor.md) wird die Arbeit automatisch auf mehrere Consumer verteilt. Beim Pullmodell für den Änderungsfeed können Sie `FeedRange` verwenden, um die Verarbeitung des Änderungsfeeds zu parallelisieren. Ein `FeedRange` stellt einen Bereich von Partitionsschlüsselwerten dar.

Das folgende Beispiel zeigt, wie Sie eine Liste von Bereichen für Ihren Container abrufen:

```csharp
IReadOnlyList<FeedRange> ranges = await container.GetFeedRangesAsync();
```

Wenn Sie eine Liste von FeedRanges für Ihren Container abrufen, erhalten Sie einen `FeedRange` pro [physischer Partition](partition-data.md#physical-partitions).

Mithilfe eines `FeedRange` können Sie dann einen `FeedIterator` erstellen, um die Verarbeitung des Änderungsfeeds parallel auf mehrere Computer oder Threads zu verteilen. Im Gegensatz zum vorherigen Beispiel, in dem gezeigt wurde, wie Sie einen einzelnen `FeedIterator` für den gesamten Container abrufen, können Sie mithilfe von `FeedRange` mehrere FeedIterators abrufen, über die der Änderungsfeed parallel verarbeitet werden kann.

Wenn Sie FeedRanges verwenden möchten, benötigen Sie einen Orchestratorprozess, der sie abruft und auf diese Computer verteilt. Diese Verteilung könnte wie folgt erfolgen:

* Verwenden von `FeedRange.ToJsonString` und Verteilen dieses Zeichenfolgenwerts. Die Consumer können diesen Wert mit `FeedRange.FromJsonString` verwenden.
* Wenn die Verteilung bereits bearbeitet wird: Übergeben eines Verweises auf das `FeedRange`-Objekt

Das folgende Beispiel zeigt, wie Sie vom Anfang des Änderungsfeeds des Containers lesen und dafür zwei hypothetische, getrennte Computer verwenden, die parallel lesen:

Computer 1:

```csharp
FeedIterator<User> iteratorA = container.GetChangeFeedIterator<User>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
while (iteratorA.HasMoreResults)
{
   FeedResponse<User> users = await iteratorA.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

Computer 2:

```csharp
FeedIterator<User> iteratorB = container.GetChangeFeedIterator<User>(ranges[1], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
while (iteratorB.HasMoreResults)
{
   FeedResponse<User> users = await iteratorB.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="saving-continuation-tokens"></a>Speichern von Fortsetzungstoken

Sie können die Position des `FeedIterator` speichern, indem Sie ein Fortsetzungstoken erstellen. Ein Fortsetzungstoken ist ein Zeichenfolgenwert, der die letzten verarbeiteten Änderungen Ihres FeedIterators nachverfolgt. Dadurch kann der `FeedIterator` zu einem späteren Zeitpunkt an dieser Stelle fortgesetzt werden. Der folgende Code liest den Änderungsfeed seit der Erstellung des Containers. Wenn keine weiteren Änderungen mehr verfügbar sind, wird ein Fortsetzungstoken persistent gespeichert, sodass der Änderungsfeed später weiter verarbeitet werden kann.

```csharp
FeedIterator<User> iterator = container.GetChangeFeedIterator<User>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

string continuation = null;

while (iterator.HasMoreResults)
{
   FeedResponse<User> users = await iterator.ReadNextAsync();
   continuation = users.ContinuationToken;

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}

// Some time later
FeedIterator<User> iteratorThatResumesFromLastPoint = container.GetChangeFeedIterator<User>(continuation);
```

Solange der Cosmos-Container vorhanden ist, läuft das Fortsetzungstoken eines FeedIterators nie ab.

## <a name="comparing-with-change-feed-processor"></a>Vergleich mit dem Änderungsfeedprozessor

In vielen Szenarien können Sie den Änderungsfeed entweder mit dem [Änderungsfeedprozessor](change-feed-processor.md) oder mit dem Pullmodell verarbeiten. Die Fortsetzungstoken des Pullmodells und der Leasecontainer des Änderungsfeedprozessors sind beides „Lesezeichen“ für das zuletzt verarbeitete Element (oder den Batch von Elementen) im Änderungsfeed.
Es ist jedoch nicht möglich, Fortsetzungstoken in einen Leasecontainer (oder umgekehrt) zu konvertieren.

In folgenden Szenarien sollten Sie die Verwendung des Pullmodells in Erwägung ziehen:

- Lesen von Änderungen von einem bestimmten Partitionsschlüssel
- Steuern der Geschwindigkeit, mit der der Client Änderungen für die Verarbeitung empfängt
- Ausführen eines einmaligen Lesevorgangs über die vorhandenen Daten im Änderungsfeed (z. B. für eine Datenmigration)

Im Folgenden sind einige wichtige Unterschiede zwischen dem Änderungsfeedprozessor und dem Pullmodell aufgeführt:

|  | Change Feed Processor| Pull-Modell |
| --- | --- | --- |
| Verfolgen der aktuellen Position bei der Verarbeitung des Änderungsfeeds | Lease (gespeichert in einem Azure Cosmos DB-Container) | Fortsetzungstoken (im Arbeitsspeicher gespeichert oder manuell persistent gespeichert) |
| Möglichkeit zur Wiederholung vergangener Änderungen | Ja, mit Pushmodell | Ja, mit Pullmodell|
| Pullen zukünftiger Änderungen | Automatische Überprüfung auf Änderungen basierend auf einem vom Benutzer angegebenen `WithPollInterval` | Manuell |
| Verarbeiten von Änderungen aus dem gesamten Container | Ja, mit automatischer Parallelisierung über mehrere Threads/Computer hinweg, die denselben Container verwenden| Ja, mit manueller Parallelisierung mithilfe von FeedTokens |
| Verarbeiten von Änderungen von nur einem Partitionsschlüssel | Nicht unterstützt | Ja|
| Supportebene | Allgemein verfügbar | Vorschau |

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Änderungsfeeds](change-feed.md)
* [Verwenden des Änderungsfeedprozessors](change-feed-processor.md)
* [Auslösen von Azure Functions](change-feed-functions.md)