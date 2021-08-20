---
title: 1000 Genomes
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie das 1000 Genomes-Dataset in Azure Open Datasets verwendet wird.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 52f948526b1575f43d722c856251eed83b2e66ef
ms.sourcegitcommit: 7c44970b9caf9d26ab8174c75480f5b09ae7c3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2021
ms.locfileid: "112982769"
---
# <a name="1000-genomes"></a>1000 Genomes

Im Rahmen des von 2008 bis 2015 durchgeführten 1000 Genomes Project entstand der größte öffentliche Katalog mit Daten zu menschlichen Variationen und Genotypdaten. Das finale Dataset enthält Daten zu 2.504 Personen aus 26 Populationen sowie 84 Millionen identifizierten Varianten. Weitere Informationen finden Sie auf der Website zum 1000 Genomes Project sowie in den folgenden Veröffentlichungen:

Pilotanalyse: A map of human genome variation from population-scale sequencing (Eine Karte mit Variationen menschlicher Genome basierend auf der Sequenzierung von Populationen) Nature 467, 1061–1073 (28. Oktober 2010)

Analyse Phase 1: An integrated map of genetic variation from 1,092 human genomes (Eine umfassende Karte mit genetischen Variationen aus 1.092 menschlichen Genomen) Nature 491, 56–65 (1. November 2012)

Analyse Phase 3: A global reference for human genetic variation (Eine globale Referenz für menschliche genetische Variationen) Nature 526, 68–74 (1. Oktober 2015) und An integrated map of structural variation in 2,504 human genomes (Eine umfassende Karte mit strukturellen Variationen in 2.504 menschlichen Genomen) Nature 526, 75–81 (1. Oktober 2015)

Ausführliche Informationen zu den Datenformaten finden Sie unter http://www.internationalgenome.org/formats.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Datenquelle

Für dieses Dataset wurde ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/ gespiegelt.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Dieses Dataset enthält etwa 815 TB Daten und wird täglich aktualisiert.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in den Azure-Regionen „USA, Westen 2“ und „USA, Westen-Mitte“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in die Regionen „USA, Westen 2“ oder „USA, Westen-Mitte“ empfohlen.

## <a name="data-access"></a>Datenzugriff

USA, Westen 2: „https://dataset1000genomes.blob.core.windows.net/dataset“

SA, Westen-Mitte: „https://dataset1000genomes-secondary.blob.core.windows.net/dataset“

[SAS-Token](../storage/common/storage-sas-overview.md): sv=2019-10-10&si=prod&sr=c&sig=9nzcxaQn0NprMPlSh4RhFQHcXedLQIcFgbERiooHEqM%3D

## <a name="use-terms"></a>Nutzungsbedingungen

Seit den abschließenden Veröffentlichungen sind die Daten aus dem 1000 Genomes Project öffentlich verfügbar und können von jedem gemäß den in der Datasetquelle ([http://www.internationalgenome.org/data](http://www.internationalgenome.org/data)) angegebenen Bedingungen genutzt werden. Für die Quellenangabe bei Nutzung der Daten sollten die in den [FAQs]() des 1000 Genomes Project aufgeführten Angaben verwendet werden.

## <a name="contact"></a>Contact

https://www.internationalgenome.org/contact

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.
