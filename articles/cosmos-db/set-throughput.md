---
title: Bereitstellen des Durchsatzes für Azure Cosmos-Container und -Datenbanken
description: Hier erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos-Container und -Datenbanken festlegen.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 81a31448a588849a410b37868cf579fbb0a9ceb6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777793"
---
# <a name="introduction-to-provisioned-throughput-in-azure-cosmos-db"></a>Einführung zum bereitgestellten Durchsatz in Azure Cosmos DB

Mit Azure Cosmos DB können Sie bereitgestellten Durchsatz für Ihre Datenbanken und Container festlegen. Es gibt zwei Arten von bereitgestelltem Durchsatz: Standard (manuell) oder automatische Skalierung. In diesem Artikel erhalten Sie einen Überblick über die Funktionsweise des bereitgestellten Durchsatzes. 

Eine Azure Cosmos-Datenbank ist eine Verwaltungseinheit für eine Gruppe von Containern. Eine Datenbank besteht aus einem Satz von schemaunabhängigen Containern. Ein Azure Cosmos-Container ist die Skalierungseinheit für Durchsatz und Speicher. Ein Container wird horizontal über eine Gruppe von Computern innerhalb einer Azure-Region partitioniert und auf alle Azure-Regionen, die Ihrem Azure Cosmos-Konto zugeordnet sind, verteilt.

Mit Azure Cosmos DB können Sie Durchsatz in zwei Größen bereitstellen:
 
- Azure Cosmos-Container
- Azure Cosmos-Datenbanken

## <a name="set-throughput-on-a-container"></a>Festlegen des Durchsatzes für einen Container  

Der für einen Azure Cosmos-Container bereitgestellte Durchsatz ist ausschließlich für diesen Container reserviert. Der Container erhält den bereitgestellten Durchsatz durchgängig. Der bereitgestellte Durchsatz für einen Container wird finanziell durch SLAs gesichert. Informationen zum Konfigurieren des standardmäßigen (manuellen) Durchsatzes für einen Container finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md). Informationen zum Konfigurieren des automatisch skalierten Durchsatzes für einen Container finden Sie unter [Bereitstellen von automatisch skaliertem Durchsatz](how-to-provision-autoscale-throughput.md).

Das Festlegen von bereitgestelltem Durchsatz für einen Container ist die am häufigsten genutzte Option. Sie können den Durchsatz für einen Container elastisch skalieren, indem Sie mithilfe von [Anforderungseinheiten (Request Units, RUs)](request-units.md) eine beliebige Menge an Durchsatz bereitstellen. 

Der für einen Container vorgesehene Durchsatz wird gleichmäßig auf die physischen Partitionen verteilt, und unter der Annahme eines guten Partitionsschlüssels, der die logischen Partitionen gleichmäßig auf die physischen Partitionen verteilt, wird der Durchsatz auch gleichmäßig auf alle logischen Partitionen des Containers verteilt. Sie können den Durchsatz für logische Partitionen nicht selektiv angeben. Da mindestens eine logische Partition eines Containers auf einer physischen Partition gehostet wird, gehören die physischen Partitionen ausschließlich zu dem Container und unterstützen den Durchsatz für den Container. 

Wenn die Workload in einer logischen Partition mehr als den Durchsatz verbraucht, der der zugrunde liegenden physischen Partition zugewiesen wurde, werden Ihre Vorgänge möglicherweise ratenbegrenzt. Eine sogenannte _heiße Partition_ tritt auf, wenn eine logische Partition unverhältnismäßig mehr Anforderungen als andere Partitionsschlüsselwerte aufweist.

Bei einer Ratenbegrenzung können Sie entweder den bereitgestellten Durchsatz für den gesamten Container erhöhen oder die Vorgänge wiederholen. Sie sollten auch sicherstellen, dass Sie einen Partitionsschlüssel auswählen, mit dem das Speicher- und Anforderungsvolumen gleichmäßig verteilt wird. Weitere Informationen zum Partitionieren finden Sie unter [Partitionierung und horizontale Skalierung in Azure Cosmos DB](partition-data.md).

Es wird empfohlen, den Durchsatz auf Containerebene zu konfigurieren, wenn Sie eine garantierte Leistung für den Container benötigen.

Die folgende Abbildung zeigt, wie eine physische Partition eine bzw. mehrere logische Partitionen eines Containers hostet:

:::image type="content" source="./media/set-throughput/resource-partition.png" alt-text="Hosten einer oder mehrerer logischer Partitionen eines Containers über eine physische Partition" border="false":::

## <a name="set-throughput-on-a-database"></a>Festlegen des Durchsatzes für eine Datenbank

> [!NOTE]
> In Konten, in denen [kundenseitig verwaltete Schlüssel](how-to-setup-cmk.md) aktiviert sind, ist derzeit keine Bereitstellung von Durchsatz für eine Azure Cosmos-Datenbank möglich.

Wenn Sie Durchsatz für eine Cosmos-Datenbank bereitstellen, wird der Durchsatz von allen Containern in der Datenbank gemeinsam genutzt (auch als gemeinsam genutzte Datenbankcontainer bezeichnet). Ausnahme: Sie haben einen bereitgestellten Durchsatz für bestimmte Container in der Datenbank angegeben. Die gemeinsame Nutzung des auf Datenbankebene bereitgestellten Durchsatzes durch die Container entspricht dem Hosten einer Datenbank in einem Computercluster. Da alle Container in einer Datenbank die auf einem Computer verfügbaren Ressourcen gemeinsam nutzen, erhalten Sie natürlich keine vorhersagbare Leistung für einen bestimmten Container. Informationen zum Konfigurieren des bereitgestellten Durchsatzes für eine Datenbank finden Sie unter [Bereitstellen von Durchsatz für eine Azure Cosmos-Datenbank](how-to-provision-database-throughput.md). Informationen zum Konfigurieren des automatisch skalierten Durchsatzes für eine Datenbank finden Sie unter [Bereitstellen von automatisch skaliertem Durchsatz](how-to-provision-autoscale-throughput.md).

Mit dem Festlegen des Durchsatzes für eine Azure Cosmos-Datenbank wird sichergestellt, dass Sie zu jeder Zeit den bereitgestellten Durchsatz für diese Datenbank erhalten. Da alle Container in der Datenbank den bereitgestellten Durchsatz gemeinsam nutzen, garantiert Azure Cosmos DB keinen vorhersagbaren Durchsatz für einen bestimmten Container in der Datenbank. Der Anteil des Durchsatzes, den ein bestimmter Container erhält, hängt von den folgenden Faktoren ab:

* Anzahl von Containern
* Auswahl der Partitionsschlüssel für verschiedene Container
* Verteilung der Workload auf die verschiedenen logischen Partitionen der Container 

Es wird empfohlen, den Durchsatz für eine Datenbank zu konfigurieren, wenn der Durchsatz von mehreren Containern gemeinsam genutzt werden soll, Sie aber keinen spezifischen Durchsatz für bestimmte Container bereitstellen möchten. 

In den folgenden Beispielen wird erläutert, wo das Bereitstellen von Durchsatz auf Datenbankebene empfehlenswert ist:

* Die gemeinsame Nutzung des für eine Datenbank bereitgestellten Durchsatzes durch eine Gruppe von Containern ist bei mehrinstanzenfähigen Anwendungen nützlich. Jeder Benutzer kann durch einen eigenen Azure Cosmos-Container dargestellt werden.

* Die gemeinsame Nutzung des bereitgestellten Durchsatzes einer Datenbank durch eine Gruppe von Containern ist nützlich, wenn Sie eine NoSQL-Datenbank (z. B. MongoDB oder Cassandra), die in einem Cluster von virtuellen Computern oder auf lokalen physischen Servern gehostet wird, zu Azure Cosmos DB migrieren. Stellen Sie sich den bereitgestellten Durchsatz, der für Ihre Azure Cosmos-Datenbank konfiguriert wurde, als logische Entsprechung (jedoch kostengünstiger und flexibler) zur Computekapazität des MongoDB- oder Cassandra-Clusters vor.  

Alle in einer Datenbank erstellten Container mit bereitgestelltem Durchsatz müssen mit einem [Partitionsschlüssel](partition-data.md) erstellt werden. Der einem Container innerhalb einer Datenbank zugeteilte Durchsatz wird jederzeit auf alle logischen Partitionen des Containers verteilt. Wenn Ihre Container den für eine Datenbank konfigurierten bereitgestellten Durchsatz gemeinsam nutzen, können Sie den Durchsatz nicht selektiv einem bestimmten Container oder einer logischen Partition zuordnen. 

Wenn die Workload in einer logischen Partition mehr als den Durchsatz verbraucht, der der jeweiligen logischen Partition zugewiesen wurde, werden Ihre Vorgänge begrenzt. Bei einer Ratenbegrenzung können Sie entweder den Durchsatz für die gesamte Datenbank erhöhen oder die Vorgänge wiederholen. Weitere Informationen zur Partitionierung finden Sie unter [Logische Partitionen](partition-data.md).

Container in einer Datenbank mit gemeinsam genutztem Durchsatz teilen den Durchsatz (RU/s), der dieser Datenbank zugeordnet ist. Können Sie bis zu vier Container mit mindestens 400 RU/s in der Datenbank haben. Bei einem standardmäßigen (manuell) bereitgestellten Durchsatz erfordert jeder zu den ersten vier Containern neu hinzugefügte Container mindestens 100 zusätzliche RU/s. Wenn Sie z. B. eine Datenbank mit gemeinsam genutztem Durchsatz mit acht Containern haben, beträgt die Mindestanforderungen für die RU/s in der Datenbank 800 RU/s. Mit dem automatisch skalierten bereitgestellten Durchsatz können Sie in einer Datenbank über bis zu 25 Container mit maximal 4000 RU/s (Skalierung von 400 bis 4.000 RU/s) verfügen.

> [!NOTE]
> Im Februar 2020 haben wir eine Änderung eingeführt, die Ihnen maximal 25 Container in einer gemeinsam genutzten Durchsatzdatenbank ermöglicht, was containerübergreifend eine bessere gemeinsame Nutzung des Durchsatzes ermöglicht. Nach den ersten 25 Containern können Sie der Datenbank nur dann weitere Container hinzufügen, wenn diese [mit dediziertem Durchsatz](#set-throughput-on-a-database-and-a-container) bereitgestellt werden, der vom gemeinsam genutzten Durchsatz der Datenbank getrennt ist.<br>
Wenn Ihr Azure Cosmos DB-Konto bereits eine gemeinsam genutzte Durchsatzdatenbank mit mindestens 25 Containern enthält, sind das Konto und alle anderen Konten im selben Azure-Abonnement von dieser Änderung ausgenommen. Wenn Sie Feedback oder Fragen haben, [wenden Sie sich an den Produktsupport](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 

Wenn Ihre Workloads das Löschen und Wiederherstellen aller Sammlungen in einer Datenbank beinhalten, wird empfohlen, die leere Datenbank zu löschen und vor der Erstellung der Sammlung eine neue Datenbank anzulegen. Die folgende Abbildung zeigt, wie eine physische Partition eine bzw. mehrere logische Partitionen hosten kann, die zu unterschiedlichen Containern innerhalb einer Datenbank gehören:

:::image type="content" source="./media/set-throughput/resource-partition2.png" alt-text="Hosten einer oder mehrerer logischer Partitionen eines Containers über eine physische Partition" border="false":::

## <a name="set-throughput-on-a-database-and-a-container"></a>Festlegen des Durchsatzes für eine Datenbank und einen Container

Sie können die beiden Modelle kombinieren. Es ist erlaubt, Durchsatz sowohl auf Datenbank- als auch auf Containerebene bereitzustellen. Das folgende Beispiel zeigt die Vorgehensweise bei der standardmäßigen (manuellen) Bereitstellung von Durchsatz für eine Azure Cosmos-Datenbank und einen Container:

* Sie können eine Azure Cosmos-Datenbank namens *Z* mit standardmäßig (manuell) bereitgestelltem Durchsatz von *K* RUs erstellen. 
* Als Nächstes erstellen Sie die fünf Container *A*, *B*, *C*, *D* und *E* innerhalb der Datenbank. Achten Sie beim Erstellen von Container B darauf, die **Option zum Bereitstellen von dediziertem Durchsatz für diesen Container**zu aktivieren, und konfigurieren Sie explizit *P* RUs des bereitgestellten Durchsatzes für diesen Container. Beachten Sie, dass Sie freigegebenen und dedizierten Durchsatz nur beim Erstellen der Datenbank und des Containers konfigurieren können. 

   :::image type="content" source="./media/set-throughput/coll-level-throughput.png" alt-text="Hosten einer oder mehrerer logischer Partitionen eines Containers über eine physische Partition":::

* Der Durchsatz von *K* RUs ist für die vier Container *A*, *C*, *D*, und *E* freigegeben. Die genaue für *A*, *C*, *D* oder *E* verfügbare Durchsatzmenge variiert. Es gibt keine SLAs für die einzelnen Containerdurchsätze.
* Für den Container *B* wird jederzeit ein Durchsatz von *P* RUs sichergestellt. Er wird durch SLAs abgedeckt.

> [!NOTE]
> Ein Container mit bereitgestelltem Durchsatz kann nicht in einen gemeinsam genutzten Datendankcontainer konvertiert werden. Umgekehrt kann ein gemeinsam genutzter Datenbankcontainer nicht in einen dedizierten Durchsatz umgewandelt werden.

## <a name="update-throughput-on-a-database-or-a-container"></a>Aktualisieren des Durchsatzes für eine Datenbank oder einen Container

Nachdem Sie einen Azure Cosmos-Container oder eine Datenbank erstellt haben, können Sie den bereitgestellten Durchsatz aktualisieren. Es gibt keine Beschränkung für den bereitgestellten Maximaldurchsatz, den Sie in der Datenbank oder dem Container konfigurieren können. 

Um den [bereitgestellten Mindestdurchsatz](concepts-limits.md#storage-and-database-operations) einer Datenbank oder eines Containers zu schätzen, ermitteln Sie folgenden Maximalwert:

* 400 RU/s 
* Aktueller Speicher in GB * 10 RU/s
* Höchstwert bereitgestellter RU/s für die Datenbank oder den Container / 100
* Containeranzahl * 100 RU/s (nur Datenbank mit gemeinsam genutztem Durchsatz)

Der tatsächliche Mindestwert an RU/s kann je nach Kontokonfiguration variieren. Sie können [Azure Monitor-Metriken](monitor-cosmos-db.md#view-operation-level-metrics-for-azure-cosmos-db) verwenden, um den Verlauf des bereitgestellten Durchsatzes (RU/s) und des Speichers für eine Ressource anzuzeigen.

Sie können den Mindestdurchsatz eines Containers oder einer Datenbank programmgesteuert mithilfe der SDKs abrufen oder den Wert im Azure-Portal anzeigen. Bei Verwendung des .NET SDK ermöglicht Ihnen die [container.ReplaceThroughputAsync](/dotnet/api/microsoft.azure.cosmos.container.replacethroughputasync?view=azure-dotnet&preserve-view=true)-Methode das Skalieren des Werts für den bereitgestellten Durchsatz. Bei Verwendung des Java-SDK ermöglicht Ihnen die [CosmosContainer.replaceProvisionedThroughput](sql-api-java-sdk-samples.md)-Methode das Skalieren des Werts für den bereitgestellten Durchsatz.

Bei Verwendung des .NET SDK ermöglicht Ihnen die [Container.ReadThroughputAsync](/dotnet/api/microsoft.azure.cosmos.container.readthroughputasync?view=azure-dotnet&preserve-view=true)-Methode das Abrufen des Mindestdurchsatzes eines Containers oder einer Datenbank. 

Sie können den bereitgestellten Durchsatz eines Containers oder einer Datenbank jederzeit skalieren. Wenn ein Skalierungsvorgang ausgeführt wird, um den Durchsatz zu erhöhen, kann sich der Zeitaufwand aufgrund der Systemtasks für die Bereitstellung der erforderlichen Ressourcen erhöhen. Sie können den Status des Skalierungsvorgangs im Azure-Portal oder programmgesteuert mit den SDKs überprüfen. Bei Verwendung des .NET SDK können Sie den Status des Skalierungsvorgangs mithilfe der Methode `Container.ReadThroughputAsync` abrufen.

## <a name="comparison-of-models"></a>Vergleich der Modelle
Diese Tabelle zeigt einen Vergleich zwischen dem standardmäßig (manuell) bereitgestellten Durchsatz in einer Datenbank und in einem Container. 

|**Parameter**  |**Standarddurchsatz (manuell) für eine Datenbank**  |**Standarddurchsatz (manuell) für einen Container**|**Automatisch skalierter Durchsatz für eine Datenbank** | **Automatisch skalierter Durchsatz für einen Container**|
|---------|---------|---------|---------|---------|
|Einstiegspunkt (minimale RU/s) |400 RU/s. Nach den ersten vier Containern benötigt jeder weitere Container mindestens 100 RU/s.</li> |400| Automatische Skalierung zwischen 400 und 4.000 RU/s. Bis zu 25 Container ohne RU/s-Minimum pro Container</li> | Automatische Skalierung zwischen 400 und 4.000 RU/s.|
|RU/s-Minimum pro Container|100|400|--|Automatische Skalierung zwischen 400 und 4.000 RU/s|
|Maximale RUs|Unbegrenzt, in der Datenbank|Unbegrenzt, im Container|Unbegrenzt, in der Datenbank|Unbegrenzt, im Container
|RUs zugewiesen oder verfügbar für einen bestimmten Container|Keine Garantien. Die einem bestimmten Container zugewiesenen RUs hängen von den Eigenschaften ab. Bei diesen Eigenschaften kann es sich um die Partitionsschlüssel der Container, die den Durchsatz gemeinsam nutzen, um die Verteilung der Workload oder um die Anzahl von Containern handeln. |Alle RUs, die für den Container konfiguriert wurden, sind ausschließlich für diesen Container reserviert.|Keine Garantien. Die einem bestimmten Container zugewiesenen RUs hängen von den Eigenschaften ab. Bei diesen Eigenschaften kann es sich um die Partitionsschlüssel der Container, die den Durchsatz gemeinsam nutzen, um die Verteilung der Workload oder um die Anzahl von Containern handeln. |Alle RUs, die für den Container konfiguriert wurden, sind ausschließlich für diesen Container reserviert.|
|Maximale Speicherkapazität für einen Container|Unbegrenzt.|Unbegrenzt|Unbegrenzt|Unbegrenzt|
|Maximaler Durchsatz pro logischer Partition eines Containers|10.000 RU/s|10.000 RU/s|10.000 RU/s|10.000 RU/s|
|Maximale Speicherkapazität (Daten und Index) pro logischer Partition eines Containers|20 GB|20 GB|20 GB|20 GB|

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB](how-to-provision-database-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen von automatisch skaliertem Durchsatz für eine Datenbank oder einen Container in Azure Cosmos DB](how-to-provision-autoscale-throughput.md).
