---
title: 'Schnellstart: Erstellen einer Unity-iOS-App'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine iOS-App mit Unity erstellen, indem Sie Spatial Anchors verwenden.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 03/18/2021
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: b341237d0e703239233f7ac0e9664f5fb90bbb4b
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105805"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Schnellstart: Erstellen einer Unity iOS-App mit Azure Spatial Anchors

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit [Azure Spatial Anchors](../overview.md) eine Unity iOS-App erstellen. Azure Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine mit Unity erstellte ARKit-iOS-App, mit der ein räumlicher Anker gespeichert und abgerufen werden kann.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines Spatial Anchors-Kontos
> * Vorbereiten von Unity-Buildeinstellungen
> * Konfigurieren des Bezeichners und Kontoschlüssels für das Spatial Anchors-Konto
> * Exportieren des Xcode-Projekts
> * Bereitstellen und Ausführen auf einem iOS-Gerät

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

- Einen macOS-Computer, auf dem die aktuelle Version von <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> und <a href="https://unity3d.com/get-unity/download" target="_blank">Unity (LTS)</a> installiert sind. Verwenden Sie **Unity 2020 LTS** mit Version 2.9 oder höher des ASA SDK (das das [Unity XR-Plug-In-Framework](https://docs.unity3d.com/Manual/XRPluginArchitecture.html) nutzt) oder **Unity 2019 LTS** mit Version 2.8 oder einer älteren Version des ASA SDK.
- Git-Installation über Homebrew. Geben Sie den folgenden Befehl in einer einzelnen Zeile am Terminal ein: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Führen Sie dann `brew install git` und `brew install git-lfs` aus.
- Ein für Entwickler geeignetes <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit-kompatibles</a> iOS-Gerät.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Herunterladen und Öffnen des Unity-Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Befolgen Sie [diese Anweisungen](../how-tos/setup-unity-project.md#download-asa-packages), um die für die iOS-Plattform erforderlichen ASA SDK-Pakete herunterzuladen und zu importieren.

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

## <a name="export-the-xcode-project"></a>Exportieren des Xcode-Projekts

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Wählen Sie in der App mithilfe der Pfeile **BasicDemo** aus, und klicken Sie dann auf die Schaltfläche **Los**, um die Demo auszuführen. Befolgen Sie die Anleitung zum Anordnen und Abrufen eines Ankers.

![Screenshot 1](./media/get-started-unity-ios/screenshot-1.jpg)
![Screenshot 2](./media/get-started-unity-ios/screenshot-2.jpg)
![Screenshot 3](./media/get-started-unity-ios/screenshot-3.jpg)

Wenn Sie fertig sind, beenden Sie die App, indem Sie in Xcode **Beenden** wählen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="rendering-issues"></a>Probleme beim Rendern

Falls beim Ausführen der App die Kamera nicht als Hintergrund angezeigt wird (sondern stattdessen beispielsweise ein leerer blauer Bereich oder andere Texturen), müssen Sie die Ressourcen wahrscheinlich erneut in Unity importieren. Beenden Sie die App. Wählen Sie in Unity im Menü am oberen Rand die Option **Assets > Re-import all** (Ressourcen > Alle erneut importieren). Führen Sie die App dann erneut aus.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors](../tutorials/tutorial-share-anchors-across-devices.md)

> [!div class="nextstepaction"]
> [Vorgehensweise: Konfigurieren von Azure Spatial Anchors in einem Unity-Projekt](../how-tos/setup-unity-project.md)
