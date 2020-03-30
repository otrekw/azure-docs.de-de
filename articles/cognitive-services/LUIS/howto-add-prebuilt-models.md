---
title: Vordefinierte Modelle für Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS umfasst einen Satz von vordefinierten Modellen zum schnellen Hinzufügen von allgemeinen Szenarien für Benutzerkonversationen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: 0a733aff14b71c6378971c0b72a9ca1f3cd4cfc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "74013563"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Hinzufügen vordefinierter Modelle für gängige Verwendungsszenarien 

LUIS umfasst einen Satz von vordefinierten Modellen zum schnellen Hinzufügen von allgemeinen Szenarien für Benutzerkonversationen. Dies ist eine schnelle und einfache Möglichkeit, um Ihrer Clientanwendung für Konversationen grundlegende Fähigkeiten hinzuzufügen, ohne die Modelle für diese Fähigkeiten entwerfen zu müssen. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-a-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

1. Wählen Sie auf der Seite **Meine Apps** Ihre App aus. Daraufhin wird Ihre App im Abschnitt **Build** der App geöffnet. 

1. Wählen Sie auf der linken Symbolleiste **Vordefinierte Domänen** aus. 

1. Suchen Sie die Domäne, die Sie der App hinzufügen möchten, und wählen Sie dann die Schaltfläche **Domäne hinzufügen** aus.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen der vordefinierten Domäne „Kalender“](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

## <a name="add-a-prebuilt-intent"></a>Hinzufügen einer vordefinierten Absicht

1. Wählen Sie auf der Seite **Meine Apps** Ihre App aus. Daraufhin wird Ihre App im Abschnitt **Build** der App geöffnet. 

1. Wählen Sie auf der Seite **Absichten** auf der Symbolleiste über der Absichtenliste **Vordefinierte Absicht hinzufügen** aus. 

1. Wählen Sie die Absicht **Utilities.Cancel** im Popupdialogfeld aus. 

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen der vordefinierten Absicht](./media/luis-prebuilt-domains/add-prebuilt-domain-intents.png)

1. Wählen Sie die Schaltfläche **Fertig** aus.

## <a name="add-a-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität

1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** auf ihren Namen und anschließend links auf **Entitäten** klicken. 

1. Klicken Sie auf der Seite **Entitäten** auf **Add prebuilt entity** (Vordefinierte Entität hinzufügen).

1. Wählen Sie im Dialogfeld **Vordefinierte Entität hinzufügen** die vordefinierte Entität aus. 

    > [!div class="mx-imgBorder"]
    > ![Dialogfeld „Vordefinierte Entität hinzufügen“](./media/luis-prebuilt-domains/add-prebuilt-entity.png)

1. Wählen Sie **Fertig**aus. Nachdem die Entität hinzugefügt wurde, müssen Sie die App nicht trainieren. 

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Veröffentlichen Sie sie, um ein vordefiniertes Modell vom Vorhersageendpunkt anzuzeigen

Die einfachste Möglichkeit, den Wert eines vordefinierten Modells anzuzeigen, ist eine Abfrage an den veröffentlichten Endpunkt. 

## <a name="entities-containing-a-prebuilt-entity-token"></a>Entitäten mit einem vordefinierten Entitätstoken
 
Wenn Sie über eine durch maschinelles Lernen erworbene Entität verfügen, die durch eine vorgefertigte Entität eingeschränkt ist, fügen Sie der durch maschinelles Lernen erworbenen Entität eine Unterkomponente hinzu, und fügen Sie dann eine Einschränkung einer vorgefertigten Entität hinzu.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Erstellen eines Modells aus CSV mit REST-APIs](./luis-tutorial-node-import-utterances-csv.md)
