---
title: ENCODE
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie das ENCODE-Dataset in Azure Open Datasets verwendet wird.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 921f04b4f874fd83403a613823df0f2bd600feb8
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038950"
---
# <a name="encode-encyclopedia-of-dna-elements"></a>ENCODE: Enzyklopädie der DNA-Elemente

Die [Enzyklopädie der DNA-Elemente (ENCODE)](https://www.encodeproject.org/help/project-overview/) ist eine fortlaufende internationale Zusammenarbeit von Forschungsgruppen, die vom National Human Genome Research Institute (NHGRI) finanziert werden. Das Ziel von ENCODE besteht darin, eine umfassende Liste der funktionalen Elemente im menschlichen Genom zu erstellen, einschließlich der Elemente, die auf der Ebene „Protein“ und „RNA“ agieren, sowie der regulatorischen Elemente, die Zellen und Umstände steuern, in denen ein Gen aktiv ist.

ENCODE-Forscher verwenden verschiedene Analysen und Methoden zur Identifizierung funktionaler Elemente. Die Ermittlung und Annotierung von Genelementen erfolgt hauptsächlich durch Sequenzierung einer Vielzahl von RNA-Quellen, vergleichende Genomik, integrative bioinformatische Methoden und menschliche Kuratierung. Regulatorische Elemente werden normalerweise durch DNA-Hypersensibilitätsassays, Assays zur DNA-Methylierung und Immunpräzipation (IP) von Proteinen untersucht, die mit DNA und RNA interagieren, d. h. modifizierte Histone, Transkriptionsfaktoren, Chromatinregulatoren und RNA-bindende Proteine, gefolgt von Sequenzierung.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Datenquelle

Bei diesem Dataset handelt es sich um eine Spiegelung des Datenspeichers unter https://www.encodeproject.org/.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Dieses Dataset beinhaltet annähernd 756 TB Daten und wird in der ersten Woche jedes Monats aktualisiert.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in den Azure-Regionen „USA, Westen 2“ und „USA, Westen-Mitte“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in den Regionen „USA, Westen 2“ oder „USA, Westen-Mitte“ empfohlen.

## <a name="data-access"></a>Datenzugriff

USA, Westen 2: https://datasetencode.blob.core.windows.net/dataset

USA, Westen-Mitte: https://datasetencode-secondary.blob.core.windows.net/dataset

[SAS-Token](/azure/storage/common/storage-sas-overview): ?sv=2019-10-10&si=prod&sr=c&sig=9qSQZo4ggrCNpybBExU8SypuUZV33igI11xw0P7rB3c%3D

## <a name="use-terms"></a>Nutzungsbedingungen

Externe Datenbenutzer können auf ENCODE-Daten basierende Ergebnisse unabhängig von Typ oder Größe kostenlos herunterladen und ohne Einschränkungen analysieren und veröffentlichen. Dabei gilt für ENCODE-Datenproduzenten (sei es als Einzelmitglied oder als Mitglied des Konsortiums) keine Toleranzperiode. Forscher, die nicht veröffentlichte ENCODE-Daten verwenden, werden darin bestärkt, sich an die Datenproduzenten zu wenden, um die Möglichkeit einer Veröffentlichung zu erörtern. Das Konsortium wird die Ergebnisse seiner eigenen Analyseaktivitäten auch weiterhin in unabhängigen Publikationen veröffentlichen.

ENCODE bittet darum, dass Forscher, die ENCODE-Datensätze (veröffentlicht oder unveröffentlicht) in Publikationen und Präsentationen verwenden, das ENCODE-Konsortium auf alle folgenden Arten zitieren, die unter [https://www.encodeproject.org/help/citing-encode/](https://www.encodeproject.org/help/citing-encode/) aufgeführt sind.

## <a name="contact"></a>Contact

Wenn Sie Fragen, Bedenken oder Kommentare haben, senden Sie eine E-Mail an unseren Helpdesk unter encode-help@lists.stanford.edu.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.