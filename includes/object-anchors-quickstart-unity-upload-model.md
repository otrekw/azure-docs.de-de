---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 03/02/2021
ms.author: crtreasu
ms.openlocfilehash: d06a6ecd8af16da3e6df21e984fbf6a727fbc27e
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105734"
---
### <a name="upload-your-model"></a>Hochladen des Modells

Wenn Sie noch nicht über ein Object Anchors-Modell verfügen, befolgen Sie die Anweisungen unter [Erstellen eines Modells](../articles/object-anchors/quickstarts/get-started-model-conversion.md), um eines zu erstellen. Kehren Sie dann hierher zurück.

Wenn HoloLens mit dem Windows-Geräteportal verbunden ist, führen Sie die folgenden Schritte aus, um ein Modell für die zu verwendende App hochzuladen:

1. Navigieren Sie im Windows-Geräteportal zu **System > Datei-Explorer > LocalAppData**. Dort sollte Ihre Anwendung in der Liste der Apps angezeigt werden.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localappdata.png" alt-text="Datei-Explorer":::

2. Öffnen Sie die Anwendung, und klicken Sie auf den Ordner `LocalState`.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localstate.png" alt-text="Öffnen des Ordners „LocalState “":::

3. Laden Sie die Modelldatei in den Ordner `LocalState` hoch.

    :::image type="content" source="./media/object-anchors-quickstarts/portal-upload-model.png" alt-text="Hochladen des Modells im Portal":::

    Starten Sie die Anwendung noch mal über HoloLens. Sie können jetzt Objekte erkennen, die mit dem Modell übereinstimmen.