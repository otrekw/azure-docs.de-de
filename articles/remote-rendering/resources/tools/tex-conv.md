---
title: TexConv – Texturkonvertierungstool
description: Benutzerhandbuch für das Befehlszeilentool TexConv
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 1d9b2ca163b70435a6c0e245e66492e8e2866639
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678840"
---
# <a name="texconv---texture-conversion-tool"></a>TexConv – Texturkonvertierungstool

TexConv ist ein Befehlszeilentool zum Verarbeiten von Texturen aus typischen Eingabeformaten wie PNG, TGA, JPEG und DDS in optimierte Formate für die Verwendung während der Laufzeit.
Das häufigste Szenario besteht im Konvertieren einer einzelnen Eingabedatei `A.xxx` in ein optimiertes Format `B.yyy`; das Tool bietet jedoch eine Vielzahl weiterer Optionen für fortgeschrittenere Zwecke.

## <a name="command-line-help"></a>Befehlszeilenhilfe

Bei Ausführung von TexConv.exe mit dem Parameter `--help` werden alle verfügbaren Optionen aufgelistet. Außerdem gibt TexConv die verwendeten Optionen bei seiner Ausführung aus, sodass Sie ein Verständnis der Prozesse des Tools erlangen. Einzelheiten können Sie dieser Ausgabe entnehmen.

## <a name="general-usage"></a>Allgemeine Verwendung

TexConv erzeugt immer **genau eine Ausgabedatei**. Die Ausgabe kann auf der Grundlage **mehrerer Eingabedateien** assembliert werden. Für die Assembly wird zudem eine **Kanalzuordnung** benötigt; diese teilt dem Tool mit, welcher Kanal (*Rot, Grün, Blau* oder *Alpha*) aus welcher Eingabedatei in welchen Kanal des Ausgabebildes zu verschieben ist.

Die einfachste Befehlszeile lautet wie folgt:

```cmd
TexConv.exe -out D:/result.dds -in0 D:/img.jpg -rgba in0
```

- `-out` gibt die Ausgabedatei und das Ausgabeformat an.
- `-in0` gibt das erste Eingabebild an.
- `-rgba` weist an, dass das Ausgabebild alle vier Kanäle verwenden soll, und dass diese 1:1 aus dem Eingabebild übernommen werden sollen.

## <a name="multiple-input-files"></a>Mehrere Eingabedateien

Um die Ausgabe aus mehreren Eingabedateien zu assemblieren, geben Sie jede Eingabedatei mit der Option `-in` mit einer zunehmenden Zahl an:

```cmd
-in0 D:/img0.jpg -in1 D:/img1.jpg -in2 D:/img2.jpg ...
```

Wenn Sie eine Cubemap aus 2D-Texturen assemblieren, können auch `-right`, `-left`, `-top`, `-bottom`, `-front`, `-back` oder `-px`, `-nx`, `-py`, `-ny`, `-pz`, `-nz` verwendet werden.

Um diese Eingaben der Ausgabedatei zuzuordnen, wird eine entsprechende Kanalzuordnung benötigt.

## <a name="channel-mappings"></a>Kanalzuordnungen

Die Optionen für die Kanalzuordnung geben an, aus welcher Eingabe die angegebenen Ausgabekanäle gefüllt werden. Sie können die Eingabe für jeden Kanal wie folgt einzeln angeben:

```cmd
-r in0.b -g in0.g -b in0.r -a in1.r
```

Hier werden die RGB-Kanäle der Ausgabe mit dem ersten Eingabebild gefüllt, Rot und Blau werden jedoch getauscht. Der Alphakanal der Ausgabe wird mit den Werten des roten Kanals des zweiten Eingabebildes gefüllt.

Das separate Angeben der Zuordnung für jeden Kanal bietet die größte Flexibilität. Benutzerfreundlicher kann dies auch mithilfe von „Swizzling-Operatoren“ ausgedrückt werden:

```cmd
-rgb in0.bgr -a in1.r
```

### <a name="output-channels"></a>Ausgabekanäle

Die folgenden Optionen für die Kanalzuordnung sind verfügbar:

- `-r`, `-g`, `-b`, `-a` : Mit diesen werden Zuweisungen einzelner Kanäle angegeben.
- `-rg` : Hiermit geben Sie die Zuweisungen für den roten und den grünen Kanal an.
- `-rgb` : Hiermit geben Sie die Zuweisungen für den roten, grünen und blauen Kanal an.
- `-rgba` : Hiermit werden Zuweisungen für alle vier Kanäle angegeben.

Dadurch, dass nur der Kanal R, RG oder RGB angegeben ist, wird TexConv angewiesen, eine Ausgabedatei mit lediglich 1, 2 bzw. 3 Kanälen zu erstellen.

### <a name="input-swizzling"></a>Swizzling der Eingabe

Bei der Angabe, welche Eingabetextur welchen Ausgabekanal füllen soll, können Sie ein Swizzling der Eingabe vornehmen:

- `-rgba in0` entspricht `-rgba in0.rgba`.
- `-rgba in0.bgra` führt ein Swizzling der Eingabekanäle durch.
- `-rgb in0.rrr` dupliziert den roten Kanal in alle Kanäle.

Sie können Kanäle auch entweder mit Schwarz oder Weiß füllen:

- `-rgb in0 -a white` erstellt eine 4-Kanal-Ausgabetextur, Alpha ist jedoch auf vollständig undurchsichtig festgelegt.
- `-rg black -b white` erstellt eine vollständig blaue Textur.

## <a name="common-options"></a>Allgemeine Optionen

Die interessantesten Optionen sind unten aufgeführt. Weitere Optionen werden mit `TexConv --help`aufgelistet.

### <a name="output-type"></a>Ausgabetyp

- `-type 2D` : Bei der Ausgabe handelt es sich um ein normales 2D-Bild.
- `-type Cubemap` : Bei der Ausgabe handelt es sich um ein Cubemap-Bild. Diese Option wird nur für DDS-Ausgabedateien unterstützt. Wenn diese Option angegeben ist, kann die Cubemap aus sechs normalen 2D-Eingabebildern assembliert werden.

### <a name="image-compression"></a>Bildkomprimierung

- `-compression none` : Das Ausgabebild wird nicht komprimiert.
- `-compression medium` : Bei Unterstützung dieser Option wird die Komprimierung auf das Ausgabebild angewendet, ohne dass die Qualität zu stark beeinträchtigt wird.
- `-compression high` : Bei Unterstützung dieser Option wird die Komprimierung auf das Ausgabebild angewendet, und die Qualität wird zugunsten einer kleineren Datei verringert.

### <a name="mipmaps"></a>Mipmaps

Standardmäßig generiert TexConv Mipmaps, wenn diese vom Ausgabeformat unterstützt werden.

- `-mipmaps none` : Es werden keine Mipmaps generiert.
- `-mipmaps Linear` : Bei Unterstützung dieser Option werden Mipmaps mit einem Box-Filter generiert.

### <a name="usage-srgb--gamma-correction"></a>Verwendung (sRGB/Gammakorrektur)

Die Option `-usage` gibt den Zweck der Ausgabe an und teilt TexConv mit, ob die Gammakorrektur auf die Eingabe-und Ausgabedateien angewendet werden soll. Die Verwendung wirkt sich nur auf die RGB-Kanäle aus. Für den Alphakanal wird immer angenommen, dass er „lineare “ Werte enthält. Ist keine Verwendung angegeben, versucht der Auto-Modus, die Verwendung anhand des Formats und des Dateinamens des ersten Eingabebildes zu bestimmen. Einzel-und Zweikanal-Ausgabeformate sind beispielsweise immer linear. Überprüfen Sie die Ausgabe, um zu ermitteln, welche Entscheidung von TexConv getroffen wurde.

- `-usage Linear` : Das Ausgabebild enthält Werte, die keine Farben darstellen. Dies ist typischerweise bei metallischen und rauen Texturen der Fall, sowie bei allen Arten von Masken.

- `-usage Color` : Im Ausgabebild wird Farbe dargestellt, z. B. diffuse/Albedo-Maps. Das sRGB-Flag ist im DDS-Header der Ausgabe festgelegt.

- `-usage HDR` : In der Ausgabedatei sind für die Codierung mehr als 8 Bit pro Pixel zu verwenden. Folglich werden alle Werte im linearen Raum gespeichert. Bei HDR-Texturen spielt es keine Rolle, ob die Daten Farben oder andere Daten darstellen.

- `-usage NormalMap` : Das Ausgabebild stellt eine Tangent-Space-Normal Map dar. Die Werte werden normalisiert, und die Mipmap-Berechnung wird leicht optimiert.

- `-usage NormalMap_Inverted` : Bei der Ausgabe handelt es sich um eine Tangent-Space-Normal Map, bei der Y in die entgegengesetzte Richtung der Eingabe zeigt.

### <a name="image-rescaling"></a>Neuskalierung von Bildern

- `-minRes 64` : Gibt die minimale Auflösung der Ausgabe an. Wenn das Eingabebild kleiner ist, wird es vergrößert.
- `-maxRes 1024` : Gibt die maximale Auflösung der Ausgabe an. Wenn das Eingabebild größer ist, wird es verkleinert.
- `-downscale 1` : Wenn dieser Wert größer als 0 ist, wird die Auflösung der Eingabebilder n Mal halbiert. Verwenden Sie diese Option, um die Gesamtqualität zu verringern.

## <a name="examples"></a>Beispiele

### <a name="convert-a-color-texture"></a>Konvertieren einer Farbtextur

```cmd
TexConv.exe -out D:/diffuse.dds -in0 D:/diffuse.jpg -rgba in0 -usage color
```

### <a name="convert-a-normal-map"></a>Konvertieren einer Normal Map

```cmd
TexConv.exe -out D:/normalmap.dds -in0 D:/normalmap.png -rgb in0 -usage normalmap
```

### <a name="create-an-hdr-cubemap"></a>Erstellen einer HDR-Cubemap

```cmd
TexConv.exe -out "D:/skybox.dds" -in0 "D:/skymap.hdr" -rgba in0 -type cubemap -usage hdr
```

Eine hervorragende Quelle für HDR-Cubemaps ist [hdrihaven.com](https://hdrihaven.com/hdris/).

### <a name="bake-multiple-images-into-one"></a>Baking von mehreren Bildern in ein Bild

```cmd
TexConv.exe -out "D:/Baked.dds" -in0 "D:/metal.tga" -in1 "D:/roughness.png" -in2 "D:/DiffuseAlpha.dds" -r in1.r -g in0.r -b black -a in2.a -usage linear
```

### <a name="extract-a-single-channel"></a>Extrahieren eines einzelnen Kanals

```cmd
TexConv.exe -out D:/alpha-mask-only.dds -in0 D:/DiffuseAlpha.dds -r in0.a
```
