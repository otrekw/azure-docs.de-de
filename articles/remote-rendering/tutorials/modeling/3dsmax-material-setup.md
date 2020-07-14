---
title: Einrichten von Materialien zum physikalisch basierten Rendering in 3DSMax
description: Es wird beschrieben, wie Sie PBR-Materialien (Physically Based Rendering, physikalisch basiertes Rendering) in 3DSMax einrichten und im FBX-Format exportieren.
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: df4be8963c93199f9fad23ab3f709f691e1da768
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85857551"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-3d-studio-max"></a>Tutorial: Einrichten von PBR-Materialien in 3D Studio Max

## <a name="overview"></a>Übersicht
In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
>
> * Weisen Sie Materialien mit erweiterter Beleuchtung Objekten der Szene zu.
> * Verarbeiten Sie die Instanziierung von Objekten und Materialien.
> * Exportieren Sie eine Szene im FBX-Format, und wählen Sie wichtige Optionen aus.

Das Erstellen von [physikalisch basierten Rendering-Materialien (PBR)](../../overview/features/pbr-materials.md) in 3D Studio Max (3DSMax) ist eine relativ unkomplizierte Aufgabe. Der Vorgang ähnelt in vielerlei Hinsicht der PBR-Einrichtung in anderen Anwendungen zum Erstellen von Inhalten, wie z. B. Maya. Dieses Tutorial ist ein Leitfaden zur grundlegenden Einrichtung eines PBR-Shaders und für den FBX-Export für Azure Remote Rendering-Projekte.

Die Beispielszene in diesem Tutorial enthält eine Reihe von Polygonfeldobjekten. Ihnen werden verschiedene Materialien zugewiesen, wie z. B. Holz, Metall, lackiertes Metall, Plastik und Gummi. Jedes Material enthält praktisch alle bzw. die meisten der folgenden Texturen:

* **Albedo**: beschreibt die Farbzuordnung des Materials und wird auch als **Diffus** oder **BaseColor** bezeichnet.
* **Metallartigkeit**: bestimmt, ob ein Material metallisch ist bzw. welche Teile metallisch sind. 
* **Rauheit**: bestimmt, wie grob oder glatt eine Oberfläche ist.
Dies wirkt sich auch auf die Schärfe oder Unschärfe der Reflexionen und Highlights auf einer Oberfläche aus.
* **Normal**: fügt einer Oberfläche Details hinzu, ohne dass weitere Polygone erforderlich sind. Beispiele für Details sind u. U. Unebenheiten und Dellen auf einer Metalloberfläche oder auf einem Holz.
* **Umgebungsverdeckung**: dient zum Hinzufügen von Soft Shading und Kontaktschatten zu einem Modell. Es handelt sich um eine Graustufenzuordnung, mit der angegeben wird, welche Bereiche eines Modells vollständige Beleuchtung (weiß) bzw. vollständigen Schatten (schwarz) erhalten.

## <a name="prepare-the-scene"></a>Vorbereiten der Szene
In **3D Studio Max** wird zum Einrichten eines PBR-Materials der folgende Prozess verwendet.

Für den Einstieg haben wir – wie in der Beispielszene zu sehen – einige Objekte vom Typ „Box“ erstellt, die jeweils für eine andere Art von Material stehen:

>[!TIP]
>Ein wichtiger Hinweis, bevor Sie mit dem Erstellen von Medienobjekten für Azure Remote Rendering (ARR) beginnen: Für die Messungen wird **Meter** als Einheit verwendet.  
>Daher ist es ratsam, dass Sie die **Systemeinheiten** für Szenen auf **Meter** festlegen. Darüber hinaus sollten Sie auch beim Exportieren in den Einstellungen für FBX-Exporte „Meter“ wählen.

In der folgenden Abbildung werden die Schritte zum Festlegen der Systemeinheiten auf „Meter“ in 3D Studio Max veranschaulicht. Navigieren Sie im Hauptmenü zu **Anpassen** > **Units Setup** (Einheiteneinrichtung)  >  **System Units Setup** (Systemeinheiteneinrichtung), und wählen Sie dann im Dropdownmenü **System Units Scale** (Systemeinheitenskala) die Option **Meter** aus. 
![Systemeinheiten](media/3dsmax/system-units.jpg)

Wenn die Systemeinheiten auf „Meter“ festgelegt sind, können wir mit dem Erstellen unserer Modelle beginnen. In unserer Beispielszene erstellen wir mehrere Box-Objekte, die jeweils einen anderen Materialtyp darstellen (z. B. Metall, Gummi, Plastik usw.). 

>[!TIP]
>Beim Erstellen von Objekten empfiehlt es sich, sie entsprechend zu benennen. Dadurch können sie später leichter gefunden werden, wenn die Szene viele Objekte enthält.

![rename-objects](media/3dsmax/rename-objects.jpg)

## <a name="assign-materials"></a>Zuweisen von Materialien

Während einige Objekte in unserer Szene erstellt werden (in diesem Fall eine Reihe von Cubes), können wir mit der PBR-Einrichtung beginnen:

* Klicken Sie in der Hauptsymbolleiste auf das Symbol für den **Material Editor** (Materialeditor), wie auf der folgenden Abbildung dargestellt. Alternativ können Sie auch die Taste **M** auf der Tastatur drücken, um den Editor zu öffnen. Der Materialeditor verfügt über zwei Modi, die im Dropdownmenü **Modi** ausgewählt werden können: den Modus **Compact material editor** (Kompakter Materialeditor) und den Modus **Slate material** (Schiefermaterial). Da diese Szene relativ einfach ist, wird der **Kompaktmodus** verwendet.

* Im Materialeditor werden einige Sphären angezeigt: Dies sind unsere Materialien. Wir weisen jedem Objekt in unserer Szene eines dieser Materialien zu. Wählen Sie für diese Zuweisung zuerst eines der Objekte im Hauptanzeigebereich aus. Wenn diese Auswahl getroffen wurde, klicken Sie auf die erste Sphäre im Fenster des Materialeditors. Nach der Zuweisung zu einem Objekt wird das ausgewählte Material wie in der nächsten Abbildung dargestellt hervorgehoben.

* Klicken Sie auf die Schaltfläche **Assign Material to Selection** (Material der Auswahl zuweisen), wie abgebildet. Das ausgewählte Material wurde dem ausgewählten Objekt zugewiesen.
![assign-material](media/3dsmax/assign-material.jpg)

Im Materialeditor können Sie je nach Anwendungsfall Materialtypen aus einer breiten Auswahl auswählen. In der Regel wird der Materialtyp auf **Standard** festgelegt. Dieses Material ist ein grundlegendes Material, das nicht zur PBR-Einrichtung geeignet ist. Daher müssen wir den Materialtyp in ein PBR-Material ändern. Das bevorzugte **3DSMax**-Material für Azure Remote Rendering-Projekte ist **Physisches Material**.

* Klicken Sie im Materialeditor auf die Registerkarte **Standard**, und wählen Sie in dem sich öffnenden Material-/Zuordnungsbrowser die Option **Physisches Material** aus. Durch diese Aktion wird das zugewiesene **Standard**-Material in ein **Physisches Material** für PBR konvertiert.
![physical-material](media/3dsmax/physical-material.jpg)

* Im Materialeditor werden nun die Eigenschaften für das physische Material angezeigt (siehe unten), und wir können mit dem Zuweisen von Texturen zum Medienobjekt beginnen.
![textures-list](media/3dsmax/textures-list.jpg)

Wie aus der obigen Abbildung ersichtlich ist, gibt es eine Vielzahl von Zuordnungen und Texturen, die dem Material hinzugefügt werden können. Zu diesem Zweck werden jedoch nur fünf Texturslots im Material verwendet.

>[!TIP]
>Es empfiehlt sich, Ihre Materialien entsprechend zu benennen, wie in der Abbildung oben gezeigt.

Wir können nun in Erwägung ziehen, unseren Materialien Texturen zuzuweisen. Die Art und Weise, wie Sie Ihre Texturen generieren, kann je nach Einstellung oder sogar entsprechend der Verwendung variieren. So können Sie beispielsweise Kacheltexturen verwenden, die auf jedes Objekt angewendet werden können, oder Sie benötigen für bestimmte Teile eines Projekts/Objekts einen eigenen, benutzerdefinierten Satz von Texturen. Vielleicht verwenden Sie generische Kacheltexturen, die Sie online erhalten haben, oder sie selbst in Anwendungen wie **Photoshop**, **Quixel Suite**, **Substance Suite** usw. erstellen. 

Bevor wir mit der Zuweisung unserer Texturen beginnen, müssen wir unsere Objekttexturkoordinaten (UVW) berücksichtigen. Zwar ist es üblich, beim Anwenden beliebiger Texturen auf ein Modell sicherzustellen, das der Wrapper des Objekts entfernt wurde (Texturen werden ohne ein korrektes Entfernen des UV-Wrappers nicht ordnungsgemäß angezeigt), besonders wichtig ist dies jedoch, wenn wir beabsichtigen, auf unserem Modell eine Zuordnung vom Typ **Umgebungsverdeckung** zu verwenden. Im Gegensatz zum **Stingray Shader** in **Maya** verfügt das **Physische Material** in **3DSMax** nicht über einen dedizierten Texturslot vom Typ **Umgebungsverdeckung**. Daher wenden wir die AO-Zuordnung auf einen anderen Slot an und weisen ihm einen eigenen UVW-Zuordnungskanal zu, damit sie getrennt von den anderen Texturen (z. B. Kacheltexturen) verwendet werden kann. 

Wir beginnen damit, dem Modell wie unten dargestellt einen **Unwrap UVW modifier** (UVW-Modifizierer zum Entfernen von Wrappern) zuzuweisen:

* Klicken Sie im Eigenschafteneditor für ausgewählte Objekte auf die Modifiziererliste, scrollen Sie in der Dropdownliste nach unten, und wählen Sie die Option zum Entfernen des UVW-Wrappers aus. Durch diese Aktion wird ein UVW-Modifizierer zum Entfernen von Wrappern auf das Objekt angewendet.
![unwrap-modifier](media/3dsmax/unwrap-modifier.jpg)

* Der Zuordnungskanal ist auf „Eins“ festgelegt. Das Entfernen des Wrappers erfolgt in der Regel auf dem ersten Zuordnungskanal. In unserem Fall wurde der Wrapper des Objekts ohne überlappende Texturkoordinaten (UV) entfernt.
![unwrapped-uvw](media/3dsmax/unwrapped-uvw.jpg)

Der nächste Schritt ist das Erstellen eines zweiten UV-Zuordnungskanals.

* Schließen Sie den UV-Editor, wenn er geöffnet ist, und wechseln Sie im Kanalabschnitt des Menüs **UVs bearbeiten** zum zweiten Kanal. Der zweite Zuordnungskanal ist der für Zuordnungen der Umgebungsverdeckung erwartete Kanal. 

* Im sich öffnenden Dialogfeld **Channel Change Warning** (Warnung zur Änderung des Kanals) haben Sie die Möglichkeit, den vorhandenen ersten Kanal in den neuen zweiten Kanal zu **Verschieben** oder die vorhandenen UVs zu **Verwerfen**, die automatisch neue **UV Unwrap** (Entfernungsvorgänge fpr UV-Wrapper) erstellen. Wählen Sie **Verwerfen** nur dann aus, wenn Sie einen neuen **UV Unwrap** (Entfernungsvorgang für UV-Wrapper) für die Zuordnung der Umgebungsverdeckung erstellen möchten, die sich von der UV-Zuordnung im ersten Kanal unterscheidet (beispielsweise wenn Sie Kacheltexturen im ersten Kanal verwenden möchten). Für unsere Zwecke **Verschieben** wir die UVs vom ersten zum zweiten Kanal, da wir den neuen UV-Kanal nicht bearbeiten müssen.

>[!NOTE]
>Selbst wenn Sie den Entfernungsvorgang des UV-Wrappers – **Verschoben** – vom ersten Zuordnungskanal in den zweiten kopiert haben, können Sie an den UVs des neuen Kanals alle notwendigen Änderungen vornehmen, ohne den ursprünglichen Zuordnungskanal zu beeinflussen.

![channel-change](media/3dsmax/channel-change.jpg)

Nachdem der neue Zuordnungskanal erstellt wurde, können wir zum physischen Material im Materialeditor zurückkehren und damit beginnen, die Texturen hinzuzufügen. Zuerst fügen wir die Zuordnung für die Umgebungsverdeckung (Ambient Occlusion, **AO**) hinzu, da ein weiterer Schritt erforderlich ist, um die ordnungsgemäße Funktionsweise zu ermöglichen. Sobald die AO-Zuordnung an unser Material angeschlossen ist, müssen wir festlegen, dass der zweite Kanal verwendet werden soll.

* Wie bereits erwähnt ist kein dedizierter Slot für AO-Zuweisungen im physischen Material von **3DSMax** vorhanden. Stattdessen wenden wir die AO-Zuordnung auf den Slot **Diffuse Roughness** (Diffuse Rauheit) an.

* Klicken Sie in der Liste der **Generischen Zuordnungen** der physischen Materialien auf den Slot **Keine Zuordnung** von **Diffuse Roughness** (Diffuse Rauheit) und laden Sie die AO-Zuordnung.

* In den Eigenschaften der AO-Texturen sehen Sie, dass der Zuordnungskanal standardmäßig auf **1** festgelegt ist. Ändern Sie diesen Wert in **2**. Mit dieser Aktion werden die erforderlichen Schritte zum Hinzufügen der Zuordnung vom Typ „Umgebungsverdeckung“ abgeschlossen.

>[!IMPORTANT]
>Dies ist ein wichtiger Schritt, insbesondere wenn sich Ihre UVs im zweiten Kanal von denen im ersten Kanal unterscheiden, da die AO bei Auswahl des falschen Kanals keine korrekte Zuordnung durchführen kann.

![assign-ao-map](media/3dsmax/assign-ao-map.jpg)

Wir befassen uns jetzt mit dem Zuweisen unserer normalen Zuordnung zu unserem PBR-Material. Diese Aktion unterscheidet sich von **Maya** darin, dass die normale Zuordnung nicht direkt auf den Bump-Zuordnungsslot angewendet wird (es ist kein normaler Zuordnungsslot im **Physischen 3Dsmax-Material** vorhanden), sondern stattdessen einem normal Zuordnungsmodifizierer hinzugefügt wird, der wiederum an den Slot **Normal** angeschlossen ist.

* Klicken Sie im Bereich **Spezielle Zuordnungen** der Eigenschaften für physische Materialien (im Materialeditor) auf den Slot **Keine Zuordnung** der **Bump-Zuordnung**. 

* Suchen Sie im Material-/Zuordnungsbrowser nach **Normaler Bump**, und klicken Sie darauf. Durch diese Aktion wird dem Material ein Modifizierer vom Typ **Normaler Bumb** hinzugefügt.

* Klicken Sie im Modifizierer **Normaler Bump** auf **Keine Zuordnung** von **Normal**, suchen Sie nach der normalen Zuordnung, und laden Sie sie.

* Überprüfen Sie, ob die Methode auf **Tangens** festgelegt ist (sollte standardmäßig der Fall sein), und schalten Sie ggf. **Flip Green (Y)** (Grün kippen (Y)) um.

![normal-bump](media/3dsmax/normal-bump.jpg)
![load-normal-map](media/3dsmax/load-normal-map.jpg)

Wenn unsere normale Zuordnung ordnungsgemäß zugewiesen ist, können wir mit der Zuweisung der verbleibenden Texturen fortfahren, um die Einrichtung des physischen Materials abzuschließen. Dabei handelt es sich um einen einfachen Vorgang, der keine besonderen Einstellungen erfordert. Die folgende Abbildung zeigt den vollständigen Satz von Texturen, die dem Material zugewiesen sind: ![all-textures](media/3dsmax/all-textures.jpg)

Nachdem Sie Ihre PBR-Materialien erstellt und eingerichtet haben, sollten Sie sich über die Instanziierungsobjekte Ihrer Szene Gedanken machen. Wenn Sie ähnliche bzw. zusammengehörige Objekte Ihrer Szene, z. B. Muttern, Bolzen, Schrauben und Scheiben, instanziieren, können Sie erhebliche Einsparungen bei der Dateigröße erzielen. Instanzen eines Masterobjekts können über eine eigene Skalierungen, Drehungen und Transformationen verfügen und daher je nach Bedarf in Ihrer Szene angeordnet werden. In **3D Studio Max** ist die **Instanziierung** ein einfacher Prozess.

* Wählen Sie im Hauptanzeigebereich die Objekte aus, die Sie exportieren möchten.

* Halten Sie die Taste **UMSCHALT** gedrückt, und ziehen Sie die Objekte mithilfe des Transformationstools (verschieben) nach oben. 

* Legen Sie im sich öffnenden Dialogfeld **Klonoptionen** die Option **Objekt** auf **Instanz** fest, und klicken Sie auf **OK**. 
![instance-object](media/3dsmax/instance-object.jpg)

Bei dieser Aktion wird eine Instanz Ihres Objekts erstellt, die unabhängig vom übergeordneten Objekt und dessen anderen Instanzen verschoben, gedreht oder skaliert werden kann.

>[!IMPORTANT]
>Allerdings werden alle Änderungen, die Sie im Unterobjektmodus an einer Instanz vornehmen, auf alle Instanzen Ihres Objekts übertragen. Wenn Sie mit den Komponenten eines instanziierten Objekts arbeiten, z. B. Scheitelpunkten, Polygonen, Flächen usw., sollten Sie daher zunächst sicherstellen, dass alle vorgenommenen Änderungen auch für all diese Instanzen gelten sollen. Beachten Sie, dass jedes instanziierte Objekt jederzeit in ein eindeutiges Objekt transformiert werden kann. 

>[!TIP]
>Die bewährte Methode für die Instanziierung in Ihrer Szene ist die fortlaufende Erstellung, da **Kopien** später nur sehr schwer durch instanziierte Objekte ersetzt werden können. 

Bevor wir mit dem Exportvorgang fortfahren, sollten Sie zunächst die Szene/das Objekt für die Freigabe verpacken. Wenn Sie das Objekt an einen Kunden oder ein Teammitglied weitergeben, sollen diese das Objekt im Idealfall mit einem Minimum an Aufhebens öffnen und so betrachten können, wie es vorgesehen ist. Daher ist es wichtig, dass die Texturpfade der Objekte gemeinsam mit der Szenendatei aufbewahrt werden. Wenn die Texturpfade für Ihr Objekt auf ein lokales Laufwerk oder einen absoluten Pfad/Speicherort weisen, werden sie beim Öffnen auf einem anderen Computer nicht in die Szene geladen, auch wenn sich die Datei „ **.max**“ im selben Ordner wie die Texturen befindet. Durch die relative Erstellung der Texturpfade in 3D Studio wird dieses Problem gelöst. Außerdem ist dieser Vorgang ziemlich einfach.

* Wechseln Sie auf der Hauptsymbolleiste zu **Datei** > **Referenz** > **Asset Tracking Toggle** (Objektverfolgungsumschalter). 

* Im daraufhin geöffneten Browser zum Verfolgen von Objekten werden alle oder die meisten Texturen angezeigt, die Sie auf Ihre PBR-Materialien angewendet haben. Sie werden in der Spalte **Zuordnungen/Shaders** aufgeführt.

* Daneben in der Spalte **Vollständiger Pfad** sehen Sie den Dateipfad zum Speicherort Ihrer Texturen, höchstwahrscheinlich der Speicherort auf dem lokalen Computer.

* Schließlich wird eine Spalte mit dem Namen **Status** angezeigt. Diese Spalte zeigt an, ob eine bestimmte Textur gefunden und auf Ihre Szene angewendet wurde, und kennzeichnet diese Textur mit einem der folgenden Begriffe: **OK**, **Gefunden** oder **Datei fehlt**. Die ersten beiden Begriffe weisen darauf hin, dass die Datei gefunden und geladen wurde. Der letzte Wert bedeutet, dass eine Datei nicht gefunden werden konnte.
![texture-paths](media/3dsmax/texture-paths.jpg)

Sie werden feststellen, dass nicht alle Texturen beim ersten Öffnen in der Objektverfolgung aufgeführt sind. Es gibt keinen Grund zur Besorgnis, denn wenn der Pfadsuchprozess ein- oder zweimal durchlaufen wurde, werden in der Regel alle Texturen der Szene gefunden. Der Prozess für die Pfadsuche lautet wie folgt: 

* Klicken Sie im Fenster der Objektverfolgung mit **UMSCHALT**+**Klick** auf die erste Textur in der Liste **Zuordnungen/Shaders**, und halten Sie die Umschalttaste weiterhin gedrückt, wenn Sie auf die letzte Textur der Liste klicken. So wählen Sie alle Texturen in der Liste aus. Die ausgewählten Texturen werden nun blau hervorgehoben (siehe Abbildung oben).

* Klicken Sie mit der rechten Maustaste auf die Auswahl, und wählen Sie im sich öffnenden Popupmenü die Option **Pfad festlegen** aus.

* Wählen Sie im sich öffnenden Textfeld **Specify Asset Path** (Objektpfad festlegen) den lokalen Pfad zu den angezeigten Texturen aus, ersetzen Sie ihn durch `.\`, und klicken Sie auf **OK**. 

* Nach einem bestimmten Zeitraum (je nachdem, wie viele Texturen in Ihrer Szene vorhanden sind und wie groß die Szene ist) sollte sich die Objektverfolgung wie folgt auflösen (siehe Abbildung).
![resolve-textures](media/3dsmax/resolve-textures.jpg)

Beachten Sie, dass die Spalte **Vollständiger Pfad** jetzt leer ist. Das bedeutet, dass die Szene nicht mehr nach den relevanten Texturen an einem bestimmten (absoluten) Ort sucht, sondern diese immer findet, solange sich die MAX-Datei oder die zugehörige FBX-Datei im gleichen Ordner wie die Texturen befindet. 

>[!NOTE]
>Manchmal kann es vorkommen, dass Sie diesen Prozess mehrmals wiederholen müssen, um alle Texturen und Pfade zu finden und aufzulösen. Das ist ganz normal. Wiederholen Sie den Vorgang einfach, bis alle relevanten Objekte gefunden wurden. Ebenso kann es vorkommen, dass einige Dateien nicht mehr gefunden werden können. Wählen Sie in diesem Fall einfach alle Objekte in der Liste aus, und klicken Sie auf **Fehlende Pfade entfernen** (siehe Abbildung oben).

## <a name="fbx-export"></a>FBX-Export

Nachdem die Objektverfolgung fertiggestellt wurde, können wir nun zum FBX-Export wechseln. Der Prozess ist einfach und kann auf verschiedene Weise durchgeführt werden. 

>[!TIP]
>Es wird empfohlen, dass Sie nur die benötigten Objekte exportieren, wenn Sie nicht die gesamte Szene exportieren möchten. Bei besonders ressourcenintensiven Szenen kann der Exportvorgang einige Zeit in Anspruch nehmen, sodass es sinnvoll ist, nur das Benötigte zu exportieren.
>
>Ratsam ist es, bei Verwendung von Modifizierern wie beispielsweise **Turbosmooth** oder **Open SubDiv** (SubDiv öffnen) diese vor dem Exportieren zu reduzieren, da Sie Probleme beim Exportieren verursachen können. Speichern Sie Ihre Szene immer vorher. 

* Wählen Sie in der Szene die Objekte aus, die Sie exportieren möchten, und wechseln Sie in der Hauptsymbolleiste zu **Datei** > **Exportieren** > **Auswahl exportieren**.

* Geben Sie im Dialogfeld **Select File to Export** (Datei zum Exportieren auswählen) den Namen der Ausgabedatei ein, oder wählen Sie sie aus, und wählen Sie in den Optionen **Save as Type** (Als Typ speichern) die Option **Autodesk (*.fbx)** aus. Daraufhin wird das TBX-Exportmenü geöffnet. 

* Wenn Sie in Ihrer Szene Instanzen erstellt haben, ist es wichtig, dass die Option **Preserve Instances** (Instanzen beibehalten) in den FBX-Exporteinstellungen eingeschaltet ist. 
![fbx-export](media/3dsmax/fbx-export.jpg)

Wie bereits erwähnt gibt es mehrere Möglichkeiten, unsere Datei zu exportieren. Wenn die Absicht beim Exportieren lautet, dass die FBX-Datei gemeinsam mit den Texturdateien in einem Ordner/Verzeichnis freigegeben werden soll, müssen die in der folgenden Abbildung gezeigten Einstellungen zutreffen und gut funktionieren. Nachdem Sie die Einstellungen ausgewählt haben, klicken Sie auf **OK**.
![fbx-settings](media/3dsmax/fbx-settings.jpg)

Wenn Sie jedoch lieber große Texturordner/-verzeichnisse gemeinsam mit der FBX-Datei freigeben möchten, können Sie die Texturen in die FBX-Datei **Einbetten**. Dies bedeutet, dass die gesamten enthaltenen Objekttexturen einer einzelnen FBX-Datei hinzugefügt werden. Seien Sie sich jedoch bewusst, dass die FBX-Datei dadurch erheblich größer wird, obwohl sie Ihren Export in einem einzigen Objekt zusammenfasst.

>[!IMPORTANT]
>Wenn Ihre FBX-Ergebnisdatei größer als 2,4 GB ist, sollte die Mindestversion der FBX-Exporteinstellungen (siehe oben) 2016 oder neuer sein. Denn neuere Versionen unterstützen 64 Bit und somit größere Dateien.

* Schalten Sie in den FBX-Exporteinstellungen das Einbetten von Medien ein, und klicken Sie dann auf **OK**, um die Texturen beim Exportieren zu berücksichtigen. 

Beim Export in FBX unter Verwendung des physischen Materials wird Ihnen möglicherweise die folgende Warnmeldung angezeigt, nachdem Sie im Exportdialogfeld auf „OK“ geklickt haben: ![export-warnings](media/3dsmax/export-warnings.jpg)

Diese Warnung informiert den Benutzer darüber, dass die exportierten Materialien möglicherweise nicht mit anderen Softwarepaketen kompatibel sind. Da das physische Material mit Azure Remote Rendering kompatibel ist, müssen Sie sich keine Gedanken machen. Klicken Sie einfach auf **OK**, um den Vorgang abzuschließen und das Fenster zu schließen.

## <a name="conclusion"></a>Zusammenfassung

Im Allgemeinen sehen diese Arten von Materialien realistischer aus, da sie auf den realen Gesetzen der Physik in Bezug auf das Licht basieren. Es entsteht ein zusätzlicher immersiver Effekt und der Eindruck, dass die Szene aus der realen Welt stammt.

## <a name="next-steps"></a>Nächste Schritte

Sie wissen jetzt, wie Sie Materialien mit erweiterter Beleuchtung für Objekte in einer Szene einrichten. Außerdem wissen Sie, wie Sie die Objekte in das FBX-Format exportieren, das von Azure Remote Rendering unterstützt wird. Der nächste Schritt besteht darin, die FBX-Datei zu konvertieren und in Azure Remote Rendering visuell darzustellen.

>[!div class="nextstepaction"]
>[Schnellstart: Konvertieren eines Modells für das Rendering](../../quickstarts\convert-model.md)