---
title: Farbmaterialien
description: Beschreibt den Farbmaterialtyp.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 93fde2d20aaa5b7bac4adc6f1d7fb076569e4bb8
ms.sourcegitcommit: b437bd3b9c9802ec6430d9f078c372c2a411f11f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91893559"
---
# <a name="color-materials"></a>Farbmaterialien

*Farbmaterialien* sind einer der unterstützten [Materialtypen](../../concepts/materials.md) in Azure Remote Rendering. Sie werden für [Gittermodelle](../../concepts/meshes.md) verwendet, die keine Beleuchtung erhalten, sondern jederzeit die höchste Helligkeit aufweisen sollten. Dies kann beispielsweise bei „leuchtenden“ Materialien wie Armaturenbrettern, Glühlampen oder bei Daten der Fall sein, die bereits eine statische Beleuchtung umfassen (z. B. Modelle, die mittels [Fotogrammetrie](https://en.wikipedia.org/wiki/Photogrammetry) erstellt wurden).

Farbmaterialien können aufgrund ihres einfacheren Schattierungsmodells effizienter gerendert werden als [PBR-Materialien](pbr-materials.md). Außerdem unterstützen sie verschiedene Transparenzmodi.

## <a name="common-material-properties"></a>Allgemeine Materialeigenschaften

Diese Eigenschaften gelten für alle Materialien:

* **albedoColor**: Diese Farbe wird mit anderen Farben multipliziert, z. B. mit *albedoMap* oder *:::no-loc text="vertex":::-Farben*. Wenn *transparency* für ein Material aktiviert ist, wird der Alphakanal verwendet, um die Deckkraft anzupassen, wobei `1` vollständig undurchsichtig und `0` vollständig transparent bedeutet. Die Standardfarbe ist Weiß.

  > [!NOTE]
  > Da Farbmaterialien die Umgebung nicht widerspiegeln, wird ein vollständig transparentes Farbmaterial unsichtbar. Dies ist bei [PBR-Materialien](pbr-materials.md) anders.

* **albedoMap**: Eine [2D-Textur](../../concepts/textures.md) für Albedo-Werte pro Pixel.

* **alphaClipEnabled** und **alphaClipThreshold**: Wenn *alphaClipEnabled* „true“ ist, werden alle Pixel, deren Albedo-Alphawert geringer als *alphaClipThreshold* ist, nicht gezeichnet. Alphaclipping kann auch ohne Aktivierung der Transparenz verwendet werden, und es beschleunigt das Rendern. Materialien, auf die Alphaclipping angewendet wird, werden dennoch langsamer gerendert als vollständig undurchsichtige Materialien. Alphaclipping ist standardmäßig deaktiviert.

* **textureCoordinateScale** und **textureCoordinateOffset**: Die Skalierung wird mit den UV-Texturkoordinaten multipliziert, und der Offset wird addiert. Kann zum Strecken und Verschieben der Texturen verwendet werden. Die Standardskalierung ist (1, 1) und der Standardoffset ist (0, 0).

* **useVertexColor**: Wenn das Gittermodell :::no-loc text="vertex":::-Farben enthält und diese Option aktiviert ist, werden die :::no-loc text="vertex":::-Farben des Gittermodells in *albedoColor* und *albedoMap* multipliziert. Standardmäßig ist *useVertexColor* deaktiviert.

* **isDoubleSided**: Wenn Doppelseitigkeit auf „true“ festgelegt ist, werden Dreiecke mit diesem Material auch dann gerendert, wenn die Kamera auf die Rückseite des Materials gerichtet ist. Diese Option ist standardmäßig deaktiviert. Weitere Informationen finden Sie unter [:::no-loc text="Single-sided":::-Rendering](single-sided-rendering.md).

* **TransparencyWritesDepth:** Wenn das TransparencyWritesDepth-Flag für das Material festgelegt und das Material transparent ist, leisten Objekte, die dieses Material verwenden, einen Beitrag zum letzten Tiefenpuffer. Weitere Informationen zur Farbmaterialeigenschaft *transparencyMode* finden Sie im nächsten Abschnitt. Es wird empfohlen, dieses Feature zu aktivieren, wenn Ihr Anwendungsfall einen plausibleren [Farbverschiebungsausgleich](late-stage-reprojection.md) für vollständig transparente Szenen erfordert. Bei Szenen, die teilweise transparent und teilweise nicht transparent sind, kann diese Einstellung zu nicht plausiblem Reprojektionsverhalten oder nicht plausiblen Reprojektionsartefakten führen. Aus diesem Grund wird dieses Flag für normale Anwendungsfälle standardmäßig deaktiviert. Diese Einstellung wird auch empfohlen. Die geschriebenen Tiefenwerte werden der pixelbasierten Tiefenebene des Objekts entnommen, das sich der Kamera am nächsten befindet.

## <a name="color-material-properties"></a>Farbmaterialeigenschaften

Die folgenden Eigenschaften gelten speziell für Farbmaterialien:

* **vertexMix**: Dieser Wert zwischen `0` und `1` gibt an, wie stark die :::no-loc text="vertex":::-Farbe in einem [Gittermodell](../../concepts/meshes.md) zur finalen Farbe beiträgt. Beim Standardwert 1 wird die :::no-loc text="vertex":::-Farbe vollständig in die Albedo-Farbe multipliziert. Beim Wert 0 werden die :::no-loc text="vertex":::-Farben vollständig ignoriert.

* **transparencyMode**: Im Gegensatz zu [PBR-Materialien](pbr-materials.md) wird bei Farbmaterialien zwischen verschiedenen Transparenzmodi unterschieden:

  1. **Opaque**: Im Standardmodus ist die Transparenz deaktiviert. Die Unterdrückung von Alphawerten ist dennoch möglich und sollte vorgezogen werden, sofern ausreichend.
  
  1. **AlphaBlended**: Dieser Modus ähnelt dem Transparenzmodus für PBR-Materialien. Es sollte für durchsichtige Materialien wie Glas verwendet werden.

  1. **Additive**: Dieser Modus ist der einfachste und effizienteste Transparenzmodus. Der Beitrag des Materials wird dem gerenderten Bild hinzugefügt. Dieser Modus kann verwendet werden, um leuchtende (aber dennoch transparente) Objekte zu simulieren, z. B. Marker, die zum Hervorheben wichtiger Objekte verwendet werden.

## <a name="api-documentation"></a>API-Dokumentation

* [C#-Klasse „ColorMaterial“](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.colormaterial)
* [C# RemoteManager.CreateMaterial()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.creatematerial)
* [C++-Klasse „ColorMaterial“](https://docs.microsoft.com/cpp/api/remote-rendering/colormaterial)
* [C++ RemoteManager::CreateMaterial()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#creatematerial)

## <a name="next-steps"></a>Nächste Schritte

* [PBR-Materialien](pbr-materials.md)
* [Texturen](../../concepts/textures.md)
* [Gittermodelle](../../concepts/meshes.md)
