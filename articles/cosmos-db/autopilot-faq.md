---
title: Häufig gestellte Fragen zum Durchsatz im Autopilot-Modus von Azure Cosmos DB
description: Häufig gestellte Fragen zum Autopilot-Modus für Azure Cosmos DB-Datenbanken und -Container
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d2ad585cabefb9ea78b88e748e422c745c11d03f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476008"
---
# <a name="frequently-asked-questions-about-provisioned-throughput-in-azure-cosmos-db-autopilot-mode-preview"></a>Häufig gestellte Fragen zum bereitgestellten Durchsatz im Autopilot-Modus von Azure Cosmos DB (Vorschau)
Im Autopilot-Modus verwaltet und skaliert Azure Cosmos DB automatisch die RU/s Ihres Containers oder Ihrer Datenbank basierend auf der Nutzung. Dieser Artikel bietet Antworten auf häufig gestellte Fragen zum Autopilot-Modus. 

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="is-autopilot-mode-supported-for-all-apis"></a>Wird der Autopilot-Modus für alle APIs unterstützt?
Ja, der Autopilot-Modus wird für alle APIs unterstützt: Core (SQL), Gremlin, Table, Cassandra und API für MongoDB.

### <a name="is-autopilot-mode-supported-for-multi-master-accounts"></a>Wird der Autopilot-Modus für Multimasterkonten unterstützt?
Ja, der Autopilot-Modus wird für Multimasterkonten unterstützt. Die maximalen RU/s sind in jeder Region verfügbar, die dem Cosmos-Konto hinzugefügt wird. 

### <a name="what-is-the-pricing-for-autopilot"></a>Wie sieht das Preismodell für den Autopilot-Modus aus?
Ausführliche Informationen dazu finden Sie auf der Seite „Azure Cosmos DB – [Preise](https://azure.microsoft.com/pricing/details/cosmos-db/)“. 

### <a name="how-do-i-enable-autopilot-for-my-containers-or-databases"></a>Wie aktiviere ich den Autopilot-Modus für meine Container oder Datenbanken?
Der Autopilot-Modus kann für neue Container und Datenbanken aktiviert werden, die über das Azure-Portal erstellt wurden. 

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autopilot-mode"></a>Gibt es CLI- oder SDK-Unterstützung zum Erstellen von Containern oder Datenbanken im Autopilot-Modus?
Derzeit können Sie in der Vorschauversion nur Ressourcen mit Autopilot-Modus aus dem Azure-Portal erstellen. Unterstützung für CLI und SDK ist noch nicht verfügbar.

### <a name="can-i-enable-autopilot-on-an-existing-container-or-a-database"></a>Kann ich den Autopilot-Modus für einen vorhandenen Container oder eine Datenbank aktivieren?
Derzeit können Sie den Autopilot-Modus für neue Container und Datenbanken bei deren Erstellung aktivieren. Die Unterstützung zur Aktivierung des Autopilot-Modus für vorhandene Container und Datenbanken ist noch nicht verfügbar. Sie können vorhandene Container mit [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) oder [Änderungsfeed](change-feed.md) in einen neuen Container migrieren. 

### <a name="can-i-turn-off-autopilot-mode-on-a-container-or-database"></a>Kann ich den Autopilot-Modus für einen Container oder eine Datenbank deaktivieren?
Ja, Sie können den Autopilot-Modus deaktivieren, indem Sie für den bereitgestellten Durchsatz zur Option „Manuell“ wechseln. In der Vorschauversion können Sie nach dem Wechsel vom Autopilot-Modus in den manuellen Modus den Autopilot-Modus für dieselbe Ressource nicht erneut aktivieren. 

### <a name="is-autopilot-mode-supported-for-shared-throughput-databases"></a>Wird der Autopilot-Modus für Datenbanken mit gemeinsam genutztem Durchsatz unterstützt?
Ja, der Autopilot-Modus wird für Datenbanken mit gemeinsam genutztem Durchsatz unterstützt. Wählen Sie zum Aktivieren dieses Features bei der Erstellung der Datenbank den Autopilot-Modus und die Option **Durchsatz bereitstellen** aus. 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autopilot-is-enabled"></a>Wie viele Sammlungen sind pro Datenbank mit gemeinsam genutztem Durchsatz zulässig, wenn der Autopilot-Modus aktiviert ist?
Bei Datenbanken mit gemeinsam genutztem Durchsatz und aktiviertem Autopilot-Modus ist die Anzahl der zulässigen Sammlungen: MIN(25, Max. RU/s der Datenbank / 1000). Wenn der maximale Durchsatz der Datenbank z. B. 20.000 RU/s beträgt, kann die Datenbank MIN(25, 20.000 RU/s / 1000) = 20 Sammlungen aufweisen. 


### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>Welches Speicherlimit gilt für die einzelnen „Max. RU/s“-Optionen?  
Das Speicherlimit in GB für die einzelnen maximalen RU/s lautet: Max. RU/s der Datenbank oder des Containers / 100. Wenn z. B. der Wert für maximale RU/s 20.000 RU/s beträgt, kann die Ressource 200 GB Speicherplatz unterstützen. Informationen zu den verfügbaren maximalen RU/s und Speicheroptionen finden Sie im Artikel [Autopilot-Grenzwerte](provision-throughput-autopilot.md#autopilot-limits). 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>Was passiert, wenn ich das mit meinem maximalen Durchsatz verbundene Speicherlimit überschreite?
Wenn das mit dem maximalen Durchsatz der Datenbank oder des Containers verbundene Speicherlimit überschritten wird, erhöht Azure Cosmos DB automatisch den maximalen Durchsatz auf die nächsthöhere Ebene, die dieses Speicherniveau unterstützen kann. Nehmen Sie z. B. an, eine Datenbank oder ein Container ist mit der Option 4000 RU/s für den maximalen Durchsatz bereitgestellt, die ein Speicherlimit von 50 GB aufweist. Erhöht sich der Speicherplatz der Ressource auf 100 GB, werden die maximalen RU/s der Datenbank oder des Containers automatisch auf 20.000 RU/s erhöht, die bis zu 200 GB unterstützen können. 

### <a name="how-quickly-will-autopilot-scale-up-and-down-based-on-spikes-in-traffic"></a>Wie schnell skaliert der Autopilot-Modus aufgrund von Spitzenwerten im Datenverkehr zentral hoch oder herunter?
Der Autopilot-Modus skaliert die RU/s innerhalb des minimalen und maximalen Bereichs der RU/s, basierend auf dem eingehenden Datenverkehr, unmittelbar zentral hoch oder herunter. Die Abrechnung erfolgt mit einer Granularität von einer Stunde, wobei die höchsten RU/s in einer bestimmten Stunde berechnet werden. 

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autopilot-mode"></a>Kann ich einen benutzerdefinierten Wert für den maximalen Durchsatz (RU/s) für den Autopilot-Modus festlegen?
Derzeit können Sie in der Vorschauversion zwischen [vier Optionen](provision-throughput-autopilot.md#autopilot-limits) für den maximalen Durchsatz (RU/s) wählen.

### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>Kann ich die maximale Anzahl der RU/s (Wechsel auf eine höhere Ebene) in der Datenbank oder im Container erhöhen? 
Ja. Über die Option **Skalierung und Einstellungen** für Ihren Container oder **Skalierung** für Ihre Datenbank können Sie einen höheren Wert für die maximalen RU/s für den Autopilot-Modus auswählen. Dies ist ein asynchroner Vorgang für das zentrale Hochskalieren, der einige Zeit in Anspruch nehmen kann (in der Regel 4-6 Stunden, je nach den gewählten RU/s), da der Dienst mehr Ressourcen zur Unterstützung der höheren Skalierung bereitstellt. 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>Kann ich die maximale Anzahl der RU/s (Wechsel auf eine niedrigere Ebene) in der Datenbank oder im Container verringern?
Ja. Solange der aktuelle Speicherplatz der Datenbank oder des Containers unter dem [Speicherlimit](#what-is-the-storage-limit-associated-with-each-max-rus-option) liegt, das mit der maximalen RU/s-Ebene verbunden ist, auf die Sie zentral herunterskalieren möchten, können Sie die maximalen RU/s auf diese Ebene reduzieren. Wenn Sie z. B. 20.000 RU/s als maximale RU/s ausgewählt haben, können Sie die maximalen RU/s auf 4000 RU/s zentral herunterskalieren, wenn Sie weniger als 50 GB Speicherplatz haben (das mit 4000 RU/s verbundene Speicherlimit).

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>Welche Zuordnung besteht zwischen den maximalen RU/s und den physischen Partitionen?
Wenn Sie zum ersten Mal die maximale Anzahl der RU/s auswählen, stellt Azure Cosmos DB Folgendes bereit: Max RU/s / 10.000 RU/s = Anzahl der physischen Partitionen. Jede [physische Partition](partition-data.md#physical-partitions) kann bis zu 10.000 RU/s und 50 GB Speicherplatz unterstützen. Wenn die Speichergröße zunimmt, teilt Azure Cosmos DB die Partitionen automatisch auf, um weitere physische Partitionen hinzuzufügen, um den Speicherzuwachs zu bewältigen, oder erhöht die maximale RU/s-Ebene, wenn der Speicher [die zugehörigen Grenzwerte](#what-is-the-storage-limit-associated-with-each-max-rus-option) überschreitet. 

Die maximalen RU/s der Datenbank oder des Containers werden gleichmäßig auf alle physischen Partitionen verteilt. Daher gilt für den Gesamtdurchsatz, auf den jede einzelne physische Partition skaliert werden kann, Folgendes: Max. RU/s der Datenbank oder des Containers / Anzahl der physischen Partitionen. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>Was passiert, wenn eingehende Anforderungen die maximalen RU/s der Datenbank oder des Containers überschreiten?
Wenn die insgesamt verbrauchten RU/s die maximalen RU/s des Containers oder der Datenbank überschreiten, werden Anforderungen, die die maximalen RU/s überschreiten, gedrosselt und der Statuscode 429 zurückgegeben. Anforderungen, die zu einer über 100%igen normalisierten Auslastung führen, werden ebenfalls gedrosselt. Die normalisierte Auslastung ist definiert als die maximale Auslastung der RU/s über alle physischen Partitionen. Nehmen Sie z. B. an, Ihr maximaler Durchsatz beträgt 20.000 RU/s und Sie haben zwei physische Partitionen, P_1 und P_2, die jeweils auf 10.000 RU/s skaliert werden können. In einer bestimmten Sekunde, wenn P_1 6000 RUs und P_2 8000 RUs verwendet hat, ist die normalisierte Auslastung MAX(6000 RU / 10.000 RU, 8000 RU / 10.000 RU) = 0,8.

> [!NOTE]
> Die SDKs des Azure Cosmos DB-Clients und die Tools für den Datenimport (Azure Data Factory, BulkExecutor-Bibliothek) führen beim Code 429 automatisch einen erneuten Versuch durch, sodass gelegentliche 429er Codes akzeptabel sind. Eine anhaltend hohe Anzahl von 429 Codes kann darauf hindeuten, dass Sie die maximalen RU/s erhöhen oder Ihre Partitionierungsstrategie für eine [Hot-Partition](#is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled) überprüfen müssen.

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled"></a>Ist es weiterhin möglich, dass der Code 429 (Drosselung/Ratenbegrenzung) angezeigt wird, wenn der Autopilot-Modus aktiviert ist? 
Ja. Der Code 429 kann in zwei Szenarien angezeigt werden. Erstens, wenn die insgesamt verbrauchten RU/s die maximalen RU/s des Containers oder der Datenbank überschreiten, wird der Dienst die Anforderungen entsprechend drosseln. 

Zweitens, wenn es eine Hot-Partition gibt, d. h. einen logischen Partitionsschlüsselwert, der im Vergleich zu anderen Partitionsschlüsselwerten eine unverhältnismäßig höhere Anzahl von Anforderungen aufweist, ist es möglich, dass die zugrunde liegende physische Partition ihr RU/s-Budget überschreitet. Um Hot-Partitionen zu vermeiden, ist es am besten, [einen geeigneten Partitionsschlüssel](partitioning-overview.md#choose-partitionkey) auszuwählen, der zu einer gleichmäßigen Verteilung von Speicher und Durchsatz führt. 

Wenn Sie z. B. die Option für maximalen Durchsatz von 20.000 RU/s auswählen und über 200 GB Speicherplatz mit vier physischen Partitionen verfügen, kann jede physische Partition automatisch auf bis zu 5000 RU/s hochskaliert werden. Wenn sich auf einem bestimmten logischen Partitionsschlüssel eine Hot-Partition befand, wurde der Code 429 angezeigt, wenn die zugrunde liegende physische Partition, in der sie sich befindet, 5000 RU/s überschreitet, d. h. eine normalisierte Auslastung von 100 %.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie den [Autopilot-Modus für einen Azure Cosmos-Container oder eine -Datenbank ](provision-throughput-autopilot.md#create-a-database-or-a-container-with-autopilot-mode) aktivieren können.
* Erfahren Sie mehr über die [Vorteile des Autopilot-Modus](provision-throughput-autopilot.md#benefits-of-autopilot-mode).
* Erfahren Sie mehr über [logische und physische Partitionen](partition-data.md).
