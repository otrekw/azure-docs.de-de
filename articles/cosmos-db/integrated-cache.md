---
title: Integrierter Azure Cosmos DB-Cache
description: Der integrierte Azure Cosmos DB-Cache ist ein In-Memory-Cache, der Sie dabei unterstützt, überschaubare Kosten und niedrige Latenzen bei wachsendem Anforderungsvolumen zu gewährleisten.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/25/2021
ms.author: tisande
ms.openlocfilehash: f0a0556ce2a46f922e387d96d20b6425ab362580
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385388"
---
# <a name="azure-cosmos-db-integrated-cache---overview-preview"></a>Übersicht: Integrierter Azure Cosmos DB-Cache (Vorschauversion)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Der integrierte Azure Cosmos DB-Cache ist ein In-Memory-Cache, der Sie dabei unterstützt, überschaubare Kosten und niedrige Latenzen bei wachsendem Anforderungsvolumen zu gewährleisten. Die Einrichtung des integrierten Caches ist einfach, und Sie müssen keine Zeit damit verbringen, benutzerdefinierten Code für die Cacheinvalidierung oder zum Verwalten der Back-End-Infrastruktur zu schreiben. Ihr integrierter Cache nutzt ein [dediziertes Gateway](dedicated-gateway.md) in Ihrem Azure Cosmos DB-Konto. Der integrierte Cache ist das erste von vielen Azure Cosmos DB-Features, die ein dediziertes Gateway nutzen, um die Leistung zu verbessern. Sie können eine von drei dedizierten Gatewaygrößen anhand der Anzahl von Kernen und des Speichers auswählen, die Sie für Ihre Workload benötigen.

Ein integrierter Cache wird automatisch innerhalb des dedizierten Gateways konfiguriert. Der integrierte Cache besteht aus zwei Teilen: 

* Ein Elementcache für Punktlesevorgänge 
* Ein Abfragecache für Abfragen

Der integrierte Cache ist ein Lese-/Schreibcache mit einer Entfernungsrichtlinie nach seltener Verwendung. Der Elementcache und der Abfragecache teilen sich die gleiche Kapazität innerhalb des integrierten Caches, und die Entfernungsrichtlinie nach seltener Verwendung gilt für beide. Das bedeutet, das Entfernen von Daten aus dem Cache erfolgt streng danach, welche am längsten nicht verwendet wurden, unabhängig davon, ob es sich um einen Punktlesevorgang oder eine Abfrage handelt.

## <a name="workloads-that-benefit-from-the-integrated-cache"></a>Vom integrierten Cache profitierende Workloads

Das Hauptziel des integrierten Caches ist die Reduzierung der Kosten von Workloads mit vielen Lesevorgängen. Die niedrige Latenz ist zwar nützlich, aber nicht der Hauptvorteil des integrierten Caches, da Azure Cosmos DB auch ohne die Zwischenspeicherung bereits schnell ist.

Punktlesevorgänge und Abfragen, die den integrierten Cache betreffen, nutzen keine RUs. Das bedeutet, für alle Cachetreffer fallen keine RU-Gebühren an. Cachetreffer bieten deutlich geringere Kosten pro Vorgang als Lesevorgänge aus der Back-End-Datenbank.

Für Workloads, die den folgenden Merkmalen entsprechen, sollte in Betracht gezogen werden, ob der integrierte Cache zu Kostenvergünstigungen führt:

-   Workloads mit vielen Lesevorgängen
-   Viele wiederholte Punktlesevorgänge für große Elemente
-   Viele wiederholte Abfragen mit großen Mengen von RUs
-   Häufig angeforderte Partitionsschlüssel für Lesevorgänge

Der größte Faktor der erwarteten Einsparungen ist der Grad, in dem sich Lesevorgänge wiederholen. Wenn Ihre Workload konsistent dieselben Punktlesevorgänge oder Abfragen in einem kurzen Zeitraum ausführt, eignet sie sich bestens für den integrierten Cache. Wenn Sie den integrierten Cache für wiederholte Lesevorgänge verwenden, werden RUs nur für den ersten Lesevorgang verwendet. Nachfolgende Lesevorgänge, die über denselben dedizierten Gatewayknoten geleitet werden (im `MaxIntegratedCacheStaleness`-Fenster, sofern die Daten nicht entfernt wurden), nutzen den Durchsatz nicht.

Für einige Workloads sollte der integrierte Cache nicht in Betracht gezogen werden, zum Beispiel:

-   Workloads mit vielen Schreibvorgängen
-  Selten wiederholte Punktlesevorgänge oder Abfragen

## <a name="item-cache"></a>Elementcache

Sie können den Elementcache für Punktlesevorgänge verwenden (d. h., Schlüssel-/Wertsuchen anhand der Element-ID und des Partitionsschlüssels).

### <a name="populating-the-item-cache"></a>Auffüllen des Elementcaches

- Neue Schreib-, Update- und Löschvorgänge werden automatisch im Elementcache aufgefüllt.
- Wenn Ihre App versucht, ein bestimmtes Element zu lesen, das sich zuvor nicht im Cache befand (Cachefehler), würde das Element nun im Elementcache gespeichert werden.

### <a name="item-cache-invalidation-and-eviction"></a>Elementcacheinvalidierung und -entfernung

- Aktualisieren oder Löschen von Elementen
- Seltene Verwendung
- Cacheaufbewahrungszeit (d. h. `MaxIntegratedCacheStaleness`)

## <a name="query-cache"></a>Abfragecache

Der Abfragecache kann zum Zwischenspeichern von Abfragen verwendet werden. Der Abfragecache transformiert eine Abfrage in eine Schlüssel-/Wertsuche, bei der der Schlüssel dem Abfragetext und der Wert den Abfrageergebnissen entsprechen. Der integrierte Cache verfügt über keine Abfrage-Engine, er speichert lediglich die Schlüssel-/Wertsuche für jede Abfrage.

### <a name="populating-the-query-cache"></a>Auffüllen des Abfragecaches

- Wenn der Cache über kein Ergebnis für diese Abfrage verfügt (Cachefehler), wird die Abfrage an das Back-End gesendet. Nachdem die Abfrage ausgeführt wurde, speichert der Cache die Ergebnisse für diese Abfrage.

### <a name="query-cache-eviction"></a>Abfragecacheentfernung

- Seltene Verwendung
- Cacheaufbewahrungszeit (d. h. `MaxIntegratedCacheStaleness`)

### <a name="working-with-the-query-cache"></a>Arbeiten mit dem Abfragecache

Sie benötigen keinen speziellen Code, wenn Sie mit dem Abfragecache arbeiten, selbst wenn die Ergebnisse Ihrer Abfragen mehrere Seiten umfassen. Die bewährten Methoden und der Code für die Abfragepaginierung sind unabhängig davon identisch, ob Ihre Abfrage den integrierten Cache betrifft oder für die Back-End-Abfrage-Engine ausgeführt wird.

Abfragefortsetzungstoken werden ggf. automatisch im Abfragecache zwischengespeichert. Wenn die Ergebnisse Ihrer Abfrage mehrere Seiten umfassen, fallen für Seiten keine RU-Gebühren an, die im integrierten Cache gespeichert sind. Wenn für nachfolgende Seiten mit Abfrageergebnissen Back-End-Ausführung erforderlich ist, verfügen diese über ein Fortsetzungstoken der vorherigen Seite, um die Duplizierung des vorherigen Arbeitsaufwands zu vermeiden.

> [!NOTE]
> Integrierte Cache-Instanzen in unterschiedlichen dedizierten Gatewayknoten verfügen über voneinander unabhängige Caches. Wenn Daten in einem Knoten zwischengespeichert werden, werden diese nicht unbedingt in anderen zwischengespeichert.

## <a name="integrated-cache-consistency"></a>Konsistenz des integrierten Caches

Der integrierte Cache unterstützt nur die letztliche [Konsistenz](consistency-levels.md). Wenn ein Lesevorgang über einen konsistenten Präfix, eine Sitzung, begrenzte Veraltung oder starke Konsistenz verfügt, umgeht dieser immer den integrierten Cache.

Die einfachste Vorgehensweise zum Konfigurieren der letztlichen Konsistenz für alle Lesevorgänge besteht darin, [diese auf Kontoebene festzulegen](consistency-levels.md#configure-the-default-consistency-level). Wenn Sie die letztliche Konsistenz jedoch nur für einige Ihrer Lesevorgänge festlegen möchten, können Sie die Konsistenz auch auf [Anforderungsebene](how-to-manage-consistency.md#override-the-default-consistency-level) konfigurieren.

## <a name="integrated-cache-retention-time"></a>Aufbewahrungszeit des integrierten Caches

Die Cacheaufbewahrungszeit stellt die maximale Aufbewahrungsdauer für zwischengespeicherte Daten dar. In der Vorschauversion ist `MaxIntegratedCacheStaleness` immer auf 5 Minuten festgelegt. Dies kann nicht geändert werden.

## <a name="metrics"></a>Metriken

Bei Verwendung des integrierten Caches ist es hilfreich, einige wichtige Metriken zu überwachen. Zu diesen Metriken des integrierten Caches gehören:

- `DedicatedGatewayCpuUsage`: Hierbei handelt es sich um die CPU-Auslastung durch jeden dedizierten Gatewayknoten.
- `DedicatedGatewayMemoryUsage`: Hierbei handelt es sich um die Arbeitsspeicherauslastung durch jeden dedizierten Gatewayknoten für die Weiterleitung und Zwischenspeicherung von Anforderungen.
- `DedicatedGatewayRequests`: Hierbei handelt es sich um die Anzahl der Anforderungen, die über jeden dedizierten Gatewayknoten weitergeleitet werden.
- `IntegratedCacheEvictedEntriesSize`: Hierbei handelt es sich um die Menge der Daten, die aus dem integrierten Cache entfernt werden.
- `IntegratedCacheTTLExpirationCount`: Hierbei handelt es sich um die Anzahl der Einträge, die spezifisch aus dem integrierten Cache entfernt werden, weil die zwischengespeicherten Daten die `MaxIntegratedCacheStaleness`-Zeit überschreiten.
- `IntegratedCacheHitRate`: Hierbei handelt es sich um das Verhältnis der Punktlesevorgänge und Abfragen, die den integrierten Cache verwendet haben (aus allen dedizierten Gatewayanforderungen, die versucht haben, den integrierten Cache zu verwenden).

Alle vorhandenen Metriken sind standardmäßig über das Blatt **Metriken** verfügbar (nicht über das Blatt „Metriken (klassisch)“):

   :::image type="content" source="./media/integrated-cache/integrated-cache-metrics.png" alt-text="Screenshot: Metriken des integrierten Caches" border="false":::

Alle Metriken werden als Mittelwert für alle dedizierten Gatewayknoten zur Verfügung gestellt. Wenn Sie beispielsweise einen dedizierten Gatewaycluster mit fünf Knoten bereitstellen, spiegeln die Metriken den durchschnittlichen Wert aller fünf Knoten wider.

## <a name="troubleshooting-common-issues"></a>Behandlung häufig auftretender Probleme

In den folgenden Beispielen wird das Debuggen einiger gängiger Szenarios veranschaulicht:

### <a name="i-cant-tell-if-my-application-is-using-the-dedicated-gateway"></a>Wie erkenne ich, ob meine Anwendung das dedizierte Gateway verwendet?

Überprüfen Sie die Metrik `DedicatedGatewayRequests`. Diese Metrik umfasst alle Anforderungen, die das dedizierte Gateway nutzen, unabhängig davon, ob sie den integrierten Cache betreffen. Wenn Ihre Anwendung das Standardgateway oder den direkten Modus mit Ihrer ursprünglichen Verbindungszeichenfolge verwendet, wird Ihnen keine Fehlermeldung angezeigt, aber `DedicatedGatewayRequests` entspricht 0 (null).

### <a name="i-cant-tell-if-my-requests-are-hitting-the-integrated-cache"></a>Wie erkenne ich, ob meine Anforderungen den integrierten Cache betreffen?

Überprüfen Sie die Metrik `IntegratedCacheHitRate`. Wenn dieser Wert 0 (null) ist, betreffen die Anforderungen nicht den integrierten Cache. Überprüfen Sie, ob Sie die Verbindungszeichenfolge des dedizierten Gateways verwenden, eine Verbindung im Gatewaymodus herstellen und die letztliche Konsistenz festgelegt haben.

### <a name="i-want-to-understand-if-my-dedicated-gateway-is-too-small"></a>Woran erkenne ich, ob mein dediziertes Gateway zu klein ist?

Überprüfen Sie die Metrik `IntegratedCacheHitRate`. Wenn dieser Wert hoch ist (z. B. über 0,5 – 0,6), ist das ein gutes Anzeichen dafür, dass die Größe des dedizierten Gateways genügt.

Wenn der `IntegratedCacheHitRate`-Wert niedrig ist, überprüfen Sie den Wert `IntegratedCacheEvictedEntriesSize`. Wenn der `IntegratedCacheEvictedEntriesSize`-Wert hoch ist, bedeutet dies, dass ein größeres dediziertes Gateway von Vorteil wäre.

### <a name="i-want-to-understand-if-my-dedicated-gateway-is-too-large"></a>Woran erkenne ich, ob mein dediziertes Gateway zu groß ist?

Das ist etwas schwieriger. Im Allgemeinen sollten Sie klein anfangen und die Größe des dedizierten Gateways langsam erhöhen, bis der `IntegratedCacheHitRate`-Wert sich nicht mehr verbessert.

Wenn die meisten Daten aufgrund der Überschreitung des `MaxIntegratedCacheStaleness`-Werts aus dem Cache entfernt werden, anstatt aufgrund der Entfernungsrichtlinie nach seltener Verwendung, ist Ihr Cache möglicherweise größer als notwendig. Überprüfen Sie, ob der `IntegratedCacheTTLExpirationCount`-Wert fast so groß ist wie der `IntegratedCacheEvictedEntriesSize`-Wert. Wenn dies der Fall ist, können Sie eine kleinere Größe für das dedizierte Gateway testen und die Leistung vergleichen.

Überprüfen Sie den `DedicatedGatewayMemoryUsage`-Wert, und vergleichen Sie ihn mit der Größe des dedizierten Gateways. Wenn der `DedicatedGatewayMemoryUsage`-Wert kleiner als die Größe des dedizierten Gateways ist, sollten Sie ein kleineres dediziertes Gateway testen.

## <a name="next-steps"></a>Nächste Schritte

- [Häufig gestellte Fragen zum integrierten Cache](integrated-cache-faq.md)
- [Konfigurieren des integrierten Caches](how-to-configure-integrated-cache.md)
- [Dediziertes Gateway](dedicated-gateway.md)