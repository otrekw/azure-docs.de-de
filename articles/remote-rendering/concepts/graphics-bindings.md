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
ms.custom: devx-track-csharp
ms.openlocfilehash: 332213adf64e17c0935ddf612acac5bbca413a87
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91802292"
---
# <a name="graphics-binding"></a>Grafikbindung

Um Azure-Remote Rendering in einer benutzerdefinierten Anwendung verwenden zu können, ist eine Integration in die Renderingpipeline der Anwendung erforderlich. Für diese Integration ist die Grafikbindung zuständig.

Nach der Einrichtung ermöglicht die Grafikbindung den Zugriff auf diverse Funktionen, die sich auf das gerenderte Bild auswirken. Diese Funktionen können in zwei Kategorien unterteilt werden: allgemeine Funktionen, die immer verfügbar sind, und spezifische Funktionen, die nur für den ausgewählten `Microsoft.Azure.RemoteRendering.GraphicsApiType` relevant sind.

## <a name="graphics-binding-in-unity"></a>Grafikbindung in Unity

In Unity wird die gesamte Bindung von der `RemoteUnityClientInit`-Struktur behandelt, die an `RemoteManagerUnity.InitializeManager` übergeben wird. Zum Festlegen des Grafikmodus muss das Feld `GraphicsApiType` auf die ausgewählte Bindung festgelegt werden. Das Feld wird abhängig davon, ob ein XRDevice vorhanden ist, automatisch gefüllt. Das Verhalten kann mit den folgenden Verhaltensweisen manuell außer Kraft gesetzt werden:

* **HoloLens 2**: Es wird immer die Grafikbindung [Windows Mixed Reality](#windows-mixed-reality) verwendet.
* **Flat UWP-Desktop-App**: Es wird immer [Simulation](#simulation) verwendet.
* **Unity-Editor**: [Simulation](#simulation) wird immer verwendet, es sei denn, ein WMR VR-Headset ist angeschlossen. In diesem Fall wird ARR deaktiviert, um das Debuggen der nicht-ARR-bezogenen Teile der Anwendung zu ermöglichen. Siehe auch [Holographic Remoting](../how-tos/unity/holographic-remoting.md).

Der einzige weitere relevante Teil für Unity ist der Zugriff auf die [Basisbindung](#access). Alle anderen folgenden Abschnitte können übersprungen werden.

## <a name="graphics-binding-setup-in-custom-applications"></a>Einrichten von Grafikbindungen in benutzerdefinierten Anwendungen

Führen Sie die folgenden zwei Schritte aus, um eine Grafikbindung auszuwählen: Zuerst muss die Grafikbindung beim Initialisieren des Programms statisch initialisiert werden:

```cs
RemoteRenderingInitialization managerInit = new RemoteRenderingInitialization;
managerInit.graphicsApi = GraphicsApiType.WmrD3D11;
managerInit.connectionType = ConnectionType.General;
managerInit.right = ///...
RemoteManagerStatic.StartupRemoteRendering(managerInit);
```

```cpp
RemoteRenderingInitialization managerInit;
managerInit.graphicsApi = GraphicsApiType::WmrD3D11;
managerInit.connectionType = ConnectionType::General;
managerInit.right = ///...
StartupRemoteRendering(managerInit); // static function in namespace Microsoft::Azure::RemoteRendering
```

Der obige Aufruf ist erforderlich, um das Azure Remote Rendering in den Holographic-APIs zu initialisieren. Diese Funktion muss aufgerufen werden, bevor eine Holographic-API aufgerufen wird und bevor auf andere Remote Rendering-APIs zugegriffen wird. Ebenso muss die entsprechende Deinitialisierungsfunktion `RemoteManagerStatic.ShutdownRemoteRendering();` aufgerufen werden, wenn keine Holographic-APIs mehr aufgerufen werden.

## <a name="span-idaccessaccessing-graphics-binding"></a><span id="access">Zugreifen auf Grafikbindungen

Sobald ein Client eingerichtet ist, kann mit dem `AzureSession.GraphicsBinding`-Getter auf die Basisgrafikbindung zugegriffen werden. Beispielsweise kann die letzte Framestatistik wie folgt abgerufen werden:

```cs
AzureSession currentSession = ...;
if (currentSession.GraphicsBinding)
{
    FrameStatistics frameStatistics;
    if (currentSession.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        ...
    }
}
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
if (ApiHandle<GraphicsBinding> binding = currentSession->GetGraphicsBinding())
{
    FrameStatistics frameStatistics;
    if (*binding->GetLastFrameStatistics(&frameStatistics) == Result::Success)
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

```cs
AzureSession currentSession = ...;
IntPtr ptr = ...; // native pointer to ISpatialCoordinateSystem
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
if (wmrBinding.UpdateUserCoordinateSystem(ptr) == Result.Success)
{
    ...
}
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
void* ptr = ...; // native pointer to ISpatialCoordinateSystem
ApiHandle<GraphicsBindingWmrD3d11> wmrBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingWmrD3d11>();
if (*wmrBinding->UpdateUserCoordinateSystem(ptr) == Result::Success)
{
    //...
}
```

Dabei muss der obige `ptr` ein Zeiger auf ein natives `ABI::Windows::Perception::Spatial::ISpatialCoordinateSystem`-Objekt sein, welches das globale Koordinatensystem definiert, in dem Koordinaten in der API ausgedrückt werden.

#### <a name="render-remote-image"></a>Rendern des Remotebildes

Am Anfang jedes Frames muss der Remoteframe in den Hintergrundpuffer gerendert werden. Hierfür wird `BlitRemoteFrame` aufgerufen. Dadurch werden sowohl Farb- als auch Tiefeninformationen für beide Augen in das derzeit gebundene Renderziel gefüllt. Daher muss dies unbedingt erfolgen, nachdem der vollständige Hintergrundpuffer als Renderziel gebunden wurde.

> [!WARNING]
> Nachdem das Remotebild in den Hintergrundpuffer verschoben wurde, sollte der lokale Inhalt mit einem Stereorenderingverfahren mit einzelnem Durchlauf gerendert werden, z. B. mithilfe von **SV_RenderTargetArrayIndex**. Die Verwendung anderer Stereorenderingverfahren, wie das Rendering der einzelnen Augen in einem separaten Durchlauf, kann zu erheblichen Leistungseinbußen oder grafischen Artefakten führen und sollte vermieden werden.

```cs
AzureSession currentSession = ...;
GraphicsBindingWmrD3d11 wmrBinding = (currentSession.GraphicsBinding as GraphicsBindingWmrD3d11);
wmrBinding.BlitRemoteFrame();
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
ApiHandle<GraphicsBindingWmrD3d11> wmrBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingWmrD3d11>();
wmrBinding->BlitRemoteFrame();
```

### <a name="simulation"></a>Simulation

`GraphicsApiType.SimD3D11` ist die Simulationsbindung. Bei ihrer Auswahl wird die `GraphicsBindingSimD3d11`-Grafikbindung erstellt. Über die Schnittstelle wird die Kopfbewegung simuliert (z. B. in einer Desktopanwendung) und ein monoskopisches Bild gerendert.

Zur Implementierung der Simulationsbindung ist es wichtig, den Unterschied zwischen der lokalen Kamera und dem Remoteframe zu verstehen, wie auf der Seite [Kamera](../overview/features/camera.md) beschrieben.

Es werden zwei Kameras benötigt:

* **Lokale Kamera**: Diese Kamera stellt die aktuelle Kameraposition dar, die von der Anwendungslogik gesteuert wird.
* **Proxykamera**: Diese Kamera stimmt mit dem aktuellen *Remoteframe* überein, der vom Server gesendet wurde. Da es eine Zeitverzögerung zwischen der Anforderung eines Frames durch den Client und dessen Empfang gibt, liegt der *Remoteframe* immer etwas hinter der Bewegung der lokalen Kamera.

Der grundlegende Ansatz dabei ist, dass sowohl das Remotebild als auch der lokale Inhalt mithilfe der Proxykamera in ein Offscreenziel gerendert wird. Das Proxybild wird dann in den lokalen Kamerabereich zurückprojiziert. Dies wird in [Farbverschiebungsausgleich](../overview/features/late-stage-reprojection.md) näher erläutert.

Die Einrichtung ist etwas komplizierter und funktioniert wie folgt:

#### <a name="create-proxy-render-target"></a>Erstellen eines Proxyrenderziels

Remoteinhalte und lokale Inhalte müssen in einem Renderziel mit nicht sichtbarer Farbe/Tiefe namens „Proxy“ gerendert werden; hierbei werden die von der `GraphicsBindingSimD3d11.Update`-Funktion bereitgestellten Proxykameradaten verwendet.

Der Proxy muss der Auflösung des Hintergrundpuffers entsprechen und sollte im Format *DXGI_FORMAT_R8G8B8A8_UNORM* oder *DXGI_FORMAT_B8G8R8A8_UNORM* vorliegen. Sobald eine Sitzung bereit ist, muss vor der Verbindungsherstellung `GraphicsBindingSimD3d11.InitSimulation` aufgerufen werden:

```cs
AzureSession currentSession = ...;
IntPtr d3dDevice = ...; // native pointer to ID3D11Device
IntPtr color = ...; // native pointer to ID3D11Texture2D
IntPtr depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
GraphicsBindingSimD3d11 simBinding = (currentSession.GraphicsBinding as GraphicsBindingSimD3d11);
simBinding.InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

```cpp
ApiHandle<AzureSession> currentSession = ...;
void* d3dDevice = ...; // native pointer to ID3D11Device
void* color = ...; // native pointer to ID3D11Texture2D
void* depth = ...; // native pointer to ID3D11Texture2D
float refreshRate = 60.0f; // Monitor refresh rate up to 60hz.
bool flipBlitRemoteFrameTextureVertically = false;
bool flipReprojectTextureVertically = false;
ApiHandle<GraphicsBindingSimD3d11> simBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingSimD3d11>();
simBinding->InitSimulation(d3dDevice, depth, color, refreshRate, flipBlitRemoteFrameTextureVertically, flipReprojectTextureVertically);
```

Die Init-Funktion muss mit Zeigern auf das native D3D-Gerät sowie auf die Farb- und Tiefentextur des Proxyrenderziels bereitgestellt werden. Nach der Initialisierung können `AzureSession.ConnectToRuntime` und `DisconnectFromRuntime` mehrmals aufgerufen werden. Wenn Sie jedoch zu einer anderen Sitzung wechseln, muss `GraphicsBindingSimD3d11.DeinitSimulation` zuerst in der alten Sitzung aufgerufen werden, bevor `GraphicsBindingSimD3d11.InitSimulation` in einer anderen Sitzung aufgerufen werden kann.

#### <a name="render-loop-update"></a>Aktualisierung der Renderschleife

Die Aktualisierung der Renderschleife umfasst mehrere Schritten:

1. Für jeden Frame wird vor dem Rendering `GraphicsBindingSimD3d11.Update` mit der aktuellen Kameratransformation aufgerufen, die an den Server für das Rendering gesendet wird. Gleichzeitig ist die zurückgegebene Proxytransformation auf die Proxykamera anzuwenden, um sie in das Proxyrenderziel zu rendern.
Wenn die zurückgegebene Proxyaktualisierung `SimulationUpdate.frameId` NULL ist, sind noch keine Remotedaten vorhanden. In diesem Fall müssen anstelle des Renderings in das Proxyrenderziel lokale Inhalte mit den aktuellen Kameradaten direkt in den Hintergrundpuffer gerendert werden, und die nächsten beiden Schritte werden übersprungen.
1. Die Anwendung stellt nun eine Bindung mit dem Proxyrenderziel her und ruft `GraphicsBindingSimD3d11.BlitRemoteFrameToProxy` auf. Dadurch werden die Remoteinformationen zu Farbe und Tiefe im Proxyrenderziel gefüllt. Alle lokalen Inhalte können nun mithilfe der Proxykameratransformation auf dem Proxy gerendert werden.
1. Anschließend muss der Hintergrundpuffer als Renderziel gebunden und `GraphicsBindingSimD3d11.ReprojectProxy` aufgerufen werden; an diesem Punkt kann der Hintergrundpuffer dargestellt werden.

```cs
AzureSession currentSession = ...;
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

```cpp
ApiHandle<AzureSession> currentSession;
ApiHandle<GraphicsBindingSimD3d11> simBinding = currentSession->GetGraphicsBinding().as<GraphicsBindingSimD3d11>();

SimulationUpdate update;
// Fill out camera data with current camera data
...
SimulationUpdate proxyUpdate;
simBinding->Update(update, &proxyUpdate);
// Is the frame data valid?
if (proxyUpdate.frameId != 0)
{
    // Bind proxy render target
    simBinding->BlitRemoteFrameToProxy();
    // Use proxy camera data to render local content
    ...
    // Bind back buffer
    simBinding->ReprojectProxy();
}
else
{
    // Bind back buffer
    // Use current camera data to render local content
    ...
}
```

## <a name="api-documentation"></a>API-Dokumentation

* [C# RemoteManagerStatic.StartupRemoteRendering()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanagerstatic.startupremoterendering)
* [C# GraphicsBinding-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbinding)
* [C# GraphicsBindingWmrD3d11-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbindingwmrd3d11)
* [C# GraphicsBindingSimD3d11-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbindingsimd3d11)
* [C++ RemoteRenderingInitialization-Struktur](https://docs.microsoft.com/cpp/api/remote-rendering/remoterenderinginitialization)
* [C++ GraphicsBinding-Klasse](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbinding)
* [C++ GraphicsBindingWmrD3d11-Klasse](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbindingwmrd3d11)
* [C++ GraphicsBindingSimD3d11-Klasse](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbindingsimd3d11)

## <a name="next-steps"></a>Nächste Schritte

* [Kamera](../overview/features/camera.md)
* [Late Stage Reprojection (LSR)](../overview/features/late-stage-reprojection.md)
* [Tutorial: Anzeigen eines remote gerenderten Modells](../tutorials/unity/view-remote-models/view-remote-models.md)
