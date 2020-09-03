---
title: Komponenten
description: Definition von Komponenten im Bereich von Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: d957da572bfdd3119dda506ac8f5bb42064d7758
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89020302"
---
# <a name="components"></a>Komponenten

Azure Remote Rendering verwendet das Muster [Entitätskomponentensystem](https://en.wikipedia.org/wiki/Entity_component_system). Während [Entitäten](entities.md) die Position und die hierarchische Komposition von Objekten darstellen, sind Komponenten für das Implementieren von Verhalten verantwortlich.

Die am häufigsten verwendeten Komponententypen sind [:::no-loc text="mesh components":::](meshes.md), die der Renderingpipeline Gittermodelle hinzufügen. Analog dazu werden [Lichtkomponenten](../overview/features/lights.md) verwendet, um Beleuchtung hinzuzufügen, und [Schnittebenenkomponenten](../overview/features/cut-planes.md) werden genutzt, um Gittermodelle aufzuschneiden.

Alle diese Komponenten verwenden die Transformation (Position, Drehung, Maßstab) der Entität, an die sie angefügt sind, als ihren Bezugspunkt.

## <a name="working-with-components"></a>Arbeiten mit Komponenten

Sie können Komponenten problemlos programmgesteuert hinzufügen, entfernen und bearbeiten:

```cs
// create a point light component
AzureSession session = GetCurrentlyConnectedSession();
PointLightComponent lightComponent = session.Actions.CreateComponent(ObjectType.PointLightComponent, ownerEntity) as PointLightComponent;

lightComponent.Color = new Color4Ub(255, 150, 20, 255);
lightComponent.Intensity = 11;

// ...

// destroy the component
lightComponent.Destroy();
lightComponent = null;
```

```cpp
// create a point light component
ApiHandle<AzureSession> session = GetCurrentlyConnectedSession();

ApiHandle<PointLightComponent> lightComponent = session->Actions()->CreateComponent(ObjectType::PointLightComponent, ownerEntity)->as<PointLightComponent>();

// ...

// destroy the component
lightComponent->Destroy();
lightComponent = nullptr;
```


Eine Komponente wird zum Erstellungszeitpunkt an eine Entität angefügt. Anschließend kann sie nicht mehr in eine andere Entität verschoben werden. Komponenten werden explizit mit `Component.Destroy()` oder automatisch gelöscht, wenn die Besitzerentität der Komponente zerstört wird.

Der Entität kann jeweils nur eine Instanz eines einzelnen Komponentyps hinzugefügt werden.

## <a name="unity-specific"></a>Unity-spezifisch

Die Unity-Integration stellt zusätzliche Erweiterungsfunktionen für die Interaktion mit-Komponenten bereit. Weitere Informationen dazu finden Sie unter [Interagieren mit Spielobjekten und Komponenten](../how-tos/unity/objects-components.md).

## <a name="next-steps"></a>Nächste Schritte

* [Objektbegrenzungen](object-bounds.md)
* [Gittermodelle](meshes.md)
