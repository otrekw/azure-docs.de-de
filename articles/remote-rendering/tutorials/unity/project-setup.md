---
title: Einrichten eines ganz neuen Unity-Projekts
description: Informationen zum Konfigurieren eines leeren Unity-Projekts für den Einsatz mit Azure Remote Rendering.
author: florianborn71
ms.author: flborn
ms.date: 01/30/2020
ms.topic: tutorial
ms.openlocfilehash: 33801316e4c0446865169560bb42f98052acba70
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678651"
---
# <a name="tutorial-setting-up-a-unity-project-from-scratch"></a>Tutorial: Einrichten eines ganz neuen Unity-Projekts

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Konfigurieren eines neuen Unity-Projekts für ARR von Grund auf
> * Erstellen und Beenden von Renderingsitzungen
> * Wiederverwenden vorhandener Sitzungen
> * Herstellen und Trennen von Verbindungen mit Sitzungen
> * Laden von Modellen in eine Renderingsitzung
> * Anzeigen von Verbindungsstatistiken

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ihre Kontoinformationen (Konto-ID, Kontoschlüssel, Abonnement-ID). Falls Sie über kein Konto verfügen, [erstellen Sie ein Konto](../../how-tos/create-an-account.md).
* Windows SDK 10.0.18362.0 [(herunterladen)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Aktuelle Version von Visual Studio 2019 [(herunterladen)](https://visualstudio.microsoft.com/vs/older-downloads/)
* Git [(herunterladen)](https://git-scm.com/downloads)
* Unity 2019.3.1 [(herunterladen)](https://unity3d.com/get-unity/download)
  * Installieren Sie in Unity diese Module:
    * **UWP**: Buildunterstützung für Universelle Windows-Plattform
    * **IL2CPP**: Windows-Buildunterstützung (IL2CPP)

> [!TIP]
> Das [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) enthält für alle Tutorials vorbereitete Unity-Projekte. Sie können diese Projekte als Referenz verwenden.

## <a name="create-a-new-unity-project"></a>Erstellen eines neuen Unity-Projekts

Erstellen Sie im Unity Hub ein neues Projekt.
Bei diesem Beispiel gehen wir davon aus, dass das Projekt in einem Ordner namens `RemoteRendering` erstellt wird.

![Fenster „New Project“ (Neues Projekt)](media/new-project.png)

## <a name="configure-the-projects-manifest"></a>Konfigurieren des Manifests des Projekts

Sie müssen die Datei `Packages/manifest.json` ändern, die sich in Ihrem Unity-Projektordner befindet. Öffnen Sie die Datei in einem Text-Editor, und fügen Sie die folgenden Zeilen hinzu:

```json
{
  "scopedRegistries": [
    {
      "name": "Azure Mixed Reality Services",
      "url": "https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM/",
      "scopes": ["com.microsoft.azure"]
    }
   ],
  "dependencies": {
    "com.microsoft.azure.remote-rendering": "0.1.11",
    "com.unity.render-pipelines.universal": "7.2.1",
    ...existing dependencies...
  }
}
```

Das Paket der universellen Renderpipeline ist optional, wird aber aus Leistungsgründen empfohlen.
Nachdem Sie das Manifest geändert und gespeichert haben, wird Unity automatisch aktualisiert. Vergewissern Sie sich, dass die Pakete in das Fenster *Project* (Projekt) geladen wurden:

![Bestätigen des Imports von Paketen](media/confirm-packages.png)

## <a name="ensure-you-have-the-latest-version-of-the-package"></a>Stellen Sie sicher, dass Sie über die neueste Version des Pakets verfügen.

Die folgenden Schritte stellen sicher, dass das Projekt die neueste Version des Remoterenderingpakets verwendet.
1. Wählen Sie das Paket im Fenster „Project“ (Projekt) aus, und klicken Sie auf das Symbol „Paket“: ![Auswählen des Paketsymbols](media/package-icons.png)
1. Klicken Sie im Inspector auf „View in Package Manager“ (In Paket-Manager anzeigen): ![Package Inspector](media/package-properties.png) (Paket-Inspektor).
1. Überprüfen Sie auf der Seite mit dem Paket-Manager für das Remoterenderingpaket, ob die Schaltfläche „Update“ (Aktualisieren) verfügbar ist. Falls ja, wird das Paket durch Klicken auf diese Schaltfläche in die neueste verfügbare Version geändert: ![Das ARR-Paket im Paket-Manager](media/package-manager.png)
1. Mitunter kann das Aktualisieren des Pakets zu Fehlern in der Konsole führen. Wenn dies auftritt, schließen Sie das Projekt und öffnen es erneut.

## <a name="configure-the-camera"></a>Konfigurieren der Kamera

Wählen Sie den Knoten **Main Camera** (Hauptkamera) aus.

1. Setzen Sie dessen *Transformation* zurück:

    ![Zurücksetzen der Kameratransformation](media/camera-reset-transform.png)

1. Legen Sie **Clear flags** (Kennzeichnungen löschen) auf *Solid Color* (Volltonfarbe) fest.

1. Legen Sie **Background** (Hintergrund) auf *Black* (Schwarz) fest.

1. Legen Sie **Clipping Planes** (Clippingebenen) auf *Near (Nahe) = 0,3* und *Far (Entfernt) = 20* fest. Das bedeutet, dass beim Rendern Geometrien beschnitten werden, die näher als 30 cm oder entfernter als 20 Meter sind.

    ![Unity-Kameraeigenschaften](media/camera-properties.png)

## <a name="adjust-the-project-settings"></a>Anpassen der Projekteinstellungen

1. Öffnen Sie *Edit > Project Settings...* (Bearbeiten > Projekteinstellungen).
1. Wählen Sie in der Liste auf der linken Seite „Quality“ (Qualität) aus.
1. Ändern Sie **Default Quality Level** (Standardqualitätsstufe) in *Low* (Niedrig).

    ![Ändern der Qualitätseinstellungen des Projekts](media/settings-quality.png)

1. Wählen Sie auf der linken Seite **Graphics** (Grafik) aus.
1. Ändern Sie die Einstellung **Scriptable Rendering Pipeline** (Skriptfähige Renderingpipeline) in *HybridRenderingPipeline*. Überspringen Sie diesen Schritt, wenn die universelle Renderpipeline nicht verwendet wird.

    ![Ändern der Grafikeinstellungen von Projekten](media/settings-graphics-lwrp.png) Mitunter wird auf der Benutzeroberfläche die Liste der in den Paketen verfügbaren Pipelinetypen nicht aufgefüllt. In diesem Fall muss das Objekt *HybridRenderingPipeline* manuell auf das Feld gezogen werden: ![Ändern der Grafikeinstellungen von Projekten](media/hybrid-rendering-pipeline.png)
1. Wählen Sie auf der linken Seite **Player** aus.
1. Wählen Sie die Registerkarte **Universal Windows Platform settings** (UWP-Einstellungen) aus.
1. Ändern Sie **XR Settings** (XR-Einstellungen) so, dass Windows Mixed Reality unterstützt wird: ![Playereinstellungen](media/xr-player-settings.png).
1. Wählen Sie die Einstellungen gemäß dem obigen Screenshot aus:
    1. Aktivieren Sie **Virtual Reality Supported** (Unterstützung für virtuelle Realität).
    1. Legen Sie **Depth Format** (Tiefenformat) auf *16-Bit Depth* (16-Bit-Tiefe) fest.
    1. Aktivieren Sie **Depth Buffer Sharing** (Tiefenpufferfreigabe).
    1. Legen Sie **Stereo Rendering Mode** (Stereorenderingmodus) auf *Single Pass Instanced* (Einzeldurchlauf instanziiert) fest.

1. Erweitern Sie im selben Fenster oberhalb von *XR Settings* (XR-Einstellungen) die Option **Publishing Settings** (Veröffentlichungseinstellungen).
1. Scrollen Sie nach unten zu **Capabilities** (Funktionen), und wählen Sie Folgendes aus:
    * **InternetClient**
    * **InternetClientServer**
    * **SpatialPerception**
    * Optional für die Entwicklung: **PrivateNetworkClientServer**

      Diese Option wird benötigt, wenn Sie den Unity-Remotedebugger mit Ihrem Gerät verbinden möchten.

1. Aktivieren Sie unter **Supported Device Families** (Unterstützte Gerätefamilien) sowohl **Holographic** als auch **Desktop**

1. Wenn Sie das Mixed Reality Toolkit verwenden möchten, finden Sie in der [MRTK-Dokumentation](https://docs.microsoft.com/windows/mixed-reality/unity-development-overview) weitere Informationen zu empfohlenen Einstellungen und Funktionen.

## <a name="validate-project-setup"></a>Überprüfen der Projekteinrichtung

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob die Projekteinstellungen stimmen.

1. Wählen Sie auf der Symbolleiste des Unity-Editors im Menü RemoteRendering den Eintrag ValidateProject aus.
1. Im Fenster ValidateProject können Sie die Projekteinstellungen überprüfen und bei Bedarf korrigieren.

    ![Projektüberprüfung im Unity-Editor](media/arr-unity-validation.png)

## <a name="create-a-script-to-initialize-azure-remote-rendering"></a>Erstellen eines Skripts zum Initialisieren von Azure Remote Rendering

Erstellen Sie ein [neues Skript](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html), und nennen Sie es **RemoteRendering**. Öffnen Sie die Skriptdatei, und ersetzen Sie den gesamten Inhalt durch folgenden Code:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

#if UNITY_WSA
    using UnityEngine.XR.WSA;
#endif

// ask Unity to automatically append an ARRServiceUnity component when a RemoteRendering script is attached
[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRendering : MonoBehaviour
{
    // fill out the variables below with your account details

    // AccountDomain must be '<region>.mixedreality.azure.com' - if no '<region>' is specified, connections will fail
    // See the documentation for the list of available regions.
    public string AccountDomain = "westus2.mixedreality.azure.com";
    public string AccountId = "<enter your account id here>";
    public string AccountKey = "<enter your account key here>";

    public uint MaxLeaseTimeHours = 0;
    public uint MaxLeaseTimeMinutes = 10;
    public RenderingSessionVmSize VMSize = RenderingSessionVmSize.Standard;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        // initialize Azure Remote Rendering for use in Unity:
        // it needs to know which camera is used for rendering the scene
        RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
        RemoteManagerUnity.InitializeManager(clientInit);

        // lookup the ARRServiceUnity component and subscribe to session events
        arrService = GetComponent<ARRServiceUnity>();
        arrService.OnSessionErrorEncountered += ARRService_OnSessionErrorEncountered;
        arrService.OnSessionStatusChanged += ARRService_OnSessionStatusChanged;
    }

#if !UNITY_EDITOR
    private void Start()
    {
        StartCoroutine(AutoStartSessionAsync());
    }
#endif

    private void OnDestroy()
    {
        arrService.OnSessionErrorEncountered -= ARRService_OnSessionErrorEncountered;
        arrService.OnSessionStatusChanged -= ARRService_OnSessionStatusChanged;

        RemoteManagerStatic.ShutdownRemoteRendering();
    }

    private void CreateFrontend()
    {
        if (arrService.Frontend != null)
        {
            // early out if the front-end has been created before
            return;
        }

        // initialize the ARR service with our account details
        AzureFrontendAccountInfo accountInfo = new AzureFrontendAccountInfo();
        accountInfo.AccountKey = AccountKey;
        accountInfo.AccountId = AccountId;
        accountInfo.AccountDomain = AccountDomain;

        arrService.Initialize(accountInfo);
    }

    public void CreateSession()
    {
        CreateFrontend();

        // StartSession will call ARRService_OnSessionStarted once the session becomes available
        arrService.StartSession(new RenderingSessionCreationParams(VMSize, MaxLeaseTimeHours, MaxLeaseTimeMinutes));
    }

    public void StopSession()
    {
        arrService.StopSession();
    }

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);
    }

    private void ARRService_OnSessionErrorEncountered(ARRServiceUnity caller, SessionGeneralContext context)
    {
        Debug.LogError($"Session error encountered. Context: {context.ErrorMessage}. Request Cv: {context.RequestCorrelationVector}. Response Cv: {context.ResponseCorrelationVector}");
    }

    private async void LogSessionStatus(AzureSession session)
    {
        if (session != null)
        {
            var sessionProperties = await session.GetPropertiesAsync().AsTask();
            LogSessionStatus(sessionProperties);
        }
        else
        {
            var sessionProperties = arrService.LastProperties;
            Debug.Log($"Session ended: Id={sessionProperties.Id}");
        }
    }

    private void LogSessionStatus(RenderingSessionProperties sessionProperties)
    {
        Debug.Log($"Session '{sessionProperties.Id}' is {sessionProperties.Status}. Size={sessionProperties.Size}" +
            (!string.IsNullOrEmpty(sessionProperties.Hostname) ? $", Hostname='{sessionProperties.Hostname}'" : "") +
            (!string.IsNullOrEmpty(sessionProperties.Message) ? $", Message='{sessionProperties.Message}'" : ""));
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
    }
#endif
}
```

Dieses Skript initialisiert Azure Remote Rendering, weist an, welches Kameraobjekt zum Rendern verwendet werden soll, und platziert die Schaltfläche **Create Session** (Sitzung erstellen) im Anzeigebereich, wenn *Play Mode* (Wiedergabemodus) aktiviert ist.

> [!CAUTION]
> Wenn Sie das Skript ändern und speichern, solange der Wiedergabemodus in Unity aktiv ist, kann es zum Einfrieren von Unity kommen. Sie sind dann gezwungen, das Skript über den Task-Manager herunterzufahren. Daher sollten Sie den Wiedergabemodus erst beenden, bevor Sie das Skript *RemoteRendering* bearbeiten.

## <a name="test-azure-remote-rendering-session-creation"></a>Testen der Erstellung einer Azure Remote Rendering-Sitzung

Erstellen Sie ein neues GameObject in der Szene, und fügen Sie die Komponente *RemoteRendering* hinzu. Geben Sie für Ihr Remote Rendering-Konto die entsprechenden Angaben in *Account Domain* (Kontodomäne), *Account Id* (Konto-ID) und *Account Key* (Kontoschlüssel) ein:

![Eigenschaften von Remote Rendering-Komponenten](media/remote-rendering-component.png)

Starten Sie die Anwendung im Editor (auf **Wiedergeben** klicken oder STRG+P drücken). Im Anzeigebereich sollte die Schaltfläche **Create Session** (Sitzung erstellen) angezeigt werden. Klicken Sie darauf, um Ihre erste ARR-Sitzung zu starten:

![Erstellen der ersten Sitzung](media/test-create.png)

Wenn dies misslingt, stellen Sie sicher, dass Sie Ihre Kontodaten ordnungsgemäß in die Eigenschaften der Komponente RemoteRendering eingegeben haben. Andernfalls wird im Konsolenfenster eine Meldung mit der Ihnen zugewiesenen Sitzungs-ID angezeigt, die besagt, dass sich die Sitzung derzeit im Zustand *Starting* (Wird gestartet) befindet:

![Ausgabe beim Start der Sitzung](media/create-session-output.png)

An dieser Stelle stellt Azure einen Server für Sie bereit und startet einen virtuellen Computer für das Remoterendering. Dies dauert normalerweise **3-5 Minuten**. Wenn die VM bereit ist, wird der Rückruf `OnSessionStatusChanged` unseres Unity-Skripts ausgeführt. Der neue Sitzungsstatus wird ausgegeben:

![Ausgabe „Sitzung bereit“](media/create-session-output-2.png)

Das war schon alles! Einstweilen passiert nichts weiter. Um Kosten zu vermeiden, sollten Sie Sitzungen immer dann beenden, wenn sie nicht mehr benötigt werden. Bei diesem Beispiel können Sie dies entweder durch Klicken auf die Schaltfläche **Stop Session** (Sitzung beenden) oder durch Beenden der Unity-Simulation tun. Aufgrund der Eigenschaft **Auto-Stop Session** (Sitzung automatisch beenden), die in der Komponente *ARRServiceUnity* standardmäßig aktiviert ist, wird die Sitzung automatisch für Sie beendet. Wenn alles aufgrund von Abstürzen oder Verbindungsproblemen abbricht, kann Ihre Sitzung möglicherweise noch so lange wie Ihre *MaxLeaseTime* andauern, bevor sie vom Server heruntergefahren wird.

> [!NOTE]
> Das Beenden einer Sitzung erfolgt sofort und kann nicht rückgängig gemacht werden. Einmal beendet, müssen Sie eine neue Sitzung mit dem gleichen Startaufwand erstellen.

## <a name="reusing-sessions"></a>Wiederverwenden von Sitzungen

Das Erstellen einer neuen Sitzung ist leider ein zeitaufwändiger Vorgang. Daher sollten Sie versuchen, Sitzungen nur selten zu erstellen, und Sie nach Möglichkeit wiederverwenden.

Fügen Sie den folgenden Code in das Skript *RemoteRendering* ein, und entfernen Sie alte Versionen der duplizierten Funktionen:

```csharp
    public string SessionId = null;

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);

        var status = caller.LastProperties.Status;

        // Capture the session id for the starting/ready session so it can be reused
        if (status == RenderingSessionStatus.Ready || status == RenderingSessionStatus.Starting)
        {
            SessionId = session.SessionUUID;
        }
        // Our session is stopped, expired, or in an error state.
        else
        {
            SessionId = null;
        }
    }

    public async void QueryActiveSessions()
    {
        CreateFrontend();

        var allSessionsProperties = await arrService.Frontend.GetCurrentRenderingSessionsAsync().AsTask();

        Debug.Log($"Number of active sessions: {allSessionsProperties.Length}");

        foreach (var sessionProperties in allSessionsProperties)
        {
            if (string.IsNullOrEmpty(SessionId))
            {
                Debug.Log($"Number of active sessions: {allSessionsProperties.Length}");
                SessionId = sessionProperties.Id;
            }

            LogSessionStatus(sessionProperties);
        }
    }

    public void UseExistingSession()
    {
        CreateFrontend();

        // OpenSession will call ARRService_OnSessionStarted once the session becomes available
        arrService.OpenSession(SessionId);
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
    }
#endif
```

> [!CAUTION]
> Stellen Sie vor dem Ausführen dieses Codes sicher, dass die Option **Auto-Stop Session** (Sitzung automatisch beenden) in der Komponente RemoteRendering deaktiviert ist. Andernfalls wird jede Sitzung, die Sie erstellen, automatisch beendet, sobald Sie die Simulation beenden. Der Versuch, sie wiederzuverwenden, schlägt dann fehl.

Wenn Sie auf *Play* (Wiedergeben) klicken, erhalten Sie nun im Anzeigebereich drei Schaltflächen: **Create Session** (Sitzung erstellen), **Query Active Sessions** (Aktive Sitzungen abfragen) und **Use Existing Session** (Bestehende Sitzung verwenden). Mit der ersten Schaltfläche wird stets eine neue Sitzung erstellt. Mit der zweiten Schaltfläche wird abgefragt, ob *aktive* Sitzungen bestehen. Wenn Sie keine Sitzungs-ID manuell angegeben haben, um zu versuchen, diese zu verwenden, wird diese Aktion automatisch diese Sitzungs-ID zur künftigen Verwendung auswählen. Die dritte Schaltfläche versucht, eine Verbindung mit einer bestehenden Sitzung herzustellen. Entweder eine, die Sie manuell über die Eigenschaft der Komponente *Session Id* (Sitzungs-ID) angegeben haben, oder eine, die von *Query Active Sessions* (Aktive Sitzungen abfragen) gefunden wurde.

Die Funktion **AutoStartSessionAsync** dient zum Simulieren von Schaltflächenklicks außerhalb des Editors.

> [!TIP]
> Es ist möglich, Sitzungen zu öffnen, die beendet wurden, abgelaufen sind oder sich in einem fehlerhaften Zustand befinden. Sie können zwar nicht mehr zum Rendern verwendet werden, aber Sie können ihre Details abfragen, sobald Sie eine inaktive Sitzung geöffnet haben. Der obige Code prüft den Status einer Sitzung in `ARRService_OnSessionStarted`, um sie automatisch zu beenden, sobald die Sitzung unbrauchbar geworden ist.

Mit dieser Funktionalität können Sie jetzt Sitzungen erstellen und wiederverwenden, was Ihren Entwicklungsworkflow erheblich verbessern dürfte.

Normalerweise wird die Erstellung einer Sitzung aufgrund der für das Hochfahren des Servers erforderlichen Zeit außerhalb der Clientanwendung ausgelöst.

## <a name="connect-to-an-active-session"></a>Herstellen einer Verbindung mit einer aktiven Sitzung

Bisher haben wir Sitzungen erstellt oder geöffnet. Der nächste Schritt besteht darin, eine *Verbindung* mit einer Sitzung herzustellen. Sobald die Verbindung hergestellt ist, produziert der Renderingserver Bilder und sendet einen Videostream an unsere Anwendung.

Fügen Sie den folgenden Code in das Skript *RemoteRendering* ein, und entfernen Sie alte Versionen der duplizierten Funktionen:

```csharp
    private bool isConnected = false;

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);

        var status = caller.LastProperties.Status;

        // Capture the session id for the starting/ready session so it can be reused
        if (status == RenderingSessionStatus.Ready || status == RenderingSessionStatus.Starting)
        {
            SessionId = session.SessionUUID;

            // If our session properties are 'Ready' we are ready to start connecting to the runtime of the service.
            if (status == RenderingSessionStatus.Ready)
            {
                session.ConnectionStatusChanged += AzureSession_OnConnectionStatusChanged;
            }
        }
        // Our session is stopped, expired, or in an error state.
        else
        {
            SessionId = null;
            session.ConnectionStatusChanged -= AzureSession_OnConnectionStatusChanged;
        }
    }

    private void AzureSession_OnConnectionStatusChanged(ConnectionStatus status, Result result)
    {
        Debug.Log($"Connection status: '{status}', result: '{result}'");
        isConnected = (status == ConnectionStatus.Connected);
    }

    public void ConnectSession()
    {
        arrService.CurrentActiveSession?.ConnectToRuntime(new ConnectToRuntimeParams());
    }

    public void DisconnectSession()
    {
        if (isConnected)
        {
            arrService.CurrentActiveSession?.DisconnectFromRuntime();
        }
    }

    private void LateUpdate()
    {
        // The session must have its runtime pump updated.
        // The update will push messages to the server, receive messages, and update the frame-buffer with the remotely rendered content.
        arrService.CurrentActiveSession?.Actions.Update();
    }

    private void OnDisable()
    {
        DisconnectSession();
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }

            if (arrService.LastProperties.Status == RenderingSessionStatus.Ready)
            {
                if (!isConnected)
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Connect"))
                    {
                        ConnectSession();
                    }
                }
                else
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Disconnect"))
                    {
                        DisconnectSession();
                    }
                }
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
        while (arrService.CurrentActiveSession == null)
        {
            yield return null;
        }
        ConnectSession();
    }
#endif
```

So testen Sie diese Funktionalität

1. Klicken Sie in Unity auf **Play** (Wiedergeben).
1. Öffnen Sie eine Sitzung:
    1. Wenn bereits eine Sitzung besteht, klicken Sie auf **Query Active Sessions** (Aktive Sitzungen abfragen) und dann auf **Use Existing Session** (Bestehende Sitzung verwenden).
    1. Andernfalls klicken Sie auf **Create Session** (Sitzung erstellen).
1. Klicken Sie auf **Connect** (Verbinden).
1. Nach einigen Sekunden sollte die Konsolenausgabe besagen, dass Sie verbunden sind.
1. Einstweilen sollte nichts weiter passieren.
1. Klicken Sie auf **Disconnect** (Trennen), oder beenden Sie den Wiedergabemodus von Unity.

>[!NOTE]
> Mehrere Benutzer können eine Sitzung *öffnen*, um ihre Informationen abzufragen. Es kann jedoch immer nur ein Benutzer zur gleichen Zeit mit einer Sitzung *verbunden* sein. Wenn bereits ein anderer Benutzer verbunden ist, schlägt die Verbindung mit einem **Handshakefehler** fehl.

## <a name="load-a-model"></a>Laden eines Modells

Fügen Sie den folgenden Code in das Skript *RemoteRendering* ein, und entfernen Sie alte Versionen der duplizierten Funktionen:

```csharp

    public string ModelName = "builtin://Engine";

    private Entity modelEntity = null;
    private GameObject modelEntityGO = null;

#if UNITY_WSA
    private WorldAnchor modelWorldAnchor = null;
#endif

    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();

        // get the game object representation of this entity
        modelEntityGO = modelEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the entity will sync translations with the server
        var sync = modelEntityGO.GetComponent<RemoteEntitySyncObject>();
        sync.SyncEveryFrame = true;

        // set position to an arbitrary distance from the parent
        modelEntityGO.transform.position = Camera.main.transform.position + Camera.main.transform.forward * 2;
        modelEntityGO.transform.localScale = Vector3.one;

#if UNITY_WSA
        // anchor the model in the world
        modelWorldAnchor = modelEntityGO.AddComponent<WorldAnchor>();
#endif

        // load a model that will be parented to the entity
        // We are using the 'from SAS' flavor because that variant can load the built-in model
        var loadModelParams = new LoadModelFromSASParams(ModelName, modelEntity);
        var async = arrService.CurrentActiveSession.Actions.LoadModelFromSASAsync(loadModelParams);
        async.ProgressUpdated += (float progress) =>
        {
            Debug.Log($"Loading: {progress * 100.0f}%");
        };

        await async.AsTask();
    }

    public void DestroyModel()
    {
        if (modelEntity == null)
        {
            return;
        }

#if UNITY_WSA
        DestroyImmediate(modelWorldAnchor);
#endif

        modelEntity.Destroy();
        modelEntity = null;

        DestroyImmediate(modelEntityGO);
    }

    public void DisconnectSession()
    {
        if (isConnected)
        {
            arrService.CurrentActiveSession?.DisconnectFromRuntime();
        }

        DestroyModel();
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }

            if (arrService.LastProperties.Status == RenderingSessionStatus.Ready)
            {
                if (!isConnected)
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Connect"))
                    {
                        ConnectSession();
                    }
                }
                else
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Disconnect"))
                    {
                        DisconnectSession();
                    }

                    if (modelEntity == null)
                    {
                        if (GUI.Button(new Rect(10, 90, 175, 30), "Load Model"))
                        {
                            LoadModel();
                        }
                    }
                    else
                    {
                        if (GUI.Button(new Rect(10, 90, 175, 30), "Destroy Model"))
                        {
                            DestroyModel();
                        }
                    }
                }
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
        while (arrService.CurrentActiveSession == null)
        {
            yield return null;
        }
        ConnectSession();
        while (!isConnected)
        {
            yield return null;
        }
        LoadModel();
    }
#endif
```

Wenn Sie jetzt auf „Play“ (Wiedergeben) klicken, eine Sitzung öffnen und sich mit ihr verbinden, wird die Schaltfläche **Load Model** (Modell laden) angezeigt. Nach dem Klicken darauf wird in der Konsolenausgabe der Ladestatus angezeigt. Sobald dieser 100 % erreicht, sollten Sie das Modell eines Motors sehen:

![Im Editor geladenes Modell](media/model-loaded-replace-me.png)

Der [WorldAnchor](https://docs.unity3d.com/ScriptReference/XR.WSA.WorldAnchor.html) ist eine wichtige Komponente für die [Hologrammstabilität](https://docs.microsoft.com/windows/mixed-reality/hologram-stability). Er hat jedoch nur dann Auswirkungen, wenn er auf einem Mixed Reality-Gerät bereitgestellt wird.

> [!TIP]
> Wenn Sie die Schritte des Artikels [Schnellstart: Konvertieren eines Modells für das Rendering](../../quickstarts/convert-model.md) befolgt haben, wissen Sie bereits, wie Sie Ihre eigenen Modelle konvertieren. Alles, was Sie jetzt zum Rendern tun müssen, ist, den URI eines konvertierten Modells in die Eigenschaft *Model Name* (Modellname) einzugeben.

## <a name="display-frame-statistics"></a>Anzeigen einer Framestatistik

Azure Remote Rendering verfolgt verschiedene Informationen zur Verbindungsqualität nach. Um diese Informationen schnell anzuzeigen, gehen Sie so vor:

Erstellen Sie ein [neues Skript](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html), und nennen Sie es **RemoteFrameStats**. Öffnen Sie die Skriptdatei, und ersetzen Sie den gesamten Inhalt durch folgenden Code:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class RemoteFrameStats : MonoBehaviour
{
    public Text FrameStats = null;

    ARRServiceStats arrServiceStats = null;

#if UNITY_EDITOR
    private void OnEnable()
    {
        arrServiceStats = new ARRServiceStats();
    }

    void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            FrameStats.text = string.Empty;
            return;
        }

        arrServiceStats.Update(RemoteManagerUnity.CurrentSession);

        if (FrameStats != null)
        {
            FrameStats.text = arrServiceStats.GetStatsString();
        }
    }
#endif
}
```

Erstellen Sie ein GameObject, und nennen Sie es *FrameStats*. Fügen Sie es als untergeordneten Knoten an das Objekt *Main Camera* (Hauptkamera) an, und legen Sie seine Position auf **x = 0, y = 0, z = 0,325** fest. Fügen Sie dem Objekt die Komponente **RemoteFrameStats** hinzu.

Fügen Sie über **UI > Canvas** (Benutzeroberfläche > Canvas) dem Objekt *FrameStats* ein untergeordnetes Objekt hinzu, und legen Sie dessen Eigenschaften so fest:

![Canvaseigenschaften](media/framestats-canvas.png)

Fügen Sie über **UI > Text** (Benutzeroberfläche > Text) ein Objekt als untergeordnetes Objekt der Canvas hinzu, und legen Sie dessen Eigenschaften wie folgt fest:

![Texteigenschaften](media/framestats-text.png)

Wählen Sie das Objekt *FrameStats* aus, und füllen Sie das Feld **FrameStats** auf, indem Sie auf das Kreissymbol klicken und das Objekt **Text** auswählen:

![Festlegen der Texteigenschaft](media/framestats-set-text.png)

Wenn die Verbindung mit der Remotesitzung hergestellt ist, sollte der Text nun die Streamingstatistik zeigen:

![Ausgabe der Framestatistik](media/framestats-output.png)

Der Code deaktiviert die Statistikaktualisierung außerhalb des Editors, da ein Textfeld die Sicht einschränken und ablenken würde. Eine komplexere Implementierung finden Sie im [Schnellstart](../../quickstarts/render-model.md)-Projekt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie alle Schritte kennengelernt, die notwendig sind, um ein leeres Unity-Projekt mit Azure Remote Rendering in Betrieb zu nehmen. Im nächsten Tutorial werden wir uns näher damit befassen, wie mit Remoteentitäten gearbeitet wird.

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Remoteentitäten in Unity](working-with-remote-entities.md)
