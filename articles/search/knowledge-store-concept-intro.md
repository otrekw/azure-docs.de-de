---
title: Einführung in den Wissensspeicher (Vorschauversion)
titleSuffix: Azure Cognitive Search
description: Senden Sie angereicherte Dokumente an Azure Storage, um sie in Azure Cognitive Search sowie in anderen Anwendungen anzeigen, umstrukturieren und nutzen zu können. Dieses Feature befindet sich in der Phase der öffentlichen Vorschau.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/05/2020
ms.openlocfilehash: 20819bc6ec091eddf5d65b1c0d7aa57c821b2fc1
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858802"
---
# <a name="introduction-to-knowledge-stores-in-azure-cognitive-search"></a>Einführung in Wissensspeicher in Azure Cognitive Search

> [!IMPORTANT] 
> „Wissensspeicher“ ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Previewfunktionen werden von der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

Der Wissensspeicher ist ein Feature von Azure Cognitive Search, das Ausgaben aus einer [KI-Anreicherungspipeline](cognitive-search-concept-intro.md) zur unabhängigen Analyse oder Downstreamverarbeitung speichert. Ein *angereichertes Dokument* ist eine Pipelineausgabe, die auf der Grundlage von Inhalten erstellt wurde, die mithilfe von KI-Prozessen extrahiert, strukturiert und analysiert wurden. In einer KI-Standardpipeline sind angereicherte Dokumente kurzlebig: Sie werden nur während der Indizierung verwendet und anschließend verworfen. Mit dem Wissensspeicher bleiben erweiterte Dokumente erhalten. 

Wenn Sie in der Vergangenheit kognitive Fähigkeiten genutzt haben, wissen Sie bereits, dass Dokumente mithilfe von *Skillsets* auf verschiedene Weisen angereichert werden. Bei der Ausgabe kann es sich um einen Suchindex oder (neu in dieser Vorschauversion) um Projektionen in einem Wissensspeicher handeln. Die beiden Ausgaben in Form des Suchindexes und Wissensspeichers werden durch dieselbe Pipeline erzeugt. Sie werden von denselben Eingaben abgeleitet, resultieren jedoch in einer Ausgabe, die auf sehr unterschiedliche Weisen strukturiert, gespeichert und verwendet wird.

Physisch betrachtet handelt es sich bei einem Wissensspeicher um [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview) (Azure Table Storage, Azure Blob Storage oder beides). Jedes Tool und jeder Prozess, das bzw. der eine Verbindung mit Azure Storage herstellen kann, kann die Inhalte eines Wissensspeichers nutzen.


> [!VIDEO https://www.youtube.com/embed/XWzLBP8iWqg?version=3&start=235&end=426]


![Wissensspeicher im Pipelinediagramm](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Wissensspeicher im Pipelinediagramm")

## <a name="benefits-of-knowledge-store"></a>Vorteile von Wissensspeichern

Ein Wissensspeicher enthält die Struktur, den Kontext und den eigentlichen Inhalt. Diese Informationen werden aus unstrukturierten und teilweise strukturierten Datendateien wie Blobs und analysierten Bilddateien oder sogar aus strukturierten Daten gewonnen, die umgeformt wurden. In einer [ausführlichen exemplarischen Vorgehensweise](knowledge-store-create-rest.md) können Sie genau sehen, wie ein komplexes JSON-Dokument in Unterstrukturen partitioniert, in neue Strukturen zusammengesetzt und für nachfolgende Prozesse wie Machine Learning- und Data Science-Workloads verfügbar gemacht wird.

Auch wenn es nützlich ist, zu sehen, was eine KI-Anreicherungspipeline erzeugen kann, liegt das eigentliche Potenzial von Wissensspeichern in der Möglichkeit, Daten neu zu strukturieren. Sie können mit einem grundlegenden Skillset beginnen und dieses wiederholt durchlaufen, um weitere Strukturebenen hinzuzufügen. Diese können dann zu neuen Strukturen kombiniert werden, die sowohl in Azure Cognitive Search als auch in anderen Apps verwendet werden können.

Wissensspeicher bieten u. a. folgende Vorteile:

+ Nutzen Sie angereicherte Dokumente auch in anderen [Analyse- und Berichtstools](#tools-and-apps) als Search. Power BI mit Power Query ist eine hervorragende Wahl, allerdings sind auch alle Tools oder Apps, die eine Verbindung mit Azure Storage herstellen können, in der Lage, Daten aus einem von Ihnen erstellten Wissensspeicher abzurufen.

+ Optimieren Sie eine KI-basierte Indizierungspipeline während der Debuggingschritte und der Definition von Qualifikationen. Ein Wissensspeicher zeigt das Produkt einer Qualifikationsgruppendefinition in einer KI-Indizierungspipeline. Sie können diese Ergebnisse verwenden, um eine bessere Qualifikationsgruppe zu entwerfen, da Sie genau sehen können, wie die Anreicherungen aussehen. Verwenden Sie den [Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) in Azure Storage, um die Inhalte eines Wissensspeichers anzuzeigen.

+ Formen Sie die Daten um. Die Umgestaltung ist in Qualifikationsgruppen codiert, das Wichtigste ist aber, dass diese Funktion nun von einer Qualifikationsgruppe bereitgestellt werden kann. Die [Qualifikation „Shaper“](cognitive-search-skill-shaper.md) in Azure Cognitive Search wurde erweitert, um genau diese Aufgabe zu ermöglichen. Die Umstrukturierung ermöglicht Ihnen die Definition einer Projektion für die beabsichtigte Verwendung der Daten unter Beibehaltung der Beziehungen.

> [!Note]
> Neu bei KI-Anreicherung und kognitiven Qualifikationen? Azure Cognitive Search arbeitet mit den Bild- und Sprachfeatures von Cognitive Services zusammen, um Quelldaten unter Verwendung der optischen Zeichenerkennung (Optical Character Recognition, OCR) für Bilddateien, der Entitätserkennung, der Extraktion von Schlüsselbegriffen aus Textdateien u. v. m. zu extrahieren und anzureichern. Weitere Informationen finden Sie unter [Was ist die „kognitive Suche“ in Azure Search?](cognitive-search-concept-intro.md).

## <a name="physical-storage"></a>Physischer Speicher


> [!VIDEO https://www.youtube.com/embed/XWzLBP8iWqg?version=3&start=455&end=542]


Physisch wird ein Wissensspeicher durch das `projections`-Element einer `knowledgeStore`-Definition in einem Skillset ausgedrückt. Durch die Projektion wird eine Struktur der Ausgabe definiert, sodass sie Ihrer beabsichtigten Nutzung entspricht.

Projektionen können als Tabellen, Objekte oder Dateien formuliert werden.

```json
"knowledgeStore": { 
    "storageConnectionString": "<YOUR-AZURE-STORAGE-ACCOUNT-CONNECTION-STRING>", 
    "projections": [ 
        { 
            "tables": [ ], 
            "objects": [ ], 
            "files": [ ]
        },
                { 
            "tables": [ ], 
            "objects": [ ], 
            "files": [ ]
        }
```

Der Typ der Projektion, den Sie in dieser Struktur angeben, bestimmt den vom Wissensspeicher verwendeten Speichertyp.

+ Wenn Sie `tables` definieren, wird Table Storage verwendet. Definieren Sie eine Tabellenprojektion, wenn Sie tabellarische Berichtsstrukturen für Eingaben in Analysetools benötigen oder als Datenrahmen in andere Datenspeicher exportieren müssen. Sie können mehrere `tables` angeben, um eine Teilmenge oder einen Querschnitt angereicherter Dokumente zu erhalten. Tabellenbeziehungen innerhalb derselben Projektionsgruppe bleiben erhalten, sodass Sie mit allen arbeiten können.

+ Wenn Sie `objects` oder `files` definieren, wird Blob Storage verwendet. Die physische Darstellung eines `object` ist eine hierarchische JSON-Struktur, die ein angereichertes Dokument darstellt. Bei einer `file` handelt es sich um ein aus einem Dokument extrahiertes Bild, das intakt in Blob Storage übertragen wurde.

Ein einzelnes Projektionsobjekt enthält einen Satz von `tables`, `objects` und `files`. In vielen Fällen reicht es aus, eine Projektion zu erstellen. 

Es ist jedoch möglich, mehrere Sätze von `table`-`object`-`file`-Projektionen zu erstellen. Dies empfiehlt sich, wenn Sie unterschiedliche Datenbeziehungen herstellen möchten. Zwischen den Daten innerhalb eines Satzes bestehen Beziehungen – vorausgesetzt, diese Beziehungen existieren und können erkannt werden. Wenn Sie zusätzliche Sätze erstellen, besteht zwischen den Dokumenten in den einzelnen Gruppen niemals eine Beziehung. Beispiel für die Verwendung mehrerer Projektionsgruppen: Sie möchten dieselbe Datenprojektion für Ihr Onlinesystem verwenden, und die Daten müssen auf eine bestimmte Art und Weise dargestellt werden. Außerdem möchten Sie dieselbe Datenprojektion in einer Data Science-Pipeline verwenden, wobei die Daten auf andere Weise dargestellt werden.

## <a name="requirements"></a>Requirements (Anforderungen) 

[Azure Storage](https://docs.microsoft.com/azure/storage/) ist erforderlich und ermöglicht die physische Speicherung. Sie können Blob Storage, Table Storage oder beides verwenden. Blob Storage wird üblicherweise für intakte angereicherte Dokumente verwendet, wenn die Ausgabe an Downstreamprozesse weitergeleitet wird. Table Storage wird für Segmente angereicherter Dokumente verwendet, die häufig für Analysen und Berichte verwendet werden.

Ein [Skillset-](cognitive-search-working-with-skillsets.md) ist erforderlich. Es enthält die Definition von `knowledgeStore` und bestimmt die Struktur und Zusammensetzung eines angereicherten Dokuments. Es ist nicht möglich, einen Wissensspeicher mit einem leeren Skillset zu erstellen. Ein Skillset muss mindestens eine Qualifikation enthalten.

Ein [Indexer](search-indexer-overview.md) ist erforderlich. Ein Skillset wird von einem Indexer aufgerufen, der die Ausführung steuert. Indexer verfügen über eigene Anforderungen und Attribute. Einige dieser Attribute haben direkten Einfluss auf einen Wissensspeicher:

+ Indexer erfordern eine [unterstützte Azure-Datenquelle](search-indexer-overview.md#supported-data-sources) (für die Pipeline, von der letztendlich der Wissensspeicher erstellt wird, werden zunächst Daten aus einer unterstützten Quelle in Azure gepullt). 

+ Indexer erfordern einen Suchindex. Für einen Indexer muss selbst dann ein Indexschema bereitgestellt werden, wenn Sie es niemals verwenden werden. Ein minimaler Index verfügt über ein Zeichenfolgenfeld, das als Schlüssel bezeichnet wird.

+ Indexer stellen optionale Feldzuordnungen bereit, die verwendet werden, um ein Quellfeld mit einem Zielfeld zu verknüpfen. Wenn eine Standardfeldzuordnung geändert werden muss (um einen anderen Namen oder Typ zu verwenden), können Sie eine [Feldzuordnung](search-indexer-field-mappings.md) innerhalb eines Indexers erstellen. Als Ziel für die Ausgabe des Wissensspeichers kann ein Feld in einem Blobobjekt oder einer Blobtabelle verwendet werden.

+ Indexer verfügen über Zeitpläne und andere Eigenschaften, beispielsweise über Mechanismen zur Erkennung von Änderungen, die von verschiedenen Datenquellen bereitgestellt werden. Diese können auch auf einen Wissensspeicher angewendet werden. Beispielsweise können Sie in regelmäßigen Abständen eine Anreicherung [planen](search-howto-schedule-indexers.md), um die Inhalte zu aktualisieren. 

## <a name="how-to-create-a-knowledge-store"></a>Erstellen eines Wissensspeichers

Verwenden Sie das Portal oder die Vorschauversion der REST-API (`api-version=2019-05-06-Preview`), um einen Wissensspeicher zu erstellen.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Der **Datenimport**-Assistent bietet Optionen zum Erstellen eines Wissensspeichers. Machen Sie erste Erfahrungen, indem Sie [in vier Schritten Ihren ersten Wissensspeicher erstellen](knowledge-store-connect-power-bi.md).

1. Wählen Sie eine unterstützte Datenquelle aus.

1. Legen Sie die Anreicherung fest: Fügen Sie eine Ressource an, wählen Sie Qualifikationen aus, und geben Sie einen Wissensspeicher an. 

1. Erstellen Sie ein Indexschema. Der Assistent benötigt das Schema und kann eines für Sie ableiten.

1. Führen Sie den Assistenten aus. Die Extraktion, Anreicherung und Speicherung erfolgen in diesem letzten Schritt.

### <a name="use-create-skillset-and-the-preview-rest-api"></a>Verwenden von „Create Skillset“ und der Vorschauversion der REST-API

`knowledgeStore` wird innerhalb eines [Skillsets](cognitive-search-working-with-skillsets.md) definiert, das wiederum von einem [Indexer](search-indexer-overview.md) aufgerufen wird. Während der Anreicherung wird von Azure Cognitive Search ein Bereich in Ihrem Azure Storage-Konto erstellt. Anschließend werden die angereicherten Dokumente je nach Konfiguration als Blobs oder in Tabellen projiziert.

Die Vorschauversion der REST-API ist derzeit der einzige Mechanismus, mit dem Sie einen Wissensspeicher programmgesteuert erstellen können. Um sich auf einfache Weise damit vertraut zu machen, erstellen Sie Ihren [ersten Wissensspeicher mithilfe von Postman und der REST-API](knowledge-store-create-rest.md).

Referenzinformationen zu dieser Previewfunktion finden Sie im Abschnitt [API-Referenz](#kstore-rest-api) in diesem Artikel. 

<a name="tools-and-apps"></a>

## <a name="how-to-connect-with-tools-and-apps"></a>Herstellen einer Verbindung mit Tools und Apps

Wenn die Anreicherungen in Storage gespeichert sind, können Sie beliebige Tools oder Technologien mit einer Verbindung mit Azure Blob Storage oder Azure Table Storage verwenden, um die Inhalte zu untersuchen, zu analysieren oder zu nutzen. Die folgende Liste gibt einen ersten Einblick:

+ Mit [Storage-Explorer](knowledge-store-view-storage-explorer.md) können Sie die Struktur und den Inhalt angereicherter Dokumente anzeigen. Nutzen Sie dies als Ihr Basistool zum Anzeigen des Inhalts von Wissensspeichern.

+ Nutzen Sie [Power BI](knowledge-store-connect-power-bi.md) für die Berichterstellung und Analyse. 

+ Verwenden Sie [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) für die weiterführende Bearbeitung.

<a name="kstore-rest-api"></a>

## <a name="api-reference"></a>API-Referenz

Die REST-API-Version `2019-05-06-Preview` stellt den Wissensspeicher mithilfe zusätzlicher Definitionen für Skillsets bereit. Weitere Informationen zum Abrufen der APIs finden Sie neben der Referenz auch unter [Erstellen eines Wissensspeichers mithilfe von Postman](knowledge-store-create-rest.md).

+ [Erstellen einer Qualifikationsgruppe (api-version=2019-05-06-Preview)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/create-skillset) 
+ [Aktualisieren eines Skillsets (api-version=2019-05-06-Preview)](https://docs.microsoft.com/rest/api/searchservice/2019-05-06-preview/update-skillset) 


## <a name="next-steps"></a>Nächste Schritte

Ein Wissensspeicher ermöglicht die Aufbewahrung angereicherter Dokumente, was bei der Entwicklung eines Skillsets hilfreich ist, sowie die Erstellung neuer Strukturen und Inhalte zur Nutzung durch beliebige Clientanwendungen, die auf ein Azure Storage-Konto zugreifen können.

Am einfachsten lassen sich angereicherte Dokumente über das [Portal](knowledge-store-create-portal.md) erstellen. Sie können aber auch Postman und die REST-API verwenden. Dies empfiehlt sich, wenn Sie nachvollziehen möchten, wie Objekte erstellt und referenziert werden.

> [!div class="nextstepaction"]
> [Erstellen eines Wissensspeichers mithilfe von Postman und REST](knowledge-store-create-rest.md)

Hier finden Sie weitere Informationen zu Projektionen, den Funktionen und der Art und Weise, wie Sie [diese in einem Skillset definieren](knowledge-store-projection-overview.md).

> [!div class="nextstepaction"]
> [Projektionen in einem Wissensspeicher](knowledge-store-projection-overview.md)

Für ein Tutorial zu erweiterten Projektionskonzepten wie Aufteilen in Slices, Inlinestrukturierung und Beziehungen behandelt beginnen Sie mit [Definieren von Projektionen in einem Wissensspeicher](knowledge-store-projections-examples.md).

> [!div class="nextstepaction"]
> [Definieren von Projektionen in einem Wissensspeicher](knowledge-store-projections-examples.md)