---
title: Integrieren von Remote Rendering in eine C++/DirectX11 Holographic-App
description: Erläutert das Integrieren von Remote Rendering in eine einfache C++/DirectX11 Holographic-App, wie sie mit dem Projekt-Assistenten von Visual Studio erstellt wird
author: florianborn71
ms.author: flborn
ms.date: 05/04/2020
ms.topic: tutorial
ms.openlocfilehash: b6dceff8b777d09a6f791437eb4aaca70365b518
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300716"
---
# <a name="tutorial-integrate-remote-rendering-into-a-hololens-holographic-app"></a>Tutorial: Integrieren von Remote Rendering in eine HoloLens Holographic-App

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Verwenden von Visual Studio zum Erstellen einer Holographic-App, die in HoloLens bereitgestellt werden kann
> * Hinzufügen der erforderlichen Codeausschnitte und Projekteinstellungen, um lokales Rendering mit remote gerenderten Inhalten zu kombinieren

Dieses Tutorial legt den Schwerpunkt darauf, einem Beispiel für eine native `Holographic App` den erforderlichen Code hinzuzufügen, um lokales Rendering mit Azure Remote Rendering zu kombinieren. Die einzige Art Statusfeedback in dieser App erfolgt über den Debugausgabebereich in Visual Studio, es empfiehlt sich daher, das Beispiel in Visual Studio zu starten. Das Hinzufügen eines ordnungsgemäßen in die App integrierten Feedbacks würde den Rahmen dieses Beispiels sprengen, da das Erstellen eines dynamischen Bereichs für die Texteingabe von Grund auf eine Menge neu zu erstellenden Code mit sich bringt. Ein guter Ausgangspunkt ist die Klasse `StatusDisplay`, die Teil des [Remoting Player-Beispielprojekts auf GitHub](https://github.com/microsoft/MixedReality-HolographicRemoting-Samples/tree/master/player/common/Content) ist. Tatsächlich verwendet die vorkonfigurierte Version dieses Tutorials eine lokale Kopie dieser Klasse.

> [!TIP]
> Das [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) enthält das Ergebnis dieses Tutorials als gebrauchsfertiges Visual Studio-Projekt. Es ist außerdem dank der UI-Klasse `StatusDisplay` mit ordnungsgemäßer Fehler- und Statusberichterstattung ausgestattet. Im Tutorial wird der Bereich aller ARR-spezifischen Ergänzungen durch `#ifdef USE_REMOTE_RENDERING` / `#endif` definiert, die Remote Rendering-Erweiterungen sind daher leicht zu erkennen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ihre Kontoinformationen (Konto-ID, Kontoschlüssel, Abonnement-ID). Falls Sie über kein Konto verfügen, [erstellen Sie ein Konto](../../../how-tos/create-an-account.md).
* Windows SDK 10.0.18362.0 [(Download)](https://developer.microsoft.com/windows/downloads/windows-10-sdk).
* Die aktuellste Version von Visual Studio 2019 [(Download)](https://visualstudio.microsoft.com/vs/older-downloads/).
* [Visual Studio-Tools für Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/install-the-tools). Insbesondere sind die folgenden *Workloadinstallationen* obligatorisch:
  * **Desktopentwicklung mit C++**
  * **Entwicklung für die universelle Windows-Plattform (UWP)**
* Die Windows Mixed Reality-App-Vorlagen für Visual Studio [(Download)](https://marketplace.visualstudio.com/items?itemName=WindowsMixedRealityteam.WindowsMixedRealityAppTemplatesVSIX).

## <a name="create-a-new-holographic-app-sample"></a>Erstellen eines neuen Beispiels für eine Holographic-App

Als ersten Schritt erstellen wir ein Standardbeispiel, das die Basis für die Integration von Remote Rendering bildet. Öffnen Sie Visual Studio, wählen Sie „Neues Projekt erstellen“ aus, und suchen Sie nach „DirectX 11 Holographic-App (Universal Windows) (C++/WinRT)“.

![Erstellen eines neuen Projekts](media/new-project-wizard.png)

Geben Sie einen Projektnamen Ihrer Wahl ein, wählen Sie einen Pfad und dann die Schaltfläche „Erstellen“ aus.
Ändern Sie im neuen Projekt die Konfiguration in **„Debug/ARM64“** . Sie sollten jetzt in der Lage sein, das Projekt zu kompilieren und auf einem verbundenen HoloLens 2-Gerät bereitzustellen. Wenn Sie es auf HoloLens ausführen, sollten Sie vor sich einen rotierenden Würfel sehen.

## <a name="add-remote-rendering-dependencies-through-nuget"></a>Hinzufügen von Remote Rendering-Abhängigkeiten mithilfe von NuGet

Der erste Schritt beim Hinzufügen von Remote Rendering-Funktionen besteht darin, die clientseitigen Abhängigkeiten hinzuzufügen. Die relevanten Abhängigkeiten sind als NuGet-Paket verfügbar.
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü **NuGet-Pakete verwalten...“** aus.

Navigieren Sie in dem nun angezeigten Dialogfeld zum NuGet-Paket mit dem Namen **„Microsoft.Azure.RemoteRendering.Cpp“** :

![Navigieren Sie zum NuGet-Paket,](media/add-nuget.png)

und fügen Sie es dem Projekt hinzu, indem Sie das Paket auswählen und dann auf die Schaltfläche „Installieren“ klicken.

Das NuGet-Paket fügt dem Projekt die Remote Rendering-Abhängigkeiten hinzu. Dies gilt insbesondere in folgenden Fällen:
* Link zur Clientbibliothek (RemoteRenderingClient.lib)
* Einrichten der DLL-Abhängigkeiten
* Festlegen des richtigen Pfads, um das Verzeichnis einzuschließen.

## <a name="project-preparation"></a>Projektvorbereitung

Wir müssen geringfügige Änderungen am vorhandenen Projekt vornehmen. Diese Änderungen sind klein, aber ohne sie würde das Remote Rendering nicht funktionieren.

### <a name="enable-multithread-protection-on-directx-device"></a>Aktivieren des Multithreadschutzes auf dem DirectX-Gerät
Auf dem `DirectX11`-Gerät muss Multithreadschutz aktiviert sein. Um diese Einstellung zu ändern, öffnen Sie die Datei „DeviceResources.cpp“ im Ordner „Common“, und fügen Sie am Ende der Funktion `DeviceResources::CreateDeviceResources()` den folgenden Code ein:

```cpp
// Enable multi thread protection as now multiple threads use the immediate context.
Microsoft::WRL::ComPtr<ID3D11Multithread> contextMultithread;
if (context.As(&contextMultithread) == S_OK)
{
    contextMultithread->SetMultithreadProtected(true);
}
```

### <a name="enable-network-capabilities-in-the-app-manifest"></a>Aktivieren von Netzwerkfunktionen im App-Manifest
Netzwerkfunktionen müssen für die bereitgestellte App explizit aktiviert werden. Wird dies nicht konfiguriert, führen Verbindungsabfragen sonst letztlich zu Timeouts. Doppelklicken Sie zum Aktivieren auf das `package.appxmanifest`-Element im Projektmappen-Explorer. Wechseln Sie auf dem nächsten Bildschirm zur Registerkarte **Funktionen**, und wählen Sie Folgendes aus:
* Internet (Client und Server)
* Internet (Client)

![Netzwerkfunktionen](media/appx-manifest-caps.png)


## <a name="integrate-remote-rendering"></a>Integrieren von Remote Rendering

Da das Projekt jetzt vorbereitet ist, können wir mit dem Code beginnen. Ein guter Einstiegspunkt in die Anwendung ist die Klasse `HolographicAppMain` (Datei „HolographicAppMain.h/cpp“), da sie über alle notwendigen Hooks für Initialisierung, De-Initialisierung und Rendering verfügt.

### <a name="includes"></a>Includes

Wir beginnen, indem wir die erforderlichen Includes hinzufügen. Fügen Sie der Datei „HolographicAppMain.h“ das folgende Include hinzu:

```cpp
#include <AzureRemoteRendering.h>
```

...und diese zusätzliche `include`-Anweisung zur Datei „ HolographicAppMain.cpp“:

```cpp
#include <AzureRemoteRendering.inl>
#include <RemoteRenderingExtensions.h>
```

Um den Code einfach zu halten, definieren wir die folgende Namespaceverknüpfung oben in der Datei „HolographicAppMain.h“, hinter der `include`-Anweisung:

```cpp
namespace RR = Microsoft::Azure::RemoteRendering;
```

Diese Verknüpfung ist nützlich, damit wir nicht überall den gesamten Namespace auszuschreiben brauchen und die ARR-spezifischen Datenstrukturen besser erkennen können. Natürlich könnten wir auch die `using namespace...`-Anweisung verwenden.

### <a name="remote-rendering-initialization"></a>Initialisierung von Remote Rendering
 
Wir müssen während der Lebensdauer der Anwendung ein paar Objekte für die Sitzung usw. aufbewahren. Die Lebensdauer fällt mit der Lebensdauer des `HolographicAppMain`-Objekts der Anwendung zusammen, wir fügen unsere Objekte daher der Klasse `HolographicAppMain` als Member hinzu. Der nächste Schritt besteht darin, der Datei „HolographicAppMain.h“ die folgenden Klassenmember hinzuzufügen:

```cpp
class HolographicAppMain
{
    ...
    // members:
    std::string m_sessionOverride;                // if we have a valid session ID, we specify it here. Otherwise a new one is created
    RR::ApiHandle<RR::AzureFrontend> m_frontEnd;  // the front end instance
    RR::ApiHandle<RR::AzureSession> m_session;    // the current remote rendering session
    RR::ApiHandle<RR::RemoteManager> m_api;       // the API instance, that is used to perform all the actions. This is just a shortcut to m_session->Actions()
    RR::ApiHandle<RR::GraphicsBindingWmrD3d11> m_graphicsBinding; // the graphics binding instance
}
```

Ein guter Ort zum Vornehmen der eigentlichen Implementierung ist der Konstruktor der Klasse `HolographicAppMain`. Wir müssen hier drei Arten von Initialisierung vornehmen:
1. Die einmalige Initialisierung des Remote Rendering-Systems
1. Front-End-Erstellung
1. Sitzungserstellung

All dies erledigen wir nacheinander im Konstruktor. In echten Anwendungsfällen kann es aber sinnvoll sein, diese Schritte einzeln auszuführen.

Fügen Sie den folgenden Code am Anfang des Konstruktor-Textkörpers in der Datei „HolographicAppMain.cpp“ hinzu:

```cpp
HolographicAppMain::HolographicAppMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources)
{
    // 1. One time initialization
    {
        RR::RemoteRenderingInitialization clientInit;
        memset(&clientInit, 0, sizeof(RR::RemoteRenderingInitialization));
        clientInit.connectionType = RR::ConnectionType::General;
        clientInit.graphicsApi = RR::GraphicsApiType::WmrD3D11;
        clientInit.toolId = "<sample name goes here>"; // <put your sample name here>
        clientInit.unitsPerMeter = 1.0f;
        clientInit.forward = RR::Axis::Z_Neg;
        clientInit.right = RR::Axis::X;
        clientInit.up = RR::Axis::Y;
        RR::StartupRemoteRendering(clientInit);
    }


    // 2. Create front end
    {
        // Users need to fill out the following with their account data and model
        RR::AzureFrontendAccountInfo init;
        memset(&init, 0, sizeof(RR::AzureFrontendAccountInfo));
        init.AccountId = "00000000-0000-0000-0000-000000000000";
        init.AccountKey = "<account key>";
        init.AccountDomain = "westus2.mixedreality.azure.com"; // <change to your region>
        m_modelURI = "builtin://Engine";
        m_sessionOverride = ""; // if there is a valid session ID to re-use, put it here. Otherwise a new one is created

        m_frontEnd = RR::ApiHandle(RR::AzureFrontend(init));
    }

    // 3. Open/create rendering session
    {
        bool createNewSession = true;

        auto SetNewSession = [this](RR::ApiHandle<RR::AzureSession> newSession)
        {
            SetNewState(AppConnectionStatus::Connecting, RR::Result::Success);
            m_session = newSession;
            m_api = m_session->Actions();
            m_graphicsBinding = m_session->GetGraphicsBinding().as<RR::GraphicsBindingWmrD3d11>();
            m_session->ConnectionStatusChanged([this](auto status, auto error)
                {
                    OnConnectionStatusChanged(status, error);
                });

            // The following is async, but we'll get notifications via OnConnectionStatusChanged
            RR::ConnectToRuntimeParams init;
            init.mode = RR::ServiceRenderMode::Default;
            m_session->ConnectToRuntime(init);
        };

        // If we had an old (valid) session that we can recycle, we call synchronous function m_frontEnd->OpenRenderingSession
        if (!m_sessionOverride.empty())
        {
            auto openSessionRes = m_frontEnd->OpenRenderingSession(m_sessionOverride);
            if (openSessionRes->valid())
            {
                SetNewSession(*openSessionRes);
                createNewSession = false;
            }
        }

        if (createNewSession)
        {
            // Create a new session
            RR::RenderingSessionCreationParams init;
            memset(&init, 0, sizeof(RR::RenderingSessionCreationParams));
            init.MaxLease.minute = 10; // session is leased for 10 minutes
            init.Size = RR::RenderingSessionVmSize::Standard;
            auto createSessionAsync = *m_frontEnd->CreateNewRenderingSessionAsync(init);
            createSessionAsync->Completed([&](auto handler)
                {
                    if (handler->Result())
                    {
                        SetNewSession(*handler->Result());
                    }
                    else
                    {
                        SetNewState(AppConnectionStatus::ConnectionFailed, RR::Result::Fail);
                    }
                });
            SetNewState(AppConnectionStatus::CreatingSession, RR::Result::Success);
        }
    }

    // Rest of constructor code:
    ...
}
```
Innerhalb der lokalen Funktion `SetNewSession` registrieren wir uns bei einem Rückruf, der bei jeder Änderung des Verbindungsstatus einer bestimmten Sitzung ausgelöst wird. Wir leiten diesen Aufruf an unsere eigene Funktion `OnConnectionStatusChanged` um. Wir deklarieren und implementieren ihn (und den restlichen Code des Zustandsautomaten) im nächsten Abschnitt. Beachten Sie außerdem, dass die Anmeldeinformationen im Beispiel hartcodiert sind und an Ort und Stelle eingegeben werden müssen ([Konto-ID, Kontoschlüssel](../../../how-tos/create-an-account.md#retrieve-the-account-information) und [Domäne](../../../reference/regions.md)).

Am Ende des Destruktor-Textkörpers nehmen wir die De-Initialisierung symmetrisch und in umgekehrter Reihenfolge vor:

```cpp
HolographicAppMain::~HolographicAppMain()
{
    // Existing destructor code:
    ...
    
    // Destroy session:
    if (m_session != nullptr)
    {
        m_session->DisconnectFromRuntime();
        m_session = nullptr;
    }

    // Destroy front end:
    m_frontEnd = nullptr;

    // One-time de-initialization:
    RR::ShutdownRemoteRendering();
}
```

## <a name="state-machine"></a>Zustandsautomat

Beim Remote Rendering sind wichtige Funktionen, etwa das Erstellen einer Sitzung und das Laden eines Modells, asynchrone Funktionen. Um dem Rechnung zu tragen, benötigen wir einen einfachen Zustandsautomaten, der den Übergang durch die folgenden Zustände im Wesentlichen automaisch durchläuft:

*Initialisierung > Sitzungserstellung > Laden von Modellen (mit Statusanzeige)*

Entsprechend fügen wir der Klasse im nächsten Schritt Routinen zur Behandlung des Zustandsautomaten hinzu. Wir deklarieren eine eigene Enumeration `AppConnectionStatus` für die verschiedenen Zustände, in denen sich unsere Anwendung befinden kann. Sie ist der `RR::ConnectionStatus` ähnlich, weist aber einen zusätzlichen Zustand für fehlgeschlagene Verbindungen auf.

Fügen Sie der Klassendeklaration die folgenden Member und Funktionen hinzu:

```cpp
namespace HolographicApp
{
    // Our application's possible states:
    enum class AppConnectionStatus
    {
        Disconnected,

        CreatingSession,
        Connecting,
        Connected,

        // error state:
        ConnectionFailed,
    };

    class HolographicAppMain
    {
        ...
        // Member functions for state transition handling
        void OnConnectionStatusChanged(RR::ConnectionStatus status, RR::Result error);
        void SetNewState(AppConnectionStatus state, RR::Result error);
        void StartModelLoading();

        // Members for state handling:

        // Model loading:
        std::string m_modelURI;
        RR::ApiHandle<RR::LoadModelAsync> m_loadModelAsync;

        // Connection state machine:
        AppConnectionStatus m_currentStatus = AppConnectionStatus::Disconnected;
        RR::Result m_connectionResult = RR::Result::Success;
        RR::Result m_modelLoadResult = RR::Result::Success;
        bool m_isConnected = false;
        bool m_modelLoadTriggered = false;
        float m_modelLoadingProgress = 0.f;
        bool m_modelLoadFinished = false;

    }
```

Fügen Sie der CPP-Datei implementierungsseitig diese Funktionstextkörper hinzu:

```cpp
void HolographicAppMain::StartModelLoading()
{
    m_modelLoadingProgress = 0.f;

    RR::LoadModelFromSASParams params;
    params.ModelUrl = m_modelURI.c_str();
    params.Parent = nullptr;

    // Start the async model loading
    if (auto loadModel = m_api->LoadModelFromSASAsync(params))
    {
        m_loadModelAsync = *loadModel;
        m_loadModelAsync->Completed([this](const RR::ApiHandle<RR::LoadModelAsync>& async)
        {
            m_modelLoadResult = *async->Status();
            m_modelLoadFinished = true; // successful if m_modelLoadResult==RR::Result::Success
            m_loadModelAsync = nullptr;
            char buffer[1024];
            sprintf_s(buffer, "Remote Rendering: Model loading completed. Result: %s\n", RR::ResultToString(m_modelLoadResult));
            OutputDebugStringA(buffer);
            });
        m_loadModelAsync->ProgressUpdated([this](float progress)
        {
            // Progress callback
            m_modelLoadingProgress = progress;

            // Output progress percentage to VS output
            char buffer[1024];
            sprintf_s(buffer, "Remote Rendering: Model loading progress: %.1f\n", m_modelLoadingProgress * 100.f);
            OutputDebugStringA(buffer);
        });
    }
}



void HolographicAppMain::SetNewState(AppConnectionStatus state, RR::Result error)
{
    m_currentStatus = state;
    m_connectionResult = error;

    // Some log for the VS output panel:
    const char* appStatus = nullptr;

    switch (state)
    {
        case AppConnectionStatus::Disconnected: appStatus = "Disconnected"; break;
        case AppConnectionStatus::CreatingSession: appStatus = "CreatingSession"; break;
        case AppConnectionStatus::Connecting: appStatus = "Connecting"; break;
        case AppConnectionStatus::Connected: appStatus = "Connected"; break;
        case AppConnectionStatus::ConnectionFailed: appStatus = "ConnectionFailed"; break;
    }

    char buffer[1024];
    sprintf_s(buffer, "Remote Rendering: New status: %s, result: %s\n", appStatus, RR::ResultToString(error));
    OutputDebugStringA(buffer);
}


void HolographicAppMain::OnConnectionStatusChanged(RR::ConnectionStatus status, RR::Result error)
{
    switch (status)
    {
    case RR::ConnectionStatus::Connecting:
        SetNewState(AppConnectionStatus::Connecting, error);
        break;
    case RR::ConnectionStatus::Connected:
        if (error == RR::Result::Success)
        {
            SetNewState(AppConnectionStatus::Connected, error);
        }
        else
        {
            SetNewState(AppConnectionStatus::ConnectionFailed, error);
        }
        m_modelLoadTriggered = m_modelLoadFinished = false;
        m_isConnected = error == RR::Result::Success;

        // start model loading as soon as we have a valid connection
        if (m_isConnected && !m_modelLoadTriggered)
        {
            m_modelLoadTriggered = true;
            StartModelLoading();
        }
        break;
    case RR::ConnectionStatus::Disconnected:
        if (error == RR::Result::Success)
        {
            SetNewState(AppConnectionStatus::Disconnected, error);
        }
        else
        {
            SetNewState(AppConnectionStatus::ConnectionFailed, error);
        }
        m_modelLoadTriggered = m_modelLoadFinished = false;
        m_isConnected = false;
        break;
    default:
        break;
    }
}
```

### <a name="per-frame-update"></a>Update pro Bild

Wir benötigen einen Takt im Client für jeden Takt der Simulation. Die Klasse `HolographicApp1Main` bietet einen guten Hook für Updates pro Bild. Fügen Sie daher dem Textkörper von Funktion `HolographicApp1Main::Update` den folgenden Code hinzu:

```cpp
// Updates the application state once per frame.
HolographicFrame HolographicAppMain::Update()
{
    // Tick Remote rendering:
    if (m_session != nullptr)
    {
        // Tick the client to receive messages
        m_api->Update();
    }

    // Rest of the body:
    ...
}
```

### <a name="rendering"></a>Darstellung

Als letzten Schritt müssen wir das Rendering der Remoteinhalte aufrufen. Wir müssen diesen Aufruf an genau der richtigen Position in der Renderingpipeline ausführen, nach dem Löschen des Renderziels. Fügen Sie der Sperre `UseHolographicCameraResources` innerhalb von Funktion `HolographicAppMain::Render` den folgenden Codeausschnitt hinzu:

```cpp
        ...
        // Existing clear function:
        context->ClearDepthStencilView(depthStencilView, D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);

        // Inject remote rendering: as soon as we are connected, start blitting the remote frame.
        // We do the blitting after the Clear, and before cube rendering.
        if (m_isConnected)
        {
            m_graphicsBinding->BlitRemoteFrame();
        }

        ...
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Das Beispiel sollte jetzt einen Zustand erreicht haben, in dem es sich kompilieren und ausführen lässt.

Wenn das Beispiel ordnungsgemäß ausgeführt wird, zeigt es den rotierenden Würfel direkt vor Ihnen, und nach einiger Zeit für die Sitzungserstellung und das Laden des Modells wird das Engine-Modell an der aktuellen Kopfposition wiedergegeben. Sitzungserstellung und Laden des Modells können mehrere Minuten in Anspruch nehmen. Der aktuelle Status wird nur in den Ausgabebereich von Visual Studio geschrieben. Daher wird empfohlen, das Beispiel aus Visual Studio zu starten.

> [!CAUTION]
> Der Client trennt die Serververbindung, wenn die Taktfunktion einige Sekunden lang nicht aufgerufen wird. Das Auslösen von Haltepunkten kann also sehr leicht zu einem Trennen der Verbindung führen.

Eine ordnungsgemäße Statusanzeige mit einem Textbereich finden Sie in der vorkonfigurierten Version dieses Tutorials auf GitHub.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie alle erforderlichen Schritte zum Hinzufügen von Remote Rendering zu einem standardmäßigen C++/DirectX11-Beispiel einer **Holographic-App** kennengelernt.
Informationen zum Konvertieren Ihres eigenen Modells finden Sie in der folgenden Schnellstartanleitung:

> [!div class="nextstepaction"]
> [Schnellstart: Konvertieren eines Modells für das Rendering](../../../quickstarts/convert-model.md)
