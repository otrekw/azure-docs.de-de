---
title: 'Räumliche Analyse: Kameraplatzierung'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Kamera für die räumliche Analyse einrichten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 01/12/2021
ms.author: aahi
ms.openlocfilehash: 4c2d1cd1c73b377d85501fd31d0f5a1893df5183
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102487422"
---
# <a name="camera-placement-guide"></a>Leitfaden zur Kameraplatzierung

Dieser Artikel enthält Empfehlungen zur Platzierung der Kamera für räumliche Analysen (Public Preview). Dies umfasst allgemeine Richtlinien sowie spezifische Empfehlungen für Höhe, Winkel und Kameraabstand für alle damit zusammenhängenden Vorgänge. 

> [!NOTE]
> Dieser Leitfaden ist für die Kamera Axis M3045-V konzipiert. Diese Kamera verwendet eine Auflösung von 1.920 × 1.080 mit einem horizontalen Sichtfeld von 106°, einem vertikalen Sichtfeld 59° und einer festen Brennweite von 2,8 mm. Die unten aufgeführten Prinzipien gelten für alle Kameras, aber die spezifischen Richtlinien für Höhe und Kameraabstand müssen bei der Verwendung einer anderen Kamera angepasst werden. 

## <a name="general-guidelines"></a>Allgemeine Richtlinien

Beachten Sie die folgenden allgemeinen Richtlinien für die Positionierung von Kameras für räumliche Analysen:

* **Beleuchtungshöhe:** Platzieren Sie Kameras unter Lampen, damit diese nicht das Sichtfeld der Kameras verdecken.
* **Hindernisse:** Um das Verdecken des Sichtfelds von Kameras zu vermeiden, berücksichtigen Sie Hindernisse wie z. B. Säulen, Schilder, Regale, Wände und vorhandene LP-Kameras.
* **Umgebungshintergrundbeleuchtung:** Die Hintergrundbeleuchtung von außen wirkt sich auf die Qualität der Kamerabilder aus. Vermeiden Sie starke Hintergrundbeleuchtung, indem Sie die Kameras nicht auf Fenster oder Glastüren nach außen ausrichten.
* **Lokale Datenschutzregeln und -bestimmungen:** Lokale Bestimmungen schränken möglicherweise ein, was durch Kameras erfasst werden darf. Machen Sie sich vor dem Platzieren von Kameras mit den lokalen Regeln und Bestimmungen vertraut.
* **Gebäudestruktur:** Heizungs-, Lüftungs- und Klimaanlagen, Sprinkler und die Verkabelung können die feste Montage von Kameras einschränken.
* **Kabelverwaltung:** Vergewissern Sie sich, dass es möglich ist, ein Ethernetkabel vom geplanten Einbauort der Kamera bis zum PoE-Switch (Power over Internet) zu verlegen.

## <a name="height-focal-point-distance-and-angle"></a>Höhe, Fokuspunktabstand und Winkel

Sie müssen bei der Entscheidung für den Einbau einer Kamera für räumliche Analysen drei Dinge berücksichtigen:
- Kamerahöhe
- Kameraabstand
- Winkel der Kamera relativ zur Bodenebene

Es ist auch wichtig, wenn möglich die Richtung zu kennen, in die die Mehrzahl der Menschen in Bezug auf das Sichtfeld der Kamera gehen. Diese Richtung ist wichtig für die Systemleistung.

![Bild einer Person, die in eine Richtung geht](./media/spatial-analysis/person-walking-direction.png)

In der folgenden Abbildung wird die gehende Person von oben gezeigt.

![Draufsicht und Planansicht](./media/spatial-analysis/person-walking-direction-diagram.png)

## <a name="camera-height"></a>Kamerahöhe

Im Allgemeinen sollten Kameras 3–4 m über dem Boden montiert werden. Für die Gesichtsmaskenerkennung empfiehlt es sich, dass Kameras acht bis zwölf Fuß vom Boden montiert werden. Berücksichtigen Sie beim Planen der Kameramontage in dieser Höhe Hindernisse (z. B. Regale, Hängeleuchten, Deckenschilder und Vitrinen), die die Kamerasicht beeinträchtigen können, und passen Sie die Höhe ggf. an.

## <a name="camera-to-focal-point-distance"></a>Kameraabstand

Der _Kameraabstand_ ist die lineare Entfernung vom Fokuspunkt (oder Mittelpunkt des Kameraausschnitts) zur Kamera, auf dem Boden gemessen.

![Kameraabstand](./media/spatial-analysis/camera-focal-point.png)

Diese Entfernung wird auf der Bodenebene gemessen.

![Messen des Kameraabstands](./media/spatial-analysis/camera-focal-point-floor-plane.png)

Von oben sieht dies folgendermaßen aus:

![Messen des Kameraabstands von oben](./media/spatial-analysis/camera-focal-point-above.png)

Ermitteln Sie anhand der folgenden Tabelle den Abstand der Kamera vom Fokuspunkt basierend auf der Montagehöhe. Diese Abstände sorgen für eine optimale Platzierung. Beachten Sie, dass die Tabelle auch Hinweis für Höhen unter den empfohlenen 3–4 m enthält, da die Höhe bei einigen Gebäuden eingeschränkt sein kann. Für die Gesichtsmaskenerkennung beträgt der empfohlene Kameraabstand (min/max) vier bis zehn Zoll bei einer Kamerahöhe zwischen acht und zwölf Zoll.

| Kamerahöhe | Kameraabstand (min./max.) |  
| ------------- | ---------------------------------------- |  
| 8'            | 4,6'–8'                                  |  
| 10'           | 5,8'–10'                                 |  
| 12'           | 7'–12'                                   |  
| 14'           | 8'–14'                                  |  
| 16'           | 9,2'–16'                                 |  
| 20'           | 11,5'–20'                                |  

In der folgenden Abbildung wird die Kameraaufnahme mit dem geringsten und höchsten Kameraabstand simuliert.

| Geringster Abstand                                      | Höchster Abstand                                      |  
| -------------------------------------------- | --------------------------------------------- |  
| ![Geringster Kameraabstand](./media/spatial-analysis/focal-point-closest.png) | ![Höchster Kameraabstand](./media/spatial-analysis/focal-point-farthest.png) |  

## <a name="camera-angle-mounting-ranges"></a>Montagespielraum für den Kamerawinkel

In diesem Abschnitt werden akzeptable Montagespielräume für den Kamerawinkel beschrieben. Diese Montagebereiche zeigen akzeptable Spielräume für eine optimale Platzierung.

### <a name="line-configuration"></a>Linienanordnung

Für den Vorgang **cognitiveservices.vision.spatialanalysis-personcrossingline** beträgt der optimale Winkel für die Kameramontage +/- 5 Grad, um eine möglichst hohe Genauigkeit zu erzielen.

Für die Gesichtsmaskenerkennung beträgt der optimale Winkel für die Kameramontage +/- 30 Grad bei einer Kamerahöhe zwischen acht und zwölf Zoll.

In der folgenden Abbildung werden Kameraaufnahmen mit den Empfehlungen ganz links (-) und rechts (+) für den Kamerawinkel für den Vorgang **cognitiveservices.vision.spatialanalysis-personcrossingline** gezeigt, um die eintretenden Personen zu zählen.

| Blick von ganz links                | Blick von ganz rechts                |  
| ---------------------------- | ----------------------------- |  
| ![Kamerawinkel ganz links](./media/spatial-analysis/camera-angle-left.png) | ![Kamerawinkel ganz rechts](./media/spatial-analysis/camera-angle-right.png) |  

Die folgende Abbildung zeigt die Kameraplatzierung und die Montagewinkel aus der Vogelperspektive.

![Vogelperspektive](./media/spatial-analysis/camera-angle-top.png)

### <a name="zone-configuration"></a>Zonenkonfiguration

Es wird empfohlen, die Kameras 3 m oder höher über dem Boden zu platzieren, um sicherzustellen, dass der abgedeckte Bereich groß genug ist. 

Wenn sich die Zone neben einem Hindernis wie einer Wand oder einem Regal befindet, montieren Sie die Kameras in der angegebenen Entfernung vom Ziel innerhalb des zulässigen 120-Grad-Winkelbereichs, wie in der folgenden Abbildung dargestellt.

![Akzeptabler Kamerawinkel](./media/spatial-analysis/camera-angle-acceptable.png)

In der folgenden Abbildung werden Simulationen für die Kameraaufnahmen eines Bereichs neben einem Regal von ganz links und ganz rechts gezeigt.

| Sicht von links        | Sicht von rechts        |  
| ---------------- | ----------------- |  
| ![Sicht von links](./media/spatial-analysis/end-cap-left.png) | ![Sicht von rechts](./media/spatial-analysis/end-cap-right.png) |  

#### <a name="queues"></a>Warteschlangen

Die Skills **cognitiveservices.vision.spatialanalysis-personcount**, **cognitiveservices.vision.spatialanalysis-persondistance** und **cognitiveservices.vision.spatialanalysis-personcrossingpolygon** können zum Überwachen von Warteschlangen verwendet werden. Für eine optimale Datenqualität in Warteschlangen werden Absperrungen mit Rückzuggurten empfohlen, um das Verdecken von Personen in der Warteschlange zu minimieren und eine einheitliche Position in der Warteschlange über einen Zeitraum sicherzustellen.

![Warteschlange mit Absperrgurten](./media/spatial-analysis/retractable-belt-queue.png)

Diese Art von Absperrungen wird gegenüber nicht transparenten Barrieren für die Warteschlangensteuerung bevorzugt, um die Genauigkeit der Erkenntnisse vom System zu maximieren.

Es gibt zwei Arten von Warteschlangen: linear und Zickzack.

Die folgende Abbildung zeigt Empfehlungen für lineare Warteschlangen:

![Empfehlung für lineare Warteschlangen](./media/spatial-analysis/camera-angle-linear-queue.png)

In der folgenden Abbildung werden Simulationen für die Kameraaufnahmen von linearen Warteschlangen von ganz links und ganz rechts gezeigt. Beachten Sie, dass Sie die Kamera auf der gegenüberliegenden Seite der Warteschlange montieren können.

| Sicht von links                          | Sicht von rechts                          |  
| ---------------------------------- | ----------------------------------- |  
| ![Linker Winkel bei linearer Warteschlange](./media/spatial-analysis/camera-angle-linear-left.png) | ![Rechter Winkel bei linearer Warteschlange](./media/spatial-analysis/camera-angle-linear-right.png) |  

Bei Zickzack-Warteschlangen sollte die Kamera nicht direkt in Richtung der Warteschlange platziert werden, wie in der folgenden Abbildung dargestellt. Beachten Sie, dass jedes der vier Beispiele für Kamerapositionen in der Abbildung die ideale Sicht mit einer akzeptablen Abweichung von +/- 15° in jeder Richtung bereitstellt.

In den folgenden Abbildungen wird die Sicht von einer Kamera simuliert, die an den idealen Positionen für eine Zickzack-Warteschlange platziert ist.

| Sicht 1        | Sicht 2        |  
| ------------- | ------------- |  
| ![Sicht 1](./media/spatial-analysis/camera-angle-ideal-location-right.png) | ![Sicht 2](./media/spatial-analysis/camera-angle-ideal-location-left.png) |  

| Sicht 3 |  Sicht 4 |  
| ---- | ----  |
| ![Sicht 3](./media/spatial-analysis/camera-angle-ideal-location-back.png) |  ![Sicht 4](./media/spatial-analysis/camera-angle-ideal-location-back-left.png) | 

##### <a name="organic-queues"></a>Organische Warteschlangen

Organische Warteschlangen formieren sich von allein. Diese Art von Warteschlangen ist akzeptabel, wenn die Warteschlangen nicht länger als 2–3 Personen sind und sich die Schlangen innerhalb der Zonendefinition bilden. Wenn die Warteschlangen in der Regel länger als 2–3 Personen sind, empfiehlt sich die Verwendung von Absperrgurten, um die Richtung der Warteschlange zu steuern und sicherzustellen, dass sich die Schlangen innerhalb der Zonendefinition bilden.

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen einer Webanwendung für die Erfassung der Personenanzahl](spatial-analysis-web-app.md)
* [Konfigurieren von Vorgängen zur räumlichen Analysen](./spatial-analysis-operations.md)
* [Protokollierung und Problembehandlung](spatial-analysis-logging.md)
* [Leitfaden zur Platzierung von Zonen und Linien](spatial-analysis-zone-line-placement.md)
