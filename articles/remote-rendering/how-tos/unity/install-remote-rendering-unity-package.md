---
title: Installieren des Remote Rendering-Pakets für Unity
description: Erläutert, wie die Remote Rendering-Client-DLLs für Unity installiert werden
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 3704d1a418baeec18c3303b8203a0185790cbcc7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564309"
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
    "com.microsoft.azure.remote-rendering": "0.1.31",
    ...existing dependencies...
  }
}
```

Nachdem dies hinzugefügt wurde, können Sie den Unity Package Manager verwenden, um sicherzustellen, dass Sie über die neueste Version verfügen.
Ausführlichere Anweisungen finden Sie im [Tutorial: Anzeigen von Remotemodellen](../../tutorials/unity/view-remote-models/view-remote-models.md).

## <a name="unity-render-pipelines"></a>Unity-Renderingpipelines

Das Remote Rendering funktioniert sowohl mit der **:::no-loc text="Universal render pipeline":::** als auch mit der **:::no-loc text="Standard render pipeline":::** . Aus Leistungsgründen wird die universelle Renderpipeline empfohlen.

Um die **:::no-loc text="Universal render pipeline":::** verwenden zu können, muss das zugehörige Paket in Unity installiert werden. Dies kann entweder auf der Benutzeroberfläche des **Package Manager** von Unity (Paketname **Universal RP**, Version 7.3.1 oder höher) oder über die Datei `Packages/manifest.json` erfolgen, wie im [Tutorial zum Einrichten eines Unity-Projekts](../../tutorials/unity/view-remote-models/view-remote-models.md#include-the-azure-remote-rendering-package) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* [Interagieren mit Unity-Spielobjekten und -Komponenten](objects-components.md)
* [Tutorial: Anzeigen von Remotemodellen](../../tutorials/unity/view-remote-models/view-remote-models.md)
