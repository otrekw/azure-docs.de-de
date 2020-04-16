---
title: Einrichten von Remote Rendering für Unity
description: Initialisieren von Azure Remote Rendering in einem Unity-Projekt
author: jakrams
ms.author: jakras
ms.date: 02/27/2020
ms.topic: how-to
ms.openlocfilehash: 0415c0e7ee1432521c3cc2026feff5fc2a41d77e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679184"
---
# <a name="set-up-remote-rendering-for-unity"></a>Einrichten von Remote Rendering für Unity

Um Azure Remote Rendering (ARR) in Unity zu aktivieren, stellen wir dedizierte Methoden bereit, die einige Unity-spezifische Aspekte berücksichtigen.

## <a name="startup-and-shutdown"></a>Starten und Herunterfahren

Verwenden Sie `RemoteManagerUnity`, um Remote Rendering zu initialisieren. Diese Klasse ruft den generischen `RemoteManager` auf, implementiert aber bereits Unity-spezifische Details für Sie. Unity verwendet beispielsweise ein bestimmtes Koordinatensystem. Wenn Sie `RemoteManagerUnity.Initialize` aufrufen, wird die richtige Konvention eingerichtet. Der Aufruf erfordert auch, dass Sie die Unity-Kamera bereitstellen, die zum Anzeigen der remote gerenderten Inhalte verwendet werden soll.

```cs
// initialize Azure Remote Rendering for use in Unity:
// it needs to know which camera is used for rendering the scene
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);
```

Um Remote Rendering herunterzufahren, rufen Sie `RemoteManagerStatic.ShutdownRemoteRendering()` auf.

Nachdem eine `AzureSession` erstellt und als primäre Renderingsitzung ausgewählt wurde, muss sie bei `RemoteManagerUnity` registriert werden:

```cs
RemoteManagerUnity.CurrentSession = ...
```

### <a name="full-example-code"></a>Vollständiger Beispielcode

Der folgende Code veranschaulicht alle Schritte, die erforderlich sind, um Azure Remote Rendering in Unity zu initialisieren:

```cs
// initialize Remote Rendering
RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
RemoteManagerUnity.InitializeManager(clientInit);

// create a frontend
AzureFrontendAccountInfo accountInfo = new AzureFrontendAccountInfo();
// ... fill out accountInfo ...
AzureFrontend frontend = new AzureFrontend(accountInfo);

// start a session
AzureSession session = await frontend.CreateNewRenderingSessionAsync(new RenderingSessionCreationParams(RenderingSessionVmSize.Standard, 0, 30)).AsTask();

// let RemoteManagerUnity know about the session we want to use
RemoteManagerUnity.CurrentSession = session;

session.ConnectToRuntime(new ConnectToRuntimeParams());

/// When connected, load and modify content

RemoteManagerStatic.ShutdownRemoteRendering();
```

## <a name="convenience-functions"></a>Hilfsfunktionen

### <a name="session-state-events"></a>Sitzungsstatusereignisse

`RemoteManagerUnity.OnSessionUpdate` gibt Ereignisse für die Änderung des Sitzungszustands aus. Weitere Informationen dazu finden Sie in der Codedokumentation.

### <a name="arrserviceunity"></a>ARRServiceUnity

`ARRServiceUnity` ist eine optionale Komponente zum Optimieren der Einrichtung und der Sitzungsverwaltung. Sie enthält Optionen zum automatischen Beenden der Sitzung, wenn die Anwendung beendet oder der Wiedergabemodus im Editor beendet wird. Außerdem wird die Sitzungslease bei Bedarf automatisch erneuert. Sie speichert Daten, z. B. die Sitzungseigenschaften (siehe Variable `LastProperties`), und stellt Ereignisse für Sitzungszustandsänderungen und Sitzungsfehler bereit.

Es darf nicht mehr als eine Instanz von `ARRServiceUnity` gleichzeitig vorhanden sein. Dies soll Ihren Einstieg beschleunigen, indem einige allgemeine Funktionen implementiert werden. Für eine größere Anwendung ist es jedoch möglicherweise besser, diese Dinge selbst auszuführen.

Ein Beispiel zum Einrichten und Verwenden von `ARRServiceUnity` finden Sie im [Tutorial: Einrichten eines ganz neuen Unity-Projekts](../../tutorials/unity/project-setup.md).

## <a name="next-steps"></a>Nächste Schritte

* [Installieren des Remote Rendering-Pakets für Unity](install-remote-rendering-unity-package.md)
* [Tutorial: Einrichten eines ganz neuen Unity-Projekts](../../tutorials/unity/project-setup.md)
