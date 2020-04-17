---
title: Grafikbindung
description: Einrichten von Grafikbindungen und Anwendungsfälle
author: florianborn71
manager: jlyons
services: azure-remote-rendering
titleSuffix: Azure Remote Rendering
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.service: azure-remote-rendering
ms.openlocfilehash: 8b5db0532f3dcc8b6dfb024238d0cacff2e6d2a1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679412"
---
# <a name="graphics-binding"></a>Grafikbindung

Um Azure-Remote Rendering in einer benutzerdefinierten Anwendung verwenden zu können, ist eine Integration in die Renderingpipeline der Anwendung erforderlich. Für diese Integration ist die Grafikbindung zuständig.

Nach der Einrichtung ermöglicht die Grafikbindung den Zugriff auf diverse Funktionen, die sich auf das gerenderte Bild auswirken. Diese Funktionen können in zwei Kategorien unterteilt werden: allgemeine Funktionen, die immer verfügbar sind, und spezifische Funktionen, die nur für den ausgewählten `Microsoft.Azure.RemoteRendering.GraphicsApiType` relevant sind.

## <a name="graphics-binding-in-unity"></a>Grafikbindung in Unity

In Unity wird die gesamte Bindung von der `RemoteUnityClientInit`-Struktur behandelt, die an `RemoteManagerUnity.InitializeManager` übergeben wird. Zum Festlegen des Grafikmodus muss das Feld `GraphicsApiType` auf die ausgewählte Bindung festgelegt werden. Das Feld wird abhängig davon, ob ein XRDevice vorhanden ist, automatisch gefüllt. Das Verhalten kann mit den folgenden Verhaltensweisen manuell außer Kraft gesetzt werden:

* **HoloLens 2**: Es wird immer die Grafikbindung [Windows Mixed Reality](#windows-mixed-reality) verwendet.
* **Flat UWP-Desktop-App**: Es wird immer [Simulation](#simulation) verwendet. Um diesen Modus zu verwenden, müssen Sie die Schritte im [Tutorial: Einrichten eines ganz neuen Unity-Projekts](../tutorials/unity/project-setup.md) ausführen.
* **Unity-Editor**: [Simulation](#simulation) wird immer verwendet, es sei denn, ein WMR VR-Headset ist angeschlossen. In diesem Fall wird ARR deaktiviert, um das Debuggen der nicht-ARR-bezogenen Teile der Anwendung zu ermöglichen. Siehe auch [Holographic Remoting](../how-tos/unity/holographic-remoting.md).

Der einzige weitere relevante Teil für Unity ist der Zugriff auf die [Basisbindung](#access). Alle anderen folgenden Abschnitte können übersprungen werden.

## <a name="graphics-binding-setup-in-custom-applications"></a>Einrichten von Grafikbindungen in benutzerdefinierten Anwendungen

Führen Sie die folgenden zwei Schritte aus, um eine Grafikbindung auszuwählen: Zuerst muss die Grafikbindung beim Initialisieren des Programms statisch initialisiert werden:

``` cs
RemoteRenderingInitialization managerInit = new RemoteRenderingInitialization;
managerInit.graphicsApi = GraphicsApiType.WmrD3D11;
managerInit.connectionType = ConnectionType.General;
managerInit.right = ///...
RemoteManagerStatic.StartupRemoteRendering(managerInit);
```

Der obige Aufruf ist erforderlich, um das Azure Remote Rendering in den Holographic-APIs zu initialisieren. Diese Funktion muss aufgerufen werden, bevor eine Holographic-API aufgerufen wird und bevor auf andere Remote Rendering-APIs zugegriffen wird. Ebenso muss die entsprechende Deinitialisierungsfunktion `RemoteManagerStatic.ShutdownRemoteRendering();` aufgerufen werden, wenn keine Holographic-APIs mehr aufgerufen werden.

## <a name="span-idaccessaccessing-graphics-binding"></a><span id="access">Zugreifen auf Grafikbindungen

Sobald ein Client eingerichtet ist, kann mit dem `AzureSession.GraphicsBinding`-Getter auf die Basisgrafikbindung zugegriffen werden. Beispielsweise kann die letzte Framestatistik wie folgt abgerufen werden:

``` cs
AzureSession currentSesson = ...;
if (currentSesson.GraphicsBinding)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        ...
    }
}
```

## <a name="graphic-apis"></a>Grafik-APIs

Derzeit sind zwei Grafik-APIs vorhanden, die ausgewählt werden können: `WmrD3D11` und `SimD3D11`. Eine Dritte (`Headless`) ist vorhanden, wird jedoch clientseitig noch nicht unterstützt.

### <a name="windows-mixed-reality"></a>Windows Mixed Reality

`GraphicsApiType.WmrD3D11` ist die Standardbindung, die auf HoloLens 2 ausgeführt wird. Damit wird die `GraphicsBindingWmrD3d11`-Bindung erstellt. In diesem Modus wird Azure Remote Rendering direkt in die Holographic-APIs eingebunden.

Um auf die abgeleiteten Grafikbindungen zuzugreifen, muss die Basis-`GraphicsBinding` umgewandelt werden.
Es müssen zwei Vorgänge ausgeführt werden, um die WMR-Bindung zu verwenden:

#### <a name="inform-remote-rendering-of-the-used-coordinate-system"></a>Informieren von Remote Rendering über das verwendete Koordinatensystems

``` cs
AzureSession currentSesson = ...;
IntPtr ptr = ...; // native pointer to ISpatialCoordinateSystem
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
if (binding.UpdateUserCoordinateSystem(ptr) == Result.Success)
{
    ...
}
```

Dabei muss der obige `ptr` ein Zeiger auf ein natives `ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem`-Objekt sein, welches das globale Koordinatensystem definiert, in dem Koordinaten in der API ausgedrückt werden.

#### <a name="render-remote-image"></a>Rendern des Remotebildes

Am Anfang jedes Frames muss der Remoteframe in den Hintergrundpuffer gerendert werden. Hierfür wird `BlitRemoteFrame` aufgerufen. Dadurch werden sowohl Farb- als auch Tiefeninformationen in das derzeit gebundene Renderziel gefüllt. Daher muss dies unbedingt erfolgen, nachdem der Hintergrundpuffer als Renderziel gebunden wurde.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
binding.BlitRemoteFrame();
```

### <a name="simulation"></a>Simulation

`GraphicsApiType.SimD3D11` ist die Simulationsbindung. Bei ihrer Auswahl wird die `GraphicsBindingSimD3d11`-Grafikbindung erstellt. Über die Schnittstelle wird die Kopfbewegung simuliert (z. B. in einer Desktopanwendung) und ein monoskopisches Bild gerendert.
Die Einrichtung ist etwas komplizierter und funktioniert wie folgt:

#### <a name="create-proxy-render-target"></a>Erstellen eines Proxyrenderziels

Remoteinhalte und lokale Inhalte müssen in einem Renderziel mit nicht sichtbarer Farbe/Tiefe namens „Proxy“ gerendert werden; hierbei werden die von der `GraphicsBindingSimD3d11.Update`-Funktion bereitgestellten Proxykameradaten verwendet. Der Proxy muss der Auflösung des Hintergrundpuffers entsprechen. Sobald eine Sitzung bereit ist, muss vor der Verbindungsherstellung `GraphicsBindingSimD3d11.InitSimulation` aufgerufen werden:

``` cs
AzureSession currentSesson = ...;
IntPtr d3dDevice = ...; // native pointer to ID3D11Device
IntPtr color = ...; // native pointer to ID3D11Texture2D
IntPtr depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
simBinding.InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

Die Init-Funktion muss mit Zeigern auf das native D3D-Gerät sowie auf die Farb- und Tiefentextur des Proxyrenderziels bereitgestellt werden. Nach der Initialisierung können `AzureSession.ConnectToRuntime` und `DisconnectFromRuntime` mehrmals aufgerufen werden. Wenn Sie jedoch zu einer anderen Sitzung wechseln, muss `GraphicsBindingSimD3d11.DeinitSimulation` zuerst in der alten Sitzung aufgerufen werden, bevor `GraphicsBindingSimD3d11.InitSimulation` in einer anderen Sitzung aufgerufen werden kann.

#### <a name="render-loop-update"></a>Aktualisierung der Renderschleife

Die Aktualisierung der Renderschleife umfasst mehrere Schritten:

1. Für jeden Frame wird vor dem Rendering `GraphicsBindingSimD3d11.Update` mit der aktuellen Kameratransformation aufgerufen, die an den Server für das Rendering gesendet wird. Gleichzeitig ist die zurückgegebene Proxytransformation auf die Proxykamera anzuwenden, um sie in das Proxyrenderziel zu rendern.
Wenn die zurückgegebene Proxyaktualisierung `SimulationUpdate.frameId` NULL ist, sind noch keine Remotedaten vorhanden. In diesem Fall müssen anstelle des Renderings in das Proxyrenderziel lokale Inhalte mit den aktuellen Kameradaten direkt in den Hintergrundpuffer gerendert werden, und die nächsten beiden Schritte werden übersprungen.
1. Die Anwendung stellt nun eine Bindung mit dem Proxyrenderziel her und ruft `GraphicsBindingSimD3d11.BlitRemoteFrameToProxy` auf. Dadurch werden die Remoteinformationen zu Farbe und Tiefe im Proxyrenderziel gefüllt. Alle lokalen Inhalte können nun mithilfe der Proxykameratransformation auf dem Proxy gerendert werden.
1. Anschließend muss der Hintergrundpuffer als Renderziel gebunden und `GraphicsBindingSimD3d11.ReprojectProxy` aufgerufen werden; an diesem Punkt kann der Hintergrundpuffer dargestellt werden.

``` cs
AzureSession currentSesson = ...;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
SimulationUpdate update = new SimulationUpdate();
// Fill out camera data with current camera data
...
SimulationUpdate proxyUpdate = new SimulationUpdate();
simBinding.Update(update, out proxyUpdate);
// Is the frame data valid?
if (proxyUpdate.frameId != 0)
{
    // Bind proxy render target
    simBinding.BlitRemoteFrameToProxy();
    // Use proxy camera data to render local content
    ...
    // Bind back buffer
    simBinding.ReprojectProxy();
}
else
{
    // Bind back buffer
    // Use current camera data to render local content
    ...
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Einrichten eines ganz neuen Unity-Projekts](../tutorials/unity/project-setup.md)
