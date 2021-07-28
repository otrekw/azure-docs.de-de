---
title: SnpEff
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie das SnpEff-Dataset in Azure Open Datasets verwendet wird.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 3cc23edff82dce0c14a795e1b3b0a29975cb96e4
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038720"
---
# <a name="snpeff-genomic-variant-annotations-and-functional-effect-prediction-toolbox"></a>SnpEff: Toolbox für Genomvariantenanmerkungen und Vorhersagen der funktionalen Auswirkung

[SnpEff](https://pcingola.github.io/SnpEff/) Toolbox für das Versehen von genetischen Varianten mit Anmerkungen und das Vorhersagen der funktionalen Auswirkung Es versieht genetische Varianten mit Anmerkungen und sagt deren Auswirkungen auf Gene und Proteine vorher (z. B. Änderungen an Aminosäuren).

Weitere Informationen zu den Daten finden Sie im [Benutzerhandbuch](https://pcingola.github.io/SnpEff/se_introduction/).

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Datenquelle

Dieses Dataset ist eine Spiegelung von http://downloads.sourceforge.net/project/snpeff/databases/.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Dieses Dataset enthält etwa 2 TB Daten und wird monatlich aktualisiert.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in den Azure-Regionen „USA, Westen 2“ und „USA, Westen-Mitte“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in die Regionen „USA, Westen 2“ oder „USA, Westen-Mitte“ empfohlen.

## <a name="data-access"></a>Datenzugriff

USA, Westen 2: https://datasetsnpeff.blob.core.windows.net/dataset

USA, Westen-Mitte: https://datasetsnpeff-secondary.blob.core.windows.net/dataset

[SAS-Token](../storage/common/storage-sas-overview.md): sv=2019-10-10&st=2020-09-01T00%3A00%3A00Z&se=2050-09-01T00%3A00%3A00Z&si=prod&sr=c&sig=isafOa9tGnYBAvsXFUMDGMTbsG2z%2FShaihzp7JE5dHw%3D

## <a name="use-terms"></a>Nutzungsbedingungen
Die Daten stehen uneingeschränkt zur Verfügung. Weitere Informationen und Zitatdetails finden Sie unter [Zugreifen auf und Verwenden von Daten in ClinVar](https://pcingola.github.io/SnpEff/SnpEff_manual.html#intro).

## <a name="contact"></a>Contact

Bei Fragen oder Feedback zu diesem Dataset wenden Sie sich an [Pablo Cingolani](http://www.linkedin.com/in/pablocingolani).


## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.