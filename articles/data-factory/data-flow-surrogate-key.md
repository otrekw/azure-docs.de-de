---
title: Transformation für Ersatzschlüssel im Zuordnungsdatenfluss
description: Hier erfahren Sie, wie Sie die Transformation für Ersatzschlüssel von Azure Data Factory Mapping Data Flow zum Generieren nachfolgender Schlüsselwerte verwenden.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/08/2020
ms.openlocfilehash: ade2fd6011bbcdaed4ce31ce70bfb4235429bb0d
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606292"
---
# <a name="surrogate-key-transformation-in-mapping-data-flow"></a>Transformation für Ersatzschlüssel im Zuordnungsdatenfluss 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verwenden Sie die Transformation für Ersatzschlüssel, um einen inkrementell geänderten Schlüsselwert für jede Datenzeile hinzuzufügen. Dieses Vorgehen ist nützlich, wenn Sie Dimensionstabellen in einem analytischen Datenmodell mit Sternschema entwerfen. In einem Sternschema erfordert jedes Mitglied in Dimensionstabellen einen eindeutigen Schlüssel, bei dem es sich nicht um einen Geschäftsschlüssel handelt.

## <a name="configuration"></a>Konfiguration

![Transformation für Ersatzschlüssel](media/data-flow/surrogate.png "Transformation für Ersatzschlüssel")

**Schlüsselspalte**: Der Name der Spalte mit dem generierten Ersatzschlüssel.

**Startwert**: Die niedrigste Schlüsselwert, der generiert wird.

## <a name="increment-keys-from-existing-sources"></a>Inkrementelle Schlüssel aus vorhandenen Quellen

Um Ihre Sequenz von einem Wert aus zu starten, der in einer Quelle vorhanden ist, verwenden Sie nach der Transformation für Ersatzschlüssel eine Transformation für abgeleitete Spalten, und fügen Sie die beiden Werte zusammen:

![Maximalwert für Ersatzschlüssel hinzufügen](media/data-flow/sk006.png "Transformation für Ersatzschlüssel – Maximalwert hinzufügen")

### <a name="increment-from-existing-maximum-value"></a>Inkrement des vorhandenen Maximalwerts

Ein Seeding für den Schlüsselwert mit dem vorherigen Maximalwert lässt sich mit zwei Methoden durchführen, je nachdem, wo sich Ihre Quelldaten befinden.

#### <a name="database-sources"></a>Datenbankquellen

Verwenden Sie eine SQL-Abfrageoption, um „MAX()“ aus Ihrer Quelle auszuwählen. Beispiel: `Select MAX(<surrogateKeyName>) as maxval from <sourceTable>`/.

![Ersatzschlüsselabfrage](media/data-flow/sk002.png "Transformation für Ersatzschlüssel – Abfrage")

#### <a name="file-sources"></a>Dateiquellen

Wenn sich der vorherige Maximalwert in einer Datei befindet, verwenden Sie die `max()`-Funktion in der Aggregattransformation, um den vorherigen Maximalwert abzurufen:

![Ersatzschlüsseldatei](media/data-flow/sk008.png "Ersatzschlüsseldatei")

In beiden Fällen müssen Sie die eingehenden neuen Daten mit der Quelle verknüpfen, die den vorherigen Maximalwert enthält.

![Ersatzschlüsselverknüpfung](media/data-flow/sk004.png "Ersatzschlüsselverknüpfung")

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream> 
    keyGenerate(
        output(<surrogateColumnName> as long),
        startAt: <number>L
    ) ~> <surrogateKeyTransformationName>
```

### <a name="example"></a>Beispiel

![Transformation für Ersatzschlüssel](media/data-flow/surrogate.png "Transformation für Ersatzschlüssel")

Der nachfolgende Codeausschnitt zeigt das Datenflussskript für die obige Konfiguration des Ersatzschlüssels.

```
AggregateDayStats
    keyGenerate(
        output(key as long),
        startAt: 1L
    ) ~> SurrogateKey1
```

## <a name="next-steps"></a>Nächste Schritte

Diese Beispiele verwenden die Transformationen [Verbinden](data-flow-join.md) und [Abgeleitete Spalten](data-flow-derived-column.md) Transformationen.
