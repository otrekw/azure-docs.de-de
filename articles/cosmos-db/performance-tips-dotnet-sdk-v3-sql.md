---
title: Leistungstipps für Azure Cosmos DB und .NET SDK v3
description: Informationen zu den Clientkonfigurationsoptionen zur Verbesserung der Leistung von Azure Cosmos DB und NET SDK v3.
author: j82w
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/16/2020
ms.author: jawilley
ms.openlocfilehash: 90b4ffb273fc314a7c92971490fb09b6f0c131ee
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2020
ms.locfileid: "88258342"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Leistungstipps für Azure Cosmos DB und .NET

> [!div class="op_single_selector"]
> * [.NET SDK v3](performance-tips-dotnet-sdk-v3-sql.md)
> * [.NET SDK v2](performance-tips.md)
> * [Java SDK v4](performance-tips-java-sdk-v4-sql.md)
> * [Async Java SDK v2](performance-tips-async-java.md)
> * [Sync Java SDK v2](performance-tips-java.md)

Azure Cosmos DB ist eine schnelle und flexible verteilte Datenbank mit nahtloser Skalierung, garantierter Latenz und garantiertem Durchsatz. Die Skalierung Ihrer Datenbank mit Azure Cosmos DB erfordert weder aufwendige Änderungen an der Architektur noch das Schreiben von komplexem Code. Zentrales Hoch- und Herunterskalieren ist ebenso problemlos möglich wie ein einzelner API-Aufruf. Weitere Informationen finden Sie unter [Bereitstellen von Containerdurchsatz](how-to-provision-container-throughput.md) und [Bereitstellen von Datenbankdurchsatz](how-to-provision-database-throughput.md). Da der Zugriff auf Azure Cosmos DB jedoch über Netzwerkaufrufe erfolgt, können Sie clientseitige Optimierungen vornehmen, um bei Verwendung des [SQL .NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) eine optimale Leistung zu erzielen.

Wenn Sie versuchen möchten, die Datenbankleistung zu verbessern, sollten Sie die folgenden Optionen beachten:

## <a name="hosting-recommendations"></a>Hostingempfehlungen

**Verwenden Sie für abfrageintensive Workloads die Windows-64-Bit- anstelle der Linux- oder Windows-32-Bit-Hostverarbeitung.**

Für eine bessere Leistung wird die Windows-64-Bit-Hostverarbeitung empfohlen. Das SQL SDK enthält die native Datei „ServiceInterop.dll“, um Abfragen lokal zu analysieren und zu optimieren. „ServiceInterop.dll“ wird nur auf der Windows x64-Plattform unterstützt. Bei Linux und anderen nicht unterstützten Plattformen, bei denen die Datei „ServiceInterop.dll“ nicht verfügbar ist, erfolgt ein zusätzlicher Netzwerkaufruf an das Gateway, um die optimierte Abfrage zu erhalten. Bei den folgenden Anwendungstypen wird standardmäßig die 32-Bit-Hostverarbeitung verwendet. Um die Hostverarbeitung auf die 64-Bit-Verarbeitung umzustellen, führen Sie je nach Typ Ihrer Anwendung die folgenden Schritte aus:

- Bei ausführbaren Anwendungen legen Sie zum Ändern der Hostverarbeitung im Fenster **Projekteigenschaften** auf der Registerkarte **Build** die Option [Zielplattform](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) auf **x64** fest.

- Bei VSTest-basierten Testprojekten können Sie die Hostverarbeitung ändern, indem Sie in der Menüoption **Visual Studio Test** die Option **Test** > **Testeinstellungen** > **Default Processor Architecture as X64** (Standardprozessorarchitektur als x64) auswählen.

- Bei lokal bereitgestellten ASP.NET-Webanwendungen ändern Sie die Hostverarbeitung, indem Sie unter **Extras** > **Optionen** > **Projekte und Projektmappen** > **Webprojekte** die Option **64-Bit-Version von IIS Express für Websites und Projekte verwenden** aktivieren.

- Für ASP.NET-Webanwendungen, die in Azure bereitgestellt werden, können Sie die Hostverarbeitung ändern, indem Sie im Azure-Portal unter **Anwendungseinstellungen** die **64-Bit**-Plattform auswählen.

> [!NOTE] 
> Standardmäßig ist bei neuen Visual Studio-Projekten **Beliebige CPU** festgelegt. Es wird empfohlen, Ihr Projekt auf **x64** festzulegen, damit es nicht zu **x86** wechselt. Ein Projekt mit der Einstellung **Beliebige CPU** kann schnell zu **x86** wechseln, wenn eine Abhängigkeit hinzugefügt wird, die nur bei x86 verfügbar ist.<br/>
> Die Datei „ServiceInterop.dll“ muss sich im selben Ordner befinden, aus dem die SDK-DLL ausgeführt wird. Dies sollte nur dann von Bedeutung sein, wenn Sie DLLs manuell kopieren oder über benutzerdefinierte Build-/Bereitstellungssysteme verfügen.
    
**Aktivieren der serverseitigen Garbage Collection (GC)**

In einigen Fällen kann es hilfreich sein, die Häufigkeit zu verringern, mit der die Garbage Collection ausgeführt wird. Legen Sie [gcServer](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection) in .NET auf `true` fest.

**Erweitern Ihrer Clientworkload**

Wenn Sie auf einem hohen Durchsatzniveau testen (> 50.000 RU/s), kann sich die Clientanwendung als Engpass erweisen, da der Computer die CPU- oder Netzwerkauslastung deckelt. Wenn dieser Punkt erreicht wird, können Sie das Azure Cosmos DB-Konto weiter auslasten, indem Sie Ihre Clientanwendungen auf mehrere Server horizontal hochskalieren.

> [!NOTE] 
> Eine hohe CPU-Auslastung kann zu erhöhter Latenz und Ausnahmen des Typs „Anforderungstimeout“ führen.

## <a name="networking"></a>Netzwerk
<a id="direct-connection"></a>

**Verbindungsrichtlinie: Verwenden des direkten Verbindungsmodus**

Die Art der Verbindungsherstellung zwischen einem Client und Azure Cosmos DB hat erhebliche Auswirkungen auf die Leistung, insbesondere im Hinblick auf die clientseitige Latenz. Für die Konfiguration der Verbindungsrichtlinie stehen zwei wichtige Konfigurationseinstellungen zur Verfügung: der *Verbindungsmodus* und das *Verbindungsprotokoll*.  Folgende Modi sind verfügbar:

   * Direkter Modus (Standard)

     Der direkte Modus unterstützt Konnektivität über das TCP-Protokoll und ist der Standardkonnektivitätsmodus, wenn Sie das [Microsoft.Azure.Cosmos/.NET V3 SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) verwenden. Dies bietet eine bessere Leistung und erfordert weniger Netzwerkhops als der Gatewaymodus.

   * Gatewaymodus
      
     Wenn Ihre Anwendung in einem Unternehmensnetzwerk mit strengen Firewalleinschränkungen ausgeführt wird, ist der Gatewaymodus die beste Wahl, da er den HTTPS-Standardport und einen einzelnen Endpunkt verwendet. Im Gatewaymodus ist jedoch jeweils ein zusätzlicher Netzwerkhop erforderlich, wenn Daten in Azure Cosmos DB geschrieben oder daraus gelesen werden, was sich negativ auf die Leistung auswirkt. Aus diesem Grund bietet der direkte Modus die bessere Leistung, da weniger Netzwerkhops erforderlich sind. Der Gatewayverbindungsmodus wird auch empfohlen, wenn Sie Anwendungen in Umgebungen mit einer begrenzten Anzahl von Socketverbindungen ausführen.

     Beachten Sie bei Verwendung des SDK in Azure Functions – insbesondere beim [Verbrauchstarif](../azure-functions/functions-scale.md#consumption-plan) – die aktuellen [Grenzwerte für Verbindungen](../azure-functions/manage-connections.md). In diesem Fall empfiehlt sich möglicherweise der Gatewaymodus, wenn Sie auch mit anderen HTTP-basierten Clients innerhalb Ihrer Azure Functions-Anwendung arbeiten.
     
Wenn Sie das TCP im direkten Modus verwenden, müssen Sie zusätzlich zu den Gatewayports sicherstellen, dass der Portbereich zwischen 10000 und 20000 offen ist, da Azure Cosmos DB dynamische TCP-Ports verwendet. Wenn Sie den direkten Modus auf [privaten Endpunkten](./how-to-configure-private-endpoints.md) verwenden, sollte der gesamte Bereich der TCP-Ports von 0 bis 65535 geöffnet sein. Die Ports sind bei der Azure VM-Standardkonfiguration standardmäßig offen. Wenn diese Ports nicht geöffnet sind und Sie versuchen, TCP zu verwenden, wird der Fehler „503 – Dienst nicht verfügbar“ angezeigt. Die folgende Tabelle enthält die für verschiedene APIs verfügbaren Konnektivitätsmodi und die für jede API verwendeten Dienstports:

|Verbindungsmodus  |Unterstütztes Protokoll  |Unterstützte SDKs  |API/Dienstport  |
|---------|---------|---------|---------|
|Gateway  |   HTTPS    |  Alle SDKs    |   SQL (443), MongoDB (10250, 10255, 10256), Tabelle (443), Cassandra (10350), Graph (443) <br> Port 10250 ist einer Azure Cosmos DB API für MongoDB-Instanz ohne Georeplikation zugeordnet. Die Ports 10255 und 10256 hingegen sind der Instanz mit Georeplikation zugeordnet.   |
|Direkt    |     TCP    |  .NET SDK    | Bei Verwendung von öffentlichen oder Dienstendpunkten: Ports im Bereich zwischen 10000 und 20000<br>Bei Verwendung von privaten Endpunkten: Ports im Bereich zwischen 0 und 65535 |

Azure Cosmos DB bietet ein einfaches und offenes RESTful-Programmiermodell über HTTPS. Darüber hinaus ist ein effizientes TCP-Protokoll vorhanden, das ebenfalls über ein RESTful-Kommunikationsmodell verfügt und über das .NET-Client-SDK verfügbar ist. Das TCP-Protokoll nutzt TLS für die erste Authentifizierung und Verschlüsselung des Datenverkehrs. Die beste Leistung erzielen Sie mit dem TCP-Protokoll.

Für das SDK V3 konfigurieren Sie den Verbindungsmodus beim Erstellen der `CosmosClient`-Instanz in `CosmosClientOptions`. Beachten Sie, dass der direkte Modus die Standardeinstellung darstellt.

```csharp
string connectionString = "<your-account-connection-string>";
CosmosClient client = new CosmosClient(connectionString,
new CosmosClientOptions
{
    ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
});
```

Da TCP nur im direkten Modus unterstützt wird, wird bei Verwendung des Gatewaymodus immer das HTTPS-Protokoll für die Kommunikation mit dem Gateway verwendet.

:::image type="content" source="./media/performance-tips/connection-policy.png" alt-text="Die Azure Cosmos DB-Verbindungsrichtlinie" border="false":::

**Kurzfristige Portauslastung**

Wenn Sie ein hohes Verbindungsaufkommen oder eine hohe Portauslastung Ihrer Instanzen feststellen, überprüfen Sie zunächst, ob die Clientinstanzen Singletons sind. Anders ausgedrückt: Die Clientinstanzen müssen für die Lebensdauer der Anwendung eindeutig sein.

Bei der Ausführung über das TCP-Protokoll optimiert der Client die Latenz mithilfe langlebiger Verbindungen. Im Gegensatz dazu werden beim HTTPS-Protokoll Verbindungen nach 2 Minuten Inaktivität getrennt.

Wenn Sie in Szenarien mit geringem Zugriff eine im Vergleich zum Zugriff im Gatewaymodus höhere Anzahl von Verbindungen feststellen, haben Sie folgende Möglichkeiten:

* Legen Sie die Eigenschaft [CosmosClientOptions.PortReuseMode](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.portreusemode) auf `PrivatePortPool` fest (gültig ab Frameworkversion 4.6.1 und ab .NET Core-Version 2.0): Über diese Eigenschaft kann das SDK einen kleinen Pool von kurzlebigen Ports für verschiedene Azure Cosmos DB-Zielendpunkte verwenden.
* Konfigurieren Sie die Eigenschaft [CosmosClientOptions.IdleConnectionTimeout](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.idletcpconnectiontimeout) so, dass sie größer gleich 10 Minuten ist. Die empfohlenen Werte liegen zwischen 20 Minuten und 24 Stunden.

<a id="same-region"></a>

**Platzieren von Clients in derselben Azure-Region, um die Leistung zu verbessern**

Platzieren Sie nach Möglichkeit sämtliche Anwendungen, die Azure Cosmos DB aufrufen, in der gleichen Region wie die Azure Cosmos DB-Datenbank. Damit Sie einen ungefähren Vergleich haben: Azure Cosmos DB-Aufrufe aus derselben Region werden normalerweise innerhalb von ca. 1 bis 2 ms abgeschlossen, während die Latenz zwischen West- und Ostküste der USA mehr als 50 ms beträgt. Diese Latenz variiert ggf. von Anforderung zu Anforderung und ist abhängig von der Route, die die Anforderung zwischen dem Client und der Grenze des Azure-Rechenzentrums nimmt. Die geringste Latenz erzielen Sie, wenn sich die aufrufende Anwendung in der gleichen Azure-Region wie der bereitgestellte Azure Cosmos DB-Endpunkt befindet. Eine Liste mit den verfügbaren Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services).

:::image type="content" source="./media/performance-tips/same-region.png" alt-text="Die Azure Cosmos DB-Verbindungsrichtlinie" border="false":::

   <a id="increase-threads"></a>

**Erhöhen der Anzahl von Threads/Aufgaben**

Da Azure Cosmos DB-Aufrufe über das Netzwerk erfolgen, müssen Sie eventuell den Parallelitätsgrad Ihrer Anforderungen variieren, um die Wartezeit für die Clientanwendung zwischen Anforderungen auf ein Minimum zu reduzieren. Erstellen Sie beispielsweise bei Verwendung der [Task Parallel Library](https://msdn.microsoft.com//library/dd460717.aspx) von .NET mehrere Hundert Aufgaben für Lese- und Schreibvorgänge in Azure Cosmos DB.

**Aktivieren des beschleunigten Netzwerkbetriebs**
 
 Um Latenzen und CPU-Jitter zu reduzieren, wird empfohlen, auf den virtuellen Clientcomputern den beschleunigten Netzwerkbetrieb zu aktivieren. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Windows-Computers mit beschleunigtem Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-powershell.md) oder [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md).

## <a name="sdk-usage"></a>SDK-Verwendung
**Installieren des neuesten SDKs**

Azure Cosmos DB-SDKs werden ständig verbessert, um eine optimale Leistung zu ermöglichen. Informationen zum neuesten SDK und zu den Verbesserungen finden Sie auf den Seiten unter [Azure Cosmos DB SDK](https://github.com/Azure/azure-cosmos-dotnet-v3).

**Verwenden von Stream-APIs**

[.NET SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) enthält Stream-APIs, die Daten ohne Serialisierung empfangen und zurückgeben können. 

Anwendungen der mittleren Ebene, die die Antworten aus dem SDK nicht direkt nutzen, sondern an andere Logikschichten weiterleiten, können von den Stream-APIs profitieren. Beispiele zur Handhabung von Streams finden Sie in den Beispielen für die [Elementverwaltung](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement).

**Verwenden eines Singleton-Azure Cosmos DB-Clients für die Lebensdauer der Anwendung**

Bei Verwendung des direkten Modus ist jede Instanz von `CosmosClient` threadsicher und verfügt über eine effiziente Verbindungsverwaltung und Adressenzwischenspeicherung. Zur Ermöglichung einer effizienten Verbindungsverwaltung und einer besseren Leistung des SDK-Clients empfiehlt es sich, für die Lebensdauer der Anwendung pro `AppDomain` eine einzelne Instanz zu verwenden.

Bei der Arbeit mit Azure Functions sollten die Instanzen ebenfalls den bestehenden [Richtlinien](../azure-functions/manage-connections.md#static-clients) folgen und eine einzelne Instanz beibehalten.

<a id="max-connection"></a>

**Deaktivieren der Inhaltsantwort bei Schreibvorgängen**

Legen Sie für Workloads mit hohen Erstellungsnutzlasten die Anforderungsoption „EnableContentResponseOnWrite“ auf „False“ fest. Der Dienst gibt die erstellte oder aktualisierte Ressource nicht mehr an das SDK zurück. Normalerweise verfügt die Anwendung über das zu erstellende Objekt, sodass sie den Dienst nicht benötigt, um es zurückzugeben. Die Headerwerte sind nach wie vor zugänglich wie die Gebühr für Anforderungen. Dies kann die Leistung verbessern, da das SDK keinen Speicher mehr zuweisen oder den Hauptteil der Antwort serialisieren muss. Dies reduziert auch die Auslastung der Netzwerkbandbreite, um die Leistung weiter zu steigern.  

```csharp
ItemRequestOption requestOptions = new ItemRequestOptions() { EnableContentResponseOnWrite = false };
ItemResponse<Book> itemResponse = await this.container.CreateItemAsync<Book>(book, new PartitionKey(book.pk), requestOptions);
// Resource will be null
itemResponse.Resource
```

**Aktivieren Sie die Massenbearbeitung zum Optimieren des Durchsatzes anstelle der Wartezeit**: Aktivieren Sie die Massenbearbeitung für Szenarien, in denen die Workload einen hohen Durchsatz erfordert und die Wartezeit nicht so wichtig ist. Weitere Informationen zum Aktivieren des Features und zu den Szenarien, in denen es verwendet werden sollte, finden Sie unter [Einführung zur Massenbearbeitung](https://devblogs.microsoft.com/cosmosdb/introducing-bulk-support-in-the-net-sdk).

**Erhöhen von System.Net MaxConnections pro Host bei Verwendung des Gatewaymodus**

Azure Cosmos DB-Anforderungen erfolgen im Gatewaymodus über HTTPS/REST. Sie unterliegen dem Standardverbindungslimit pro Hostname oder IP-Adresse. Unter Umständen müssen Sie einen höheren `MaxConnections`-Wert festlegen (100 – 1.000), damit die Clientbibliothek mehrere Verbindungen mit Azure Cosmos DB gleichzeitig nutzen kann. In .NET SDK 1.8.0 und höher ist der Standardwert für [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) „50“. Um den Wert zu ändern, können Sie [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) auf einen höheren Wert festlegen.

**Optimieren von parallelen Abfragen für partitionierte Sammlungen**

Das SQL .NET SDK unterstützt parallele Abfragen, die es Ihnen ermöglichen, einen partitionierten Container parallel abzufragen. Weitere Informationen finden Sie in den [Codebeispielen](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs) für die Arbeit mit den SDKs. Parallele Abfragen sind darauf ausgelegt, Latenz und Durchsatz im Vergleich zu seriellen Abfragen zu verbessern. Parallele Abfragen bieten zwei Parameter, die Sie an Ihre Anforderungen anpassen können: 
- `MaxConcurrency` steuert die maximale Anzahl von Partitionen, die parallel abgefragt werden können. 
- `MaxBufferedItemCount` steuert die Anzahl der vorab abgerufenen Ergebnisse.

***Optimierungsgrad der Parallelität***

Bei parallelen Abfragen werden mehrere Partitionen gleichzeitig abgefragt. Die Daten einer individuellen Partition werden in Bezug auf die Abfrage aber seriell abgerufen. Wenn Sie `MaxConcurrency` in [SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) auf die Anzahl von Partitionen festlegen, ist die Wahrscheinlichkeit am höchsten, dass die bestmögliche Leistung für die Abfrage erzielt wird (vorausgesetzt, alle anderen Systembedingungen bleiben unverändert). Wenn Sie die Anzahl der Partitionen nicht kennen, können Sie den Grad der Parallelität auf eine hohe Zahl festlegen. Das System wählt den Mindestwert (Anzahl der Partitionen, vom Benutzer bereitgestellte Eingabe) als Grad der Parallelität aus.

Für parallele Abfragen ergeben sich die größten Vorteile, wenn die Daten in Bezug auf die Abfrage gleichmäßig auf alle Partitionen verteilt sind. Wenn die partitionierte Sammlung so partitioniert ist, dass sich alle Daten bzw. die meisten Daten, die von einer Abfrage zurückgegeben werden, auf einigen wenigen Partitionen befinden (schlimmstenfalls nur auf einer Partition), können diese Partitionen Leistungsengpässe verursachen.

***Optimieren von MaxBufferedItemCount***
    
Parallele Abfragen sind so konzipiert, dass Ergebnisse vorab abgerufen werden, während der Client den aktuellen Batch der Ergebnisse verarbeitet. Dieser Vorababruf führt zu einer Verbesserung der allgemeinen Latenz einer Abfrage. Der Parameter `MaxBufferedItemCount` begrenzt die Anzahl von vorab abgerufenen Ergebnissen. Wenn Sie `MaxBufferedItemCount` auf die erwartete Anzahl von zurückgegebenen Ergebnissen (oder eine höhere Zahl) festlegen, ist der Vorteil durch das vorherige Abrufen für die Abfrage am größten.

Das vorherige Abrufen funktioniert unabhängig vom Parallelitätsgrad, und es ist nur ein Puffer für die Daten aller Partitionen vorhanden.  

**Implementieren eines Backoffs in RetryAfter-Intervallen**

Es empfiehlt sich, die Last während Leistungstests so lange erhöhen, bis eine geringe Menge von Anforderungen gedrosselt wird. Wenn die Anforderungen gedrosselt werden, sollte die Clientanwendung diese Drosselung für das vom Server angegebene Wiederholungsintervall aussetzen. Durch das Aussetzen wird die geringstmögliche Wartezeit zwischen den Wiederholungsversuchen gewährleistet. 

Weitere Informationen finden Sie unter [RetryAfter](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosexception.retryafter?view=azure-dotnet#Microsoft_Azure_Cosmos_CosmosException_RetryAfter).
    
Es gibt einen Mechanismus zum Protokollieren zusätzlicher Diagnoseinformationen und zum Beheben von Problemen mit der Wartezeit, wie im folgenden Beispiel gezeigt. Sie können die Diagnosezeichenfolge für Anforderungen protokollieren, die eine höhere Leselatenz aufweisen. Mithilfe der erfassten Diagnosezeichenfolge können Sie ermitteln, wie häufig bei einer bestimmten Anforderung der Fehler 429 aufgetreten ist.

```csharp
ItemResponse<Book> readItemResponse = await this.cosmosContainer.ReadItemAsync<Book>("ItemId", new PartitionKey("PartitionKeyValue"));
readItemResponse.Diagnostics.ToString(); 
```

**Erhöhen der Anzahl von Threads/Aufgaben**

Siehe [Erhöhen der Anzahl von Threads/Aufgaben](#increase-threads) im Abschnitt „Netzwerk“ dieses Artikels.

## <a name="indexing-policy"></a>Indizierungsrichtlinie
 
**Beschleunigen von Schreibvorgängen durch Ausschließen nicht verwendeter Pfade von der Indizierung**

Die Indizierungsrichtlinie von Azure Cosmos DB ermöglicht auch die Verwendung von Indizierungspfaden („IndexingPolicy.IncludedPaths“ und „IndexingPolicy.ExcludedPaths“), um anzugeben, welche Dokumentpfade in die Indizierung ein- bzw. ausgeschlossen werden sollen. Die alleinige Indizierung der erforderlichen Pfade kann die Schreibleistung verbessern, die verbrauchten RUs für Schreibvorgänge reduzieren und die Indexspeicherung für Szenarien verringern, in denen die Abfragemuster im Voraus bekannt sind. Dies liegt daran, dass der Indizierungsaufwand direkt mit der Anzahl der indizierten eindeutigen Pfade zusammenhängt. Der folgende Code zeigt beispielsweise, wie Sie einen vollständigen Abschnitt der Dokumente (eine Unterstruktur) mit dem Platzhalter „*“ von der Indizierung ausschließen:

```csharp
var containerProperties = new ContainerProperties(id: "excludedPathCollection", partitionKeyPath: "/pk" );
containerProperties.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
containerProperties.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
Container container = await this.cosmosDatabase.CreateContainerAsync(containerProperties);
```

Weitere Informationen finden Sie unter [Indizierungsrichtlinien für Azure Cosmos DB](index-policy.md).

## <a name="throughput"></a>Throughput
<a id="measure-rus"></a>

**Messen und Optimieren zum Senken der erforderlichen Anforderungseinheiten pro Sekunde**

Azure Cosmos DB bietet einen umfangreichen Satz von Datenbankvorgängen. Diese Vorgänge umfassen relationale und hierarchische Abfragen mit UDFs, gespeicherte Prozeduren und Trigger, die alle in den Dokumenten innerhalb einer Datenbanksammlung ausgeführt werden. Die Kosten im Zusammenhang mit diesen Vorgängen variieren in Abhängigkeit vom CPU-, E/A- und Speicheraufwand, der für den jeweiligen Vorgang erforderlich ist. Anstelle sich Gedanken über Hardwareressourcen und deren Verwaltung zu machen, können Sie sich eine Anforderungseinheit (RU) als alleinige Maßeinheit für die Ressourcen vorstellen, die für das Durchführen der verschiedenen Datenbankvorgänge und das Ausführen einer Anwendungsanforderung erforderlich sind.

Der Durchsatz wird basierend auf der für jeden Container festgelegten Anzahl von [Anforderungseinheiten](request-units.md) bereitgestellt. Der Verbrauch von Anforderungseinheiten wird als Rate pro Sekunde bemessen. Anwendungen, die die bereitgestellte Rate von Anforderungseinheiten für ihre Container überschreiten, werden begrenzt, bis die Rate wieder unter das bereitgestellte Niveau für den Container fällt. Wenn Ihre Anwendung einen höheren Durchsatz erfordert, können Sie ihn durch Bereitstellung zusätzlicher Anforderungseinheiten erhöhen.

Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl von Prädikaten, die Art der Prädikate, die Anzahl von UDFs und die Größe des Quelldatasets beeinflussen die Kosten von Abfragevorgängen.

Untersuchen Sie zum Ermitteln des Aufwands für einen Vorgang (Erstellen, Aktualisieren oder Löschen) den Header [x-ms-request-charge](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) (oder die entsprechende `RequestCharge`-Eigenschaft in `ResourceResponse\<T>` oder `FeedResponse\<T>` im .NET SDK), um die Anzahl von Anforderungseinheiten zu ermitteln, die von diesen Vorgängen verbraucht werden:

```csharp
// Measure the performance (Request Units) of writes
ItemResponse<Book> response = await container.CreateItemAsync<Book>(myBook, new PartitionKey(myBook.PkValue));
Console.WriteLine("Insert of item consumed {0} request units", response.RequestCharge);
// Measure the performance (Request Units) of queries
FeedIterator<Book> queryable = container.GetItemQueryIterator<ToDoActivity>(queryString);
while (queryable.HasMoreResults)
    {
        FeedResponse<Book> queryResponse = await queryable.ExecuteNextAsync<Book>();
        Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
    }
```             

Bei den in diesem Header zurückgegebenen Anforderungskosten handelt es sich um einen Bruchteil Ihres bereitgestellten Durchsatzes (2.000 RU/s). Falls die obige Abfrage also beispielsweise 1.000 Dokumente mit einer Größe von 1 KB zurückgibt, fallen für den Vorgang Kosten in Höhe von 1.000 an. Somit werden vom Server innerhalb einer Sekunde nur zwei solcher Anforderungen berücksichtigt, und für weitere Anforderungen wird die Rate begrenzt. Weitere Informationen finden Sie unter [Anforderungseinheiten](request-units.md) sowie unter dem [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner).
<a id="429"></a>

**Behandeln von Ratenbeschränkungen/zu hohen Anforderungsraten**

Wenn ein Client versucht, den für ein Konto reservierten Durchsatz zu überschreiten, wird die Serverleistung nicht beeinträchtigt, und es wird kein über die reservierte Kapazität hinausgehender Durchsatz in Anspruch genommen. Der Server beendet die Anforderung vorab mit RequestRateTooLarge (HTTP-Statuscode 429). Er gibt einen [x-ms-retry-after-ms](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers)-Header zurück, der die Zeitspanne in Millisekunden angibt, die der Benutzer warten muss, bevor er die Anforderung erneut versuchen kann.

```xml
    HTTP Status 429,
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100
```

Alle SDKs fangen diese Antwort implizit ab, berücksichtigen den vom Server angegebenen Header vom Typ „retry-after“ und wiederholen die Anforderung. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

Wenn insgesamt mehrere Clients beständig die Anforderungsrate überschreiten, reicht die standardmäßige Wiederholungsanzahl (vom Client intern auf 9 festgelegt) möglicherweise nicht aus. In diesem Fall löst der Client eine CosmosException mit dem Statuscode 429 für die Anwendung aus. 

Sie können die Standardanzahl von Wiederholungsversuchen ändern, indem Sie die `RetryOptions` für die `CosmosClientOptions`-Instanz festlegen. Die CosmosException-Klasse mit dem Statuscode 429 wird standardmäßig nach einer kumulierten Wartezeit von 30 Sekunden zurückgegeben, wenn die Anforderung weiterhin die Anforderungsrate übersteigt. Dieser Fehler wird auch dann zurückgegeben, wenn die aktuelle Wiederholungsanzahl unter der maximalen Wiederholungsanzahl liegt – ganz gleich, ob es sich dabei um den Standardwert (9) oder um einen benutzerdefinierten Wert handelt.

Das automatisierte Wiederholungsverhalten trägt dazu bei, die Resilienz und Benutzerfreundlichkeit für die meisten Anwendungen zu verbessern. Dies stellt jedoch möglicherweise nicht das beste Verhalten dar, wenn Sie Leistungsbenchmarks durchführen, und insbesondere nicht beim Messen der Latenz. Die Wartezeit für den Client nimmt stark zu, wenn das Experiment die Serverdrosselung erreicht und damit die automatische Wiederholung durch das Client-SDK auslöst. Ermitteln Sie zur Vermeidung von Latenzspitzenwerten bei Leistungsexperimenten die von den einzelnen Vorgängen zurückgegebene Belastung, und stellen Sie sicher, dass die Anforderungen die reservierte Anforderungsrate nicht überschreiten. Weitere Informationen finden Sie unter [Anforderungseinheiten](request-units.md).

**Verwenden eines auf kleinere Dokumente ausgerichteten Designs für einen höheren Durchsatz**

Der Anforderungsaufwand (also die Kosten für die Anforderungsverarbeitung) eines Vorgangs hängt direkt mit der Größe des Dokuments zusammen. Vorgänge für große Dokumente sind teurer als Vorgänge für kleine Dokumente.

## <a name="next-steps"></a>Nächste Schritte
Eine Beispielanwendung zur Evaluierung von Azure Cosmos DB für Hochleistungsszenarien auf einigen Clientcomputern finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).

Weitere Informationen zum Entwerfen einer auf Skalierung und hohe Leistung ausgelegten Anwendung finden Sie unter [Partitionieren und Skalieren in Azure Cosmos DB](partition-data.md).
