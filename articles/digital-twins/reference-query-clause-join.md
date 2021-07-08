---
title: 'Referenz zur Azure Digital Twins-Abfragesprache: JOIN-Klauseln'
titleSuffix: Azure Digital Twins
description: Referenzdokumentation zur JOIN-Klausel der Azure Digital Twins-Abfragesprache
author: baanders
ms.author: baanders
ms.date: 03/31/2021
ms.topic: article
ms.service: digital-twins
ms.openlocfilehash: 7dc6827f7ebd7b034ffc00906629bafe04036fbd
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109789567"
---
# <a name="azure-digital-twins-query-language-reference-join-clause"></a>Referenz zur Azure Digital Twins-Abfragesprache: JOIN-Klausel

Dieses Dokument enthält Referenzinformationen zur **JOIN-Klausel** für die [Azure Digital Twins-Abfragesprache](concepts-query-language.md).

Die `JOIN`-Klausel wird bei der Azure Digital Twins-Abfragesprache als Teil der [FROM-Klausel](reference-query-clause-from.md) verwendet, wenn Sie Abfragen durchführen und der Azure Digital Twins-Graph durchlaufen werden soll.

Diese Klausel kann bei Abfragen optional genutzt werden.

## <a name="core-syntax-join--related"></a>Core-Syntax: JOIN ... RELATED 
Da Beziehungen in Azure Digital Twins Teil von digitalen Zwillingen und keine unabhängigen Entitäten sind, wird das Schlüsselwort `RELATED` in `JOIN`-Abfragen genutzt, um aus der Zwillingssammlung auf die Beziehungen eines bestimmten Typs zu verweisen. Dieser Gruppe von Beziehungen kann ein Sammlungsname zugewiesen werden.

Für die Abfrage muss dann die `WHERE`-Klausel verwendet werden, um anzugeben, welche spezifischen Zwillinge zur Unterstützung der Beziehungsabfrage verwendet werden. Hierfür wird entweder für den Quell- oder den Zielzwilling nach dem Wert `$dtId` gefiltert.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="JoinSyntax":::

### <a name="example"></a>Beispiel

Mit der folgenden Abfrage werden alle digitalen Zwillinge ausgewählt, die zum Zwilling mit der ID *ABC* gehören, indem eine *contains*-Beziehung verwendet wird.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="JoinExample":::

## <a name="multiple-joins"></a>Mehrere JOIN-Klauseln

Es werden bis zu fünf `JOIN`-Klauseln pro Abfrage unterstützt. Dies ermöglicht es, dass mehrere Ebenen mit Beziehungen gleichzeitig durchlaufen werden.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="MultiJoinSyntax":::

### <a name="example"></a>Beispiel

Die folgende Abfrage basiert auf Räumen (Rooms) mit Beleuchtungselementen (LightPanels), die jeweils mehrere Glühlampen (LightBulbs) enthalten. Mit der Abfrage werden alle „LightBulbs“ der „LightPanels“ der Räume 1 und 2 abgerufen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="MultiJoinExample":::

## <a name="limitations"></a>Einschränkungen

Die folgenden Grenzwerte gelten für Abfragen mit `JOIN`.
* [Tiefenlimit von 5](#depth-limit-of-five)
* [Keine OUTER JOIN-Semantik (äußere Verknüpfung)](#no-outer-join-semantics)
* [Quellzwilling erforderlich](#twins-required)

Weitere Informationen finden Sie in den folgenden Abschnitten.

### <a name="depth-limit-of-five"></a>Tiefenlimit von 5

Die Durchlauftiefe für den Graphen ist auf fünf `JOIN`-Ebenen pro Abfrage beschränkt.

#### <a name="example"></a>Beispiel

Die folgende Abfrage veranschaulicht die maximale Anzahl von `JOINs`, die in einer Azure Digital Twins-Abfrage möglich ist. Es werden alle „LightBulbs“ in „Building1“ abgerufen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="MaxJoinExample":::

### <a name="no-outer-join-semantics"></a>Keine OUTER JOIN-Semantik (äußere Verknüpfung)

`OUTER JOIN`-Semantik wird nicht unterstützt. Das bedeutet, dass die gesamte „Zeile“ aus dem Ausgaberesultset entfernt wird, wenn die Beziehung den Rang null aufweist.

#### <a name="example"></a>Beispiel

Mit der folgenden Abfrage wird das Durchlaufen eines Gebäudes veranschaulicht.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="NoOuterJoinExample":::

Falls „Building1“ keine Etagen hat, wird bei dieser Abfrage ein leeres Resultset zurückgegeben (anstelle einer Zeile mit einem Wert für „Building“ und mit `undefined` für „Floor“ (Etage)).

### <a name="twins-required"></a>Zwillinge erforderlich

Beziehungen in Azure Digital Twins können nicht als unabhängige Entitäten abgefragt werden. Sie müssen zudem Informationen über den Quellzwilling angeben, von dem die Beziehung stammt. Dies ist Teil der Standardnutzung von `JOIN` in Azure Digital Twins mit dem Schlüsselwort `RELATED`. 

Für Abfragen mit einer `JOIN`-Klausel muss in der `WHERE`-Klausel auch nach der `$dtId`-Eigenschaft eines Zwillings gefiltert werden, um zu ermitteln, welche Zwillinge zur Unterstützung der Beziehungsabfrage verwendet werden.