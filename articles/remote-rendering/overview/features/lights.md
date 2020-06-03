---
title: Szenenbeleuchtung
description: Beschreibung und Eigenschaften der Lichtquelle
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: e33e012480c876dc5befbb93404bdb131ea9329a
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84022145"
---
# <a name="scene-lighting"></a>Szenenbeleuchtung

Standardmäßig werden die remote gerenderten Objekte mit [Himmelslicht](sky.md) beleuchtet. Bei den meisten Anwendungen ist dies bereits ausreichend, aber Sie können der Szene weitere Lichtquellen hinzufügen.

> [!IMPORTANT]
> Nur [PBR-Materialien](pbr-materials.md) werden von Lichtquellen beeinflusst. [Farbige Materialien](color-materials.md) wirken stets vollständig leuchtend.

> [!NOTE]
> Schattenwurf wird derzeit nicht unterstützt. Azure Remote Rendering ist so optimiert, dass riesige Mengen von Geometriedaten gerendert werden können, bei Bedarf unter Verwendung mehrerer GPUs. Herkömmliche Ansätze für Schattenwurf funktionieren in solchen Szenarien nicht einwandfrei.

## <a name="common-light-component-properties"></a>Gemeinsame Eigenschaften von Lichtkomponenten

Alle Lichttypen leiten sich von der abstrakten Basisklasse `LightComponent` ab und haben diese Eigenschaften gemeinsam:

* **Color** (Farbe): Die Farbe des Lichts im [Gammabereich](https://en.wikipedia.org/wiki/SRGB). Alpha wird ignoriert.

* **Intensity** (Stärke): Die Helligkeit des Lichts. Bei Punkt- und Spotlicht definiert die Stärke auch, wie weit das Licht strahlt.

## <a name="point-light"></a>Punktlicht

In Azure Remote Rendering kann `PointLightComponent` nicht nur Licht von einem einzigen Punkt aus abgeben, sondern auch von einer kleinen Kugel oder einer kleinen Röhre, um weichere Lichtquellen zu simulieren.

### <a name="pointlightcomponent-properties"></a>Eigenschaften von PointLightComponent

* **Radius:** Der Standardradius ist null, was bedeutet, dass das Licht als Punktlicht wirkt. Wenn der Radius größer als null ist, wirkt es als kugelförmige Lichtquelle, die das Erscheinungsbild spiegelnder Schlaglichter verändert.

* **Length** (Länge): Wenn sowohl `Length` als auch `Radius` ungleich null sind, wirkt das Licht wie ein Röhrenlicht. Dies kann zum Simulieren von Neonröhren verwendet werden.

* **AttenuationCutoff:** Bei Belassen auf (0,0) hängt die Dämpfung des Lichts nur von dessen `Intensity` ab. Sie können jedoch benutzerdefinierte Mindest- und Höchstabstände angeben, über die die Lichtstärke linear auf 0 verringert wird. Dieses Feature kann verwendet werden, um einen kleineren Einflussbereich eines bestimmten Lichts zu erzwingen.

* **ProjectedCubemap:** Bei Festlegung auf eine gültige [Cubemap](../../concepts/textures.md) wird die Textur auf die umgebende Geometrie des Lichts projiziert. Die Farbe der Cubemap wird mit der Farbe des Lichts moduliert.

## <a name="spot-light"></a>Spotlicht

`SpotLightComponent` ist `PointLightComponent` ähnlich, aber das Licht ist auf die Form eines Kegels beschränkt. Die Ausrichtung des Kegels wird durch die *negative z-Achse der Entität des Besitzers* definiert.

### <a name="spotlightcomponent-properties"></a>Eigenschaften von SpotLightComponent

* **Radius:** Identisch wie für `PointLightComponent`.

* **SpotAngleDeg:** Dieses Intervall bestimmt den Innen- und Außenwinkel des Kegels, gemessen in Grad. Alles innerhalb des Innenwinkels wird mit maximaler Helligkeit beleuchtet. Zum äußeren Winkel hin wird eine Abschwächung angewendet, die einen halbschattenartigen Effekt erzeugt.

* **FalloffExponent:** Legt fest, wie scharf die Abschwächung zwischen dem inneren und dem äußeren Kegelwinkel verläuft. Ein höherer Wert führt zu einem schärferen Übergang. Der Standardwert 1,0 führt zu einem linearen Übergang.

* **AttenuationCutoff:** Identisch wie für `PointLightComponent`.

* **Projected2dTexture:** Bei Festlegung auf eine gültige [2D-Textur](../../concepts/textures.md) wird das Bild auf Geometrie projiziert, die vom Licht angestrahlt wird. Die Farbe der Textur wird mit der Farbe des Lichts moduliert.

## <a name="directional-light"></a>Gerichtetes Licht

`DirectionalLightComponent` simuliert eine Lichtquelle, die unendlich weit entfernt ist. Das Licht strahlt in die Richtung der *negativen z-Achse der Entität des Besitzers*. Die Position der Entität wird ignoriert.

Es gibt keine weiteren Eigenschaften.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Lichtquellen haben einen erheblichen Einfluss auf die Renderingleistung. Verwenden Sie sie mit Bedacht und nur dann, wenn es die Anwendung erfordert. Alle statischen globalen Lichtverhältnisse, einschließlich einer statischen gerichteten Komponente, können mit einer [benutzerdefinierten Himmelstextur](sky.md) ohne zusätzliche Renderingkosten erreicht werden.

## <a name="next-steps"></a>Nächste Schritte

* [Materialien](../../concepts/materials.md)
* [Himmel](sky.md)
