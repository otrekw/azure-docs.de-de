---
title: Zuordnen von Datenflüssen
description: Übersicht über Mapping Data Flows in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/09/2020
ms.openlocfilehash: 850879675d4554329f24c86f2ac28660b303084c
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475565"
---
# <a name="what-are-mapping-data-flows"></a>Was sind Zuordnungsdatenflüsse?

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mapping Data Flows (Zuordnungsdatenflüsse) sind visuell entworfene Datentransformationen in Azure Data Factory. Mit Data Flows können Data Engineers grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die daraus resultierenden Datenflüsse werden als Aktivitäten in Azure Data Factory-Pipelines ausgeführt, für die erweiterte Apache Spark-Cluster verwendet werden. Datenflussaktivitäten können über vorhandene Planungs-, Steuerungs-, Fluss- und Überwachungsfunktionen in Data Factory aktiviert werden.

Zuordnungsdatenflüsse bieten eine vollständig visuelle Darstellung, ohne Code geschrieben werden muss. Ihre Datenflüsse werden in Ihrem eigenen Ausführungscluster für erweiterte Datenverarbeitung ausgeführt. Azure Data Factory übernimmt die gesamte Codeübersetzung, Pfadoptimierung und Ausführung Ihrer Datenflussaufträge.

![Aufbau](media/data-flow/adf-data-flows.png "Aufbau")

## <a name="getting-started"></a>Erste Schritte

Wählen Sie zum Erstellen eines Datenflusses unter **Factory Resources** (Factory-Ressourcen) das Pluszeichen und dann die Option **Datenfluss** aus. 

![Neuer Datenfluss](media/data-flow/newdataflow2.png "neuer Datenfluss")

Mit dieser Aktion gelangen Sie zur Datenflusscanvas, auf der Sie Ihre Transformationslogik erstellen können. Wählen Sie **Quelle hinzufügen** aus, um mit der Konfiguration Ihrer Quelltransformation zu beginnen. Weitere Informationen finden Sie im Artikel zur [Quelltransformation](data-flow-source.md).

## <a name="data-flow-canvas"></a>Datenflusscanvas

Die Datenflusscanvas ist in drei Bereiche unterteilt: die obere Leiste, das Diagramm und den Konfigurationsbereich. 

![Canvas](media/data-flow/canvas1.png "Canvas")

### <a name="graph"></a>Graph

Das Diagramm zeigt den Transformationsdatenstrom. Es zeigt die Herkunft der Quelldaten beim Fließen in eine oder mehrere Senken. Wählen Sie die Option **Quelle hinzufügen** aus, um eine neue Quelle hinzuzufügen. Wählen Sie zum Hinzufügen einer neuen Transformation unten rechts in einer vorhandenen Transformation das Pluszeichen aus.

![Canvas](media/data-flow/canvas2.png "Canvas")

### <a name="azure-integration-runtime-data-flow-properties"></a>Azure Integration Runtime: Datenflusseigenschaften

![Schaltfläche „Debuggen“](media/data-flow/debugbutton.png "Schaltfläche „Debuggen“")

Wenn Sie in ADF mit der Verwendung von Datenflüssen beginnen, sollten Sie oben in der Benutzeroberfläche des Browsers die Option „Debuggen“ für Datenflüsse aktivieren. Hiermit wird ein Spark-Cluster erstellt, der für das interaktive Debuggen, die Datenvorschau und die Ausführung von Debugvorgängen für die Pipeline verwendet wird. Sie können die Größe des verwendeten Clusters festlegen, indem Sie eine benutzerdefinierte [Azure Integration Runtime](concepts-integration-runtime.md)-Instanz auswählen. Die Debugsitzung bleibt nach Ihrer letzten Datenvorschau bzw. letzten Ausführung eines Debugvorgangs für die Pipeline bis zu 60 Minuten lang aktiv.

Wenn Sie Ihre Pipelines mit Datenflussaktivitäten operationalisieren, wird von ADF die Azure Integration Runtime-Instanz verwendet, die in der „Run On“-Eigenschaft der [Aktivität](control-flow-execute-data-flow-activity.md) zugeordnet ist.

Die Azure Integration Runtime-Standardinstanz ist ein einzelner kleiner Workerknotencluster mit vier Kernen, der Ihnen das Anzeigen einer Datenvorschau und das schnelle Ausführen von Debugpipelines zu geringen Kosten ermöglicht. Legen Sie eine höhere Azure IR-Konfiguration fest, wenn Sie Vorgänge für große Datasets durchführen.

Sie können ADF anweisen, einen Pool mit Clusterressourcen (VMs) vorzuhalten, indem Sie in den Datenflusseigenschaften der Azure IR-Instanz eine Gültigkeitsdauer angeben. Diese Aktion führt bei nachfolgenden Aktivitäten zu einer schnelleren Auftragsausführung.

#### <a name="azure-integration-runtime-and-data-flow-strategies"></a>Azure Integration Runtime und Datenflussstrategien

##### <a name="execute-data-flows-in-parallel"></a>Paralleles Ausführen von Datenflüssen

Wenn Sie Datenflüsse in einer Pipeline parallel ausführen, erstellt ADF separate Spark-Cluster für jede Ausführung einer Aktivität. Dies erfolgt basierend auf den Einstellungen in Ihrer Azure Integration Runtime-Instanz, die jeder Aktivität zugeordnet ist. Fügen Sie zum Entwerfen von parallelen Ausführungen in ADF-Pipelines Ihre Datenflussaktivitäten ohne Rangfolgeneinschränkung in der Benutzeroberfläche hinzu.

Von diesen drei Optionen weist diese Option normalerweise die kürzeste Ausführungsdauer auf. Da jeder parallele Datenfluss gleichzeitig in separaten Clustern ausgeführt wird, ist die Sortierung der Ereignisse nicht deterministisch.

Wenn Sie Ihre Datenflussaktivitäten in ihren Pipelines parallel ausführen, sollten Sie nicht TTL verwenden. Dies liegt daran, dass die gleichzeitige parallele Ausführung Ihrer Datenflüsse mit derselben Azure Integration Runtime zu mehreren aktiven Poolinstanzen für Ihre Data Factory führt.

##### <a name="overload-single-data-flow"></a>Überladen eines einzelnen Datenflusses

Wenn Sie Ihre gesamte Logik in einem einzelnen Datenfluss anordnen, verwendet ADF für die gesamte Ausführung denselben Auftragsausführungskontext in einer einzelnen Spark-Clusterinstanz.

Bei dieser Option kann die Verfolgung und Problembehandlung unter Umständen eine größere Herausforderung sein, weil Ihre Geschäftsregeln und die Geschäftslogik durcheinander geworfen werden können. Zudem bietet diese Option keine gute Wiederverwendbarkeit.

##### <a name="execute-data-flows-sequentially"></a>Sequenzielles Ausführen von Datenflüssen

Wenn Sie Ihre Datenflussaktivitäten in der Pipeline nacheinander ausführen und für die Azure IR-Konfiguration eine Gültigkeitsdauer festgelegt haben, werden die Computeressourcen (VMs) von ADF wiederverwendet. Dies führt zu kürzeren nachfolgenden Ausführungszeiten. Sie erhalten weiterhin einen neuen Spark-Kontext für jede Ausführung.

Von diesen drei Optionen weist diese Aktion normalerweise die längste Ausführungsdauer des Gesamtprozesses auf. Sie ermöglicht aber eine saubere Trennung der logischen Vorgänge in jedem Datenflussschritt.

### <a name="configuration-panel"></a>Konfigurationsbereich

Im Konfigurationsbereich werden die spezifischen Einstellungen für die derzeit ausgewählte Transformation angezeigt. Wenn keine Transformation ausgewählt ist, wird der Datenfluss angezeigt. In der allgemeinen Datenflusskonfiguration können Sie den Namen und die Beschreibung auf der Registerkarte **Allgemein** bearbeiten oder Parameter über die Registerkarte **Parameter** hinzufügen. Weitere Informationen finden Sie unter [Mapping Data Flow-Parameter](parameters-data-flow.md).

Jede Transformation enthält mindestens vier Registerkarten für die Konfiguration.

#### <a name="transformation-settings"></a>Transformationseinstellungen

Die erste Registerkarte im Konfigurationsbereich jeder Transformation enthält die Einstellungen, die für diese Transformation spezifisch sind. Weitere Informationen finden Sie auf der Dokumentationsseite für diese Transformation.

![Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Registerkarte „Quelleinstellungen“")

#### <a name="optimize"></a>Optimieren

Die Registerkarte **Optimieren** enthält Einstellungen zum Konfigurieren von Partitionierungsschemas. Weitere Informationen zum Optimieren Ihrer Datenflüsse finden Sie in der [Anleitung zur Leistung des Zuordnungsdatenflusses](concepts-data-flow-performance.md).

![Optimieren](media/data-flow/optimize.png "Optimieren")

#### <a name="inspect"></a>Überprüfen

Die Registerkarte **Überprüfen** bietet einen Einblick in die Metadaten des Datenstroms, den Sie transformieren. Sie können die Spaltenanzahl, geänderte Spalten, hinzugefügte Spalten, Datentypen, die Spaltensortierung und Spaltenverweise sehen. **Überprüfen** ist eine schreibgeschützte Ansicht Ihrer Metadaten. Der Debugmodus muss nicht aktiviert sein, um die Metadaten im Bereich **Überprüfen** anzeigen zu können.

![Überprüfen](media/data-flow/inspect1.png "Überprüfen")

Wenn Sie die Form Ihrer Daten durch Transformationen ändern, wird der Fluss der Metadatenänderungen im Bereich **Überprüfen** angezeigt. Falls in Ihrer Quelltransformation kein definiertes Schema vorhanden ist, werden im Bereich **Überprüfen** keine Metadaten angezeigt. Fehlende Metadaten kommen in Schemaabweichungsszenarien häufiger vor.

#### <a name="data-preview"></a>Datenvorschau

Bei aktiviertem Debugmodus können Sie auf der Registerkarte **Datenvorschau** eine interaktive Momentaufnahme der Daten bei jeder Transformation anzeigen. Weitere Informationen finden Sie unter [Datenvorschau im Debugmodus](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Obere Leiste

Die obere Leiste enthält Aktionen, die sich auf den gesamten Datenfluss auswirken, z. B. das Speichern und Überprüfen. Sie können auch mithilfe der Schaltflächen **Diagramm anzeigen** und **Diagramm ausblenden** zwischen dem Diagramm- und Konfigurationsmodus wechseln.

![Diagramm ausblenden](media/data-flow/hideg.png "Diagramm ausblenden")

Wenn Sie das Diagramm ausblenden, können Sie über die Schaltflächen **Zurück** und **Weiter** seitwärts durch die Transformationsknoten blättern.

![Schaltflächen „Zurück“ und „Weiter“](media/data-flow/showhide.png "Schaltflächen „Zurück“ und „Weiter“")

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die Erstellung einer [Quelltransformation](data-flow-source.md).
* Informieren Sie sich darüber, wie Sie Ihre Datenflüsse im [Debugmodus](concepts-data-flow-debug-mode.md) erstellen.
