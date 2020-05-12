---
title: Häufig gestellte Fragen zu dem im Autoskalierungsmodus bereitgestellten Durchsatz von Azure Cosmos DB
description: Häufig gestellte Fragen zum im Autoskalierungsmodus bereitgestellten Durchsatz für Azure Cosmos DB-Datenbanken und -Container
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: fa4c2708f34a377a17914c7e6e5abdd709cbb5b1
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791781"
---
# <a name="frequently-asked-questions-about-autoscale-provisioned-throughput-in-azure-cosmos-db"></a>Häufig gestellte Fragen zum im Autoskalierungsmodus bereitgestellten Durchsatz von Azure Cosmos DB

Bei per Autoskalierung bereitgestelltem Durchsatz verwaltet und skaliert Azure Cosmos DB die RU/s Ihres Containers oder Ihrer Datenbank automatisch basierend auf der Nutzung. Dieser Artikel bietet Antworten auf häufig gestellte Fragen zur Autoskalierung.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="is-autoscale-supported-for-all-apis"></a>Wird die Autoskalierung für alle APIs unterstützt?
Ja, die Autoskalierung wird für alle APIs unterstützt: Core (SQL), Gremlin, Table, Cassandra und API für MongoDB.

### <a name="is-autoscale-supported-for-multi-master-accounts"></a>Wird die Autoskalierung für Multimasterkonten unterstützt?
Ja, die Autoskalierung wird für Multimasterkonten unterstützt. Die maximalen RU/s sind in jeder Region verfügbar, die dem Cosmos-Konto hinzugefügt wird. 

### <a name="what-is-the-pricing-for-autoscale-"></a>Wie sieht das Preismodell für die Autoskalierung aus?
Ausführliche Informationen dazu finden Sie auf der Seite „Azure Cosmos DB – [Preise](https://azure.microsoft.com/pricing/details/cosmos-db/)“. 

### <a name="how-do-i-enable-autoscale-for-my-containers-or-databases"></a>Wie aktiviere ich den Autoskalierungsmodus für meine Container oder Datenbanken?
Die Autoskalierung kann für neue Container und Datenbanken aktiviert werden, die über das Azure-Portal erstellt wurden.

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autoscale-provisioned-throughput"></a>Gibt es CLI- oder SDK-Unterstützung zum Erstellen von Containern oder Datenbanken im per Autoskalierung bereitgestellten Durchsatz?
Derzeit können Sie nur Ressourcen mit per Autoskalierung bereitgestelltem Durchsatz über das Azure-Portal erstellen. Unterstützung für CLI und SDK ist noch nicht verfügbar.

### <a name="can-i-enable-autoscale-on-an-existing-container-or-a-database"></a>Kann ich den Autoskalierungsmodus für einen vorhandenen Container oder eine Datenbank aktivieren?
Derzeit können Sie den Autoskalierungsmodus für neue Container und Datenbanken bei deren Erstellung aktivieren. Eine Aktivierung der Autoskalierung für vorhandene Container und Datenbanken wird noch nicht unterstützt. Sie können vorhandene Container mit [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) oder [Änderungsfeed](change-feed.md) in einen neuen Container migrieren. 

### <a name="can-i-turn-off-autoscale-on-a-container-or-database"></a>Kann ich die Autoskalierung für einen Container oder eine Datenbank deaktivieren?
Ja, Sie können die Autoskalierung deaktivieren, indem Sie für den bereitgestellten Durchsatz zur Option „Standard (manuell)“ wechseln. Im aktuellen Release können Sie nach dem Wechsel von der Autoskalierung zum standardmäßig bereitgestellten Durchsatz die Autoskalierung für dieselbe Ressource nicht erneut aktivieren. 

### <a name="is-autoscale-supported-for-shared-throughput-databases"></a>Wird die Autoskalierung für Datenbanken mit gemeinsam genutztem Durchsatz unterstützt?
Ja, die Autoskalierung wird für Datenbanken mit gemeinsam genutztem Durchsatz unterstützt. Wählen Sie zum Aktivieren dieses Features bei der Erstellung der Datenbank die Autoskalierung und die Option **Durchsatz bereitstellen** aus. 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autoscale-is-enabled"></a>Wie viele Sammlungen sind pro Datenbank mit gemeinsam genutztem Durchsatz zulässig, wenn der Autoskalierungsmodus aktiviert ist?
Bei Datenbanken mit gemeinsam genutztem Durchsatz und aktivierter Autoskalierung lautet die Anzahl der zulässigen Sammlungen: MIN(25, Max. RU/s der Datenbank / 1000). Wenn der maximale Durchsatz der Datenbank z. B. 20.000 RU/s beträgt, kann die Datenbank MIN(25, 20.000 RU/s / 1000) = 20 Sammlungen aufweisen. 


### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>Welches Speicherlimit gilt für die einzelnen „Max. RU/s“-Optionen?  
Das Speicherlimit in GB für die einzelnen maximalen RU/s lautet: Max. RU/s der Datenbank oder des Containers / 100. Wenn z. B. der Wert für maximale RU/s 20.000 RU/s beträgt, kann die Ressource 200 GB Speicherplatz unterstützen. Informationen zu den verfügbaren maximalen RU/s und Speicheroptionen finden Sie im Artikel [Beschränkungen für den Autoskalierungsmodus](provision-throughput-autoscale.md#autoscale-limits). 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>Was passiert, wenn ich das mit meinem maximalen Durchsatz verbundene Speicherlimit überschreite?
Wenn das mit dem maximalen Durchsatz der Datenbank oder des Containers verbundene Speicherlimit überschritten wird, erhöht Azure Cosmos DB automatisch den maximalen Durchsatz auf die nächsthöhere Ebene, die dieses Speicherniveau unterstützen kann. Nehmen Sie z. B. an, eine Datenbank oder ein Container ist mit der Option 4000 RU/s für den maximalen Durchsatz bereitgestellt, die ein Speicherlimit von 50 GB aufweist. Erhöht sich der Speicherplatz der Ressource auf 100 GB, werden die maximalen RU/s der Datenbank oder des Containers automatisch auf 20.000 RU/s erhöht, die bis zu 200 GB unterstützen können. 

### <a name="how-quickly-will-autoscale-up-and-down-based-on-spikes-in-traffic"></a>Wie schnell skaliert der Autoskalierungsmodus aufgrund von Spitzenwerten im Datenverkehr hoch oder herunter?
Bei der Autoskalierung können Sie die RU/s innerhalb des minimalen und maximalen Bereichs der RU/s basierend auf dem eingehenden Datenverkehr unmittelbar hoch- oder herunterskalieren. Die Abrechnung erfolgt mit einer Granularität von einer Stunde, wobei die höchsten RU/s in einer bestimmten Stunde berechnet werden.

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autoscale"></a>Kann ich einen benutzerdefinierten Wert für den maximalen Durchsatz (RU/s) für die Autoskalierung festlegen?
Derzeit können Sie zwischen [vier Optionen](provision-throughput-autoscale.md#autoscale-limits) für den maximalen Durchsatz (RU/s) auswählen.

### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>Kann ich die maximale Anzahl der RU/s (Wechsel auf eine höhere Ebene) in der Datenbank oder im Container erhöhen? 
Ja. Über die Option **Skalierung und Einstellungen** für Ihren Container oder **Skalierung** für Ihre Datenbank können Sie einen höheren Wert für die maximalen RU/s für die Autoskalierung auswählen. Dies ist ein asynchroner Vorgang für das zentrale Hochskalieren, der einige Zeit in Anspruch nehmen kann (in der Regel 4-6 Stunden, je nach den gewählten RU/s), da der Dienst mehr Ressourcen zur Unterstützung der höheren Skalierung bereitstellt. 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>Kann ich die maximale Anzahl der RU/s (Wechsel auf eine niedrigere Ebene) in der Datenbank oder im Container verringern?
Ja. Solange der aktuelle Speicherplatz der Datenbank oder des Containers unter dem [Speicherlimit](#what-is-the-storage-limit-associated-with-each-max-rus-option) liegt, das mit der maximalen RU/s-Ebene verbunden ist, auf die Sie zentral herunterskalieren möchten, können Sie die maximalen RU/s auf diese Ebene reduzieren. Wenn Sie z. B. 20.000 RU/s als maximale RU/s ausgewählt haben, können Sie die maximalen RU/s auf 4000 RU/s zentral herunterskalieren, wenn Sie weniger als 50 GB Speicherplatz haben (das mit 4000 RU/s verbundene Speicherlimit).

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>Welche Zuordnung besteht zwischen den maximalen RU/s und den physischen Partitionen?
Wenn Sie zum ersten Mal die maximale Anzahl der RU/s auswählen, stellt Azure Cosmos DB Folgendes bereit: Max RU/s / 10.000 RU/s = Anzahl der physischen Partitionen. Jede [physische Partition](partition-data.md#physical-partitions) kann bis zu 10.000 RU/s und 50 GB Speicherplatz unterstützen. Wenn die Speichergröße zunimmt, teilt Azure Cosmos DB die Partitionen automatisch auf, um weitere physische Partitionen hinzuzufügen, um den Speicherzuwachs zu bewältigen, oder erhöht die maximale RU/s-Ebene, wenn der Speicher [die zugehörigen Grenzwerte](#what-is-the-storage-limit-associated-with-each-max-rus-option) überschreitet. 

Die maximalen RU/s der Datenbank oder des Containers werden gleichmäßig auf alle physischen Partitionen verteilt. Daher gilt für den Gesamtdurchsatz, auf den jede einzelne physische Partition skaliert werden kann, Folgendes: Max. RU/s der Datenbank oder des Containers / Anzahl der physischen Partitionen. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>Was passiert, wenn eingehende Anforderungen die maximalen RU/s der Datenbank oder des Containers überschreiten?
Wenn die insgesamt verbrauchten RU/s die maximalen RU/s des Containers oder der Datenbank überschreiten, werden Anforderungen, die die maximalen RU/s überschreiten, gedrosselt und der Statuscode 429 zurückgegeben. Anforderungen, die zu einer über 100%igen normalisierten Auslastung führen, werden ebenfalls gedrosselt. Die normalisierte Auslastung ist definiert als die maximale Auslastung der RU/s über alle physischen Partitionen. Nehmen Sie z. B. an, Ihr maximaler Durchsatz beträgt 20.000 RU/s und Sie haben zwei physische Partitionen, P_1 und P_2, die jeweils auf 10.000 RU/s skaliert werden können. In einer bestimmten Sekunde, wenn P_1 6000 RUs und P_2 8000 RUs verwendet hat, ist die normalisierte Auslastung MAX(6000 RU / 10.000 RU, 8000 RU / 10.000 RU) = 0,8.

> [!NOTE]
> Die SDKs des Azure Cosmos DB-Clients und die Tools für den Datenimport (Azure Data Factory, BulkExecutor-Bibliothek) führen beim Code 429 automatisch einen erneuten Versuch durch, sodass gelegentliche 429er Codes akzeptabel sind. Eine anhaltend hohe Anzahl von 429 Codes kann darauf hindeuten, dass Sie die maximalen RU/s erhöhen oder Ihre Partitionierungsstrategie für eine [Hot-Partition](#autoscale-rate-limiting) überprüfen müssen.

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autoscale-is-enabled"></a><a id="autoscale-rate-limiting"></a> Ist es weiterhin möglich, dass der Code 429 (Drosselung/Ratenbegrenzung) angezeigt wird, wenn der Autoskalierungsmodus aktiviert ist? 
Ja. Der Code 429 kann in zwei Szenarien angezeigt werden. Erstens, wenn die insgesamt verbrauchten RU/s die maximalen RU/s des Containers oder der Datenbank überschreiten, wird der Dienst die Anforderungen entsprechend drosseln. 

Zweitens, wenn es eine Hot-Partition gibt, d. h. einen logischen Partitionsschlüsselwert, der im Vergleich zu anderen Partitionsschlüsselwerten eine unverhältnismäßig höhere Anzahl von Anforderungen aufweist, ist es möglich, dass die zugrunde liegende physische Partition ihr RU/s-Budget überschreitet. Um Hot-Partitionen zu vermeiden, ist es am besten, [einen geeigneten Partitionsschlüssel](partitioning-overview.md#choose-partitionkey) auszuwählen, der zu einer gleichmäßigen Verteilung von Speicher und Durchsatz führt. 

Wenn Sie z. B. die Option für maximalen Durchsatz von 20.000 RU/s auswählen und über 200 GB Speicherplatz mit vier physischen Partitionen verfügen, kann jede physische Partition automatisch auf bis zu 5000 RU/s hochskaliert werden. Wenn sich auf einem bestimmten logischen Partitionsschlüssel eine Hot-Partition befand, wurde der Code 429 angezeigt, wenn die zugrunde liegende physische Partition, in der sie sich befindet, 5000 RU/s überschreitet, d. h. eine normalisierte Auslastung von 100 %.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie die [Autoskalierung für einen Azure Cosmos-Container oder eine -Datenbank ](provision-throughput-autoscale.md#create-db-container-autoscale) aktivieren können.
* Erfahren Sie mehr über die [Vorteile des bereitgestellten Durchsatzes mit Autoskalierung](provision-throughput-autoscale.md#autoscale-benefits).
* Erfahren Sie mehr über [logische und physische Partitionen](partition-data.md).
