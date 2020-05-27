---
title: Einseitiges Rendering
description: Beschreibt Einstellungen für einseitiges Rendering sowie Anwendungsfälle.
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: 97e0456e274adee7d678e373cfd92b5003f3d801
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83759097"
---
# <a name="single-sided-rendering"></a>Einseitiges Rendering

Die meisten Renderer verwenden [Backface Culling](https://en.wikipedia.org/wiki/Back-face_culling), um die Leistung zu verbessern. Wenn die Gittermodelle jedoch mit [Schnittebenen](cut-planes.md) aufgeschnitten werden, sehen Benutzer häufig die Rückseite von Dreiecken. Wenn diese Dreiecke mit Culling entfernt werden, sieht das Ergebnis nicht überzeugend aus.

Sie können dieses Problem zuverlässig vermeiden, indem Sie Dreiecke *doppelseitig* rendern. Da die Nichtverwendung von Backface Culling Auswirkungen auf die Leistung besitzt, schaltet Azure Remote Rendering standardmäßig nur zum doppelseitigen Rendering bei Gittermodelln um, die sich mit einer Schnittebene überschneiden.

Mit der Einstellung für *einseitiges Rendering* können Sie dieses Verhalten anpassen.

> [!CAUTION]
> Die Einstellung für einseitiges Rendering ist ein experimentelles Feature. Möglicherweise wird es in Zukunft wieder entfernt. Ändern Sie die Standardeinstellung nicht, es sei denn, dies löst tatsächlich ein wichtiges Problem in Ihrer Anwendung.

## <a name="prerequisites"></a>Voraussetzungen

Die Einstellung für einseitiges Rendering wirkt sich nur auf Gittermodelle aus, die [konvertiert wurden](../../how-tos/conversion/configure-model-conversion.md), wobei die Option `opaqueMaterialDefaultSidedness` auf `SingleSided` festgelegt wurde. Standardmäßig ist diese Option auf `DoubleSided` festgelegt.

## <a name="single-sided-rendering-setting"></a>Einstellung für einseitiges Rendering

Es gibt drei verschiedene Modi:

**Normal:** In diesem Modus werden die Gittermodelle beim Konvertieren immer gerendert. Das bedeutet, dass Gittermodelle, die mit `opaqueMaterialDefaultSidedness` auf `SingleSided` festgelegt konvertiert wurden, immer mit aktiviertem Backface Culling gerendert werden, auch wenn sie sich mit einer Schnittebene überschneiden.

**DynamicDoubleSiding:** In diesem Modus wird automatisch in doppelseitiges Rendering umgeschaltet, wenn sich eine Schnittebene mit einem Gittermodell überschneidet. Dieser Modus ist der Standardmodus.

**AlwaysDoubleSided:** Erzwingt, dass die gesamte einseitige Geometrie immer doppelseitig gerendert wird. Dieser Modus wird meistens verwendet. Sie können daher die Leistungseinbußen zwischen einseitigem und doppelseitigem Rendering problemlos vergleichen.

Das Ändern der Einstellungen für einseitiges Rendering kann wie folgt erfolgen:

```cs
void ChangeSingleSidedRendering(AzureSession session)
{
    SingleSidedSettings settings = session.Actions.SingleSidedSettings;

    // Single-sided geometry is rendered as is
    settings.Mode = SingleSidedMode.Normal;

    // Single-sided geometry is always rendered double-sided
    settings.Mode = SingleSidedMode.AlwaysDoubleSided;
}
```

```cpp
void ChangeSingleSidedRendering(ApiHandle<AzureSession> session)
{
    ApiHandle<SingleSidedSettings> settings = *session->Actions()->SingleSidedSettings();

    // Single-sided geometry is rendered as is
    settings->Mode(SingleSidedMode::Normal);

    // Single-sided geometry is always rendered double-sided
    settings->Mode(SingleSidedMode::AlwaysDoubleSided);
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Schnittebenen](cut-planes.md)
* [Konfigurieren der Modellkonvertierung](../../how-tos/conversion/configure-model-conversion.md)
