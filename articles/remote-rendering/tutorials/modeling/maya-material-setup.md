---
title: Einrichten von PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in Maya
description: Hier erfahren Sie, wie Sie PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in Maya einrichten und in das FBX-Format exportieren.
author: FlorianBorn71
ms.author: flborn
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: 56aa0d91372ac2d21a20f28b1044f0811c716b0c
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91358031"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-maya"></a>Tutorial: Einrichten von PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in Maya

## <a name="overview"></a>Übersicht
In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Zuweisen von Materialien mit erweiterter Beleuchtung zu Objekten der Szene
> * Behandeln der Instanziierung von Objekten und Materialien
> * Exportieren einer Szene im FBX-Format und Auswählen wichtiger Optionen

Das Erstellen von [PBR-Materialien](../../overview/features/pbr-materials.md) (Physically Based Rendering, physikalisch basiertes Rendering) in Maya ist relativ unkompliziert. Es ähnelt in vielerlei Hinsicht der PBR-Einrichtung in anderen Anwendungen zum Erstellen von Inhalten (beispielsweise 3DS Max). Dieses Tutorial ist ein Leitfaden zur grundlegenden Einrichtung eines PBR-Shaders und für den FBX-Export für Azure Remote Rendering-Projekte. 

Die Beispielszene in diesem Tutorial enthält eine Reihe von Objekten in Form von Polygonwürfeln. Ihnen werden verschiedene Materialien wie Holz, Metall, lackiertes Metall, Plastik und Gummi zugewiesen. Jedes Material enthält praktisch alle bzw. die meisten der folgenden Texturen:

* **Albedo**: Die Farbzuordnung des Materials. Wird auch als **Diffuse** (Diffus) oder **BaseColor** (Basisfarbe) bezeichnet.
* **Metallartigkeit**: Bestimmt, ob ein Material metallisch ist bzw. welche Teile metallisch sind. 
* **Rauheit**: Bestimmt, wie rau oder glatt eine Oberfläche ist, und wirkt sich auf die Schärfe bzw. Unschärfe der Reflexionen und Lichter auf einer Oberfläche aus.
* **Normal**: Fügt einer Oberfläche Details hinzu, ohne dass weitere Polygone erforderlich sind. Beispiele für solche Details wären etwa Korrosion und Dellen bei einer Metalloberfläche oder die Maserung im Holz.
* **Umgebungsverdeckung**: Dient dazu, einem Modell Soft Shading und Kontaktschatten hinzuzufügen. Hierbei handelt es sich um eine Graustufenzuordnung, mit der angegeben wird, welche Bereiche eines Modells vollständige Beleuchtung (weiß) bzw. vollständigen Schatten (schwarz) erhalten. 

## <a name="prerequisites"></a>Voraussetzungen
* Autodesk Maya 2017 oder neuer

## <a name="set-up-materials-in-the-scene"></a>Einrichten von Materialien in der Szene
Hier erfahren Sie, wie Sie PBR-Material in Maya einrichten.

Wie Sie in der Beispielszene sehen, haben wir eine Reihe von Würfelobjekten erstellt. Jedes Objekt stellt einen anderen Materialtyp dar. Wie im Bild zu sehen, wurden die Objekte jeweils mit einem eigenen passenden Namen versehen.

In Azure Remote Rendering wird die Maßeinheit „Meter“ verwendet, und die Richtung „nach oben“ ist die Y-Achse. Bevor Sie mit der Erstellung von Medienobjekten beginnen, empfiehlt es sich, die Szeneneinheiten in Maya auf Meter festzulegen. Legen Sie für den Export die Einheiten in den FBX-Exporteinstellungen auf Meter fest.

> [!TIP]
> Versehen Sie die Medienobjekte Ihres Modells mit geeigneten Namen, die auf dem relevanten Teil oder Materialtyp basieren. Aussagekräftige Namen erleichtern die Navigation in Szenen mit zahlreichen Objekten.

![Objektnamen](media/object-names.jpg)

Nachdem Sie einige Texturen erstellt oder abgerufen haben, können Sie auch individuelle Texturen erstellen. Dazu können Sie entweder Textur-Apps wie Quixel Suite, Photoshop oder Substance Suite nutzen oder generische Kacheltexturen aus anderen Quellen verwenden.

So wenden Sie Texturen auf Ihr Modell an:

1. Wählen Sie im Viewport der Szene Ihr Modell oder Ihre Geometrie aus, und klicken Sie mit der rechten Maustaste darauf. Wählen Sie im daraufhin angezeigten Menü **Assign New Material** (Neues Material zuweisen) aus.
1. Navigieren Sie im Dialogfeld **Assign New Material** (Neues Material zuweisen) zu **Maya** > **Stingray PBS**. Dadurch wird Ihrem Modell PBR-Material zugewiesen. 

In Maya 2020 stehen verschiedene PBR-Shader zur Verfügung. Hierzu zählen **Maya Standard Surface** (Maya-Standardoberfläche), **Arnold Standard Surface** (Arnold-Standardoberfläche) und **Stingray PBR**. Für den Shader **Maya Standard Surface** (Maya-Standardoberfläche) ist noch kein Export über das FBX 2020-Plug-In möglich. Der Shader **Arnold Standard Surface** (Arnold-Standardoberfläche) kann mit FBX-Dateien exportiert werden. Ansonsten ist er größtenteils mit dem Shader **Maya Standard Surface** (Maya-Standardoberfläche) identisch. Er entspricht der Option **Physical Material** (Physisches Material) in 3D Studio Max.

Der Shader **Stingray PBR** ist mit vielen anderen Anwendungen kompatibel und erfüllt die Anforderungen von Azure Remote Rendering am besten. Er wird seit Maya 2017 unterstützt. Die Visualisierung dieser Art von Material im Viewport ähnelt der späteren Visualisierung in Azure Remote Rendering.

![Stingray-Material](media/stingray-material.jpg)

Nachdem Ihr Material dem Medienobjekt zugewiesen und ein geeigneter Name festgelegt wurde, können Sie Ihre verschiedenen Texturen zuweisen. Die folgenden Abbildungen veranschaulichen, wie die einzelnen Texturtypen zum PBR-Material passen. Beim Material „Stingray PBR“ können die aktivierbaren Attribute ausgewählt werden. Vor der Integration Ihrer Texturzuordnungen müssen die relevanten Attribute aktiviert werden.

![Materialeinrichtung](media/material-setup.jpg)

Versehen Sie Ihre Materialien mit einem geeigneten Namen, und berücksichtigen Sie dabei deren Verwendung bzw. Typ. Ein Material, das für ein spezielles Teil verwendet wird, kann beispielsweise nach diesem Teil benannt werden. Ein Material, das in mehreren Bereichen verwendet wird, sollte dagegen auf der Grundlage seiner Eigenschaften oder seines Typs benannt werden.

Weisen Sie Ihre Texturen zu, wie in der Abbildung zu sehen.

![Textureinrichtung](media/texture-setup.jpg)

Nachdem Sie Ihre PBR-Materialien erstellt und eingerichtet haben, sollten Sie sich Gedanken über die [Instanziierung von Objekten](../../how-tos/conversion/configure-model-conversion.md#instancing) in Ihrer Szene machen. Durch die Instanziierung ähnlicher Objekte in Ihrer Szene (beispielsweise Muttern, Bolzen, Schrauben und Unterlegscheiben) lässt sich die Dateigröße erheblich verringern. Instanzen eines Masterobjekts können über eine eigene Skalierung und Drehung sowie über eigene Transformationen verfügen und daher ganz nach Bedarf in Ihrer Szene platziert werden. 

In Maya ist die Instanziierung ein einfacher Prozess.

1. Navigieren Sie im Menü **Edit** (Bearbeiten) zu **Duplicate Special** (Inhalte duplizieren), um die entsprechenden Optionen zu öffnen.
1. Wählen Sie im Dialogfeld **Duplicate Special Options** (Optionen für „Inhalte duplizieren“) unter **Geometry type** (Geometrietyp) die Option **Instance** (Instanz) aus. 
1. Wählen Sie **Duplicate Special** (Inhalte duplizieren) aus.

   ![Der Screenshot zeigt ein Maya-Fenster mit geöffnetem Dialogfeld „Duplicate Special Options“ (Optionen für „Inhalte duplizieren“) und ausgewählter Option „Duplicate Special“ (Inhalte duplizieren).](media/instancing.jpg)

Durch diese Aktion wird eine Instanz Ihres Objekts erstellt. Sie kann unabhängig von ihrem übergeordneten Element sowie unabhängig von anderen Instanzen des übergeordneten Elements bewegt, gedreht und skaliert werden. 

Änderungen, die Sie an einer Instanz im Komponentenmodus vornehmen, werden an alle Instanzen Ihres Objekts übertragen. So können Sie beispielsweise mit den Komponenten eines instanziierten Objekts arbeiten – etwa mit Vertices und Polygonflächen. Vergewissern Sie sich, dass sich die vorgenommenen Änderungen auf alle diese Instanzen auswirken sollen. 

In der Beispielszene wurde jedes einzelne Würfelobjekt instanziiert. Diese Aktion ist beim Exportieren der Szene im FBX-Format relevant.

![Szenenübersicht](media/scene-overview.jpg)

> [!TIP]
> Es empfiehlt sich, Instanzen in Ihrer Szene immer gleich zu erstellen. Kopien lassen sich später nur sehr schwer durch instanziierte Objekte ersetzen. 

## <a name="fbx-export-process"></a>FBX-Exportprozess

Kommen wir nun zum FBX-Export Ihrer Szene oder der Medienobjekte Ihrer Szene. Beim Exportieren von Medienobjekten empfiehlt es sich, nur die (Medien-)Objekte aus Ihrer Szene auszuwählen, die Sie exportieren möchten. Ein Beispiel: Angenommen, Ihre Szene enthält 100 Objekte. Wenn Sie nur 30 davon verwenden möchten, ist es nicht besonders sinnvoll, die gesamte Szene zu exportieren. 

So treffen Sie Ihre Auswahl:

1. Navigieren Sie zu **File** > **Export Selection** („Datei“ > „Auswahl exportieren“), um das Dialogfeld **Export Selection** (Auswahl exportieren) zu öffnen.
1. Wählen Sie im Feld **Files of type** (Dateityp) die Option **FBX export** (FBX-Export) aus, um die FBX-Exporteinstellungen anzuzeigen. Die Haupteinstellungen für den FBX-Export sind in der Abbildung rot hervorgehoben.

   ![FBX-Export](media/FBX-exporting.jpg)

Abhängig von Ihren Anforderungen haben Sie verschiedene Möglichkeiten. Nehmen wir beispielsweise an, Sie möchten ein Medienobjekt an einen Client senden. Sie möchten aber nicht zahlreiche Texturdateien mit dem Medienobjekt senden. In diesem Fall können Sie die Texturen in die exportierte FBX-Datei einbetten. Bei dieser Option müssen Sie zwar lediglich eine einzelne Datei verpacken, dafür wird das FBX-Medienobjekt allerdings deutlich größer. Die Option zum Einbetten von Texturen kann wie gezeigt durch Auswählen der Option **Embed Media** (Medien einbetten) aktiviert werden.

> [!TIP]
> In diesem Beispiel wurde die Datei entsprechend benannt. Dieser Benennungsstil eignet sich sehr gut, um den Überblick über Ihre Medienobjekte zu behalten. 

Wählen Sie nach dem Festlegen der Exportkonfiguration rechts unten die Option **Export Selection** (Auswahl exportieren) aus.

![Einbetten von Medien](media/embedding-media.jpg)

## <a name="conclusion"></a>Zusammenfassung

Im Allgemeinen sehen diese Arten von Materialien realistischer aus, da sie auf den realen Gesetzen der Physik in Bezug auf das Licht basieren. Es entsteht ein zusätzlicher immersiver Effekt und der Eindruck, dass die Szene aus der realen Welt stammt.

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie Sie Materialien mit erweiterter Beleuchtung für Objekte in einer Szene einrichten. Außerdem haben Sie gelernt, wie Sie die Objekte in das von Azure Remote Rendering unterstützte FBX-Format exportieren. Im nächsten Schritt wird die FBX-Datei konvertiert und in Azure Remote Rendering visualisiert.

> [!div class="nextstepaction"]
> [Schnellstart: Konvertieren eines Modells für das Rendering](../../quickstarts\convert-model.md)