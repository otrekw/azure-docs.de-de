---
title: Gittermodelle
description: Definition von Gittermodellen im Bereich von Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: b800272a538243d39a89e4eed64bc5cbc2d53ad8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679376"
---
# <a name="meshes"></a>Gittermodelle

## <a name="mesh-resource"></a>Gittermodellressource

Bei Gittermodellen (Meshes) handelt es sich um eine unveränderliche [gemeinsam genutzte Ressource](../concepts/lifetime.md), die nur per [Modellkonvertierung](../how-tos/conversion/model-conversion.md) erstellt werden kann. Gittermodelle enthalten mindestens ein *untergeordnetes Gittermodell*. Für jedes untergeordnete Gittermodell wird auf ein [Material](materials.md) verwiesen, das standardmäßig zum Rendern verwendet werden soll. Fügen Sie zum Platzieren eines Gittermodells in einem 3D-Raum einer [Entität](entities.md) ein [MeshComponent](#meshcomponent)-Element hinzu.

### <a name="mesh-resource-properties"></a>Eigenschaften von Gittermodellressourcen

Die Eigenschaften der `Mesh`-Klasse lauten:

* **Materials** (Materialien): Ein Array mit Materialien. Jedes Material wird von einem anderen untergeordneten Gittermodell genutzt. Es ist möglich, dass von mehreren Einträgen im Array auf dasselbe [Material](materials.md) verwiesen wird. Diese Daten können zur Runtime nicht geändert werden.

* **Bounds** (Begrenzungen): Ein an den Achsen ausgerichteter Begrenzungsrahmen (Axis-Aligned Bounding Box, AABB) für die Scheitelpunkte des Gittermodells im lokalen Raum.

## <a name="meshcomponent"></a>MeshComponent

Die `MeshComponent`-Klasse wird verwendet, um eine Instanz einer Gittermodellressource zu platzieren. Jedes „MeshComponent“-Element verweist auf ein Gittermodell. Hiermit kann überschrieben werden, welche Materialien zum Rendern der einzelnen untergeordneten Gittermodelle verwendet werden.

### <a name="meshcomponent-properties"></a>Eigenschaften von „MeshComponent“

* **Mesh** (Gittermodell): Die von dieser Komponente verwendete Gittermodellressource.

* **Materials** (Materialien): Das Array mit den Materialien, die für die Gittermodellkomponente selbst angegeben wurden. Das Array hat immer die gleiche Länge wie das Array *Materials* (Materialien) der Gittermodellressource. Materialien, die nicht durch die Standardeinstellung des Gittermodells überschrieben werden sollen, sind in diesem Array auf *null* festgelegt.

* **UsedMaterials** (Verwendete Materialien): Das Array mit den tatsächlich genutzten Materialien für die einzelnen untergeordneten Gittermodelle. Dies ist für Werte, die nicht „null“ sind, mit den Daten im Array *Materials* (Materialien) identisch. Andernfalls ist hierin der Wert aus dem Array *Materials* (Materialien) der Gittermodellinstanz enthalten.

## <a name="next-steps"></a>Nächste Schritte

* [Materialien](materials.md)
