---
title: Unterstützte Kartenstile | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie mehr über die verschiedenen Kartenrenderingstile, die von Microsoft Azure Maps unterstützt werden.
author: philmea
ms.author: philmea
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2eafe3c16a89723d55ec52fde785e9ec69e45e0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334044"
---
# <a name="azure-maps-supported-map-styles"></a>Unterstützte Kartenstile in Azure Maps
Azure Maps unterstützt mehrere integrierte Kartenstile, die im Folgenden beschrieben werden.

## <a name="road"></a>Straße
Der Kartenstil **Straße** ist eine Standardkarte, die Straßen, natürliche und künstliche Strukturen zusammen mit den Bezeichnungen für diese Strukturen anzeigt.

![Kartenstil „Straße“](./media/supported-map-styles/road.png)

**Anwendbare APIs:**
* [Kartenbild](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="blank-and-blank_accessible"></a>„blank“ und „blank_accessible“

Die Kartenstile **blank** und **blank_accessible** stellen einen leeren Zeichenbereich bereit, in dem Daten visualisiert werden können. Der Stil **blank_accessible** wird weiterhin Updates für die Sprachausgabe mit Details zum Standort der Karte bereitstellen, obwohl die Basiskarte nicht angezeigt wird.

> [!Note]
> Im Web-SDK können Sie die Hintergrundfarbe der Karte ändern, indem Sie den CSS-Stil `background-color` des DIV-Kartenelements festlegen.

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement

## <a name="satellite"></a>Satellit 
Der Kartenstil **Satellit** ist eine Kombination aus Satellitenbildern und Luftaufnahmen.

![Kartenstil „Satellitenkachel“](./media/supported-map-styles/satellite.png)

**Anwendbare APIs:**
* [Satellitenkachel](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="satellite_road_labels"></a>Satellite_Road_Labels
Dieser Kartenstil ist eine Hybriddarstellung, bei der Straßen und Bezeichnungen Satellitenbilder und Luftaufnahmen überlagern.

![Kartenstil „satellite_road_labels“](./media/supported-map-styles/satellite-road-labels.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="grayscale_dark"></a>grayscale_dark
**grayscale_dark** ist eine dunkle Version des Kartenstils „Straße“.

![Kartenstil „gray_scale“](./media/supported-map-styles/grayscale-dark.png)

**Anwendbare APIs:**
* [Kartenbild](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web-SDK-Kartensteuerelement 
* Android-Kartensteuerelement


## <a name="grayscale_light"></a>grayscale_light
**grayscale_light** ist eine helle Version des Stils „Straßenkarte“.

![Kartenstil „grayscale_light“](./media/supported-map-styles/grayscale-light.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement


## <a name="night"></a>Nacht
**Nacht** ist eine dunkle Version des Kartenstils „Straße“ mit farbigen Straßen und Symbolen.

![Kartenstil „Nacht“](./media/supported-map-styles/night.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="road_shaded_relief"></a>road_shaded_relief
**road shaded relief** (Relief mit schattierten Straßen) ist eine Azure Maps-Hauptformatvorlage mit Konturen der Erde.

![Kartenstil „Schattiertes Relief“](./media/supported-map-styles/shaded-relief.png)

**Anwendbare APIs:**
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="high_contrast_dark"></a>high_contrast_dark

**high_contrast_dark** ist ein dunkler Kartenstil mit einem höheren Kontrast als bei anderen Stilen.

![Dunkler Kartenstil mit starkem Kontrast](./media/supported-map-styles/high-contrast-dark.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement

## <a name="next-steps"></a>Nächste Schritte

Informationen, wie ein Kartenstil in Azure Maps festgelegt wird:

> [!div class="nextstepaction"]
> [Auswählen eines Kartenstils](https://docs.microsoft.com/azure/azure-maps/choose-map-style)
