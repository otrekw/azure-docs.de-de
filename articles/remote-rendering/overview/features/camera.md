---
title: Kamera
description: Beschreibt Kameraeinstellungen und Anwendungsfälle
author: christophermanthei
ms.author: chmant
ms.date: 03/07/2020
ms.topic: article
ms.openlocfilehash: fc82d046caa3663cffcda585258642813ab3a7d8
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92207256"
---
# <a name="camera"></a>Kamera

Um sicherzustellen, dass lokal und remote gerenderte Inhalte nahtlos zusammengesetzt werden können, müssen verschiedene Kameraeigenschaften zwischen Server und Client synchron bleiben. Vor allem die Kameratransformation und -projektion. Beispielsweise muss für die lokal gerenderten Inhalte die gleiche Kameratransformation und -projektion verwendet werden, mit denen der letzte Remoteframe gerendert wurde.

![Lokale und Remote-Kamera](./media/camera.png)

In der Abbildung oben hat sich die lokale Kamera gegenüber dem vom Server gesendeten Remoteframe bewegt. Das hat zur Folge, dass die lokalen Inhalte aus der gleichen Perspektive wie der Remoteframe gerendert werden müssen, und schließlich wird das resultierende Bild erneut in den lokalen Kameraraum projiziert, was im Detail unter [Late Stage Reprojection (LSR)](late-stage-reprojection.md) erläutert wird.

Die Verzögerung zwischen dem Remote- und dem lokalen Rendering bedeutet, dass jede Änderung dieser Einstellungen mit einer Verzögerung angewendet wird. Daher kann jeder neue Wert nicht sofort innerhalb des gleichen Frames verwendet werden.

Die Abstände für nahe und ferne Ebenen können in den Kameraeinstellungen festgelegt werden. In der HoloLens 2 sind die verbleibenden Transformations- und Projektionsdaten durch die Hardware definiert. Wenn Sie die [Desktopsimulation](../../concepts/graphics-bindings.md) verwenden, werden sie über die Eingabe ihrer `Update`-Funktion festgelegt.

Die geänderten Werte können lokal verwendet werden, sobald das erste Remoteframe ankommt, das mit ihnen gerendert wurde. In HoloLens 2 werden die Daten, die für das Rendering verwendet werden sollen, vom *HolographicFrame* bereitgestellt, ganz wie in jeder anderen Holografieanwendung. In der [Desktopsimulation](../../concepts/graphics-bindings.md) werden sie über die Ausgabe der `Update`-Funktion abgerufen.

## <a name="camera-settings"></a>Kameraeinstellungen

Die folgenden Eigenschaften können in den Kameraeinstellungen geändert werden:

**Nahe und ferne Ebene:**

Damit sichergestellt ist, dass keine ungültigen Bereiche festgelegt werden können, sind die Eigenschaften **NearPlane** und **FarPlane** schreibgeschützt, und es ist eine separate Funktion **SetNearAndFarPlane** zum Ändern des Bereichs implementiert. Diese Daten werden am Ende des Frames an den Server gesendet.

> [!IMPORTANT]
> In Unity geschieht dies automatisch, wenn die nahe und ferne Ebene der Hauptkamera geändert werden.

**EnableDepth**:

Manchmal ist es nützlich, den Schreibvorgang des Tiefenpuffers für das Remotebild zu Debuggingzwecken zu deaktivieren. Durch das Deaktivieren der Tiefe beendet der Server außerdem das Senden von Tiefeninformationen, wodurch sich die Bandbreite verringert.

> [!TIP]
> In Unity steht eine Debugkomponente mit dem Namen **EnableDepthComponent** zur Verfügung, die zum Umschalten dieses Features in der Editor-Benutzeroberfläche verwendet werden kann.

Das Ändern der Kameraeinstellungen kann wie folgt erfolgen:

```cs
void ChangeCameraSetting(AzureSession session)
{
    CameraSettings settings = session.Actions.CameraSettings;

    settings.SetNearAndFarPlane(0.1f, 20.0f);
    settings.EnableDepth = false;
}
```

```cpp
void ChangeStageSpace(ApiHandle<AzureSession> session)
{
    ApiHandle<CameraSettings> settings = session->Actions()->GetCameraSettings();

    settings->SetNearAndFarPlane(0.1f, 20.0f);
    settings->SetEnableDepth(false);
}
```

## <a name="api-documentation"></a>API-Dokumentation

* [C# CameraSettings](/dotnet/api/microsoft.azure.remoterendering.camerasettings)
* [C++ CameraSettings](/cpp/api/remote-rendering/camerasettings)
* [C# GraphicsBindingSimD3d11.Update-Funktion](/dotnet/api/microsoft.azure.remoterendering.graphicsbindingsimd3d11.update)
* [C++ GraphicsBindingSimD3d11::Update-Funktion](/cpp/api/remote-rendering/graphicsbindingsimd3d11#update)

## <a name="next-steps"></a>Nächste Schritte

* [Grafikbindung](../../concepts/graphics-bindings.md)
* [Late Stage Reprojection (LSR)](late-stage-reprojection.md)