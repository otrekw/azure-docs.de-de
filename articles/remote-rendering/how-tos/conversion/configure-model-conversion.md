---
title: Konfigurieren der Modellkonvertierung
description: Enthält eine Beschreibung aller Parameter der Modellkonvertierung.
author: florianborn71
ms.author: flborn
ms.date: 03/06/2020
ms.topic: how-to
ms.openlocfilehash: eb287b812c477b2e472c48d7bd8f44574a398bac
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679316"
---
# <a name="configure-the-model-conversion"></a>Konfigurieren der Modellkonvertierung

In diesem Kapitel sind die Optionen für die Modellkonvertierung dokumentiert.

## <a name="settings-file"></a>Einstellungsdatei

Wenn im Eingabecontainer neben dem Eingabemodell eine Datei mit dem Namen `ConversionSettings.json` gefunden wird, wird sie verwendet, um eine weitere Konfiguration für den Prozess zur Modellkonvertierung anzugeben.

Für den Inhalt der Datei sollte das folgende JSON-Schema erfüllt sein:

```json
{
    "$schema" : "http://json-schema.org/schema#",
    "description" : "ARR ConversionSettings Schema",
    "type" : "object",
    "properties" :
    {
        "scaling" : { "type" : "number", "exclusiveMinimum" : 0, "default" : 1.0 },
        "recenterToOrigin" : { "type" : "boolean", "default" : false },
        "opaqueMaterialDefaultSidedness" : { "type" : "string", "enum" : [ "SingleSided", "DoubleSided" ], "default" : "DoubleSided" },
        "material-override" : { "type" : "string", "default" : "" },
        "gammaToLinearMaterial" : { "type" : "boolean", "default" : false },
        "gammaToLinearVertex" : { "type" : "boolean", "default" : false },
        "sceneGraphMode": { "type" : "string", "enum" : [ "none", "static", "dynamic" ], "default" : "dynamic" },
        "generateCollisionMesh" : { "type" : "boolean", "default" : true },
        "unlitMaterials" : { "type" : "boolean", "default" : false },
        "fbxAssumeMetallic" : { "type" : "boolean", "default" : true },
        "axis" : {
            "type" : "array",
            "items" : {
                "type" : "string",
                "enum" : ["default", "+x", "-x", "+y", "-y", "+z", "-z"]
            },
            "minItems": 3,
            "maxItems": 3
        }
    },
    "additionalProperties" : false
}
```

Ein Beispiel für die Datei `ConversionSettings.json` ist:

```json
{
    "scaling" : 0.01,
    "recenterToOrigin" : true,
    "material-override" : "box_materials_override.json"
}
```

### <a name="geometry-parameters"></a>Geometrieparameter

* `scaling`: Mit diesem Parameter wird ein Modell einheitlich skaliert. Die Skalierung kann genutzt werden, um ein Modell zu vergrößern oder zu verkleinern, z. B. zum Anzeigen eines Gebäudemodells auf einer Tischplatte. Da das Renderingmodul die Angabe von Längenwerten in Metern erwartet, ist die Verwendung dieses Parameters auch wichtig, wenn ein Modell mit anderen Einheiten definiert wurde. Wenn zum Definieren eines Modells beispielsweise Zentimeter verwendet wurden, sollte es in der richtigen Größe gerendert werden, wenn eine Skalierung mit dem Wert 0,01 angewendet wird.
Bei einigen Quelldatenformaten (z. B. FBX) wird auf die Einheitenskalierung hingewiesen. In diesem Fall wird das Modell bei der Konvertierung implizit auf „Meter“ als Einheit skaliert. Die implizite Skalierung durch das Quellformat wird zusätzlich zum Skalierungsparameter angewendet.
Der abschließende Skalierungsfaktor wird auf die Scheitelpunkte der Geometrie und die lokalen Transformationen der Szenengraphknoten angewendet. Die Skalierung für die Transformation der Stammentität bleibt unverändert.

* `recenterToOrigin`: Gibt an, dass ein Modell so konvertiert werden sollte, dass der Begrenzungsrahmen am Ursprung zentriert ist.
Die Zentrierung ist wichtig, wenn das Quellmodell gegenüber dem Ursprung weit versetzt wurde. In diesem Fall können Probleme mit der Gleitkommagenauigkeit nämlich zu Artefakten beim Rendering führen.

* `opaqueMaterialDefaultSidedness`: Vom Renderingmodul wird vorausgesetzt, dass undurchsichtige Materialien doppelseitig sind.
Wenn dies nicht das beabsichtigte Verhalten ist, sollte dieser Parameter auf „SingleSided“ (Einseitig) festgelegt werden. Weitere Informationen finden Sie unter [Einseitiges Rendering](../../overview/features/single-sided-rendering.md).

### <a name="material-overrides"></a>Materialüberschreibungen

* `material-override`: Dieser Parameter ermöglicht die Verarbeitung von Materialien, damit sie [während der Konvertierung angepasst werden können](override-materials.md).

### <a name="color-space-parameters"></a>Farbraumparameter

Vom Renderingmodul wird erwartet, dass sich die Farbwerte im linearen Raum befinden.
Wenn ein Modell mit dem Gamma-Raum definiert wird, sollten diese Optionen auf „true“ festgelegt werden.

* `gammaToLinearMaterial`: Dient zum Konvertieren der Materialfarben aus dem Gamma-Raum in den linearen Raum.
* `gammaToLinearVertex`: Dient zum Konvertieren der Scheitelpunktfarben aus dem Gamma-Raum in den linearen Raum.

> [!NOTE]
> Für FBX-Dateien sind diese Einstellungen standardmäßig auf `true` festgelegt. Für alle anderen Dateitypen wird `false` als Standardwert verwendet.

### <a name="scene-parameters"></a>Szenenparameter

* `sceneGraphMode`: Definiert, wie der Szenengraph in der Quelldatei konvertiert wird:
  * `dynamic` (Standard): Alle Objekte in der Datei werden als [Entitäten](../../concepts/entities.md) in der API verfügbar gemacht und können unabhängig voneinander transformiert werden. Die Knotenhierarchie zur Runtime ist mit der Struktur in der Quelldatei identisch.
  * `static`: Alle Objekte werden in der API verfügbar gemacht, aber sie können nicht unabhängig voneinander transformiert werden.
  * `none`: Der Szenengraph wird zu einem Objekt reduziert.

Jeder Modus weist eine andere Runtimeleistung auf. Im Modus `dynamic` ist der Leistungsaufwand linear von der Anzahl von [Entitäten](../../concepts/entities.md) im Graphen abhängig. Dies gilt auch, wenn kein Teil verschoben wird. Er sollte nur genutzt werden, wenn für die Anwendung das separate Verschieben von Teilen erforderlich ist, z. B. für eine Animation vom Typ „Explosionsansicht“.

Im Modus `static` wird der vollständige Szenengraph exportiert, aber die Teile dieses Graphen verfügen über eine konstante Transformation relativ zum Stammteil. Der Stammknoten des Objekts kann aber weiterhin ohne größeren Leistungsaufwand verschoben, gedreht oder skaliert werden. Darüber hinaus werden bei [räumlichen Abfragen](../../overview/features/spatial-queries.md) einzelne Teile zurückgegeben, und jedes Teil kann anhand von [Zustandsüberschreibungen](../../overview/features/override-hierarchical-state.md) geändert werden. In diesem Modus ist der Runtime-Mehraufwand pro Objekt vernachlässigbar. Er eignet sich ideal für große Szenen, in denen Sie weiterhin Untersuchungen pro Objekt benötigen, aber keine Transformationsänderungen für einzelne Objekte durchführen.

Im Modus `none` fällt der geringste Runtime-Mehraufwand an, und es werden etwas kürzere Ladedauern erreicht. Die Überprüfung oder Transformation einzelner Objekte ist in diesem Modus nicht möglich. Ein Beispiel für einen Anwendungsfall sind Fotogrammetrie-Modelle, die gar nicht über einen aussagekräftigen Szenengraphen verfügen.

> [!TIP]
> Bei vielen Anwendungen werden mehrere Modelle geladen. Sie sollten die Konvertierungsparameter für jedes Modell je nach Nutzung optimieren. Wenn Sie beispielsweise ein PKW-Modell anzeigen möchten, das vom Benutzer auseinandergenommen und im Detail untersucht werden kann, müssen Sie es im Modus `dynamic` konvertieren. Falls Sie den PKW zusätzlich in einem Ausstellungsraum platzieren möchten, können Sie das Modell mit der Einstellung `static` oder auch `none` für `sceneGraphMode` konvertieren.

### <a name="physics-parameters"></a>Physikparameter

* `generateCollisionMesh`: Falls Sie für ein Modell Unterstützung für [räumliche Abfragen](../../overview/features/spatial-queries.md) benötigen, muss diese Option aktiviert sein. Im schlimmsten Fall kann es bei der Erstellung eines Kollisionsgittermodells zu einer Verdoppelung der Konvertierungsdauer kommen. Das Laden von Modellen mit Kollisionsgittermodellen dauert länger, und bei Verwendung eines Szenengraphen vom Typ `dynamic` fällt auch mehr Aufwand in Bezug auf die Runtimeleistung an. Zur Erzielung einer optimalen Gesamtleistung sollten Sie diese Option für alle Modelle deaktivieren, für die Sie keine räumlichen Abfragen benötigen.

### <a name="unlit-materials"></a>Unbeleuchtete Materialien

* `unlitMaterials`: Bei der Konvertierung wird standardmäßig versucht, [PBR-Materialien](../../overview/features/pbr-materials.md) zu erstellen. Bei dieser Option wird das Konvertierungsmodul angewiesen, alle Materialien stattdessen als [Farbmaterialien](../../overview/features/color-materials.md) zu behandeln. Falls Sie bereits über Daten mit Beleuchtung verfügen, z. B. bei Modellen auf Grundlage der Fotogrammetrie, können Sie mit dieser Option schnell die richtige Konvertierung für alle Materialien erzwingen, ohne dass eine [Überschreibung für jedes Material](override-materials.md) einzeln durchgeführt werden muss.

### <a name="converting-from-older-fbx-formats-with-a-phong-material-model"></a>Konvertieren aus älteren FBX-Formaten mit einem Phong-Materialmodell

* `fbxAssumeMetallic`: Bei älteren Versionen des FBX-Formats werden die Materialien mit einem Phong-Materialmodell definiert. Beim Konvertierungsprozess muss abgeleitet werden, wie diese dem [PBR-Modell](../../overview/features/pbr-materials.md) des Renderers zugeordnet sind. Normalerweise funktioniert dies gut. Es kann aber zu Mehrdeutigkeiten kommen, wenn ein Material keine Texturen aufweist und über hohe Glanzwerte und eine andere Albedofarbe als Grau verfügt. Unter diesen Umständen muss bei der Konvertierung zwischen den folgenden Optionen gewählt werden: der Priorisierung der hohen Glanzwerte, der Definition eines stark reflektierenden metallischen Materials, bei dem sich die Albedofarbe quasi auflöst, oder der Priorisierung der Albedofarbe, indem beispielsweise ein glänzender farbiger Kunststoff definiert wird. Standardmäßig wird beim Konvertierungsprozess vorausgesetzt, dass hohe Glanzwerte in Fällen, in denen Mehrdeutigkeit besteht, ein metallisches Material implizieren. Dieser Parameter kann auf `false` festgelegt werden, um dies umzukehren.

### <a name="coordinate-system-overriding"></a>Überschreibung des Koordinatensystems

* `axis`: Dient zum Außerkraftsetzen der Einheitenvektoren des Koordinatensystems. Die Standardwerte lauten wie folgt: `["+x", "+y", "+z"]`. Theoretisch verfügt das FBX-Format über einen Header, in dem diese Vektoren definiert sind, und diese Informationen werden vom Konvertierungsmodul zum Transformieren der Szene verwendet. Vom glTF-Format wird auch ein festes Koordinatensystem definiert. In der Praxis enthalten einige Objekte entweder fehlerhafte Informationen im Header oder wurden mit einer anderen Koordinatensystemkonvention gespeichert. Mit dieser Option können Sie das Koordinatensystem außer Kraft setzen, um dies zu lösen. Beispiel: Mit `"axis" : ["+x", "+z", "-y"]` werden die Z- und die Y-Achse vertauscht, und die „Händigkeit“ des Koordinatensystems wird beibehalten, indem die Richtung der Y-Achse invertiert wird.

### <a name="vertex-format"></a>Scheitelpunktformat

Es ist möglich, das Scheitelpunktformat für ein Gittermodell anzupassen, um auf Kosten der Genauigkeit Arbeitsspeicher zu sparen. Ein geringerer Speicherbedarf ermöglicht Ihnen das Laden größerer Modelle oder das Erzielen einer besseren Leistung. Je nach Ihren Daten kann ein falsches Format aber erhebliche Auswirkungen auf die Renderingqualität haben.

> [!CAUTION]
> Eine Änderung des Scheitelpunktformats sollte nur das letzte Mittel sein, wenn im Speicher für die Modelle nicht mehr genug Platz ist oder wenn die höchstmögliche Leistung erreicht werden soll. Durch Änderungen kann es leicht zu Artefakten beim Rendern kommen – sowohl offensichtlicher als auch subtiler Art. Wenn Sie nicht wissen, worauf Sie achten müssen, sollten Sie die Standardeinstellung nicht ändern.

Die folgenden Anpassungen sind möglich:

* Bestimmte Datenströme können explizit eingebunden oder ausgeschlossen werden.
* Die Genauigkeit von Datenströmen kann verringert werden, um den Speicherbedarf zu reduzieren.

Der folgende `vertex`-Abschnitt in der `.json`-Datei ist optional. Für jeden Teil, der nicht explizit angegeben ist, greift der Konvertierungsdienst auf die Standardeinstellung zurück.

```json
{
    ...
    "vertex" : {
        "position"  : "32_32_32_FLOAT",
        "color0"    : "NONE",
        "color1"    : "NONE",
        "normal"    : "NONE",
        "tangent"   : "NONE",
        "binormal"  : "NONE",
        "texcoord0" : "32_32_FLOAT",
        "texcoord1" : "NONE"
    },
    ...
```

Wenn Sie für eine Komponente die Einstellung `NONE` erzwingen, ist sichergestellt, dass das Ausgabegittermodell nicht über den entsprechenden Datenstrom verfügt.

#### <a name="component-formats-per-vertex-stream"></a>Komponentenformate pro Scheitelpunkt-Datenstrom

Die folgenden Formate sind für die jeweiligen Komponenten zulässig:

| Scheitelpunktkomponente | Unterstützte Formate (Fett = Standardeinstellung) |
|:-----------------|:------------------|
|position| **32_32_32_FLOAT**, 16_16_16_16_FLOAT |
|color0| **8_8_8_8_UNSIGNED_NORMALIZED**, NONE |
|color1| 8_8_8_8_UNSIGNED_NORMALIZED, **NONE**|
|normal| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, NONE |
|tangent| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, NONE |
|binormal| **8_8_8_8_SIGNED_NORMALIZED**, 16_16_16_16_FLOAT, NONE |
|texcoord0| **32_32_FLOAT**, 16_16_FLOAT, NONE |
|texcoord1| **32_32_FLOAT**, 16_16_FLOAT, NONE |

#### <a name="supported-component-formats"></a>Unterstützte Komponentenformate

Für die Formate gilt der folgende Speicherbedarf:

| Format | BESCHREIBUNG | Byte pro Scheitelpunkt |
|:-------|:------------|:---------------|
|32_32_FLOAT|Vollständige Gleitkommagenauigkeit mit zwei Komponenten|8
|16_16_FLOAT|Halbe Gleitkommagenauigkeit mit zwei Komponenten|4
|32_32_32_FLOAT|Vollständige Gleitkommagenauigkeit mit drei Komponenten|12
|16_16_16_16_FLOAT|Halbe Gleitkommagenauigkeit mit vier Komponenten|8
|8_8_8_8_UNSIGNED_NORMALIZED|Byte mit vier Komponenten, auf Bereich `[0; 1]` normalisiert|4
|8_8_8_8_SIGNED_NORMALIZED|Byte mit vier Komponenten, auf Bereich `[-1; 1]` normalisiert|4

#### <a name="best-practices-for-component-format-changes"></a>Bewährte Methoden für Änderungen des Komponentenformats

* `position`: Die geringere Genauigkeit reicht nur in seltenen Fällen aus. Mit **16_16_16_16_FLOAT** kommt es auch bei kleinen Modellen zu erkennbaren Quantisierungartefakten.
* `normal`, `tangent`, `binormal`: Diese Werte werden normalerweise zusammen geändert. Sofern es bei der normalen Quantisierung nicht zu erkennbaren Beleuchtungsartefakten kommt, gibt es keinen Grund, die Genauigkeit hierfür zu erhöhen. In einigen Fällen können diese Komponenten aber auf **NONE** festgelegt werden:
  * `normal`, `tangent` und `binormal` werden nur benötigt, wenn mindestens ein Material des Modells beleuchtet sein soll. Bei ARR ist dies der Fall, wenn für das Modell zu irgendeinem Zeitpunkt ein [PBR-Material](../../overview/features/pbr-materials.md) verwendet wird.
  * `tangent` und `binormal` werden nur benötigt, wenn für ein beleuchtetes Material eine normale Mapping-Textur verwendet wird.
* `texcoord0`, `texcoord1`: Für Texturkoordinaten kann eine reduzierte Genauigkeit (**16_16_FLOAT**) verwendet werden, wenn sich die Werte im Bereich `[0; 1]` befinden und die jeweiligen Texturen eine maximale Größe von 2.048 x 2.048 Pixel aufweisen. Bei Überschreitung dieser Grenzwerte leidet die Qualität des Textur-Mappings.

#### <a name="example"></a>Beispiel

Angenommen, Sie verfügen über ein Fotogrammetriemodell, bei dem die Beleuchtung in die Texturen integriert ist. Zum Rendern des Modells werden hierbei nur die Scheitelpunktpositionen und Texturkoordinaten benötigt.

Standardmäßig wird vom Konvertierungsmodul vorausgesetzt, dass Sie für ein Modell ggf. irgendwann PBR-Materialien nutzen. Daher werden für Sie Daten vom Typ `normal`, `tangent` und `binormal` generiert. Die Speichernutzung pro Scheitelpunkt beträgt daher `position` (12 Byte) + `texcoord0` (8 Byte) + `normal` (4 Byte) + `tangent` (4 Byte) + `binormal` (4 Byte) = 32 Byte. Größere Modelle dieses Typs können ohne Weiteres mehrere Millionen Scheitelpunkte aufweisen. Dies führt zu Modellen, die mehrere Gigabyte an Arbeitsspeicher belegen können. Diese hohen Datenmengen wirken sich auf die Leistung aus, und unter Umständen ist nicht genügend Arbeitsspeicher vorhanden.

Da Sie wissen, dass Sie für das Modell niemals eine dynamische Beleuchtung benötigen und dass alle Texturkoordinaten im Bereich `[0; 1]` liegen, können Sie `normal`, `tangent` und `binormal` auf `NONE` und `texcoord0` auf die halbe Genauigkeit (`16_16_FLOAT`) festlegen, sodass sich nur 16 Byte pro Scheitelpunkt ergeben. Indem Sie die Gittermodelldaten halbieren, können Sie größere Modelle laden und ggf. die Leistung steigern.

## <a name="typical-use-cases"></a>Typische Anwendungsfälle

Die Ermittlung von gut geeigneten Importeinstellungen für einen bestimmten Anwendungsfall kann ein aufwendiger Prozess sein. Andererseits können die Konvertierungseinstellungen auch große Auswirkungen auf die Runtimeleistung haben.

Es gibt bestimmte Klassen von Anwendungsfällen, die für bestimmte Optimierungen geeignet sind. Unten sind einige Beispiele angegeben.

### <a name="use-case-architectural-visualization--large-outdoor-maps"></a>Anwendungsfall: Architekturvisualisierung/Große Landschaftskarten

* Diese Arten von Szenen sind meist statischer Art. Dies bedeutet, dass sie keine beweglichen Teile benötigen. `sceneGraphMode` kann daher auf `static` oder auch auf `none` festgelegt werden, um die Runtimeleistung zu verbessern. Im Modus `static` kann der Stammknoten der Szene trotzdem verschoben, gedreht und skaliert werden, um beispielsweise dynamisch zwischen einer 1:1-Skalierung (für die Ich-Perspektive) und einer Darstellung auf einer Tischplatte umzuschalten.

* Beim Verschieben von Teilen bedeutet dies in der Regel auch, dass Sie Unterstützung für Raycasting oder andere [räumliche Abfragen](../../overview/features/spatial-queries.md) benötigen, damit Sie diese Teile überhaupt auswählen können. Falls Sie keine Verschiebungen durchführen möchten, ist die Wahrscheinlichkeit dagegen hoch, dass Sie hierfür auch keine räumlichen Abfragen nutzen müssen. Aus diesem Grund können Sie das Flag `generateCollisionMesh` deaktivieren. Diese Umschaltung hat eine erhebliche Auswirkung die Konvertierungsdauer, die Ladedauer und auch auf den Aktualisierungsaufwand pro Frame während der Runtime.

* Wenn für die Anwendung keine [Schnittebenen](../../overview/features/cut-planes.md) (Cut planes) verwendet werden, sollte das Flag `opaqueMaterialDefaultSidedness` deaktiviert werden. Dies führt normalerweise zu einem Leistungszuwachs von 20 bis 30 %. Schnittebenen können weiter genutzt werden, aber beim Betrachten der inneren Teile eines Objekts sind keine rückwärtigen Flächen vorhanden, die ggf. störend wirken können. Weitere Informationen finden Sie unter [Einseitiges Rendering](../../overview/features/single-sided-rendering.md).

### <a name="use-case-photogrammetry-models"></a>Anwendungsfall: Fotogrammetriemodelle

Beim Rendern von Fotogrammetriemodellen wird normalerweise kein Szenengraph benötigt, sodass Sie `sceneGraphMode` auf `none` festlegen können. Da diese Modelle nur selten über einen komplexen Szenengraphen verfügen, sollte diese Option aber keine größere Auswirkung haben.

Es wird keine dynamische Beleuchtung benötigt, weil sie bereits in die Texturen integriert ist. Deshalb gilt Folgendes:

* Legen Sie das Flag `unlitMaterials` auf `true` fest, um alle Materialien in unbeleuchtete [Farbmaterialien](../../overview/features/color-materials.md) zu verwandeln.
* Entfernen Sie nicht benötigte Daten aus dem Scheitelpunktformat. Sehen Sie sich hierzu das obige [Beispiel](#example) an.

### <a name="use-case-visualization-of-compact-machines-etc"></a>Anwendungsfall: Visualisierung von kompakten Maschinen usw.

Bei diesen Anwendungsfällen weisen die Modelle häufig einen sehr hohen Detailgrad bei kleinem Volumen auf. Der Renderer wurde speziell optimiert, um diese Fälle gut verarbeiten zu können. Die meisten Optimierungen für den vorherigen Anwendungsfall gelten hier aber nicht:

* Da einzelne Teile auswählbar und verschiebbar sein sollten, muss für `sceneGraphMode` die Einstellung `dynamic` beibehalten werden.
* Das Raycasting ist meist integraler Bestandteil der Anwendung, sodass Kollisionsgittermodelle generiert werden müssen.
* Schnittebenen sehen besser aus, wenn das Flag `opaqueMaterialDefaultSidedness` aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte

* [Modellkonvertierung](model-conversion.md)
* [Farbmaterialien](../../overview/features/color-materials.md)
* [PBR-Materialien](../../overview/features/pbr-materials.md)
* [Überschreiben von Materialien während der Modellkonvertierung](override-materials.md)
