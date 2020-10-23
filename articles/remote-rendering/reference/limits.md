---
title: Einschränkungen
description: Codeeinschränkungen für SDK-Features
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: b500a3777ee24d6615022dae2571d021bd0d675a
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92201170"
---
# <a name="limitations"></a>Einschränkungen

Eine Reihe von Features besitzen Einschränkungen hinsichtlich Größe, Anzahl oder anderer Aspekte.

## <a name="azure-frontend"></a>Azure-Front-End

* Gesamtanzahl der Azure-Front-End-Instanzen pro Prozess: 16.
* Gesamtanzahl der Azure-Sitzungsinstanzen pro Azure-Front-End: 16.

## <a name="objects"></a>Objekte

* Zulässige Gesamtanzahl zulässiger Objekte eines einzelnen Typs (Entität, CutPlaneComponent usw.): 16.777.215.
* Zulässige Gesamtanzahl aktiver Schnittebenen: 8.

## <a name="geometry"></a>Geometrie

* **Animation:** Animationen sind auf die Animation einzelner Transformationen von Spielobjekten beschränkt. Skelettanimationen („Rigging“) mit Skinning- oder Vertexanimationen werden nicht unterstützt. Animationsspuren aus der Quellmedienobjektdatei werden nicht beibehalten. Stattdessen müssen Transformationsanimationen von Objekten durch Clientcode gesteuert werden.
* **Benutzerdefinierte Shader:** Das Erstellen benutzerdefinierter Shader wird nicht unterstützt. Es können nur [Farbmaterialien](../overview/features/color-materials.md) oder [PBR-Materialien](../overview/features/pbr-materials.md) verwendet werden.
* **Maximale Anzahl unterschiedlicher Materialien** in einem Medienobjekt: 65.535. Weitere Informationen zur Verringerung der automatischen Materialanzahl finden Sie im Kapitel [Deduplizierung von Materialien](../how-tos/conversion/configure-model-conversion.md#material-de-duplication).
* **Maximale Dimension einer einzelnen Textur:** 16.384 x 16.384. Größere Quelltexturen werden durch den Konvertierungsprozess verkleinert.

### <a name="overall-number-of-polygons"></a>Gesamtanzahl von Polygonen

Die zulässige Anzahl von Polygonen für alle geladenen Modelle ist von der Größe der VM abhängig, die an die [Sitzungsverwaltungs-REST-API](../how-tos/session-rest-api.md#create-a-session) übergeben wurde:

| Servergröße | Maximale Anzahl von Polygonen |
|:--------|:------------------|
|Standard| 20 Mio. |
|premium| Keine Begrenzung |

Ausführliche Informationen zu dieser Einschränkung finden Sie im Kapitel [Servergröße](../reference/vm-sizes.md).

## <a name="platform-limitations"></a>Plattformeinschränkungen

**Windows 10 Desktop**

* Win32/x64 ist die einzige unterstützte Win32-Plattform. Win32/x86 wird nicht unterstützt.

**HoloLens 2**

* Die Funktion [Von PV-Kamera rendern](/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) wird nicht unterstützt.