---
title: Hinzufügen von Tags zu digitalen Zwillingen
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Sie Tags für digitale Zwillinge implementieren.
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5631218e40b701a4a8f5cc42531f052e133044c5
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107904062"
---
# <a name="add-tags-to-digital-twins"></a>Hinzufügen von Tags zu digitalen Zwillingen 

Sie können das Konzept der Tags verwenden, um Ihre digitalen Zwillinge genauer zu anzugeben und zu kategorisieren. Insbesondere möchten Benutzer möglicherweise Tags aus vorhandenen Systemen wie etwa [Haystack-Tags](https://project-haystack.org/doc/TagModel) in ihren Azure Digital Twins-Instanzen replizieren. 

In diesem Artikel werden Muster beschrieben, die zum Implementieren von Tags für digitale Zwillinge verwendet werden können.

Tags werden zuerst als Eigenschaften innerhalb des [Modells](concepts-models.md) hinzugefügt, das einen digitalen Zwilling beschreibt. Diese Eigenschaft wird dann für den Zwilling festgelegt, wenn dieser auf der Grundlage des Modells erstellt wird. Danach können die Tags in [Abfragen](concepts-query-language.md) verwendet werden, um die Zwillinge zu identifizieren und zu filtern.

## <a name="marker-tags"></a>Markertags 

Bei einem **Markertag** handelt es sich um eine einfache Zeichenfolge, die zum Markieren oder Kategorisieren eines digitalen Zwillings (z. B. „blue“ oder „red“) verwendet wird. Bei dieser Zeichenfolge handelt es sich um den Namen des Tags. Markertags haben keinen aussagekräftigen Wert – das Tag ist allein durch sein Vorhandensein (oder Fehlen) von Bedeutung. 

### <a name="add-marker-tags-to-model"></a>Hinzufügen eines Markertags zu einem Modell 

Markertags werden als [DTDL](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)-Zuordnung zwischen `string` und `boolean` modelliert. Der boolesche `mapValue` wird ignoriert, da nur das Vorhandensein des Tags wichtig ist. 

Im Folgenden finden Sie einen Auszug aus einem Zwillingsmodell, mit dem ein Markertag als Eigenschaft implementiert wird:

:::code language="json" source="~/digital-twins-docs-samples/models/tags.json" range="2-16":::

### <a name="add-marker-tags-to-digital-twins"></a>Hinzufügen von Markertags zu digitalen Zwillingen

Nachdem die `tags`-Eigenschaft Teil des Modells des digitalen Zwillings ist, können Sie das Markertag im digitalen Zwilling festlegen, indem Sie den Wert dieser Eigenschaft festlegen. 

Im Folgenden finden Sie ein Beispiel, bei dem der Marker `tags` für drei Zwillinge aufgefüllt wird:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesMarker":::

Hier sehen Sie ein Codebeispiel zum Festlegen des Markers `tags` für einen Zwilling unter Verwendung des [.NET SDK](/dotnet/api/overview/azure/digitaltwins/client):

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesCsharp":::

### <a name="query-with-marker-tags"></a>Abfragen mit Markertags

Nachdem digitalen Zwillingen Tags hinzugefügt wurden, können die Tags zum Filtern der Zwillinge in Abfragen verwendet werden. 

Im Folgenden finden Sie eine Abfrage, mit der alle Zwillinge abgerufen werden, die mit „red“ getaggt sind: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="QueryMarkerTags1":::

Tags können auch zu komplexeren Abfragen kombiniert werden. Im Folgenden finden Sie eine Abfrage, mit der alle Zwillinge abgerufen werden, die rund und nicht rot sind: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="QueryMarkerTags2":::

## <a name="value-tags"></a>Werttags 

Bei einem **Werttag** handelt es sich um ein Schlüssel-Wert-Paar, das verwendet wird, um einem Tag einen Wert zuzuweisen, z. B. `"color": "blue"` oder `"color": "red"`. Nachdem ein Werttags erstellt wurde, kann es durch Ignorieren des Tagwerts auch als Markertag verwendet werden. 

### <a name="add-value-tags-to-model"></a>Hinzufügen eines Werttags zu einem Modell 

Werttags werden als [DTDL](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)-Zuordnung zwischen `string` und `string` modelliert. Sowohl `mapKey` als auch `mapValue` sind von Bedeutung. 

Im Folgenden finden Sie einen Auszug aus einem Zwillingsmodell, mit dem ein Werttag als Eigenschaft implementiert wird:

:::code language="json" source="~/digital-twins-docs-samples/models/tags.json" range="17-31":::

### <a name="add-value-tags-to-digital-twins"></a>Hinzufügen von Werttags zu digitalen Zwillingen

Wie ein Markertag können Sie auch das Werttag in einem digitalen Zwilling festlegen, indem Sie den Wert der `tags`-Eigenschaft über das Modell festlegen. Wenn Sie ein Werttag als Markertag verwenden möchten, können Sie das `tagValue`-Feld auf den leeren Zeichenfolgenwert (`""`) festlegen. 

Im Folgenden finden Sie ein Beispiel, bei dem der Wert `tags` für drei Zwillinge aufgefüllt wird:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesValue":::

In diesem Beispiel werden `red` und `purple` als Markertags verwendet.

### <a name="query-with-value-tags"></a>Abfragen mit Werttags

Wie Markertags können Sie auch Werttags zum Filtern von Zwillingen in Abfragen verwenden. Sie können Werttags und Markertags auch zusammen verwenden.

Im obigen Beispiel wird `red` als Markertag verwendet. Denken Sie daran, dass es sich hierbei um eine Abfrage handelt, mit der alle Zwillinge abgerufen werden, die mit „red“ getaggt sind: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="QueryMarkerTags1":::

Im Folgenden finden Sie eine Abfrage, mit der alle Entitäten abgerufen werden, die klein (Werttag) und nicht rot sind: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="QueryMarkerValueTags":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Entwerfen und Verwalten von Modellen digitaler Zwillinge:
* [*Vorgehensweise: Verwalten von DTDL-Modellen*](how-to-manage-model.md)

Erfahren Sie mehr über das Abfragen des Zwillingsgraphen:
* [*Verwenden Abfragen des Zwillingsgraphen*](how-to-query-graph.md)
