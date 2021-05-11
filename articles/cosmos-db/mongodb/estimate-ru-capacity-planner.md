---
title: Kostenabschätzung mithilfe des Azure Cosmos DB-Kapazitätsplaners - API für Mongo DB
description: Mit dem Azure Cosmos DB Capacity Planner können Sie den erforderlichen Durchsatz (RU/s) und die Kosten für Ihre Workload schätzen. In diesem Artikel wird beschrieben, wie Sie mit dem Kapazitätsplaner den erforderlichen Durchsatz und die Kosten abschätzen, um Azure Cosmos DB API für MongoDB zu verwenden.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 04/20/2021
ms.author: dech
ms.openlocfilehash: da3d6ca0323d7a0a9a7d5f5d2f0b800615f773cd
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108065677"
---
# <a name="estimate-rus-using-the-azure-cosmos-db-capacity-planner---azure-cosmos-db-api-for-mongodb"></a>Schätzen Sie RU/s mithilfe des Azure Cosmos DB-Kapazitätsplaners ab - Azure Cosmos DB API für MongoDB
[!INCLUDE[appliesto-sql-api](../includes/appliesto-mongodb-api.md)]

Die Konfiguration Ihrer Datenbanken und Sammlungen mit der richtigen Menge an bereitgestelltem Durchsatz, bzw. [Anforderungseinheiten (RU/s)](../request-units.md), für Ihre workload ist entscheidend für die Optimierung von Kosten und Leistung. In diesem Artikel wird beschrieben, wie Sie den Azure Cosmos DB-[Kapazitätsplaner](https://cosmos.azure.com/capacitycalculator/) verwenden, um eine Schätzung der erforderlichen RU/s und der Kosten für Ihre workload zu erhalten, wenn Sie die Azure Cosmos DB API für MongoDB verwenden. Falls Sie die SQL-API verwenden, lesen Sie den Artikel zur Verwendung von [Kapazitätsrechner mit SQL-API](../estimate-ru-with-capacity-planner.md).

[!INCLUDE [capacity planner modes](../includes/capacity-planner-modes.md)]

## <a name="estimate-provisioned-throughput-and-cost-using-basic-mode"></a><a id="basic-mode"></a>Schätzen des bereitgestellten Durchsatzes und der Kosten im Modus „Basic“
Navigieren Sie zum [Capacity Planner](https://cosmos.azure.com/capacitycalculator/), um eine schnelle Schätzung für Ihre Workload im Modus „Basic“ zu erhalten. Geben Sie die folgenden Parameter basierend auf Ihrer Workload ein:

|**Input** (Eingabe)  |**Beschreibung**  |
|---------|---------|
| API |MongoDB-API auswählen |
|Number of regions (Anzahl von Regionen)|Azure Cosmos DB API für MongoDB ist in allen Azure-Regionen verfügbar. Wählen Sie die Anzahl von Regionen aus, die für Ihre Workload erforderlich ist. Sie können eine beliebige Anzahl von Regionen mit Ihrem Konto verknüpfen. Details finden Sie unter [Globale Verteilung](../distribute-data-globally.md).|
|Schreibvorgänge in mehreren Regionen|Wenn Sie [Multi-region writes](../distribute-data-globally.md#key-benefits-of-global-distribution) (Schreibvorgänge in mehreren Regionen) aktivieren, kann Ihre Anwendung Lese- und Schreibvorgänge für alle Azure-Regionen durchführen. Bei einer Deaktivierung der Schreibvorgänge in mehreren Regionen kann Ihre Anwendung Daten in einer Region schreiben. <br/><br/> Aktivieren Sie Schreibvorgänge in mehreren Regionen, wenn Sie mit einer Aktiv-Aktiv-Workload rechnen, für die Schreibvorgänge mit geringer Latenz in mehreren Regionen erforderlich sind. Ein Beispiel hierfür ist eine IoT-Workload, bei der große Datenmengen in unterschiedlichen Regionen in die Datenbank geschrieben werden. <br/><br/> Für Schreibvorgänge in mehreren Regionen ist eine Lese- und Schreibverfügbarkeit von 99,999 % garantiert. Für Schreibvorgänge in mehreren Regionen ist im Vergleich mit dem Schreiben in einzelnen Regionen ein höherer Durchsatz erforderlich. Weitere Informationen finden Sie in dem Artikel, in dem beschrieben wird, [wie sich Anforderungseinheiten (RUs) für Schreibvorgänge in einzelne und mehrere Regionen unterscheiden](../optimize-cost-regions.md).|
|Gesamtanzahl der im Transaktionsspeicher gespeicherten Daten |Gesamtzahl der geschätzten gespeicherten Daten (GB) im Transaktionsspeicher in einer einzelnen Region.|
|Analysespeicher verwenden| Wählen Sie **Ein**, wenn Sie [Synapse analytical store](../synapse-link.md) verwenden möchten. Geben Sie die **Gesamtzahl der im analytischen Speicher** gespeicherten Daten ein, d. h. die geschätzten Daten (GB), die im analytischen Speicher in einer einzelnen Region gespeichert sind.  |
|Elementgröße|Die ungefähre Größe der Dokumente, liegt zwischen 1 KB und 2 MB. |
|Funde/Sek |Anzahl der erwarteten Suchvorgänge pro Sekunde, pro Region. |
|Einfügungen/s |Anzahl der erwarteten Einfügeoperationen pro Sekunde, pro Region. |
|Aktualisierungen/Sek |Anzahl der erwarteten Aktualisierungen pro Sekunde. |
|Löschvorgänge/Sek |Anzahl der erwarteten Löschvorgänge pro Sekunde, pro Region. |

Wählen Sie **Berechnen**, nachdem Sie die erforderlichen Angaben eingefügt haben. Auf der Registerkarte **Kostenschätzung** werden die Gesamtkosten für den Speicher und den bereitgestellten Durchsatz angezeigt. Sie können die Anzeige über den Link **Details anzeigen** auf dieser Registerkarte erweitern, um eine Aufstellung der Aufschlüsselung zu erhalten, die für Lese- und Schreibanforderungen jeweils benötigt wird. **Bei jeder Änderung des Wertes eines beliebigen Feldes klicken Sie auf Berechnen, um die geschätzten Kosten neu zu berechnen.**

:::image type="content" source="./media/estimate-ru-capacity-planner/basic-mode-mongodb-api.png" alt-text="Capacity Planner: Modus „Basic“" border="true":::

## <a name="estimate-provisioned-throughput-and-cost-using-advanced-mode"></a><a id="advanced-mode"></a>Schätzen des bereitgestellten Durchsatzes und der Kosten im Modus „Advanced“

Im Modus „Advanced“ können Sie weitere Einstellungen nutzen, die sich auf die Schätzung des Durchsatzes (RU/s) auswirken. Navigieren Sie zum Verwenden dieser Option zum [Capacity Planner](https://cosmos.azure.com/capacitycalculator/) und melden Sie sich im Tool mit einem Konto an, das Sie für Azure verwenden. Die Anmeldeoption befindet sich oben rechts.

Nachdem der Anmeldung sehen Sie im Vergleich zu den Feldern im Basismodus mehr Felder. Geben Sie die zusätzlichen Parameter basierend auf Ihrer Workload ein.

|**Input** (Eingabe)  |**Beschreibung**  |
|---------|---------|
|API|Azure Cosmos DB ist ein Dienst mit mehreren Modellen und APIs. Wählen Sie MongoDB-API aus. |
|Number of regions (Anzahl von Regionen)|Azure Cosmos DB API für MongoDB ist in allen Azure-Regionen verfügbar. Wählen Sie die Anzahl von Regionen aus, die für Ihre Workload erforderlich ist. Sie können Ihrem Cosmos-Konto eine beliebige Anzahl von Regionen zuordnen. Details finden Sie unter [Globale Verteilung](../distribute-data-globally.md).|
|Schreibvorgänge in mehreren Regionen|Wenn Sie [Multi-region writes](../distribute-data-globally.md#key-benefits-of-global-distribution) (Schreibvorgänge in mehreren Regionen) aktivieren, kann Ihre Anwendung Lese- und Schreibvorgänge für alle Azure-Regionen durchführen. Bei einer Deaktivierung der Schreibvorgänge in mehreren Regionen kann Ihre Anwendung Daten in einer Region schreiben. <br/><br/> Aktivieren Sie Schreibvorgänge in mehreren Regionen, wenn Sie mit einer Aktiv-Aktiv-Workload rechnen, für die Schreibvorgänge mit geringer Latenz in mehreren Regionen erforderlich sind. Ein Beispiel hierfür ist eine IoT-Workload, bei der große Datenmengen in unterschiedlichen Regionen in die Datenbank geschrieben werden. <br/><br/> Für Schreibvorgänge in mehreren Regionen ist eine Lese- und Schreibverfügbarkeit von 99,999 % garantiert. Für Schreibvorgänge in mehreren Regionen ist im Vergleich mit dem Schreiben in einzelnen Regionen ein höherer Durchsatz erforderlich. Weitere Informationen finden Sie in dem Artikel, in dem beschrieben wird, [wie sich Anforderungseinheiten (RUs) für Schreibvorgänge in einzelne und mehrere Regionen unterscheiden](../optimize-cost-regions.md).|
|Standardkonsistenz|Azure Cosmos DB API für MongoDB unterstützt 5 Konsistenzstufen, um Entwicklern zu ermöglichen, den Kompromiss zwischen Konsistenz, Verfügbarkeit und Latenz auszugleichen. Weitere Informationen finden Sie im Artikel zu [Konsistenzebenen](../consistency-levels.md). <br/><br/> Standardmäßig verwendet API für MongoDB Sitzungskonsistenz, wodurch gewährleistet wird, dass Sie Ihre eigenen Schreibvorgänge in einer Sitzung lesen können. <br/><br/> Bei der Auswahl der starken oder begrenzten Veraltung wird für Lesevorgänge der doppelte Durchsatz (RU/s) im Vergleich zu „Sitzungskonsistenz“, „Präfixkonsistenz“ und „Letztliche Konsistenz“ benötigt. Die starke Konsistenz mit Schreibvorgängen in mehreren Regionen wird nicht unterstützt, und es wird automatisch die Nutzung von Schreibvorgängen in einer einzelnen Region mit starker Konsistenz festgelegt. |
|Indizierungsrichtlinie| Bei Auswahl der Option **Aus** wird keine der Eigenschaften indexiert. Dies führt zu den niedrigsten RU-Gebühren für Schreibvorgänge. Deaktivieren Sie die Indexierungsrichtlinie, wenn Sie nur das Feld _id und den Shard-Schlüssel für jede Abfrage verwenden wollen (beide pro Abfrage).<br/><br/> Bei Verwendung der Option **Automatisch** indexieren die Versionen 3.6 und höher von API für MongoDB automatisch das Feld _id.<br/><br/>Falls Sie die Option **Benutzerdefiniert** wählen, können Sie bestimmte Eigenschaften indexieren, Indexe mit mehreren Schlüsseln, zusammengesetzte Indexe oder Wildcard-Indexe verwenden. Wildcard-Indexe für alle Felder zur flexiblen und effizienten Abfrage. Sie können die Anzahl der indexierten Eigenschaften später im Formular eingeben. Weitere Informationen finden Sie unter [Indexverwaltung](../mongodb-indexing.md) in API für MongoDB.|
|Gesamtanzahl der im Transaktionsspeicher gespeicherten Daten |Gesamtzahl der geschätzten gespeicherten Daten (GB) im Transaktionsspeicher in einer einzelnen Region.|
|Analysespeicher verwenden| Wählen Sie **Ein**, wenn Sie [Synapse analytical store](../synapse-link.md) verwenden möchten. Geben Sie die **Gesamtzahl der im analytischen Speicher** gespeicherten Daten ein, d. h. die geschätzten Daten (GB), die im analytischen Speicher in einer einzelnen Region gespeichert sind.  |
|Workloadmodus|Wählen Sie die **Konstant**-Option, wenn Ihr Workload-Volumen konstant ist. <br/><br/> Wählen Sie die **Variable**-Option, wenn sich Ihr Workload-Volumen im Laufe der Zeit ändert.  Beispielsweise an einem bestimmten Tag oder in einem Monat. Die folgende Einstellung ist verfügbar, wenn Sie die Variable-Workload-Option wählen:<ul><li>Percentage of time at peak (Spitzenzeiten in Prozent): Der Prozentsatz der Zeit eines Monats, in der für Ihre Workload der höchste Durchsatz (Spitzendurchsatz) erforderlich ist. </li></ul> <br/><br/> Wenn Sie beispielsweise über eine Workload verfügen, für die während der Geschäftszeiten an Werktagen zwischen 9 Uhr und 18 Uhr eine hohe Aktivität besteht, beträgt der Prozentsatz der Spitzenzeit: 45 Spitzenstunden/730 Stunden/Monat = ~6 %.<br/><br/>Mit Intervallen für Spitzenzeiten und andere Zeiten können Sie Ihre Kosten optimieren, indem Sie [Ihren bereitgestellten Durchsatz entsprechend programmgesteuert zentral hoch- und herunterskalieren](../set-throughput.md#update-throughput-on-a-database-or-a-container).|
|Elementgröße|Die Größe der Dokumente, liegt zwischen 1 KB und 2 MB. Sie können Schätzungen für mehrere Musterelemente hinzufügen. <br/><br/>Sie können auch die Option zum **Hochladen eines Beispieldokuments (JSON)** verwenden, um eine genauere Schätzung zu erhalten.<br/><br/>Wenn Ihre Workload über mehrere Arten von Elementen (mit unterschiedlichem JSON-Inhalt) in demselben Container verfügt, können Sie mehrere JSON-Dokumente hochladen und eine entsprechende Schätzung erhalten. Verwenden Sie die Schaltfläche **Add new item** (Neues Element hinzufügen), um mehrere JSON-Beispieldokumente hinzuzufügen.|
| Vorgangsart | Der Operationstyp wie **Suchen**, **Aggregieren**, **Ändern** usw.  |
| Gebühr für Anforderungseinheiten (Request Unit, RU) pro Anruf | Die geschätzte RU/s-Ladung für die Ausführung der gewählten Betriebsart. |
| Anrufe/Sek. pro Region | Ausgewählte Betriebsarten, die pro Sekunde pro Region ausgeführt werden. |

Sie können auch die Schaltfläche **Save Estimate** (Schätzung speichern) verwenden, um eine CSV-Datei mit der aktuellen Schätzung herunterzuladen.

:::image type="content" source="./media/estimate-ru-capacity-planner/advanced-mode-mongodb-api.png" alt-text="Capacity Planner: Modus „Advanced“" border="true":::

Die angezeigten Preise im Kapazitätsplaner sind Schätzungen, die auf den öffentlichen Preissätzen für Durchsatz und Speicherung basieren. Alle Preise sind in US-Dollar angegeben. Auf der Seite [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/) sind alle Preise nach Region aufgeführt.  

## <a name="estimating-throughput-for-queries"></a>Schätzen des Durchsatzes für Abfragen

Die Kapazitätsplanung geht von punktuellen Lesevorgängen (Abfragen, die die Feld _id und den Shard-Schlüssel enthalten) und Schreibvorgängen für die Workload aus. Führen Sie Ihre Abfrage zum Schätzen des Durchsatzes, der für Abfragen benötigt wird, für ein repräsentatives Dataset in einem Cosmos-Container aus, und [ermitteln Sie die Gebühr für Anforderungseinheiten](../find-request-unit-charge.md). Multiplizieren Sie die Gebühren für Anforderungseinheiten (RUs) mit der Anzahl von Abfragen, deren Ausführung Sie pro Sekunde erwarten, um die insgesamt benötigten Anforderungseinheiten pro Sekunde (RU/s) zu erhalten.

Falls für Ihre Workload beispielsweise eine Abfrage wie ``SELECT * FROM c WHERE c.id = 'Alice'`` erforderlich ist, die 100-mal pro Sekunde ausgeführt wird und über 10 RUs verfügt, benötigen Sie für diese Anforderungen insgesamt 100 Abfragen/s · 10 RUs/Abfrage = 1.000 RU/s. Addieren Sie diese Anforderungen pro Sekunde (RU/s) zu den Anforderungen pro Sekunde, die für die Lese- oder Schreibvorgänge Ihrer Workload erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich weiter über das [Preismodell von Azure Cosmos DB](../how-pricing-works.md).
* Erstellen Sie ein neues [Cosmos-Konto, eine Datenbank und einen Container](../create-cosmosdb-resources-portal.md).
* Erfahren Sie, wie Sie die [Kosten für den bereitgestellten Durchsatz optimieren](../optimize-cost-throughput.md) können.
* Informieren Sie sich über das [Optimieren der Kosten mit Reservekapazität](../cosmos-db-reserved-capacity.md).
