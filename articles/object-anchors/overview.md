---
title: 'Azure Object Anchors: Übersicht'
description: Hier erfahren Sie, wie Azure Object Anchors Ihnen dabei hilft, Objekte in der physischen Welt zu erkennen.
author: craigktreasure
manager: vriveras
ms.author: crtreasu
ms.date: 03/02/2021
ms.topic: overview
ms.service: azure-object-anchors
ms.openlocfilehash: 84a69c46e9d7587ed87d8288fca5e7d8ef25f546
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111983574"
---
# <a name="azure-object-anchors-overview"></a>Azure Object Anchors: Übersicht

Azure Object Anchors ermöglicht einer Anwendung mithilfe eines 3D-Modells das Erkennen eines Objekts in der physischen Welt und das Schätzen der räumlichen Lage nach den sechs Freiheitsgraden. Die 6DoF-Lage (sechs Freiheitsgrade gesamt) wird als Rotation und Übersetzung zwischen einem 3D-Modell und dem physischen Gegenstück definiert, also dem realen Objekt.

Azure Object Anchors besteht aus einem Dienst für die Modellkonvertierung und einem Runtimeclient-SDK für HoloLens. Der Dienst akzeptiert ein 3D-Objektmodell und gibt ein Azure Object Anchors-Modell aus. Das Azure Object Anchors-Modell wird zusammen mit dem Runtime-SDK verwendet, um einer HoloLens-Anwendung das Laden eines Objektmodells und das Erkennen und Nachverfolgen von Instanzen dieses Modells in der physischen Welt zu ermöglichen.

:::image type="content" source="./media/object-anchors-overview.jpg" alt-text="Azure Object Anchors in Aktion":::

## <a name="examples"></a>Beispiele

Einige Beispiele für Anwendungsfälle, die mit Azure Object Anchors ermöglicht werden, sind:

- **Training**: Erstellen Sie Mixed Reality-Trainingsumgebungen für Ihre Worker, ohne Marker zu platzieren oder Zeit für die manuelle Anpassung einer Hologrammausrichtung aufwenden zu müssen. Wenn Sie Ihre Mixed Reality-Trainingsumgebungen mit automatisierter Erkennung und Nachverfolgung erweitern möchten, erfassen Sie Ihr Modell im Azure Object Anchors-Dienst. So sind Sie einer markerlosen Umgebungen einen Schritt näher.

- **Leitfaden für die Aufgaben:** Mit Mixed Reality können Sie Ihre Mitarbeiter ganz einfach durch eine Reihe von Aufgaben führen. Die Überlagerung digitaler Anweisungen und Best Practices zusätzlich zum physischen Objekt (z. B. eine Maschine in der Fabrik oder eine Kaffeemaschine in der Büroküche) kann das Abschließen dieser Aufgaben deutlich vereinfachen. Für das Auslösen dieser Umgebungen ist in der Regel ein Marker oder eine manuelle Ausrichtung erforderlich. Mit Azure Object Anchors haben Sie jedoch die Möglichkeit, eine Umgebung zu erstellen, die automatisch das Objekt im Zusammenhang mit der Aufgabe erkennt. Anschließend können Sie nahtlos mit Mixed Reality-Anleitungen ohne Marker oder manuelle Ausrichtung arbeiten.

- **Objektsuche:** Wenn Sie bereits über ein 3D-Modell eines Objekts im physischen Raum verfügen, können Sie mit Azure Object Anchors Instanzen dieses Objekts in Ihrer physischen Umgebung suchen und nachverfolgen.

## <a name="usage-flow"></a>Nutzungsablauf

Laden Sie zunächst Ihr 3D-Objekt in unseren Azure Object Anchors Conversion-Dienst hoch. Sie können die Schritte in einem der folgenden Schnellstarts ausführen:

  - [Unity (HoloLens)](quickstarts/get-started-unity-hololens.md)
  - [Unity HoloLens mit MRTK](quickstarts/get-started-unity-hololens-mrtk.md)
  - [HoloLens DirectX](quickstarts/get-started-hololens-directx.md)

Der Dienst konvertiert dann Ihr Objekt in ein Azure Object Anchors-Modell. Laden Sie das konvertierte Modell herunter, damit Sie auf Wunsch das [Mesh visualisieren](visualize-converted-model.md) können. Zum Schluss kopieren Sie das Modell auf ein HoloLens-Gerät mit dem Runtime SDK für [Unity](/dotnet/api/Microsoft.Azure.ObjectAnchors) oder [HoloLens C++/WinRT](/cpp/api/object-anchors/winrt). Sie können nun physische Objekte erkennen, die mit Ihrem ursprünglichen Modell übereinstimmen.

:::image type="content" source="./media/object-anchors-flow.png" alt-text="Nutzungsablauf":::

## <a name="asset-requirements"></a>Objektanforderungen

Jede Dimension eines Objekts sollte zwischen einem Meter und zehn Metern betragen, und die Dateigröße sollte kleiner als 150 MB sein.

Die derzeit unterstützten Objektformate sind `fbx`, `ply`, `obj`, `glb` und `gltf`.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Abschnitte enthalten Informationen zu den ersten Schritten bei der Verwendung und Erstellung von Apps mit Azure Object Anchors.

> [!div class="nextstepaction"]
> [Modellerfassung](quickstarts/get-started-model-conversion.md)

> [!div class="nextstepaction"]
> [Unity (HoloLens)](quickstarts/get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Unity HoloLens mit MRTK](quickstarts/get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [HoloLens DirectX](quickstarts/get-started-hololens-directx.md)
