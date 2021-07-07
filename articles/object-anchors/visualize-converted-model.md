---
title: Visualisieren eines Object Anchors-Modells
description: Beschreiben Sie, wie ein konvertiertes Modell visualisiert wird.
author: rgarcia
manager: vrivera
ms.author: rgarcia
ms.date: 05/28/2021
ms.topic: overview
ms.service: azure-object-anchors
ms.openlocfilehash: a9140004d9f4d79cabb9890851ba5c5cb7b815f2
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111987866"
---
# <a name="how-to-visualize-an-object-anchors-model"></a>Visualisieren eines Object Anchors-Modells

Sie müssen ein konvertiertes Modell nicht visualisieren, um es zu verwenden. Es gibt jedoch eine einfache Möglichkeit, das Gittermodell anzuzeigen, bevor Sie es bei Bedarf verwenden.

Führen Sie die Schritte in unserer [Schnellstartanleitung zur Unity-App](quickstarts/get-started-unity-hololens.md) mit einer geringfügigen Änderung aus. Wenn Sie die Beispielszene erstellen, öffnen Sie anstelle von **AOASampleScene** einfach **VisualizeScene**, und fügen Sie es der Liste für die Szenenerstellung hinzu. Stellen Sie dann unter **Buildeinstellungen** sicher, dass *nur* **VisualizeScene** über ein daneben stehendes Häkchen verfügt. Alle anderen Szenen sollten nicht eingeschlossen werden.

:::image type="content" source="../../includes/media/object-anchors-quickstarts-unity/aoa-unity-build-settings-visualize.png" alt-text="Visualisieren von Buildeinstellungen":::

Schließen Sie das Dialogfeld **Buildeinstellungen**, *anstatt* die Schaltfläche **Erstellen** auszuwählen.

Wählen Sie im Bereich **Hierarchie** das **Schnellansicht**-GameObject aus.

:::image type="content" source="../../includes/media/object-anchors-quickstarts-unity/aoa-unity-hierarchy.png" alt-text="Hierarchie":::

Suchen Sie im Bereich für den **Inspector** die **Modellpfad**-Eigenschaft im Abschnitt **Mesh Loader (Script)** (Gittermodell-Ladeprogramm (Skript)), und geben Sie den Pfad zu Ihrer Object Anchors-Modelldatei ein, einschließlich der Dateierweiterung.

:::image type="content" source="../../includes/media/object-anchors-quickstarts-unity/aoa-unity-inspector.png" alt-text="Inspector":::

Wählen Sie oben im Unity-Editor die Schaltfläche **Wiedergeben** aus, und stellen Sie dann sicher, dass die Ansicht **Szene** ausgewählt ist.

:::image type="content" source="../../includes/media/object-anchors-quickstarts-unity/aoa-unity-editor.png" alt-text="Wiedergabe und Szenenansicht":::

Mithilfe der [Navigationssteuerelemente für die Szenenansicht von Unity](https://docs.unity3d.com/Manual/SceneViewNavigation.html) können Sie nun Ihr Object Anchors-Modell überprüfen.

:::image type="content" source="./media/object-anchors-visualize.png" alt-text="Visualisieren des Modells":::
