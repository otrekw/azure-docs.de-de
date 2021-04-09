---
title: Installieren des Remote Rendering-Pakets für Unity
description: Erläutert, wie die Remote Rendering-Client-DLLs für Unity installiert werden
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: bfb383a7079e98db1db1f9b5077558c187bcea96
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105047727"
---
# <a name="install-the-remote-rendering-package-for-unity"></a>Installieren des Remote Rendering-Pakets für Unity

Azure Remote Rendering verwendet ein Unity-Paket, um die Integration in Unity zu kapseln.
Dieses Paket enthält die gesamte C#-API sowie alle Plug-In-Binärdateien, die für die Verwendung von Azure Remote Rendering mit Unity erforderlich sind.
Gemäß dem Benennungsschema von Unity für Pakete lautet der Name des Pakets **com.microsoft.azure.remote-rendering**.

Sie können eine der folgenden Optionen wählen, um das Unity-Paket zu installieren.

## <a name="install-remote-rendering-package-using-the-mixed-reality-feature-tool"></a>Installieren des Remote Rendering-Pakets mit dem Mixed Reality-Featuretool

Das [Mixed Reality-Featuretool](/windows/mixed-reality/develop/unity/welcome-to-mr-feature-tool) ([Download](https://aka.ms/mrfeaturetool)) wird verwendet, um Pakete mit Mixed Reality-Features in Unity-Projekte zu integrieren. Das Paket ist nicht im [Repository mit den ARR-Beispielen](https://github.com/Azure/azure-remote-rendering) enthalten und nicht in der internen Paketregistrierung von Unity verfügbar.

Gehen Sie wie folgt vor, um das Paket einem Projekt hinzuzufügen:
1. [Laden Sie das Mixed Reality-Featuretool herunter](https://aka.ms/mrfeaturetool).
1. Befolgen Sie die [vollständige Anleitung](/windows/mixed-reality/develop/unity/welcome-to-mr-feature-tool) zur Verwendung dieses Tools.
1. Aktivieren Sie auf der Seite **Funktionen ermitteln** das Kontrollkästchen für das Paket **Microsoft Azure Remote Rendering**, und wählen Sie die Version des Pakets aus, das Sie Ihrem Projekt hinzufügen möchten.

![Mixed_Reality_feature_tool_package](media/mixed-reality-feature-tool-package.png)

Wählen Sie zum Aktualisieren Ihres lokalen Pakets im Mixed Reality-Featuretool einfach eine neuere Version aus, und installieren Sie sie. Das Aktualisieren des Pakets kann gelegentlich zu Konsolenfehlern führen. Wenn dies auftritt, schließen Sie das Projekt und öffnen es erneut.

## <a name="install-remote-rendering-package-manually"></a>Manuelles Installieren des Remote Rendering-Pakets

Für die manuelle Installation des Remote Rendering-Pakets sind die folgenden Schritte erforderlich:

1. Laden Sie das Paket über den npm-Feed für die Mixed Reality-Pakete unter `https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry` herunter.
    * Eine Option besteht darin, [npm](https://www.npmjs.com/get-npm) zu nutzen und den folgenden Befehl auszuführen, um das Paket in den aktuellen Ordner herunterzuladen.
      ```
      npm pack com.microsoft.azure.remote-rendering --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry
      ```

    * Alternativ können Sie das PowerShell-Skript unter `Scripts/DownloadUnityPackages.ps1` aus dem [GitHub-Repository „azure-remote-rendering“](https://github.com/Azure/azure-remote-rendering) verwenden.
        * Bearbeiten Sie den Inhalt von `Scripts/unity_sample_dependencies.json` wie folgt:
          ```json
          {
            "packages": [
              {
                "name": "com.microsoft.azure.remote-rendering", 
                "version": "latest", 
                "registry": "https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry"
              }
            ]
          }
          ```

        * Führen Sie in PowerShell den folgenden Befehl aus, um das Paket in das angegebene Zielverzeichnis herunterzuladen.
          ```
          DownloadUnityPackages.ps1 -DownloadDestDir <destination directory>
          ```

1. Führen Sie die [Installation des heruntergeladenen Pakets](https://docs.unity3d.com/Manual/upm-ui-tarball.html) mit dem Paket-Manager von Unity durch.

Zum Aktualisieren Ihres lokalen Pakets führen Sie einfach den von Ihnen verwendeten Befehl erneut aus und wiederholen den Importvorgang für das Paket. Das Aktualisieren des Pakets kann gelegentlich zu Konsolenfehlern führen. Wenn dies auftritt, schließen Sie das Projekt und öffnen es erneut.

## <a name="unity-render-pipelines"></a>Unity-Renderingpipelines

Das Remote Rendering funktioniert sowohl mit der **:::no-loc text="Universal render pipeline":::** als auch mit der **:::no-loc text="Standard render pipeline":::** . Aus Leistungsgründen wird die universelle Renderpipeline empfohlen.

Um die **:::no-loc text="Universal render pipeline":::** verwenden zu können, muss das zugehörige Paket in Unity installiert werden. Dies kann entweder auf der Benutzeroberfläche des **Package Manager** von Unity (Paketname **Universal RP**, Version 7.3.1 oder höher) oder über die Datei `Packages/manifest.json` erfolgen, wie im [Tutorial zum Einrichten eines Unity-Projekts](../../tutorials/unity/view-remote-models/view-remote-models.md#include-the-azure-remote-rendering-package) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* [Interagieren mit Unity-Spielobjekten und -Komponenten](objects-components.md)
* [Tutorial: Anzeigen von Remotemodellen](../../tutorials/unity/view-remote-models/view-remote-models.md)