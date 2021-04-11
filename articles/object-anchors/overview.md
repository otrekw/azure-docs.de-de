---
title: 'Azure Object Anchors: Übersicht'
description: Hier erfahren Sie, wie Azure Object Anchors Ihnen dabei hilft, Objekte in der physischen Welt zu erkennen.
author: craigktreasure
manager: vriveras
ms.author: crtreasu
ms.date: 03/02/2021
ms.topic: overview
ms.service: azure-object-anchors
ms.openlocfilehash: 90b718e34a12fe71eec4e972bbf8de42d8b52007
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106063762"
---
# <a name="azure-object-anchors-overview"></a>Azure Object Anchors: Übersicht

Azure Object Anchors ermöglicht einer Anwendung mithilfe eines 3D-Modells das Erkennen eines Objekts in der physischen Welt und das Schätzen der räumlichen Lage nach den sechs Freiheitsgraden. Die 6DoF-Lage (sechs Freiheitsgrade gesamt) wird als Rotation und Übersetzung zwischen einem 3D-Modell und dem physischen Gegenstück definiert, also dem realen Objekt.

Azure Object Anchors besteht aus einem Dienst für die Modellkonvertierung und einem Runtimeclient-SDK für HoloLens. Der Dienst akzeptiert ein 3D-Objektmodell und gibt ein Azure Object Anchors-Modell aus. Das Azure Object Anchors-Modell wird zusammen mit dem Runtime-SDK verwendet, um einer HoloLens-Anwendung das Laden eines Objektmodells und das Erkennen und Nachverfolgen von Instanzen dieses Modells in der physischen Welt zu ermöglichen.

:::image type="content" source="./media/aoa-overview.jpg" alt-text="Azure Object Anchors in Aktion":::

## <a name="examples"></a>Beispiele

Einige Beispiele für Anwendungsfälle, die mit Azure Object Anchors ermöglicht werden, sind:

- **Training**: Erstellen Sie Mixed Reality-Trainingsumgebungen für Ihre Worker, ohne Marker zu platzieren oder Zeit für die manuelle Anpassung einer Hologrammausrichtung aufwenden zu müssen. Wenn Sie Ihre Mixed Reality-Trainingsumgebungen mit automatisierter Erkennung und Nachverfolgung erweitern möchten, erfassen Sie Ihr Modell im Azure Object Anchors-Dienst. So sind Sie einer markerlosen Umgebungen einen Schritt näher.

- **Leitfaden für die Aufgaben:** Mit Mixed Reality können Sie Ihre Mitarbeiter ganz einfach durch eine Reihe von Aufgaben führen. Die Überlagerung digitaler Anweisungen und Best Practices zusätzlich zum physischen Objekt (z. B. eine Maschine in der Fabrik oder eine Kaffeemaschine in der Büroküche) kann das Abschließen dieser Aufgaben deutlich vereinfachen. Für das Auslösen dieser Umgebungen ist in der Regel ein Marker oder eine manuelle Ausrichtung erforderlich. Mit Azure Object Anchors haben Sie jedoch die Möglichkeit, eine Umgebung zu erstellen, die automatisch das Objekt im Zusammenhang mit der Aufgabe erkennt. Anschließend können Sie nahtlos mit Mixed Reality-Anleitungen ohne Marker oder manuelle Ausrichtung arbeiten.

- **Objektsuche:** Wenn Sie bereits über ein 3D-Modell eines Objekts im physischen Raum verfügen, können Sie mit Azure Object Anchors Instanzen dieses Objekts in Ihrer physischen Umgebung suchen und nachverfolgen.

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
