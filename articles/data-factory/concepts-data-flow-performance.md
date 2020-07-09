---
title: Anleitung zur Leistung und Optimierung des Zuordnungsdatenflusses
description: Erfahren Sie, welche Faktoren sich entscheidend auf die Leistung der Mapping Data Flow-Funktion in Azure Data Factory auswirken.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.custom: seo-lt-2019
ms.date: 05/21/2020
ms.openlocfilehash: 327fffd807d93fda67ff650954ece65e5db58e63
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83798112"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Die Mapping Data Flow-Funktion in Azure Data Factory bietet eine codefreie Schnittstelle zum Entwerfen, Bereitstellen und Orchestrieren von Datentransformationen in beliebigen Größenordnungen. Wenn Sie mit der Mapping Data Flow-Funktion nicht vertraut sind, finden Sie weitere Informationen in der [Übersicht über Mapping Data Flow](concepts-data-flow-overview.md).

Stellen Sie beim Entwerfen und Testen von Datenflüssen auf der Benutzeroberfläche von ADF sicher, dass der Debugmodus aktiviert ist, damit Sie Ihre Datenflüsse in Echtzeit ausführen können, ohne die Aufwärmphase eines Clusters abwarten zu müssen. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

Dieses Video zeigt einige Beispielzeiten für das Transformieren von Daten mit Datenflüssen:
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4rNxM]

## <a name="monitoring-data-flow-performance"></a>Überwachen der Datenflussleistung

Beim Entwerfen einer Mapping Data Flow-Funktion können Sie Komponententests für die einzelnen Transformationen durchführen, indem Sie im Konfigurationsbereich auf die Registerkarte für die Datenvorschau klicken. Nachdem Sie die Logik überprüft haben, führen Sie einen End-to-End-Test des Datenflusses als Aktivität in einer Pipeline durch. Fügen Sie eine Aktivität zum Ausführen der Datenflussaktivität (Execute Data Flow) aus, und verwenden Sie die Schaltfläche „Debug“ zum Testen der Leistung des Datenflusses. Um den Ausführungsplan und das Leistungsprofil Ihres Datenflusses zu öffnen, klicken Sie auf der Registerkarte „Ausgabe“ der Pipeline unter „Aktionen“ auf das Brillensymbol.

![Datenflussüberwachung](media/data-flow/mon002.png "Datenflussüberwachung 2")

 Anhand dieser Informationen können Sie die Leistung des Datenflusses für Datenquellen verschiedener Größen schätzen. Weitere Informationen finden Sie unter [Überwachen von Datenflüssen](concepts-data-flow-monitoring.md).

![Datenflussüberwachung](media/data-flow/mon003.png "Datenflussüberwachung 3")

 Bei Debugausführungen der Pipeline muss ungefähr eine Minute Einrichtungszeit für den Cluster in die Berechnung der Gesamtleistung für einen „warmen“ Cluster eingerechnet werden. Wenn Sie die standardmäßige Azure Integration Runtime initialisieren, kann die Einrichtungszeit etwa 5 Minuten betragen.

## <a name="increasing-compute-size-in-azure-integration-runtime"></a>Vergrößern der Computegröße in Azure Integration Runtime

Durch eine Integration Runtime mit mehr Kernen wird die Anzahl der Knoten in den Spark-Computeumgebungen erhöht und eine höhere Verarbeitungsleistung für das Lesen, Schreiben und Transformieren Ihrer Daten geboten. ADF-Datenflüsse nutzen Spark für die Compute-Engine. Die Spark-Umgebung funktioniert sehr gut für speicheroptimierte Ressourcen.
* Versuchen Sie es mit einem **computeoptimierten** Cluster, wenn die Verarbeitungsrate höher als die Eingaberate sein soll.
* Versuchen Sie es mit einem **arbeitsspeicheroptimierten** Cluster, wenn Sie mehr Daten im Arbeitsspeicher zwischenspeichern möchten. Bei der arbeitsspeicheroptimierten Variante fallen zwar höhere Kosten pro Kern an als bei der computeoptimierten Variante, dafür ist jedoch wahrscheinlich die Transformationsgeschwindigkeit höher. Wenn beim Ausführen der Datenflüsse Fehler aufgrund von nicht genügend Arbeitsspeicher auftreten, wechseln Sie zu einer arbeitsspeicheroptimierten Azure IR-Konfiguration.

![Neue Integration Runtime](media/data-flow/ir-new.png "Neue Integration Runtime")

Weitere Informationen zum Erstellen einer Integration Runtime finden Sie unter [Integrationslaufzeit in Azure Data Factory](concepts-integration-runtime.md).

### <a name="increase-the-size-of-your-debug-cluster"></a>Erhöhen der Größe Ihres Debugclusters

Standardmäßig wird beim Aktivieren des Debugmodus die standardmäßige Azure Integration Runtime verwendet, die automatisch für jede Data Factory erstellt wird. Diese standardmäßige Azure Integration Runtime ist auf acht Kerne festgelegt, d.h. vier für einen Treiberknoten und vier für einen Workerknoten, wobei die Eigenschaften für „Compute allgemein“ verwendet werden. Beim Testen mit größeren Datenmengen können Sie Ihren Debugcluster vergrößern, indem Sie eine Azure Integration Runtime mit umfangreicheren Konfigurationen erstellen und diese neue Azure Integration Runtime auswählen, wenn Sie das Debuggen aktivieren. Dadurch wird ADF angewiesen, diese Azure Integration Runtime für die Datenvorschau und das Debuggen der Pipeline mit Datenflüssen zu verwenden.

### <a name="decrease-cluster-compute-start-up-time-with-ttl"></a>Verkürzen der Computestartzeit für Cluster mit TTL

In der Azure IR gibt es unter den Datenflusseigenschaften eine Eigenschaft, mit der Sie einen Pool aus Clustercomputeressourcen für Ihre Factory einrichten können. Mit diesem Pool können Sie Datenflussaktivitäten sequenziell zur Ausführung übermitteln. Wenn der Pool eingerichtet ist, benötigt der bedarfsgesteuerte Spark-Cluster 1-2 Minuten für jeden nachfolgenden Auftrag. Die anfängliche Einrichtung des Ressourcenpools dauert etwa 6 Minuten. Geben Sie in der TTL-Einstellung (Time-To-Live) an, wie lange der Ressourcenpool beibehalten werden soll.

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse-synapse"></a>Optimierung für Azure SQL-Datenbank und Azure SQL Data Warehouse – Synapse

### <a name="partitioning-on-source"></a>Partitionierung für die Quelle

1. Wechseln Sie zur Registerkarte **Optimieren**, und wählen Sie **Partitionierung festlegen** aus.
1. Wählen Sie **Quelle** aus.
1. Legen Sie unter **Anzahl von Partitionen** die maximale Anzahl von Verbindungen mit Ihrer Azure SQL-Datenbank-Instanz fest. Sie können eine höhere Einstellung testen, um parallele Verbindungen mit Ihrer Datenbank nutzen zu können. In einigen Fällen erreichen Sie jedoch mit einer begrenzten Anzahl von Verbindungen eine höhere Leistung.
1. Wählen Sie aus, ob nach einer bestimmten Tabellenspalte oder einer Abfrage partitioniert werden soll.
1. Wenn Sie **Spalte** ausgewählt haben, wählen Sie noch die Partitionsspalte aus.
1. Wenn Sie **Abfrage** ausgewählt haben, geben Sie eine Abfrage ein, die dem Partitionierungsschema Ihrer Datenbanktabelle entspricht. Diese Abfrage ermöglicht der Quelldatenbank-Engine die Nutzung der Partitionsbeseitigung. Ihre Quelldatenbanktabellen müssen nicht partitioniert werden. Ist Ihre Quelle nicht bereits partitioniert, wird von ADF die Datenpartitionierung in der Spark-Transformationsumgebung weiterhin basierend auf dem in der Quelltransformation ausgewählten Schlüssel verwendet.

![Quellteil](media/data-flow/sourcepart3.png "Quellteil")

> [!NOTE]
> Ein guter Anhaltspunkt für die Auswahl der Anzahl der Partitionen für Ihre Quelle basiert auf der Anzahl der Kerne, die Sie für Ihre Azure Integration Runtime festgelegt haben, multipliziert mit fünf. Wenn Sie beispielsweise eine Reihe von Dateien in Ihren ADLS-Ordnern transformieren und eine Azure IR mit 32 Kernen verwenden möchten, ist die Zielanzahl der Partitionen 32 x 5 = 160.

### <a name="source-batch-size-input-and-isolation-level"></a>Batchgröße, Eingabe und Isolationsstufe der Quelle

Die folgenden Einstellungen unter **Quelloptionen** in der Quelltransformation können sich auf die Leistung auswirken:

* Durch die Batchgröße wird ADF angewiesen, Daten im Spark-Speicher in Gruppen statt zeilenweise zu speichern. Die Batchgröße ist eine optionale Einstellung. Sie könnte dazu führen, dass nicht genügend Ressourcen auf den Computeknoten vorhanden sind, wenn diese nicht ordnungsgemäß dimensioniert sind. Wenn diese Eigenschaft nicht festgelegt wird, werden Batchstandardwerte für die Spark-Zwischenspeicherung verwendet.
* Durch das Festlegen einer Abfrage können Sie Zeilen an der Quelle filtern, bevor sie im Datenfluss zur Verarbeitung eintreffen. Dadurch kann die anfängliche Datenerfassung beschleunigt werden. Wenn Sie eine Abfrage verwenden, können Sie optionale Abfragehinweise für Ihre Azure SQL-Datenbank-Instanz hinzufügen, z.B. READ UNCOMMITTED.
* READ UNCOMMITTED bietet schnellere Abfrageergebnisse zur Quelltransformation.

![Quelle](media/data-flow/source4.png "`Source`")

### <a name="sink-batch-size"></a>Batchgröße für die Senke

Um die zeilenweise Verarbeitung Ihrer Datenflüsse zu vermeiden, legen Sie die **Batchgröße** auf der Registerkarte „Einstellungen“ für die Azure SQL-Datenbank- und Azure SQL Data Warehouse-Senke fest. Ist die Batchgröße festgelegt, verarbeitet ADF Schreibvorgänge in der Datenbank in Batches basierend auf der angegebenen Größe. Wenn diese Eigenschaft nicht festgelegt wird, werden Batchstandardwerte für die Spark-Zwischenspeicherung verwendet.

![Senke](media/data-flow/sink4.png "Senke")

### <a name="partitioning-on-sink"></a>Partitionierung für die Senke

Auch wenn die Daten in den Zieltabellen nicht partitioniert sind, wird empfohlen, die Daten in der Senkentransformation zu partitionieren. Partitionierte Daten bewirken häufig sehr viel schnellere Ladevorgänge, da alle Verbindungen gezwungen werden, einen einzigen Knoten bzw. eine einzige Partition zu verwenden. Wechseln Sie zur Registerkarte „Optimieren“ der Senke, und wählen Sie die *Roundrobin*-Partitionierung aus, um die ideale Anzahl von Partitionen zum Schreiben in die Senke auszuwählen.

### <a name="disable-indexes-on-write"></a>Deaktivieren von Indizes beim Schreiben

Fügen Sie in der Pipeline eine [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md) vor der Datenflussaktivität hinzu, die Indizes in Ihren Zieltabellen deaktiviert, in die von der Senke aus geschrieben wird. Fügen Sie nach der Datenflussaktivität eine weitere Aktivität „Gespeicherte Prozedur“ hinzu, die diese Indizes aktiviert. Nutzen Sie alternativ die vorab und nachträglich verarbeiteten Skripts in einer Datenbanksenke.

### <a name="increase-the-size-of-your-azure-sql-db-and-dw"></a>Erhöhen der Größe Ihrer Azure SQL-Datenbank- und Azure SQL Data Warehouse-Instanz

Planen Sie die Anpassung der Größe Ihrer Quelle und Senke in Azure SQL-Datenbank und Azure SQL Data Warehouse vor der Pipelineausführung, um den Durchsatz zu erhöhen und die Drosselung durch Azure beim Erreichen der DTU-Grenzwerte zu minimieren. Stellen Sie nach Abschluss der Pipelineausführung die Größe Ihrer Datenbanken für die normale Ausführung wieder her.

* Bei Verwendung arbeitsspeicheroptimierter Debug-Azure IRs mit 80 Kernen dauert die Transformation einer SQL-Datenbank-Quelltabelle mit 887.000 Zeilen und 74 Spalten in eine SQL-Datenbank-Tabelle mit einer einzelnen abgeleiteten Spalte etwa drei Minuten.

### <a name="azure-synapse-sql-dw-only-use-staging-to-load-data-in-bulk-via-polybase"></a>Verwenden von Staging zum Massenladen von Daten über Polybase [nur Azure Synapse SQL-DW]

Um zeilenweise Einfügungen in Data Warehouse zu vermeiden, aktivieren Sie unter den Senkeneinstellungen die Option **Staging aktivieren**, damit ADF [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) verwenden kann. PolyBase ermöglicht ADF das Massenladen der Daten.
* Wenn Sie die Datenflussaktivität aus einer Pipeline ausführen, müssen Sie einen Blob- oder ADLS Gen2-Speicherort auswählen, um die Daten während des Massenladevorgangs bereitzustellen.

* Bei Verwendung arbeitsspeicheroptimierter Debug-Azure IRs mit 80 Kernen dauert die Transformation einer Dateiquelle mit einer Datei (421 MB) mit 74 Spalten in eine Synapse-Tabelle und eine einzelne abgeleitete Spalte etwa vier Minuten.

## <a name="optimizing-for-files"></a>Optimieren für Dateien

Bei jeder Transformation können Sie das Partitionierungsschema, das von Data Factory verwendet werden soll, auf der Registerkarte „Optimieren“ festlegen. Es empfiehlt sich, zunächst dateibasierte Senken zu testen und dabei die Standardpartitionierung und -optimierungen beizubehalten.

* Bei kleineren Dateien kann manchmal das Auswählen von weniger Partitionen besser und schneller sein, als Spark anzuweisen, die kleinen Dateien zu partitionieren.
* Wenn Sie nicht über genügend Informationen zu Ihren Quelldaten verfügen, wählen Sie die *Roundrobin*-Partitionierung aus, und legen Sie die Anzahl der Partitionen fest.
* Wenn die Daten Spalten aufweisen, die gute Hashschlüssel darstellen können, wählen Sie *Hashpartitionierung* aus.

* Bei Verwendung arbeitsspeicheroptimierter Debug-Azure IRs mit 80 Kernen dauert die Transformation einer Dateiquelle mit einer Dateisenke einer Datei (421 MB) mit 74 Spalten in eine einzelne abgeleitete Spalte etwa zwei Minuten.

Beim Debuggen in der Datenvorschau sowie beim Debuggen der Pipeline gelten das Limit und die Stichprobengrößen für dateibasierte Quelldatasets nur für die Anzahl zurückgegebener Zeilen (nicht für die Anzahl gelesener Zeilen). Dies kann sich auf die Leistung der Debugausführungen auswirken und ggf. dazu führen, dass der Ablauf nicht erfolgreich ist.
* Debugcluster sind standardmäßig kleine Cluster mit einem einzelnen Knoten, und es wird empfohlen, kleine Beispieldateien zum Debuggen zu verwenden. Navigieren Sie zu den Debugeinstellungen, und verweisen Sie unter Verwendung einer temporären Datei auf eine kleine Teilmenge Ihrer Daten.

    ![Debugeinstellungen](media/data-flow/debugsettings3.png "Debugeinstellungen")

### <a name="file-naming-options"></a>Optionen für die Benennung von Dateien

Die gängigste Methode zum Schreiben transformierter Daten in Mapping Data Flow ist das Schreiben in den Blob- oder ADLS-Dateispeicher. Sie müssen in Ihrer Senke ein Dataset auswählen, das auf einen Container oder Ordner verweist, und nicht auf eine benannte Datei. Da Mapping Data Flow für die Ausführung Spark verwendet, wird die Ausgabe basierend auf dem Partitionierungsschema auf mehrere Dateien aufgeteilt.

Ein häufig verwendetes Partitionierungsschema ist die Auswahl von _Ausgabe in eine einzelne Datei_, wodurch alle PART-Dateien der Ausgabe in einer einzigen Datei in der Senke zusammengefasst werden. Dieser Vorgang erfordert, dass die Ausgabe sich auf eine einzige Partition auf einem einzigen Clusterknoten beschränkt. Wenn Sie viele große Quelldateien in einer einzigen Ausgabedatei kombinieren, kann dies dazu führen, dass die Clusterknotenressourcen nicht mehr ausreichen.

Um zu vermeiden, dass die Computeknotenressourcen erschöpft werden, behalten Sie das standardmäßige, optimierte Schema im Datenfluss bei, und fügen Sie eine Kopieraktivität zur Pipeline hinzu, durch die alle PART-Dateien des Ausgabeordners in eine neue Einzeldateien zusammengeführt werden. Diese Technik wird verwendet, um die Aktion der Transformation von der Dateizusammenführung zu trennen, erzielt aber dasselbe Ergebnis wie das Festlegen der _Ausgabe in eine einzelne Datei_.

### <a name="looping-through-file-lists"></a>Durchlaufen von Dateilisten

Eine Mapping Data Flow-Funktion wird besser ausgeführt, wenn die Quelltransformation mehrere Dateien durchläuft, anstatt über die ForEach-Aktivität als Schleife ausgeführt zu werden. Es wird empfohlen, Platzhalter oder Dateilisten in der Quelltransformation zu verwenden. Der Datenflussprozess wird schneller ausgeführt, wenn die Schleife innerhalb des Spark-Clusters ausgeführt werden kann. Weitere Informationen finden Sie unter [Platzhalter in der Quelltransformation](connector-azure-data-lake-storage.md#mapping-data-flow-properties).

Wenn Sie z.B. über eine Liste von Datendateien vom Juli 2019 verfügen, die Sie in einem Ordner in Blob Storage verarbeiten möchten, können Sie den folgenden Platzhalter in der Quelltransformation verwenden.

```DateFiles/*_201907*.txt```

Durch die Verwendung von Platzhaltern enthält die Pipeline nur eine Datenflussaktivität. Dadurch erzielen Sie eine bessere Leistung als bei einem Suchvorgang für den Blobspeicher, bei der dann alle entsprechenden Dateien mittels „ForEach“ mit einer eingeschlossenen Aktivität zum Ausführen des Datenflusses durchlaufen werden.

Die Pipeline „ForEach“ im parallelen Modus erzeugt mehrere Cluster, indem sie Auftragscluster für jede ausgeführte Datenflussaktivität erzeugt. Dies kann zu einer Drosselung des Azure-Diensts mit einer hohen Anzahl gleichzeitiger Ausführungen führen. Durch das Ausführen des Datenflusses in einer ForEach-Schleife, wobei die sequenzielle Ausführung in der Pipeline festgelegt ist, werden jedoch Drosselung und Ressourcenauslastung vermieden. Dies erzwingt die sequenzielle Ausführung der einzelnen Dateien für einen Datenfluss durch die Data Factory.

Es wird empfohlen, bei sequenzieller Verwendung von ForEach mit einem Datenfluss die TTL-Einstellung in der Azure Integration Runtime verwenden. Der Grund dafür ist, dass jede Datei eine Clusterstartzeit von 5 Minuten im Iterator erfordert.

### <a name="optimizing-for-cosmosdb"></a>Optimieren für Cosmos DB

Die Festlegungen für Durchsatz- und Batcheigenschaften für Cosmos DB-Senken gelten nur während der Ausführung dieses Datenflusses von einer Pipeline-Datenflussaktivität. Nach der Datenflussausführung gelten in Cosmos DB wieder die ursprünglichen Sammlungseinstellungen.

* Batchgröße: Berechnen Sie die ungefähre Zeilengröße der Daten, und stellen Sie sicher, dass Zeilengröße × Batchgröße kleiner als 2 Millionen ist. Erhöhen Sie andernfalls die Batchgröße, um einen besseren Durchsatz zu erzielen.
* Durchsatz: Legen Sie hier einen höheren Durchsatz fest, damit Dokumente schneller in Cosmos DB geschrieben werden können. Beachten Sie die höheren RU-Kosten bei einer höheren Durchsatzeinstellung.
*   Schreibdurchsatz: Verwenden Sie einen Wert, der kleiner als die Gesamtanzahl der RUs pro Minute ist. Wenn Ihr Datenfluss eine hohe Anzahl von Spark-Partitionen enthält, können Sie durch das Festlegen eines Durchsatzbudgets eine bessere Balance zwischen diesen Partitionen erzielen.

## <a name="join-performance"></a>Verknüpfungsleistung

Das Verwalten der Leistung von Verknüpfungen in Ihrem Datenfluss ist ein sehr gängiger Vorgang, den Sie während des Lebenszyklus Ihrer Datentransformationen ausführen. In ADF müssen Daten in Datenflüssen nicht vor Verknüpfungen sortiert werden, da diese Vorgänge in Spark als Hashjoins ausgeführt werden. Sie können jedoch von der verbesserten Leistung mit der Join-Optimierung „Broadcast“ (Übertragen) profitieren, die für die Transformationen „Joins“, „Exists“ und „Lookup“ gilt.

Hierdurch werden dynamische Zufallswiedergaben vermieden, indem die Inhalte von beiden Seiten Ihrer Verknüpfungsbeziehung per Push in den Spark-Knoten übertragen werden. Dies funktioniert gut bei kleineren Tabellen, die für Verweissuchvorgänge verwendet werden. Größere Tabellen, die möglicherweise nicht in den Arbeitsspeicher des Knotens passen, sind für die Broadcastoptimierung wenig geeignet.

Die empfohlene Konfiguration für Datenflüsse mit vielen Join-Vorgängen ist die Beibehaltung der Optimierung auf „Auto“ (für „Broadcast“) und die Verwendung einer arbeitsspeicheroptimierten Azure Integration Runtime-Konfiguration. Wenn bei den Datenflussausführungen Speicherfehler oder Übertragungstimeouts auftreten, können Sie die Broadcastoptimierung deaktivieren. Dies führt jedoch zu Datenflüssen mit geringerer Leistung. Optional können Sie den Datenfluss anweisen, nur die linke oder rechte Seite des Joins bzw. beide Seiten weiterzugeben.

![Broadcasteinstellungen](media/data-flow/newbroad.png "Broadcasteinstellungen")

Eine andere Optimierung der Verknüpfung besteht darin, ihre Verknüpfungen so zu erstellen, dass die Spark-Tendenz zum Implementieren von Kreuzprodukten vermieden wird. Wenn Sie beispielsweise Literalwerte in Ihre Verknüpfungsbedingungen einbeziehen, kann Spark dies zuerst als eine Anforderung zur Ausführung eines vollständigen kartesischen Produkts erkennen und dann die verknüpften Werte herausfiltern. Wenn Sie aber sicherstellen, dass es auf beiden Seiten Ihrer Verknüpfungsbedingung Spaltenwerte gibt, können Sie dieses durch Spark ausgelöste kartesische Produkt vermeiden und die Leistung Ihrer Verknüpfungen und Datenflüsse verbessern.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel zu Datenflüssen in Bezug auf die Leistung:

- [Registerkarte „Optimieren“ für Datenflüsse](concepts-data-flow-overview.md#optimize)
- [Datenflussaktivität](control-flow-execute-data-flow-activity.md)
- [Überwachen der Datenflussleistung](concepts-data-flow-monitoring.md)
