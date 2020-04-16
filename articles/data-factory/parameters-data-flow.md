---
title: Parametrisieren von Zuordnungsdatenflüssen
description: Erfahren Sie, wie Sie eine Mapping Data Flow-Funktion aus Data Factory-Pipelines parametrisieren können.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 82660cdb4ab6523bae7608fe3b071f20cb3603f8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419169"
---
# <a name="parameterizing-mapping-data-flows"></a>Parametrisieren von Zuordnungsdatenflüssen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)] 

Zuordnungsdatenflüsse in Azure Data Factory unterstützen die Verwendung von Parametern. Sie können Parameter innerhalb Ihrer Datenflussdefinition definieren, die Sie dann in Ihren Ausdrücken verwenden können. Die Parameterwerte können von der aufrufenden Pipeline über die Aktivität „Datenfluss ausführen“ festgelegt werden. Sie haben drei Möglichkeiten, die Werte in den Ausdrücken der Datenflussaktivität festzulegen:

* Verwenden der Ausdruckssprache für die Pipeline-Ablaufsteuerung, um einen dynamischen Wert festzulegen
* Verwenden der Ausdruckssprache für den Datenfluss, um einen dynamischen Wert festzulegen
* Verwenden einer der beiden Ausdruckssprachen, um einen statischen Literalwert festzulegen

Nutzen Sie diese Funktion, um Ihre Datenflüsse universell einsetzbar, flexibel und wiederverwendbar zu gestalten. Mit diesen Parametern können Sie Datenflusseinstellungen und -ausdrücke parametrisieren.

## <a name="create-parameters-in-a-mapping-data-flow"></a>Erstellen von Parametern in einem Zuordnungsdatenfluss

Wenn Sie Parameter zu Ihrem Datenfluss hinzufügen möchten, klicken Sie auf den leeren Bereich der Datenflusscanvas, um die allgemeinen Eigenschaften anzuzeigen. Im Einstellungsbereich sehen Sie die Registerkarte **Parameter**. Wählen Sie **Neu** aus, um einen neuen Parameter zu generieren. Für jeden Parameter müssen Sie einen Namen angeben, einen Typ auswählen und optional einen Standardwert festlegen.

![Erstellen von Datenflussparametern](media/data-flow/create-params.png "Erstellen von Datenflussparametern")

## <a name="use-parameters-in-a-mapping-data-flow"></a>Verwenden von Parametern in einem Zuordnungsdatenfluss 

Auf Parameter kann in jedem beliebigen Datenflussausdruck verwiesen werden. Parameter beginnen mit $ und sind unveränderlich. Die Liste der verfügbaren Parameter finden Sie im Ausdrucks-Generator auf der Registerkarte **Parameter**.

![Datenflussparameterausdruck](media/data-flow/parameter-expression.png "Datenflussparameterausdruck")

Sie können zusätzliche Parameter schnell hinzufügen, indem Sie **Neuer Parameter** auswählen und dann den Namen und Typ angeben.

![Datenflussparameterausdruck](media/data-flow/new-parameter-expression.png "Datenflussparameterausdruck")

### <a name="passing-in-a-column-name-as-a-parameter"></a>Übergeben eines Spaltennamens als Parameter

Ein gängiges Muster ist die Übergabe eines Spaltennamen als Parameterwert. Mithilfe der Funktion `byName()` können Sie auf die Spalte verweisen, die dem Parameter zugeordnet ist. Denken Sie daran, die Spalte mithilfe einer Umwandlungsfunktion wie `toString()` in ihren entsprechenden Typ umzuwandeln.

Wenn Sie beispielsweise eine Zeichenfolgenspalte aufgrund eines `columnName`-Parameters zuordnen möchten, können Sie eine abgeleitete Spaltentransformation gleich `toString(byName($columnName))` hinzufügen.

![Übergeben eines Spaltennamens als Parameter](media/data-flow/parameterize-column-name.png "Übergeben eines Spaltennamens als Parameter")

## <a name="assign-parameter-values-from-a-pipeline"></a>Zuweisen von Parameterwerten aus einer Pipeline

Nachdem Sie Ihren Datenfluss mit Parametern erstellt haben, können Sie ihn mit der Aktivität „Datenfluss ausführen“ aus einer Pipeline ausführen. Nachdem Sie die Aktivität zu Ihrer Pipelinecanvas hinzugefügt haben, werden Ihnen auf der Registerkarte **Parameter** der Aktivität die verfügbaren Datenflussparameter angezeigt.

![Festlegen eines Datenflussparameters](media/data-flow/parameter-assign.png "Festlegen eines Datenflussparameters")

Wenn Ihr Parameterdatentyp eine Zeichenfolge ist, können Sie beim Klicken auf das Textfeld zum Festlegen von Parameterwerten entweder einen Pipelineausdruck oder einen Datenflussausdruck eingeben. Wenn Sie sich für den Pipelineausdruck entscheiden, wird Ihnen das Fenster für den Pipelineausdruck angezeigt. Stellen Sie sicher, dass Pipelinefunktionen in die Zeichenfolgen-Interpolationssyntax mit `'@{<expression>}'` aufgenommen werden, z. B.:

```'@{pipeline().RunId}'```

Wenn Ihr Parameter nicht vom Zeichenfolgentyp ist, wird Ihnen immer der Data Flow-Ausdrucks-Generator angezeigt. Hier können Sie beliebige Ausdrücke oder Literalwerte eingeben, die dem Datentyp des Parameters entsprechen. Nachfolgend sehen Sie Beispiele für den Datenflussausdruck und ein Zeichenfolgenliteral aus dem Ausdrucks-Generator:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Jede Mapping Data Flow-Funktion kann eine beliebige Kombination von Pipeline- und Datenfluss-Ausdrucksparametern aufweisen. 

![Beispiel für Datenflussparameter](media/data-flow/parameter-example.png "Beispiel für Datenflussparameter")



## <a name="next-steps"></a>Nächste Schritte
* [Ausführen der Datenflussaktivität](control-flow-execute-data-flow-activity.md)
* [Ausdrücke für die Ablaufsteuerung](control-flow-expression-language-functions.md)
