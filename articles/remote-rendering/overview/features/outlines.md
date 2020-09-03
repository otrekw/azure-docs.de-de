---
title: Konturenrendering
description: In diesem Artikel wird erläutert, wie Konturenrendering für ausgewählte Objekte funktioniert.
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: a3b4ba62072e26f16a0e39416c9ae346d1acefd9
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88997522"
---
# <a name="outline-rendering"></a>Konturenrendering

Ausgewählte Objekte können visuell hervorgehoben werden, indem über die [Komponente für hierarchische Zustandsüberschreibung](../../overview/features/override-hierarchical-state.md) Konturenrendering hinzugefügt wird. In diesem Kapitel wird erläutert, wie globale Parameter für das Konturenrendering über die Client-API geändert werden.

Bei Kontureneigenschaften handelt es sich um eine globale Einstellung. Alle Objekte mit Konturenrendering verwenden dieselbe Einstellung. Es ist nicht möglich, Farben nur für die Konturen einzelner Objekte zu verwenden.

## <a name="parameters-for-outlinesettings"></a>Parameter für `OutlineSettings`

Die Klasse `OutlineSettings` enthält die Einstellungen für globale Kontureneigenschaften. Sie macht die folgenden Member verfügbar:

| Parameter      | type    | BESCHREIBUNG                                             |
|----------------|---------|---------------------------------------------------------|
| `Color`          | Color4Ub | Mit diesem Parameter wird die Farbe festgelegt, die für die Konturen verwendet wird. Der Alphateil wird ignoriert.         |
| `PulseRateHz`    | float   | Mit diesem Parameter wird die Oszillationsrate der Konturen pro Sekunde festgelegt.|
| `PulseIntensity` | float   | Mit diesem Parameter wird die Intensität des Pulsierens der Konturen festgelegt. Der Wert muss zwischen 0,0 für kein Pulsieren und 1,0 für volles Pulsieren liegen. Die Intensität legt die Mindestdeckkraft der Konturen implizit auf `MinOpacity = 1.0 - PulseIntensity` fest. |

![Konturen](./media/outlines.png) Auswirkungen der Änderung des Parameters `color` von Gelb (links) in Magenta (Mitte) und des Parameters `pulseIntensity` von 0 in 0,8 (rechts)

## <a name="example"></a>Beispiel

Der folgende Code ist ein Beispiel für das Festlegen von Konturenparametern über die API:

```cs
void SetOutlineParameters(AzureSession session)
{
    OutlineSettings outlineSettings = session.Actions.OutlineSettings;
    outlineSettings.Color = new Color4Ub(255, 255, 0, 255);
    outlineSettings.PulseRateHz = 2.0f;
    outlineSettings.PulseIntensity = 0.5f;
}
```

```cpp
void SetOutlineParameters(ApiHandle<AzureSession> session)
{
    ApiHandle<OutlineSettings> outlineSettings = session->Actions()->GetOutlineSettings();
    Color4Ub outlineColor;
    outlineColor.channels = { 255, 255, 0, 255 };
    outlineSettings->SetColor(outlineColor);
    outlineSettings->SetPulseRateHz(2.0f);
    outlineSettings->SetPulseIntensity(0.5f);
}
```

## <a name="performance"></a>Leistung

Konturenrendering kann die Renderingleistung erheblich beeinflussen. Dieser Einfluss variiert basierend auf dem Platzverhältnis in Bezug auf den verwendeten Bildschirmplatz zwischen ausgewählten und nicht ausgewählten Objekten in einem bestimmten Frame.

## <a name="next-steps"></a>Nächste Schritte

* [Komponente für hierarchische Zustandsüberschreibung](../../overview/features/override-hierarchical-state.md)
