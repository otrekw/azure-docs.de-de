---
title: Einrichten von PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in Maya
description: Es wird beschrieben, wie Sie PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in Maya einrichten und im FBX-Format exportieren.
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: 5579994b0746a2de4b0f2ca927027ac709940024
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977380"
---
# <a name="tutorial-setting-up-physically-based-rendering-materials-in-maya"></a>Tutorial: Einrichten von PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in Maya

## <a name="overview"></a>Übersicht
In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Zuweisen von Materialien mit erweitertem Beleuchtungsmodell zu Objekten der Szene
> * Verarbeiten der Instanziierung von Objekten und Materialien
> * Exportieren einer Szene im FBX-Format und Auswählen wichtiger Optionen

Die Erstellung von [PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering)](../../overview/features/pbr-materials.md) in `Maya` ist eine relativ einfache Aufgabe. Der Vorgang ähnelt in vielerlei Hinsicht der PBR-Einrichtung in anderen Apps für die Inhaltserstellung, z. B. `3DS Max`. Das folgende Tutorial ist ein Leitfaden zur grundlegenden Einrichtung eines PBR-Shaders und für den FBX-Export für ARR-Projekte. 

Die Beispielszene in diesem Tutorial enthält einige `Polygon Box`-Objekte, denen verschiedene Materialien zugewiesen wurden: Holz, Metall, lackiertes Metall, Kunststoff und Gummi. Jedes Material enthält praktisch alle bzw. die meisten der folgenden Texturen: 

* `Albedo`: Dies ist die Farbzuordnung für die Materialien. Wird auch als `Diffuse` oder `BaseColor` bezeichnet.
* `Metalness`: Bestimmt, ob ein Material metallisch ist bzw. welche Teile metallisch sind. 
* `Roughness`: Bestimmt, wie rau oder glatt eine Oberfläche ist. Dies wirkt sich auf die Schärfe bzw. Unschärfe der Reflexionen und Lichter auf einer Oberfläche aus.
* `Normals`: Dient zum Hinzufügen von Details zu einer Oberfläche, z. B. Grübchen und Kerben auf einer Metalloberfläche oder Faserungen in Holz, ohne dass weitere Polygone hinzugefügt werden müssen.
* `Ambient Occlusion`: Dient zum Hinzufügen von Soft Shading und Kontaktschatten zu einem Modell. Es handelt sich um eine Graustufenzuordnung, mit der angegeben wird, welche Bereiche eines Modells vollständige Beleuchtung (weiß) bzw. vollständigen Schatten (schwarz) erhalten. 

## <a name="prerequisites"></a>Voraussetzungen
* `Autodesk Maya 2017` oder höher

## <a name="setting-up-materials-in-the-scene"></a>Einrichten von Materialien in der Szene
In Maya wird zum Einrichten eines PBR-Materials der folgende Prozess verwendet:

Für den Einstieg haben wir – wie in der Beispielszene zu sehen – einige Objekte vom Typ „Box“ erstellt, die jeweils für eine andere Art von Material stehen. Beachten Sie, dass jedes dieser Objekte mit einem eigenen passenden Namen versehen wurde (wie im Bild unten zu sehen). 

> Ein wichtiger Hinweis, bevor Sie mit dem Erstellen von Medienobjekten für Azure Remote Rendering (ARR) beginnen: Für die Messungen wird „Meter“ als Einheit verwendet, und die Y-Achse zeigt nach oben. Daher ist es ratsam, dass Sie die Einheiten für Szenen in Maya auf „Meter“ festlegen. Darüber hinaus sollten Sie auch beim Exportieren in den Einstellungen für FBX-Exporte „Meter“ wählen. 

> [!TIP]
Es ist eine bewährte Methode, Ihre Modellmedienobjekte entsprechend zu benennen (normalerweise mit Verwendung des relevanten Teils oder Materialtyps), weil Namen die Navigation in Szenen mit vielen Objekten vereinfachen.

![Objektnamen](media/object-names.jpg)

Gehen Sie wie folgt vor, nachdem Sie Ihre Texturen erstellt bzw. erworben haben: Je nach Ihren Anforderungen können Sie einzigartige Texturen für ein Modell in Textur-Apps wie `Quixel Suite`, `Photoshop` oder `Substance Suite` erstellen oder generische Kacheltexturen aus anderen Quellen verwenden. Diese können Sie dann wie folgt auf Ihr Modell anwenden:

* Wählen Sie im Anzeigebereich der Szene Ihr Modell bzw. Ihre Geometrie aus, und klicken Sie mit der rechten Maustaste darauf. Klicken Sie im angezeigten Menü auf `Assign New Material`.
* Navigieren Sie in den Optionen unter `Assign New Material` zu `Maya`>`Stingray PBS`. Bei dieser Aktion wird Ihrem Modell PBR-Material zugewiesen. 

In `Maya 2020` sind mehrere unterschiedliche PBR-Shader verfügbar: `Maya Standard Surface`, `Arnold Standard Surface` und `Stingray PBR`. Der `Maya Standard Surface Shader` kann noch nicht per `FBX plugin 2020` exportiert werden, während dies für den `Arnold Standard Surface Shader` über FBX-Dateien möglich ist. In den meisten anderen Punkten ist er mit dem `Maya Standard Surface Shader` identisch und ist analog zu `Physical Material` in `3D Studio Max`.

**`The Stingray PBR Shader`** ist mit vielen anderen Anwendungen kompatibel und erfüllt am ehesten die Anforderungen von `ARR`. Er wird seit `Maya 2017` unterstützt. Es ist auch ein Vorteil, dass diese Art von Materialien, die im Anzeigebereich visualisiert werden, der späteren Visualisierungsanzeige in ARR ähnelt.

![„Stingray“-Material](media/stingray-material.jpg)

Nachdem Sie Ihr Material Ihrem Medienobjekt zugewiesen und entsprechend benannt haben, können Sie nun verschiedene Texturen zuweisen. In den folgenden Abbildungen ist dargestellt, wie die einzelnen Texturtypen zum PBR-Material passen. Beim Material vom Typ `Stingray PBR` können Sie auswählen, welche Attribute Sie aktivieren können. Bevor Sie für Ihre Texturzuordnungen also den `plug in`-Vorgang durchführen können, müssen Sie die relevanten Attribute aktivieren: 

![Materialeinrichtung](media/material-setup.jpg)

> [!TIP]
Es ist eine bewährte Methode, die Materialien passend zu benennen, indem ihre Verwendung bzw. ihr Typ berücksichtigt wird. Ein Material, das für ein spezielles Teil verwendet werden soll, kann beispielsweise nach diesem Teil benannt werden. Ein Material mit einem größeren Anwendungsbereich sollte dagegen nach seinen Eigenschaften oder seinem Typ benannt werden.

Weisen Sie Ihre Texturen wie folgt zu:

![Textureinrichtung](media/texture-setup.jpg)

Nachdem Sie Ihre PBR-Materialien erstellt und eingerichtet haben, sollten Sie sich über die [Instanziierungsobjekte](../../how-tos/conversion/configure-model-conversion.md#instancing) Ihrer Szene Gedanken machen. Wenn Sie ähnliche bzw. zusammengehörige Objekte Ihrer Szene, z. B. Muttern, Bolzen, Schrauben und Scheiben, instanziieren, können Sie erhebliche Einsparungen bei der Dateigröße erzielen. Instanzen eines Masterobjekts können über eine eigene Skalierungen, Drehungen und Transformationen verfügen und daher je nach Bedarf in Ihrer Szene angeordnet werden. In Maya ist die Instanziierung ein einfacher Prozess.

* Navigieren Sie im Menü `Edit` zu `Duplicate Special`, und öffnen Sie `Options`. 
* Wechseln Sie unter `Duplicate Special` für `Geometry Type` von `Copy` zu `Instance`. 
* Klicken Sie auf `Duplicate Special`.

![Instanziierung](media/instancing.jpg)

Bei dieser Aktion wird eine Instanz Ihres Objekts erstellt, die unabhängig vom übergeordneten Objekt und dessen anderen Instanzen verschoben, gedreht oder skaliert werden kann. 
>Allerdings werden alle Änderungen, die Sie im Komponentenmodus an einer Instanz vornehmen, auf alle Instanzen Ihres Objekts übertragen. Wenn Sie mit den Komponenten eines instanziierten Objekts arbeiten, z. B. Scheitelpunkten, Polygonen, Flächen usw., sollten Sie daher zunächst sicherstellen, dass alle vorgenommenen Änderungen auch für all diese Instanzen gelten sollen.

In der Beispielszene wurde jedes einzelne Box-Objekt instanziiert. Diese Aktion ist beim Exportieren der Szene im FBX-Format relevant.

![Szenenübersicht](media/scene-overview.jpg)

>Die bewährte Methode für die Instanziierung in Ihrer Szene ist die fortlaufende Erstellung, da „Kopien“ später nur sehr schwer durch instanziierte Objekte ersetzt werden können. 

## <a name="fbx-export-process"></a>FBX-Exportprozess

Wir können nun mit dem FBX-Export Ihrer Szene bzw. der Medienobjekte der Szene fortfahren. Im Allgemeinen ist es sinnvoll, beim Exportieren von Medienobjekten jeweils nur die gewünschten Objekte bzw. Medienobjekte der Szene für den Export auszuwählen. Wenn eine Szene 100 Objekte enthält und Sie nur 30 davon nutzen möchten, muss nicht die gesamte Szene exportiert werden. Falls Sie also nicht die gesamte Szene exportieren möchten, sollten Sie die entsprechende Auswahl treffen und zur folgenden Option navigieren:

* `File` > `Export Selection`. Legen Sie unten im Exportdialogfeld dann `Files of Type` auf `FBX Export` fest. In diesem Fenster werden die Einstellungen für den FBX-Export verfügbar gemacht. Die Haupteinstellungen für den FBX-Export sind in der Abbildung unten rot hervorgehoben.

![FBX-Export](media/FBX-exporting.jpg)

Je nach Ihren Anforderungen können Sie auswählen, ob die Texturen in die exportierte FBX-Datei eingebettet werden sollen. Es kann beispielsweise sein, dass Sie ein Medienobjekt an einen Client senden möchten, ohne hierbei eine große Zahl von Texturdateien mitzuschicken. Bei dieser Option müssen Sie nur eine Datei verpacken, aber sie führt zu einem erheblich größeren FBX-Medienobjekt. Sie können die Option zum Einbetten von Texturen aktivieren, indem Sie wie unten gezeigt `Embed Media` auswählen.

> [!TIP]
> Beachten Sie, dass die Datei in diesem Fall benannt wurde, um diese Bedingung widerzuspiegeln. Dies ist eine bewährte Methode, um Ihre Medienobjekte besser nachverfolgen zu können. 

Klicken Sie nach dem Festlegen Ihrer Konfiguration für den Export unten rechts auf die Schaltfläche „Export Selection“ (Auswahl exportieren).

![Einbetten von Medien](media/embedding-media.jpg)

## <a name="conclusion"></a>Zusammenfassung

Im Allgemeinen sehen diese Arten von Materialien realistischer aus, da sie auf den realen Gesetzen der Physik in Bezug auf das Licht basieren. Es entsteht ein zusätzlicher immersiver Effekt und der Eindruck, dass die Szene aus der realen Welt stammt.

## <a name="next-steps"></a>Nächste Schritte

Sie kennen nun die wichtigsten von ARR unterstützten Funktionen für das Einrichten von Materialien mit erweiterter Beleuchtung für Objekte einer Szene und das Exportieren im FBX-Format. Der nächste Schritt umfasst die Konvertierung der FBX-Datei und die Visualisierung in ARR.

> [!div class="nextstepaction"]
> [Schnellstart: Konvertieren eines Modells für das Rendering](../../quickstarts\convert-model.md)