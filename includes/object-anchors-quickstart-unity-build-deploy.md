---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 04/03/2020
ms.author: crtreasu
ms.openlocfilehash: 43c8c578587c65b6e0317caf77ff2d0604cb4fa3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102044573"
---
### <a name="build-and-deploy-the-app"></a>Erstellen und Bereitstellen der App

Öffnen Sie die von Unity generierte `.sln`-Datei. Ändern Sie die Buildkonfiguration wie folgt.

:::image type="content" source="./media/object-anchors-quickstarts-unity/aoa-unity-vs-build-config.png" alt-text="Buildkonfiguration":::

Als Nächstes müssen Sie die **IP-Adresse des Remotecomputers** konfigurieren, um die App bereitzustellen und zu debuggen.

Klicken Sie mit der rechten Maustaste auf das App-Projekt, und klicken Sie anschließend auf **Properties** (Eigenschaften). Navigieren Sie auf der Eigenschaftenseite zu **Configuration Properties -> Debugging** (Konfigurationseigenschaften -> Debuggen). Ändern Sie den Wert für **Machine Name** (Computername) in die IP-Adresse Ihres HoloLens-Geräts, und klicken Sie auf **Apply** (Übernehmen).

:::image type="content" source="./media/object-anchors-quickstarts-unity/aoa-unity-vs-remote-debug.png" alt-text="Remotedebuggen":::

Schließen Sie die Eigenschaftenseite. Klicken Sie auf **Remote Machine** (Remotecomputer). Die Erstellung und Bereitstellung der App auf Ihrem Remotegerät sollte nun beginnen. Stellen Sie sicher, dass Ihr Gerät aktiv ist.
