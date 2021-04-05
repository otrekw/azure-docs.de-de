---
title: Abrufen von Informationen zu Konvertierungen
description: Abrufen von Informationen zu Konvertierungen
author: malcolmtyrrell
ms.author: matyrr
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: 89ec0ad40822785457e988cf9e0f9bd6d00ed81f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91576624"
---
# <a name="get-information-about-conversions"></a>Abrufen von Informationen zu Konvertierungen

## <a name="information-about-a-conversion-the-result-file"></a>Informationen zu einer Konvertierung: Die Ergebnisdatei

Wenn der Konvertierungsdienst ein Objekt konvertiert, wird eine Zusammenfassung aller Probleme in eine „Ergebnisdatei“ geschrieben. Wenn beispielsweise eine Datei `buggy.gltf` konvertiert wird, enthält der Ausgabecontainer eine Datei namens `buggy.result.json`.

In der Ergebnisdatei sind alle Fehler und Warnungen aufgelistet, die während der Konvertierung aufgetreten sind, sowie eine Ergebniszusammenfassung, in Form von `succeeded`, `failed` oder `succeeded with warnings`.
Die Ergebnisdatei ist als ein JSON-Array von Objekten strukturiert, von denen jedes eine Zeichenfolgeneigenschaft in Form der Eigenschaften `warning`, `error`, `internal warning`, `internal error` bzw. `result` aufweist. Es wird höchstens einen Fehler (entweder `error` oder `internal error`) sowie immer ein `result`-Objekt geben.

## <a name="example-result-file"></a>*Beispielergebnisdatei*

Im folgenden Beispiel wird eine Konvertierung beschrieben, die erfolgreich ein arrAsset-Objekt generiert hat. Da jedoch eine Struktur fehlte, ist das resultierende arrAsset-Objekt möglicherweise nicht so wie beabsichtigt.

```JSON
[
  {"warning":"4004","title":"Missing texture","details":{"texture":"buggy_baseColor.png","material":"buggy_col"}},
  {"result":"succeeded with warnings"}
]
```

## <a name="information-about-a-converted-model-the-info-file"></a>Informationen zu einem konvertierten Modell: Die Infodatei

Die vom Konvertierungsdienst erstellte Medienobjektdatei ist ausschließlich für die Nutzung durch den Renderingdienst vorgesehen. Es kann jedoch vorkommen, dass Sie auf Informationen zu einem Modell zugreifen möchten, ohne eine Renderingsitzung zu starten. Um diesen Workflow zu unterstützen, speichert der Konvertierungsdienst eine JSON-Datei neben der arrAsset-Datei im Ausgabecontainer. Wenn beispielsweise eine Datei `buggy.gltf` konvertiert wird, enthält der Ausgabecontainer eine Datei namens `buggy.info.json` neben dem konvertierten `buggy.arrAsset`-Medienobjekt. Sie enthält Informationen zum Quellmodell, zum konvertierten Modell und zur Konvertierung selbst.

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
    "materialOverrides": {
        "numOverrides": 4,
        "numOverriddenMaterials": 4
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

### <a name="the-materialoverrides-section"></a>Der Abschnitt *materialOverrides*

In diesem Abschnitt erhalten Sie Informationen zum [Überschreiben von Materialien](override-materials.md), wenn eine Materialüberschreibungsdatei für den Konvertierungsdienst bereitgestellt wurde.
Es enthält die folgenden Informationen:
* `numOverrides`: Die Anzahl der Überschreibungseinträge, die aus der Materialüberschreibungsdatei gelesen wurde
* `numOverriddenMaterials`: Die Anzahl der Materialien, die überschrieben wurden

### <a name="the-inputstatistics-section"></a>Abschnitt *inputStatistics*

In diesem Abschnitt finden Sie Informationen zur Quellszene. Häufig gibt es Abweichungen zwischen den Werten in diesem Abschnitt und den entsprechenden Werten in dem Tool, das das Quellmodell erstellt hat. Diese Unterschiede werden erwartet, da das Modell während der Export- und Konvertierungsschritte geändert wird.

* `numMeshes`: Die Anzahl der Gittermodellteile, wobei jedes Teil auf ein einzelnes Material verweisen kann.
* `numFaces`: Die Gesamtanzahl der _Dreiecke_ im gesamten Modell. Beachten Sie, dass das Gittermodell während der Konvertierung in Dreiecke zerlegt wird. Diese Anzahl trägt zum Polygongrenzwert in der [Serverstandardgröße zum Rendern](../../reference/vm-sizes.md#how-the-renderer-evaluates-the-number-of-polygons) bei.
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

## <a name="deprecated-features"></a>Veraltete Features

Der Konvertierungsdienst schreibt die Dateien `stdout.txt` und `stderr.txt` in den Ausgabecontainer, und diese waren die einzige Quelle für Warnungen und Fehler.
Diese Dateien sind jetzt veraltet. Verwenden Sie stattdessen [Ergebnisdateien](#information-about-a-conversion-the-result-file) zu diesem Zweck.

## <a name="next-steps"></a>Nächste Schritte

* [Modellkonvertierung](model-conversion.md)
* [Konfigurieren der Modellkonvertierung](configure-model-conversion.md)
