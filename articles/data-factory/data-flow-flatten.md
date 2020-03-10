---
title: 'Zuordnungsdatenfluss: Vereinfachungstransformation'
description: 'Azure Data Factory-Zuordnungsdatenfluss: Vereinfachungstransformation'
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 415a093fd8a8fbe27e1d240b061548e18f2ca6b6
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "78164728"
---
# <a name="azure-data-factory-flatten-transformation"></a>Azure Data Factory: Vereinfachungstransformation

Die Vereinfachungstransformation kann verwendet werden, um Arraywerte in einer hierarchischen Struktur in neue Zeilen zu pivotieren, wodurch Ihre Daten im Wesentlichen denormalisiert werden.

![Transformationstoolbox](media/data-flow/flatten5.png "Transformationstoolbox")

![Vereinfachungstransformation 1](media/data-flow/flatten7.png "Vereinfachungstransformation 1")

## <a name="unroll-by"></a>Auflösen

Wählen Sie zuerst die Arrayspalte aus, die Sie auflösen und pivotieren möchten.

![Einstellungen der Vereinfachungstransformation](media/data-flow/flatten1.png "Einstellungen der Vereinfachungstransformation")

## <a name="unroll-root"></a>Stamm auflösen

Standardmäßig vereinfacht ADF die Struktur an dem Auflösungsarray, das Sie oben ausgewählt haben. Sie können aber auch einen anderen Teil der Hierarchie auswählen, für den die Auflösung ausgeführt werden soll. „Unroll root“ (Stamm auflösen) ist eine optionale Einstellung.

## <a name="input-columns"></a>Eingabespalten

Wählen Sie zuletzt die Projektion Ihrer neuen Struktur basierend auf den eingehenden Feldern und der normalisierten Spalte aus, die Sie aufgelöst haben.

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
foldDown(unroll(<unroll cols>),
    mapColumn(
        name,
        each(<array>(type == '<arrayDataType>')),
        each(<array>, match(true())),
        location
    )) ~> <transformationName>
```

### <a name="example"></a>Beispiel

```
source(output(
        name as string,
        location as string,
        satellites as string[],
        goods as (trade as boolean, customers as string[], orders as (orderId as string, orderTotal as double, shipped as (orderItems as (itemName as string, itemQty as string)[]))[])
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source2
source2 foldDown(unroll(goods.orders.shipped.orderItems),
    mapColumn(
        name,
        each(goods.orders, match(type == 'integer')),
        each(goods.orders.shipped.orderItems, match(true())),
        location
    )) ~> Flatten1
Flatten1 sink(allowSchemaDrift: true,
    validateSchema: false,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> sink1
```    

## <a name="next-steps"></a>Nächste Schritte

* Verwenden der [Pivotierungstransformation](data-flow-pivot.md) zum Pivotieren von Zeilen zu Spalten
* Verwenden Sie die [Entpivotierungstransformation](data-flow-unpivot.md) zum Pivotieren von Spalten in Zeilen.
