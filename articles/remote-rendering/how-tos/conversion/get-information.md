---
title: Abrufen von Informationen zu einem konvertierten Modell
description: Beschreibung aller Parameter der Modellkonvertierung.
author: malcolmtyrrell
ms.author: matyrr
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: d5f843add0649682bae8c472bc50b6beea33bf93
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679300"
---
# <a name="get-information-about-a-converted-model"></a>Abrufen von Informationen zu einem konvertierten Modell

Die vom Konvertierungsdienst erstellte Medienobjektdatei ist ausschließlich für die Nutzung durch den Renderingdienst vorgesehen. Es kann jedoch vorkommen, dass Sie auf Informationen zu einem Modell zugreifen möchten, ohne eine Renderingsitzung zu starten. Daher speichert der Konvertierungsdienst eine JSON-Datei neben der arrAsset-Datei im Ausgabecontainer. Wenn beispielsweise eine Datei `buggy.gltf` konvertiert wird, enthält der Ausgabecontainer eine Datei namens `buggy.info.json` neben dem konvertierten `buggy.arrAsset`-Medienobjekt. Sie enthält Informationen zum Quellmodell, zum konvertierten Modell und zur Konvertierung selbst.

## <a name="example-info-file"></a>Beispiel für eine Datei *info*

Dies ist ein Beispiel für eine Datei *info*, die durch das Konvertieren einer Datei namens `buggy.gltf`generiert wird:

```JSON
{
    "files": {
        "input": "Buggy.gltf"
    },
    "conversionSettings": {
        "recenterToOrigin": true
    },
    "inputInfo": {
        "sourceAssetExtension": ".gltf",
        "sourceAssetFormat": "glTF2 Importer",
        "sourceAssetFormatVersion": "2.0",
        "sourceAssetGenerator": "COLLADA2GLTF"
    },
    "inputStatistics": {
        "numMeshes": 148,
        "numFaces": 308306,
        "numVertices": 245673,
        "numMaterial": 149,
        "numFacesSmallestMesh": 2,
        "numFacesBiggestMesh": 8764,
        "numNodes": 206,
        "numMeshUsagesInScene": 236,
        "maxNodeDepth": 3
    },
    "outputInfo": {
        "conversionToolVersion": "3b28d840de9916f9d628342f474d38c3ab949590",
        "conversionHash": "CCDB1F7A4C09F565"
    },
    "outputStatistics": {
        "numMeshPartsCreated": 236,
        "numMeshPartsInstanced": 88,
        "recenteringOffset": [
            -24.1,
            -50.9,
            -16.5974
        ],
        "boundingBox": {
            "min": [
                -43.52,
                -61.775,
                -79.6416
            ],
            "max": [
                43.52,
                61.775,
                79.6416
            ]
        }
    }
}
```

## <a name="information-in-the-info-file"></a>Informationen in der Datei „info“

### <a name="the-files-section"></a>Abschnitt *files* (Dateien)

Dieser Abschnitt enthält die angegebenen Dateinamen.

* `input`: Der Name der Quelldatei.
* `output`: Der Name der Ausgabedatei, wenn der Benutzer einen nicht standardmäßigen Namen angegeben hat.

### <a name="the-conversionsettings-section"></a>Abschnitt *conversionSettings*

Dieser Abschnitt enthält eine Kopie der [conversionSettings](configure-model-conversion.md#settings-file), die beim Konvertieren des Modells angegeben wurden.

### <a name="the-inputinfo-section"></a>Abschnitt *inputInfo*

In diesem Abschnitt werden Informationen zum Quelldateiformat aufgezeichnet.

* `sourceAssetExtension`: Die Dateierweiterung der Quelldatei.
* `sourceAssetFormat`: Eine Beschreibung des Quelldateiformats.
* `sourceAssetFormatVersion`: Die Version des Quelldateiformats.
* `sourceAssetGenerator`: Der Name des Tools, das die Quelldatei generiert hat (falls verfügbar).

### <a name="the-inputstatistics-section"></a>Abschnitt *inputStatistics*

In diesem Abschnitt finden Sie Informationen zur Quellszene. Häufig gibt es Abweichungen zwischen den Werten in diesem Abschnitt und den entsprechenden Werten in dem Tool, das das Quellmodell erstellt hat. Diese Unterschiede werden erwartet, da das Modell während der Export- und Konvertierungsschritte geändert wird.

* `numMeshes`: Die Anzahl der Gittermodellteile, wobei jedes Teil auf ein einzelnes Material verweisen kann.
* `numFaces`: Die Gesamtanzahl der _Dreiecke_ im gesamten Modell. Beachten Sie, dass das Gittermodell während der Konvertierung in Dreiecke zerlegt wird.
* `numVertices`: Die Gesamtanzahl der Eckpunkte im gesamten Modell.
* `numMaterial`: Die Gesamtanzahl der Materialien im gesamten Modell.
* `numFacesSmallestMesh`: Die Anzahl der Dreiecke im kleinsten Gittermodell des Modells.
* `numFacesBiggestMesh`: Die Anzahl der Dreiecke im größten Gittermodell des Modells.
* `numNodes`: Die Anzahl der Knoten im Szenegraphen des Modells.
* `numMeshUsagesInScene`: Die Anzahl der Verweise von Knoten auf Gittermodelle. Mehrere Knoten können auf dasselbe Gittermodell verweisen.
* `maxNodeDepth`: Die maximale Tiefe der Knoten innerhalb des Szenegraphen.

### <a name="the-outputinfo-section"></a>Abschnitt *outputInfo*

In diesem Abschnitt werden allgemeine Informationen zur generierten Ausgabe aufgezeichnet.

* `conversionToolVersion`: Die Version des Modellkonverters.
* `conversionHash`: Ein Hash der Daten in arrAsset, die zum Rendering beitragen können. Kann verwendet werden, um zu verstehen, ob der Konvertierungsdienst bei der erneuten Ausführung für dieselbe Datei ein anderes Ergebnis generiert hat.

### <a name="the-outputstatistics-section"></a>Abschnitt *outputStatistics*

In diesem Abschnitt werden die vom konvertierten Medienobjekt berechneten Informationen aufgezeichnet.

* `numMeshPartsCreated`: Die Anzahl der Gittermodelle in arrAsset. Sie kann sich von `numMeshes` im Abschnitt `inputStatistics` unterscheiden, da die Instanziierung durch den Konvertierungsprozess beeinflusst wird.
* `numMeshPartsInstanced`: Die Anzahl der Gittermodelle, die in arrAsset wiederverwendet werden.
* `recenteringOffset`: Wenn die Option `recenterToOrigin` in den [ConversionSettings](configure-model-conversion.md) aktiviert ist, ist dieser Wert die Übersetzung, die das konvertierte Modell zurück an seine ursprüngliche Position verschieben würde.
* `boundingBox`: Die Begrenzungen des Modells.

## <a name="next-steps"></a>Nächste Schritte

* [Modellkonvertierung](model-conversion.md)
* [Konfigurieren der Modellkonvertierung](configure-model-conversion.md)
