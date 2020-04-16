---
title: Installieren des Remote Rendering-Pakets für Unity
description: Erläutert, wie die Remote Rendering-Client-DLLs für Unity installiert werden
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 86ffe47c009f9e0ae121ffc6ab57bff8fb73210f
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679196"
---
# <a name="install-the-remote-rendering-package-for-unity"></a>Installieren des Remote Rendering-Pakets für Unity

Azure Remote Rendering verwendet ein Unity-Paket, um die Integration in Unity zu kapseln.

## <a name="manage-the-remote-rendering-packages-in-unity"></a>Verwalten der Remote Rendering-Pakete in Unity

Bei Unity-Paketen handelt es sich um Container, die im [Package Manager](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@1.8/manual/index.html) von Unity verwaltet werden können.
Dieses Paket enthält die gesamte C#-API sowie alle Plug-In-Binärdateien, die für die Verwendung von Azure Remote Rendering mit Unity erforderlich sind.
Gemäß dem Benennungsschema von Unity für Pakete lautet der Name des Pakets **com.microsoft.azure.remote-rendering**.

Das Paket ist nicht im [Repository mit den ARR-Beispielen](https://github.com/Azure/azure-remote-rendering) enthalten und nicht in der internen Paketregistrierung von Unity verfügbar. Wenn Sie es einem Projekt hinzufügen möchten, müssen Sie die Datei `manifest.md` des Projekts manuell bearbeiten, um Folgendes hinzuzufügen:
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
    ...existing dependencies...
  }
}
```
Nachdem dies hinzugefügt wurde, können Sie den Unity Package Manager verwenden, um sicherzustellen, dass Sie über die neueste Version verfügen.
Ausführlichere Anweisungen finden Sie im [Tutorial: Einrichten eines ganz neuen Unity-Projekts](../../tutorials/unity/project-setup.md).

## <a name="unity-render-pipelines"></a>Unity-Renderingpipelines

Remote Rendering kann sowohl mit der **universellen Renderpipeline** als auch mit der **Standardrenderpipeline** verwendet werden. Aus Leistungsgründen wird die universelle Renderpipeline empfohlen.

Um die **universelle Renderpipeline** verwenden zu können, muss das zugehörige Paket in Unity installiert werden. Dies kann entweder auf der Benutzeroberfläche des **Package Manager** von Unity (Paketname **Universal RP**, Version 7.2.1 oder höher) oder über die Datei `Packages/manifest.json` erfolgen, wie im [Tutorial zum Einrichten eines Unity-Projekts](../../tutorials/unity/project-setup.md#configure-the-projects-manifest) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* [Interagieren mit Unity-Spielobjekten und -Komponenten](objects-components.md)
* [Tutorial: Einrichten eines ganz neuen Unity-Projekts](../../tutorials/unity/project-setup.md)
