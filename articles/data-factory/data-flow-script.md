---
title: Zuordnungsdatenflussskript
description: Übersicht über die CodeBehind-Sprache für das Datenflussskript von Data Factory
author: kromerm
ms.author: nimoolen
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/10/2019
ms.openlocfilehash: d861a4355158dfe18ac3aa40a7f98dc11ebda90b
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930252"
---
# <a name="data-flow-script-dfs"></a>Datenflussskript (DFS)

Beim Datenflussskript (DFS) handelt es sich um die zugrunde liegenden Metadaten (ähnlich wie eine Programmiersprache), die zur Ausführung der in einem Zuordnungsdatenfluss enthaltenen Transformationen dienen. Jede Transformation wird durch eine Reihe von Eigenschaften dargestellt, die die erforderlichen Informationen zur ordnungsgemäßen Ausführung des Auftrags liefern. Durch Klicken auf die Schaltfläche „Skript“ im oberen Menüband der Browser-Benutzeroberfläche kann das Skript aus ADF heraus angezeigt und bearbeitet werden.

![Schaltfläche „Skript“](media/data-flow/scriptbutton.png "Schaltfläche „Skript“")

So weist beispielsweise `allowSchemaDrift: true,` in einer Quelltransformation den Dienst an, alle Spalten aus dem Quelldataset in den Datenfluss einzubeziehen – sogar, wenn sie in der Schemaprojektion nicht enthalten sind.

## <a name="use-cases"></a>Anwendungsfälle
Das DFS wird von der Benutzeroberfläche automatisch erstellt. Wenn Sie das Skript anzeigen und anpassen möchten, können Sie auf die Schaltfläche „Skript“ klicken. Sie können Skripts auch außerhalb der ADF-Benutzeroberfläche generieren und dann an das PowerShell-Cmdlet übergeben. Beim Debuggen komplexer Datenflüsse finden Sie es vielleicht einfacher, den Script-CodeBehind zu überprüfen, statt die Darstellung der Diagramm-Benutzeroberfläche für Ihre Datenflüsse überprüfen zu müssen.

Hier sind ein paar Beispiele für Anwendungsfälle:
- Programmgesteuerte Erstellung vieler Datenflüsse, die ziemlich ähnlich sind, d.h., „Ausmerzen“ von Datenflüssen.
- Komplexe Ausdrücke, die in der Benutzeroberfläche schwierig zu verwalten sind oder zu Problemen bei der Überprüfung führen.
- Debuggen von verschiedenen Fehlern, die während der Ausführung zurückgegeben werden, und ein besseres Verständnis dafür.

Wenn Sie ein Datenflussskript zur Verwendung von PowerShell oder einer API erstellen, müssen Sie den formatierten Text in eine einzige Zeile reduzieren. Sie können Tabstopps und Zeilenumbrüche als Escapezeichen beibehalten. Der Text muss aber so formatiert werden, dass er in eine JSON-Eigenschaft passt. In der Benutzeroberfläche des Skript-Editors gibt es ganz unten eine Schaltfläche, über die das Skript als eine einzige Zeile formatiert wird.

![Schaltfläche „Kopieren“](media/data-flow/copybutton.png "Schaltfläche „Kopieren“")

## <a name="how-to-add-transforms"></a>Hinzufügen von Transformationen
Zum Hinzufügen von Transformationen sind drei grundlegende Schritte erforderlich: Hinzufügen der Haupttransformationsdaten, Umleiten des Eingabestreams und Umleiten des Ausgabestreams. Dies lässt sich am einfachsten in einem Beispiel zeigen.
Angenommen, Sie beginnen mit einer einfachen Quelle zum Senken des Datenflusses wie hier:

```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Wenn Sie beschließen, eine abgeleitete Transformation hinzuzufügen, müssen Sie zuerst den Haupttransformationstext erstellen, der einen einfachen Ausdruck zum Hinzufügen der neuen Großbuchstabenspalte `upperCaseTitle` enthält:
```
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
```

Anschließend fügen Sie dem vorhandenen DFS die Transformation hinzu:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Dann leiten Sie den eingehenden Stream um, indem Sie identifizieren, nach welcher Transformation die neue Transformation erfolgen soll (in diesem Fall, `source1`), und den Namen des Streams in die neue Transformation kopieren:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Und schließlich identifizieren Sie die Transformation, die nach dieser neuen Transformation erfolgen soll, und ersetzen deren Eingabestream (in diesem Fall, `sink1`) durch den Namen des Ausgabestreams für die neue Transformation:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
deriveTransformationName sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="dfs-fundamentals"></a>DFS-Grundlagen
Der DFS besteht aus einer Reihe von verbundenen Transformationen, einschließlich Quellen, Senken und verschiedenen anderen, die neue Spalten hinzufügen, Daten filtern, Daten verknüpfen und vieles mehr erledigen können. Normalerweise startet das Skript mit einer oder mehreren Quellen, gefolgt von vielen Transformationen, und es endet mit einer oder mehreren Senken.

Alle Quellen haben denselben Standardaufbau:
```
source(
  source properties
) ~> source_name
```

Beispielsweise würde eine einfache Quelle mit drei Spalten („movieId“, „title“, „genres“) so aussehen:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
```

Alle anderen Transformationen als Quellen haben denselben Standardaufbau:
```
name_of_incoming_stream transformation_type(
  properties
) ~> new_stream_name
```

Beispielsweise würde eine einfache abgeleitete Transformation, die eine Spalte („title“) mit einer Version in Großbuchstaben überschreibt, so aussehen:
```
source1 derive(
  title = upper(title)
) ~> derive1
```

Und eine Senke ohne Schema würde so aussehen:
```
derive1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie Datenflüsse, und lesen Sie zunächst den [Artikel „data flows overview“](concepts-data-flow-overview.md) (Übersicht über Datenflüsse).
