---
title: Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB
description: Verwenden Sie Features wie clientseitige Protokollierung und andere Tools von Drittanbietern, um bei Verwenden des .NET SDK Probleme im Zusammenhang mit Azure Cosmos DB zu erkennen, zu diagnostizieren und zu beheben.
author: anfeldma-ms
ms.service: cosmos-db
ms.date: 05/06/2020
ms.author: anfeldma
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 55c462795b29cd678a5fd7816211bce720d554e1
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170357"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB

> [!div class="op_single_selector"]
> * [Java SDK v4](troubleshoot-java-sdk-v4-sql.md)
> * [Asynchrones Java SDK v2](troubleshoot-java-async-sdk.md)
> * [.NET](troubleshoot-dot-net-sdk.md)
> 

Dieser Artikel behandelt allgemeine Probleme, Problemumgehungen, Diagnoseschritte und Tools bei der Verwendung des [.NET SDK](sql-api-sdk-dotnet.md) mit Azure Cosmos DB-SQL-API-Konten.
Das .NET SDK bietet eine clientseitige logische Darstellung für den Zugriff auf die Azure Cosmos DB-SQL-API. Dieser Artikel beschreibt hilfreiche Tools und Vorgehensweisen für Problemfälle.

## <a name="checklist-for-troubleshooting-issues"></a>Checkliste für die Problembehandlung
Gehen Sie die folgende Prüfliste durch, bevor Sie Ihre Anwendung in die Produktion überführen. Mithilfe der Prüfliste werden einige häufige Probleme vermieden, die möglicherweise auftreten. Sie können ein Problem auch schnell diagnostizieren, sobald es auftritt:

*    Verwenden Sie das neueste [SDK](sql-api-sdk-dotnet-standard.md). SDKs in der Vorschauversion sollten nicht für die Produktion verwendet werden. Dadurch wird verhindert, dass bekannte Probleme, die bereits behoben wurden, erneut auftreten.
*    Überprüfen Sie die [Leistungstipps](performance-tips.md), und implementieren Sie die Empfehlungen. Dadurch können Skalierungs-, Latenz- und andere Leistungsprobleme vermieden werden.
*    Aktivieren Sie die SDK-Protokollierung, um die Problembehandlung zu unterstützen. Die Aktivierung der Protokollierung kann die Leistung beeinträchtigen, weshalb es am besten ist, sie nur für die Problembehandlung zu aktivieren. Sie können die folgenden Protokolle aktivieren:
    *    [Protokollmetriken](monitor-accounts.md) im Azure-Portal. Metriken im Portal zeigen die Azure Cosmos DB-Telemetriedaten, die hilfreich sind, um festzustellen, ob das Problem im Zusammenhang mit Azure Cosmos DB steht oder ob es von der Clientseite kommt.
    *    Protokollieren Sie die [DiagnosticsString](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring)-Eigenschaft im V2 SDK oder die [Diagnostics](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics)-Eigenschaft im V3 SDK in den Antworten der Punktoperationen.
    *    Protokollieren Sie in allen Abfrageantworten die [SQL-Abfragemetriken](sql-api-query-metrics.md). 
    *    Führen Sie das Setup für die [SDK-Protokollierung]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md) aus.

Sehen Sie sich den Abschnitt [Häufig auftretende Probleme und Problemumgehungen](#common-issues-workarounds) in diesem Artikel an.

Überprüfen Sie den [Problemabschnitt auf GitHub](https://github.com/Azure/azure-cosmos-dotnet-v2/issues), der aktiv überwacht wird. Überprüfen Sie, ob Sie ein ähnliches Problem finden, für das bereits eine Problemumgehung dokumentiert wurde. Wenn Sie keine Lösung gefunden haben, legen Sie ein GitHub-Problem an. Bei dringenden Problemen können Sie ein Supportticket öffnen.


## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>Häufig auftretende Probleme und Problemumgehungen

### <a name="general-suggestions"></a>Allgemeine Empfehlungen
* Führen Sie Ihre App nach Möglichkeit in der gleichen Azure-Region wie Ihr Azure Cosmos DB-Konto aus. 
* Aufgrund fehlender Ressourcen auf Ihrem Clientcomputer kann es zu Verbindungs-/Verfügbarkeitsproblemen kommen. Wir empfehlen, die CPU-Auslastung auf Knoten zu überwachen, auf denen der Azure Cosmos DB-Client ausgeführt wird, und hoch bzw. zentral zu skalieren, wenn ihre Last hoch ist.

### <a name="check-the-portal-metrics"></a>Überprüfen der Metriken im Portal
Das Überprüfen der [Metriken im Portal](monitor-accounts.md) hilft festzustellen, ob es sich um ein Problem auf Clientseite handelt oder ob es ein Problem mit dem Dienst gibt. Wenn die Metriken beispielsweise viele Anforderungen mit Ratenbegrenzungen aufweisen (HTTP-Statuscode 429), bedeutet dies, dass die Anforderung gedrosselt wird. Siehe dann den Abschnitt [Anforderungsrate zu hoch]. 

### <a name="requests-timeouts"></a><a name="request-timeouts"></a>Timeouts von Anforderungen
Ein Timeout einer Anforderung tritt in der Regel bei Verwenden von Direkt-/TCP-Verbindungen auf, kann aber auch im Gatewaymodus auftreten. Nachstehend sind die Fehler der häufigsten bekannten Ursachen und Vorschläge zur Behebung des Problems aufgeführt.

* Die CPU-Auslastung ist hoch, was zu Wartezeiten und/oder Timeouts bei Anforderungen führt. Der Kunde kann den Hostcomputer hochskalieren, um ihn mit mehr Ressourcen auszustatten, oder die Last auf mehrere Computer verteilen.
* Die Verfügbarkeit von Sockets und Ports kann niedrig sein. Bei Ausführung in Azure kann es auf Clients, die das .NET SDK verwenden, zur Azure SNAT-Portauslastung (PAT) kommen. Um die Wahrscheinlichkeit zu verringern, dass dieses Problem auftritt, verwenden Sie die neueste Version 2.x oder 3.x des .NET SDK. Dies ist ein Beispiel dafür, warum es empfehlenswert ist, immer die neueste SDK-Version zu verwenden.
* Das Erstellen mehrerer DocumentClient-Instanzen kann zu Verbindungskonflikten und Timeoutproblemen führen. Befolgen Sie die [Leistungstipps](performance-tips.md), und verwenden Sie eine einzige „DocumentClient“-Instanz für den gesamten Prozess.
* Benutzer beobachten mitunter erhöhte Latenzen oder Timeouts bei Anforderungen, weil ihre Sammlungen unzureichend bereitgestellt sind, das Back-End Anforderungen drosselt und der Client intern Vorgänge wiederholt. Überprüfen Sie die [Metriken im Portal](monitor-accounts.md).
* Azure Cosmos DB verteilt den gesamten bereitgestellten Durchsatz gleichmäßig auf die physischen Partitionen. Überprüfen Sie die Metriken im Portal, um festzustellen, ob die Workload einen sehr aktiven [Partitionsschlüssel](partition-data.md) vorfindet. Dies führt dazu, dass der gesamte in Anspruch genommene Durchsatz (Anforderungseinheiten pro Sekunde, RU/s) anscheinend unter den bereitgestellten RU/s liegt, während der von einer einzelnen Partition genutzte Durchsatz (RU/s) den bereitgestellten Durchsatz übersteigt. 
* Zusätzlich fügt das SDK 2.0 Direkt-/TCP-Verbindungen eine Kanalsemantik hinzu. Eine TCP-Verbindung wird für mehrere Anforderungen gleichzeitig verwendet. Dies kann unter bestimmten Umständen zu zwei Problemen führen:
    * Ein hoher Grad an Parallelität kann zu Konflikten im Kanal führen.
    * Große Anforderungen oder Antworten können zu Head-of-Line-Blockierungen im Kanal führen und die Konfliktsituation verschärfen, selbst bei einem relativ geringen Grad an Parallelität.
    * Wenn der Fall zu einer dieser beiden Kategorien gehört (oder wenn hohe CPU-Auslastung vermutet wird), sind folgende Abhilfemaßnahmen möglich:
        * Versuchen Sie, die Anwendung zentral/hoch zu skalieren.
        * Darüber hinaus können über den [Ablaufverfolgungslistener](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) SDK-Protokolle erfasst werden, um weitere Details zu erhalten.

### <a name="high-network-latency"></a><a name="high-network-latency"></a>Hohe Netzwerklatenz
Eine hohe Netzwerklatenz kann mithilfe der [DiagnosticsString](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet)-Eigenschaft im V2 SDK oder der [Diagnostics](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics?view=azure-dotnet#Microsoft_Azure_Cosmos_ResponseMessage_Diagnostics)-Eigenschaft im V3 SDK ermittelt werden.

Wenn keine [Timeouts](#request-timeouts) vorhanden sind und die Diagnose einzelne Anforderungen zeigt, bei denen die hohe Latenz durch den Unterschied zwischen `ResponseTime` und `RequestStartTime` wie folgt ersichtlich ist (in diesem Beispiel > 300 Millisekunden):

```bash
RequestStartTime: 2020-03-09T22:44:49.5373624Z, RequestEndTime: 2020-03-09T22:44:49.9279906Z,  Number of regions attempted:1
ResponseTime: 2020-03-09T22:44:49.9279906Z, StoreResult: StorePhysicalAddress: rntbd://..., ...
```

Diese Latenz kann mehrere Ursachen haben:

* Die Anwendung wird nicht in der gleichen Region ausgeführt, in der sich auch Ihr Azure Cosmos DB-Konto befindet.
* Die [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations)- oder [ApplicationRegion](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationregion)-Konfiguration ist falsch und versucht, eine Verbindung zu einer anderen Region herzustellen, in der die Anwendung derzeit ausgeführt wird.
* Durch hohen Datenverkehr kann es zu einem Engpass in der Netzwerkschnittstelle kommen. Wenn die Anwendung auf virtuellen Azure-Computern ausgeführt wird, gibt es mögliche Problemumgehungen:
    * Erwägen Sie die Erstellung eines [virtuellen Computers, auf dem der beschleunigte Netzwerkbetrieb aktiviert ist](../virtual-network/create-vm-accelerated-networking-powershell.md).
    * Aktivieren Sie den [beschleunigten Netzwerkbetrieb auf einem vorhandenen virtuellen Computer](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms).
    * Verwenden Sie ggf. einen [leistungsfähigeren virtuellen Computer](../virtual-machines/windows/sizes.md).

### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT-Portauslastung (PAT)

Wenn Ihre App auf einem [virtuellen Azure-Computer ohne öffentliche IP-Adresse](../load-balancer/load-balancer-outbound-connections.md#defaultsnat) bereitgestellt wird, werden standardmäßig [Azure SNAT-Ports](../load-balancer/load-balancer-outbound-connections.md#preallocatedports) verwendet, um Verbindungen mit beliebigen Endpunkten außerhalb Ihres virtuellen Computers herzustellen. Die Anzahl zulässiger Verbindungen des virtuellen Computers mit dem Azure Cosmos DB-Endpunkt wird durch die [Azure SNAT-Konfiguration](../load-balancer/load-balancer-outbound-connections.md#preallocatedports) eingeschränkt. Diese Situation kann zu einer Bandbreiteneinschränkung der Verbindung, zum Beenden der Verbindung oder den oben erwähnten [Timeouts von Anforderungen](#request-timeouts) führen.

 Azure SNAT-Ports werden nur verwendet, wenn Ihr virtueller Computer eine private IP-Adresse besitzt und eine Verbindung mit einer öffentlichen IP-Adresse hergestellt wird. Es gibt zwei Problemumgehungen, um die Azure-SNAT-Einschränkung zu vermeiden (vorausgesetzt, dass Sie bereits eine einzelne Clientinstanz für die gesamte Anwendung verwenden):

* Fügen Sie Ihren Azure Cosmos DB-Dienstendpunkt dem Subnetz Ihres virtuellen Netzwerks von Azure Virtual Machines hinzu. Weitere Informationen finden Sie unter [Azure Virtual Network-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md). 

    Wenn der Dienstendpunkt aktiviert ist, werden die Anforderungen nicht mehr von einer öffentlichen IP-Adresse an Azure Cosmos DB gesendet. Stattdessen wird die Identität des virtuellen Netzwerks und des Subnetzes gesendet. Diese Änderung kann zu Firewallproblemen führen, wenn nur öffentliche IP-Adressen zulässig sind. Wenn Sie eine Firewall verwenden und den Dienstendpunkt aktivieren, fügen Sie der Firewall mithilfe von [VNET-ACLs](../virtual-network/virtual-networks-acl.md) ein Subnetz hinzu.
* Weisen Sie Ihrem [virtuellen Azure-Computer eine öffentliche IP-Adresse](../load-balancer/troubleshoot-outbound-connection.md#assignilpip) zu.

### <a name="http-proxy"></a>HTTP-Proxy
Wenn Sie einen HTTP-Proxy verwenden, vergewissern Sie sich, dass er die Anzahl von Verbindungen unterstützt, die in `ConnectionPolicy` des SDK konfiguriert ist.
Andernfalls treten Verbindungsprobleme auf.

### <a name="request-rate-too-large"></a><a name="request-rate-too-large"></a>Anforderungsrate zu hoch
„Anforderungsrate zu groß“ oder Fehlercode 429 bedeutet, dass Ihre Anforderungen gedrosselt werden, weil der in Anspruch genommene Durchsatz (RU/s) den [bereitgestellten Durchsatz](set-throughput.md) überschritten hat. Das SDK wiederholt Anforderungen basierend auf der angegebenen [Wiederholungsrichtlinie](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet) automatisch. Wenn dieser Fehler häufig auftritt, erwägen Sie das Erhöhen des Durchsatzes für die Sammlung. Überprüfen Sie die [Metriken im Portal](use-metrics.md) auf 429-Fehler. Überprüfen Sie Ihren [Partitionsschlüssel](partitioning-overview.md#choose-partitionkey), um sicherzustellen, dass dieser zu einer gleichmäßigen Verteilung von Speicher- und Anforderungsvolumen führt. 

### <a name="slow-query-performance"></a>Schwache Abfrageleistung
Anhand der [Metriken zu Abfragen](sql-api-query-metrics.md) können Sie bestimmen, wo die Abfrage die meiste Zeit verbringt. Sie können erkennen, wie viel Zeit im Back-End und auf dem Client verbracht wird.
* Wenn die Back-End-Abfrage schnell zurückgegeben wird und viel Zeit auf den Client verbringt, überprüfen Sie die Last auf dem Computer. Es ist wahrscheinlich, dass nicht genügend Ressourcen vorhanden sind und das SDK darauf wartet, dass Ressourcen für die Bearbeitung der Antwort verfügbar werden.
* Wenn die Back-End-Abfrage langsam ist, versuchen Sie, [die Abfrage zu optimieren](optimize-cost-queries.md), und sehen Sie sich die aktuelle [Indexierungsrichtlinie](index-overview.md) an. 

### <a name="http-401-the-mac-signature-found-in-the-http-request-is-not-the-same-as-the-computed-signature"></a>HTTP 401: Die in der HTTP-Anforderung gefundene MAC-Signatur entspricht nicht der berechneten Signatur
Wenn Sie die folgende 401-Fehlermeldung erhalten haben: „Die in der HTTP-Anforderung gefundene MAC-Signatur entspricht nicht der berechneten Signatur.“ kann durch die folgenden Szenarien verursacht werden.

1. Der Schlüssel wurde rotiert und entsprach nicht den [bewährten Methoden](secure-access-to-data.md#key-rotation). Dies ist normalerweise der Fall. Abhängig von der Größe des Cosmos DB-Kontos kann die Schlüsselrotation für das Cosmos DB-Konto nur wenige Sekunden bis möglicherweise Tage dauern.
   1. Die 401-MAC-Signatur wird kurz nach der Schlüsselrotation angezeigt und schließlich ohne Änderungen angehalten. 
2. Der Schlüssel ist für die Anwendung falsch konfiguriert, sodass er nicht mit dem Konto übereinstimmt.
   1. Das Problem mit der 401-MAC-Signatur ist konsistent und tritt bei allen Aufrufen auf.
3. Es liegt eine Racebedingung bei der Containererstellung vor. Eine Anwendungsinstanz versucht, auf den Container zuzugreifen, bevor die Containererstellung abgeschlossen ist. Dies ist das häufigste Szenario, wenn die Anwendung ausgeführt wird und der Container gelöscht und mit demselben Namen neu erstellt wird, während die Anwendung ausgeführt wird. Das SDK versucht, den neuen Container zu verwenden, aber die Containererstellung wird noch ausgeführt, sodass die Schlüssel noch nicht verfügbar sind.
   1. Das Problem mit der 401-MAC-Signatur tritt kurz nach der Erstellung eines Containers auf und besteht nur, bis die Containererstellung abgeschlossen ist.
 
 ### <a name="http-error-400-the-size-of-the-request-headers-is-too-long"></a>HTTP-Fehler 400. Die Anforderungsheader sind zu lang.
 Der Header ist zu groß geworden und überschreitet die maximal zulässige Größe. Es wird immer empfohlen, das aktuelle SDK zu verwenden. Stellen Sie sicher, dass Sie mindestens Version [3.x](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/changelog.md) oder [2.x](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md) verwenden. In dieser Version wird der Ausnahmemeldung die Verfolgung der Headergröße hinzugefügt.

Ursachen:
 1. Das Sitzungstoken ist zu groß geworden. Die Größe des Sitzungstokens nimmt mit der steigenden Anzahl von Partitionen im Container zu.
 2. Das Fortsetzungstoken ist zu groß geworden. Bei verschiedenen Abfragen sind die Fortsetzungstoken unterschiedlich groß.
 3. Der Fehler wird durch eine Kombination aus Sitzungstoken und Fortsetzungstoken verursacht.

Lösung:
   1. Befolgen Sie die [Leistungstipps](performance-tips.md), und konvertieren Sie die Anwendung in den Verbindungsmodus „Direkt + TCP“. Bei „Direkt + TCP“ besteht keine Größenbeschränkung für Header wie bei HTTP, wodurch dieses Problem vermieden wird.
   2. Wenn das Sitzungstoken die Ursache ist, besteht eine vorübergehende Minderung darin, die Anwendung neu zu starten. Durch einen Neustart der Anwendungsinstanz wird das Sitzungstoken zurückgesetzt. Wenn die Ausnahmen nach dem Neustart nicht mehr auftreten, bestätigt dies, dass das Sitzungstoken die Ursache ist. Es nimmt schließlich wieder die Größe an, die die Ausnahme verursacht.
   3. Wenn die Anwendung nicht in „Direkt + TCP“ konvertiert werden kann und das Sitzungstoken die Ursache ist, kann die Minderung durch Ändern der [Konsistenzebene](consistency-levels.md) des Clients erfolgen. Das Sitzungstoken wird nur für die Sitzungskonsistenz verwendet. Dabei handelt es sich um die Standardkonsistenzebene für Cosmos DB. Für andere Konsistenzebenen wird das Sitzungstoken nicht verwendet. 
   4. Wenn die Anwendung nicht in „Direkt + TCP“ konvertiert werden kann und das Fortsetzungstoken die Ursache ist, versuchen Sie, die Option „ResponseContinuationTokenLimitInKb“ festzulegen. Die Option finden Sie in V2 unter „FeedOptions“ oder in V3 unter „QueryRequestOptions“.

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[Anforderungsrate zu hoch]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Azure SNAT (PAT) port exhaustion]: #snat
[Production check list]: #production-check-list
