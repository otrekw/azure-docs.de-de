---
title: Überschreiben von Materialien während der Modellkonvertierung
description: Erläutert den Materialüberschreibungsworkflow zur Konvertierungszeit.
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: how-to
ms.openlocfilehash: 90653db4c572877a728964851a99beebf2e823a4
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679288"
---
# <a name="override-materials-during-model-conversion"></a>Überschreiben von Materialien während der Modellkonvertierung

Während der Konvertierung werden die Materialeinstellungen im Quellmodell verwendet, um die [PBR-Materialien](../../overview/features/pbr-materials.md) zu definieren, die vom Renderer verwendet werden.
Manchmal gibt die [Standardkonvertierung](../../reference/material-mapping.md) nicht die gewünschten Ergebnisse aus, und Sie müssen Änderungen vornehmen.
Wenn ein Modell für die Verwendung in Azure Remote Rendering konvertiert wird, können Sie eine Materialüberschreibungsdatei bereitstellen, um anzupassen, wie die Materialkonvertierung pro Material durchgeführt wird.
Der Abschnitt zum [Konfigurieren der Modellkonvertierung](configure-model-conversion.md) enthält Anweisungen zum Deklarieren des Namens der Materialüberschreibungsdatei.

## <a name="the-override-file-used-during-conversion"></a>Die während der Konvertierung verwendete Überschreibungsdatei

Angenommen, ein Boxmodell verfügt über ein einzelnes Material mit dem Namen „Default“ (Standard). Die Rückstrahlfarbe muss für die Verwendung in ARR angepasst werden.
In diesem Fall kann eine Datei `box_materials_override.json` wie folgt erstellt werden:

```json
[
    {
        "name": "Default",
        "albedoColor": {
            "r": 0.33,
            "g": 0.33,
            "b": 0.33,
            "a": 1.0
        }
    }
]
```

Die Datei `box_materials_override.json` wird im Eingabecontainer gespeichert, und neben `box.fbx` wird eine Datei `ConversionSettings.json` hinzugefügt, die die Konvertierung informiert, wo die Überschreibungsdatei zu finden ist (siehe [Konfigurieren der Modellkonvertierung](configure-model-conversion.md)):

```json
{
    "material-override" : "box_materials_override.json"
}
```

Wenn das Modell konvertiert wird, werden die neuen Einstellungen angewendet.

### <a name="color-materials"></a>Farbmaterialien

Das [Farbmaterialmodell](../../overview/features/color-materials.md) beschreibt eine ständig beschattete Oberfläche, die unabhängig von der Beleuchtung ist.
Dies ist z. B. nützlich für Medienobjekte, die von Fotogrammmetriealgorithmen erstellt werden.
In Materialüberschreibungsdateien kann ein Material als Farbmaterial deklariert werden, indem `unlit` auf `true` festgelegt wird.

```json
[
    {
        "name": "Photogrametry_mat1",
        "unlit" : true
    },
    {
        "name": "Photogrametry_mat2",
        "unlit" : true
    }
]
```

### <a name="ignore-specific-texture-maps"></a>Ignorieren bestimmter Texturzuordnungen

Manchmal wünschen Sie möglicherweise, dass der Konvertierungsprozess bestimmte Texturzuordnungen ignoriert. Dies kann der Fall sein, wenn das Modell von einem Tool generiert wurde, das spezielle Zuordnungen generiert, die vom Renderer nicht richtig verstanden werden. Beispielsweise eine „OpacityMap“, die verwendet wird, um etwas anderes als Deckkraft zu definieren, oder ein Modell, in dem die „NormalMap“ als „BumpMap“ gespeichert wird. (In letzterem Fall möchten Sie „NormalMap“ ignorieren, was bewirkt, dass der Konverter „BumpMap“ als „NormalMap“ verwendet.)

Das Prinzip ist einfach. Fügen Sie einfach eine Eigenschaft namens `ignoreTextureMaps` hinzu, und fügen Sie eine beliebige Texturzuordnung hinzu, die Sie ignorieren möchten:

```json
[
    {
        "name": "Default",
        "ignoreTextureMaps": ["OpacityMap", "NormalMap"]
    }
]
```

Die vollständige Liste der Texturzuordnungen, die Sie ignorieren können, finden Sie im nachfolgenden JSON-Schema.

## <a name="json-schema"></a>JSON-Schema

Das vollständige JSON-Schema für Materialdateien finden Sie hier. Mit Ausnahme von `unlit` und `ignoreTextureMaps` handelt es sich bei den verfügbaren Eigenschaften um eine Teilmenge der Eigenschaften, die in den Abschnitten zu den [Farbmaterial-](../../overview/features/color-materials.md) und [PBR-Materialmodellen](../../overview/features/pbr-materials.md) beschrieben werden.

```json
{
    "definitions" :
    {
        "color":
        {
            "type" : "object",
            "description" : "Color as 4 components vector",
            "properties":
            {
                "r": {"type":"number"},
                "g": {"type":"number"},
                "b": {"type":"number"},
                "a": {"type":"number"}
            },
            "required": ["r", "g", "b"]
        },
        "alpha":
        {
            "type" : "object",
            "description" : "Alpha channel for color",
            "properties":
            {
                "a": {"type":"number"}
            },
            "required": ["a"]
        },
        "colorOrAlpha":
        {
            "anyOf": [
                {"$ref": "#/definitions/color"},
                {"$ref": "#/definitions/alpha"}
            ]
        },
        "listOfMaps":
        {
            "type": "array",
            "items": {
                "type": "string",
                "enum": ["AlbedoMap",
                            "EmissiveMap",
                            "NormalMap",
                            "OcclusionMap",
                            "RoughnessMap",
                            "MetalnessMap",
                            "ReflectivityMap",
                            "BumpMap",
                            "OpacityMap",
                            "DiffuseMap",
                            "SpecularMap",
                            "ShininessMap",
                            "MetallicRoughnessMap",
                            "SpecularGlossinessMap"]
            }
        }
    },
    "type" : "array",
    "description" : "List of materials to override",
    "items":
    {
        "type" : "object",
        "description" : "List of parameters to override",
        "properties":
        {
            "name": { "type" : "string"},
            "unlit": { "type" : "boolean" },
            "albedoColor": { "$ref": "#/definitions/colorOrAlpha" },
            "roughness": { "type": "number" },
            "metalness": { "type": "number" },
            "transparent": { "type" : "boolean" },
            "alphaClipEnabled": { "type" : "boolean" },
            "alphaClipThreshold": { "type": "number" },
            "useVertexColor": { "type" : "boolean" },
            "isDoubleSided": { "type" : "boolean" },
            "ignoreTextureMaps": { "$ref" : "#/definitions/listOfMaps" }
        },
        "required": ["name"],
        "additionalProperties" : false
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Farbmaterialien](../../overview/features/color-materials.md)
* [PBR-Materialien](../../overview/features/pbr-materials.md)