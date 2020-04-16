---
title: Farbmaterialien
description: Beschreibt den Farbmaterialtyp.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 7cbcaefcc087c9f1c7c09668a27fbdef9a4802d3
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679164"
---
# <a name="color-materials"></a>Farbmaterialien

*Farbmaterialien* sind einer der unterstützten [Materialtypen](../../concepts/materials.md) in Azure Remote Rendering. Sie werden für [Gittermodelle](../../concepts/meshes.md) verwendet, die keine Beleuchtung erhalten, sondern jederzeit die höchste Helligkeit aufweisen sollten. Dies kann beispielsweise bei „leuchtenden“ Materialien wie Armaturenbrettern, Glühlampen oder bei Daten der Fall sein, die bereits eine statische Beleuchtung umfassen (z. B. Modelle, die mittels [Fotogrammetrie](https://en.wikipedia.org/wiki/Photogrammetry) erstellt wurden).

Farbmaterialien können aufgrund ihres einfacheren Schattierungsmodells effizienter gerendert werden als [PBR-Materialien](pbr-materials.md). Außerdem unterstützen sie verschiedene Transparenzmodi.

## <a name="common-material-properties"></a>Allgemeine Materialeigenschaften

Diese Eigenschaften gelten für alle Materialien:

* **albedoColor**: Diese Farbe wird mit anderen Farben multipliziert, z. B. mit *albedoMap* oder *Scheitelpunktfarben*. Wenn *Transparenz* für ein Material aktiviert ist, wird der Alphakanal verwendet, um die Deckkraft anzupassen. Dabei bedeutet `1` vollständig undurchsichtig und `0` vollständig transparent. Die Standardfarbe ist Weiß.

  > [!NOTE]
  > Da Farbmaterialien die Umgebung nicht widerspiegeln, wird ein vollständig transparentes Farbmaterial unsichtbar. Dies ist bei [PBR-Materialien](pbr-materials.md) anders.

* **albedoMap**: Eine [2D-Textur](../../concepts/textures.md) für Albedo-Werte pro Pixel.

* **alphaClipEnabled** und **alphaClipThreshold**: Wenn *alphaClipEnabled* „true“ ist, werden alle Pixel, deren Albedo-Alphawert geringer als *alphaClipThreshold* ist, nicht gezeichnet. Die Unterdrückung von Alphawerten kann auch ohne Aktivierung der Transparenz verwendet werden, und sie beschleunigt das Rendern. Materialien mit unterdrückten Alphawerten werden dennoch langsamer gerendert als vollständig undurchsichtige Materialien. Die Unterdrückung von Alphawerten ist standardmäßig deaktiviert.

* **textureCoordinateScale** und **textureCoordinateOffset**: Die Skalierung wird mit den UV-Texturkoordinaten multipliziert, und der Offset wird addiert. Kann zum Strecken und Verschieben der Texturen verwendet werden. Die Standardskalierung ist (1, 1) und der Standardoffset ist (0, 0).

* **useVertexColor**: Wenn das Gittermodell Scheitelpunktfarben enthält und diese Option aktiviert ist, werden die Scheitelpunktfarben des Gittermodells mit *albedoColor* und *albedoMap* multipliziert. Standardmäßig sind Scheitelpunktfarben deaktiviert.

* **isDoubleSided**: Wenn Doppelseitigkeit auf „true“ festgelegt ist, werden Dreiecke mit diesem Material auch dann gerendert, wenn die Kamera auf die Rückseite des Materials gerichtet ist. Diese Option ist standardmäßig deaktiviert. Siehe auch [Einseitiges Rendering](single-sided-rendering.md).

## <a name="color-material-properties"></a>Farbmaterialeigenschaften

Die folgenden Eigenschaften gelten speziell für Farbmaterialien:

* **vertexMix**: Dieser Wert zwischen `0` und `1` gibt an, wie stark die Scheitelpunktfarbe in einem [Gittermodell](../../concepts/meshes.md) zur finalen Farbe beiträgt. Beim Standardwert 1 wird die Scheitelpunktfarbe vollständig in die Albedo-Farbe multipliziert. Beim Wert 0 werden die Scheitelpunktfarben vollständig ignoriert.

* **transparencyMode**: Im Gegensatz zu [PBR-Materialien](pbr-materials.md) wird bei Farbmaterialien zwischen verschiedenen Transparenzmodi unterschieden:

  1. **Opaque**: Im Standardmodus ist die Transparenz deaktiviert. Die Unterdrückung von Alphawerten ist dennoch möglich und sollte vorgezogen werden, sofern ausreichend.
  
  1. **AlphaBlended**: Dieser Modus ähnelt dem Transparenzmodus für PBR-Materialien. Es sollte für durchsichtige Materialien wie Glas verwendet werden.

  1. **Additive**: Dieser Modus ist der einfachste und effizienteste Transparenzmodus. Der Beitrag des Materials wird dem gerenderten Bild hinzugefügt. Dieser Modus kann verwendet werden, um leuchtende (aber dennoch transparente) Objekte zu simulieren, z. B. Marker, die zum Hervorheben wichtiger Objekte verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [PBR-Materialien](pbr-materials.md)
* [Texturen](../../concepts/textures.md)
* [Gittermodelle](../../concepts/meshes.md)
