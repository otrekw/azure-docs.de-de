---
title: Transformieren von Daten mit einem verwalteten Azure Data Factory-Zuordnungsdatenfluss für VNET
description: Dieses Tutorial enthält Schritt-für-Schritt-Anleitungen für die Verwendung von Azure Data Factory zum Transformieren von Daten mithilfe von Mapping Data Flow.
author: djpmsft
ms.author: daperlov
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/19/2019
ms.openlocfilehash: 5515f6c4dfbc5d3c0e391373e763597d7fdc89ed
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528150"
---
# <a name="transform-data-securely-using-mapping-data-flows"></a>Sicheres Transformieren von Daten mithilfe von Zuordnungsdatenflüssen

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) zu lesen.

In diesem Tutorial erstellen Sie mithilfe der Azure Data Factory-Benutzeroberfläche eine Pipeline, mit der Daten **aus einer Azure Data Lake Storage (ADLS) Gen2-Quelle in eine ADLS Gen2-Senke (die jeweils nur den Zugriff auf ausgewählte Netzwerke zulassen)** kopiert und mithilfe eines Zuordnungsdatenflusses in ein [verwaltetes virtuelles Azure Data Factory-Netzwerk](managed-virtual-network-private-endpoint.md) transformiert werden. Das Konfigurationsmuster in diesem Tutorial kann beim Transformieren von Daten mithilfe eines Zuordnungsdatenflusses erweitert werden.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
>
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mit einer Datenflussaktivität
> * Erstellen einer Mapping Data Flow-Funktion mit vier Transformationen
> * Ausführen eines Testlaufs für die Pipeline
> * Überwachen einer Datenflussaktivität

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure-Speicherkonto**. Sie verwenden den ADLS-Speicher als *Quelldatenspeicher* und *Senkendatenspeicher*. Wenn Sie kein Speicherkonto besitzen, finden Sie unter [Informationen zu Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal) Schritte zum Erstellen eines solchen Kontos. **Stellen Sie sicher, dass das Storage-Konto nur den Zugriff über ausgewählte Netzwerke zulässt.** 

Die Datei, die in diesem Tutorial transformiert wird, ist „MoviesDB.csv“, die sich [hier](https://raw.githubusercontent.com/djpmsft/adf-ready-demo/master/moviesDB.csv) befindet. Zum Abrufen der Datei aus GitHub kopieren Sie den Inhalt in einen Text-Editor Ihrer Wahl, und speichern Sie ihn lokal als CSV-Datei. Wenn Sie die Datei in Ihr Speicherkonto hochladen möchten, finden Sie Informationen dazu unter [Hochladen von Blobs mit dem Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). In den Beispielen wird auf einen Container mit dem Namen „sample-data“ verwiesen.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

In diesem Schritt erstellen Sie eine Data Factory und öffnen die Data Factory-Benutzeroberfläche, um eine Pipeline in der Data Factory zu erstellen.

1. Öffnen Sie **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird derzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
2. Klicken Sie im Menü auf der linken Seite auf **Ressource erstellen** > **Analytics** > **Data Factory**.
3. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein.

   Der Name der Azure Data Factory muss *global eindeutig*sein. Wenn eine Fehlermeldung zum Namenswert angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. (Verwenden Sie beispielsweise „IhrNameADFTutorialDataFactory“.) Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Azure Data Factory – Benennungsregeln](naming-rules.md).

4. Wählen Sie das **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll.
5. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:

    a. Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

    b. Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 
         
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/management/overview.md). 
6. Wählen Sie unter **Version** die Option **V2**.
7. Wählen Sie unter **Standort** einen Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Datenspeicher (etwa Azure Storage und SQL-Datenbank) und Computeeinheiten (etwa Azure HDInsight), die von der Data Factory genutzt werden, können sich in anderen Regionen befinden.

8. Klicken Sie auf **Erstellen**.

9. Nach Abschluss der Erstellung wird der Hinweis im Benachrichtigungscenter angezeigt. Wählen Sie **Zu Ressource wechseln** aus, um zur Data Factory-Seite zu navigieren.
10. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.

## <a name="create-an-azure-integration-runtime-in-adf-managed-virtual-network"></a>Erstellen einer Azure Integration Runtime-Instanz in einem verwalteten virtuellen ADF-Netzwerk
In diesem Schritt erstellen Sie eine Azure Integration Runtime-Instanz und aktivieren ein verwaltetes virtuelles Netzwerk.

1. Navigieren Sie im ADF-Portal zu **Manage Hub** (Hub verwalten), und klicken Sie auf **Neu**, um eine neue Azure Integration Runtime-Instanz zu erstellen.
   ![Erstellen einer neuen Azure Integration Runtime-Instanz](./media/tutorial-copy-data-portal-private/create-new-azure-ir.png)
2. Wählen Sie die Erstellung einer **Azure** Integration Runtime-Instanz.
   ![Neue Azure Integration Runtime-Instanz](./media/tutorial-copy-data-portal-private/azure-ir.png)
3. Aktivieren Sie das **virtuelle Netzwerk**.
   ![Neue Azure Integration Runtime-Instanz](./media/tutorial-copy-data-portal-private/enable-managed-vnet.png)
4. Klicken Sie auf **Erstellen**.

## <a name="create-a-pipeline-with-a-data-flow-activity"></a>Erstellen einer Pipeline mit einer Datenflussaktivität

In diesem Schritt erstellen Sie eine Pipeline, die eine Datenflussaktivität enthält.

1. Wählen Sie auf der Seite **Erste Schritte** die Option **Pipeline erstellen** aus.

   ![Erstellen der Pipeline](./media/doc-common-process/get-started-page.png)

1. Geben Sie im Bereich **Eigenschaften** für die Pipeline als **Name** der Pipeline **TransformMovies** ein.
1. Setzen Sie auf der oberen Leiste der Factory den Schieberegler **Datenfluss debuggen** auf „Ein“. Der Debugmodus ermöglicht das interaktive Testen von Transformationslogik mit einem aktiven Spark-Cluster. Die Aufwärmphase von Datenflussclustern dauert 5 bis 7 Minuten. Den Benutzern wird empfohlen, zuerst den Debugmodus zu aktivieren, wenn sie die Entwicklung eines Datenflusses planen. Weitere Informationen finden Sie unter [Debugmodus](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-debug-mode).

    ![Datenfluss debuggen](media/tutorial-data-flow-private/dataflow-debug.png)
1. Erweitern Sie im Bereich **Aktivitäten** das Accordion-Element **Verschieben und transformieren**. Ziehen Sie die **Datenfluss**-Aktivität per Drag & Drop aus dem Bereich auf die Pipelinecanvas.

1. Wählen Sie im Popupfenster **Datenfluss hinzufügen** die Option **Neuen Datenfluss erstellen** und dann **Zuordnungsdatenfluss** aus. Klicken Sie nach Abschluss des Vorgangs auf **OK**.

    ![Zuordnungsdatenfluss](media/tutorial-data-flow-private/mapping-dataflow.png)

1. Geben Sie Ihrem Datenfluss im Bereich „Eigenschaften“ den Namen **TransformMovies**.

## <a name="build-transformation-logic-in-the-data-flow-canvas"></a>Erstellen von Transformationslogik auf der Datenflusscanvas

Sobald Sie den Datenfluss erstellen, werden Sie automatisch zur Datenflusscanvas geführt. In diesem Schritt erstellen Sie einen Datenfluss, der die Datei „moviesDB.csv“ aus dem ADLS-Speicher abruft und die durchschnittliche Bewertung von Komödien von 1910 bis 2000 aggregiert. Anschließend schreiben Sie diese Datei zurück in den ADLS-Speicher.

### <a name="add-the-source-transformation"></a>Hinzufügen der Quelltransformation

In diesem Schritt richten Sie Azure Data Lake Storage Gen2 als Quelle ein.

1. Fügen Sie auf der Datenflusscanvas eine Quelle hinzu, indem Sie auf das Feld **Quelle hinzufügen** klicken.

1. Geben Sie der Quelle den Namen **MoviesDB**. Klicken Sie auf **Neu**, um ein neues Quelldataset zu erstellen.

1. Wählen Sie **Azure Data Lake Storage Gen2** aus. Klicken Sie auf „Weiter“.

1. Wählen Sie **DelimitedText** aus. Klicken Sie auf „Weiter“.

1. Geben Sie dem Dataset den Namen **MoviesDB**. Wählen Sie in der Dropdownliste für den verknüpften Dienst die Option **Neu** aus.

1. Geben Sie im Erstellungsbildschirm für einen verknüpften Dienst dem verknüpften ADLS Gen2-Dienst den Namen **ADLSGen2**, und geben Sie die Authentifizierungsmethode an. Dann geben Sie Ihre Verbindungsanmeldeinformationen ein. In diesem Tutorial wird der Kontoschlüssel zum Herstellen einer Verbindung mit dem Speicherkonto verwendet. 

1. Aktivieren Sie unbedingt **Interactive Authoring** (Interaktive Dokumenterstellung). Die Aktivierung kann ungefähr 1 Minute dauern.

    ![Interaktive Erstellung](./media/tutorial-data-flow-private/interactive-authoring.png)

1. Wählen Sie **Verbindung testen** aus. Dann sollte ein Verbindungsfehler auftreten, weil – ohne die Erstellung und Genehmigung eines privaten Endpunkts – auf das Speicherkonto nicht zugegriffen werden kann. In der Fehlermeldung sollte ein Link zum Erstellen eines **privaten Endpunkts** angezeigt werden, dem Sie folgen können, um einen verwalteten privaten Endpunkt zu erstellen. *Alternativ dazu können Sie direkt zur Registerkarte „Verwalten“ navigieren und die Anweisungen in [diesem Abschnitt](#create-a-managed-private-endpoint) befolgen, um einen verwalteten privaten Endpunkt zu erstellen.*

1. Lassen Sie das Dialogfeld geöffnet, und navigieren Sie dann zu dem Storage-Konto, das Sie oben ausgewählt haben.

1. Befolgen Sie die Anweisungen in [diesem Abschnitt](#approval-of-a-private-link-in-storage-account), um die private Verbindung zu genehmigen.

1. Gehen Sie zurück zum Dialogfeld. Wählen Sie erneut **Verbindung testen** und anschließend **Erstellen** aus, um den verknüpften Dienst bereitzustellen.

1. Wenn Sie sich wieder im Erstellungsbildschirm für das Dataset befinden, geben Sie den Speicherort Ihrer Datei unter dem Feld **Dateipfad** ein. In diesem Tutorial befindet sich die Datei „moviesDB.csv“ im Container „sample-data“. Da die Datei Header enthält, aktivieren Sie das Kontrollkästchen **Erste Zeile als Header**. Wählen Sie **Aus Verbindung/Speicher** aus, um das Headerschema direkt aus der Datei in den Speicher zu importieren. Klicken Sie anschließend auf „OK“.

    ![Quellpfad](media/tutorial-data-flow-private/source-file-path.png)

1. Wenn Ihr Debugcluster gestartet wurde, wechseln Sie zur Registerkarte **Datenvorschau** der Quelltransformation, und klicken Sie auf **Aktualisieren**, um eine Momentaufnahme der Daten zu erhalten. Mithilfe der Datenvorschau können Sie überprüfen, ob die Transformation ordnungsgemäß konfiguriert ist.

    ![Datenvorschau](media/tutorial-data-flow-private/data-preview.png)

#### <a name="create-a-managed-private-endpoint"></a>Erstellen eines verwalteten privaten Endpunkts

Sollten Sie beim Testen der oben genannten Verbindung nicht auf den Link geklickt haben, folgen Sie dem nachstehenden Pfad. Jetzt müssen Sie einen verwalteten privaten Endpunkt erstellen, den Sie mit dem zuvor erstellten verknüpften Dienst verbinden werden.

1. Navigieren Sie zur Registerkarte „Verwalten“.
> [!NOTE]
> Die Registerkarte „Verwalten“ ist möglicherweise nicht für alle Data Factory-Instanzen verfügbar. Wird sie nicht angezeigt, können Sie wie folgt auf private Endpunkte zugreifen: Registerkarte **Ersteller** > **Verbindungen** > **Privater Endpunkt**.
1. Navigieren Sie zum Abschnitt „Verwaltete private Endpunkte“.
1. Wählen Sie unter „Verwaltete private Endpunkte“ die Option **+ Neu** aus.

    ![Neuer verwalteter privater Endpunkt](./media/tutorial-data-flow-private/new-managed-private-endpoint.png) 

1. Wählen Sie in der Liste die Kachel „Azure Data Lake Storage Gen2“ und dann **Weiter** aus.
1. Geben Sie den Namen des zuvor erstellten Speicherkontos ein.
1. Klicken Sie auf **Erstellen**.
1. Nach einigen Sekunden Wartezeit sollten Sie informiert werden, dass für den erstellten privaten Link eine Genehmigung erforderlich ist.
1. Wählen Sie den zuvor erstellten privaten Endpunkt aus. Ein Link wird angezeigt, über den Sie den privaten Endpunkt auf Storage-Kontoebene genehmigen können.

    ![Verwalten eines privaten Endpunkts](./media/tutorial-data-flow-private/manage-private-endpoint.png) 

#### <a name="approval-of-a-private-link-in-storage-account"></a>Genehmigung einer privaten Verbindung im Speicherkonto

1. Navigieren Sie im Storage-Konto im Abschnitt „Einstellungen“ zu **Verbindungen mit privatem Endpunkt**.

1. Aktivieren Sie den privaten Endpunkt, den Sie oben erstellt haben, und wählen Sie **Genehmigen** aus.

    ![Genehmigen des privaten Endpunkts](./media/tutorial-data-flow-private/approve-private-endpoint.png)

1. Fügen Sie eine Beschreibung hinzu, und klicken Sie auf **ja**.
1. Gehen Sie zurück zum Abschnitt **Verwaltete private Endpunkte** der Registerkarte **Verwalten** in Azure Data Factory.
1. Es sollte ungefähr 1 Minute dauern, bis die Genehmigung für Ihren privaten Endpunkt angezeigt wird.

### <a name="add-the-filter-transformation"></a>Hinzufügen der Filtertransformation

1. Klicken Sie auf der Datenflusscanvas neben dem Quellknoten auf das Pluszeichen, um eine neue Transformation hinzuzufügen. Als erste Transformation fügen Sie einen **Filter** hinzu.

    ![Filter hinzufügen](media/tutorial-data-flow-private/add-filter.png)
1. Geben Sie der Filtertransformation den Namen **FilterYears**. Klicken Sie auf das Ausdrucksfeld neben **Filtern nach**, um den Ausdrucks-Generator zu öffnen. Hier geben Sie dann die Filterbedingung an.

    ![Filtern nach Jahren](media/tutorial-data-flow-private/filter-years.png)
1. Mit dem Datenfluss-Ausdrucks-Generator können Sie Ausdrücke interaktiv erstellen, die dann in verschiedenen Transformationen verwendet werden können. Ausdrücke können integrierte Funktionen, Spalten aus dem Eingabeschema und benutzerdefinierte Parameter enthalten. Weitere Informationen zum Erstellen von Ausdrücken finden Sie unter [Mapping Data Flow: Ausdrucks-Generator](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-expression-builder).

    * In diesem Tutorial möchten Sie Filme des Genres „Komödie“ filtern, die zwischen den Jahren 1910 und 2000 entstanden sind. Da die Jahresangabe derzeit eine Zeichenfolge ist, müssen Sie sie mithilfe der Funktion ```toInteger()``` in eine ganze Zahl konvertieren. Verwenden Sie die Operatoren für größer als oder gleich (>=) und kleiner als oder gleich (<=) für einen Vergleich mit den Literalwerten für die Jahre 1910 und 2000. Verbinden Sie diese Ausdrücke mit dem Und-Operator (&&). Der Ausdruck sieht wie folgt aus:

        ```toInteger(year) >= 1910 && toInteger(year) <= 2000```

    * Um zu ermitteln, welche Filme Komödien sind, können Sie mithilfe der Funktion ```rlike()``` nach dem Muster „Comedy“ in der Spalte „genres“ suchen. Verbinden Sie den rlike-Ausdruck mit dem Jahresvergleich, um Folgendes zu erhalten:

        ```toInteger(year) >= 1910 && toInteger(year) <= 2000 && rlike(genres, 'Comedy')```

    * Wenn ein Debugcluster aktiv ist, können Sie die Logik überprüfen. Klicken Sie dazu auf **Aktualisieren**, um die Ausdrucksausgabe im Vergleich zu den verwendeten Eingaben anzuzeigen. Es gibt mehrere Möglichkeiten, wie Sie diese Logik mithilfe der Ausdruckssprache für Datenflüsse erzielen können.

        ![Filtern von Ausdrücken](media/tutorial-data-flow-private/filter-expression.png)

    * Sobald Sie den Ausdruck fertiggestellt haben, klicken Sie auf **Speichern und beenden**.

1. Rufen Sie eine **Datenvorschau** ab, um zu überprüfen, ob der Filter ordnungsgemäß funktioniert.

    ![Vorschau auf Filterdaten](media/tutorial-data-flow-private/filter-data.png)

### <a name="add-the-aggregate-transformation"></a>Hinzufügen der Transformation für das Aggregieren

1. Als nächste Transformation fügen Sie eine **Aggregat**-Transformation unter **Schemamodifizierer** hinzu.

    ![Hinzufügen des Aggregats](media/tutorial-data-flow-private/add-aggregate.png)
1. Geben Sie der Aggregattransformation den Namen **AggregateComedyRatings**. Wählen Sie auf der Registerkarte **Gruppieren nach** in der Dropdownliste die Option **year** aus, um die Aggregationen nach dem Jahr zu gruppieren, in dem der Film entstanden ist.

    ![Aggregatgruppe](media/tutorial-data-flow-private/group-by-year.png)
1. Wechseln Sie zur Registerkarte **Aggregate**. Geben Sie im linken Textfeld der Aggregatspalte den Namen **AverageComedyRating**. Klicken Sie auf das rechte Ausdrucksfeld, um den Aggregatausdruck über den Ausdrucks-Generator einzugeben.

    ![Name der Aggregatspalte](media/tutorial-data-flow-private/name-column.png)
1. Verwenden Sie die Aggregatfunktion ```avg()```, um den Durchschnitt der Spalte **Rating** zu erhalten. Da **Rating** eine Zeichenfolge ist und ```avg()``` eine numerische Eingabe benötigt, muss der Wert über die Funktion ```toInteger()``` in eine Zahl konvertiert werden. Dieser Ausdruck sieht wie folgt aus:

    ```avg(toInteger(Rating))```

    Klicken Sie anschließend auf **Speichern und beenden**.

    ![Speichern des Aggregats](media/tutorial-data-flow-private/save-aggregate.png)
1. Wechseln Sie zur Registerkarte **Datenvorschau**, um die Transformationsausgabe anzuzeigen. Es sind nur zwei Spalten vorhanden: **year** und **AverageComedyRating**.

### <a name="add-the-sink-transformation"></a>Hinzufügen der Senkentransformation

1. Als Nächstes fügen Sie eine Transformation vom Typ **Senke** unter **Ziel** hinzu.

    ![Hinzufügen der Senke](media/tutorial-data-flow-private/add-sink.png)
1. Geben Sie der Senke den Namen **Sink**. Klicken Sie auf **Neu**, um das Senkendataset zu erstellen.

    ![Erstellen der Senke](media/tutorial-data-flow-private/create-sink.png)
1. Wählen Sie auf der Seite „Neues Dataset“ die Option **Azure Data Lake Storage Gen2** aus. Klicken Sie auf „Weiter“.

1. Wählen Sie auf der Seite „Format auswählen“ die Option **DelimitedText** aus. Klicken Sie auf „Weiter“.

1. Geben Sie dem Senkendataset den Namen **MoviesSink**. Wählen Sie als verknüpften Dienst denselben verknüpften ADLSGen2-Dienst aus, den Sie für die Quelltransformation erstellt haben. Geben Sie einen Ausgabeordner ein, in den die Daten geschrieben werden sollen. In diesem Tutorial wird in den Ordner „output“ im Container „sample-data“ geschrieben. Der Ordner muss nicht vorab vorhanden sein und kann dynamisch erstellt werden. Aktivieren Sie das Kontrollkästchen **Erste Zeile als Header**, und wählen Sie für **Schema importieren** die Option **Kein** aus. Klicken Sie auf OK.

    ![Senkenpfad](media/tutorial-data-flow-private/sink-file-path.png)

Sie haben nun die Erstellung des Datenflusses beendet. Jetzt können Sie ihn in ihrer Pipeline ausführen.

## <a name="running-and-monitoring-the-data-flow"></a>Ausführen und Überwachen des Datenflusses

Sie können eine Pipeline vor der Veröffentlichung debuggen. In diesem Schritt lösen Sie eine Debugausführung der Datenflusspipeline aus. Während bei der Datenvorschau keine Daten geschrieben werden, werden bei einer Debugausführung Daten in das Senkenziel geschrieben.

1. Wechseln Sie zur Pipelinecanvas. Klicken Sie auf **Debuggen**, um eine Debugausführung auszulösen.

1. Für das Debuggen der Pipeline von Datenflussaktivitäten wird der aktive Debugcluster verwendet, doch dauert die Initialisierung dennoch mindestens eine Minute. Sie können den Fortschritt über die Registerkarte **Ausgabe** verfolgen. Wenn die Ausführung erfolgreich ist, klicken Sie auf das Brillensymbol, um Ausführungsdetails anzuzeigen.

1. Auf der Detailseite werden die Anzahl der Zeilen und die Zeit für die einzelnen Transformationsschritte angezeigt.

    ![Überwachen der Ausführung](media/tutorial-data-flow-private/run-details.png)
1. Klicken Sie auf eine Transformation, um ausführliche Informationen über die Spalten und die Partitionierung der Daten zu erhalten.

Wenn Sie dieses Tutorial richtig durchgeführt haben, sollten 83 Zeilen und 2 Spalten in den Senkenordner geschrieben worden sein. Sie können sich von der Richtigkeit der Daten überzeugen, indem Sie den Blobspeicher überprüfen.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie mithilfe der Azure Data Factory-Benutzeroberfläche eine Pipeline erstellt, mit der Daten **aus einer Azure Data Lake Storage (ADLS) Gen2-Quelle in eine ADLS Gen2-Senke (die jeweils nur den Zugriff auf ausgewählte Netzwerke zulassen)** kopiert und mithilfe eines Zuordnungsdatenflusses in ein [verwaltetes virtuelles Azure Data Factory-Netzwerk](managed-virtual-network-private-endpoint.md) transformiert werden.
