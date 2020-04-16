---
title: Himmelsreflexionen
description: In diesem Artikel wird beschrieben, wie Sie Environment Maps für Himmelsspiegelungen einrichten.
author: florianborn71
ms.author: flborn
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: 7316df7bcf78e3a154510e69116c288b2b293d4c
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679020"
---
# <a name="sky-reflections"></a>Himmelsreflexionen

In Azure Remote Rendering wird eine Himmelstextur verwendet, um Objekte realistisch zu beleuchten. In Augmented-Reality-Anwendungen sollte diese Textur der realen Umgebung ähneln, damit die Objekte überzeugend echt erscheinen. In diesem Artikel wird beschrieben, wie Sie die Himmelstextur ändern.

> [!NOTE]
> Die Himmelstextur wird auch als *Environment Map* bezeichnet. Beide Begriffe werden synonym verwendet.

## <a name="object-lighting"></a>Objektbeleuchtung

In Azure Remote Rendering werden mithilfe von *Physically Based Rendering* (PBR) realistische Lichteffekte berechnet. Obwohl Sie auch [Lichtquellen](lights.md) zu Ihrer Szene hinzufügen können, ist die Verwendung einer guten Himmelstextur die effektivste Methode.

Die folgenden Bilder zeigen, was geschieht, wenn verschiedene Oberflächen nur mit einer Himmeltextur beleuchtet werden:

| Rauheit  | 0                                        | 0,25                                          | 0.5                                          | 0,75                                          | 1                                          |
|:----------:|:----------------------------------------:|:---------------------------------------------:|:--------------------------------------------:|:---------------------------------------------:|:------------------------------------------:|
| Nicht metallisch  | ![Dielektrikum0](media/dielectric-0.png)   | ![GreenPointPark](media/dielectric-0.25.png)  | ![GreenPointPark](media/dielectric-0.5.png)  | ![GreenPointPark](media/dielectric-0.75.png)  | ![GreenPointPark](media/dielectric-1.png)  |
| Metallisch      | ![GreenPointPark](media/metallic-0.png)  | ![GreenPointPark](media/metallic-0.25.png)    | ![GreenPointPark](media/metallic-0.5.png)    | ![GreenPointPark](media/metallic-0.75.png)    | ![GreenPointPark](media/metallic-1.png)    |

Weitere Informationen zum Belichtungsmodell finden Sie im Kapitel [Materialien](../../concepts/materials.md).

> [!IMPORTANT]
> Himmelstexturen werden in Azure Remote Rendering nur für Belichtungsmodelle verwendet. Damit lässt sich nicht der Himmel als Hintergrund rendern, da Augmented-Reality-Anwendungen bereits über einen angemessenen Hintergrund verfügen: die echte Welt.

## <a name="changing-the-sky-texture"></a>Ändern der Himmelstextur

Zum Ändern der Environment Map müssen Sie lediglich [eine Textur laden](../../concepts/textures.md) und das `SkyReflectionSettings`-Objekt der Sitzung ändern:

``` cs
LoadTextureAsync _skyTextureLoad = null;
void ChangeEnvironmentMap(AzureSession session)
{
    _skyTextureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap));

    _skyTextureLoad.Completed += (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                try
                {
                    session.Actions.SkyReflectionSettings.SkyReflectionTexture = res.Result;
                }
                catch (RRException exception)
                {
                    System.Console.WriteLine($"Setting sky reflection failed: {exception.Message}");
                }
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
        };
}
```

Beachten Sie, dass oben die Variante `LoadTextureFromSASAsync` verwendet wird, da eine integrierte Textur geladen wird. Wenn Sie Daten aus [verknüpften Blobspeichern](../../how-tos/create-an-account.md#link-storage-accounts) laden, verwenden Sie die Variante `LoadTextureAsync`.

## <a name="sky-texture-types"></a>Typen von Himmelstexturen

Sie können sowohl *[Cube Maps](https://en.wikipedia.org/wiki/Cube_mapping)* als auch *2D-Texturen* als Environment Map verwenden.

Alle Texturen müssen in einem [unterstützten Texturformat](../../concepts/textures.md#supported-texture-formats) vorliegen. Für Himmelstexturen müssen Sie keine MipMaps bereitstellen.

### <a name="cube-environment-maps"></a>Cube-Environment-Maps

Dies ist ein Beispiel für eine aufgeklappte Cube Map:

![Aufgeklappte Cube Map](media/Cubemap-example.png)

Verwenden Sie `AzureSession.Actions.LoadTextureAsync`/ `LoadTextureFromSASAsync` mit `TextureType.CubeMap`, um Cube-Map-Texturen zu laden.

### <a name="sphere-environment-maps"></a>Sphärische Environment Maps

Bei einer 2D-Textur als Environment Map muss sich das Bild in einem [sphärischen Koordinatenraum](https://en.wikipedia.org/wiki/Spherical_coordinate_system) befinden.

![Ein Himmelsbild in einem sphärischen Koordinatenraum](media/spheremap-example.png)

Verwenden Sie `AzureSession.Actions.LoadTextureAsync` mit `TextureType.Texture2D`, um sphärische Environment Maps zu laden.

## <a name="built-in-environment-maps"></a>Integrierte Environment Maps

Azure Remote Rendering bietet einige integrierte Environment Maps, die immer verfügbar sind. Alle integrierten Environment Maps sind Cube Maps.

|Bezeichner                         | BESCHREIBUNG                                              | Abbildung                                                      |
|-----------------------------------|:---------------------------------------------------------|:-----------------------------------------------------------------:|
|builtin://Autoshop                 | Vielfalt von Lichtstreifen, helle Innenraumgrundbeleuchtung    | ![Autoshop](media/autoshop.png)
|builtin://BoilerRoom               | Helle Innenraum-Lichtszene, mehrere Fensterlichter      | ![BoilerRoom](media/boiler-room.png)
|builtin://ColorfulStudio           | Unterschiedlich farbige Lichter in einer mittelhellen Innenraum-Lichtszene  | ![ColorfulStudio](media/colorful-studio.png)
|builtin://Hangar                   | Mäßig helles Umgebungsflurlicht                     | ![SmallHangar](media/hangar.png)
|builtin://IndustrialPipeAndValve   | Gedimmte Innenraum-Lichtszene mit einem Hell-Dunkel-Kontrast              | ![IndustrialPipeAndValve](media/industrial-pipe-and-valve.png)
|builtin://Lebombo                  | Tagesumgebungslicht, Innenraum, helles Fensterlicht     | ![Lebombo](media/lebombo.png)
|builtin://SataraNight              | Dunkler Nachthimmel und Boden mit vielen umgebenden Lichtern   | ![SataraNight](media/satara-night.png)
|builtin://SunnyVondelpark          | Kontrast zwischen hellem Sonnenlicht und Schatten                      | ![SunnyVondelpark](media/sunny-vondelpark.png)
|builtin://Syferfontein             | Klares Himmelslicht mit mäßiger Bodenbeleuchtung            | ![Syferfontein](media/syferfontein.png)
|builtin://TearsOfSteelBridge       | Moderater Übergang zwischen Licht (Sonne) und Schatten                         | ![TearsOfSteelBridge](media/tears-of-steel-bridge.png)
|builtin://VeniceSunset             | Licht des Sonnenuntergangs am Abend, fast Dämmerung                    | ![VeniceSunset](media/venice-sunset.png)
|builtin://WhippleCreekRegionalPark | Helle, üppig grüne und weiße helle Töne, dunklerer Boden | ![WhippleCreekRegionalPark](media/whipple-creek-regional-park.png)
|builtin://WinterRiver              | Tagsüber, helles Umgebungsbodenlicht                 | ![WinterRiver](media/winter-river.png)
|builtin://DefaultSky               | Identisch mit TearsOfSteelBridge                               | ![DefaultSky](media/tears-of-steel-bridge.png)

## <a name="next-steps"></a>Nächste Schritte

* [Lichter](../../overview/features/lights.md)
* [Materialien](../../concepts/materials.md)
* [Texturen](../../concepts/textures.md)
* [TexConv – das Texturkonvertierungstool](../../resources/tools/tex-conv.md)
