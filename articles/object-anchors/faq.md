---
title: Häufig gestellte Fragen
description: Dies sind häufig gestellte Fragen zum Azure Object Anchors-Dienst.
author: craigktreasure
manager: vriveras
ms.author: crtreasu
ms.date: 04/01/2020
ms.topic: overview
ms.service: azure-object-anchors
ms.openlocfilehash: 6de027f317072512bd9893303d3a79d0f34f3e3e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747745"
---
# <a name="frequently-asked-questions-about-azure-object-anchors"></a>Häufig gestellte Fragen zu Azure Object Anchors

Azure Object Anchors ermöglicht einer Anwendung mithilfe eines 3D-Modells das Erkennen eines Objekts in der physischen Welt und das Schätzen der räumlichen Lage nach den sechs Freiheitsgraden.

Weitere Informationen finden Sie unter [Azure Object Anchors-Übersicht](overview.md).

## <a name="product-faq"></a>Häufig gestellte Fragen zum Produkt
**F: Welche Empfehlungen gibt es für die zu verwendenden Objekte?**

**A:** Für Objekte werden die folgenden Merkmale empfohlen:

* Ein bis zehn Meter für jede Dimension
* Nicht symmetrisch und mit ausreichend geometrischen Variationen
* Niedrige Reflektivität (matte Oberflächen) mit heller Farbe
* Stationäre Objekte
* Keine oder kaum Artikulation
* Klare Hintergründe ohne bzw. mit nur wenig anderen herumliegenden Objekten
* Gescanntes Objekt sollte 1:1 dem Modell entsprechen, mit dem Sie trainiert haben

**F: Wie lauten die maximalen Objektabmessungen, die bei der Modellerfassung verarbeitet werden können?**

**A:** Alle Maße eines CAD-Modells sollten weniger als zehn Meter betragen.

**F: Wie lautet die maximale CAD-Modellgröße, die bei der Erfassung verarbeitet werden kann?**

**A:** Die Modelldateigröße sollte kleiner als 150 MB sein.

**F: Welche CAD-Formate werden unterstützt?**

**A:** Zurzeit werden die Dateitypen `fbx`, `ply`, `obj`, `glb` und `gltf` unterstützt.

**F: Welche Schwerkraftrichtung und Einheit erfordert der Modellerfassungsdienst? Wie werden diese ermittelt?**

**A:** Die Schwerkraftrichtung ist der Down-Vektor, der zur Erde zeigt. Bei CAD-Modellen ist die Richtung der Schwerkraft in der Regel das Gegenteil der Richtung „Nach oben“. In vielen Fällen steht „+Z“ für die Aufwärtsrichtung, sodass in diesem Fall „–Z“ oder `Vector3(0.0, 0.0, -1.0)` die Richtung der Schwerkraft darstellen würden. Beim Ermitteln der Schwerkraft sollten Sie nicht nur das Modell, sondern auch die Ausrichtung berücksichtigen, in der das Modell während der Laufzeit betrachtet wird. Wenn Sie versuchen, einen Stuhl in der realen Welt auf einer glatten Oberfläche zu erkennen, kann der Wert für die Schwerkraft `Vector3(0.0, 0.0, -1.0)` lauten. Wenn der Stuhl mit einer Neigung von 45 Grad positioniert ist, könnte die Schwerkraft `Vector3(0.0, -Sqrt(2)/2, -Sqrt(2)/2)` sein.

Die Richtung der Schwerkraft kann mit einem 3D-Renderingtool wie [MeshLab](http://www.meshlab.net/) ermittelt werden.

Die Einheit stellt die Maßeinheit des Modells dar. Die unterstützten Einheiten finden Sie mithilfe der Enumeration **Microsoft.Azure.ObjectAnchors.Ingestion.Unit**.

**F: Wie lange dauert es, um ein CAD-Modell zu erfassen?**

**A:** Für ein `ply`-Modell werden in der Regel 3 bis 15 Minuten benötigt. Wenn Sie Modelle in anderen Formaten übermitteln, müssen Sie je nach Dateigröße mit 15 bis 60 Minuten rechnen.

**F: Welche Geräte werden von Object Anchors unterstützt?**

**A:** HoloLens 2 

**F: Welcher Betriebssystembuild sollte mit HoloLens ausgeführt werden?**

**A:** Betriebssystembuild 18363.720 oder höher, Veröffentlichung nach dem 12. März 2020.

  Weitere Informationen finden Sie unter [12. März 2020 – KB4551762 (Betriebssystembuilds 18362.720 und 18363.720)](https://support.microsoft.com/help/4551762).

**F: Wie lange dauert es, ein Objekt auf einem HoloLens-Gerät zu erkennen?**

**A:** Dies hängt von der Objektgröße und dem Scanvorgang ab. Versuchen Sie, die bewährten Methoden für einen gründlichen Scanvorgang zu befolgen, um Objekte schneller zu erkennen. Die Erkennung kann bei kleineren Objekten, deren Seitenmaße jeweils zwei Meter nicht überschreiten, innerhalb von wenigen Sekunden erfolgen. Bei größeren Objekten wie Autos sollte der Benutzer komplett um diese Objekte herumgehen, um eine zuverlässige Erkennung sicherzustellen. Dies bedeutet, dass für die Erkennung mehrere Sekunden benötigt werden.

**F: Welche bewährte Methoden gibt es für die Verwendung von Object Anchors in einer HoloLens-Anwendung?**

**A:**

 1. Führen Sie eine Augenkalibrierung aus, um ein genaues Rendering zu ermöglichen.
 2. Stellen Sie sicher, dass der Raum eine ideale visuelle Struktur und gute Lichtverhältnisse aufweist.
 3. Verwenden Sie ein feststehendes Objekt, um das keine anderen Objekte herumliegen.
 4. Optional können Sie den Cache [Spatial Mapping](https://docs.microsoft.com/windows/mixed-reality/spatial-mapping) (Räumliche Zuordnung) auf dem HoloLens-Gerät löschen.
 5. Scannen Sie das Objekt, indem Sie um es herumgehen. Stellen Sie sicher, dass der größte Teil des Objekts erfasst wird.
 6. Legen Sie einen ausreichend großen Suchbereich fest, um das Objekt abzudecken.
 7. Das Objekt sollte während der Erkennung nicht bewegt werden.
 8. Starten Sie die Objekterkennung, und visualisieren Sie das Rendering basierend auf der geschätzten räumlichen Lage.
 9. Sperren Sie erkannte Objekte, oder beenden Sie die Nachverfolgung, sobald die räumliche Lage stabil und genau ist, um die Batterie zu schonen.

**F: Wie präzise ist die geschätzte räumliche Lage?**

**A:** Dies hängt beispielsweise von der Objektgröße, dem Material und der Umgebung ab. Bei kleinen Objekten kann die geschätzte räumliche Lage um bis zu zwei Zentimeter abweichen. Bei großen Objekten wie Autos können Abweichungen von zwei bis acht Zentimetern vorliegen.

**F: Kann Object Anchors sich bewegende Objekte verarbeiten?**

**A:** Das Verarbeiten von **sich ständig bewegenden** oder **dynamischen** Objekten wird nicht unterstützt.

**F: Kann Object Anchors Verformungen oder Artikulationen verarbeiten?**

**A:** Dies ist teilweise möglich und hängt davon ab, inwieweit sich der Umriss bzw. die Geometrie des Objekts aufgrund der Verformung oder Artikulation verändert. Wenn sich die Geometrie des Objekts stark ändert, kann der Benutzer ein weiteres Modell für diese Konfiguration erstellen und zur Erkennung verwenden.

**F: Wie viele verschiedene Objekte kann Object Anchors gleichzeitig erkennen?**

**A:** Zurzeit wird das Erkennen eines einzelnen Objektmodells unterstützt. 

**F: Kann Object Anchors mehrere Instanzen desselben Objektmodells erkennen?**

**A:** Ja, Sie können bis zu drei Objekte desselben Modelltyps erkennen. Die Anwendung kann mehrmals `ObjectObserver.DetectAsync` mit unterschiedlichen Abfragen aufrufen, um mehrere Instanzen des gleichen Modells zu erkennen.

**F: Was muss ich tun, wenn die Object Anchors-Runtime das Objekt nicht erkennen kann?**

**A:**

* Stellen Sie sicher, dass der Raum ausreichend Struktur aufweist, indem Sie einige Plakate aufhängen.
* Scannen Sie das Objekt noch genauer.
* Passen Sie die Modellparameter wie oben beschrieben an.
* Geben Sie einen engen Begrenzungsrahmen als Suchbereich ein, der alle oder die meisten Objekte enthält.
* Löschen Sie den Cache für die räumliche Zuordnung, und scannen Sie das Objekt erneut.
* Erfassen Sie Diagnosedaten, und senden Sie diese an uns.

**F: Wie wähle ich Objektabfrageparameter aus?**

**A:**

* Geben Sie genaue Suchbereiche an, um das vollständige Objekt präzise zu erfassen und die Geschwindigkeit und Genauigkeit bei der Erkennung zu erhöhen.
* Der Standardwert `ObjectQuery.MinSurfaceCoverage` des Objektmodells ist normalerweise gut. Verwenden Sie andernfalls einen kleineren Wert, damit die Erkennung schneller durchgeführt werden kann.
* Verwenden Sie einen kleinen Wert für `ObjectQuery.ExpectedMaxVerticalOrientationInDegrees`, wenn das Objekt schätzungsweise aufrecht positioniert ist.
* Eine App sollte immer ein `1:1`-Objektmodell für die Erkennung verwenden. Die Skalierung sollte in der Regel nahe 1 liegen und im Idealfall eine maximale Abweichung von einem Prozent aufweisen. Eine App kann `ObjectQuery.MaxScaleChange` auf `0` oder `0.1` festlegen, um die Skalierungsschätzung zu aktivieren oder zu deaktivieren und die räumliche Instanzlage qualitativ zu bewerten.

**F: Wie rufe ich Object Anchors-Diagnosedaten vom HoloLens-Gerät ab?**

**A:** Die Anwendung kann den Speicherort der Diagnosedatenarchive ermitteln. Die Object Anchors-Beispiel-App schreibt Diagnosedaten in den Ordner **TempState**.

**F: Warum wird das Quellmodell nicht am physischen Objekt ausgerichtet, wenn die vom Object Anchors-Unity-SDK zurückgegebene räumliche Lage verwendet wird?**

**A:** Unity ändert beim Importieren eines Objektmodells möglicherweise das Koordinatensystem. Beim Konvertieren invertiert das Object Anchors-Unity-SDK beispielsweise die Z-Achse von einem rechtshändigen in ein linkshändiges Koordinatensystem, aber Unity führt ggf. eine zusätzliche Drehung der X- oder Y-Achse aus. Ein Entwickler kann diese zusätzliche Drehung ermitteln, indem er die Koordinatensysteme visualisiert und vergleicht.

**F: Wird eine 2D-Darstellung unterstützt?**

**A:** Da die Modelle geometriebasiert sind, wird nur die 3D-Darstellung unterstützt.

**F: Kann zwischen demselben Modell in unterschiedlichen Farben differenziert werden?**

**A:** Da unsere Algorithmen geometriebasiert sind, verhalten sich unterschiedliche Farben des gleichen Modells während der Erkennung nicht unterschiedlich.

**F: Kann ich Object Anchors ohne Internetverbindung nutzen?**

**A:** 
* Für die Modellerfassung und das Training wird eine Internetverbindung benötigt, da diese Vorgänge über die Cloud durchgeführt werden.
* Laufzeitsitzungen erfolgen vollständig auf dem Gerät und erfordern keine Internetverbindung, da alle Berechnungen über HoloLens 2 erfolgen.

## <a name="privacy-faq"></a>Datenschutz: Häufig gestellte Fragen
**F: Wie werden Daten von Azure Object Anchors gespeichert?**

**A:** Es werden nur Systemmetadaten gespeichert, die im Ruhezustand mit einem von Microsoft verwalteten Datenverschlüsselungsschlüssel verschlüsselt werden.
