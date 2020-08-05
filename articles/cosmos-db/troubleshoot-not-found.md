---
title: Troubleshooting für die Ausnahme „Nicht gefunden“ in Azure Cosmos DB
description: Diagnose und Behebung der Ausnahme „Nicht gefunden“
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: f391772672904a1e2bdfa0a741d9f85a6edeea8b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293807"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-not-found"></a>Diagnose und Troubleshooting für „Nicht gefunden“ in Azure Cosmos DB
Der HTTP-Statuscode 404 steht dafür, dass die Ressource nicht mehr vorhanden ist.

## <a name="expected-behavior"></a>Erwartetes Verhalten
Es gibt viele gültige Szenarios, in denen eine Anwendung eine 404-Meldung erwartet und das Szenario ordnungsgemäß verarbeitet.

## <a name="not-found-was-returned-for-an-item-that-should-exist-or-does-exist"></a>„Nicht gefunden“ wurde für ein Element zurückgegeben, das vorhanden sein sollte oder vorhanden ist
Im Folgenden sind mögliche Ursachen für das Zurückgeben des Statuscodes 404 in Fällen aufgeführt, in denen das betreffende Element vorhanden sein sollte oder vorhanden ist.

### <a name="1-race-condition"></a>1. Racebedingung
Es sind mehrere SDK-Clientinstanzen vorhanden, und der Lesevorgang ist vor dem Schreibvorgang erfolgt.

#### <a name="solution"></a>Lösung:
1. Die standardmäßig für Cosmos DB festgelegte Kontokonsistenz ist die Sitzungskonsistenz. Wenn ein Element erstellt oder aktualisiert wird, wird bei der Antwort ein Sitzungstoken zurückgegeben, das zwischen den SDK-Instanzen weitergegeben werden kann, um sicherzustellen, dass bei der Leseanforderung aus einem Replikat mit dieser Änderung gelesen wird.
2. Ändern Sie die [Konsistenzebene](consistency-levels-choosing.md) in eine [höhere Ebene](consistency-levels-tradeoffs.md).

### <a name="2-invalid-partition-key-and-id-combination"></a>2. Ungültige Kombination aus Partitionsschlüssel und ID
Die Kombination aus Partitionsschlüssel und ID ist ungültig.

#### <a name="solution"></a>Lösung:
Korrigieren Sie die Anwendungslogik, die die falsche Kombination verursacht. 

### <a name="3-invalid-character-in-item-id"></a>3. Ungültiges Zeichen in Element-ID
Ein Element mit einem [ungültigen Zeichen](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet#remarks) in der Element-ID wird in Cosmos DB eingefügt.

#### <a name="solution"></a>Lösung:
Es wird empfohlen, dass Benutzer die ID in einen anderen Wert ändern, der keine Sonderzeichen enthält. Wenn das Ändern der ID keine Option ist, können Sie die ID mit Base64 codieren, um keine Sonderzeichen zu verwenden.

Bei Elementen, die bereits in den Container eingefügt wurden, kann die ID ersetzt werden, indem RID-Werte anstelle von namensbasierten Verweisen verwendet werden.
```c#
// Get a container reference that use RID values
ContainerProperties containerProperties = await this.Container.ReadContainerAsync();
string[] selfLinkSegments = containerProperties.SelfLink.Split('/');
string databaseRid = selfLinkSegments[1];
string containerRid = selfLinkSegments[3];
Container containerByRid = this.cosmosClient.GetContainer(databaseRid, containerRid);

// List of invalid characters are listed here.
//https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet#remarks
FeedIterator<JObject> invalidItemsIterator = this.Container.GetItemQueryIterator<JObject>(
    @"select * from t where CONTAINS(t.id, ""/"") or CONTAINS(t.id, ""#"") or CONTAINS(t.id, ""?"") or CONTAINS(t.id, ""\\"") ");
while (invalidItemsIterator.HasMoreResults)
{
    foreach (JObject itemWithInvalidId in await invalidItemsIterator.ReadNextAsync())
    {
        // It recommend to chose a new ID that does not contain special characters, but
        // if that is not possible then it can be Base64 encoded to escape the special characters
        byte[] plainTextBytes = Encoding.UTF8.GetBytes(itemWithInvalidId["id"].ToString());
        itemWithInvalidId["id"] = Convert.ToBase64String(plainTextBytes);

        // Update the item with the new ID value using the rid based container reference
        JObject item = await containerByRid.ReplaceItemAsync<JObject>(
            item: itemWithInvalidId,
            ID: itemWithInvalidId["_rid"].ToString(),
            partitionKey: new Cosmos.PartitionKey(itemWithInvalidId["status"].ToString()));

        // Validate the new ID can be read using the original name based contianer reference
        await this.Container.ReadItemAsync<ToDoActivity>(
            item["id"].ToString(),
            new Cosmos.PartitionKey(item["status"].ToString())); ;
    }
}
```

### <a name="4-time-to-live-ttl-purge"></a>4. Bereinigung aufgrund der Gültigkeitsdauer (TTL)
Für das Element wurde die Eigenschaft [Gültigkeitsdauer (TTL)](https://docs.microsoft.com/azure/cosmos-db/time-to-live) festgelegt. Das Element wurde bereinigt, da die Gültigkeitsdauer abgelaufen war.

#### <a name="solution"></a>Lösung:
Ändern Sie die Gültigkeitsdauer, um zu verhindern, dass das Element bereinigt wird.

### <a name="5-lazy-indexing"></a>5. Verzögerte Indizierung
Die Container mit [verzögerter Indizierung](index-policy.md#indexing-mode) sind noch nicht auf dem aktuellen Stand.

#### <a name="solution"></a>Lösung:
Warten Sie, bis die Indizierung die Container auf den aktuellen Stand gebracht hat, oder ändern Sie die Indizierungsrichtlinie.

### <a name="6-parent-resource-deleted"></a>6. Übergeordnete Ressource gelöscht
Die Datenbank und/oder der Container, in der bzw. dem sich das Element befindet, wurde gelöscht.

#### <a name="solution"></a>Lösung:
1. [Stellen Sie die übergeordnete Ressource wieder her](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore#backup-retention-period), oder erstellen Sie die Ressourcen neu.
2. Erstellen Sie eine neue Ressource, um die gelöschte Ressource zu ersetzen.

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln](troubleshoot-dot-net-sdk.md) von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)